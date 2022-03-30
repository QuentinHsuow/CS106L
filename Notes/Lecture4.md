## Associative Containers & Iterators

#### Associative Containers

Have no ideas of a sequence.

Data is accessed using the key instead of indexes.

**Associative Containers & Use Cases** 

* `std::map<T1, T2>` `std::set<T>` Sorted based on ordering property of keys. Keys need to be comparable, using < operator

  Map/set: keys in sorted order, faster to iterate through a range of elements

* `std::unordered_map<T1, T2>` `std::unordered_map<T>` based on hash function

  unordered_map/set: faster to aceess individual elements by key



**`std::map<T1, T2>`**

frequencyMap.count()

frequencyMap.containsKey()

Mmap.at(key) vs. mymap[key]



**`std::set<T>`**

works almost the same as map - at() or operator[]



`Multimap` same key point to different values

add elements by calling .insert on a key value std::pair

```cpp
multimap<int, int> myMMap;
myMMap.insert(make_pair(3, 3));
myMMap.insert({3, 12});  // shorter syntax
cout << myMMap.count(3) << endl;
```



#### Iterators

Assoc. container have no notion of a sequence of index

Iterators allow iteration over any container in a linear manner, whether it is ordered or not

`*iter` dereferencing to reach the value

`++iter` increment

A summary of the essential iterator operations:

create, Dereference, advance, Compare



