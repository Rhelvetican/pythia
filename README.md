# Pythia

- A Python like programming language with some fun features and readable syntax.

## Hello, World

- A simple hello world program in Pythia.

```python
print("Hello, World!")
```

## Defining stuffs

- In Pythia, type annotation are optional as types are inferred at runtime.

```python
num = 10                                            # Number

string = "This is a string."                        # String

anon = (x: int, y: int) x + y                       # Anonymous Function

fn add(x: int, y: int)                              # Function
    x + y                                           # Return statement is optional, the function will automatically return the last 
                                                    # statement evaluated.
```

## Importing libraries

- To import libraries, you can use the `import` keyword.
- To import multiple modules/functions/classes from a library, you can nest them in a pair of square brackets.

```python
import std.[json, io.input]                         # Import the JSON module from the Standard Library, and the input() function from the io 
                                                    # module.
path = input("Enter the path to your JSON file: ")
match json.from_file(path)                          # The function may fail to find the file, or ,in which case it will returns an Err, 
                                                    # otherwise it will return an Ok which contains the result of the function.
    case Ok(json)                           
        json = json
    case Err(err)
        print(err)
```

## Manifest file

- Pythia comes with it's own package manager, `Theodora`.
    - As such, all projects using it must contains a valid `proj.json`, `proj.toml` or `proj.json5`.

    - Minimal Examples:

    ```json
    {
        "package": {
            "name": "pythiac",
            "ver": "0.13.2"
        },
        "deps": {
            // Dependencies
        }
    }
    ```

    ```toml
    [package]
    name = "pythiac"
    ver = "0.13.2"
    [deps]
    # Dependencies
    ```

- For a list of possible values in the manifest, you can refer to `Theodora.md`.

## Features

- Zero-cost abstractions.
    - Zero-sized types.
- Rust-inspired pattern matching.
- Generics.
- Etc.
