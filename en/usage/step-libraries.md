# Step Libraries

Step libraries are the mechanism for mapping step text to executable functions. Yadda supports both synchronous and asynchronous steps.

```js
new Yadda.Library()
    .define('A synchronous step', function() {
        // Code goes here
    })
    .define('An asynchronous step', function(next) {
        // Code goes here
        next();
    });
```

### Given / When / Then
```js
Yadda.localisation.English.library()
    .given('some precondition', function() {
        // Code goes here
    })
    .when('I do something', function() {
        // Code goes here
    })
    .then('expect some result', function() {
        // Code goes here
    });
```
While can write whatever steps you want, 'Given', 'Then' and 'When' steps are so common we've included localised shorthand methods. Several [languages](./localisation.md) are supported.

### Regular Expressions
Use regular expressions for fuzzy matching
```js
new Yadda.library()
    .define('[Ss]etup a new user', function() {
        // Code goes here
    })
```
Use actual regular expression objects to avoid escaping backslashes
```js
new Yadda.library()
    .define(/[Ss]etup a new user/, function() {
        // Code goes here
    });
```

### Paramterised Steps
Use regular expressions with matching groups to extract parameters from the step text
```js
Yadda.localisation.English.library()
    .given('a user called (\\w+)', function(name) {
        // Code goes here
    });
```
Use terms to make your steps more friendly.
```js
Yadda.localisation.English.library()
    .given('a user called $name', function(name) {
        // Code goes here
    });
```
All the above steps are equivalent.

### Multiline Steps
```
Scenario: Some title

    Given some csv
    --------------
    First Name,Last Name,Age
    Joe,Bloggs,23
    John,Smith,41
    --------------
```
```js
var dictionary = new Yadda.Dictionary()
    .define('csv', /([^\u0000]*)/, csvConverter);
    
Yadda.localisation.English.library(dictionary)
    .given('some csv\n$csv', function(csv) {
        // Code goes here
    });
```
Multiline steps are appended to the preceding single line step, and should be used with a dictionary defintion and optional converter.

### Step Aliases
```
    Given Bob has 1 book
    And Alice has 2 books
```

```js
Yadda.localisation.English.library()
    .given(['$name has $num book', '$name has $num books'], function(name, number_of_books) {
        // Code goes here
    })
```
Use Step aliases when you want to map multiple steps to the same function without writing complicated regular expressions.

### Pending Steps
```js
new Yadda.Library()
    .define('Some step')
```
Steps without a function are skipped.

