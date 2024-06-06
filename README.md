# Practice Go-Language and It's Concepts
For Learning Go language

Learning materials:

https://go.dev/learn/#tutorials

https://gobyexample.com/

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
  
  - Ending a file's name with _test.go tells the go test command that this file contains test functions. Better to create the test file parallel to the file for which you are writing unit tests.
    Example: In the greetings directory, create a file called greetings_test.go. (with respect to the modules created in examples given earlier)
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
      - Create two test functions to test the greetings.Hello function. Test function names have the form Test**Name**, where **_Name_** says something about the specific test.
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
  <summary>Sample Point 1</summary>
  
</details>
<details>
  <summary>Sample Point 1</summary>
  
</details>
