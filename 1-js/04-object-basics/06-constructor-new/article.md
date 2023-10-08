# Constructor, operatorul „new”

Sintaxa `{...}` ne permite să creăm un nou obiect. Dar de multe ori trebuie să creăm mai multe obiect asemănătoare, cum ar fi mai mulți utilizatori sau mai mulți itemi pentru meniuri etc.  

Acest proces poate fi obținut prin utilizarea funcțiilor de tip constructor și a operatorului `"new"`.

## Funcția de tip constructor

Funcțiile de tip constructor sunt de fapt funcții obișnuite. Însă, exista două convenții:

1. Acestea sint numite cu literă mare.
2. Aceste trebuie să fie executate împreună cu operatorul `"new"`.

De exemplu:

```js run
function User(name) {
  this.name = name;
  this.isAdmin = false;
}

*!*
let user = new User("Jack");
*/!*

alert(user.name); // Jack
alert(user.isAdmin); // false
```

Când o funcție este executată cu operatorul `new`, aceasta urmează următorii pași: 

1. Un nou obiect este creat și este atribuit lui `this`.
2. În momentul executării funcției, `this` este modificat, adică îi sunt atribuite noi proprietăți. 
3. În final este returnată valoarea lui `this`.

Prin alte cuvinte, ceea ce face `new User(...)` este:

```js
function User(name) {
*!*
  // this = {};  (implicit)
*/!*

  // this primește noi proprietăți
  this.name = name;
  this.isAdmin = false;

*!*
  // return this;  (implicit)
*/!*
}
```

Deci `let user = new User("Jack")` are același rezultat precum:

```js
let user = {
  name: "Jack",
  isAdmin: false
};
```

Dacă dorim să creăm alți utilizatori, putem executa funcția `new User("Ann")`, `new User("Alice")` și tot așa. Este mult mai rapid decât să folosești literals de fiecare dată, și de asemenea mult mai ușor de citit.

Acesta este este principalul scop al constructorilor, să implementeze cod reutilizabil pentru crearea de noi obiecte.

<!-- Let's note once again -- technically, any function (except arrow functions, as they don't have `this`) can be used as a constructor. It can be run with `new`, and it will execute the algorithm above. The "capital letter first" is a common agreement, to make it clear that a function is to be run with `new`. -->
Haideți să obesrvăm încă odată -- din punct de vedere tehnic, orice funcție (cu excepția funcțiilor de tip arrow, deoarece acestea nu au dețin `this`) pot fi folosite drept constructori. Poate fi folosită 

````smart header="new function() { ... }"
If we have many lines of code all about creation of a single complex object, we can wrap them in an immediately called constructor function, like this:

```js
// create a function and immediately call it with new
let user = new function() { 
  this.name = "John";
  this.isAdmin = false;

  // ...other code for user creation
  // maybe complex logic and statements
  // local variables etc
};
```

This constructor can't be called again, because it is not saved anywhere, just created and called. So this trick aims to encapsulate the code that constructs the single object, without future reuse.
````

## Constructor mode test: new.target

```smart header="Advanced stuff"
The syntax from this section is rarely used, skip it unless you want to know everything.
```

Inside a function, we can check whether it was called with `new` or without it, using a special `new.target` property.

It is undefined for regular calls and equals the function if called with `new`:

```js run
function User() {
  alert(new.target);
}

// without "new":
*!*
User(); // undefined
*/!*

// with "new":
*!*
new User(); // function User { ... }
*/!*
```

That can be used inside the function to know whether it was called with `new`, "in constructor mode", or without it, "in regular mode".

We can also make both `new` and regular calls to do the same, like this:

```js run
function User(name) {
  if (!new.target) { // if you run me without new
    return new User(name); // ...I will add new for you
  }

  this.name = name;
}

let john = User("John"); // redirects call to new User
alert(john.name); // John
```

This approach is sometimes used in libraries to make the syntax more flexible. So that people may call the function with or without `new`, and it still works.

Probably not a good thing to use everywhere though, because omitting `new` makes it a bit less obvious what's going on. With `new` we all know that the new object is being created.

## Return from constructors

Usually, constructors do not have a `return` statement. Their task is to write all necessary stuff into `this`, and it automatically becomes the result.

But if there is a `return` statement, then the rule is simple:

- If `return` is called with an object, then the object is returned instead of `this`.
- If `return` is called with a primitive, it's ignored.

In other words, `return` with an object returns that object, in all other cases `this` is returned.

For instance, here `return` overrides `this` by returning an object:

```js run
function BigUser() {

  this.name = "John";

  return { name: "Godzilla" };  // <-- returns this object
}

alert( new BigUser().name );  // Godzilla, got that object
```

And here's an example with an empty `return` (or we could place a primitive after it, doesn't matter):

```js run
function SmallUser() {

  this.name = "John";

  return; // <-- returns this
}

alert( new SmallUser().name );  // John
```

Usually constructors don't have a `return` statement. Here we mention the special behavior with returning objects mainly for the sake of completeness.

````smart header="Omitting parentheses"
By the way, we can omit parentheses after `new`, if it has no arguments:

```js
let user = new User; // <-- no parentheses
// same as
let user = new User();
```

Omitting parentheses here is not considered a "good style", but the syntax is permitted by specification.
````

## Methods in constructor

Using constructor functions to create objects gives a great deal of flexibility. The constructor function may have parameters that define how to construct the object, and what to put in it.

Of course, we can add to `this` not only properties, but methods as well.

For instance, `new User(name)` below creates an object with the given `name` and the method `sayHi`:

```js run
function User(name) {
  this.name = name;

  this.sayHi = function() {
    alert( "My name is: " + this.name );
  };
}

*!*
let john = new User("John");

john.sayHi(); // My name is: John
*/!*

/*
john = {
   name: "John",
   sayHi: function() { ... }
}
*/
```

To create complex objects, there's a more advanced syntax, [classes](info:classes), that we'll cover later.

## Summary

- Constructor functions or, briefly, constructors, are regular functions, but there's a common agreement to name them with capital letter first.
- Constructor functions should only be called using `new`. Such a call implies a creation of empty `this` at the start and returning the populated one at the end.

We can use constructor functions to make multiple similar objects.

JavaScript provides constructor functions for many built-in language objects: like `Date` for dates, `Set` for sets and others that we plan to study.

```smart header="Objects, we'll be back!"
In this chapter we only cover the basics about objects and constructors. They are essential for learning more about data types and functions in the next chapters.

After we learn that, we return to objects and cover them in-depth in the chapters <info:prototypes> and <info:classes>.
```
