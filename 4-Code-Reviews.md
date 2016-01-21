# Code Reviews

Code reviews come from an very important team wide value of having every single line of code subjected to the process of a code review.  

Also knows as peer code reviews are an essential part in the process of:

## 1. Maintaining high quality of code of each codebase

Quality of code is subjected to many opinions and points of view so in this guide we'll not be explicitly listing down a full and exhaustive list of properties of high quality code but here are a few **very obvious** examples.

### 1.1. Commented out Code

```python
def some_function():
  # add to enable debugging -----------
  #logger.setLevel(logging.DEBUG)
  #logger.setHandler(logging.FileHandler('my.log'))
  # -----------------------------------
  ...
```
You get the picture. Commented out code is an arguably a bad practice since you never know if the code around it is compatible with the commented out snippet.

An additional example might be something like this:
```python
# ---------------------------------------------
# not used, but I'll keep it here just in case
# def my_function(arg1, arg2):
#   "this is my function"
#   return arg1 + arg2
# ---------------------------------------------
```
We invited version control systems like git and mercurial so that we don't have to worry about losing code. If you care so much about losing your function in the above example, we'd suggest deleting it, make a commit and tagging that commit for easier retrieval. Just don't leave it hanging around.

### 1.2. Cyclomatic Complexity

Wikipedia's definition of [cyclomatic complexity](https://en.wikipedia.org/wiki/Cyclomatic_complexity) is *"Cyclomatic complexity is a software metric (measurement), used to indicate the complexity of a program. It is a quantitative measure of the number of linearly independent paths through a program's source code."*

Although we measure this via Codeclimate it's still very important that a reviewer pays attention to this metric when reviewing code and proposes ways to reduce the complexity thus improving the readability of code.


### 1.3. Module Length

This applies to all aspects of structuring code into logical modules.

By module we mean: functions, modules in python or go-lang, classes, something that holds lines of code together and logically separates them. If such a module is getting too large, start thinking on how to split one function into multiple function, breaking one huge source code file into a separate package etc.

### 1.4. Code Coverage

Make sure the newly committed code is properly tested. Writing tests alongside writing code will usually produce better results in terms of overall quality, modularity and cyclomatic complexity mentioned above. So as long as you apply this principle to your work this shouldn't be a problem.  

### 1.5. Breaking the Build

As you'll learn in the chapter "Code Reviews at Zemanta" we use github's pull requests to review code of our peers before it get's merged into master. Alongside pull requests we also continuously test & build our code on Circle CI and the information about a build on our CI environment is also fed back to pull requests on github.

![alt text](img/pull_request_success.png)

When reviewing code you should never give somebody a green light, if their build is not successful since they'll not be able to deploy this code anyway and code that isn't deployable is of no value to anyone.

## 2. Applying the Right Technology to the Problem at Hand

Sometimes a proposed changeset comes out as a beautiful piece of code that's tested, applies really good and smart coding practices and at first glance improves the overall quality of a particular module, but if it doesn't solve the problem it stated it did, it's of little to no value.

To avoid such instances both the reviewer and the code review submitter have to play their part by:

* **When submitting** a code review, make sure you provide enough context about the problem to the reviewer.
* **When reivewing**, make sure you understand what the context of the proposed changes and invest extra time into validating that the code actually solves the problem at hand correctly.  

*Pro tip:* An even more extreme case would be that we wouldn't be solving the right problem in the first place, but that's an issue that lies higher up in the entire engineering process, but pay attention to such cases also.


## 3. Preventing potential security issues or introducing harmful code

## 4. Adhering to good and established code practices and patterns

## 5. Preventing over-engineering or premature optimizations

## 6. Detecting potential vulnerabilities

# Code Review Process at Zemanta

TODO:

* code review length
* think from the receiving end
* merge tactics
* ship it!
