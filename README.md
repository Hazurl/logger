# module

A simple header for making the task of piping data between processes easier.

![demo](pic.png)

## Examples

#### 1. Processing streamed data

```cpp
#include <iostream>
#include "module.h"


int main(int argc, char const *argv[]) {
    // Stream input data and process it in chunks.
    
    ws::module::receive([&] (const std::string& buffer, int id, bool is_end) {
        // Pass the data along through the pipe.
        ws::module::piper << buffer;
    });


    return 0;
}
```

#### 2. Processing accumulated data

```cpp
#include <iostream>
#include "module.h"


int main(int argc, char const *argv[]) {
    // Accumulate all input data.
    auto contents = ws::module::receive_all();

    // Pipe the contents.
    ws::module::piper << contents;

    return 0;
}
```

#### 3. Logging & IO

```cpp
#include <iostream>
#include "module.h"


int main(int argc, char const *argv[]) {

    ws::module::print("Hello", " ", "there!", "\n");
    ws::module::println("Hello", " ", "there!");

    ws::module::pipe("Pipe ", 1, "\n");
    ws::module::pipeln("Pipe ", 2);

    ws::module::notice("Notice ", 1, "\n");
    ws::module::noticeln("Notice ", 2);

    ws::module::warn("Warn ", 1, "\n");
    ws::module::warnln("Warn ", 2);

    ws::module::error("Error ", 1, "\n");
    ws::module::errorln("Error ", 2);

    ws::module::success("Success ", 1, "\n");
    ws::module::successln("Success ", 2);

    ws::module::print("Yes!") << " " << "No!" << std::endl;

    ws::module::printer << "Printer\n";
    ws::module::piper << "Piper\n";

    return 0;
}
```

#### 4. Colours & Styles!

```cpp
#include <iostream>
#include "../module.h"


int main(int argc, char const *argv[]) {
    // Indentation.
    ws::module::println(ws::module::style::bold, "Indentation!");

    ws::module::print(ws::module::tabs(1));
    ws::module::noticeln(ws::module::spaces(0), "Testing...");

    ws::module::print(ws::module::tabs(1));
    ws::module::warnln(ws::module::spaces(1),   "Testing...");

    ws::module::print(ws::module::tabs(1));
    ws::module::errorln(ws::module::spaces(2),  "Testing...");

    ws::module::print(ws::module::tabs(1));
    ws::module::successln(ws::module::spaces(3),  "Testing...");




    // Styles.
    ws::module::println(ws::module::style::bold, "\nStyles!");

    ws::module::println(
        ws::module::tabs(1),
        ws::module::style::notice,
        "Hello!"
    );

    ws::module::println(
        ws::module::tabs(1),
        ws::module::style::warn,
        "Hello!"
    );

    ws::module::println(
        ws::module::tabs(1),
        ws::module::style::error,
        "Hello!"
    );

    ws::module::println(
        ws::module::tabs(1),
        ws::module::style::success,
        "Hello!"
    );

    ws::module::println(
        ws::module::tabs(1),
        ws::module::style::bold,
        "Hello!"
    );

    ws::module::println(
        ws::module::tabs(1),
        ws::module::style::reverse,
        "Hello!"
    );




    // Logging.
    ws::module::println(ws::module::style::bold, "\nLogging!");

    ws::module::print(ws::module::tabs(1));
    ws::module::noticeln("Notice");

    ws::module::print(ws::module::tabs(1));
    ws::module::warnln("Warn ");

    ws::module::print(ws::module::tabs(1));
    ws::module::errorln("Error ");

    ws::module::print(ws::module::tabs(1));
    ws::module::successln("Success ");




    // Colours.
    ws::module::println(ws::module::style::bold, "\nColours!");

    ws::module::println(
        ws::module::colour::fg::dim::black,   "black",   " ",
        ws::module::colour::fg::dim::grey,    "grey",    " ",
        ws::module::colour::fg::dim::red,     "red",     " ",
        ws::module::colour::fg::dim::yellow,  "yellow",  " ",
        ws::module::colour::fg::dim::green,   "green",   " ",
        ws::module::colour::fg::dim::blue,    "blue",    " ",
        ws::module::colour::fg::dim::magenta, "magenta", " ",
        ws::module::colour::fg::dim::cyan,    "cyan",    " "
    );

    ws::module::println(
        ws::module::colour::fg::black,   "black",   " ",
        ws::module::colour::fg::grey,    "grey",    " ",
        ws::module::colour::fg::red,     "red",     " ",
        ws::module::colour::fg::yellow,  "yellow",  " ",
        ws::module::colour::fg::green,   "green",   " ",
        ws::module::colour::fg::blue,    "blue",    " ",
        ws::module::colour::fg::magenta, "magenta", " ",
        ws::module::colour::fg::cyan,    "cyan",    " "
    );

    ws::module::println(
        ws::module::colour::fg::bright::black,   "black",   " ",
        ws::module::colour::fg::bright::grey,    "grey",    " ",
        ws::module::colour::fg::bright::red,     "red",     " ",
        ws::module::colour::fg::bright::yellow,  "yellow",  " ",
        ws::module::colour::fg::bright::green,   "green",   " ",
        ws::module::colour::fg::bright::blue,    "blue",    " ",
        ws::module::colour::fg::bright::magenta, "magenta", " ",
        ws::module::colour::fg::bright::cyan,    "cyan",    " "
    );




    // Lines, Tabs and Spaces.
    ws::module::println(ws::module::style::bold, "\nFormatting!");
    ws::module::println(ws::module::tabs(1), "Tabs!");
    ws::module::println(ws::module::spaces(1), "Spaces!");
    ws::module::println(ws::module::lines(1), "Lines!");
    ws::module::println(ws::module::repeat(">", 5), "Repeated string!");




    // Rainbows.
    ws::module::println(ws::module::style::bold, "\nRainbows!");
    ws::module::rainbowln("Hello ", "how ", "are ", "you?");

    return 0;
}
```

## Run

> Note: This works with `bash` but should also work with other shells that support the same functionality.

`echo "Hello World!" | ./reciever.out`

`cat <filename> | ./reciever.out`

`./reciever.out < <filename>`

`cat <filename> | ./reciever.out | ./reciever.out`

## Interface

> Note: anything defined within the "details" namespace is used internally and should not be used directly by the user.

#### Constants & Globals

| Styles  | ws::module::style::                             |
| ------- | ----------------------------------------------- |
| bold    | Makes the text bold.                            |
| reverse | Swaps the background and foreground colours.    |
| reset   | Reset the style so the text appears normal.     |
| notice  | Set the text to bold and use the notice colour. |
| warn    | Set the text to bold and use the warn colour.   |
| error   | Set the text to bold and use the error colour.  |

| Streams | ws::module::                       |
| :------ | ---------------------------------- |
| printer | Alias the std::cerr output steam.  |
| piper   | Alias the std::cout output stream. |

| Colours | ws::module::colour::                                     |
| ------- | -------------------------------------------------------- |
| black   | Looks like coal.                                         |
| red     | Same colour as the liquid being pumped around your body. |
| green   | That furry stuff that grows in your garden colour.       |
| yellow  | The Sun. (don't stare at it)                             |
| blue    | The Ocean or The Sky depending on shade.                 |
| magenta | Some flowers look like this.                             |
| cyan    | Also flowers.                                            |
| grey    | All fifty shades of it.                                  |

> All of the above colours exist in `fg::`, `bg::`, `fg::bright::`, `bg::bright::`, `fg::dim::` and `bg::dim::`.

| Special Colours | ws::module::colour::        |
| --------------- | --------------------------- |
| notice          | Colour to use for notices.  |
| warn            | Colour to use for warnings. |
| error           | Colour to use for errors.   |
| success         | Colour to use for success.  |

#### Functions.

| Data Handling | ws::module::                                                 |
| ------------- | ------------------------------------------------------------ |
| receive       | Recieve data on std::cin in chunks using a callback. |
| receive_all   | Accumulate all the data from std::cin and return a std::string. |

| Logging | ws::module::                                                                          |
| ------- | ------------------------------------------------------------------------------------- |
| print   | Prints a list of objects to ws::module::printer and appends ws::module::style::reset. |
| pipe    | Same as print but outputs to ws::module::piper.                                       |
| notice  | Same as print but prepends message with `[-]` and uses ws::module::style::notice.     |
| warn    | Same as print but prepends message with `[*]` and uses ws::module::style::warn.       |
| error   | Same as print but prepends message with `[!]` and uses ws::module::style::error.      |
| success | Same as print but prepends message with `[^]` and uses ws::module::style::success.    |
| rainbow | Same as print but prints each object with a random colour.   |

> All of the above functions also have newline variants like: `println` or `errorln` etc.

| Utility | ws::module::                                  |
| ------- | --------------------------------------------- |
| repeat  | Generate a string of N sequential strings.    |
| spaces  | Generate a string of N spaces.                |
| tabs    | Generate a string of N tabs.                  |
| lines   | Generate a string of N newlines.              |

