title: Filters
type: api
order: 7
---

### capitalize

*'abc' => 'Abc'*

### uppercase

*'abc' => 'ABC'*

### lowercase

*'ABC' => 'abc'*

### currency

- this filter takes one optional argument

*12345 => $12,345.00*

You can pass an optional argument which will be used as the currency symbol (default is $).

### pluralize

- this filter takes at least one argument

Pluralizes the argument based on the filtered value. When there is exactly one arg, plural forms simply add an "s" at the end. When there are more than one argument, the arguments will be used as array of strings corresponding to the single, double, triple ... forms of the word to be pluralized. When the number to be pluralized exceeds the length of the args, it will use the last entry in the array.

**Example:**

``` html
{{count}} {{count | pluralize item}}
```

*1 => '1 item'*  
*2 => '2 items'*

``` html
{{date}}{{date | pluralize st nd rd th}}
```

Will result in:

*1 => '1st'*  
*2 => '2nd'*
*3 => '3rd'*
*4 => '4th'*
*5 => '5th'*

### json

- this filter takes one optional argument

JSON.stringify() incoming value rather than outputting the string representation (i.e. `[object Object]`). It also takes one optional argument which is the indent level (defaults to 2):

``` html
<pre>{{$data | json 4}}</pre>
```

### key

- this filter only works in `v-on`
- this filter takes exactly one argument

Wrap the handler so it only gets called when the keyCode matches the argument. You can also use string aliases for a few commonly-used keys:

- enter
- tab
- delete
- esc
- up
- down
- left
- right

**Example:**

``` html
<input v-on="keyup:doSomething | key enter">
```

`doSomething` will only be called when the Enter key is pressed.

### filterBy

**Syntax:** `filterBy searchKey [in dataKey]`.

- this filter only works in `v-repeat`
- this is a computed filter

Make `v-repeat` only display a filtered version of the source Array. The `searchKey` argument is a property key on the context ViewModel. The value of that property will be used as the string to search for:

``` html
<input v-model="searchText">
<ul>
  <li v-repeat="users | filterBy searchText">{{name}}</li>
</ul>
```

When the filter is applied, it will filter the `users` Array by recursively searching for the current value of `searchText` on each item in the Array. For example, if an item is `{ name: 'Jack', phone: '555-123-4567' }` and `searchText` has value `'555'`, the item will be considered a match.

Optionally, you can narrow down which specific property to search in with the optional `in dataKey` argument:

``` html
<input v-model="searchText">
<ul>
  <li v-repeat="users | filterBy searchText in name">{{name}}</li>
</ul>
```

Now the item will only match if the value of `searchText` is found in its `name` property. So `searchText` with value `'555'` will no longer match this item, but `'Jack'` will.

Finally, you can use quotes to indicate literal arguments:

``` html
<ul>
  <li v-repeat="users | filterBy '555' in 'phone'">{{name}}</li>
</ul>
```

### orderBy

**Syntax:** `orderBy sortKey [reverseKey]`.

- this filter only works in `v-repeat`
- this is a computed filter

Sort `v-repeat`'s displayed result. The `sortKey` argument is a property key on the context ViewModel. The value of that property will be used as the key to sort the Array items with. The optional `reverseKey` argument is also a property key on the context ViewModel, but the value's truthiness will determine whether the result should be reversed.

``` html
<ul>
  <li v-repeat="users | orderBy field reverse">{{name}}</li>
</ul>
```

``` js
new Vue({
  /* ... */
  data: {
    field: 'name',
    reverse: false
  }
})
```

You can also use quotes for literal sort key. To indicate a literal reverse, use `-1`:

``` html
<ul>
  <li v-repeat="users | orderBy 'name' -1">{{name}}</li>
</ul>
```
