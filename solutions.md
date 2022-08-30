## SOLUTIONS EXPLAINED
### Alternate Solution 1

#### using `splice()`: 
This creates side effects (changes to the original array) and should be avoided in practice.

In order for the method `tabClose` to work properly,
```js
const tabsAfterIndex = this.tabs.splice(index + 1);
```
should be replaced with
```js
const tabsAfterIndex = this.tabs.splice(1);
```
This way, after second line is executed on the current array `['Vimeo', 'Vine']`, it will always omit the first value (index 0) and the one with index 1 until the end, resulting in the proper array returned.

### Alternate Solution 2
#### using `slice()`: 
This does not create side effects and should be preferred over `splice()`.

This part of the code:
```js
const tabsBeforeIndex = this.tabs.splice(0, index);     // get the tabs before the tab
const tabsAfterIndex = this.tabs.splice(index + 1);     // get the tabs after the tab
```
should be replaced with:
```js
const tabsBeforeIndex = this.tabs.slice(0, index);      // get the tabs before the tab
const tabsAfterIndex = this.tabs.slice(index + 1);      // get the tabs after the tab
```

### Word of caution
`splice()` should be always used carefully as it modiifes the contents it is working on. For documentation and differences between `splice` and `slice` please take a look at:
- [`Array.prototype.splice()` - JavaScript|MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)
- [`Array.prototype.slice()` - JavaScript|MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

