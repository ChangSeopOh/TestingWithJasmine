# Testing with Jasmine framework

- Unit testing involves testing pieces of functionality
- Jasmine is a testing framework that allows us to easily write unit tests
- Jasmine has quite a few matchers for testing almost any kind of expectation
- Using beforeEach / afterEach / beforeAll / afterAll hooks can help reduce duplication and confusion
- Jasmine provides spies for mimicking the behavior of a function
- Jasmine provides a clock object for testing timers and a callback function for testing asynchronous code
- Unit testing is just one part of testing applications








===========================================================
# Essential Keywords
- describe  = 'let me describe _____ to you.'
- it        = 'let me tell you about _____'
- expect    = 'here's what i expeect'
===========================================================
# A conceptual exercise
describe("Earth")
    it("is round")
       expect(earth.isRound.toBe(true))
    if("is the third planet from the sun")
       expect(earth.numberFromSun).toBe(3)

============================================================
Psuedo Code

```JS
var earth = {
    isRound: true,
    numberFromSun : 3
}
describe("Earth", function(){
    it("is round", function(){
        expect(earth.isRound).toBe(true)
    });
    it("is  the third planet from the sun", function(){
         expect(earth.numberFromSun).toBe(3)
    });
})
```

===========================================================
# Matchers
- toBe / not.toBe
- toBeClostTo 
- toBeDefined
- toBeFalsey/toBeTruthy
- toBeGraterThan/toBeLessThan
- toContain
- toEqual (check references in memory)
- jasmine.any()
===========================================================
Examples (Matchers)
```JS
     describe("Jasmine Matchers", function() {
      it("allows for === and deep equality", function() {
        expect(1+1).toBe(2);
        expect([1,2,3]).toEqual([1,2,3]);
      });
      it("allows for easy precision checking", function() {
        expect(3.1415).toBeCloseTo(3.14,2);
      });
      it("allows for easy truthy / falsey checking", function() {
        expect(0).toBeFalsy();
        expect([]).toBeTruthy();
      });
      it("allows for easy type checking", function() {
        expect([]).toEqual(jasmine.any(Array));
        expect(function(){}).toEqual(jasmine.any(Function));
      });
      it("allows for checking contents of an object", function() {
        expect([1,2,3]).toContain(1);
        expect({name:'Elie', job:'Instructor'}).toEqual(jasmine.objectContaining({name:'Elie'}));
      });
    });
```

===========================================================
- beforeEach
: run before each "it" callback

Examples (beforeEach)
```JS
 describe("Arrays", function() {
     var arr;
     beforeEach(function(){
         arr = [1,3,5];
     })
      it("adds elements to an array", function() {
        arr.push(7);
        expect(arr).toEqual([1,3,5,7]);
      });
      it("returns the new length of the array", function() {
        expect(arr.push(7)).toBe(4);
      });
      it("adds anything into the array", function() {
        expect(arr.push({})).toBe(4);
      });
 });

```

===========================================================
- afterEach
: run after each "it" callback - useful for teardown

Examples (afterEach)
```JS
 describe("Counting", function() {
     var count =0;
     beforeEach(function(){
        count++;
     });
     afterEach(function(){
        count =0;
     })
     
      it("has a counter that increments", function() {
        expect(count).toBe(1);
      }); 

      it("gets reset", function() {
        expect(count).toBe(1);
      }); 
 });
```

===========================================================
- beforeAll / afterAll
: run before/after all tests! Does not reset in between

Examples (beforeAll)
```JS
 var =[];
 beforeAll(function(){
     arr = [1,2,3];
 })
 describe("Counting", function() {
    it("starts with an array", function(){
        arr.push(4);
        expect(1).toBe(1);
    });
    it("keeps mutating that array", function(){
        console.log(arr); //[1,2,3,4]
        arr.push(5);
        expect(1).toBe(1);
    });
 });

 describe("Again", function() {
    it("keeps mutating the array...again", function(){
        console.log(arr); //[1,2,3,4,5] 
        expect(1).toBe(1);
    });
 });
```

===========================================================
- Nesting describe
```JS
describe("Array", function(){
  var arr;
  beforeEach(function(){
    arr = [1,3,5];
  });
  describe("#unshift", function(){
    it("adds an element to the beginning of an array", function(){
      arr.unshift(17);
      expect(arr[0]).toBe(17);
    });
    it("returns the new length", function(){
      expect(arr.unshift(1000)).toBe(4);
    });
  });
  describe("#push", function(){
    it("adds elements to the end of an array", function(){
      arr.push(7);
      expect(arr[arr.length-1]).toBe(7);
    });
    it("returns the new length", function(){
      expect(arr.push(1000)).toBe(4);
    });
  });
```

===========================================================
- Pending tests
: Sometimes you just don't know

```JS
describe("Pending specs", function(){

  xit("can start with an xit", function(){
    expect(true).toBe(true);
  });

  it("is a pending test if there is no callback function");

  it("is pending if the pending function is invoked inside the callback", function(){
    expect(2).toBe(2);
    pending();
  });

});
```

===========================================================

# Spies
- Jasmine has test double functions called spies.
- A spy can stub (mimic) any function and track calls to it and all arguments.
- Spies only exists in the describe or it block in which it is defined,
- Spies are removed after each spec.
- There are special matchers for interacting with spies.

===========================================================
- Creating a spy

```JS
function add(a,b,c){
  return a+b+c;
}

describe("add", function(){
  var addSpy, result;
  beforeEach(function(){
    addSpy = spyOn(window, 'add');
    result = addSpy();
  })
  it("is can have params tested", function(){
    expect(addSpy).toHaveBeenCalled();
  });
});
```
===========================================================
- Testing parameters

```JS
function add(a,b,c){
  return a+b+c;
}
describe("add", function(){
  var addSpy, result;
  beforeEach(function(){
    addSpy = spyOn(window, 'add');
    result = addSpy(1,2,3);
  });
  it("is can have params tested", function(){
    expect(addSpy).toHaveBeenCalled();
    expect(addSpy).toHaveBeenCalledWith(1,2,3);
  });
});
```
===========================================================
- Returning parameters
: To test small unitets of codes, so don't need to invokie lots of functions that depend on other ones.

```JS
function add(a,b,c){
  return a+b+c;
}
describe("add", function(){
  var addSpy, result;
  beforeEach(function(){
    addSpy = spyOn(window, 'add').and.callThrough();
    result = addSpy(1,2,3);
  })
  it("can have a return value tested", function(){
    expect(result).toEqual(6);
  });
});
```

===========================================================
- Testing frequency
: how many times function is called.


```JS
function add(a,b,c){
  return a+b+c;
}
describe("add", function(){
  var addSpy, result;
  beforeEach(function(){
    addSpy = spyOn(window, 'add').and.callThrough();
    result = addSpy(1,2,3);
  })
  it("is can have params tested", function(){
    expect(addSpy.calls.any()).toBe(true);
    expect(addSpy.calls.count()).toBe(1);
    expect(result).toEqual(6);
  });
});
```
===========================================================
# Clock

- The Jasmine Clock is available for testing time dependent code.
- It is installed by invoking jasmine.clock().install()
- Be sure to uninstall the clock after you are done to restore the original functions.
===========================================================
- setTimeout
```JS
describe("a simple setTimeout", function(){
  var sample;
  beforeEach(function() {
    sample = jasmine.createSpy("sampleFunction");
    jasmine.clock().install();
  });

  afterEach(function() {
    jasmine.clock().uninstall();
  });

  it("is only invoked after 1000 milliseconds", function(){
    setTimeout(function() {
      sample();
    }, 1000);
    jasmine.clock().tick(999);
    expect(sample).not.toHaveBeenCalled();
    jasmine.clock().tick(1);
    expect(sample).toHaveBeenCalled();
  });
});
```
===========================================================
- setInterval
```JS
describe("a simple setInterval", function(){
  var dummyFunction;

  beforeEach(function() {
    dummyFunction = jasmine.createSpy("dummyFunction");
    jasmine.clock().install();
  });

  afterEach(function() {
    jasmine.clock().uninstall();
  });

  it("checks to see the number of times the function is invoked", function(){
    setInterval(function() {
      dummyFunction();
    }, 1000);
    jasmine.clock().tick(999);
    expect(dummyFunction.calls.count()).toBe(0);
    jasmine.clock().tick(1000);
    expect(dummyFunction.calls.count()).toBe(1);
    jasmine.clock().tick(1);
    expect(dummyFunction.calls.count()).toBe(2);
  });
});
```

===========================================================
# Testing async code

- Jasmine also has support for running specs that require testing async code    
- beforeAll, afterAll, beforeEach, afterEach, and it take an optional single argument (commonly called 'done') that should be called when the async work is complete.
- A test will not complete until its 'done' is called.   
- Jasmine will wait 5 seconds by default and you can modify the internal timer with jasmine.DEFAULT_TIMEOUT_INTERVAL (done callback to run or the test will timeout.)
===========================================================
- Async tests
: notice 'done' being passed and used in the callback function
```JS
function getUserInfo(username){
  return $.getJSON('https://api.github.com/users/' + username); //call back
}
describe("#getUserInfo", function(){
  it("returns the correct name for the user", function(done){
    getUserInfo('changseop').then(function(data){
      expect(data.name).toBe('Changseop Oh');
      done();
    });
  });
}); 
```


===========================================================
# TDD (Test Driven Development)

1. Write the tests
2. See the tests fail
3. Write code to pass the tests
4. Refactor code as necessary
5. Repeat


===========================================================
# BDD (Behavior Driven Development)

- A subset of TDD
- Not mutually exclusive with TDD
- Involves being verbose with our style and describing the behavior of the functionality
- Helpful when testing the design of the software