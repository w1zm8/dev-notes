# Primitive and Reference values

**Primitive Values**

Primitive values - immutable values (can't be changed), non-object values (doesn't have methods or fields)

This is about VALUE, not objects like Number, String, etc

- number
- string
- boolean
- undeinfed
- null
- symbol
- bigint

**Reference Values**

Reference Values - mutable object values

- objects ({})
- arrays
- functions (basically they are objects too)


## JavaScript Storing Data

**Primitive Values**

```text
[number, string, boolean]
  ||
  ||
  \/
created and stored copies ===> Memory
  ||
  ||
  \/
copied value is stored
  ||
  ||
  \/
JS Variables
```

```text
**Reference Values**

[object, array]
  ||
  ||
  \/
stored this once and setted pointer ===> Memory
  ||
  ||
  \/
stored pointer to address in memory
  ||
  ||
  \/
JS Variables
```

**Example (scheme)**

```text

|-------------------------------|----------------------------------|
|               code            |          memory                  |
+-------------------------------|----------------------------------+
| const numb = 42; --stored-->  | 42 --+                           |
|                               |      | copied                    |
| const anotherNumb = numb;     | 42 <-+                           |
|                               |                                  |
|                               |      012321 - address in memory  |
| const fruits = ['apple'...]  ---------> [012321: ['apple'...]]   |
|                               |          ^                       |
|                               |          |                       |
| const newFruits = ['apple'..] -----------+                       |
|                               |                                  |
|                               |                                  |
+-------------------------------+----------------------------------+

```

**Example**

```js
// primitive value
let someNumber = 5;
// copied value that is in variable 'someNumber'
// for primitive values - copied === created new value based on value itself
let someNewNumber = someNumber;

someNumber++;

console.log(someNumber); // 6
console.log(someNewNumber); // 5

// reference values
let car = {
  brand: "Porsche",
  model: "911",
  year: 1963,
};
let secondCar = car; // 'secondCar' contains pointer to 'car'

car.model = "some model";

console.log(car.model); // some model
console.log(secondCar.model); // some model
```

**Example - similar reference values but different pointers**

```js
// have different addresses in memory
const listOne = ["one", "two", "three"];
const listTwo = ["one", "two", "three"];

// have different addresses in memory
const objOne = {
  val: 42,
};
const objTwo = {
  val: 42,
};

// compare address in memory
console.log(listOne == listTwo); // false
console.log(listOne === listTwo); // false

// compare address in memory
console.log(objOne == objTwo); // false
console.log(objOne === objTwo); // false
```

**Example - copying reference values**

```js
// copying reference values

// NOT DEEP - is when values/fields of array/object are primitive values

/**
ARRAY

Ways:

1. slice()
2. spread operator

 **/
const listOne = ["one", "two", "three"];
// the same pointer !!
//const listTwo = listOne;

const listTwo = listOne.slice();

/** modern (hipster) way
const listTwo = [...listOne];
*/

listOne.push("four");

//console.log(listOne);
//console.log(listTwo);

/**
OBJECT

Ways:

1. Object.assign
2. spread operator

 */

const car = {
  brand: "Porsche",
  model: "911",
  year: 1963,
  child: {
    brand: "Porsche",
    model: "911",
    year: 1963,
  },
};

// NOT DEEP
//const car2 = Object.assign({}, car);
//const car2 = {
//...car,
//};

// DEEP CLONE
const car2 = JSON.parse(JSON.stringify(car));

car.model = "some model";
car.child.model = "some model";

console.log(car);
console.log(car2);

// DEEP

const listOfCars = [
  {
    brand: "Porsche",
    model: "911",
    year: 1963,
    child: {
      brand: "Porsche",
      model: "911",
      year: 1963,
    },
  },
  {
    brand: "Porsche",
    model: "912",
    year: 1964,
  },
  {
    brand: "Porsche",
    model: "913",
    year: 1965,
  },
];

// does not work 'cause all values of array are reference
// it copies array of pointers!
//const listOfCars2 = [...listOfCars];

// it just copies array of nested 1
// it means that 'child' in fist element in two arrays will have same pointer
const listOfCars2 = listOfCars.map((car) => ({ ...car }));

// that doesn't affect listOfCars2 'cause variables listOfCars and listOfCars2 have different pointers
// but not their values! their values have the same pointers
//listOfCars.push({
//brand: "Porsche",
//model: "914",
//year: 1966,
//});
listOfCars[0].model = "1232131";
listOfCars[0].child.model = "1232131";

// they are the same
//console.log(listOfCars[0].model);
//console.log(listOfCars2[0].model);

//console.log(listOfCars[0]);
//console.log(listOfCars2[0]);

// each value of array is reference and has own address in memory
```

