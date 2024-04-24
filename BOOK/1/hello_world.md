# Hello, World

With Pythia installed, now it's time for you to write your first Pythia program. It’s traditional when learning a new language to write a little program that prints the text `Hello, world!` to the screen, so we’ll do the same here!

## Creating your project

Let's make a directory called `projects` to demonstrate. You can name yours whichever name you like.

```sh
root $ mkdir projects/
```

Now, in the `projects/` directory, let's make another directory, conveniently named `hello_world`.

```sh
root $ mkdir hello_world/
```

## Writing and running a Pythia program

Next, make a source file, named `main.pi`. Pythia files always end with the `.pi` extension. If you’re using more than one word in your filename, the convention is to use an underscore to separate them. For example, use `hello_world.pi` rather than `helloworld.pi`.

Now open the `main.pi` file you just created and enter the following code:

```py
def main()
    print("Hello, world!")
```

Save the file and return to your shell in the `projects/hello_world` directory. Enter the following commands to compile and run the file:

```sh
root $ pyc --execute main.pi
> Hello, world!
```

In the command, we passed in the `--execute` flag to run the program after it's success. Regardless of your operating system, the string `Hello, world!` should print to the terminal.

## Anatomy of a Pythia program

Let’s review this `“Hello, world!”` program in detail. We will go step by step.
The first part of the program is:

```py
def main()
```

These lines define a function named `main`. The `main` function is special: it is always the first code that runs in every executable Pythia program. Here, the first line declares a function named `main` that has no parameters and returns nothing. If there were parameters, they would go inside the parentheses `()`.

The body of a function is determined by it's indentation, similar to a Python program.

The body of the `main` function holds the following code:

```py
    print(“Hello, world!”)
```

This line does all the work in this little program: it prints text to the screen. There are some important things to notice here:
    - Pythia is indented by 4 spaces instead of a tab.
    - `print` calls a function.
    - You see the `"Hello, world!"` string. We pass this string as an argument to `print`, and the string is printed to the screen.
