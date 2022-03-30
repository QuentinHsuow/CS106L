## Sequence Container

#### Initialization

**Uniform Initialization**

Uniform: it works with many kinds of things

```cpp
int main(){
  vector<int> vec{3, 1, 4, 1, 5, 9};
  Course now {"CS106L", {15, 30}, {16, 30}, 
              {"Wang", "Zeng"}};
}
```

using the uniform initialization syntax, the initializer list ctor is preferred over constructor

```cpp
int main(){
  vector<int> vec1{3}; // vec1: {3}
  vector<int> vec2(3); // vec2: {0, 0, 0}
}
```



**Caution: Use thoughtfully **

When should I use a stringstream

* Processing strings: Simplify "/.a/b/.." to "/a"
* Formatting input/output: uppercase, hex, and other stream manipulators
* Parsing different types: stringToInteger

If you're just concatenating strings, str.append is faster than using a stringstream



#### Overview of STL

Algorithms - Iterators - Containeers - Functors/Lambdas - Adaptors

```cpp
// the magic of STL
int main(){
  vector<int> vec(kNumInts);
  std::generate(vec.begin(), vec.end(), rand);
  std::sort(vec.begin(), vec.end());
  std::copy(vec.begin(), vec.end(), std::ostream_iterator<int>(cout, "\n"));
  return 0;
}
// STL verson: the most efficient possible
```





#### Sequence Containers

**`std::vector<T>`**

Why doesn't `std::vector` bounds check by default

​	- Hint: Remember our discussion of the philosophy of C++

​	- If you write your program correctly, bounds checking will just slow your code down

Why is `push_front` slow?

​	- You need to shift all that is after



**`std::deque<T>`**

Deque can do anything that a vector can do and also: `push_front` and `pop_front`

Why use a vector at all?

​	- for other common operations, deque fails vector





#### Container Adaptors

**Stack**: just limit the functionality of a vector/deque to only allow `push_back` and `pop_back`

**Queue**: just limit the functionality of a deque to only allow `push_back` and `pop_front`

You can pass any container you like into it (the second parameter)



**Why not just use a vector/deque?**

Design philosophy of C++:

* Express ideas and intent directly in code
* Compartmentalize messy constructs