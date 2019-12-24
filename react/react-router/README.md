# React Router

[[Official docs (Web)](https://reacttraining.com/react-router/web/guides/quick-start)]

> I'm using react-router which has version 4 and more. react-router with previous versions has different API which doesn't base on React components

**Install dependencies**

This dependency just for browser version of react-router. Also they have module "react-router" as core functionality and "react-router-native" for React Native applications

```
npm i react-router-dom
```

## Basic things

- Router (BrowserRouter)
- Route
- Link

- **Router** has HTML5 history api and provide that to your app
- **Route** is component which has info about some route - path and what is component should be render then path is matched
- **Link** is just a wrapper of tag _a_ which open to you a specific route by using HTML5 history

### exact

**Using for root component**

If you have several router which exist on the same route level and one of them has path which is matched in others then you can use prop **exact**

exact - exactly

```javascript
    <Route exact path="/" component={Home} />
    <Route path="/contacts" component={Contacts} />
    <Route path="/notes" component={Notes} />
```

After that a route _Home_ won't be matched on _/contacts_ and _/notes_

**Using for case when we have components with list of child components**

We can use "exact" for component if we want to display a message just when we still didn't open a child route from list of component.

Also we use prop **render** which provide ability to render something outside of specific component:

```javascript
const Notes = ({ match }) => (
  <div>
    <h2>Notes</h2>
    <ul>
      <li>
        <Link to={`${match.url}/third`}>Third</Link>
      </li>
    </ul>

    <Route exact path={match.url} render={() => <h3>Pls open a note</h3>} />
    <Route path="/notes/:noteId" component={Note} />
  </div>
);
```

### How to use a parent route url in child routes

If you have something like:

**_App.js_**

```javascript
<Route path="/notes" component={Notes} />
```

**_Notes_**

```javascript
    <ul>
      <li>
        <Link to={`/notes/first`}>First</Link>
      </li>
        ...
    </ul>

    <Route path="/notes/:noteId" component={Note} />
```

You will see what we duplicate the same string _/notes_ which is not good

You can just replace _/notes_ in "child" routes by prop **match**:

```javascript
    <ul>
      <li>
        <Link to={`${match.url}/first`}>First</Link>
      </li>
        ...
    </ul>

    <Route path="/notes/:noteId" component={Note} />
```
