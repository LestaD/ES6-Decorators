# ES6-Decorators
Shows example of using decorators


## Deprecated
You can define deprecated decorator

```es6
function Deprecated(why) {
  if (typeof why !== 'string') {
    why = '';
  }
  return function(target, key, descriptor) {
    let className = target.constructor.name;
    let old = descriptor.value;
    descriptor.value = function(...args) {
      console.error(`DEPRECATE: Method ${className}.${key}() is deprecated. ${why}`);
      return old(...args);
    }
    return descriptor;
  }
}
```

You can mark your deprecated methods with `@deprecated()`. And when you call your deprecated method in console you will see the warning.

```es6
class APIv6 extends APIBase {
  filterData(keys) {
    /* ... */
  }
  
  @Deprecated('Use filterData(String keys)')
  regexpData(wtf=1) {
    return this.method();
  }
}
```

```
 DEPRECATE: Method APIv6.regexpData() is deprecated. Use filterData(String keys)
```

For simple deprecate without description you can write this:

```es6
function deprecated(target, key, descriptor) {
  let className = target.constructor.name;
  let old = descriptor.value;
  descriptor.value = function(...args) {
    console.error(`DEPRECATE: Method ${className}.${key}() is deprecated.`);
    return old(...args);
  }
  return descriptor;
}
```

Simple using:

```es6
class APIv5 extends APIBase {
  @deprecated
  logout() {
    this.clearToken(this.user);
  }
}
```

```
// DEPRECATE: Method APIv5.logout() is deprecated.
```

