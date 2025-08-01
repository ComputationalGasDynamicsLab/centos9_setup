#### Command to enable syntax highlighting inside `vim`.

Add below lines to `~/.vimrc` file:
```
autocmd BufNewFile,BufRead *.tpp setfiletype cpp                                
syntax on                                                                       
set colorcolumn=80                                                              
set number                                                                                                                                                   
au BufRead,BufNewFile *.tpp set filetype=cpp
```
