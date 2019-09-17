# Javascript snippets

```javascript
    const foo = 1;
```

## Immutability
Immutable: after it has been created, it can never change.
`Strings` and `Numbers` are immutable. In fact, no string methods change the string they operate on, they all return new strings.
`Arrays` instead are mutable:
```javascript
var arr = [];
var v2 = arr.push(2); // v2 = 1 , arr = [2]
```
The arr reference has been updated to contain the number, and v2 contains the new length of arr.

## Spread operator

The Spread syntax allows you to expand something that is currently grouped inside a particular container and assign it to a different container. Compatible containers include: arrays, strings, objects and any iterable (such as Maps, Sets, TypedArrays, etc) and their elements can be expanded into function arguments, array elements and key-value pairs
```javascript
const numbers = [1, 2, 3];
console.log(...numbers); //Result: 1 2 3

const pokemon = ['Squirtle', 'Bulbasur', 'Charmander'];
console.log(...pokemon); //Squirtle Bulbasur Charmander

const pokedex = [
  { name: 'Squirtle', type: 'Water' },
  { name: 'Bulbasur', type: 'Plant' },
  { name: 'Charmander', type: 'Fire' }
];
console.log(...pokedex); //{ name: 'Squirtle', type: 'Water' } { name: 'Bulbasur', type: 'Plant' } { name: 'Charmander', type: 'Fire' }
```
**Note**: Bear in mind the fact that the spread operator only performs shallow copies. This means that if you have a reference data type stored inside your Array/Object, when you make a copy with the spread operator, the nested Array/Object will contain a reference to the original, and will thus be mutable.

## Arrays
`Arrays` and `Objects` are reference data types. Unlike Primitive data types, they point to the memory address where the actual Object/Array is stored.
To avoid the accidental mutation of variables, what we have to do is create a new instance of our Array/Object whenever we want to copy an Array/Object. 
[Hacks for Creating JavaScript Arrays](https://www.freecodecamp.org/news/https-medium-com-gladchinda-hacks-for-creating-javascript-arrays-a1b80cb372b/)

#### Clone array
```javascript
const originalArray = [1, 2, 3];
const shallowArrayClone = [...originalArray]; // [   1, 2, 3   ]
```
BE CAREFUL with arrays containing objects (= binds to objects!)
An important thing to note, when using spread syntax to copy a deep object, is that spread only goes one level deep
```javascript
const arr1 = [{name:"Omar"}, {name:"Vito"}];
const arr2 = [...arr1] // clones binds to objects
arr2[0].name = "Alexander"
console.log(arr1[0].name) // Alexander
console.log(arr1) // [ { name: 'Alexander' }, { name: 'Vito' } ]
console.log(arr2) // [ { name: 'Alexander' }, { name: 'Vito' } ]

//DEEP CLONE
let a = [{ x:{z:1} , y: 2}];
let b = JSON.parse(JSON.stringify(a));
b[0].x.z=0
console.log(JSON.stringify(a)); //[{"x":{"z":1},"y":2}]
console.log(JSON.stringify(b)); // [{"x":{"z":0},"y":2}]
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
const flattenedArray = [].concat(...arrayToFlatten); // [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### Concat
```javascript
arr1.push(...arr2);
// faster than
arr1 = arr1.concat(arr2);
```


### Converting Array-like Objects to Arrays
Array-like Objects are very similar to Arrays. They usually have numbered elements and a length property. However, they have one crucial difference: Array-like Objects **do not have any of the Array functions**.
Among the Array-like Objects are the **HTML node lists returned by most DOM methods**, the arguments variable generated automatically in every JS function and a few others.
Here's an example, converting a NodeList to an Array: 
```javascript
const nodeList = document.getElementsByClassName("pokemon");
const array = [...nodeList];  
console.log(nodeList); //Result: HTMLCollection [ div.pokemon, div.pokemon ]
console.log(array); //Result: Array [ div.pokemon, div.pokemon ]
// or (old way)
const array = [].slice.apply(nodeList);
// What this line of code is doing [].slice.apply(nodeList) is that changes the context from the empty array [], to the nodeList object, to make it seem that the object that called slice was nodeList and not the empty array itself
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
const personArr = [{ name: 'Param' },{ name: 'Ahmed' },{ name: 'Jesus' }];

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

## Optional Chaining

```javascript
// API response object
const person = {
    details: {
        name: {
            firstName: "Michael",
            lastName: "Lampe",
        }
    },
    jobs: [
        "Senior Full Stack Web Developer",
        "Freelancer"
    ]
}
// Getting the firstName
const personFirstName = person.details.name.firstName;

// Checking if firstName exists
if( person &&
    person.details &&
    person.details.name ) {
        const personFirstName = person.details.name.firstName || 'stranger';
}
```
A better solution?

**lodash**
```javascript
_.get(person, 'details.name.firstName', 'stranger');
```

**or Optional chaining** 
Right now no browser supports this out of the box — Babel to the rescue. There is a [babel.js plugin](https://babeljs.io/docs/en/babel-plugin-proposal-optional-chaining) already that is pretty easy to integrate if you have already Babel setup.
```javascript
const personFirstName = person?.details?.name?.firstName ?? 'stranger';
// ?? called Nullish coalescing operator
```
Other samples
```javascript
const jobNumber = 1;
const secondJob = person?.jobs?.[jobNumber] ?? 'none';
// jobs?.[jobNumber] is the same as jobs[jobNumber] but it will not throw an error; instead, it will return 'none'.

const currentJob = person?.jobs.getCurrentJob?.() ?? 'none';
```
