# Coding Practices

This is a very short intro to a few opinionated beliefs about coding practices that are ever evolving and are being continuously improved. Nevertheless we decided to documented these practices for the sake of on-boarding newer team members and making their first couple of code review submissions easier.

The principles below apply to all languages we use: Python, Go, JavaScript.

## 1. Writing Expressive Code

We believe in writing expressive code vs documenting blocks of code with inline comments. Although inline comments can sometime aid into understanding code they aren't a replacement for expressively named variables, functions, exceptions etc.

Let's take this Django view as an  exaggerated example.


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

Some coders might argue that there's nothing wrong with the above example and that's true to some degree, if you're used to the aforementioned principle of inline-ing comments alongside blocks of code to explain the flow of the program.

At Zemanta we subscribe to the principle: ***"Expressive code the point where comments become obsolete."***

Arguably the above example could be rewritten to:

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

Notice that we've re-factored the above code by:

* renamed vague function names that required a comment to explain their purpose i.e. `login_view` to `end_user_for_account_login`
* replaced blocks of code by introducing smaller helper function with expressive names i.e. `get_user_from_DB()`
* renamed short acronym named variables i.e. `aid` to `account_id`
* introducing custom exceptions with expressive names that explain the error conditions i.e. `DBConnectionError`

By just applying these principles we've done ourselves a huge favor since the re-factored code is now easier to understand because it's shorter, has lesser cyclomatic complexity and doesn't require cognitive load to tie the comments with code logic.

## 2. Functions not Classes

Over the years we've also adopted a functional approach to organizing and structuring code opposed to an object oriented approach. You don't need a purely functional language to write code in functional manner.

A functional approach means that you organize code into small, easy to understand functions and then you compose these functions into larger logical blocks.   

At the same time, a clever use classes can be beneficial in cases:

**Classes as Namespaces**

```python
class ErrorCode:
  ok = 200
  not_found = 404
  server_error = 500
```

**Classes or Interfaces as IO Client Encapulation**

Encapsulating any IO client state (i.e. sockets, open files ..) is generally a good practice if they are written with the goal to abstract away any low level client code and provide a higher level functionality to the business logic communicating via client instance . Instances of such clients are injected into business logic thus making the injected code easier to test since clients can be mocked.

```python
# Python
class ClientForRandomAPI:
  def __init__(self, auth_token):
    # ...

  def get_item_by_id(self, id):
    # ...
```


Being able to mock a behavior of a client promotes the use of interfaces in statically typed languages as Go.


```go
// Go
type RandomAPI interface {
  GetItemById(id int) Item
}

type ImplementationOfRandomAPI struct {
  AuthToken string
  // ...
}

func (client *ImplementationOfRandomAPI) GetItemById(id int) Item {
  // ...
}

func validateItem(itemId int, client RandomAPI){
  // ...
}

```

**Classes used to Encapsulate Mutable State**

Arguably it also makes sense to introduce classes to encapsulate complicated mutable state that is used across the codebase via object methods.

Example: A custom implementation of heap as a priority queue with high level methods.

## 3. Separation of Concern

Some languages enable you to apply the principles of aspect oriented programming by being able to explicitly separate concerns.

Example: Django view with code that makes sure a user is logged in and that a timer is being emited.

```python

def my_bloated_view(request):
  time_started = time.time()

  if not user_logged_in(request):
    raise NotLoggedInError

  # ...

  influxdb.timer('my_bloated_view_timer', time.time() - time_started)
```

By applying the principles above we can separate the concern of emitting the timer and making sure the user is logged in away from `my_bloated_view` and re-factor code into a much more readable and clean block of code using python decorators in our example

```python

@make_sure_user_is_logged_in
@influxdb.timer_wrapper('my_bloated_view_timer')
def my_clean_view(request):
  # ...

```
