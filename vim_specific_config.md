### Grep
1. Install Ripgrep

`sudo pacman -S ripgrep`

2. Install mhinz/vim-grepper in $vimrc

`Plug 'mhinz/vim-grepper', { 'on': ['Grepper', '<plug>(GrepperOperator)'] }`

3. $vimrc config

```vim
let g:grepper       = {}
let g:grepper.tools = ['grep', 'rg', 'git']

" Search for the current word
nnoremap <Leader>* :Grepper -cword -noprompt<CR>

" Search for the current selection
nmap gs <plug>(GrepperOperator)
xmap gs <plug>(GrepperOperator)
```

4. alias

```vim
function! SetupCommandAlias(input, output)
  exec 'cabbrev <expr> '.a:input
        \ .' ((getcmdtype() is# ":" && getcmdline() is# "'.a:input.'")'
        \ .'? ("'.a:output.'") : ("'.a:input.'"))'
endfunction
call SetupCommandAlias("grep", "GrepperGrep")
```

5. Interact with Grepper

`:Grepper`

Using < TAB > to change Grepper tools


---

### neovim-remote
`pip3 install --user --upgrade neovim-remote`
using nvr to launch neovim-remote
