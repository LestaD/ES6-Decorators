# ES6-Decorators
Shows example of using decorators


## Deprecated
You can define deprecated decorator

```js
function Deprecated(why) {
  if (typeof why !== 'string') {
    why = '';
  }
  return function(target, key, descriptor) {
    let className = target.constructor.name;
    let old = descriptor.value;

    descriptor.value = function(...args) {
      let that = this;
      console.warn(`DEPRECATE: Method ${className}.${key}() is deprecated. ${why}`);
      return old.call(that, ...args);
    }
    return descriptor;
  }
}
```

You can mark your deprecated methods with `@Deprecated('description')`. And when you call your deprecated method in console you will see the warning.

```js
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

For simple deprecate without description you can write:

```js
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

### Usage

```js
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

## Say

When call method, say to console

```js
function Say(what) {
  if (typeof what !== 'string') {
    what = 'Pings!';
  }
  return function(target, key, descriptor) {
    let className = target.constructor.name;
    let old = descriptor.value;

    descriptor.value = function(...args) {
      let that = this;
      console.info(`%c${className}.${key} said: %c ${what}`, 'color: gray', 'color: black');
      return old.call(that, ...args);
    }
    return descriptor;
  }
}
```

### Usage

```js
class APIv2 extends APIBase {
  @Say('Filtering data is very slow process')
  filterData(keys) {
    /* ... */
  }
}
```

```
APIv2.filterData said: Filtering data is very slow process
```


## Warning

Same like `Deprecated`

```js
function Warning(what) {
  if (typeof what !== 'string') {
    what = '';
  }
  return function(target, key, descriptor) {
    let className = target.constructor.name;
    let old = descriptor.value;

    descriptor.value = function(...args) {
      let that = this;
      console.warn(`Warning in ${className}.${key}(): ${what}`);
      return old.call(that, ...args);
    }
    return descriptor;
  }
}
```

### Usage

```js
class Player extends BasePlayer {
  constructor(...args) {
    super(...args);
    this.kill();
    this.health = 0;
  }
  
  @Warning("Don't call from code! It's break game logic!")
  kill() {
    this.fireEvent('onkill');
  }
}
```

```
Warning in Player.kill(): Don't call from code! It's break game logic!
```


## Timer

Simple log how long you method execute:

```js
function Timer() {
  return function(target, key, descriptor) {
    let className = target.constructor.name;
    let old = descriptor.value;

    descriptor.value = function(...args) {
      let that = this;
      let timer = null;
      let result = null;
      console.info(`%cRun ${className}.${key}(${args.join(',')})`, 'color: blue');

      timer = 'End ' + className + '.' + key + '(' + args.join(',') + ')';
      console.time(timer);

      result = old.call(that, ...args);
      console.timeEnd(timer);
      return result;
    }
    return descriptor;
  }
}
```

### Usage

```js
class UserPage extends Controller {
  @Timer()
  actionHome() {
    this.render('user/home', { user: this.global.user });
  }
}
```

Console:

```
Run UserPage.actionHome()
End UserPage.actionHome(): 3.765ms
```



