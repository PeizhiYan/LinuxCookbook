[return to table](../README.md)

---

# Vim

## Quit and Save
- ```:q``` for quit
- ```:q!``` for quit without saving
- ```:wq``` for save and quit
- ```:w``` for save (write)

## Edit
- ```i``` for insert
- ```Esc``` return to command mode


## Vim Configuration File

Place the file at ```~/.vimrc```. Following is an example ```.vimrc```:
```
:syntax on
filetype indent on
set ai
set mouse=a
set incsearch
set confirm
set number
set ignorecase
set smartcase
set wildmenu
set wildmode=list:longest,full
set softtabstop=4
```
