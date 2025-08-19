# Python Dev Setup

## UV
One tool to manage all Python development

### Python Version Management
```sh
uv python install # install latest python version
uv python install 3.11 3.12 # install specific versions

uv python list # view installed python versions
```

### Project Management
Initialise a project from scratch or takeover the current project
```sh
# initialization
uv init <project-name> # create from scratch including directory
uv init # takeover existing project - make sure no pyproject.toml exists

# dependency management
uv add -r requirements.txt # migrate requirements
uv add <package_name>
uv add --upgrade <package_name>
uv remove <package_name>

uv add --dev <package_name> # development dependency
uv pip list # list installed dependencies

# explicit locking and syncing
uv lock
uv sync
uv lock --check # check if lockfile is up-to-date

# building and publishing
uv build
uv publish --index testpypi --token <your_token_here>
```

### Tools
```sh
uvx ruff # run tool without installing
uv tool install ruff # install tool
```


## Resources
1. [UV Tutorial](https://realpython.com/python-uv/)
2. [UV Documentation](https://docs.astral.sh/uv/)
