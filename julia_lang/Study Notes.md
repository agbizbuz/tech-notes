# Study Notes
## Data Types
- Strings have double or triple quotes
- Data Structures
  - Dictionary
  - Tuple => Immutable sequence
  - Array => Mutable sequence
- Structs
  - Immutable by default
  - Custom constructor with new Keyword
- Accessing elements by columns of a Matrix is faster than accessing by rows as Matrix is stored as array in memory
## Functions
- function arguments can be
  - default
  - positional
  - keyword => after ';'
- Function Broadcast => Apply function to each member of a collection (like for each)
- Multiple Dispatch => functions with same name (called methods of a function) but different arguments
- Info functions
  - methodswith
  - typeof
  - supertype, supertypes
  - subtypes
  - isa => works also as a binary op
  - Base.summarysize => type size
  - isabstract

## Scope
- Use `let` to create a local scope, this helps flow control blocks to access outer variables
- `begin` and `if` blocks don´t introduce new scopes
- **Hard scope** refers to local scopes created by constructs like functions and macros, where variables are not accessible outside their defined block. 
- **Soft scope**, on the other hand, allows access to variables from the outer scope, but can lead to confusion, especially with global variables.
- The constructs introducing scope blocks are:

| Construct                                                                                                                                                                                                                                                                                                                                                | Scope Type Introduced | Scope Types Able to Contain Construct |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- | ------------------------------------- |
| [`module`](https://docs.julialang.org/en/v1/base/base/#module), [`baremodule`](https://docs.julialang.org/en/v1/base/base/#baremodule)                                                                                                                                                                                                                   | global                | global                                |
| [`struct`](https://docs.julialang.org/en/v1/base/base/#struct)                                                                                                                                                                                                                                                                                           | local (hard)          | global                                |
| [`macro`](https://docs.julialang.org/en/v1/base/base/#macro)                                                                                                                                                                                                                                                                                             | local (hard)          | global                                |
| [`for`](https://docs.julialang.org/en/v1/base/base/#for), [`while`](https://docs.julialang.org/en/v1/base/base/#while), [`try`](https://docs.julialang.org/en/v1/base/base/#try)                                                                                                                                                                         | local (soft)          | global, local                         |
| [`function`](https://docs.julialang.org/en/v1/base/base/#function), [`do`](https://docs.julialang.org/en/v1/base/base/#do), [`let`](https://docs.julialang.org/en/v1/base/base/#let), [comprehensions](https://docs.julialang.org/en/v1/manual/arrays/#man-comprehensions), [generators](https://docs.julialang.org/en/v1/manual/arrays/#man-generators) | local (hard)          | global, local                         |
## Editing Environment
### REPL
  - ? => help mode
  - ] => Package mode
  - ; => Shell mode
  - Backspace => Exit a mode
### Misc
- In Pluto.jl, get live documentation when a word is highlighted
- To type a unicode character or emoji start with a '\' and name
  - e.g. \omega => Ω (omega symbol)
## Naming conventions
  - CamelCase => Packages, Modules, Types
  - contineouslowercase/ snake_case => functions, variables
  - Variable names can't start with a number
## Packages and Modules
- `include` brings the whole file into current file
- `using` brings all the exported names in the current file
- `import` or `import as` brings qualified names in the current file
- **module** is a way to organize code into namespace
- **package** is a way to distribute/re-use a set of functionality (can contain multiple modules)
- To generate the bare minimum files for a new package, use `pkg> generate`.
## Environments a.k.a Pkg.jl
- Pkg (or package mode) provides environments for using different packages/versions for each project
- Environments are stackable, e.g. Project specific and development environment
- If different packages use same versions of a package, it is downloaded only once on the hard drive and hence adding lot of packages in the default (Main) environment is not required.
- Shared environments have a `@` before their name in the Pkg REPL prompt