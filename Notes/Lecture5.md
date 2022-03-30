## Advanced Containers

Useful abstraction for "associating" a key with a value

iterators provide a guaranteed interface: we can use iterators in unordered_maps

This means that we can use the exact same code to perform a logical action, regardless of the data structure



#### Map iterators

**`std::pair Class`** is simply 2 objects bundled together

Quicker ways to make a pair `{"Phone number", 6507232300}` `make_pair`

```cpp
map<int, int> m;
auto i = m.begin();
auto end = m.end();
while(i != end) {
  cout << (*i).first << (*i).second << endl;
  ++i;
}
```



23:39