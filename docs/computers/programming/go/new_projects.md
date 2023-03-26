# New Go projects

2023-02-16

Every Go project needs two things: a folder on disk to keep its source code in, and a go.mod file which identifies the module, or project name.[0]

Go organizes code into units called modules, and each module needs to be in a separate folder (you can’t have modules within modules). Each module needs a go.mod file which tells Go that this folder contains a module, and what the name of the module is.[0]

`go mod init MODULE_NAME` initializes new project folder

## Citations
- [0] For the Love of Go, by John Arundel