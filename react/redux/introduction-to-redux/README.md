## Introduction to Redux (outdated)

> Redux - A predictable state container for JavaScript apps.

### Common definitions

🚥 **action** - object which is some "signal", description of what should happen with state **store**

**action creator** - function which returns new **action**

**reducers** - functions which control state (store), return new state which based on coming **action**

**store** - main object of redux. Common data storage for application.

### Installing Redux

```
yarn add redux
```

or

```
npm i --save redux
```

### Create a store

```
/store/index.js
```

Using a function **createStore()** from module **redux**

```javascript
import { createStore } from "redux";

const store = createStore();

// only dev
window.store = store;

export default store;
```

**store** should take main **reducer** so let's create him

### Create reducer

```
/reducer/index.js
```

```javascript
import { combineReducers } from "redux";
import counter from "./counter";

export default combineReducers({
  counter
});
```

Main **reducer** combine all others reducers into one. For example, there is a separate reducer for working with data **counter**

```
/reducer/counter.js
```

```javascript
export default (count = 0, action) => {
  const { type } = action;

  switch (type) {
    case "INCREMENT":
      return count + 1;
  }

  return count;
};
```

Now we write in **store** to use main **reducer**

```javascript
import reducer from "../reducer";

const store = createStore(reducer);
```

For "call" any **action** using method **store.dispatch(action)**

Now to use **redux** and **react** together we're using module **react-redux**

**Installing react-redux**

```
yarn add react-redux
```

or

```
npm i --save react-redux
```

### reselect

**selector** - a function which returns a desired part of state

[[Reselect on GitHub](https://github.com/reduxjs/reselect)]

```
yarn add reselect
```

Let's create new folder for selectors **_/selectors_** and new file **index.js** which will contains all selectors of application

Define a function which returns filtered by date list of articles: **filteredArticles**

```javascript
export const filteredArticles = state =>
  state.articles.filter(article => {
    const date = new Date(article.date).valueOf();

    return (
      date >= state.dateRange.from.valueOf() &&
      date <= state.dateRange.to.valueOf()
    );
  });
```

Now this function can be used in **mapStateToProps**

```javascript
const mapStateToProps = state => ({
  filteredArticles: filteredArticles(state)
});
```

Creating selector by using function **createSelector** from **reselect**

```javascript
import { createSelector } from "reselect";

const dateRangeGetter = state => state.dateRange;
const articlesGetter = state => state.articles;

/**
 * articlesGetter and dateRangeGetter are other selectors which return desired state
 * a function in the third parameter is a function into which coming states from selectors from first and second parameters
 * a function in the third parameter should returns the desired state
 */
export const filteredArticlesSelector = createSelector(
  articlesGetter,
  dateRangeGetter,
  (articles, dateRange) =>
    articles.filter(article => {
      const date = new Date(article.date).valueOf();

      return date >= dateRange.from.valueOf() && date <= dateRange.to.valueOf();
    })
);
```
