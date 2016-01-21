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





### 1.4. Absence of Tests


## 2. Applying the right technology to the problem at hand

Sometimes a proposed changeset

## 3. Preventing potential security issues or introducing harmful code

## 4. Adhering to good and established code practices and patterns

## 5. Preventing over-engineering or premature optimizations

## 6. Detecting potential vulnerabilities

# Code Review Process at Zemanta
