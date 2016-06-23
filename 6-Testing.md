
# Testing

Testing code is important and we believe that every change of functionally should be accompanied with tests. We're not religious about 100% coverage, but that shouldn't be an excuse for not adding tests.

So far we have introduced the following categories of tests:

* **Unit tests** - You all know unit tests. This is where you test your business logic, helper functions, handlers, etc. We write tests for both backend and frontend code.
* **End to end tests (e2e)** - We've adopted this technique from the angluar stack. It basically means: spinning up a mock instance of the whole web application (i.e. dashboard) and then instructing [protractor](http://www.protractortest.org/#/) to trigger browser events and asserting facts afterwards.

Not yet introduced:

* **Integration tests** - We currently don't have separate integration tests suites in any of our codebases. Integration tests in general: testing endpoints of code that leaves the boundaries of your system and requires a resource from another system.

## 1. Testing Tools, Utilities and Frameworks

We use the following tooling for testing:

* [**Jasmine**](http://jasmine.github.io/) - we use Jasmine to test basic angular business logic
* **Django Unit Tests** - all of our python apps use the standard python / django unit testing framework
* **Go Tests** - all of our Go code is tested via the standard [`go test`](https://golang.org/pkg/testing/) framework. In addition we use the powerful set of utils in [`github.com/stretchr/testify`](https://github.com/stretchr/testify).
* [**Protractor**](http://www.protractortest.org/) - as already mentioned we use protractor for e2e

## 2. Continious Integration

All the tests are run continiously on Circle CI, where we care about the following:

* **Zero breaking change in master** i.e. master should always be deployable and for master branch to be deployable, the build on Circle CI must pass.
* **Build duration** - we pay attention to build duration on Circle CI. A successful build is a prerequisite to a successful deploy in our release pipeline so optimising build times are important and should grow proportionally to number of tests in the suite.

## 3. Good Testing Practices

Good testing practices go hand in hand with good coding practices discussed in the previous chapter. One of our core team beliefs around testing is:

* **Test while you code**
* **Treat tests as first class citizens (i.e. yes DRY does apply also for tests)**

In the following chapters we will review some arguably good testing practices.

### 3.1. Write Tests While You Code

... and not after the fact. If you write tests after you've written your code you will probably not be able or you'll find it very difficult to test all paths in your code.

Let's take our user login handler from our previous chapter.


```python

# end user login endpoint for account
def login_view(request, aid):

  # init login form
  form = LoginForm(request.POST)
  # validate login form
  if not form.is_valid():
    # handle non valid form here
    influxdb.counter('invalid_form_sumission', 1)
    logger.debug('user invalid on account: %s', aid)
    # ...
    # ... more handling code would come here
    # ...
    return render(request, 'login_error_template.html', {'form': form})

  # let's first try to fetch the user from DB
  try:
    usr = User.objects.get(username=form.username)
    if UserAccount.objects(user=usr.id, account=aid).exists():
      # ...
      # ... more handling code would come here
      # ...
  except:
    # if we get an exception we add an error to the login form
    form.add_error('username', 'user does not exist')
    influxdb.counter('invalid_user', 1)
    logger.debug('user not in db on account: %s', aid)
    # ...
    # ... more handling code would come here
    # ...
    return render(request, 'login_template.html', {'form': form})

  # let's also check is user is in the legacy system
  if settings.HTTP_LEGACY_AUTH_ENABLED:
    resonse = requests.post(
      setting.HTTP_LEGACY_AUTH_ENDPOINT+'/user/auth/',
      data={'user': form.username, 'account': aid, 'password':form.password}
    )
    if response.code != 200:
      # ...
      # ... more handling code would come here
      # ...


    return render(request, 'login_template.html', {'form': form})
```

Looking at this monolithic handler and thinking about how to test it, we would first have to develop helper functions for mocking the `form`, `request` and even more complex helpers that would allow us to assert the handler produced the correct result based on the already rendered html template. Now think what happens if such a monolith does reads and writes to the DB. Think about the complex fixtures you'll have to write for each test case. Not think of how hard would it be for someone else to understand what is going on in such a test. Good luck with that.

Now let's look at our "expressive" refactored login handler.

```python

def end_user_for_account_login(request, account_id):

  login_form = LoginForm(request.POST)
  if not login_form.is_valid():
    return handle_login_form_invalid(request, login_form)

  try:
    user_object = get_user_from_DB(form, account_id)
  except UserMissingException:
    return handle_missing_user(request, form)
  except: DBConnectionError:
    return handle_db_error(request)

  if not is_user_addded_to_account(user_object, account_id):
    return handle_user_not_in_account(request, form)

  try:
    authentication_with_legacy_system(user_object)
  except LegacyAuthError as legacy_error:
    return handle_legacy_system_error(legacy_error)

  return render(request, 'login_template.html', {'form': form})
```

In this particular case we can write tests for all the logical components included in the handler:

* Form validation and handling of an invalid form,

```python
class EndUserAccountLoginHandlerCase(TestCase):

 def test_form_validation_valid_form(self):
  # ...
 def test_form_validation_invalid_form(self):
  # ...
 def test_form_handle_invalid_form(self):
  # ...
 def test_form_validation_valid_form(self):
  # ...
```



* continue with testing with user fetching from DB and it's error states,


```python
 def test_get_from_db_success(self):
  # ...
 def test_get_from_db_error(self):
  # ...
```


* testing business logic around adding user in the account,


```python

 def test_user_not_in_account(self):
 # ...
 def test_user_in_account(self):
 # ...
```


* and lastly for auth with legacy system.


```python

 def test_auth_with_legacy_success(self):
  # ...
 def test_auth_with_legacy_error(self):
 # ...
```

* Now that we have tests and coverage for all the components we can now write just very basic "sanity" check test that covered the entire handler.


```python

 # sanity check
 def test_entire_handler(self):
  # ...
```

Notice the hierarchial divide and conquer approach we took when testing the handler. First we tested the components and now that we are sure how components work, we've tested the composition of these components in `test_entire_handler`.

### 3.2. Treat Tests as First Class Citizens

... i.e. treat them the same way as you would treat your core code components and adhere to the principles of good coding practices in the previous chapter.

Alongside good coding principles try to follow these best testing practices:

* **Name tests expressively.** Notice that we stayed true to the principle of treating tests as first class citizen since we named our unit tests using a pattern that reveals the contents of that test i.e. `test_auth_with_legacy_error`.
* **Follow the DRY principle.** Try to identify code blocks that get repeated across your tests and make sure you write appropriate helpers for better structured tests.
* **Make best use of testing framework idioms.** For example: it makes a lot of sense to make use of `setUp`, `tearDown` and `setUpClass` and `tearDownClass` methods in python's `unittest` [framework](https://docs.python.org/2.7/library/unittest.html#test-cases).


### 3.3. Mocking

Mocking objects and functions when writing tests can be tricky and should be used with care. At Zemanta we're very fond of the following mocking libraries:

* for python we use [`mock`](https://pypi.python.org/pypi/mock),
* and for Go we make use of [`github.com/stretchr/testify/mock`](https://godoc.org/github.com/stretchr/testify/mock).

Mocks can be useful in cases where code path leaves our system. In the example below, it makes a lot of sense to mock a client object that communicates with a system outside of apps boundary.

```python

from unittest.mock import MagicMock

class HeavyClientTestCase(TestCase):

  @classmethod
  def setUpClass(cls):
    cls.client = HeavyClient()
    cls.client.important_method = MagicMock(return_value=True)

  def test_function_that_uses_heavy_client(self):
    result = my_function('first_parameter', self.client)
    self.assertTrue(result)
    self.client.important_method.assert_called_with('first_parameter')

```

### 3.4. Fixtures

[Fixtures](https://docs.djangoproject.com/en/1.9/howto/initial-data/) can be very useful for initiating your tests with data required for tests to function.

So instead of putting your data initialization spaghetti code into into your tests:

```python
class SphagettiTestCase(TestCase):

  @classmethod
  def setUpClass(cls):
    cls.user = User(
      username= 'name',
      password= 'password',
      created_dt= datetime.datetime(2016,1,1),
      # ...
    )
    cls.user.save()
    cls.my_entity = Entity(
      # ...
    )
    cls.my_entity.save()
    # ...
    # 100 lines more code here

  def test_my_test(self):
    # ...
```  

You can initialize this data via fixtures and since we promote YAML format you can even inline the fixtures with comments:

`my_fixture.yaml`
```yaml

# admin user required for all sorts of tests
- model: User
  pk: 1
    username: name
    password: pass
```

... and then load up this data in your tests:

```python
class FixturesTestCase(TestCase):

  fixtures = ['my_fixture']

  def test_my_test(self):
    # ...

```

### 3.5. One Unit per Test

... hence the name Unit tests. Don't crowd your tests with more than one functionality and one assertion of outcome.

Let's again take a monolithic example:

```python

class MonolithicTestCase(TestCase):

  def test_all_the_things(self):
    result = my_first_function('my first param')
    self.assertTrue(result.truth)
    self.assertEqual(result.obj, {'this': 'that'})

    result = my_first_function('my second param')
    self.assertFalse(result.truth)
    self.assertEqual(result.obj, {'this': 'that'})

    second_result = my_second_function('my third param')
    self.assertTrue(second_result)

    second_result = my_second_function('my fourth param')
    self.assertFalse(second_result)
```

... and now let's break it down into multiple expressive unit tests:

```python

class NoLongerAMonolithicTestCase(TestCase):

  def test_my_first_function_success(self):
    result = my_first_function('my first param')
    self.assertTrue(result.truth)
    self.assertEqual(result.obj, {'this': 'that'})

  def test_my_first_function_fail(self):
    result = my_first_function('my second param')
    self.assertFalse(result.truth)
    self.assertEqual(result.obj, {'this': 'that'})

  def test_my_second_function_success(self):
    second_result = my_second_function('my third param')
    self.assertTrue(second_result)

  def test_my_first_function_fail(self):
    second_result = my_second_function('my fourth param')
    self.assertFalse(second_result)
```
