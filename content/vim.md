[return to table](../README.md)

---

# Vim

## Quit and Save
- ```:q``` for quit
- ```:q!``` for quit without saving
- ```:wq``` for save and quit
- ```:w``` for save (write)


## Edit
- ```i``` (in command mode) for insert
- ```Esc``` (in insert mode) return to command mode
- ```dd``` (in command mode) for delete the entire line
- ```x``` (in visual mode) for delete the selected text
- ```v``` (in command mode) for selecting (activate visual mode)

### Copy and Paste (in Visual Mode)
- ```d``` cut
- ```y``` copy
- ```p``` paste



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
