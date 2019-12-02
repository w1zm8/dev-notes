## Main methods that creates Observables

```javascript
import { from, of, fromEvent } from "rxjs";
import { fromFetch } from "rxjs/fetch";
import { switchMap } from "rxjs/operators";

// from, of
// of(...arg) - creates and returns Observable of values from arguments "arg"
// from(M) - creates and returns Observable of M
// that can be Array, array-like object, Promise, iterable object or Observable-like object

const numbers = of(1, 2, 3, 4);
const strNumbers = from(["one", "two", "three", "four"]);

numbers.subscribe(console.log);
strNumbers.subscribe(console.log);

// fromEvent(target, eventName) - creates and returns Observable of events of "target" but just events that have type "eventName" of event
const clicks = fromEvent(window, "click");
clicks.subscribe(event => console.log("clicked", event));

// fromFetch(url) - the wrapper of Fetch API that creates and returns Observable of Response that is a result of Request to "url"
// switchMap - see https://rxjs-dev.firebaseapp.com/api/operators/switchMap
const posts = fromFetch("https://jsonplaceholder.typicode.com/posts");
posts
  .pipe(switchMap(response => response.json()))
  .subscribe(res => console.log(res));

```
