# dotfiles Management with Gnu Stow

## dotfiles structure
```sh
~/dotfiles/
├── vim/
│   ├── .vimrc
│   ├── .vim/
├── git/
│   ├── .gitconfig
│   ├── .gitignore
```

## Basic commands

```sh
# Adopt preexisting files
stow --adopt package_name
# Stow a package
stow package_name
# unstow a package
stow -D package_name
# restow a package
stow -R package_name

# stow all packages in the current directory
stow -v .
```

## References
1. [Managing dotfiles with stow - Apiumhub](https://apiumhub.com/tech-blog-barcelona/managing-dotfiles-with-stow/)
2. [Never Lose Your Configs Again | Typecraft Learn](https://typecraft.dev/tutorial/never-lose-your-configs-again)
3. [GNU Stow command cheat sheet](https://community.inkdrop.app/1670abd373a245635cce1efd87fb95d5/PHQECLN_)
4. [How to Manage Your Dotfiles Like a Pro with Git and Stow - DEV Community](https://dev.to/crafts69guy/how-to-manage-your-dotfiles-like-a-pro-with-git-and-stow-3pg1)
