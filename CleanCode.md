# Chapter 2: Meaningful Names
### Use Intention-revealing Names
1. Use CONSTANT to reveal meanings of hard coded numbers
2. Avoid `list1`, `myList`, `int x`

### Avoid Disinformation
1. Don't name it "list" if it's not a List<>
2. Avoid abbreviations
3. Avoid long names with subtle differences: `XYZControllerForEfficientHandlingOfStrings` and `XYZControllerForEfficientStorageOfStrings`
4. Lower case L and number 1, upper case O and number 0 

### Make Meaningful Distinctions
1. Avoid distinct noise words: there's no actual difference between `ProductInfo` and `ProductData`, `Customer` and `CustomerObject`, `money` and `moneyAmount`

### Use Pronounceable Names
1. Try to discuss `genymdhms` (generation date, year, month, day, hour, minute,
and second) with your colleagues. How about `generationTimestamp`?

### Use Searchable Names
1. Number, single letter are generally unsearchable. Use them only as local variables. There's no way you can find the `int i` you wanted through IDE's search window
2. The length of a name should correspond to the size of its scope

### Avoid Encodings
1. No prefix, `ShapeFactoryImpl` instead of `IShapeFactory`

### Avoid Mental Mapping
1. Clarity is king. "r" is not for "the ower-cased version of the url with the host and scheme removed"

### Method names
Use static factory methods rather than constructors
```
Complex fulcrumPoint = Complex.FromRealNumber(23.0);
Complex fulcrumPoint = new Complex(23.0);
```

### Don't Be Cute
1. No jokes, no slangs, no culture-dependent texts

### Pick One Word per Concept
1. Use consistent lexicon throughout the program
2. Pick one of `fetch`, `retrieve`, and `get` for your methods. Similarly, avoid `controller`, `manager`, and `driver` in the same context

### Don't Pun 
1. Avoid using the same word for two different things: `add`, `append` and `insert` are different

### Use Solution Domain Names
### Use Problem Domain Names

### Add Meaningful Context
1. Enclose with well-named classes, functions, and namespaces
2. `state` make sense if you know the context is `address`, better be `addressState` if it's used alone

### Don’t Add Gratuitous Context
1. Given `MailingAddress` class in `GSD` namespace, you don't need `string gsdMailingAddress` inside the class


# Chapter 3: Functions
### Small!
1. Small functions are transparently obvious, each tells a story
2. The block within `if`, `else`, or `while` statement should be one line long, probably a function
3. Avoid nesting, the indent level should not be greater than two
4. DO ONE THING - functions contain only functions of the next level of abstraction

### Switch Statement
1. It should appear only once, used to create polymorphic objects, and hidden behind a inheritance relationship.
2. We may have a switch statement to check instance type in each methods such as `pay(Employee e)`. To avoid them, we should hide switch statement in an abstract factory, which create different instances of derivatives of abstract class `Employee` 

```
public abstract class Employee {
  public abstract double calculatePay();
}

public interface EmployeeFactory {
  public Employee makeEmployee(EmployeeRecord r);
}

public class EmployeeFactoryImpl implements Employee Factory {
  public Employee makeEmployee(EmployeeRecord r) {
    switch(r.Type) {
    case Permanent:
      return new PermanentEmployee(r);
    case Contractor:
      return new ContractorEmployee(r);
    }
    default:
      // throw exception: Invalid employee type r.Type
  }
}
```

### Use Descriptive Names
1. The smaller the function is, the easier it is to choose a descriptive name
2. Long name is ok: a long and descriptive name is better than a short and enigmatic name or a long and descriptive comment

### Function Arguments 
1. The fewer the better, avoid three or more arguments
2. Monadic Forms:
  1. Question about the argument `boolean isExist(File file)`; 
  2. Operate on the argument and return the result: `InputStream openFile(File file)`; 
  3. Event that has input argument but no return value or output argument: `void shutDownSystem()`
3. Output argument
  1. Should be avoided as it's confusing: `appendFooter(s);` Is s the footer of something or the footer attaches to s?
  2. If the function is going to transform the input argument, we should return transformation even if it's not necessary: `StringBuffer transform(StringBuffer in)`. Otherwise, readers need to check the code to know whether it's just a input argument or it takes the output of the function
  3.In OO programing, we have `this` to eliminate the usage of output arguments. If the function intends to change the state of something, have it change the state of its owning object: `report.appendFooter();`
4. Flag argument: ugly and avoid, it declares that the function is doing more than one thing! use two different functions instead
5. Argument Objects: When there are many arguments, it is likely that some of them could be wrapped into a class: `(double x, double y)` to `(Point p)`
6. Verbs and keywords: The function and argument should form a nice verb/noun pair: from `writeField(name)`, we know name is a field. `assertExpectedEqualsActual(expected, actual)` is better than `assertEquals(expected, actual)` as it mitigate the problem of remembering the order of arguments

### Have No Side Effects
1. Don't do anything or change any value that are not mentioned in function name: Having `Session.initialize()` in `checkPassword()` function creates temporal coupling and order dependency, as it can only be called when it's safe to initialize a session

### Command Query Separation
1. Commands returns void and Queries return values. Function should either do something or answer something, but not both: `bool success = login(username, password)` is not good, it's better to throw an exception rather than to check for error states
2. Special case: stack.pop()

### Prefer Exceptions To Returning Error Codes
1. It leads to deeply nested structures, as we have to handle the error immediately:
```
if (deletePage(page) == E_OK) {
  if (registry.deleteReference(page.name) == E_OK) {
    if (configKeys.deleteKey(page.name.makeKey()) == E_OK){
      logger.log("page deleted");
    } else {
      logger.log("configKey not deleted");
    }
  } else {
    logger.log("deleteReference from registry failed");
  }
} else {
   logger.log("delete failed");
   return E_ERROR;
}
```
much better with exception handling:
```
try {     
  deletePage(page);     
  registry.deleteReference(page.name);     
  configKeys.deleteKey(page.name.makeKey());   
}   catch (Exception e) {     
  logger.log(e.getMessage());   
}
```
2. Extract Try/Catch Blocks: They mix error processing with normal processing, so it's better to extract them into their own functions to achieve nice separation of responsibilities:
```
// all about error handling
public void delete(Page page) {
  try {
    deletePageAndAllReferences(page);
  }
  catch (Exception e) {
    logError(e);
  }
}

// all about normal processing
private void deletePageAndAllReferences(Page page) throws Exception {
  deletePage(page);
  registry.deleteReference(page.name);
  configKeys.deleteKey(page.name.makeKey());
}
```
3. Functions should do one thin and error handling is one thing. A function handling error should do nothing else.
4. Avoid Dependency Magnet: `if (deletePage(page) == E_OK)` Enums such as ErrorType are dependency magnet, once they have been used widely, no one wants to touch the code, as we have to deal with the legacy code. This forces us to reuse it in an incorrect or unintended ways.  
5. One example of **Open Close Principle**: open for extension but close for modification. 

### Don't Repeat Yourself

# Chapter 4: Comments
1. The proper use of comments is to compensate for our failure to express ourself in code
2. Programmers can’t realistically maintain them.
3. Truth can only be found in one place: the code

### Comments Do Not Make Up For Bad Code
1. Ooh, I’d better comment that!” No! You’d better clean it!

### Explain Yourself In Code
1. `if (user.isEligible())` is better than `if (user.age > 25 & user.balance > 0)`

# Chapter 5: Formatting
### Vertical Formatting
1. Vertical Density: Lines of code that are tightly related should appear vertically dense, no irrelevant elements or pointless comments in between
2. Vertical Distance: Concepts that are closely related should be kept vertically close to each other. They should not be separated into different files
3. Variable Declarations: as close to their usage as possible, unless it's been used throughout the scope
4. Instance Variables: declared at the top of the class (Java)
5. Dependent Functions: If one function calls another, they should be vertically close

### Horizontal Formatting
1. The Hollerith 80 limit is not necessary
2. Rule: never scroll right
3. Density: Use white space flexibily, we don't have to impose same spacing throughout: `b*b - 4*a*c;`
4. Indentation: used to indicate code hiearchy
5. Break Indentation: skipping indentation and {} for short if or loop is discouraged

### Team Rules
1. A team should agree on one formatting style

# Chapter 6: Objects and Data Structures
