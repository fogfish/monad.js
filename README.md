# monad.js

The experimental library implements [State-Action-Model](http://jdubray.github.io/sam/) pattern using IO monads.

## Inspiration 

The [State-Action-Model](http://jdubray.github.io/sam/) pattern facilitate reactive User Experience development, using just plain old JavaScript function. This pattern addresses REST API development issues such as API inflation (api per screen, back-end for front-end /BFF/). Secondly, it solves challenges of MVC-based frameworks with poorly designed interfaces which induces the programmer to mix multiple concerns in one component. 

We can develop SAM pattern further. The pattern uses "Temporary Logic of Actions" to proof that actions are pure functions - the computation that produces event. We need to pass this result (event) to further computation and so on. The bind (`>>=`) operation allow us to formalize a computation which runs `x`, then applies `f` to its result, getting a computation which it then runs `x >>= f`. The library exposes SAM pattern in terms of IO monads to benefit function binding in asynchronous environment.


## Getting started

The latest version of the library is available at its master branch. All development, including new features and bug fixes, take place on the master branch using forking and pull requests as described in contribution guidelines.


### Installation

The library is hosted using http://rawgit.com service. Use the following code to link the file
```
<script src="https://cdn.rawgit.com/fogfish/monad.js/master/src/monad.js"></script>
```

### Usage

The library provides a Rocket Launch example as SAM pattern show case, it uses 
* d3.js for I/O and DOM manipulation
* whiskers.js to build a pure view function 


### SAM pattern

Please see references for detailed definition of SAM pattern. In the nut shell, it is formalized as
```
V = S( vm( M.present( A(data) ) ), nap(M))
```

Using IO monad formalism it becomes
```javascript
monad.do([
   monad.IO(A),        
   M.present,
   vm,
   S,
   nap
])
```


## Library interface

### `monad.IO` 

Build a monadic computation from asynchronous i/o (e.g. HTTP GET). The i/o function takes an acceptor function as parameter, which is used by i/o routine to propose accepted value to computation chain.
```javascript
monad.IO(
   function(acceptor)
   {
      d3.json("https://example.org/json", acceptor)
   }
).bind(
   function(json){
      console.log(json)
   }
)
```

### `monad.UI`

Build a monadic computation from asynchronous DOM event (e.g. user click). The event handler function takes and acceptor function as parameter, which is used by routine to propose accepted event to computation chain.
```javascript
monad.UI(
   function(acceptor)
   {
      d3.select('button').on('click', function(_){acceptor(d3.event)}
   }
).bind(
   function(event){
      console.log(event)
   }
)
```

### do-notation (`monad.do`)

The library implements a do-notation `monad.do([...])` as a syntax-sugar of multiple `bind` (`>>=`) operations.
```javascript
monad.do([
   monad.IO(function(_){...}),
   function(x){return x;},
   function(x){return x;},
   function(x){return x;}
])
```

### chaining multiple IO

It might be required to chain multiple IO operation. The definition of bind operation says that computation function of type `a -> m b`. Just return a monadic computation of different type 
```
(>>=) :: (Monad m) => m a -> (a -> m b) -> m b
```

```javascript
monad.do([
   monad.IO(function(_){...}),
   function(x){return monad.IO(function(_){...});}
])
```



## How to Contribute

The library is MIT licensed and accepts contributions via GitHub pull requests.

### getting started

* Fork the repository on GitHub
* Read the README.md for build instructions


### commit message

The commit message helps us to write a good release note, speed-up review process. The message should address two question what changed and why. The project follows the template defined by chapter [Contributing to a Project](http://git-scm.com/book/ch5-2.html) of Git book.

>
> Short (50 chars or less) summary of changes
>
> More detailed explanatory text, if necessary. Wrap it to about 72 characters or so. In some contexts, the first line is treated as the subject of an email and the rest of the text as the body. The blank line separating the summary from the body is critical (unless you omit the body entirely); tools like rebase can get confused if you run the two together.
> 
> Further paragraphs come after blank lines.
> 
> Bullet points are okay, too
> 
> Typically a hyphen or asterisk is used for the bullet, preceded by a single space, with blank lines in between, but conventions vary here
>

## Bugs

If you detect a bug, please bring it to our attention via GitHub issues. Please make your report detailed and accurate so that we can identify and replicate the issues you experience:
- specify the configuration of your environment, including which operating system you're using and the versions of your runtime environments
- attach logs, screen shots and/or exceptions if possible
- briefly summarize the steps you took to resolve or reproduce the problem



## References

* http://www.infoq.com/articles/no-more-mvc-frameworks
* http://jdubray.github.io/sam/



## License

Copyright 2016 Dmitry Kolesnikov

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.




