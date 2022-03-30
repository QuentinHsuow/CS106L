## Streams 2

#### cin and cout

`cin` Standard input stream

`cout` standard outpupt stream (buffered)

`cerr` Standard error stream (unbuffered)

`clog` standard error stream (buffered)



**3 reasons why >> with cin is a nightmare:**

1. cin reads the entire line into the buffer

2. Trash in the buffer will make cin not prompt the user for input at the right time

3. When cin fails, all future cin operations fail too



**read the entire line**

`getline(cin, name, '\n')` reads the entire line

getline does not skip a leading newline character

cin will skip all the intial white space, but will not skip the backslash

```cpp
getline(cin, name, '\n');
cin >> age;  // does not do error-checking
cout << name << age;
cin.ignore();  // ignores one character
getline(cin, response, '\n');
```

```cpp
int getInteger(const string& prompt, const string& reprompt){
  // if the user types in the integer, then returns integer
  // if the user doesn't type in the integer, then reprompt the user
  while(true){
		cout << prompt;
    string line;
    if (!getline(cin, line)) throw domain_error("...");
    
    istringstream iss(line);
    int val; char remain;
    
    if(!(iss >> val) && !(iss >> remain)) return val;
    
    cout << reprompt << endl;
  }
  
  return 0;
}
```



```cpp
// the version which google recommends
int getInteger() {  // note: this is buggy(?
  while(true){
    int result;
    if (cin >> result) return result;
    cin.clear(); // clears the fail flag, but will not flush the buffer
    // skips as many characters as possible, cin will automatically refresh the buffer
    cin.ignore(numeric_limits<streamsize>::max(), '\n'); 
  }
  return 0;
}
```





#### Types

**`auto` C++11 supports automatically tupe inference**

(sometimes STL type can be really long.)

* it discards any references:

  `auto& refMult = multiplier`

* Sometimes you don't know the type, and need to ask the compiler for it:

  `auto func = [](auto i) {return i*2};`



***when* auto should be used is a pretty contentious topic:**

* use it when the type is clear from the context
* use it when the exact type is unimportant
* Don't use it when it obviously hurts readability

`auto spliceString (const string& str)` is not recommended.



**C++17 allows structured bindings, allowing you to unpack the variables in a pair**

```cpp
pair<int, int> findPriceRange(int dist){
  int min = ...;
  int max = ...;
  return make_pair(min, max);
}

int main(){
  int dist = 1234;
  auto [min, max] = findPriceRange(dist);
  cout << min << '\n' << max << endl;
  return 0;
}
```

**you can also unpack a vector immediately**

*Drawback: how do you know the first thing is min and the second is max?*

```cpp
// the better way is to use a struct
struct PriceRange {
	int min;
  int max;
}

struct Course {
  string code;
  Time startTimel Time endTime;
  
  vector<string> instructors;
}
```



```cpp
PriceRange findPriceRange(int dist){
  int min = static_cast<int>(dist * 0.08);
  int max = static_cast<int>(dist * 0.36);
  return PriceRange{min, max};
}

int main(){
  int dist = 1234;
  PriceRange p = findPriceRange(dist);
  // auto [min, max]  = findPriceRange(dist);
}
```





**Parameters(in) and return value(out) guidelines for modern C++ code**

|                      | Cheap or impossible to copy(int, unique_ptr) | cheap/moderate/unknown cost to move(vector<>) | expensive to move(BIGPOD[]) |
| :------------------: | :------------------------------------------: | :-------------------------------------------: | :-------------------------: |
|       **Out**        |                   `X f()`                    |                    `X f()`                    |           `f(X&)`           |
|      **In/out**      |                   `f(X&)`                    |                    `f(X&)`                    |           `f(X&)`           |
|        **In**        |                    `f(X)`                    |                 `f(const X&)`                 |        `f(const X&)`        |
| **in & retain copy** |                    `f(X)`                    |         `f(const X&) + f(X&&) & move`         |        `f(const X&)`        |
|  **in & move from**  |                   `f(X&&)`                   |                   `f(X&&)`                    |          `f(X&&)`           |

