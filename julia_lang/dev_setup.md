# Julia Dev Setup

# Font
[Julia Mono](https://juliamono.netlify.app/download/)

## Installation
Install using [juliaup](https://github.com/JuliaLang/juliaup)

## Scenarios
1. REPL - For quick checks and practice
2. Standalone Scripts - for reproducible quick practice
3. Standalone Project - bunch of reusable code with multiple files
4. Package - Sharable library
5. App - *Experimental as of today* - executable apps
- Use project environments whenever there is external package dependency and always for the Scenarios 4 & 5.
- Keep the default environment clean with only bare necessary packages (may be like Pluto)
- It's a good idea to create a main function and return `nothing` from standalone (See [[]])
## References
1. [Modern Julia Workflows](https://modernjuliaworkflows.org/) 
2. [Working with Julia projects](https://www.juliabloggers.com/working-with-julia-projects/)