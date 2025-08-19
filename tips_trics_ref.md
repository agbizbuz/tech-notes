# Tips N Tricks, Quick References

# ZSH
```sh
# append
path+=('/home/david/pear/bin')
# or prepend
path=('/home/david/pear/bin' $path)

# and don't forget to export to make it inherited by child processes
export PATH
```
