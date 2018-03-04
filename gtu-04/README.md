# GTU 04

## for, for-in, context & strict mode

#### 1. for vs for-in

* test and compare the printed results of this two loops:
```javascript
for(var i = 0; i < a.length; i++) {
    console.log('for', i, a[i]);
}
for(var key in a) {
    console.log('for-in', key, a[key]);
}
```
for the following examples:
  1. `var a = [1, 2, 3,,,,7];`
  2. `var a = []; a.length = 23;`
  3. `var a = new Array(12);`
  4. `var a = [1, 2, 3]; a[17] = 23; a[7] = 12;`
  5. `var a = [1, 2, 3]; a[7] = 12; a[17] = 23;`

* try all the previous examples with adding `a.x = 23.12; a.y = 12.23;`

* try to write a the following loop with `while` and `do-while`:
```javascript
for(var i = 0; i < a.length; i++) {
    console.log('for', i, a[i]);
}
```

#### 2. context

* guess the context

```javascript
function f() {
  console.log(this);
}
var a = {
  g: function() {
    console.log(this);
  }
}
var g = a.g;
```

  1. `f();`
  2. `a.g();`
  3. `g();`
  4. `a.f = f; a.f();`
  5. `a.g2 = g; a.g2();`
  6. `new f();`
  7. `new a.f();`
  8. `new a.g();`
  9. `new a.g2();`
  10. call
      ```javascript
      var values = [
        a, {}, f, g, Array, [], undefined,
        null, false, true, 0, 1, NaN, 'hello', '',
        /hello*/i, val, i, [].slice, new Date()
      ];
      for(var i = 0; i < values.length; i++) {
        var val = values[i];

        f.call(val);
        g.call(val);
        a.f.call(val);
        a.g.call(val);
        a.g2.call(val);
      }
      ```
  11. try `10.` with `apply`
  12. a use case of call:
      ```javascript
      var arrayLike = {
        0: 10, 1: 11,
        2: 12, 3: 13,
        4: 14, 5: 15,
        length: 6
      };
    
      var arr = [].slice.call(arrayLike);
      ```

#### 3. try all the examples also in strict mode