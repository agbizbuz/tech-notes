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
