# JSON parser

This JSON parser for c++ is easy to use, and easy to learn. Below is an example of how the parser is used in a basic c++ program.

## Usage

Example JSON:

```json
{
	"string": "Hello world!",
	"object": {
		"floatInObject": 3.1415
	}
}
```

Example c++:

```cpp
#include "JSON.h"

int main() {
    JSON json("pathToJSON.json");
    std::string string = json.get<std::string>("string");
    float floatInObject = json.get<float>("object.floatInObject");
}
```

## How to use my JSON parser

First of all, include the header.<br/>
`#include <JSON.h>`<br/>
Next, create a json object like this: <br/>

```cpp
int main() {
    JSON json("pathToJSON.json");
}
```

Now you can work with json data in c++! The function used to extract data from json is the `json.get<type>("key");` function. In the complete example above, it demonstrates extracting a string and float value from json data. It does so by providing the json.get function with the appropiate type, (for number in json it is float in c++, and string in json is std::string in c++) and then a key. A key is what is used to specify which value you'd like in the json. In this example:<br/>

```json
{
	"person1": {
		"full-name": "John Doe",
		"age": 23,
		"friends": ["Mary", "Frank", "John"]
	},
	"person2": {
		"full-name": "Jane Doe",
		"age": 26,
		"friends": ["Joel", "Marie", "Dorothy"]
	}
}
```

To access person1's full name, the key you need to use is this: `"person1.full-name"`. ie: Every time you want to travel 1 object or array deeper, use a `'.'` then add the name of the next object or array index.
<br/>
<br/>
To access person2's first friend, the function would be: `json.get<std::string>("person2.friends.0")` . The 0 at the end of the key is the index inside of the person2's friends array. The value of that index in the array may be an object, and it is valid to tag on extra `.valueName` tags onto the end of that 0.
<br/>
<br/>
If your json starts off with an array (like the example below) then your key will have a slightly different format. Your key must start with the string: `"(ar)"` . This indicates that the json starts with an array. As a result of this, the next chacter in the key after the first `"(ar)"` must be an index inside the array. Here is an example:

```json
[
	{
		"object in root array": 25.235
	}
]
```

The function required to extract the value `"object in root array"` is as follows: `json.get<float>("(ar)0.object in root array")` .

## Writing to json

In order to modify JSON, the `JSON::write(key, value [, writeToFile]);` function is used<br>
Given the below JSON, (assuming a JSON object has been created under the name `json`) if the value at `user.username` needed to be changed to `John Doe`, then the following function call is used:

```c++
json.write("user.username", "John Doe");
```

If the value at `user.email` needed to be changed to `null` then the following function call is used:

```c++
json.write("user.email", NULL)
```

```json
{
	"Example": "JSON",
	"user": {
		"username": "Anyonymous",
		"email": "anon@example.com",
		"phone-number": 01189998819991197253
	}
}
```

C++ example:

```cpp
#include "JSON.h"

int main() {
    JSON json("file.json");
    std::cout << json.get<std::string>("user.username") << std::endl;
    json.write("user.username", "John Doe");
    std::cout << json.get<std::string>("user.username") << std::endl;
}
```

This code prints out the initial value of `user.username`, then modifies it and prints out the new value.

## Current Known Issues/Limitations

-   Cannot modify a whole json object or array
-   Cannot input number with an exponent
