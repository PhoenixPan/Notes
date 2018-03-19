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
