# js-dtdrzt

[Edit on StackBlitz ⚡️](https://stackblitz.com/edit/js-dtdrzt)

### [Understand the Hazards of Using Imperative Code](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/functional-programming/understand-the-hazards-of-using-imperative-code)

## PROBLEM EXPLANATION
What you should notice is the fact that output is not as suggested in instructions, which should be the following array:
`['FB', 'Gitter', 'Reddit', 'Twitter', 'Medium', 'new tab', 'Docs', 'freeCodeCamp', 'new tab']`.

Instead, you will receive this array: `['FB', 'Gitter', 'Reddit', 'Twitter', 'Medium', 'new tab', 'Netflix', 'YouTube', 'GMail', 'Docs', 'freeCodeCamp', 'new tab']`.

Take a look at the last part of code to make a conclusion where is the issue.

### Part 1
```js
const finalTabs = socialWindow
```
After this part of the code is executed, our array is `['FB', 'Gitter', 'Reddit', 'Twitter', 'Medium']`.

### Part 2
```js
.tabOpen()    // Open a new tab for cat memes
```

### Part 3
After adding a 'new tab' to the array, our array is now `['FB', 'Gitter', 'Reddit', 'Twitter', 'Medium', 'new tab']`.
```js
.join(videoWindow.tabClose(2))      // Close third tab in video window, and join
```
This part of the code should supposedly close the third tab (index 2 as the count starts from 0) and return video window without the third tab which is `Vimeo` in this case. So, returned array should look like `['Netflix', 'YouTube', 'Vine']` and after addit it to the main array, our array should be `['FB', 'Gitter', 'Reddit', 'Twitter', 'Medium', 'new tab', 'Netflix', 'YouTube', 'Vine']`.

### Part 4
```js
.join(workWindow.tabClose(1).tabopen());
```
This part would close second tab (index 1) in the workWindow `['GMail', 'Inbox', 'Work mail', 'Docs', 'freeCodeCamp']`, which would be `'Inbox'`, and after that push a 'new tab' to the array, return `['GMail', 'Work mail', 'Docs', 'freeCodeCamp', 'new tab']` and adding it to the main array.

If we compare the requested array and the one we received after running the initial code, we can see that values `'Vine'` and `'Work mail'` are omitted.  Therefore, we need to see what is the cause of this.

Now that we know this, we will check the operations done on that array. This is done in Part 3:
```js
.join(videoWindow.tabClose(2))    // Close third tab in videow window, and join
```
We can see that `tabClose()` is performed on this array, so we will analyze the code contained in that method.
```js
tabClose = function (index) {
  const tabsBeforeIndex = this.tabs.splice(0, index);   // (1) get the tabs before the tab
  const tabsAfterIndex = this.tabs.splice(index);     // (2) get the tabs after the tab 
  
  this.tabs = tabsBeforeIndex.concat(tabsAfterIndex);   // (3) join them together
  return this;    // (4)
};
```
For an index, we will take 2 - as doen in the challenge.
1. At the beginning, our array `videoWindow` looks like this: `['Netflix', 'YouTube', 'Vimeo', 'Vine']`.
2. After first line is executed, variable `tabsBeforeIndex` will take 2 (`index`) values starting from 0 (`splice(0,2)`) and create a new array containing them.  
Arrays now look like this: 
`tabsBeforeIndex`: `['Netflix', 'YouTube']`
`videoWindow`: `['Vimeo', 'Vine']`
You can already see why `splice()` can be very hazardous, as it always modifies the array it is executed upon and returns that modified array - it is not deterministic.
3. After second line is executed, `tabsAfterIndex` should take values starting from index (in this case 2) and remove them from `videoWindow` array.  As we can see that in the current state (`['Vimeo', 'Vine']`), `videoWindow` does not have index 2 so empty array is returned. Final state: 
`tabsBeforeIndex`: `['Netflix', 'YouTube']`
`videoWindow`: `['Vimeo', 'Vine']`
`tabsAfterIndexIndex`; `[]`
After the third line and concatenation the returned array is the same as `tabsBeforeIndex`, which results in both `'Vimeo'` and `'Vine'` values not being in the array.

