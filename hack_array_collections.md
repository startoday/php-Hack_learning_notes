1. Hack arrays are value types for storing iterable data. The types available are vec, dict and keyset. When in doubt, use Hack arrays.

```
//create hack arrays
$v = vec[2, 1, 2];

$k = keyset[2, 1];

$d = dict['a' => 1, 'b' => 3];

// The C namespace contains generic functions that are relevant to
// all array and collection types.
C\count(vec[]); // 0
C\is_empty(keyset[]); // true

// The Vec, Keyset and Dict namespaces group functions according
// to their return type.
Vec\keys(dict['x' => 1]); // vec['x']
Keyset\keys(dict['x' => 1]); // keyset['x']

Vec\map(keyset[1, 2], $x ==> $x + 1); // vec[2, 3]

```

2. Collections are object types for storing iterable data. The types available include Vector, Map, Set, Pair and helper interfaces.
3. PHP arrays are legacy value types for storing iterable data. The types available are varray, darray and varray_or_darray.

4. Hack arrays are value types. Any mutation produces a new value, and does not modify the original value. There are three Hack array types: vec, keyset and dict.
- A vec is an ordered, iterable data structure.
```
// Creating a vec.
function get_items(): vec<string> {
  $items = vec['a', 'b', 'c'];
  return $items;
}

// Accessing items by index.
$items[0]; // 'a'
$items[3]; // throws OutOfBoundsException

// Accessing items that might be out-of-bounds.
idx($items, 0); // 'a'
idx($items, 3); // null
idx($items, 3, 'default'); // 'default'

// Modifying items. These operations set $items
// to a modified copy, and do not modify the original value.
$items[0] = 'xx'; // vec['xx', 'b', 'c']
$items[] = 'd'; // vec['xx', 'b', 'c', 'd']

// Getting the length.
C\count($items); // 4

// Seeing if a vec contains a value or index.
C\contains('b'); // true
C\contains_key(2); // true

// Iterating.
foreach ($items as $item) {
  echo $item;
}
// Iterating with the index.
foreach ($items as $index => $item) {
  echo $index; // e.g. 0
  echo $item;  // e.g. 'a'
}

// Equality checks. Elements are recursively compared with ===.
vec[1] === vec[1]; // true
vec[1, 2] === vec[2, 1]; // false

// Combining vecs.
Vec\concat(vec[1], vec[2, 3]); // vec[1, 2, 3]

// Removing items at an index.
$items = vec['a', 'b', 'c'];
$n = 1;
Vec\concat(Vec\take($items, $n), Vec\drop($items, $n + 1)); // vec['a', 'c']

// Converting from an Iterable.
vec(keyset[10, 11]); // vec[10, 11]
vec(Vector { 20, 21 }); // vec[20, 21]
vec(dict['key1' => 'value1']); // vec['value1']

// Type checks.
$items is vec<_>; // true


```
- A keyset is an ordered data structure without duplicates. It can only contain string or int values.
```
// Creating a keyset.
function get_items(): keyset<string> {
  $items = keyset['a', 'b', 'c'];
  return $items;
}

// Checking if a keyset contains a value.
C\contains($items, 'a'); // true

// Adding/removing items. These operations set $items to a modified copy, 
// and do not modify the original value.
$items[] = 'd'; // keyset['a', 'b', 'c', 'd']
$items[] = 'a'; // keyset['a', 'b', 'c', 'd']
unset($items['b']); // keyset['a', 'c', 'd']

// Getting the length.
C\count($items); // 3

// Iterating.
foreach ($items as $item) {
  echo $item;
}

// Equality checks. === returns false if the order does not match.
keyset[1] === keyset[1]; // true
keyset[1, 2] === keyset[2, 1]; // false
Keyset\equal(keyset[1, 2], keyset[2, 1]); // true

// Combining keysets.
Keyset\union(keyset[1, 2], keyset[2, 3]) // keyset[1, 2, 3]

// Converting from an Iterable.
keyset(vec[1, 2, 1]); // keyset[1, 2]
keyset(Vector { 20, 21 }); // keyset[20, 21]
keyset(dict['key1' => 'value1']); // keyset['value1']

// Type checks.
$items is keyset<_>; // true
```
- A dict is an ordered key-value data structure. Keys must be strings or ints.
```
// Creating a dict.
function get_items(): dict<string, int> {
  $items = dict['a' => 1, 'b' => 3];
  return $items;
}

// Accessing items by key.
$items['a']; // 1
$items['foo']; // throws OutOfBoundsException

// Accessing keys that may be absent.
idx($items, 'a'); // 1
idx($items, 'z'); // null
idx($items, 'z', 'default'); // 'default'

// Inserting, updating or removing values in a dict. These operations 
// set $items to a modified copy, and do not modify the original value.
$items['a'] = 42; // dict['a' => 42, 'b' => 3]
$items['z'] = 100; // dict['a' => 42, 'b' => 3, 'z' => 100]
unset($items['b']); // dict['a' => 42, 'z' => 100]

// Getting the keys.
Vec\keys(dict['a' => 1, 'b' => 3]); // vec['a', 'b']

// Getting the values.
vec(dict['a' => 1, 'b' => 3]); // vec[1, 3]

// Getting the length.
C\count($items); // 2

// Checking if a dict contains a key or value.
C\contains_key($items, 'a'); // true
C\contains($items, 3); // true

// Iterating values.
foreach ($items as $value) {
  echo $value; // e.g. 1
}
// Iterating keys and values.
foreach ($items as $key => $Value) {
  echo $key;   // e.g. 'a'
  echo $value; // e.g. 1
}

// Equality checks. === returns false if the order does not match.
dict[] === dict[]; // true
dict[0 => 10, 1 => 11] === dict[1 => 11, 0 => 10]; // false
Dict\equal(dict[0 => 10, 1 => 11], dict[1 => 11, 0 => 10]); // true

// Combining dicts (last item wins).
Dict\merge(dict[10 => 1, 20 => 2], dict[30 => 3, 10 => 0]);
// dict[10 => 0, 20 => 2, 30 => 3];

// Converting from an Iterable.
dict(vec['a', 'b']); // dict[0 => 'a', 1 => 'b']
dict(Map {'a' => 5}); // dict['a' => 5]

// Type checks.
$items is dict<_, _>; // true
```
- If you want different keys to have different value types, or if you want a fixed set of keys, consider using a shape instead.
