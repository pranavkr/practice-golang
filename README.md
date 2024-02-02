# Pracice Go-Language and It's Concepts
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
</details>