# Javascript snippets

```javascript
    const foo = 1;
```

## Arrays

#### Clone array
```javascript
const originalArray = [1, 2, 3];
const shallowArrayClone = [...originalArray];
```
#### Get unique values
```javascript
const arrayWithDuplicateValues = [1, 2, 3, 3, 1, 5];
const uniqueArray = [...new Set(arrayWithDuplicateValues)];
```
#### same values(unordered)?
```javascript
const a = [1, 2, 3];
const b = [2, 3, 4];

const uniques = new Set(a.concat(b));
const haveSameValues = uniques.length === a.length // or uniques.length === b.length;
```
#### Flatten an array
```javascript
const arrayToFlatten = [ [1,2,3], [4,5,6], [7,8,9] ];
const flattenedArray = [].concat(...arrayToFlatten);
```

## Objects

#### Clone object
```javascript
const originalObject = { a:1, b: 2, c: 3 };
const shallowObjectClone = {...originalObject};
```
with property overriden
```javascript
const originalObject = { a:1, b: 2, c: 3 };
const shallowObjectClone = {...originalObject, c: 45 };
```

## Fetch - JSON
```javascript
// Async/Await requirements: Latest Chrome/FF browser or Babel: https://babeljs.io/docs/plugins/transform-async-to-generator/
// Fetch requirements: Latest Chrome/FF browser or Github fetch polyfill: https://github.com/github/fetch

// async function
async function fetchAsync () { // OR const fetchAsyncA = async () =>
return (await fetch('https://api.github.com')).json();
}
// trigger async function
fetchAsync()
    .then(data => console.log(data))
    .catch(reason => console.log(reason.message))

// BETTER THAN
fetch('https://api.github.com/users/wesbos')
.then(res => res.json()) //json() also returns a promise
.then(data => {console.log(data)});
```

## THIS

#### this
```javascript
let person = {
    name: "Sally",
    getName: function() {
        return this.name;
    }
};
let name = "Sergie";
person.getName(); // "Sally"
let foo = person.getName;
foo(); // "Sergie"
```
same, but using arrow function
```javascript
let person = {
  name: "Sally",
  getName: () => this.name;
}
let name = "Sergie";
person.getName(); // "Sergie"
let foo = person.getName;
foo(); // "Sergie"
```
```javascript
let object = {
    whatsThis: this,
    getThisNew: () => this,
    getThisOld: function() {
        return this;
    }
};
object.whatsThis(); // global
object.getThisNew(); // global
object.getThisOld(); // object
```
A good "hack" that works most of the times is to check if the function call is preceded by the dot operator. If it is, then this in the function definition will refer to the object before the dot operator. In the above case person.getName(), resulted in this being referenced to person. If there is no dot operator, this will usually refer to the global object.
Another hack to check where does the _this_ object of the arrow function points to, is to observe what would be the value of _this_ just before declaring the arrow function.

Classes: unlike objects where this refers does not refer to the object itself, in classes it does refer to the instance of the class:
![arrowFunc-this.jpg]({{site.baseurl}}/javascript/arrowFunc-this.jpg)
```javascript
class Fruit {
    constructor(name) {
        this.name = name;
    }
    getNameOld() {
        return this.name;
    }
    getNameNew = () => this.name;
}
// global variable
let name = "Sally";
let apple = new Fruit("Apple");
apple.getNameNew();// "Apple"
apple.getNameOld();// "Apple"
// Now let's make two new functions:
let foo = apple.getNameOld;
let bar = apple.getNameNew;
foo();// "Sally"
bar();// "Apple"
```
Vanilla functions follow the dot operator "hack" while the arrow functions stay binded to the value of this that was there just before the function was defined. This binding stays even if the function is re-declared unlike the vanilla flavour.


## Destructuring

#### Simple object
```javascript
// for simple object
const obj = {
  name: 'Param',
  city: 'Tallinn',
  age: 20,
  company: 'Learn with Param OU',
};

const { name, age, ...rest } = obj;

console.log(name); // Param
console.log(age); // 20
console.log(rest); // { city: 'Tallinn', company: 'Learn with Param OU', }
```

#### Array
```javascript
const personArr = [
  {
    name: 'Param',
  },
  {
    name: 'Ahmed',
  },
  {
    name: 'Jesus',
  },
];

const [first, ...restOfArr] = personArr;

console.log(first); // { name: 'Param' }
console.log(restOfArr); // [{ name: 'Ahmed' }, { name: 'Jesus' }]
```

#### Not defined variable
```javascript
const firstObj = {
  name: 'Param',
  city: 'Tallinn',
  age: 20,
  company: 'Learn with Param OU',
};

const { firstName, city } = firstObj;

console.log(firstName); // undefined
console.log(city); // Tallinn
```

#### Default value
```javascript
const secondObj = {
  firstName: 'Param',
  country: 'Estonia',
};

const { lastName = 'Harrison', country } = secondObj;

console.log(lastName); // Harrison
console.log(country); // Estonia
```