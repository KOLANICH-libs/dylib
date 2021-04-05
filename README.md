# DyLib - Dynamic Library Loader for C++  
[![DyLib](https://img.shields.io/badge/DyLib-v0.4-blue.svg)](https://github.com/tocola/DyLib)
[![MIT license](https://img.shields.io/badge/License-MIT-orange.svg)](https://github.com/tocola/DyLib/blob/main/LICENSE)
[![CPP Version](https://img.shields.io/badge/C++-11/14/17/20-darkgreen.svg)](https://isocpp.org/)

[![GitHub watchers](https://img.shields.io/github/watchers/tocola/DyLib.svg?style=social&label=Watch&maxAge=2592000)](https://github.com/tocola/DyLib/watchers/)
[![GitHub forks](https://img.shields.io/github/forks/tocola/DyLib.svg?style=social&label=Fork&maxAge=2592000)](https://github.com/tocola/DyLib/network/members/)
[![GitHub stars](https://img.shields.io/github/stars/tocola/DyLib.svg?style=social&label=Star&maxAge=2592000)](https://github.com/tocola/DyLib/stargazers/)

[![GitHub all releases](https://img.shields.io/github/downloads/tocola/DyLib/total?style=for-the-badge)](https://github.com/tocola/DyLib/releases/download/v0.4/DyLib.hpp)

The goal of this Library is to load Dynamic Libraries (.so, .dll, .dylib) and access its functions and global variables at runtime.

## Compatibility
Works on `Linux`, `Windows`, `MacOS`

## Installation

1. Clone this repository
2. Go to the `DyLib` folder
3. Grab the `DyLib.hpp` file

## Usage

Lets write some functions in our lib
```c++
// myLib.cpp

#include <iostream>

extern "C" {
    double pi_value = 3.14159;
    void *ptr = nullptr;

    int adder(int a, int b)
    {
        return a + b;
    }

    void printHello()
    {
        std::cout << "Hello!" << std::endl;
    }
}
```

Lets build our lib :  

`g++ -std=c++11 -shared myLib.cpp -o myLib.so`

Lets try to access the functions of our dynamic lib at runtime with this code :
```c++
// main.cpp

#include <iostream>
#include "DyLib.hpp"

int main(int ac, char **av)
{
    try {
        DyLib lib("./myLib.so");

        auto adder = lib.getFunction<int(int, int)>("adder");
        std::cout << adder(5, 10) << std::endl;

        auto printer = lib.getFunction<void()>("printHello");
        printer();

        double pi_value = lib.getVariable<double>("pi_value");
        std::cout << pi_value << std::endl;

        void *ptr = lib.getVariable<void *>("ptr");
        if (ptr == nullptr)
            std::cout << "nullptr" << std::endl;
    }
    catch (const DyLib::exception &e) {
        std::cerr << e.what() << std::endl;
        return EXIT_FAILURE;
    }
    return EXIT_SUCCESS;
}
```

Lets build :  
`g++ -std=c++11 main.cpp -ldl`

Result :
```
15
Hello!
3.14159
nullptr
```
