# GTU 05

## pseudo scopes and cheating scopes

#### 1. try-catch-finally

* gtu the following examples:

  1. try
      ```javascript
      console.log(a, b, c);
      try {
        var a = hello;
        var b = function() {
          console.log(this);
        };
        function c() {
          console.log(this);
        }
      } catch(e) {
      }
      ```
  2. catch
      ```javascript
      console.log(a, b, c, e);
      try {
        throw new Error('hello');
      } catch(e) {
        var a = hello;
        var b = function() {
          console.log(this);
        };
        function c() {
          console.log(this);
        }
      }
      ```
  3. finally
      ```javascript
      console.log(a, b, c);
      try {
        throw new Error('hello');
      } catch(e) {
      } finally {
        var a = hello;
        var b = function() {
          console.log(this);
        };
        function c() {
          console.log(this);
        }
      }
      ```

* take a look to previous examples and understand which one of the blocks can be
considered as pseudo-scope and why

#### 2. eval

* gtu the following examples

  1. eval var vs normal var
      ```javascript
      !function() {
        var a = 12;
        console.log(a, b, c);
        var b = 3;
        eval('var c = 3;');
      }();
      ```
      &
      ```javascript
      !function() {
        var a = 12;
        eval('var c = 3;');
        var b = 3;
        console.log(a, b);
      }();
      ```

  2. use the following code instead of the previous to understand
    engine behaviour

      ```javascript
      !function(codeStr) {
        var a = 12;
        console.log(a, b, c);
        var b = 3;
        eval(codeStr);
      }('var c = 3;');
      ```
      &
      ```javascript
      !function(codeStr) {
        var a = 12;
        eval(codeStr);
        var b = 3;
        console.log(a, b, c);
      }('var c = 3;');
      ```

  3. eval function
      ```javascript
      !function() {
        console.log(f, g);
        var g = eval('function f() { }');
      }();
      ```
      &
      ```javascript
      !function() {
        var g = eval('function f() { }');
        console.log(f, g);
      }();
      ```

  4. eval values(complex values vs primitives)
      ```javascript
      var values = ['1 + 2', 1 + 1, false, true, {x: 3},
        new String('2 + 2'), new Number(1), new Boolean(false),
        new Object('2 + 2'), new Object(1), new Object(true),
        /hello/ig, undefined, null, NaN, 'null', 'undefined',
        '"hello"', '"hello" + 1', '0 / 0', 'Infinity / Infinity',
        'NaN', 'Infinity', 'isNaN(Object)', '({x: 3})', '{x: 3}',
        '(function() { })', 'function() { }', eval
      ];

      for(var i = 0; i < values.length; i++) {
        console.log(eval(values[i]));
      }
      ```

  5. undeclared and declared variables
      ```javascript
      var a = 8;

      b = 9;
      eval('a = 3; b = 13; c = 23;');

      console.log(a, b, c);
      ```

  6. example of local functions
      ```javascript
      var d0 = eval('new Date()');
      function eval2() {
        function Date() { return {} }
        
        return eval('new Date()');
      }
      var d1 = eval2();
      console.log(d0, d1);
      ```
      &
      ```javascript
      var d0 = eval('new Date()');
      function eval2(str) {
        function Date() { return {} }
        
        return eval(str);
      }
      var d1 = eval2('new Date()');
      console.log(d0, d1);
      ```
      
* try all the following examples with appending `'use strict'` before the code
  (e.g. instead of `eval('4')` use `eval('"use strict"; 4')`);

> for creating similar experience you can always use the `Function` constructor

```javascript
function myEval(codeStr) {
  var fn = new Function('return ' + codeStr + ';');
  return fn();
}
```

* try all the previous examples with `myEval`

#### 3. `with` statement

* gtu the following examples
```javascript
var a = {
  x: {},
  y: 1,
  a: 2
};
var b = 3;
var c = 4;
var d = 5;
```

  1. access
      ```javascript
      console.log(a, b, c, d);
      with(a) {
        console.log(a, b, c, d);
      }
      console.log(a, b, c, d);
      ```

  2. assign
      ```javascript
      with(a) {
        a = 1;
        b = 1;
        c = 1;
        d = 1;
        x = 1;
        y = 1;
      }
      console.log(a, b, c, d);
      ```

  3. delete
      ```javascript
      with(a) {
        delete a;
        delete b;
        delete c;
        delete d;
        delete x;
        delete y;
      }
      console.log(a, b, c, d);
      ```

#### 4. try all the examples also in strict mode