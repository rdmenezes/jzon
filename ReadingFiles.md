# From file #

## Code ##

**file.json**
```
{
	"name": "value",
	"number": 20,
	"anothernumber": 15.3,
	"stuff": true,
	"listOfStuff": [
		"json",
		"more stuff",
		null,
		false
	]
}
```

**main.cpp**
```
#include "Jzon.h"

#include <iostream>

int main()
{
	Jzon::Object rootNode;
	Jzon::FileReader::ReadFile("file.json", rootNode);

	for (Jzon::Object::iterator it = rootNode.begin(); it != rootNode.end(); ++it)
	{
		std::string name = (*it).first;
		Jzon::Node &node = (*it).second;

		std::cout << name << " = ";
		if (node.IsValue())
		{
			switch (node.AsValue().GetValueType())
			{
			case Jzon::Value::VT_NULL   : std::cout << "null"; break;
			case Jzon::Value::VT_STRING : std::cout << node.ToString(); break;
			case Jzon::Value::VT_NUMBER : std::cout << node.ToFloat(); break;
			case Jzon::Value::VT_BOOL   : std::cout << (node.ToBool()?"true":"false"); break;
			}
		}
		else if (node.IsArray())
		{
			std::cout << "*Array*";
		}
		else if (node.IsObject())
		{
			std::cout << "*Object*";
		}
		std::cout << std::endl;
	}

	const Jzon::Array &stuff = rootNode.Get("listOfStuff").AsArray();
	for (Jzon::Array::const_iterator it = stuff.begin(); it != stuff.end(); ++it)
	{
		std::cout << (*it).ToString() << std::endl;
	}

	return 0;
}
```

## Result ##

```
 name = value
 number = 20
 anothernumber = 15.3
 stuff = true
 listOfStuff = *Array*
 json
 more stuff
 null
 false
```

# From string #

## Code ##

**main.cpp**
```
#include "Jzon.h"

#include <iostream>

int main()
{
	std::string jsonData = "{\"name\":\"John\",\"age\":20}";

	Jzon::Object rootNode;
	Jzon::Parser parser(rootNode, jsonData);
	if (!parser.Parse())
	{
		std::cout << "Error: " << parser.GetError() << std::endl;
	}
	else
	{
		std::cout << "Name: " << rootNode.Get("name").ToString() << std::endl;
		std::cout << "Age: " << rootNode.Get("age").ToInt() << std::endl;
	}

	return 0;
}
```

## Result ##

```
 Name: John
 Age: 20
```