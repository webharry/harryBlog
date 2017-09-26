---
title: angular.copy()方法
date: 2017-09-11 20:01:11
tags:
    - angular
    - JavaScript
---
### angular.copy()方法的定义
angular.copy()方法实现对象属性的深度拷贝，返回一个新创建的对象。
### 用法实例
```js
var a = {
        name: 'lucy',
        age: 20
    }
var b = angular.copy(a);
console.log('a=',a);
//{name: "lucy", age: 20}
console.log('b=',b);
//{name: "lucy", age: 20}
```
当我们改变a对象的属性，b对象属性并不会随之改变：
```js
a.name = 'angular';
console.log('a=',a);
//{name: "angular", age: 20}
console.log('b=',b);
//{name: "lucy", age: 20}
```
### angular.copy()源码
```js
/**
 * @ngdoc function
 * @name angular.copy
 * @module ng
 * @kind function
 *
 * @description
 * Creates a deep copy of `source`, which should be an object or an array.
 *
 * * If no destination is supplied, a copy of the object or array is created.
 * * If a destination is provided, all of its elements (for arrays) or properties (for objects)
 *   are deleted and then all elements/properties from the source are copied to it.
 * * If `source` is not an object or array (inc. `null` and `undefined`), `source` is returned.
 * * If `source` is identical to `destination` an exception will be thrown.
 *
 * <br />
 * <div class="alert alert-warning">
 *   Only enumerable properties are taken into account. Non-enumerable properties (both on `source`
 *   and on `destination`) will be ignored.
 * </div>
 *
 * @param {*} source The source that will be used to make a copy.
 *                   Can be any type, including primitives, `null`, and `undefined`.
 * @param {(Object|Array)=} destination Destination into which the source is copied. If
 *     provided, must be of the same type as `source`.
 * @returns {*} The copy or updated `destination`, if `destination` was specified.
 *
 * @example
  <example module="copyExample">
    <file name="index.html">
      <div ng-controller="ExampleController">
        <form novalidate class="simple-form">
          <label>Name: <input type="text" ng-model="user.name" /></label><br />
          <label>Age:  <input type="number" ng-model="user.age" /></label><br />
          Gender: <label><input type="radio" ng-model="user.gender" value="male" />male</label>
                  <label><input type="radio" ng-model="user.gender" value="female" />female</label><br />
          <button ng-click="reset()">RESET</button>
          <button ng-click="update(user)">SAVE</button>
        </form>
        <pre>form = {{user | json}}</pre>
        <pre>master = {{master | json}}</pre>
      </div>
    </file>
    <file name="script.js">
      // Module: copyExample
      angular.
        module('copyExample', []).
        controller('ExampleController', ['$scope', function($scope) {
          $scope.master = {};

          $scope.reset = function() {
            // Example with 1 argument
            $scope.user = angular.copy($scope.master);
          };

          $scope.update = function(user) {
            // Example with 2 arguments
            angular.copy(user, $scope.master);
          };

          $scope.reset();
        }]);
    </file>
  </example>
 */
function copy(source, destination) {
  var stackSource = [];
  var stackDest = [];

  if (destination) {
    if (isTypedArray(destination) || isArrayBuffer(destination)) {
      throw ngMinErr('cpta', "Can't copy! TypedArray destination cannot be mutated.");
    }
    if (source === destination) {
      throw ngMinErr('cpi', "Can't copy! Source and destination are identical.");
    }

    // Empty the destination object
    if (isArray(destination)) {
      destination.length = 0;
    } else {
      forEach(destination, function(value, key) {
        if (key !== '$$hashKey') {
          delete destination[key];
        }
      });
    }

    stackSource.push(source);
    stackDest.push(destination);
    return copyRecurse(source, destination);
  }

  return copyElement(source);

  function copyRecurse(source, destination) {
    var h = destination.$$hashKey;
    var key;
    if (isArray(source)) {
      for (var i = 0, ii = source.length; i < ii; i++) {
        destination.push(copyElement(source[i]));
      }
    } else if (isBlankObject(source)) {
      // createMap() fast path --- Safe to avoid hasOwnProperty check because prototype chain is empty
      for (key in source) {
        destination[key] = copyElement(source[key]);
      }
    } else if (source && typeof source.hasOwnProperty === 'function') {
      // Slow path, which must rely on hasOwnProperty
      for (key in source) {
        if (source.hasOwnProperty(key)) {
          destination[key] = copyElement(source[key]);
        }
      }
    } else {
      // Slowest path --- hasOwnProperty can't be called as a method
      for (key in source) {
        if (hasOwnProperty.call(source, key)) {
          destination[key] = copyElement(source[key]);
        }
      }
    }
    setHashKey(destination, h);
    return destination;
  }
/*
拷贝source的属性，并返回
*/
  function copyElement(source) {
    // Simple values
    if (!isObject(source)) {
      return source;
    }

    // Already copied values
    var index = stackSource.indexOf(source);
    if (index !== -1) {
      return stackDest[index];
    }

    if (isWindow(source) || isScope(source)) {
      throw ngMinErr('cpws',
        "Can't copy! Making copies of Window or Scope instances is not supported.");
    }

    var needsRecurse = false;
    var destination = copyType(source);
//创建数组或对象实例--destination
    if (destination === undefined) {
      destination = isArray(source) ? [] : Object.create(getPrototypeOf(source));
      needsRecurse = true;
    }

    stackSource.push(source);
    stackDest.push(destination);

    return needsRecurse
      ? copyRecurse(source, destination)
      : destination;
  }

  function copyType(source) {
    switch (toString.call(source)) {
      case '[object Int8Array]':
      case '[object Int16Array]':
      case '[object Int32Array]':
      case '[object Float32Array]':
      case '[object Float64Array]':
      case '[object Uint8Array]':
      case '[object Uint8ClampedArray]':
      case '[object Uint16Array]':
      case '[object Uint32Array]':
        return new source.constructor(copyElement(source.buffer), source.byteOffset, source.length);

      case '[object ArrayBuffer]':
        //Support: IE10
        if (!source.slice) {
          var copied = new ArrayBuffer(source.byteLength);
          new Uint8Array(copied).set(new Uint8Array(source));
          return copied;
        }
        return source.slice(0);

      case '[object Boolean]':
      case '[object Number]':
      case '[object String]':
      case '[object Date]':
        return new source.constructor(source.valueOf());

      case '[object RegExp]':
        var re = new RegExp(source.source, source.toString().match(/[^\/]*$/)[0]);
        re.lastIndex = source.lastIndex;
        return re;

      case '[object Blob]':
        return new source.constructor([source], {type: source.type});
    }

    if (isFunction(source.cloneNode)) {
      return source.cloneNode(true);
    }
  }
}
```
源码中，copy()方法接收两个参数，第一个是源source，是一个对象或者数组，第二个参数destination可选，作为存储源source的对象，该方法返回一个和源source一样的对象，且该对象是新建的实例，使用时与源source不会相互影响。
