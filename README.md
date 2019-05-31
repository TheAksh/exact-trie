# exact-trie
Node.js implementation of a trie and a radix tree tailored towards exact matches.

# Methods

#### `new ExactTrie([options])`
Used to create the trie: `const trie = new ExactTrie();`.
* `options` (optional)
    * `ignoreCase` (`true`, optional) set to `true` to convert all keys to lower case.

#### `trie.put(key, value, [reverse])`
Used to insert elements into the trie.
* `key` search key, must be a string.
* `value` value to be inserted, can by any type.
* `reverse` (`false`, optional) set to `true` to reverse the key before inserting.

#### `trie.get(key, [reverse])`
Used to retrieve elements from the trie. Returns `undefined` if the key is not present in the trie.
* `key` search key, must be a string.
* `reverse` (`false`, optional) set to `true` to reverse the key before searching.

#### `trie.has(key, [reverse])`
Used to check if the key is present in the true. Returns a boolean.
* `key` search key, must be a string.
* `reverse` (`false`, optional) set to `true` to reverse the key before searching.

#### `trie.getWithCheckpoints(string, [checkpointChar], [reverse])`
Used to retrieve the value corresponding to they key that matches the longest prefix of `string`. If there are no
matches, `undefined` is returned.
* `string` string to check for prefixes, must be a string.
* `checkpointChar` (`null`, optional) character used to separate potential prefixes. When set to `null`, every possible
prefix is considered.
* `reverse` (`false`, optional) set to `true` to reverse the key before searching.

# Example

```js
const ExactTrie = require('exact-trie');

// Creating a trie instance
const trie = new ExactTrie({ignoreCase: true}); // `ignoreCase` is `true` by default

// Basic API and exact key matches
trie.put('life', 32);
console.log(trie.has('life')); // prints `true`
console.log(trie.get('life')); // prints `32`
console.log(trie.has('lif'));  // prints `false`
console.log(trie.get('lif'));  // prints `undefined`

// Various values and overwriting
const world = {response: 'world'};
trie.put('hello', world);
console.log(trie.get('hello')); // prints `{ response: 'world' }`
trie.put('hello', 3);
console.log(trie.get('hello')); // prints `3`

// Reversing keys
trie.put('oxygen', 8, true);
console.log(trie.has('oxygen'));               // prints `false`
console.log(trie.has('negyxo'));               // prints `true`
console.log(trie.has('oxygen', true)); // prints `true`

// Matching with checkpoints
trie.put('tim', 'Name is Tim');
trie.put('tim.kuzh', 'Tim Kuzh is the name');
console.log(trie.getWithCheckpoints('tim.kuzh', '.')); // prints `Tim Kuzh is the name`
console.log(trie.getWithCheckpoints('tim.cook', '.')); // prints `Name is Tim`

// Matching file extensions
trie.put('tar.gz', 'archive', true);
trie.put('gz', 'gzipped file', true);
console.log(trie.getWithCheckpoints('MyArchive.tar.gz', '.', true));
// -> prints `archive`
console.log(trie.getWithCheckpoints('DataSet.gz', '.', true));
// -> prints `gzipped file`
```
