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
<summary>First step before running any go code (even if it is Hello World)</summary>
1. Target code must belong to a module.
  To create a module you need to run the mod init command
  
  example:
    
  ```
  go mod init module_path
  ```
  _**module_path:**_

  If you publish a module, this must be a path from which your module can be downloaded by Go tools. That would be your code's repository.
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

</details>


<details>
  <summary>Importance of main package in a go module</summary>
  
  - In Go, the main package holds the entry point to your application. When you build a Go program, you compile it into an executable binary. This binary must contain a package called "main" with a special function called "main()". This function serves as the entry point of your program, meaning it's the first function executed when you run your compiled program.
  
  - The main package is crucial because it defines where the execution of your program begins. Without it, the Go compiler wouldn't know where to start executing your code. It's the backbone of any executable Go program.
  
  - Additionally, when you're working with Go modules, the main package often serves as a reference point for module dependencies. Other packages within your module might depend on functionality provided by the main package, or they might be dependencies of the main package itself. This hierarchical structure helps organize and manage the dependencies of your Go project.
</details>
