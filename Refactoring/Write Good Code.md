# How to write good code

## Function Best pratices
* Each function should do exactly one thing
* Limit the number of parameters in a function
* Return early from functions and avoid too much nesting
* If a code is repeated extract functions out of it
* Ensure that arguments passed to functions are immutable

## Formatting
* Having proper indentation helps make code reading easier.

## Error Handling
* Have proper error handling
* Log important error messages

## Tests
* Having TDD or BDD ensures high test case coverage and ensure all important buisness scenarios are covered.


## Dead Code or Features
* Remove it instead of commenting it out
* Incase it is a temperory or tactical code which is to be removed having comments explaing why it is required, the story/bug id and which all sections to be removed.

## Variable Naming
Naming variables ,functions, classes etc while coding is important for other developers to understand in the future. Also it reduces the probability of introduction of bugs.
It also makes the code maintainable.

### Guidelines for better naming while coding

#### For Class Names
* Use Nouns

#### For Functions
* Use verbs

#### For variables
* use adjectives
eg: isActive

## Constants
* Use constants for numbers and strings 

## Commit Messages
* Commit messages should explain what and why and not how
* Commits should be atomic

## References
* https://substack.com/@petarivanovv9/note/c-98122490?r=40ln3j
* https://substack.com/@javinpaul/note/c-90893058?r=40ln3j