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

## Arrow functions

#### this
![arrowFunc-this.jpg]({{site.baseurl}}/javascript/arrowFunc-this.jpg)

