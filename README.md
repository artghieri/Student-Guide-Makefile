## Introduction

I created this guide because Makefiles have always been a source of confusion and unresolved questions for me during my early years of programming. Even when I tried to learn from internet resources, nothing seemed to make sense. To put an end to this, I decided to create this guide, incorporating everything I've learned after weeks of study.

Within this guide, I've distilled all the essential knowledge and the most common topics about Makefiles. Each topic features a concise description followed by practical examples that you can execute on your own machine.

Let's tackle this challenging subject together and put an end to the confusion once and for all. Best of luck with your studies, and I hope this guide proves valuable in helping you navigate through it.

---

## A First Look To MakeFiles

**Makefiles** originated in the late 1970s as a solution for ***automating software compilation*** on ***Unix systems***. They were invented by ***Stuart Feldman*** at **Bell Labs**. These files enabled developers to define *rules* and *dependencies*, streamlining the build process and improving *software development efficiency*. Over time, **Makefiles** expanded beyond ***Unix*** and became a crucial tool in software development, ensuring reliable and automated builds. **Today, they remain a cornerstone of modern software engineering practices.**

### Why do MakeFiles Existed?

**Makefiles**, often referred to as *make* or *compilation files*, play a pivotal role in the world of software development. They are essential for automating and simplifying a process that would otherwise be complex and error-prone. 

When designing software programs, they often consist of various components, such as multiple source code files and external libraries. Manually compiling and building these components would be a daunting task prone to human errors. This is where **Makefiles** come into play, automating the compilation process, managing dependencies, and allowing programmers to focus on code development rather than dealing with tedious and error-prone tasks. 

```mermaid
flowchart LR
  A{{"Source Code (.c/.cpp)"}} --> B{{"Compilation & Linking"}}
  B --> C{{Executable File}}
```

### What alternatives are there to Make?

Popular **C/C++** alternative build systems are [SCons](https://scons.org), [CMake](https://cmake.org), [Bazel](https://bazel.build/?hl=pt-br), and [Ninja](https://ninja-build.org). Some code editors like [Microsoft Visual Studio](https://visualstudio.microsoft.com/pt-br/) have their own built in build tools. For Java, there's [Ant](https://ant.apache.org), [Maven](https://maven.apache.org/what-is-maven.html), and [Gradle](https://gradle.org). Other languages like [Go](https://go.dev), [Rust](https://www.rust-lang.org), and [TypeScript](https://www.typescriptlang.org) have their own build tools.

Interpreted languages like [Python](https://www.python.org), [Ruby](https://www.ruby-lang.org/pt/), and raw [Javascript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/What_is_JavaScript) don't require an analogue to **Makefiles**. The goal of **Makefiles** is to compile whatever files need to be compiled, based on what files have changed. But when files in *interpreted languages* change, nothing needs to get recompiled. When the program runs, the most recent version of the file is used.

#### Native Compilation x Cross Compilation

***Native Compilation*** is used to create applications for the same architecture as your development machine. For example, a ***point-of-sale program*** developed in [Java](https://dev.java/learn/getting-started/) on your **x86 machine**, which will run on another **x86 machine**.

In contrast, with ***Cross-Compilation***, you generate a *binary* for an architecture different from your development machine, as is the case with microcontrollers, embedded [Linux](https://www.linux.org), and mobile applications.

The most famous ***open-source tool*** for this purpose is the [GNU Compiler Collection (GCC)](https://gcc.gnu.org). It handles both native and cross-compilation.

#### GCC: How Does It Works

**GCC** is the key component of a [GNU toolchain](https://www.linux.org/threads/gnu-toolchain-explained.10570/), which, as the name suggests, is a *chain of tools* for development and debugging. In a nutshell, **GCC** will transform your source code into an object file and link these object files together to create a binary.

```mermaid
  flowchart LR
    A{{"Source File 1"}} --> |Compiler| B(["Object File 1"])
    C{{"Source File 2"}} --> |Compiler| D(["Object File 2"])

    B --> |Linker| E("Binary")
    D --> |Linker| E

```

> *Simplified process of binary generation. Here we omit the generation of assembly files.*


### The Versions and Types of MakeFiles

**Makefiles** come in various versions and types, each designed to cater to specific needs and development environments. The **two primary categories** are:

**GNU Make**  
This is the most common and widely used version of Make. It is the default on many ***Unix-like*** systems and is known for its *powerful* and *flexible* features. **GNU Make** allows you to define complex build rules, manage dependencies, and automate the build process efficiently.

**Non-GNU Makes**  
These are variations of Make that are not based on the **GNU Make** utility. Examples include ***BSD Make*** (used in *FreeBSD* and *macOS*) and ***Microsoft's NMake*** (used in *Visual Studio* environments). These versions often have their *own syntax* and *features*, which may differ from **GNU Make**.

The choice of Makefile version and type depends on the project's requirements, the development environment, and personal preferences. 

### MakeFile Syntax

A **Makefile** consists of a set of rules. A rule generally looks like this:

```makefile
targets: prerequisites
  command
  command
  command
```

- The *targets* are file names, separated by spaces. Typically, there is only one per rule.
- The *commands* are a series of steps typically used to make the target(s).
- The *prerequisites* are also file names, separated by spaces. These files need to exist before the commands for the target are run. These are also called *dependencies*.

> *Note: Makefiles **must** be indented using **TABs** and not spaces.*


### The Essence of Make

```makefile
hello:
  echo "Hello, World"
```

Let's start analyzing the *hello world* example. Here we have:

- One *target* called hello.
- This *target* has one *command*.
- This *target* has no *prerequisites*.

Let's provide a more detailed explanation of each part of the code:

```makefile
hello:
```
This line is called a *target definition*. In a **Makefile**, **targets** represent *tasks* or *actions* that you want to execute. In this case, we're defining a target named *hello*. When you run `make hello`, you're instructing **Make** to execute the commands associated with this **target**.

```makefile
echo "Hello, World"
```
This line is a command associated with the ***hello target***. It's the action that **Make** will perform when you ***run make hello***. The *echo command* is a standard command in *Unix-like operating systems* used to **print text** to the terminal. In this case, it's set to display the text `"Hello, World"`.

So, here's how the code works step by step:

- You create a **Makefile** with a target named ***hello***.
- When you run `make hello` in the terminal, **Make** recognizes the ***hello*** target and proceeds to execute the associated command.
- The associated command, `echo "Hello, World"`, is executed, resulting in `"Hello, World"` being printed to the terminal.

Make sure that you understand this. It's the **crux of Makefiles**, and might take you a few minutes to properly understand. 

#### A Brief Example of How MakeFiles Work

```makefile
blah: blah.o
    cc blah.o -o blah  # Runs third

blah.o: blah.c
    cc -c blah.c -o blah.o  # Runs second

blah.c:
    echo "int main() { return 0; }" > blah.c  # Runs first
```

When you run `make` in the terminal, it will build a program called **blah** in a series of steps:

```makefile
black.c:

# This line defines a target named blah.c. It's used to specify that the blah.c file should be generated.
```

```makefile
echo "int main() { return 0; }" > blah.c

# This line is the command associated with the blah.c target.
# When this is executed, it runs the echo command to create a C source file blah.c with a main function.
```

```makefile
blah.o: blah.c:

# This line defines a target named blah.o.
# It indicates that the blah.o object file depends on the blah.c source file.
```

```makefile
cc -c blah.c -o blah.o

# This line is the command associated with the blah.o target.
# It compiles the blah.c source file into an object file blah.o using the cc compiler.
```

```makefile
blah: blah.o:

# This line defines a target named blah, which depends on the blah.o target.
# It specifies the rule to build the final executable blah by linking the blah.o object file.
```

In summary, this is what happens when you run make:

- It checks if *blah.c* exists. If not, it creates *blah.c* with the simple C code.
- It then compiles *blah.c* into *blah.o*.
- Finally, it links *blah.o* to create the *blah* executable.

This is just a simple example of using a Makefile. Take your time to fully understand it before proceeding to future topics.

> *Note: cc It's a command that invokes the C compiler.*  
> *Note: -c This flag tells the compiler to compile the source code into an object file without linking it to create an executable.*


### Make Clean

Clean is often used to remove temporary and build-related files for other targets in a project directory. In the following example, `make clean` is used to remove the build-related files created by the *some_file* target.

```makefile
some_file: 
  (commands)

clean:
  rm -f some_file
```

Note that **clean** is doing two new things here:

- It's a *target* that is not first, and not a *prerequisite*. That means it'll never run unless you explicitly call `make clean`.
- If you happen to have a file named **clean**, this target **won't run**. 

This command ensures a clean and organized project directory, making it easier to maintain and debug your code.

> *Note: `rm -f some_file` is a Unix command that forcefully removes a file named "some_file" without asking for confirmation (-f flag ), even if the file is write-protected or doesn't exist.*

## Targets

In **Make**, *Targets* are defined tasks in a **Makefile** representing specific build objectives. These objectives can include compiling source code, linking binaries, or other project-related actions. **Make** uses these targets to automate the build process by managing dependencies and executing tasks as efficiently as possible.

### The All Target

The `all` target in a **Makefile** is a special target used to build multiple components or perform various tasks with a single `make` command. It enhances the efficiency of the build process by specifying a list of other targets to be built.

**Example:**  
Suppose you have a C project with multiple source files, and you want to compile them into separate object files and then link them into an executable. Your Makefile might include an `all` target like this:

```makefile
all: program
  
program: main.o utils.o
  # To specify the output executable filename, append the filename with the -o flag.
  gcc -o program main.o utils.o
  
main.o: main.c
  gcc -c main.c
  
utils.o: utils.c
  gcc -c utils.c
  
clean:
  rm -f program *.o
```

In this example, running make all would build the **program** target, which depends on the **main.o** and **utils.o** targets. This compiles the source files, creates object files, and links them into an executable, all with a single command.

> *Note: Since all is the first rule listed, it will run by default if make is called without specifying a target.*

### Multiple Targets

Multiple targets in a **Makefile** allow you to define and execute multiple build objectives or tasks within a single **Makefile**. They enable you to efficiently manage various components of a project's build process using a single `make` command.

```makefile
all: target1 target2

target1:
  # Build target1
target2:
  # Build target2
```

> *Note: In this example, running make all will execute both *target1* and *target2* in a single command.*

## Automatic Variables and Wildcards

**Automatic Variables** and **Wildcards** are powerful tools designed to simplify the build process and improve the readability of your rules.

### Automatic Variables
These handy placeholders, like `$@` and `$<`, allow you to refer to specific elements within rules. For example, `$@` represents the target, and `$<` represents the first prerequisite. Using these variables, you can create more flexible and understandable rules.

**Here's a table summarizing some commonly used automatic variables in Makefiles:**

| Automatic Variable   | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Represents&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Description&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
| :------------------: | ---------------------------------- | ---------------------------------------------------------------- |
| `$@`                 | Target                             | The target of the rule, typically the file being generated.      |
| `$<`                 | First prerequisite                 | The first prerequisite (dependency) of the target.               |
| `$^`                 | All prerequisites                  | All the prerequisites (dependencies) of the target.              |
| `$?`                 | Newer prerequisites                | Prerequisites that are newer than the target.                    |
| `$*`                 | Stem of the target and prerequisite| The stem shared between the target and prerequisite filenames.   |

These automatic variables simplify **Makefile** rules by allowing you to refer to specific elements of the rule without needing to explicitly name them. They enhance flexibility and maintainability in **Makefile** development.

> [!NOTE]
> *For more reference, check [Automatic Variables](https://www.gnu.org/software/make/manual/html_node/Automatic-Variables.html).*

### Wildcards
When you need to work with multiple files matching a pattern, wildcards come to the rescue. For instance, `*.c` matches all `.c` files in a directory. This simplifies tasks like compiling or processing multiple source files without the need to list them individually in your Makefile.

#### * Wildcard

The **\* Wildcard** is a powerful tool for matching multiple files based on a specified pattern. It simplifies the process of working with a group of files that share a common characteristic, such as a specific file extension. The asterisk (*) acts as a placeholder that can represent any characters or no characters at all, making it a versatile tool for tasks like compiling or processing multiple source files simultaneously.

I suggest that you always wrap it in the wildcard function, because otherwise you may fall into a common pitfall described below.

```makefile
# Print out file information about every .c file \
print: $(wildcard *.c)
  ls -la  $?
```

```makefile
wrong := *.o # Don't do this! '*' will not get expanded
right := $(wildcard *.o)

all: one two 

# Fails, because $(wrong) is the string "*.o"
one: $(wrong)

# Works as you would expect! 
two: $(right)

```

**Example:**  

```makefile
# Compile all .c files into executables
all: $(wildcard *.c)

%: %.c
  gcc $< -o $@

# Clean rule to remove executables
clean:
  rm -f a.out
```

In this example, the **\* Wildcard** matches all `.c` files in the directory and compiles them into executables with the same name as the source file.

> *Note: * may not be directly used in a variable definitions.*  
> *Note: * may be used in the target, prerequisites, or in the wildcard function.*

#### % Wildcard

In Makefiles, the **% Wildcard** is a versatile tool for pattern matching. It allows you to create generic rules that apply to multiple targets and prerequisites based on a pattern or template. This wildcard simplifies the process of defining rules for a group of files that share a common structure or naming convention.

**Example:**  
Consider a scenario where you have multiple source files with a common naming convention, such as file1.c, file2.c, and file3.c, and you want to compile each of them into separate executables. You can use the "%" wildcard to create a generic rule:

```makefile
# Compile all .c files into executables
all: file1 file2 file3

%: %.c
  gcc $< -o $@
```

In this example, the **% Wildcard** matches the common naming pattern of `.c` files and allows you to compile them into executables with the same name as the source file (e.g., file1.c becomes file1). This simplifies the **Makefile** by avoiding the need to list each file individually in the rules.


These features make your **Makefile** more powerful, maintainable, and efficient, particularly in projects with numerous files and dependencies.

## Fancy Rules

Fancy Rules in **Makefiles** are advanced, customized rules that extend beyond basic functionality. These rules empower developers to automate specialized tasks by specifying precise steps and commands tailored to project requirements. In essence, Fancy Rules are a means for developers to craft highly customized and efficient build systems.

### Implicit Rules

While **Makefiles** offer built-in implicit rules for common tasks like compiling source code and linking binaries, developers often need to customize these rules to suit their project's unique requirements. In this context, *Fancy Rules* or custom rules become crucial. Here's a few examples of implicit rules:

**Compile a C source file with dependencies listed in `$(INCLUDES)`:** 
```makefile
$(CC) -c $(CPPFLAGS) $(CFLAGS) $(INCLUDES) $^ -o $@
```

**Create a static library using `$(AR)` and custom archiver flags:**
```makefile
$(AR) $(ARFLAGS) $@ $^
```

#### Here's a reference table highlighting key Makefile variables for build configuration.

| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Variable**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;    | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Description**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
|:----------------|:-------------------------------------------------------------|
| **$(CC)**      | Specifies the C compiler to be used for compiling source code.                        |
| **$(CXX)**     | Specifies the C++ compiler to be used for compiling C++ source code.                      |
| **$(CPPFLAGS)**| Flags for the C/C++ preprocessor, such as include directories and macros.|
| **$(CFLAGS)**  | Flags for the C compiler, including optimization settings.         |
| **$(CXXFLAGS)**| Flags for the C++ compiler, including optimization settings.       |
| **$(LDFLAGS)** | Flags for the linker, such as library directories and linker options.           |
| **$(LDLIBS)**  | Libraries to link with during the linking process, e.g., '-lm' for the math library.     |
| **$(AR)**      | Specifies the archiver used for creating static libraries. |
| **$(ARFLAGS)** | Flags for the archiver, allowing customization of library creation.                                      |
| **$(RM)**      | Specifies the command used to remove files, enabling cleanup operations.                       |
| **$(MAKEFLAGS)**| Contains flags passed to Make during its execution, influencing its behavior.          |

Let's now explore building a C program with the help of implicit rules. In this process, we'll demonstrate how these rules streamline source code compilation and linking.

**Example: Implicit Rules and Variable Usage for C Project**  
 This example highlights how implicit rules and variables can save time, enhance code reliability, and streamline development workflows.

```makefile
# Variables
CC := gcc
CFLAGS := -Wall -O2
LDLIBS := -lm

# Target and dependencies
target: main.o math_functions.o

# Implicit rule to build object files from C source files
%.o: %.c
  $(CC) $(CFLAGS) -c $< -o $@

# Rule to build the final executable
target: main.o math_functions.o
  $(CC) $^ -o $@ $(LDLIBS)

# Clean rule
clean:
  rm -f *.o target
```

In this example:

- **CC** is set to the C compiler, `gcc`.
- **CFLAGS** contains compilation flags.
- **LDLIBS** contains libraries to link with (in this case, the math library `-lm`).

The provided example showcases a Makefile for automating the build process of a C project. It defines rules and variables to compile source code into object files, link them to create an executable, and clean up generated files. 

### Static Pattern Rules

Static Pattern Rules in **Makefiles** allow you to define rules for building multiple target patterns from corresponding prerequisite patterns in a concise manner. These rules provide a powerful way to manage dependencies and automate the build process for similar files with distinct names.

```makefile
targets...: target-pattern: prerequisite-patterns...
  recipe  
```

The essence is that the given ***target*** is matched by the ***target-pattern*** (via a `% wildcard`). Whatever was matched is called the stem. The stem is then substituted into the ***prereq-pattern***, to generate the target's prereqs.

Here's an illustration of Static Pattern Rules in action:

```makefile
# Build all .o files from .c files in the src/ directory
%.o: src/%.c
  $(CC) -c $(CFLAGS) $< -o $@
```

In this example, the static pattern rule compiles all `.c` files in the `src/` directory into corresponding `.o` files using the specified compiler and flags.

#### Static Pattern Rules and Filter

Additionally, the ***filter*** function in **Makefiles** allows for efficient filtering and manipulation of lists. The ***filter*** function can be used in Static Pattern Rules to match the correct files. 

```makefile
# Build all .o files from corresponding .c files using Static Pattern Rules
%.o: %.c
  $(CC) -c $(CFLAGS) $< -o $@

# Use the filter function to select specific source files
sources := file1.c file2.c file3.cpp
c_sources := $(filter %.c,$(sources))
```

> *Note: The Function Topic will be adressed later on this guide.*

In this example, Static Pattern Rules simplify compiling `.c` files to `.o` files, the filter function selects only `.c` source files from a list of sources.

### Pattern Rules

Pattern rules provide a mechanism for automating build processes by defining generic rules for transforming files. They are essential for managing dependencies and streamlining compilation tasks.

```makefile
targets...: target-pattern: prerequisite-patterns...
  recipe  
```

**Examples:**
```makefile
# Rule to compile all .c files into corresponding .o files
%.o: %.c
  $(CC) -c $(CFLAGS) $< -o $@
```

```makefile
# Define a pattern rule that has no pattern in the prerequisites.
# This just creates empty .c files when needed.
%.c:
  touch $@
```
> *Note: Pattern rules contain a '%' in the target.*

The `%` matches any nonempty string, and the other characters match themselves. 

### Double-Colon Rules

Double-Colon Rules are rarely used, but provide a unique method for specifying multiple rules for a single target.

```makefile
targets... :: prerequisites...
  recipe
```

**Examples:**
```makefile
all: blah

blah::
  echo "hello"
blah::
  echo "hello again"
```

```makefile
# Double-Colon Rule for 'all' target with multiple prerequisites
all:: hello_world.cpp main.cpp
  g++ $^ -o $@
```

> *Note: In this example, the Double-Colon Rule all:: specifies that the all target can be built from both hello_world.cpp and main.cpp, allowing for separate recipes for each prerequisite.*

## Commands and Execution

Commands, specified as recipes, define how targets are built by executing a series of shell commands. These commands are responsible for building targets and managing dependencies. 

#### Here's an example demonstrating how the *all* target would execute the following commands

```makefile
all: 
  cd ..
  # The cd above does not affect this line, because each command is effectively run in a new shell
  echo `pwd`
  
  # This cd command affects the next because they are on the same line
  cd ..;
  echo `pwd`
  
  # Same as above
  cd ..; \
  echo `pwd`
```

The **default shell** in **Makefiles** refers to the command interpreter or shell environment used to execute recipes. It plays a important role in how recipes are interpreted and commands are executed during the build process. 

The default shell is `/bin/sh`. You can change this by changing the variable SHELL:

```makefile
SHELL=/bin/bash

cool:
  echo "Hello from bash"
```

#### Command Echoing/Silencing

Command Echoing/Silencing involves controlling whether the shell commands within recipe lines are displayed or hidden during execution. This feature is essential for aiding in debugging, and improving the clarity of output messages. 

Simply add an `@` before a command to stop it from being printed.

```makefile
all: 
  @echo "This make line will not be printed"
  echo "But this will"
```
> *Note: You can also run make with -s to add an @ before each line.*

#### Double Dollar Sign

Double Dollar Signs `($$)` are used in **Makefiles** to escape a Single Dollar Sign `($)` when you want to include a literal dollar sign in a command or recipe without triggering variable expansion.

For example, if you want to *echo a dollar sign* in a recipe, you would write it as `$$` to prevent Make from interpreting it as the start of a variable reference. Here's an example:

```makefile
my_target:
  echo "This is a dollar sign: $$"
```

Note the differences between **Makefile Variables** and **Shell Variables** in this next example.

```makefile
# Make variable
make_var = I am a make variable
  
all:
  # Shell variable assignment and usage
  sh_var="I am a shell variable"; \
  echo $$sh_var
  
  # Make variable usage
  echo $(make_var)
```

#### Error Handling

In a **Makefile**, you can use several options to control error handling and the behavior of the make command. 

Here are three common options: `-k (Keep-Going)`, `-i (Ignore-Errors)`, and `- (Just-Print)`:

#### Keep-Going

```makefile
# Allows make to continue building other targets even if one target encounters an error.
# It reports errors at the end but doesn't halt the entire build process.

make -k
```

#### Ignore-Errors
```makefile
# Instructs make to ignore errors entirely and continue building all targets,
# regardless of individual failures.
make -i

# It's often used with `-k` to suppress error messages.
make -ki or make -k -i
```

#### Just-Print
```makefile
# Displays the commands that make would execute without actually running them.
# Useful for previewing the build process without making any changes to the system.

all: target

target:
  echo "Building target"
  -false
  echo "This will not be printed if - is used"
```

#### Interrupting or Killing Make
If you `ctrl+c` make, it will delete the newer targets it just made.

### Recursive Use of Make

Recursive Use of Make refers to the practice of *invoking the make command* within a **Makefile** to build targets in subdirectories, effectively creating a hierarchical or recursive build system. This approach is particularly useful when you have a complex project with multiple subcomponents or when you want to manage dependencies and builds for different parts of a larger project independently.

To implement recursive use of Make, you typically create a Makefile in each subdirectory, and the parent Makefile calls `make` in those subdirectories, passing along the appropriate targets and dependencies.

Suppose you have a project structure like this:
```css
project/
    Makefile (top-level Makefile)
    subdirectory1/
      Makefile
      source1.c
      source2.c
    subdirectory2/
      Makefile
      source3.c
      source4.c
```

In this scenario:

 - The top-level Makefile may contain rules and dependencies that apply to the entire project.

- Each subdirectory (`subdirectory1`, `subdirectory2`, etc.) has its own Makefile responsible for building targets within that directory.

- The top-level **Makefile** calls `make` in each subdirectory, passing down the appropriate targets and dependencies.

For instance, the top-level Makefile may include rules like:

```makefile
all:
  $(MAKE) -C subdirectory1
  $(MAKE) -C subdirectory2
```

This recursively invokes make in `subdirectory1` and `subdirectory2`, causing their respective **Makefiles** to be executed.

#### Export and Environment 

You can use the `export` directive to make variables available to child processes (e.g., sub-makes) or as environment variables for the commands executed in the recipes. This is useful when you need to pass variables to sub-makes or when you want to set environment variables for commands within a Makefile.

#### Exporting Variables for Sub-Makes:
To export variables so that they are available to sub-makes, you can use the export directive in your top-level Makefile:

```makefile
# Top-level Makefile

# Export variables to sub-makes
export VAR_NAME = value

all:
  $(MAKE) -C subdirectory
```

> *Note: In this example, `VAR_NAME` is exported, making it available to the sub-make executed in the `subdirectory`.*

```makefile
# Makefile

# Variables
VAR1 = Value1
VAR2 = Value2
VAR3 = Value3

# Export all variables to the environment for sub-makes
.EXPORT_ALL_VARIABLES:

all: sub-make

sub-make:
  @$(MAKE) -C subdir
```

> *Note: `.EXPORT_ALL_VARIABLES` exports all variables for you.*

#### Setting Environment Variables for Commands:

Environment variables in Makefiles enable the configuration of build environments, making them a versatile tool for customizing compilation settings, controlling build behavior, and passing information to sub-makes.

```makefile
# Makefile

# Setting an environment variable for the entire Makefile
export MESSAGE = Hello, Make!

all:
  # Using the environment variable in a command
  echo "$(MESSAGE)"
```

In this example:

- We export an environment variable called `MESSAGE` with the value `"Hello, Make!"` to make it available to all commands within the Makefile.  
- In the all target's recipe, we use `$(MESSAGE)` to access the value of the environment variable and print it using the `echo` command.

Running make in this directory will execute the all target, which prints `"Hello, Make!"` as the output, demonstrating the use of environment variables in Makefiles.

These techniques allow you to control the scope and availability of variables and environment settings within your Makefiles, which can be helpful for managing complex build processes.

#### Arguments to Make

When you run the make command, you can pass various [arguments and options](http://www.gnu.org/software/make/manual/make.html#Options-Summary)   to control the behavior of the build process. 

```makefile
make -f CustomMakefile -n main
```

> *Note: These arguments allow you to specify targets, override variables, and customize the build.*

## Variables

Variables are used to store values that can be referenced and manipulated throughout the Makefile. These variables make it easier to manage and customize your build or automation process by allowing you to define values once and reuse them in multiple places within your Makefile. 

In a Makefile, you can define variables using the following syntaxes:

#### Simply Expanded

```makefile
VARIABLE_NAME = value
```

> *Note: Using = assigns the value with simple expansion, which means the value is expanded when it is used.*

#### Immediate Assignment
```makefile
# Define variables
CC := gcc
CFLAGS := -Wall -O2
SRC_FILES := main.c utils.c
OBJ_FILES := $(SRC_FILES:.c=.o)
TARGET := my_program

# Default target
all: $(TARGET)

# Compile the target
$(TARGET): $(OBJ_FILES)
  $(CC) $(CFLAGS) -o $(TARGET) $(OBJ_FILES)

# Compile individual source files
%.o: %.c
  $(CC) $(CFLAGS) -c $< -o $@

# Clean up object files and the target executable
clean:
  rm -f $(OBJ_FILES) $(TARGET)
```

> *Note: Using := assigns the value with immediate expansion, which means the value is expanded at the point of assignment.*

You can think of this type of declaration as being used to define constant values (e.g., FLAGS, FILENAMES, etc) that remain unchanged throughout the execution of the Makefile.

#### Conditional Assignment 

```makefile
# Define a variable
MY_VARIABLE := existing_value

# Since MY_VARIABLE is already defined, the assignment has no effect.
MY_VARIABLE ?= new_value

# Usage example
all:
  echo "My Variable: $(MY_VARIABLE)"
```

> *Note: The ?= operator is used for conditional assignment, meaning it assigns a value to a variable only if the variable is not already defined.*

#### Appending Values

```makefile
# Appending values to a variable
CFLAGS := -Wall  # -Wall
CFLAGS += -O2    # -Wall -02

all:
  echo "Compiler Flags: $(CFLAGS)"
```

> *Note: Used to add values to an existing variable.*

#### Removing Values  

```makefile
# Defining a variable with multiple values
SRC_FILES := main.c utils.c helper.c

# Removing a value from the variable
SRC_FILES -= helper.c

all:
  echo "Source Files: $(SRC_FILES)"
```

> *Note: Used to remove values to an existing variable.*

### Command Line Arguments and Override

**Command Line** arguments enable customization of variables and targets during execution, while the **override** directive ensures that specific variable values defined in the Makefile take precedence over any other assignments.

#### Command Line

Makefiles can accept command line arguments that allow you to modify variables or specify targets when invoking make.

For example, suppose you have a Makefile with a variable `CC` that specifies the compiler:

```makefile
CC := gcc
```

You can override the value of `CC` by passing a command line argument:

```makefile
make CC = clang
```

#### Override Directive

The `override` directive is used in a Makefile to force the assignment of a variable, even if it has already been defined elsewhere. 

```makefile
# Define a variable
CC := gcc

# Use the override directive to force the assignment
override CC := clang
```

### Define

The [define directive](https://www.gnu.org/software/make/manual/html_node/Multi_002dLine.html) is used to create multi-line variable values or macros. It allows you to define complex text blocks that can be reused throughout the Makefile. It's commonly used for defining [canned recipes](https://www.gnu.org/software/make/manual/html_node/Canned-Recipes.html#Canned-Recipes) and pairs effectively with the [eval function](https://www.gnu.org/software/make/manual/html_node/Eval-Function.html#Eval-Function) to enhance Makefile configurations for advanced build processes.

```makefile
define VARIABLE_NAME
  # Multiline content goes here
endef
```

#### Example:
```makefile
define compile_rule
  $(CC) $(CFLAGS) -c $< -o $@
endef

CC := gcc
CFLAGS := -Wall -O2
SRC_FILES := main.c utils.c
OBJ_FILES := $(SRC_FILES:.c=.o)

all: my_program

my_program: $(OBJ_FILES)
  $(CC) $(CFLAGS) -o $@ $^

%.o: %.c
  $(compile_rule)

clean:
  rm -f $(OBJ_FILES) my_program
```

> *Note: In this example, the compile_rule is defined using define, and it contains the compilation command for C source files*

### Target-Specific Variables

Target-specific variables allow you to assign values to variables that are specific to particular targets or groups of targets. Target-specific variables are defined within the scope of a specific target rule and take precedence over global variable values when building that target.

```makefile
target: variable := value
```

```makefile
CC := gcc
CFLAGS := -Wall

all: program1 program2

program1: CC := clang
program1: source1.c
  $(CC) $(CFLAGS) -o $@ $^

program2: source2.c
  $(CC) $(CFLAGS) -o $@ $^
```

> *Note: In this example, we're assigning a different value to the CC variable for the program1 target, specifying that it should be compiled with the "clang" compiler, while program2 uses the global value of CC (e.g., "gcc").*

### Pattern-Specific Variables

Pattern-Specific Variables allow you to assign values to variables for a specific pattern of targets. These variables are defined using pattern rules (often involving wildcard characters like `%`) and are used to customize variable values based on a pattern match in the target or prerequisites.

```makefile
pattern%: variable := value
```

```makefile
%.o: CFLAGS := -O2

%.o: %.c
  $(CC) $(CFLAGS) -o $@ $<
```

> *Note: n this example, the % sign in the pattern rule %.o matches any target ending with .o. We set a pattern-specific value for CFLAGS to enable optimization (-O2) for object files. When a target like main.o or utils.o is built, it will use the specified CFLAGS value for optimization.*

## Conditional Statements

Conditionals Statements in **GNU Make** allow you to define build rules and behaviors based on conditions or criteria. 

Using directives such as `ifeq`, `ifneq`, `ifdef`, and `ifndef`, Makefiles can execute different commands or targets, adapting to specific build requirements. The basic syntax is as follows:

```makefile
ifeq (condition1, condition2)
  # Commands to execute if condition1 equals condition2
else
  # Commands to execute if condition1 does not equal condition2
endif
```

```makefile
ifneq (condition1, condition2)
  # Commands to execute if condition1 does not equal condition2
else
  # Commands to execute if condition1 equals condition2
endif
```

```makefile
ifdef variable
  # Commands to execute if 'variable' is defined (not empty)
else
  # Commands to execute if 'variable' is not defined (empty)
endif
```

```makefile
ifndef variable
  # Commands to execute if 'variable' is not defined (empty)
else
  # Commands to execute if 'variable' is defined (not empty)
endif
```

Here's an example of using conditionals in a Makefile to set different compilation flags based on a condition:

```makefile
# Makefile

# Compiler and flags
CC = gcc
CFLAGS = -Wall

# Target
TARGET = my_program

# Conditional: Release build or Debug build
BUILD_TYPE = Release

ifeq ($(BUILD_TYPE), Release)
  CFLAGS += -O2
else ifeq ($(BUILD_TYPE), Debug)
  CFLAGS += -g -O0
else
  $(error Invalid BUILD_TYPE: $(BUILD_TYPE))
endif

# Source files
SOURCE = main.c utils.c

# Build target
$(TARGET): $(SOURCE)
  $(CC) $(CFLAGS) -o $(TARGET) $(SOURCE)

clean:
  rm -f $(TARGET)
```

In this example:

- The `BUILD_TYPE` variable is set to `"Release"`.

- A conditional block checks the value of `BUILD_TYPE`. If it is `"Release"`, **optimization flags** are added to `CFLAGS;` if it is "`Debug"`, **debugging flags** are added; otherwise, an error is displayed.

- The `$(TARGET)` rule compiles the program using the specified `CFLAGS` based on the `BUILD_TYPE`.

### Check if A Variable is Empty

In **GNU Make**, checking whether a string is empty or not is a common operation that can be useful in various situations within your Makefile. An empty string is one that contains no characters or is considered as whitespace-only.

You can achieve this check using conditional statements such as `ifeq`, `ifneq`, `ifdef`, or `ifndef`, as shown in these examples:

#### Checking Empty String With ifeq
```makefile
# Check if a variable is empty
ifeq ($(variable),)
  # Variable is empty, execute commands
  @echo "Variable is empty"
else
  # Variable is not empty, execute other commands
  @echo "Variable is not empty: $(variable)"
endif
```

#### Checking Empty String With ifdef
```makefile
# Define a variable
my_variable := 

# Check if the variable is defined (not empty)
ifdef my_variable
  # Variable is defined and not empty, execute commands
  @echo "my_variable is defined and not empty: $(my_variable)"
else
  # Variable is not defined or empty, execute other commands
  @echo "my_variable is not defined or is empty"
endif
```

You can also combine the conditional statements with the `$(strip)` function or other string manipulation functions. 

```makefile
# Check if a variable is empty
ifeq ($(strip $(variable)),)
  # Variable is empty, execute commands
  @echo "Variable is empty"
else
  # Variable is not empty, execute other commands
  @echo "Variable is not empty: $(variable)"
endif
```

> *Note: The strip function is used to remove leading and trailing whitespace characters from the value of the variable.*

```makefile
# Define a variable with leading and trailing whitespace
my_variable :=     This is some text with spaces    

# Use the 'strip' function to remove leading and trailing whitespace
stripped_variable := $(strip $(my_variable))

Original Variable: '    This is some text with spaces    '
Stripped Variable: 'This is some text with spaces'
```

### $(MAKEFLAGS)

In **GNU Make**, `$(MAKEFLAGS)` is a variable that allows you to access the value of the **MAKEFLAGS** environment variable within your **Makefile**. The **MAKEFLAGS** environment variable contains a set of flags and options that were passed to the make command when it was invoked.

```makefile
MAKEFLAGS = -i

my_target:
ifeq (,-$(findstring -i,$(MAKEFLAGS)))
  @echo "Ignore errors (-i) flag is not enabled."
  # Add rules or commands for when the -i flag is not enabled
else
  @echo "Ignore errors (-i) flag is enabled."
  # Add rules or commands for when the -i flag is enabled
endif
```

> *Note: In this example, we check for the presence of the `-i` (ignore errors) flag in the `$(MAKEFLAGS)` variable within a Makefile.*

## Functions

Makefile functions, key for automation, streamline text manipulation and conditional operations. These powerful tools enhance efficiency in managing complex projects, offering essential flexibility in the build process.

#### Syntax

In Makefiles, functions are invoked using the `$()` notation, followed by the name of the function and its arguments, enclosed within parentheses. The basic syntax for invoking a function is as follows:

```makefile
$(function_name argument1 argument2 ...)
```

Here's a breakdown of the syntax components:

- **function_name:** The name of the function you want to use.
- **argument1, argument2, and so on:** Arguments or parameters passed to the function. The number and type of arguments depend on the specific function being used.

#### Practical Example

In the following example, we illustrate the effective use of custom functions alongside built-in ones, allowing for dynamic compiler flag assignment based on source file conditions. This technique enhances code compilation control within the build process.

```makefile
# List of source files
SRC_FILES := file1.c file2.c file3.c

# Function to generate object file names
obj_files = $(patsubst %.c, %.o, $(1))

# Generate object file names from source files
OBJ_FILES := $(call obj_files, $(SRC_FILES))

# Custom function to determine compiler flags for specific files
define get_compiler_flags
ifeq ($(findstring file1.c,$(1)),file1.c)
  FLAGS := -Wall -O2
else
  FLAGS := -Wall -O0
endif
endef

# Compiler
CC := gcc

# Target for the final executable
TARGET := my_program

all: $(TARGET)

$(TARGET): $(OBJ_FILES)
  $(CC) $(FLAGS) -o $@ $^

%.o: %.c
  $(call get_compiler_flags, $<)
  $(CC) $(FLAGS) -c -o $@ $<

clean:
  rm -f $(OBJ_FILES) $(TARGET)

# A custom function named get_compiler_flagsis defined using the define keyword.
# This function takes a file name as an argument and sets the FLAGS variable with 
# compiler flags based on whether the file name contains file1.c.
# If it does, it sets -Wall -O2 as flags; otherwise, it sets -Wall -O0 as flags.
```

#### Commonly Used Makefile Functions

| Function                 | Description                                                             |
| -------------------------| ----------------------------------------------------------------------- |
| **$(wildcard pattern)**  | Returns a list of file names matching the specified pattern.           |
| **$(patsubst pattern,replacement,text)** | Replaces occurrences of `pattern` with `replacement` in `text`.   |
| **$(foreach var,list,text)** | Evaluates `text` with `var` successively set to each item in `list`.   |
| **$(if condition,true-action[,false-action])** | Evaluates `condition` and executes `true-action` if true; otherwise, `false-action`. |
| **$(shell command)**     | Executes the shell command and returns its output as a string.        |
| **$(call function,args...)** | Calls a custom Makefile function with the specified arguments.         |
| **$(addprefix prefix,names...)** | Adds `prefix` to each word in `names`.                                |
| **$(strip string)**      | Removes leading and trailing whitespace from `string`.               |
| **$(filter pattern,names...)** | Returns a list of words that match the specified `pattern`.         |
| **$(firstword names...)** | Returns the first word from `names`.                                    |

> *Note: For more reference about functions, check [Functions for Transforming Text](https://www.gnu.org/software/make/manual/html_node/Functions.html)*

#### Error Handling in Functions

Error handling is a critical aspect of Makefiles, ensuring the reliability and robustness of the build process. It involves the identification, reporting, and management of issues that may arise during compilation and execution.

In Makefiles, error handling often involves using the $(error ...) function to generate an error message and halt the build process when a certain condition is met. The syntax is as follows:

```makefile
$(error error_message)
```

#### Example of Error Handling in Makefiles:

```makefile
# Check if a required dependency exists
ifeq (,$(wildcard dependency_file.txt))
  $(error Dependency 'dependency_file.txt' not found. Please install it.)
endif

# Compiler and compiler flags
CC := gcc
CFLAGS := -Wall -O2

# Target for the final executable
TARGET := my_program

all: $(TARGET)

$(TARGET): main.c
	$(CC) $(CFLAGS) -o $@ $^

clean:
	rm -f $(TARGET)
```

In this example:

- We use `ifeq` to check if the `dependency_file.txt` exists using the `$(wildcard)` function.
- If the file is not found, we use `$(error ...)` to display an error message.
- The build process stops at this point, and the specified error message is printed.
- If the file exists, the build continues as usual.
- 
This demonstrates how error handling in Makefiles can be used to check for prerequisites or conditions and handle errors when they occur.

#### Performance Considerations in Makefile Functions

Optimizing the performance of Makefile functions is crucial for efficient build systems. Making the right choices in how you design and use functions can significantly impact the speed and resource usage of your build process. 

Performance considerations within Makefile functions primarily involve optimizing the way functions are defined and called.

#### Example of Performance Considerations in Makefile Functions:

Let's consider an example where we need to apply a function repeatedly to a large list of files, which can be a performance bottleneck if not handled efficiently. In this case, we'll demonstrate how to use the `$(foreach)` function optimally to avoid unnecessary overhead:

```makefile
# List of source files
SRC_FILES := file1.c file2.c file3.c ... (a large list of files)

# Function to generate object file names
obj_files = $(patsubst %.c, %.o, $(1))

# Generate object file names from source files
OBJ_FILES := $(call obj_files, $(SRC_FILES))
```

In this example:

- Instead of using multiple `$(call obj_files, ...)` calls for each source file, we utilize the `$(foreach)` function to apply `$(call obj_files, ...)` to the entire list of `SRC_FILES`.
- This optimization avoids repeated function calls and improves the overall performance, especially when dealing with a large number of files.

Performance considerations like this can make a substantial difference in the efficiency of your Makefile functions, ultimately resulting in faster and more streamlined build processes.

### String Substitution

String substitution allows you to manipulate and transform text within your build rules and variable assignments. This versatile technique involves replacing specific substrings or characters with new values in text. String substitution is essential for tasks like renaming files, modifying compiler flags, or customizing build configurations.

Among these, the `$(patsubst)` function stands out as a powerful tool for replacing patterns with desired values in a given text. To explore other String Substitution Functions, refer to the [GNU documentation](https://www.gnu.org/software/make/manual/html_node/Text-Functions.html#Text-Functions).

#### Syntax

```makefile
$(patsubst pattern, replacement, text)
```

#### Example:

Here's an example of how to use the `$(patsubst)` function in a Makefile to replace file extensions:

```makefile
# List of source files with .c extensions
SRC_FILES := file1.c file2.c file3.c

# Use $(patsubst) to replace .c with .o for object files
OBJ_FILES := $(patsubst %.c, %.o, $(SRC_FILES))

# Print the list of object files
all:
  @echo "Source Files: $(SRC_FILES)"
  @echo "Object Files: $(OBJ_FILES)"

# We use the $(patsubst) function to replace the .c extension with .o
# to obtain the list of object files (OBJ_FILES).
```

When you run `make`, it will display the following output:

```makefile
Source Files: file1.c file2.c file3.c
Object Files: file1.o file2.o file3.o
```

### Iterating with $(foreach)

The `$(foreach)` function is a versatile tool that allows you to iterate over elements in a list and perform operations on each element. It is particularly useful for automating repetitive tasks within the build process. 

```makefile
$(foreach var, list, text)
```

- **var:** The variable that takes on each value from the list in each iteration.
- **list:** The list of values that var iterates over.
- **text:** The operations or text to be performed or generated for each var.

#### Example:

Here's an example of how to use the `$(foreach)` function in a Makefile to create a list of targets:

```makefile
# List of source files
SRC_FILES := file1.c file2.c file3.c

# Use $(foreach) to generate a list of targets
TARGETS := $(foreach src, $(SRC_FILES), $(basename $(src)))

# Print the list of targets
all:
  @echo "Source Files: $(SRC_FILES)"
  @echo "Generated Targets: $(TARGETS)"

# We use the $(foreach) function to iterate over each source file (src) in SRC_FILES.
# Inside the iteration, we use $(basename) to remove the file extension from each source file 
# and generate a list of target names (TARGETS).
```

When you run make, it will display the following output:

```makefile
Source Files: file1.c file2.c file3.c
Generated Targets: file1 file2 file3
```

### Conditional Logic with $(if)

The `$(if)` function provides conditional evaluation, allowing you to make decisions and perform actions based on whether a condition is true or false. 

```makefile
$(if condition, true-action, false-action)
```
- **condition:** The condition to evaluate.
- **true-action:** The action or value to be returned if condition is true.
- **false-action:** (Optional) The action or value to be returned if condition is false.

#### Example:

Here's an example of how to use the $(if) function in a Makefile to conditionally set a variable:

```makefile
# Condition: Is optimization enabled?
OPTIMIZE := yes

# Use $(if) to conditionally set the compiler flags variable
CFLAGS := $(if $(filter $(OPTIMIZE), yes), -O2 -Wall, -O0 -Wall)

# Print the selected compiler flags
all:
  @echo "Optimization: $(OPTIMIZE)"
  @echo "Compiler Flags: $(CFLAGS)"

# We use the $(if) function to check the condition $(filter $(OPTIMIZE), yes).
# If the condition is true (i.e., if OPTIMIZE is equal to "yes"), it sets CFLAGS to -O2 -Wall. 
```

When you run `make`, it will display the following output:

```makefile
Optimization: yes
Compiler Flags: -O2 -Wall
```

### Creating Custom Functions with $(call)

The `$(call)` function allows you to create and invoke custom functions within your build process. It offers the flexibility to define reusable operations and dynamically generate text or perform tasks based on variable arguments. 

```makefile
$(call function, args...)
```

#### Example:

Here's an example of how to use the `$(call)` function in a Makefile to create and invoke a custom function that calculates the sum of numbers in a list:

```makefile
# Custom function to calculate the sum of numbers in a list
define sum
  $(eval result := 0)
  $(foreach num,$1,$(eval result := $(result) + $(num)))
endef

# Define a list of numbers
NUMBERS := 1 2 3 4 5

# Invoke the custom function to calculate the sum of numbers
$(call sum,$(NUMBERS))

# Print the result
all:
  @echo "Numbers: $(NUMBERS)"
  @echo "Sum: $(result)"

# The $(eval) function is used to update the result variable with the sum.
# We invoke the custom function sum to calculate the sum of the numbers in the list.
```

When you run `make`, it will display the following output:

```makefile
Numbers: 1 2 3 4 5
Sum: 15
```

### Running Shell Commands with $(shell)

The `$(shell)` function enables you to execute shell commands from within your Makefile rules and capture their output as variables. This functionality allows you to incorporate external commands seamlessly into your build process, making it easier to obtain information or perform tasks that require interaction with the underlying system. 

```makefile
$(shell command)
```

#### Example:

Here's an example of how to use the `$(shell)` function in a Makefile to capture the output of a shell command:

```makefile
# Define a shell command to get the current date
DATE_COMMAND := date +%Y-%m-%d

# Use the $(shell) function to execute the command and store the output
CURRENT_DATE := $(shell $(DATE_COMMAND))

# Print the current date
all:
	@echo "Current Date: $(CURRENT_DATE)"

# We define a variable DATE_COMMAND that contains a shell command 
to retrieve the current date in the "YYYY-MM-DD" format.

# We use the $(shell) function to execute the $(DATE_COMMAND) command 
# and capture its output in the CURRENT_DATE variable.
```

When you run `make`, it will display the following output:

```makefile
Current Date: 2023-09-11
```

> *Note: The specific date will depend on when you run the make command.*

## Other

#### INCLUDE

The include directive tells make to read one or more other makefiles. It enables code modularity, simplifies maintenance, and facilitates the integration of external configurations, making complex projects more manageable.

```makefile
include filename
```

When you include a Makefile, its rules, variables, and targets become part of the current Makefile, allowing you to reuse code and settings across multiple parts of your project's build system.

#### VPATH

VPATH to specify where some set of prerequisites exist. It allows you to specify directories where Make should search for prerequisites when building targets. 

```makefile
VPATH := dir1:dir2:dir3

# dir1, dir2, dir3, etc.: Directories where Make should search for source files and prerequisites.
```

#### Example:

```makefile
# Define VPATH to specify source file directories
VPATH := src:lib

# Compile all .c files into object files
%.o: %.c
	gcc -c $(addprefix $(VPATH)/,$<) -o $@
	
# Build the final executable
my_program: main.o util.o
	gcc $^ -o $@
	
clean:
	rm -f *.o my_program

# We use VPATH to specify that Make should search for source files in the src and lib directories.
```

VPATH simplifies locating source files, ensuring that Make can find them regardless of their location in the specified directories.

#### MULTILINE

The backslash ( \\ ) character gives us the ability to use multiple lines when the commands are too long.

```makefile
my_target:
	echo "This line is too long, so \
	it is broken up into multiple lines"
```

#### .PHONY

.PHONY targets serve a special purpose: they don't represent files to be built, but rather actions or commands to be executed unconditionally. These targets are useful for defining tasks that don't result in the creation of output files, such as cleaning, testing, or documentation generation.

```makefile
.PHONY: target_name
```

#### Example:

```makefile
.PHONY: clean test doc

# Clean target: Remove object files and executables
clean:
	rm -f *.o my_program

# Test target: Run unit tests
test:
	./run_tests.sh

# Documentation target: Generate documentation
doc:
	doxygen Doxyfile
```

By marking these targets as `.PHONY`, you ensure that Make will always execute the associated actions, even if files with the same names as the targets exist in the directory. 

#### .DELETE_ON_ERROR

The `.DELETE_ON_ERROR` special target serves a specific purpose: it ensures that if any command in a recipe (a set of commands executed for a target) fails (returns a non-zero exit status), Make will delete the target file rather than leaving it in an undefined or partially updated state.

To use the `.DELETE_ON_ERROR` special target in a Makefile, you don't need to specify any additional syntax. Simply add the following line to your Makefile:

```makefile
.DELETE_ON_ERROR:
```

#### Example:

```makefile
# Ensure that the .DELETE_ON_ERROR target is active
.DELETE_ON_ERROR:

# Rule to compile a C source file into an object file
%.o: %.c
	gcc -c $< -o $@
	
# Rule to build the final executable
my_program: main.o util.o
	gcc $^ -o $@
	
clean:
	rm -f *.o my_program
	```

With `.DELETE_ON_ERROR` enabled, if the compilation of any `.c` file fails (e.g., due to a syntax error), Make will delete the corresponding `.o` file to ensure that the build remains in a consistent state. This can help prevent issues where a partially updated target could lead to unexpected behavior.

#### MAKEFILE COOKBOOK

Here's a Makefile with common tasks and techniques that you can use as a reference in your Makefile projects. 

```makefile
# Makefile Cookbook

# 1. Basic Variables
CC := gcc
CFLAGS := -Wall -O2

# 2. Targets and Dependencies
my_program: main.o util.o
	$(CC) $^ -o $@

main.o: main.c
	$(CC) $(CFLAGS) -c $< -o $@

util.o: util.c
	$(CC) $(CFLAGS) -c $< -o $@

# 3. Pattern Rules
%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

# 4. Phony Targets
.PHONY: clean test

clean:
	rm -f *.o my_program

test:
	./run_tests.sh

# 5. Conditional Build
ifdef DEBUG
CFLAGS += -g
endif

# 6. Automatic Variables
SRCS := main.c util.c
OBJS := $(SRCS:.c=.o)

# 7. Wildcard Function
SOURCE_FILES := $(wildcard *.c)

# 8. VPATH for Source Files
VPATH := src:lib

# 9. Command Echoing
.SILENT:

# 10. .DELETE_ON_ERROR
.DELETE_ON_ERROR:

# 11. User-Specified Variables
ifeq ($(OS),Windows_NT)
CC := x86_64-w64-mingw32-gcc
endif

# 12. Conditional Inclusion of Other Makefiles
ifdef USE_ADDITIONAL_MAKEFILE
include additional.mk
endif

# 13. Special Targets
.DEFAULT_GOAL := my_program

```

Now, Let's go through a Makefile example that works well for medium sized projects.

```makefile
# Thanks to Job Vranish (https://spin.atomicobject.com/2016/08/26/makefile-c-projects/)
TARGET_EXEC := final_program

BUILD_DIR := ./build
SRC_DIRS := ./src

# Find all the C and C++ files we want to compile
# Note the single quotes around the * expressions. 
# The shell will incorrectly expand these, but we want to send the * directly to the find command.
SRCS := $(shell find $(SRC_DIRS) -name '*.cpp' -or -name '*.c' -or -name '*.s')

# Prepends BUILD_DIR and appends .o to every src file
# As an example, ./your_dir/hello.cpp turns into ./build/./your_dir/hello.cpp.o
OBJS := $(SRCS:%=$(BUILD_DIR)/%.o)

# String substitution (suffix version without %).
# As an example, ./build/hello.cpp.o turns into ./build/hello.cpp.d
DEPS := $(OBJS:.o=.d)

# Every folder in ./src will need to be passed to GCC so that it can find header files
INC_DIRS := $(shell find $(SRC_DIRS) -type d)
# Add a prefix to INC_DIRS. So moduleA would become -ImoduleA. GCC understands this -I flag
INC_FLAGS := $(addprefix -I,$(INC_DIRS))

# The -MMD and -MP flags together generate Makefiles for us!
# These files will have .d instead of .o as the output.
CPPFLAGS := $(INC_FLAGS) -MMD -MP

# The final build step.
$(BUILD_DIR)/$(TARGET_EXEC): $(OBJS)
	$(CXX) $(OBJS) -o $@ $(LDFLAGS)

# Build step for C source
$(BUILD_DIR)/%.c.o: %.c
	mkdir -p $(dir $@)
	$(CC) $(CPPFLAGS) $(CFLAGS) -c $< -o $@

# Build step for C++ source
$(BUILD_DIR)/%.cpp.o: %.cpp
	mkdir -p $(dir $@)
	$(CXX) $(CPPFLAGS) $(CXXFLAGS) -c $< -o $@


.PHONY: clean
clean:
	rm -r $(BUILD_DIR)

# Include the .d makefiles. The - at the front suppresses the errors of missing
# Makefiles. Initially, all the .d files will be missing, and we don't want those
# errors to show up.
-include $(DEPS)
```

---
