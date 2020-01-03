# Mock Functions With Jest

[[Docs - Mock Functions](https://jestjs.io/docs/en/mock-functions)]

**Input data 🐑 💤**

We have two function which one of them uses second one

```javascript
// sheepUtils.js

export const getLastSheep = maxAttemptCount =>
  Math.floor(Math.random() * Math.floor(maxAttemptCount)); // yes, that's from MDN;
```

```javascript
// sleepSheep.js

import { getLastSheep } from "./sheepUtils";

export const tryToSleep = maxCount => {
  let sheepsCount = 0;

  for (let i = 0; i < maxCount; i++) {
    const lastSheep = getLastSheep(maxCount);
    if (lastSheep === i) break;
    sheepsCount++;
  }

  return sheepsCount;
};

```

We have to test what a function **tryToSleep** called function **getLastSheep** a certain numbers of times and with specific arguments

## jest.fn()

[[Docs - jest.fn()](https://jestjs.io/docs/en/jest-object#jestfnimplementation)]

We can use **jest.fn()** for mock function **getLastSheep** but we should "save" original function and "reset" that in the end of test:

```javascript
import { tryToSleep } from "../sleepSheep";
import * as sheepUtils from "../sheepUtils";

it("returns count of sheep", () => {
  const maxCount = 20;
  const lastSheep = 3;

  const initialGetLastSheep = sheepUtils.getLastSheep;
  sheepUtils.getLastSheep = jest.fn(() => lastSheep);

  const sheepsCount = tryToSleep(maxCount);
  expect(Number.isInteger(sheepsCount)).toBe(true);
  expect(sheepsCount).toBeLessThan(maxCount);
  expect(sheepUtils.getLastSheep).toHaveBeenCalledTimes(lastSheep);
  expect(sheepUtils.getLastSheep).toHaveBeenCalledWith(maxCount);

  // need to "reset" mocking
  sheepUtils.getLastSheep = initialGetLastSheep;
});
```

## jest.spyOn()

[[Docs - jest.spyOn()](https://jestjs.io/docs/en/jest-object#jestspyonobject-methodname)]

Also we can use method **jest.spyOn()** which is similar as **jest.fn()** but has a couple of useful methods like **mockImplementation** and **mockRestore**

```javascript
import { tryToSleep } from "../sleepSheep";
import * as sheepUtils from "../sheepUtils";

it("returns count of sheep", () => {
  const maxCount = 20;
  const lastSheep = 3;
  jest.spyOn(sheepUtils, "getLastSheep");

  sheepUtils.getLastSheep.mockImplementation(() => lastSheep);

  const sheepsCount = tryToSleep(maxCount);
  expect(Number.isInteger(sheepsCount)).toBe(true);
  expect(sheepsCount).toBeLessThan(maxCount);
  expect(sheepUtils.getLastSheep).toHaveBeenCalledTimes(lastSheep);
  expect(sheepUtils.getLastSheep).toHaveBeenCalledWith(maxCount);

  // need to "reset" mocking
  sheepUtils.getLastSheep.mockRestore();
});
```

## jest.mock()

[[Docs - jest.mock()](https://jestjs.io/docs/en/jest-object#jestmockmodulename-factory-options)]

Instead of mock specific function we can also mock all module which contains functions that we're using in our tests

Also we can save mocked functions in another file, specific mock file and directory

```javascript
// __mocks__/sheepUtils.js

export const getLastSheep = jest.fn(() => 3);
```

```javascript
import { tryToSleep } from "../sleepSheep";
import * as sheepUtils from "../sheepUtils";

const lastSheep = 3;

// jest matches path '../__mocks__/sheepUtils.js'
jest.mock("../sheepUtils");

it("returns count of sheep", () => {
  const maxCount = 20;

  const sheepsCount = tryToSleep(maxCount);
  expect(Number.isInteger(sheepsCount)).toBe(true);
  expect(sheepsCount).toBeLessThan(maxCount);
  expect(sheepUtils.getLastSheep).toHaveBeenCalledTimes(lastSheep);
  expect(sheepUtils.getLastSheep).toHaveBeenCalledWith(maxCount);

  sheepUtils.getLastSheep.mockReset();
});
```

