[![Build](https://github.com/nao1215/mkgoprj/actions/workflows/build.yml/badge.svg?branch=main)](https://github.com/nao1215/mkgoprj/v2/actions/workflows/build.yml)
[![reviewdog](https://github.com/nao1215/mkgoprj/actions/workflows/review_dog.yml/badge.svg)](https://github.com/nao1215/mkgoprj/v2/actions/workflows/review_dog.yml)
[![Go Report Card](https://goreportcard.com/badge/github.com/nao1215/mkgoprj/v2)](https://goreportcard.com/report/github.com/nao1215/mkgoprj/v2)   
[[日本語]](./doc/README.ja.md)
# mkgoprj - Golang project template generator
![Screenshot](./doc/images/demo.gif) 
  
mkgoprj command generate golang project template at current directory. The following three projects can be created.
- Library project
- Command Line Interface project with [cobra](https://github.com/spf13/cobra)

The automatically generated files include "Makefile for easy project management" and "GitHub Actions files (build, unit test, review-dog, goreleaser, dependabot)". However, it does not run "$ git init".  
  
# How to install
## Step1. Install golang
If you don't install golang in your system, please install Golang first. Check the [Go official website](https://go.dev/doc/install) for how to install golang.
## Step2. Install mkgoprj
```
$ go install github.com/nao1215/mkgoprj/v2@latest
```
  
# How to use
## Generate application project
In the following example, the mkgoprj command will generate a sample project. The binary name will be sample, and build using Makefile.
```
$ mkgoprj cli github.com/nao1215/sample  ※ Argument is same as "$ go mod init"
mkgoprj starts creating the 'sample' application project (import path='github.com/nao1215/sample')

[START] check if mkgoprj can create the project
[START] create directories
[START] create files
        sample (your project root)
         ├─ CODE_OF_CONDUCT.md
         ├─ main.go
         ├─ Makefile
         ├─ Changelog.md
         ├─ .goreleaser.yml
         ├─ cmd
         │  ├─ version.go
         │  └─ root.go
         ├─ .github
         │  ├─ dependabot.yml
         │  ├─ ISSUE_TEMPLATE
         │  │  ├─ issue.md
         │  │  └─ bug_report.md
         │  └─ workflows
         │     ├─ reviewdog.yml
         │     ├─ build.yml
         │     ├─ unit_test.yml
         │     ├─ release.yml
         │     └─ contributors.yml
         └─ internal
            ├─ cmdinfo
            │  └─ cmdinfo.go
            ├─ completion
            │  └─ completion.go
            └─ print
               └─ print.go
[START] Execute 'go mod init github.com/nao1215/sample'
[START] Execute 'go mod tidy'

BUILD SUCCESSFUL in 2485[ms]

$ cd sample
$ make build
$ ls
CODE_OF_CONDUCT.md  Changelog.md  Makefile  cmd  go.mod  go.sum  internal  main.go  sample
```

## Generate library project
```
$ mkgoprj library github.com/nao1215/sample
mkgoprj starts creating the 'sample' library project (import path='github.com/nao1215/sample')

[START] check if mkgoprj can create the project
[START] create directories
[START] create files
        sample (your project root)
         ├─ sample_test.go
         ├─ CODE_OF_CONDUCT.md
         ├─ Makefile
         ├─ Changelog.md
         ├─ sample.go
         └─ .github
            ├─ dependabot.yml
            ├─ ISSUE_TEMPLATE
            │  ├─ issue.md
            │  └─ bug_report.md
            └─ workflows
               ├─ reviewdog.yml
               ├─ unit_test.yml
               └─ contributors.yml
               
[START] Execute 'go mod init github.com/nao1215/sample'

BUILD SUCCESSFUL in 5[ms]

$ cd sample/
$ make test
env GOOS=linux go test -v -cover ./... -coverprofile=cover.out
=== RUN   TestHelloWorld
--- PASS: TestHelloWorld (0.00s)
PASS
coverage: 100.0% of statements
ok      github.com/nao1215/sample       0.002s  coverage: 100.0% of statements
go tool cover -html=cover.out -o cover.html
```

# Self-documented Makefile
The Makefile generated by the mkgoprj command is [self-documenting](https://marmelab.com/blog/2016/02/29/auto-documented-makefile.html). When you run the make command, you will see the target list that exists in the Makefile. A help message is showed next to the target name.
```
$ make
build           Build binary 
clean           Clean project
fmt             Format go source code 
test            Start test
vet             Start go vet
```
If you want to add a new target, please comment with **"##"** next to the target. The string after "##" is extracted and used as a help message. An example is shown below.
```
build:  ## Build binary 
	env GO111MODULE=on GOOS=$(GOOS) $(GO_BUILD) $(GO_LDFLAGS) -o $(APP) cmd/sample/main.go

clean: ## Clean project
	-rm -rf $(APP) cover.out cover.html
```

# Auto-generate shell completion file (for bash, zsh, fish)
mkgoprj command automatically generates shell completion files for bash, zsh, and fish. After the user executes mkgoprj, if the shell completion file does not exist in the system, the auto-generation process will begin. To activate the completion feature, restart the shell.

```
$ mkgoprj 
mkgoprj:INFO : append bash-completion file: /home/nao/.bash_completion
mkgoprj:INFO : create fish-completion file: /home/nao/.config/fish/completions/mkgoprj.fish
mkgoprj:INFO : create zsh-completion file: /home/nao/.zsh/completion/_mkgoprj
```

# Contact
If you would like to send comments such as "find a bug" or "request for additional features" to the developer, please use one of the following contacts.

- [GitHub Issue](https://github.com/nao1215/mkgoprj/v2/issues)

# LICENSE
The mkgoprj project is licensed under the terms of [the Apache License 2.0](./LICENSE).