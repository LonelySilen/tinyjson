
# tinyjson - a simple json parser for C++

## Features
- 轻量级
- Header only

## Example
read json:

```c++
#include <string>
#include <iostream>
#include <cassert>
#include "tinyjson.hpp"
using namespace std;
using namespace tiny;

string jsonstring = "\
{\
	\"name\":\"zhangsan\",\
	\"age\" : 26,\
	\"data\" : [\
	{\
		\"one\":\"chenone\",\
		\"two\" : {\
			\"love1\":\"2233\",\
			\"love2\":44444\
		}\
	}\
  ]\
}\
";

void TEST1() {
	TinyJson json;
	json.ReadJson(jsonstring);

	string name = json.Get<string>("name");
	int age = json.Get<int>("age");

	assert(name == "zhangsan");
	assert(age == 26);

	Values data = json.GetChild("data");
	for (int i = 0; i < data.GetCount(); i++) {
		data.Parse(i);
		string one = data.Get<string>("one");
		assert(one == "chenone");
		Values two = data.GetChild("two");
		for (int ii = 0; ii < two.GetCount(); ii++) {
			two.Parse(ii);
			string love1 = two.Get<string>("love1");
			int love2 = two.Get<int>("love2");
			assert(love1 == "2233");
			assert(love2 == 44444);
		}
	}
	cout << "TEST 1 PASS" << endl;
}

```

write json: 

```c++

void TEST3() {
	TinyJson wjson;
	wjson["name"].Set("liergou");
	wjson["age"].Set(26);
	wjson["handsome"].Set(true);

	TinyJson subjson;
	subjson["love1"].Set("book");
	subjson["love2"].Set(666);

	TinyJson subjson2;
	subjson2["love1"].Set("book2");
	subjson2["love2"].Set(6662);

	wjson["data"].Set(subjson);
	wjson["data2"].Set(subjson2);

	string str = wjson.WriteJson();
	cout << "json string: \r\n" << endl;
	cout << str << endl;
}

output :

/*

{
    "name": "liergou",
    "age": 26,
    "handsome": true,
    "data": {
        "love1": "book",
        "love2": 666
    },
    "data2": {
        "love1": "book2",
        "love2": 6662
    }
}

*/

```

## Usage

- 更多例子见 tinyjson_test.h 

