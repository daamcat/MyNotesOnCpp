## Algorithms
### Linked list
Is a list of nodes in which each node keeps two major components:
1. Data.
2. Pointer of its next node. `nullptr` if there is no next node.

* Node "head" points to the first node.
* The pointer to the next node in the last node is set to `nullptr`.

Example:
```c++
struct Node
{
  int m_data;
  Node* m_next;
}
```
### Binary Search Tree (BST)
Is a binary tree of nodes. Each node has three major components:
1. Data.
2. Pointer to the node on left. The value of the data in node on left is less than the value of the data in this node.
3. Pointer to the node on right. The value of the data in node on right is more than the value of the data in this node.

Example:
```
             50
            /   \
          30     70
        /   \   /  \
      20    40 60   80
```
Example of node declaration:
```c++
struct Node
{
  int m_data;
  Node* m_left;
  Node* m_right;
}
```
Searching for a node, based on its data, in such tree is easy: We start from the root, 50 in above example, and iteratively:
* If our value is smaller than the value of current node, take the node on left.
* If our value is greater than the value of current node, take the node on right.

till the target node is found.

### Bitmask flags
*Bitmasking* is a technique in which we use an integer to store multiple boolean `true`/`false` flags. A value in decimal numeral system can be represented in binary numeral system. An integer can be represented by a set of booleans. For example: (78)<sub>10</sub> = (01001110)<sub>2</sub>

In this technique, each flag occupies a certain bit in integer. This means flags values are defined using "power of two":

* `00000001` (Binary) = 1 (Decimal) → Bit 0
* `00000010` (Binary) = 2 (Decimal) → Bit 1
* `00000100` (Binary) = 4 (Decimal) → Bit 2
* `00001000` (Binary) = 8 (Decimal) → Bit 3

#### Bitwise left shift & bitwise right shift
These are two operators in C++:

* *Bitwise left shift* (`<<`): shifts all the bits to the left by 1 place.
* *Bitwise right shift* (`>>`): shifts all the bits to the right by 1 place.
Example:
```c++
#include <iostream>
#include <bitset>

int main()
{
  int a = 21;
  std::cout<< "int a =  " << a << ". Binary is: "<< std::bitset<8>(a) << std::endl;
  std::cout<< "a << 1 = " << (a << 1) << ". Binary is: "<< std::bitset<8>((a << 1)) << std::endl;
  std::cout<< "a >> 1 = " << (a >> 1) << ". Binary is: "<< std::bitset<8>((a >> 1)) << std::endl;

  int c = 1024;
  int d = c << 1;
  std::cout<< "c = " << c << ". Binary is: "<< std::bitset<16>(c) << std::endl;
  std::cout<< "d = " << d << ". Binary is: "<< std::bitset<16>(d) << std::endl;

  // Below we take a 1 and shift it some bits (zero to 3 bits) to left. Notice the power of two:
  std::cout<< "1 << 0 = " << (1 << 0) << ". Binary is: "<< std::bitset<8>(1 << 0) << std::endl;
  std::cout<< "1 << 1 = " << (1 << 1) << ". Binary is: "<< std::bitset<8>(1 << 1) << std::endl;
  std::cout<< "1 << 2 = " << (1 << 2) << ". Binary is: "<< std::bitset<8>(1 << 2) << std::endl;
  std::cout<< "1 << 3 = " << (1 << 3) << ". Binary is: "<< std::bitset<8>(1 << 3) << std::endl;

  return 0;
}
```

Prints (Notice the shifts to left and to right):
```
int a =  21. Binary is: 00010101
a << 1 = 42. Binary is: 00101010
a >> 1 = 10. Binary is: 00001010
c = 1024. Binary is: 0000010000000000
d = 2048. Binary is: 0000100000000000
1 << 0 = 1. Binary is: 00000001
1 << 1 = 2. Binary is: 00000010
1 << 2 = 4. Binary is: 00000100
1 << 3 = 8. Binary is: 00001000
```

#### Defining flags in `enum`
We group the boolean flags in an `enum` (notice that using `enum class` will need extra conversion and is not very practical):
```c++
enum Car : uint8_t
{
  none   = 0, // We set this normally to the case that no other enumerator is a match.
  opel   = 1 << 0, // 1 to the power of 0
  bmw    = 1 << 1, // 1 to the power of 1
  toyota = 1 << 2, // 1 to the power of 2
  mazda  = 1 << 3, // 1 to the power of 3
  ford   = 1 << 4  // 1 to the power of 4
};
```
#### Manipulating bitmasks
Manipulating bitmasks relies on four basic operators:
* *OR*: To set a bit to true `|` 
* *AND*: To check a bit `&`
* *NOT*: To set a bit to false `~`
* *XOR*: To toggle a bit (from true to false or from false to true) `^`

Example:
```c++
#include <iostream>
#include <bitset>

enum Car : uint8_t
{
  none   = 0, // We set this normally to the case that no other enumerator is a match.
  opel   = 1 << 0, // 1 to the power of 0
  bmw    = 1 << 1, // 1 to the power of 1
  toyota = 1 << 2, // 1 to the power of 2
  mazda  = 1 << 3, // 1 to the power of 3
  ford   = 1 << 4  // 1 to the power of 4
};

int main()
{
  std::cout << "The value of opel   = " << (int)Car::opel   << ", binary is: " << std::bitset<8>((int)Car::opel)   << std::endl;
  std::cout << "The value of bmw    = " << (int)Car::bmw    << ", binary is: " << std::bitset<8>((int)Car::bmw)    << std::endl;
  std::cout << "The value of toyota = " << (int)Car::toyota << ", binary is: " << std::bitset<8>((int)Car::toyota) << std::endl;
  std::cout << "The value of mazda  = " << (int)Car::mazda  << ", binary is: " << std::bitset<8>((int)Car::mazda)  << std::endl;
  std::cout << "The value of ford   = " << (int)Car::ford   << ",binary is: "  << std::bitset<8>((int)Car::ford)   << std::endl;

  // At initialization, there is no car in garage:
  uint8_t carsInGarage = Car::none;

  // Then three cars enter the garage (setting a bit to true):
  carsInGarage |= Car::bmw;
  carsInGarage |= Car::mazda;
  carsInGarage |= Car::ford;

  std::cout << "carsInGarage = " << (int)carsInGarage << ", binary is: " << std::bitset<8>(carsInGarage) << std::endl;

  // Mazda leaves the garage (setting a bit to false):
  carsInGarage &= ~Car::mazda;

  std::cout << "carsInGarage = " << (int)carsInGarage << ", binary is: " << std::bitset<8>(carsInGarage) << std::endl;
  
  // Check if there is a toyota in garage or not:
  if (carsInGarage & Car::toyota)
  {
    std::cout << "There is a Toyota in garage" << std::endl;
  }
  else
  {
    std::cout << "There is no Toyota in garage" << std::endl;
  }

  // Toggle Toyota (if one is in garage, take it out. If none in garage, bring one in):
  carsInGarage ^= Car::toyota;
  std::cout << "carsInGarage = " << (int)carsInGarage << ", binary is: " << std::bitset<8>(carsInGarage) << std::endl;

  // Check if there is a toyota in garage or not:
  if (carsInGarage & Car::toyota)
  {
    std::cout << "There is a Toyota in garage" << std::endl;
  }
  else
  {
    std::cout << "There is no Toyota in garage" << std::endl;
  }

  return 0;
}
```

Prints (notice how the value of each enumerator is a power of two, instead of the continuous set {0,1,2,3,...}):
```
The value of opel   = 1, binary is: 00000001
The value of bmw    = 2, binary is: 00000010
The value of toyota = 4, binary is: 00000100
The value of mazda  = 8, binary is: 00001000
The value of ford   = 16,binary is: 00010000
carsInGarage = 26, binary is: 00011010
carsInGarage = 18, binary is: 00010010
There is no Toyota in garage
carsInGarage = 22, binary is: 00010110
There is a Toyota in garage
```
