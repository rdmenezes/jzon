# To file #

## Code ##

**main.cpp**
```
#include <Jzon.h>

int main()
{
	Jzon::Object root;
	root.Add("name", "value");
	root.Add("number", 20);
	root.Add("anothernumber", 15.3);
	root.Add("stuff", true);

	Jzon::Array listOfStuff;
	listOfStuff.Add("json");
	listOfStuff.Add("more stuff");
	listOfStuff.Add(Jzon::null);
	listOfStuff.Add(false);
	root.Add("listOfStuff", listOfStuff);

	Jzon::FileWriter::WriteFile("file.json", root, Jzon::StandardFormat);

	return 0;
}
```

## Result ##

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

# To string #

## Code ##

**main.cpp**
```
#include <Jzon.h>

int main()
{
	Jzon::Object root;
	root.Add("name", "value");
	root.Add("number", 20);
	root.Add("anothernumber", 15.3);
	root.Add("stuff", true);

	Jzon::Array listOfStuff;
	listOfStuff.Add("json");
	listOfStuff.Add("more stuff");
	listOfStuff.Add(Jzon::null);
	listOfStuff.Add(false);
	root.Add("listOfStuff", listOfStuff);

	Jzon::Writer writer(root, Jzon::StandardFormat);
	writer.Write();
	std::string result = writer.GetResult();

	return 0;
}
```

# Formatting #

You can change the way Jzon formats the file by defining a **Jzon::Format** and setting the members it holds.

There are two pre-defined formats that you can use: **Jzon::StandardFormat** which formats the file as above and **Jzon::NoFormat** which doesn't use any formatting at all.

### Jzon::Format ###
  * bool **newline** - If true sub-nodes will be written on separate lines.
  * bool **spacing** - If true ":" and "," will be trailed by a space.
  * bool **useTabs** - If true the lines will be indented by tabs instead of spaces.
  * int **indentSize** - The number of spaces or tabs to indent with.
**Note:** to disable indentation, **indentSize** to 0.