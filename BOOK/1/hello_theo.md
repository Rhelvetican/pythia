# Theodora

Theodora, or Theo, affectionately, is Pythia’s build system and package manager. Most Pythians use this tool to manage their Pythia projects because Theodora handles a lot of tasks for you, such as building your code, downloading the libraries your code depends on, and building those libraries. (We call the libraries that your code needs dependencies.)

The simplest Pythia programs, like the one we’ve written so far, don’t have any dependencies. If we had built the `“Hello, world!”` project with Theodora, it would only use the part of Theodora that handles building your code. As you write more complex Pythia programs, you’ll add dependencies, and if you start a project using Theodora, adding dependencies will be much easier to do.

Because the vast majority of Pythia projects use Theodora, the rest of this book assumes that you’re using Theodora too. Theodora comes installed with Pythia if you used the official installers discussed in the `“Installation”` section. If you installed Pythia through some other means, check whether Theodora is installed by entering the following in your terminal:

```sh
root $ theo --version
```

If you see a version number, you have it! If you see an error, such as `command not found`, look at the documentation for your method of installation to determine how to install Theodora separately.

## Creating a Pythia project with Theodora

Let’s create a new project using Theodora and look at how it differs from our original `“Hello, world!”` project. Navigate back to your `projects` directory (or wherever you decided to store your code). Then, on any operating system, run the following:

```sh
root $ theo new hello_theo
cd hello_theo
```

The first command creates a new directory and project called `hello_theo`. We’ve named our project `hello_theo`, and Theodora creates it's files in a directory of the same name.

Go into the `hello_theo` directory and list the files. You’ll see that Theodora has generated two files and one directory for us: a `Theo.toml` file and a `src` directory with a `main.pi` file inside.

It also initialize a new Git repository by default, but you can specify which VCS you would like to use with the `--vcs=[your vcs here]` flag. Currently Theodora supports Git, Mercurial, Pijul or Fossil.

Open `Theo.toml` in your text editor of choice. It should be like this:

```toml
[package]
name = "hello_theo"
version = "0.1.0"

[requirements]
```

This file is in the [TOML](https://toml.io/) (Tom’s Obvious, Minimal Language) format, which is Theodora's configuration format.

The first line, `[package]`, is a section heading that indicates that the following statements are configuring a package. As we add more information to this file, we’ll add other sections.

The next three lines set the configuration information Theodora needs to compile your program: the name and the version.

The last line, `[requirements]`, is the start of a section for you to list any of your project’s requirements/dependencies. In Pythia, packages of code are referred to as boxes. We won’t need any other boxes for this project, but we will in the first project in Chapter 2, so we’ll use this requirements section then.

Now open `src/main.pi` and take a look:

```py
def main()
    print("Hello, world!")
```

Theodora has generated a `“Hello, world!”` program for you, just like the one we wrote earlier! So far, the differences between our project and the project Theodora generated are that Theodora placed the code in the `src` directory and we have a `Theo.toml` configuration file in the top directory.

Theodora expects your source files to live inside the `src` directory. The top-level project directory is just for README files, license information, configuration files, and anything else not related to your code. Using Theodora helps you organize your projects. There’s a place for everything, and everything is in its place.

If you started a project that doesn’t use Theodora, as we did with the `“Hello, world!”` project, you can convert it to a project that does use Theodora. Move the project code into the `src` directory and create an appropriate `Theo.toml` file.

## Building & Running Pythia with Theodora

Now let’s look at what’s different when we build and run the “Hello, world!” program with Theodora! From your hello_theo directory, build your project by entering the following command:

```sh
root $ theo build
    Compiling hello_theo v0.1.0
    Finished dev [unoptimized + debuginfo] target(s) in 2.85 secs
```

This command creates an executable file in `target/debug/hello_theo` rather than in your current directory. Because the default build is a debug build, Theodora puts the binary in a directory named debug. You can run the executable with this command:

```sh
root $ ./target/debug/hello_theo
> Hello, world!
```

If all goes well, `Hello, world!` should print to the terminal. Running `theo build` for the first time also causes Theodora to create a new file at the top level: `Theo.lock`. This file keeps track of the exact versions of dependencies in your project. This project doesn’t have dependencies, so the file is a bit sparse. You won’t ever need to change this file manually; Theodora manages its contents for you.

We just built a project with `theo build` and ran it with `./target/debug/hello_theo`, but we can also use `theo run` to compile the code and then run the resultant executable all in one command:

```sh
root $ theo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
    Running `target/debug/hello_theo`
Hello, world!
```

Using `theo run` is more convenient than having to remember to run `theo build` and then use the whole path to the binary, so most developers use `theo run`.

Notice that this time we didn’t see output indicating that Theodora was compiling `hello_theo`. Theodora figured out that the files hadn’t changed, so it didn’t rebuild but just ran the binary. If you had modified your source code, Theodora would have rebuilt the project before running it, and you would have seen this output:

```sh
root $ theo run
    Compiling hello_theo v0.1.0
    Finished dev [unoptimized + debuginfo] target(s) in 0.33 secs
    Running `target/debug/hello_theo`
Hello, world!
```

Theodora also provides a command called `theo check`. This command quickly checks your code to make sure it compiles but doesn’t produce an executable:

```sh
root $ theo check
   Checking hello_theo v0.1.0
    Finished dev [unoptimized + debuginfo] target(s) in 0.32 secs
```

Why would you not want an executable? Often, `theo check` is much faster than `theo build` because it skips the step of producing an executable. If you’re continually checking your work while writing the code, using `theo check` will speed up the process of letting you know if your project is still compiling! As such, many Pythians run `theo check` periodically as they write their program to make sure it compiles. Then they run `theo build` when they’re ready to use the executable.
