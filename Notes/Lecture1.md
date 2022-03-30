## Streams 1

#### ostringstream

```cpp
// stringstream does not interact with external I/O, but it can complete type conversion.
#include <sstream>  
#include <iostream>

using namespace std;

int main(){
  ostringstream oss("Ito En Green Tea");
  // ostringstream oss("Ito En Green Tea", ostringstream::ate), input will always be added to the end of the original content
  cout << oss.str() << endl;  
  
  oss << 16.9 << " Ounce ";  // automatically change double(16.9) into string
  cout << oss.str() << endl;  // will print "16.9 Ounce n Tea"
  
  oss << "(Pack of " << 12 << ")"
  cout << oss.str() << endl;  // necessary to use oss.str() rather than simply oss
  
  return 0;
}
```



#### istringstream

```cpp
#include <sstream>
#include <iostream>

using namespace std;

int main(){
  ostringstream oss("Ito En Green Tea ");
  
  oss << 16.9 << string(" Ounce ");
  
  oss << "(Pack of " << 12 << ")";
  
  
  istringstream iss(oss.str);  
  double amount;  //"16.9" can be interpreted as either double or string
  string unit;
  
  iss >> amount >> unit;
  
  cout << "Now each bottle is sold as: " << amount/2 << " " << unit;
  
  return 0;
}
```



```cpp
// reposition
#include <sstream>
#include <iostream>

using namespace std;

int main(){
  ostringstream oss("Ito En Green Tea ");
  
  oss << 16.9;
  fpos pos = oss.tellp() + streamoff(3); // index 4+3=7, cannot simply add 3
  oss.seekp(pos);
  oss << "Black";
  
  cout << oss.str() << endl;
  
	return 0;
}
```



#### stream key methods

* Constructors with initial text in the buffer.

  `istringstream iss("Initial");`, `ostringstream oss("Initial");`

  Can optionally provide "modes" such as ate (start at end) or bin (read as binary)

  `istringstream iss("Initial", stringstream::bin);`

  `ostringstream oss("Initial", stringstream::ate);`

* `oss << var1 << var2;` `iss >> var1 >> var2;`

  Insert or extract into the buffer. Convert type of var to and from the string type. Read about the get/put and read/write functions which provide unformatted input/output.

* get position `oss.tellp();` `oss.tellg();`

  set position `oss.seekp(pos);` `iss.seekg(pos);`

  create offset: `streamoff(n);`



#### State of the stream

```cpp
int stringToInteger(const string& str){
  istringstream iss(str);
  int vlaue;
  
  iss >> value;
  return value;
}

// First attemp: no error-checking
```



**Four bits indicate the state of the stream**

* Good bit: ready for write/read

* Fail bit: previous operation failed, all future operation frozen

* EOF bit: previous operation reached the end of the bufferr content, future operation fails

* Bad bit: external error, likely irrecoverable, future operation fails



**Common reasons why that bit is on**

* Nothing unusual, on when other bits are off

* Type mismatch, file can't be opened, seekg failed

* Reached the end of the file

* Could not move the characters to buffer from external source(eg. the file you are reading from suddenly is deleted)



*Fail and EOF are normally the ones you will be checking*



```cpp
// second attempt
int stringToInteger(string str){
  istringstream iss(str);
  
  int value;
  iss >> value;
  
  if(iss.fail() || !iss.eof()) throw domain_error(...);
  
  char remain;
  // if there is something remains in the buffer, then the input is not valid
  iss >> remain;  
  if(!iss.fail()) throw domain_error(...);
  
  return result;
}
```

`if(iss.fail())` equals `if(!(iss>>ch))`

```cpp
// third attemp: complete error-checking
int stringToInteger(const string& str){
  istringstream iss(str);
  
  int result; char remain;
  if(!(iss >> result) || iss >> ch)
    throw domain_error(...);
  
  return result;
}
```