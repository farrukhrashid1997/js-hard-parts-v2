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
