# Posts App Assessment

## Considerations

I ran into problems setting up my personal machine in order to install some testing libraries, using the newest Angular version and NgRx. Because I spent some time trying to fix it, and in the end wrote some mock State for testing, I didn't spend much time making sure the layout is fully responsive.

The styling choice for opening a post was initially thought as using `grid-column: span 3` and `grid-row: span 2`, but I noticed that this would always move everything around and it could be a bit of an annoying experince in a real case. So I chose `position: absolute` in order to keep everything in place.

Other than that, I'd like to thank you for the opportunity of showing my skills.

## Project versions and running

This project uses Angular version 18.2.9 and node 20+. I ran using node 22.9, in case any problems appear.

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`.

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Assessment Q&A

### 1. We use JWTs a lot throughout our API. For instance, when a user logs in on our API, a JWT is issued and our web-application uses this token for every request for authentication. Why is it (or isn't it) safe to use this?

JWT are very secure when used correctly, as they offer:
* Integrity, since they're based on a secret key that is only known to the server and can be validated on the server side
* Stateless communication, as the server doesn't need to maintain session information

It becomes a risk when not correctly implemented, being prone to token theft and malicious attacks. Steps to prevent this include:
* Using https
* Avoid storing sensitive information in the token payload
* Have a short expiration for the tokens

### 2. In our web-application, messages sent from one user to another, can contain HTML, which poses some security risks. Describe two attack vectors bad actors might try to abuse? And how would you mitigate these vectors?

1. Cross-Site Scripting
In this, users can add scripts into messages, which will be then executed in the recipient's browser. It can, for example, steal the recipient's cookies. We can avoid this by having a CSP, which will limit the sources from which Javascript can be loaded. It also prevents inline code from executing. And a second mitigation would be encoding user input when rendering HTML.

2. HTML Injection
This ones come in the form of unauthorized HTML content into the application. One of the usages of this is rendering elements that user can interact with and be tricked, like a fake form. To avoid this, we can use HTML and Angular DomSanitizer to escape potentially dangerous tags and attributes. External links limation is also another way of avoiding it.

### 3. Explain the difference between mutable and immutable objects. 
Mutable objects can be changed after they are created, while immutable objects cannot be modified. In Javascript, primitives types like numbers and strings are immutable. For example:
```
    let myStr = 'Angular';
    myStr[0] = 'B';
    console.log(myStr); // Angular
```

But for objects and arrays, you can modify them by changing directly their properties, using `push()` and etc.

#### What is an example of an immutable object in JavaScript? 
```
    // mutable
    const myObj = {
        id: 1,
        name: 'Gustavo'
    };

    myObj.name = 'Gustavo Castro';

    console.log(myObj); // { id: 1, name: 'Gustavo Castro' }

    // immutable
    const myFrozenObj = Object.freeze({
        age: 28,
        city: 'Utrecht'
    });

    myFrozenObj.city = 'Amsterdam';

    console.log(myFrozenObj); // { age: 28, city: 'Utrecht' }
```
#### What are the pros and cons of immutability?
Pros:
* Predictability, since they cannot be changed after creation
* Prevents unexpected behaviour cause by side effects
* Easier to debug, as the new changes results in a new object

Cons:
* Overhead, since creating new objects might be costly for processing time
* More garbage collection, as we must clean these old objects
* It can lead to a more verbose code

#### How can you achieve immutability in your own code? 
* Using `const` to avoid it being reassigned
* `Object.freeze()` as in the example
* Using functions such as `map()`, `filter()`, `reduce()`
* Copying objects by using spread operator or `Object.assign()`

### 4. If you would have to speed up the loading of a web-application, how would you do that?
1. Use performance tools to identify possible bottlenecks
2. Optimize rendering by avoid async operation in `ngOnInit` and reducing DOM complexity and
3. Use lazy loading techniques to load only what's necessary at the moment
4. Reduce third-party dependencies
5. Introduce efficient caching of files that can be reused on following requests
6. Reduze the size of requests
7. Optmize fonts and images
8. Minify and bundle the assets of the application