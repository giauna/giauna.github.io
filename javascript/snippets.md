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
```j


