# Practice Go-Language and It's Concepts
For Learning Go language

Learning materials:

https://go.dev/learn/#tutorials

https://gobyexample.com/

https://go.dev/doc/effective_go


Installing Go:
https://go.dev/doc/install

Check if GO is installed on your machine:
```
go version
```
<details>
<summary>Modules in Go language</summary>
Go code is grouped into packages, and packages are grouped into modules. Your module specifies dependencies needed to run your code, including the Go version and the set of other modules it requires.
In a module, you collect one or more related packages for a discrete and useful set of functions. For example, you might create a module with packages that have functions for doing financial analysis so that others writing financial applications can use your work. For more about developing modules, see [Developing and publishing modules.](https://go.dev/doc/modules/developing)https://go.dev/doc/modules/developing)
</details>



<details>
<summary>First step before running any go code (even if it is Hello World) - Create a Module</summary>
1. Target code must belong to a module.
  To create a module you need to run the mod init command
  
  example:
    
  ```
  go mod init module_path
  ```
  _**module_path:**_

  If you publish a module, this must be a path from which your module can be downloaded by Go tools. That would be your code's repository.

  * The go mod init command creates a go.mod file to track your code's dependencies.
</details>

<details>
<summary>Package</summary>
A package is a way to group functions, and it's made up of all the files in the same directory. for example: fmt is a package, which contains functions for formatting text, including printing to the console. This package is one of the standard library packages you got when you installed Go.

Example:

```
package main // declaration of a package

import "fmt" // Importing an external package

func main() {
    fmt.Println("Hello, World!")
}
```
</details>

<details>
<summary>Writing and executing first go program</summary>
Create a file hello.go file and paste below example code into hello.go file.

Example:

```
package main // declaration of a package

import "fmt" // Importing an external package

func main() {
    fmt.Println("Hello, World!")
}
```

In this code, you:

- _Declare a main package (a package is a way to group functions, and it's made up of all the files in the same directory)._
- _Import the popular fmt package, which contains functions for formatting text, including printing to the console. This package is one of the standard library packages you got when you installed Go._
- _Implement a main function to print a message to the console. A main function executes by default when you run the main package._
- 

**Run your code and see the greeting message:**
```
go run .
```
or
```
go run hello.go
```

**Output:**
```
Hello, World!
```
</details>

<details>

<summary>Call code in the external package</summary>
To create a package, we just need to declare the package name as the first statement of the file:

```
package main // declaration of a package
```
To call a method from another package in Go, you need to follow a few steps:

1. **Import the Package**: First, you need to import the package containing the method you want to call. You do this by including an import statement at the top of your Go source file.

2. **Use the Method**: Once the package is imported, you can access the method by prefixing it with the package name.

Here's a simple example to illustrate these steps:

Suppose you have a package named `utils` with a method called `PrintMessage()` defined in a file named `utils.go`:

```go
// utils.go
package utils

import "fmt"

func PrintMessage(message string) {
    fmt.Println(message)
}
```

Now, in another file, let's say `main.go`, you want to call the `PrintMessage()` method from the `utils` package:

```go
// main.go
package main

import "your-package-path/utils" // import the package

func main() {
    // Call the method from the utils package
    utils.PrintMessage("Hello, World!")
}
```

Make sure to replace `"your-package-path/utils"` with the correct import path to your `utils` package.

By importing the `utils` package and prefixing the method with the package name (`utils.PrintMessage()`), you can call the `PrintMessage()` method from the `utils` package in your `main` function.
</details>


<details>
  <summary>Importance of main package in a go module</summary>
  
  - In Go, the main package holds the entry point to your application. When you build a Go program, you compile it into an executable binary. This binary must contain a package called "main" with a special function called "main()". This function serves as the entry point of your program, meaning it's the first function executed when you run your compiled program.
  
  - The main package is crucial because it defines where the execution of your program begins. Without it, the Go compiler wouldn't know where to start executing your code. It's the backbone of any executable Go program.
  
  - Additionally, when you're working with Go modules, the main package often serves as a reference point for module dependencies. Other packages within your module might depend on functionality provided by the main package, or they might be dependencies of the main package itself. This hierarchical structure helps organize and manage the dependencies of your Go project.
</details>


<details>
  <summary>Pointing your module to use other module dependencies from local machine</summary>
  
  - For production use, you’d publish the example.com/greetings module from its repository (with a module path that reflected its published location), where Go tools could find it to download it. For now, because you haven't published the module yet, you need to adapt the example.com/hello module so it can find the example.com/greetings code on your local file system.
  - To do that, use the go mod edit command to edit the example.com/hello module to redirect Go tools from its module path (where the module isn't) to the local directory (where it is).

  Example:
      
        ```
        
        <home>/
         |-- greetings/
         |-- hello/
        
        ```
  
  Consider above two as modules you have.
    
      ```
  
        go mod edit -replace example.com/greetings=../greetings
        
    ```
  
  * We are saying that instead of downloding the greetings module from any repository, pick it from greetings directory which is parallel to hello directory (we are editing the go.mod of hello module)
  * The command specifies that example.com/greetings should be replaced with ../greetings for the purpose of locating the dependency. After you run the command, the go.mod file in the hello directory should include a replace directive as below:
    ```
    module example.com/hello

    go 1.16
    
    replace example.com/greetings => ../greetings
    ```

- From the command prompt in the hello directory, run the go mod tidy command to synchronize the example.com/hello module's dependencies, adding those required by the code, but not yet tracked in the module.
    ```
    go mod tidy
    ```

  * After the command completes, the example.com/hello module's go.mod file should look like this:
    ```
    module example.com/hello

    go 1.16
    
    replace example.com/greetings => ../greetings
    
    require example.com/greetings v0.0.0-00010101000000-000000000000

     ```
    * The command found the local code in the greetings directory, then added a require directive to specify that example.com/hello requires example.com/greetings. You created this dependency when you imported the greetings package in hello.go.
    * The number following the module path is a pseudo-version number -- a generated number used in place of a semantic version number (which the module doesn't have yet).

- To **reference a published module**, a go.mod file would typically omit the replace directive and use a require directive with a tagged version number at the end.
    ```
    require example.com/greetings v1.1.0
    ```
    
</details>

<details>
  <summary>Return and handle an error</summary>
Sample Code for returnin gerror:
  
  ```
    package greetings
    
    import (
        "errors"
        "fmt"
    )
    
    // Hello returns a greeting for the named person.
    func Hello(name string) (string, error) {
        // If no name was given, return an error with a message.
        if name == "" {
            return "", errors.New("empty name")
        }
    
        // If a name was received, return a value that embeds the name
        // in a greeting message.
        message := fmt.Sprintf("Hi, %v. Welcome!", name)
        return message, nil
    }
  ```

- Change the function so that it returns two values: a string and an error. Your caller will check the second value to see if an error occurred. (Any Go function can return multiple values.)
- Import the Go standard library errors package so you can use its errors.New function.
- Add an if statement to check for an invalid request (an empty string where the name should be) and return an error if the request is invalid. The errors.New function returns an error with your message inside.
- Add nil (meaning no error) as a second value in the successful return. That way, the caller can see that the function succeeded.

Sample code for handling Error:

```
  package main
  
  import (
      "fmt"
      "log"
  
      "example.com/greetings"
  )
  
  func main() {
      // Set properties of the predefined Logger, including
      // the log entry prefix and a flag to disable printing
      // the time, source file, and line number.
      log.SetPrefix("greetings: ")
      log.SetFlags(0)
  
      // Request a greeting message.
      message, err := greetings.Hello("")
      // If an error was returned, print it to the console and
      // exit the program.
      if err != nil {
          log.Fatal(err)
      }
  
      // If no error was returned, print the returned message
      // to the console.
      fmt.Println(message)
  }
```

- Configure the log package to print the command name ("greetings: ") at the start of its log messages, without a time stamp or source file information.
- Assign both of the Hello return values, including the error, to variables.
- Change the Hello argument from Gladys’s name to an empty string, so you can try out your error-handling code.
- Look for a non-nil error value. There's no sense continuing in this case.
- Use the functions in the standard library's log package to output error information. If you get an error, you use the log package's Fatal function to print the error and stop the program.
  
</details>

<details>
  <summary>Writing Unit Tests</summary>
  Go's built-in support for unit testing makes it easier to test as you go. Specifically, using naming conventions, Go's testing package, and the go test command, you can quickly write and execute tests.
  
  - Ending a file's name with **_test.go** tells the go test command that this file contains test functions. Better to create the test file parallel to the file for which you are writing unit tests.
    Example: In the greetings directory, create a file called **greetings_test.go**. (with respect to the modules created in examples given earlier)
  - Sample Unit Test:

    ```
      package greetings

      import (
          "testing"
          "regexp"
      )
      
      // TestHelloName calls greetings.Hello with a name, checking
      // for a valid return value.
      func TestHelloName(t *testing.T) {
          name := "Gladys"
          want := regexp.MustCompile(`\b`+name+`\b`)
          msg, err := Hello("Gladys")
          if !want.MatchString(msg) || err != nil {
              t.Fatalf(`Hello("Gladys") = %q, %v, want match for %#q, nil`, msg, err, want)
          }
      }
      
      // TestHelloEmpty calls greetings.Hello with an empty string,
      // checking for an error.
      func TestHelloEmpty(t *testing.T) {
          msg, err := Hello("")
          if msg != "" || err == nil {
              t.Fatalf(`Hello("") = %q, %v, want "", error`, msg, err)
          }
      }
    ```

      - Implement **test functions in the same package as the code** you're testing.
      - Create two test functions to test the greetings.Hello function. Test function names have the form _Test_**Name**, where **_Name_** says something about the specific test.
      - Test functions take a pointer to the testing package's **testing.T** _type_ as a **parameter**. You use this _parameter's _methods for reporting and logging from your test.
      - Implement two tests:
        - TestHelloName calls the Hello function, passing a name value with which the function should be able to return a valid response message. If the call returns an error or an unexpected response message (one that doesn't include the name you passed in), you use the t parameter's Fatalf method to print a message to the console and end execution.
        - TestHelloEmpty calls the Hello function with an empty string. This test is designed to confirm that your error handling works. If the call returns a non-empty string or no error, you use the t parameter's Fatalf method to print a message to the console and end execution.
       
    - At the command line in the greetings directory, run the ```go test``` command to execute the test. The go test command **executes test functions (whose names begin with Test) in test files (whose names end with _test.go)**. You can add the -v flag to get verbose output that lists all of the tests and their results.

      ```
      go test -v
      ```
  
</details>

<details>
  <summary>Compile and install the application</summary>
  
  - While the go run command is a useful shortcut for compiling and running a program when you're making frequent changes, it doesn't generate a binary executable.
  - Two additional commands for building code:
    - The **go build** command compiles the packages, along with their dependencies, but it doesn't install the results.
    - The **go install** command compiles and installs the packages.
  - From the command line in the hello directory, run the ```go build``` command **to compile the code into an executable**
  - From the command line in the hello directory, run the new hello executable to confirm that the code works.
    - On Linux or Mac: ```./hello```
    - On Windows: ```hello.exe```
  - You've compiled the application into an executable so you can run it. But to run it currently, your prompt needs either to be in the executable's directory, or to specify the executable's path. **How to install the application so that it can be run from anywhere**
  - Discover the Go install path, where the go command will install the current package. You can discover the install path by running the go list command, as in the following example:

    ```
    $ go list -f '{{.Target}}'
    ```
      - For example, the command's output might say /home/gopher/bin/hello, meaning that binaries are installed to /home/gopher/bin. You'll need this install directory in the next step.
  - Add the Go install directory to your system's shell path.
    - On Linux or Mac: ```export PATH=$PATH:/path/to/your/install/directory```
    - On Windows: ```set PATH=%PATH%;C:\path\to\your\install\directory```
  - As an alternative, if you already have a directory like $HOME/bin in your shell path and you'd like to install your Go programs there, you can change the install target by setting the GOBIN variable using the go env command:
    ```
    #Mac or Linux
    go env -w GOBIN=/path/to/your/bin
    ```

    or

    ```
    # Windows
    go env -w GOBIN=C:\path\to\your\bin
    ```

  - Once you've updated the shell path, run the go install command to compile and install the package.
    ```
    go install
    ```

- Run your application by simply typing its name. To make this interesting, open a new command prompt and run the hello executable name in some other directory.
  ```
  $ hello
  map[Darrin:Hail, Darrin! Well met! Gladys:Great to see you, Gladys! Samantha:Hail, Samantha! Well met!]
  ```
</details>

<details>
  <summary>Commentary</summary>

  - Go provides C-style /* */ block comments and C++-style // line comments.
  - Comments that appear before top-level declarations, with no intervening newlines, are considered to document the declaration itself. These “doc comments” are the primary documentation for a given Go package or command
  
</details>

<details>
  <summary>Names and Visibility outside the package</summary>

  - Names are as important in Go as in any other language. They even have semantic effect: the visibility of a name outside a package is determined by whether its first character is upper case. It's therefore worth spending a little time talking about naming conventions in Go programs.
  
</details>

<details>
  <summary>Named result parameters</summary>

  - The return or result "parameters" of a Go function can be given names and used as regular variables, just like the incoming parameters. When named, they are initialized to the zero values for their types when the function begins; if the function executes a return statement with no arguments, the current values of the result parameters are used as the returned values.

  - The names are not mandatory but they can make code shorter and clearer: they're documentation. If we name the results of nextInt it becomes obvious which returned int is which.
  ```
  func nextInt(b []byte, pos int) (value, nextPos int) {}
  ```
  
  Because named results are initialized and tied to an unadorned return, they can simplify as well as clarify. Here's a version of io.ReadFull that uses them well:

  ```
  func ReadFull(r Reader, buf []byte) (n int, err error) {
      for len(buf) > 0 && err == nil {
          var nr int
          nr, err = r.Read(buf)
          n += nr
          buf = buf[nr:]
      }
      return
  }
  ```
</details>

<details>
  <summary>Defer</summary>

 - Go's defer statement schedules a function call (the deferred function) to be run immediately before the function executing the defer returns. It's an unusual but effective way to deal with situations such as resources that must be released regardless of which path a function takes to return. The canonical examples are unlocking a mutex or closing a file.
 - Two Advantages:
   - you will **never forget** to perform the action which you have have deferred.Example closing the file
   - function call which has been deferred **sits close** to the code where it was decided that it has to be peformed. Example defer ```file.close()``` this bit of code will sit near to ```file.open()```
 - The arguments to the deferred function (which include the receiver if the function is a method) are **evaluated when the defer executes**, not when the call executes. Besides avoiding worries about variables changing values as the function executes, this means that a **single deferred call site can defer multiple function executions**.
 - Deferred functions are **executed in LIFO order**
  
</details>

<details>
  <summary>Data Allocation</summary>

  Go has two allocation primitives, the built-in functions new and make. They do different things and apply to different types,
  - Data Allocatoin with **new**
    - It does **not initialize the memory**
    - It only zeros it i.e. new(T) allocates zeroed storage for a new item of type T and returns its address , a value of type *T. In Go terminology, it **returns a pointer to a newly allocated zero value of type T**.
    - zero value of each type can be used without further initialization.
    - For example, the documentation for bytes.Buffer states that "the zero value for Buffer is an empty buffer ready to use."
    - unlike in C, it's perfectly OK to return the address of a local variable; the storage associated with the variable survives after the function returns.
    - **composite literls?**
  - Allocation with **make**
    - The built-in function make(T, args) serves a purpose different from new(T). It **creates slices, maps, and channels only**, and it **returns an initialized (not zeroed) value of type T (not *T)**.
    - The reason for the distinction is that these three types represent, under the covers, references to data structures that must be initialized before use. **A slice, for example, is a three-item descriptor containing a pointer to the data (inside an array), the length, and the capacity, and until those items are initialized, the slice is nil**.
    - For slices, maps, and channels, make initializes the internal data structure and prepares the value for use.
    - Remember that make applies only to maps, slices and channels and does not return a pointer. To obtain an explicit pointer allocate with new or take the address of a variable explicitly.

</details>


<details>
  <summary>Arrays</summary>

  - Arrays are useful when planning the detailed layout of memory and sometimes can help avoid allocation, but primarily they are a building block for slices, the subject of the next section. To lay the foundation for that topic, here are a few words about arrays.

  - There are major differences between the ways arrays work in Go and C. In Go,
    - Arrays are values. Assigning one array to another copies all the elements.
    - In particular, if you pass an array to a function, it will receive a copy of the array, not a pointer to it.
    - The size of an array is part of its type. The types [10]int and [20]int are distinct.
     
</details>

<details>
  <summary>Slices</summary>
  
  - Slices wrap arrays to give a more general, powerful, and convenient interface to sequences of data. Except for items with explicit dimension such as transformation matrices, most array programming in Go is done with slices rather than simple arrays.
  - Slices **hold references to an underlying array**, and if you assign one slice to another, both refer to the same array.
  - If a function takes a slice argument, **changes it makes to the elements of the slice will be visible to the caller**, _analogous to passing a pointer to the underlying array_.
  - A Read function can therefore accept a slice argument rather than a pointer and a count; **the length within the slice sets an upper limit of how much data to read**.
  - Here is the signature of the Read method of the File type in package os:
    ```
    func (f *File) Read(buf []byte) (n int, err error)
    ```
    The method returns the number of bytes read and an error value, if any. To read into the first 32 bytes of a larger buffer buf, slice (here used as a verb) the buffer.
    ```
    n, err := f.Read(buf[0:32])
    ```
  - The length of a slice may be changed as long as it still fits within the limits of the underlying array; just assign it to a slice of itself.
  - The capacity of a slice, accessible by the built-in function cap, reports the maximum length the slice may assume.

    Example:
    ```
    make([]int, 10, 100) // it creates slice of type int, size 10 and capacity 100
    make([]int, 10) // it creates slice of type int and size 10. capacity can be omitted.
    ```
  
</details>

<details>
  <summary>Two-dimensional slices</summary>

  - Go's arrays and slices are one-dimensional. To create the equivalent of a 2D array or slice, it is necessary to define an array-of-arrays or slice-of-slices, like this:
    ```
    type Transform [3][3]float64  // A 3x3 array, really an array of arrays.
    type LinesOfText [][]byte     // A slice of byte slices.
    ```
  - 
  
</details>

<details>
  <summary>Maps</summary>

  - Data structure that associate values of one type (the key) with values of another type (the element or value).
  - The **key** can be of **any type for which the equality operator is defined**, such as integers, floating point and complex numbers, strings, pointers, interfaces (as long as the dynamic type supports equality), structs and arrays.
  - **Slices cannot be used as map keys**, because equality is not defined on them.
  - Like slices, maps _hold references to an underlying data structure_. If you pass a map to a function that **changes** the contents of the map, the changes will be **visible in the caller**.

    ```
    # Example 1:
    
    var timeZone = map[string]int{
        "UTC":  0*60*60,
        "EST": -5*60*60,
        "CST": -6*60*60,
        "MST": -7*60*60,
        "PST": -8*60*60,
    }

    offset := timeZone["EST"]


    # Example 2:
    
    attended := map[string]bool{
        "Ann": true,
        "Joe": true,
        ...
    }

    if attended[person] { // will be false if person is not in the map
        fmt.Println(person, "was at the meeting")
    }
    ```
  - An attempt to fetch a map value with a key that is not present in the map will return the zero value for the type of the entries in the map.
  - To distinguish a missing entry from a zero value. Is there an entry for "UTC" or is that 0 because it's not in the map at all?
    ```
    var seconds int
    var ok bool
    seconds, ok = timeZone[tz]
    
    func offset(tz string) int {
        if seconds, ok := timeZone[tz]; ok {
            return seconds
        }
        log.Println("unknown time zone:", tz)
        return 0
    }
    
    
    # Only to check the presence of a key
    _, present := timeZone[tz]

    ```

  - To delete a map entry, use the delete built-in function, whose arguments are the map and the key to be deleted. It's safe to do this even if the key is already absent from the map.
    ```
    delete(timeZone, "PDT")  // Now on Standard Time
    ```
  
</details>

<details>
  <summary>The init function</summary>

  - Finally, each source file can define its own niladic init function to set up whatever state is required. (Actually each file can have multiple init functions.) And finally means finally: init is called after all the variable declarations in the package have evaluated their initializers, and those are evaluated only after all the imported packages have been initialized.
  - Besides initializations that cannot be expressed as declarations, a common use of init functions is to verify or repair correctness of the program state before real execution begins.

    ```
    func init() {
        if user == "" {
            log.Fatal("$USER not set")
        }
        if home == "" {
            home = "/home/" + user
        }
        if gopath == "" {
            gopath = home + "/go"
        }
        // gopath may be overridden by --gopath flag on command line.
        flag.StringVar(&gopath, "gopath", gopath, "override default GOPATH")
    }

    ```
  
</details>


<details>
  <summary>Interfaces and other types</summary>
  
</details>





<details>
  <summary>Sample Topic  -------------------</summary>
  
</details>
<details>
  <summary>Sample Topic  -------------------</summary>
  
</details>
