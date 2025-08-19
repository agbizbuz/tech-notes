# Python Dev Setup

## UV
One tool to manage all Python development

### UV Basics
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

```


## Resources
1. [UV Tutorial](https://realpython.com/python-uv/)
2. [UV Documentation](https://docs.astral.sh/uv/)
