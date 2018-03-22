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


