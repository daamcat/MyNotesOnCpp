## Strings as arrays of chars
Ref: https://www.geeksforgeeks.org/c/strings-in-c/

C didn't provide built in data type for string. String would be represented as an array of chars ending in *null character* `'\0'`. `'\0'` would be added automatically to the end of array. This array of chars is called *null terminated*.

The following example shows how we can utilize an array of chars for strings:
```c++
#include <iostream>
#include <cstring>

int main()
{
  char str[] = "Hi"; // Sets the chars in an array and closes it with char '\0'
  std::cout << "sizeof(str): " << sizeof(str) << ", strlen(str): " << strlen(str) << std::endl;
  for(int i = 0; i < sizeof(str); i++)
  {
    std::cout << "str[" << i << "] = " << str[i] << std::endl;
  }
  
  char str2[8] = "Hi";
  std::cout << "sizeof(str2): " << sizeof(str2) << ", strlen(str2): " << strlen(str2) << std::endl;
  for(int i = 0; i < sizeof(str2); i++)
  {
    std::cout << "str2[" << i << "] = " << str2[i] << std::endl;
  }

  char str3[] = {'H','i','\0'};
  std::cout << "sizeof(str3): " << sizeof(str3) << ", strlen(str3): " << strlen(str3) << std::endl;
  for(int i = 0; i < sizeof(str3); i++)
  {
    std::cout << "str3[" << i << "] = " << str3[i] << std::endl;
  }

  // strcmp doesn't look at the size of arrays. It compares only the parts of arrays before '\0'.
  if (strcmp(str,str2) == 0)
  {
    std::cout<< "str and str2 are equal." << std::endl;
  }
  else
  {
    std::cout<< "str and str2 are not equal." << std::endl;
  }

  if (strcmp(str,str3) == 0)
  {
    std::cout<< "str and str3 are equal." << std::endl;
  }
  else
  {
    std::cout<< "str and str3 are not equal." << std::endl;
  }
  
  return 0;
}
```
Prints (notice the empty outputs are in fact `'\0'`):
```
sizeof(str): 3, strlen(str): 2
str[0] = H
str[1] = i
str[2] = 
sizeof(str2): 8, strlen(str2): 2
str2[0] = H
str2[1] = i
str2[2] = 
str2[3] = 
str2[4] = 
str2[5] = 
str2[6] = 
str2[7] = 
sizeof(str3): 3, strlen(str3): 2
str3[0] = H
str3[1] = i
str3[2] = 
str and str2 are equal.
str and str3 are equal.
```
### Strings and pointers
If we have a char pointer, `char* p`, with this pointer we can store an array of chars, an string, pointing to the first char of array.

```c++
#include <iostream>
#include <cstring>

int main()
{
  char str[] = "Hi"; // Sets the chars in an array and closes it with char '\0'
  std::cout << "sizeof(str): " << sizeof(str) << ", strlen(str): " << strlen(str) << std::endl;

  char* ptr = &str[0];
  std::cout << "ptr: " << ptr << std::endl;
  std::cout << "sizeof(ptr): " << sizeof(ptr) << ", strlen(ptr): " << strlen(ptr) << std::endl;

  return 0;
}
```
Prints:
```
sizeof(str): 3, strlen(str): 2
ptr: Hi
sizeof(ptr): 8, strlen(ptr): 2
```

#### String literal
*String literal* is a sequence of chars enclosed in double quotation mark, `"blah blah"`, and null-terminated (ending in `\0`).

We normally keep *string literal* in a `const` array of `char`s. Now! c++, facing an array, *decay*s (converts) the name of the array to the pointer to the first element of the array, enabling us to define a pointer to that *string literal*:

```c++
char str[] = "Blah";
char* ptr = str; // Because the name "str" decays to the pointer to the first element of str!
```
An example:
```c++
#include <iostream>
#include <cstring>

int main()
{
  {
    // For ints we had:
    int a = 5;
    int* p = &a;
    std::cout<< "P = " << p << ", *p = " << *p << ", a = " << ", &a = " << &a << std::endl;
  }

  // Below three are equal:
  {
    char str[] = "Hi"; // Sets the chars in an array and closes it with char '\0'
    std::cout << "sizeof(str): " << sizeof(str) << ", strlen(str): " << strlen(str) << std::endl;

    const char* ptr = &str[0]; // In C++, we usually keep the pointer to array of chars as const pointer.
    std::cout << "ptr: " << ptr << std::endl;
    std::cout << "sizeof(ptr): " << sizeof(ptr) << ", strlen(ptr): " << strlen(ptr) << std::endl;
  }

  {
    // String literal: The string "Hi" evaluates directly to memory address! The name, str, "decay"s (converts) into a pointer to its
    // first element.  
    char str[] = "Hi"; // String literal. Sets the chars in an array and closes it with char '\0'
    std::cout << "sizeof(str): " << sizeof(str) << ", strlen(str): " << strlen(str) << std::endl;

    const char* ptr = str; // Works! Exactly equal to const char* ptr = &str[0];. Because c++ decays the name str to address of its first element, i.e. to &str[0] !!!
    std::cout << "ptr: " << ptr << std::endl;
    std::cout << "sizeof(ptr): " << sizeof(ptr) << ", strlen(ptr): " << strlen(ptr) << std::endl;
  }

  {
    const char* ptr = "Hi"; // The compiler evaluates "Hi" to a memory address!!!
    std::cout << "ptr: " << ptr << std::endl;
    std::cout << "sizeof(ptr): " << sizeof(ptr) << ", strlen(ptr): " << strlen(ptr) << std::endl;
  }

  return 0;
}
```
Prints:
```
P = 0x7fff259adbbc, *p = 5, a = , &a = 0x7fff259adbbc
sizeof(str): 3, strlen(str): 2
ptr: Hi
sizeof(ptr): 8, strlen(ptr): 2
sizeof(str): 3, strlen(str): 2
ptr: Hi
sizeof(ptr): 8, strlen(ptr): 2
ptr: Hi
sizeof(ptr): 8, strlen(ptr): 2
```
### Conversion between array of chars and `std::string`
From array of chars to `std::string` is straight-forward. But the converse is a bit more tricky. Check the example:
```c++
#include <iostream>
#include <cstring>

int main()
{
  // ###############################
  // Array of chars to std::string:
  // ###############################
  const char ch[] = "Hello World";
  std::string str(ch);
  std::cout << "str(ch): " << str << std::endl;

  // ###############################
  // std::string to array of chars:
  // ###############################
  std::string str2 = "Ay Yu";

  // ##########
  // If we need a read-only object:
  // ##########
  const char* readOnly = str2.c_str();
  for (int i = 0; i < strlen(readOnly); i++)
  {
    // Notice that because we are using strlen(), the null character '\0' will not be printed; 
    std::cout << "readOnly[" << i << "] = " << readOnly[i] << std::endl;
  }

  // ##########
  // If we need to modify the object:
  // ##########
  // Important: std::string::size() or std::string::length() does not include the null character '\0'. Therefore we add 1 to size():
  char ReadAndWrite[str2.size() + 1];
  std::strncpy(ReadAndWrite, str2.c_str(), str2.size());
  ReadAndWrite[str2.size()+1] = '\0'; // Null-terminate it!

  ReadAndWrite[0] = 'S'; // Modify the first char.

  std::cout << "ReadAndWrite: " << ReadAndWrite << std::endl;

  // Notice how sizeof() includes null character '\0' and strlen() does not:
  for (int i = 0; i < sizeof(ReadAndWrite); i++)
  {
    // Notice that because we are using strlen(), the null character '\0' will not be printed; 
    std::cout << "ReadAndWrite[" << i << "] = " << ReadAndWrite[i] << std::endl;
  }

  for (int i = 0; i < strlen(ReadAndWrite); i++)
  {
    // Notice that because we are using strlen(), the null character '\0' will not be printed; 
    std::cout << "ReadAndWrite[" << i << "] = " << ReadAndWrite[i] << std::endl;
  }

  return 0;
}
```
Prints:
```
str(ch): Hello World
readOnly[0] = A
readOnly[1] = y
readOnly[2] =  
readOnly[3] = Y
readOnly[4] = u
ReadAndWrite: Sy Yu
ReadAndWrite[0] = S
ReadAndWrite[1] = y
ReadAndWrite[2] =  
ReadAndWrite[3] = Y
ReadAndWrite[4] = u
ReadAndWrite[5] = 
ReadAndWrite[0] = S
ReadAndWrite[1] = y
ReadAndWrite[2] =  
ReadAndWrite[3] = Y
ReadAndWrite[4] = u
```

