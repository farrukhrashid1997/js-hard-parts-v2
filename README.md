# js-hard-parts-v2
Notes from the FEM JS - The hard parts v2 course.

Functions and Callbacks
- There is only one thread, which means only one thread of execution runs at a time. Either the global execution or the local function execution. 
- The callstack stores the function that is running currently
- At the bottom of the call stack is the global(), which starts running once the function finishes and is removed. 
- A function finishes as soon as it sees the return keyword. 
- Every context has a thread of execution, functions, global context.  
- A new function means a new execution context
- When you are passing an variable, for example:
  
  const numArr = [1,2,3]
  function multipleBy2(ar){
    //multiply each element of the array
  }
  const multiplied = multiplyBy2(numArr)
  
  - In this case, the variable num, when passed in the function, the paramter arr refers to the value of         numArr. 
  - This means, that arr is referencing the value of numArr. If we manipulate the array directly (mutate), 
    there will be a, what we call a side effect.
    
 - So the principle should be that arrays should remain unmutated. 
 - Primitive values are copied to the new variable.
 - Variables which are non-primitive, are given a reference to that value. (array, object, functions)
 - Functions are what we call, first class objects. 
 - They can be treated like objects
 -  The outer function which takes in the baby function - Higher order function 
 -  The function that is passed into the HOF is called the callback. 
 -  Any function that takes in a function or returns a function is called a HOF.

Closures
- The state is usually the live data at that particular moment of execution.
- We can achieve memoizaition, using closures. 
- Many design patterns use closures. 
- A function definition is basically a value. 
- Javascript is a syncrhonous language. 
- Lets take an example:
  function outer(){
    let counter = 0;
    function incrementCounter() {
      counter++;
    }
    incrementCounter();
   }
Now, when increment counter is called, the execution context of incrememntCounter starts. When it runs it, the counter variable inside its own execution context isnt found, so then it goes to the very next execution context from the callstack. (down in the callstack)

- Lets take an example: 

  function outer(){
  let counter = 0;
  function incrementCounter(){counter++}
  return incrementCounter;
  }
  
  const myNewFunction = outer();
  myNewFunction();
  myNewFunction();
  
  Here's what is happening:
   - The function outer is run, and returns the value (function definition of incrementCounter to myNewFunction.
   - Now when myNewFunc runs, it doesnt find the counter variable in its local memory, so it checks the global memory, its not there, so what do we do?
   - JS basically, when a function is returned, it stores in its backpack the surrounding variables from the memory. 
   - So now, when myNewFunction will not find it in its local memory, it will go out and check the backpack of the function definition before going to the global context. 
   - This elegant feature can be used to limit the number of times a function can run. 
   - The result of running this function would be 2.
- What about, if a variabe is not being used in the inner function, will that be included in the backpack?
  - In modern implementation, JS checks if the variable referenced is being used in the inner function, if not its not included in the backpack. This is to 
    ensure to avoid a memory leak. 

- When a function within a function is declared, you gotta bond to the local variable environment through the hidden scope property. 
- JS is a lexically/static scoped language. 
- Local memory is sometimes called the variable environment. 
- This backpack is called sometimes C.O.V.E (Closed over variable environment)
- Another more famous name is, which shows your understanding is: Persistent Lexical Scope Referenced Data (P.L.S.R.D)
- Scope is at any line of the execution, what data do I have available to me. 
- JS is a lexically statically scoping language. 
- Because of this rule, the data from local memory of the outer function is returned with the inner function. 
- When inner function is returned out, the live data is attached through a hidden property called scope. 

Asynchronous Javascript

- Javascript is a synchronous language, we finish one line and then go to the next one. 
- As of now there are three things in the JS engine, the callstack, the thread of execution and the local memory.
- Typically a browser is running Javascript. 
- Javascript doesnt have the ability to make network requests. 
- The browser is an app which has other features apart from running Javascript
- Browser enables:  network requests(xhr/fetch), Rendering (its in the web browser, the HTML DOM(Document)), Timer (setTimeout)
- Javascript even does not have a feature of a timer
- Javascript gets the features mentioned above through the browser, through maybe an interface
- Basically Javascript has a setTimer function which will mainly call the timer api in the browser and move on. 
- So this means the timer wont be blocking the anything in the callstack.
- setTimeout(() => console.log("yo"), 1000) - This code, this line will call the browser api called timer, the callback defined will be pushed to the CALLBACK QUEUE. Until the sycronous code from the global context is fully run, nothing will run from the callback queue. 
- Now, at every line of execution, there is a little feature in Javascript which will check if the callstack is empty, if yes, it will check the callback queue to run any pending function, if it not empty (Callstack), it will not even bother to check the callback queue. This little feature is called the Event loop. 
- And, thus this was the entire model until es6 of the whole javascript. 
- Event loop will pick up the code from callback queue and put it on the callstack, if the callstack is empty. Event loop is pretty amazing.
- Event loop is constantly running as long as our application is running. 
- Imagine if you re calling a twitter api, now the function that should run on completion of that api (the callback) is going to get the data returned from the api. 
- This means that our response data is only available in the callback function. 

Promises
- When you make a fetch call, the browser will make a fetch request from the browser features, simultaneously its going to also, in JS return out a special object called a Promise, which sits in memory. 
- When the background function resolves and gets data, it fill the promise which is sitting in the memory. 
- A JS object called Promise is returned from the fetch api. 
- The object has two properties. Value and onFulfilled. Value is undefined. 
- The onfullfilled is hidden property, and an empty array. 
- XHR is the XML HTTP Request. 
- When the fetch request is completed, the data returned will set to the value property of the Promise object. 
- We can add methods and functions inside the onFulfilled, which JS will run automatically when the value property gets filled in. 
- One more thing that happens, the data when returned will be automatically passed as argument to the functions inside the onFulfilled list.
- We cant access the onFullfilled property, since its a hidden property. 
- The then method basically behind the scenes, does var.onFulfilled.push() for the function passed in the then() method.
