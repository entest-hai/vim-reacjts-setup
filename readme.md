# Setup Vim for JS and Python Project </br>

**01 DEC 2201** </br>

##### Summary

- File explore with defx </br>
- File search with denite and au or so-called silver search </br>
- coc-eslint (eslint, prettier) for javascript autocompletion, syntax, static analysis, document </br>
- coc-python, jedit python autocompletion, syntax, static analysis, document </br>

##### file explore with defx </br>

![vim_js_demo_defx](https://user-images.githubusercontent.com/20411077/144238197-ff3b709e-c0b2-4452-a47f-eb3ed891384b.png)

##### file search with denite and au (silver search) </br>

![vim_js_demo_denite](https://user-images.githubusercontent.com/20411077/144238292-a8f5891c-b0e8-4ea9-897a-fb97ec7134aa.png)

##### autocomple with eslint, prettier, coc-js </br>

![vim_js_demo_coc_js_eslint](https://user-images.githubusercontent.com/20411077/144238380-e366e640-6353-4104-9ac1-bc6732bf40f4.png)

# Setup Neovim for React

### 1. Install neovim

```
Export PATH={path_to_neovim_bin}
```

Simple configuration into

```
.config/nvim/init.vim
```

### 2. Install dein.vim to manage plugins

Vim-plug also works well </br>
Download dein.vim insataller by curl
Update .config/nvim/init/vim
Test install vim-airline

### 3. Install defx.vim for file explore

install defx with dein manage

```
call dein#add('Shougo/denite.nvim')
if !has('nvim')
  call dein#add('roxma/nvim-yarp')
  call dein#add('roxma/vim-hug-neovim-rpc')
endif
```

Need Python3
Then install defx plugin by dein.vim

```
pip3 install --user pynvim
```

If Python3 installed later, then need to run :UpdateRemotePlugins
Configure shorcut for defx

```
call defx#custom#option('_', {
      \ 'winwidth': 30,
      \ 'split': 'vertical',
      \ 'show_ignored_files': 0,
      \ 'buffer_name': 'defxplorer',
      \ 'toggle': 1,
      \ 'resume': 1
      \ })

" Toggle Defx using Ctrl + Space
map <C-space> :Defx<CR>

autocmd FileType defx call s:defx_my_settings()
function! s:defx_my_settings() abort
  " Define mappings
  nnoremap <silent><buffer><expr> <CR> defx#do_action('drop')
  nnoremap <silent><buffer><expr> c
  \ defx#do_action('copy')
  nnoremap <silent><buffer><expr> m
  \ defx#do_action('move')
  nnoremap <silent><buffer><expr> p
  \ defx#do_action('paste')
  nnoremap <silent><buffer><expr> l
  \ defx#do_action('open')
  nnoremap <silent><buffer><expr> E
  \ defx#do_action('open', 'vsplit')
  nnoremap <silent><buffer><expr> P
  \ defx#do_action('open', 'pedit')
  nnoremap <silent><buffer><expr> o
  \ defx#do_action('open_or_close_tree')
  nnoremap <silent><buffer><expr> K
  \ defx#do_action('new_directory')
  nnoremap <silent><buffer><expr> N
  \ defx#do_action('new_file')
  nnoremap <silent><buffer><expr> M
  \ defx#do_action('new_multiple_files')
  nnoremap <silent><buffer><expr> C
  \ defx#do_action('toggle_columns',
  \                'mark:indent:icon:filename:type:size:time')
  nnoremap <silent><buffer><expr> S
  \ defx#do_action('toggle_sort', 'time')
  nnoremap <silent><buffer><expr> d
  \ defx#do_action('remove')
  nnoremap <silent><buffer><expr> r
  \ defx#do_action('rename')
  nnoremap <silent><buffer><expr> !
  \ defx#do_action('execute_command')
  nnoremap <silent><buffer><expr> x
  \ defx#do_action('execute_system')
  nnoremap <silent><buffer><expr> yy
  \ defx#do_action('yank_path')
  nnoremap <silent><buffer><expr> .
  \ defx#do_action('toggle_ignored_files')
  nnoremap <silent><buffer><expr> ;
  \ defx#do_action('repeat')
  nnoremap <silent><buffer><expr> h
  \ defx#do_action('cd', ['..'])
  nnoremap <silent><buffer><expr> ~
  \ defx#do_action('cd')
  nnoremap <silent><buffer><expr> q
  \ defx#do_action('quit')
  nnoremap <silent><buffer><expr> <Space>
  \ defx#do_action('toggle_select') . 'j'
  nnoremap <silent><buffer><expr> *
  \ defx#do_action('toggle_select_all')
  nnoremap <silent><buffer><expr> j
  \ line('.') == line('$') ? 'gg' : 'j'
  nnoremap <silent><buffer><expr> k
  \ line('.') == 1 ? 'G' : 'k'
  nnoremap <silent><buffer><expr> <C-l>
  \ defx#do_action('redraw')
  nnoremap <silent><buffer><expr> <C-g>
  \ defx#do_action('print')
  nnoremap <silent><buffer><expr> cd
  \ defx#do_action('change_vim_cwd')
endfunction
```

### 4. Install denite.vim for file searching

Install

```
call dein#add('Shougo/denite.nvim')
if !has('nvim')
  call dein#add('roxma/nvim-yarp')
  call dein#add('roxma/vim-hug-neovim-rpc')
endif
```

Config

```
" Define mappings
autocmd FileType denite call s:denite_my_settings()
function! s:denite_my_settings() abort
  nnoremap <silent><buffer><expr> <CR>
  \ denite#do_map('do_action')
  nnoremap <silent><buffer><expr> d
  \ denite#do_map('do_action', 'delete')
  nnoremap <silent><buffer><expr> p
  \ denite#do_map('do_action', 'preview')
  nnoremap <silent><buffer><expr> q
  \ denite#do_map('quit')
  nnoremap <silent><buffer><expr> i
  \ denite#do_map('open_filter_buffer')
  nnoremap <silent><buffer><expr> <Space>
  \ denite#do_map('toggle_select').'j'
endfunction

autocmd FileType denite-filter call s:denite_filter_my_settings()
function! s:denite_filter_my_settings() abort
  imap <silent><buffer> <C-o> <Plug>(denite_filter_quit)
endfunction

" Change file/rec command.
call denite#custom#var('file/rec', 'command',
\ ['ag', '--follow', '--nocolor', '--nogroup', '-g', ''])
```

To use it need to install silver search or other search eigen
[the silver search git](https://github.com/ggreer/the_silver_searcher)

```
brew install the_silver_searcher
```

Usage </br>
:Denite file/rec </br>
type i to enter insert mode for Denite buffer </br>
then type PATTERN to search and open wanted file </br>
ESC to escape insert mode, enter back to denite list </br>

### 5. Install autocompletion and eslint with coc.envm

```
call dein#add('neoclide/coc.nvim', { 'merged': 0, 'rev': 'release' })

```

Coc extensions

```
let g:coc_global_extensions = [
  \ 'coc-json',
  \ 'coc-tsserver',
  \ 'coc-prettier',
  \ 'coc-eslint',
  \ 'coc-python',
  \ 'coc-pylsp ',
  \ 'coc-flutter',
  \ ]
```

### 6. Netrw option instaned of defx

Netrw is slower than defx but working fine, and built-in with neovim. </br>
Config

```
" netrw config
if &columns < 90
  " If the screen is small, occupy half
  let g:netrw_winsize = 30
else
  " else take 30%
  let g:netrw_winsize = 20
endif

let g:netrw_liststyle = 3
let mapleader = ","
let g:netrw_keepdir = 0
map <C-a> :Lexplore<CR>
map <C-d> :Lexplore %:p:h<CR>
```

### 7. Global config and color

```
" global neovim config
:set number
":set autoindent
:set tabstop      =2
:set shiftwidth   =2
:set softtabstop  =2
:set expandtab
:set mouse=a
:set background=dark
:set hlsearch
:set showcmd
:set title
```

Cursor movement in both insert and comamnd mode

```
" move cursor to end of line in both mode
map <C-a> <ESC>^
imap <C-a> <ESC>I
map <C-e> <ESC>$
imap <C-e> <ESC>A
```

Cursor highlight color

```
" highlight current line
:set cursorline
:highlight CursorLine cterm=NONE ctermbg=23 ctermfg=NONE guibg=Grey40
```

### 8. Denite search

```
let s:denite_win_width_percent = 0.8
let s:denite_win_height_percent = 0.7
let s:denite_default_options = {
    \ 'split': 'floating',
    \ 'winwidth': float2nr(&columns * s:denite_win_width_percent),
    \ 'wincol': float2nr((&columns - (&columns * s:denite_win_width_percent)) / 2),
    \ 'winheight': float2nr(&lines * s:denite_win_height_percent),
    \ 'winrow': float2nr((&lines - (&lines * s:denite_win_height_percent)) / 2),
    \ 'highlight_filter_background': 'DeniteFilter',
    \ 'prompt': 'Œª ',
    \ 'start_filter': v:true,
    \ }
let s:denite_option_array = []
for [key, value] in items(s:denite_default_options)
  call add(s:denite_option_array, '-'.key.'='.value)
endfor
call denite#custom#option('default', s:denite_default_options)

" Ag command on grep source
call denite#custom#var('grep', 'command', ['ag'])
call denite#custom#var('grep', 'default_opts', ['-i', '--vimgrep'])
call denite#custom#var('grep', 'recursive_opts', [])
call denite#custom#var('grep', 'pattern_opt', [])
call denite#custom#var('grep', 'separator', ['--'])
call denite#custom#var('grep', 'final_opts', [])

" grep
command! -nargs=? Dgrep call s:Dgrep(<f-args>)
function s:Dgrep(...)
  if a:0 > 0
    execute(':Denite -buffer-name=grep-buffer-denite grep -path='.a:1)
  else
    let l:path = expand('%:p:h')
    if has_key(defx#get_candidate(), 'action__path')
      let l:path = fnamemodify(defx#get_candidate()['action__path'], ':p:h')
    endif
    execute(':Denite -buffer-name=grep-buffer-denite -no-empty '.join(s:denite_option_array, ' ').' grep -path='.l:path)
  endif
endfunction

" show Denite grep results
command! Dresume execute(':Denite -resume -buffer-name=grep-buffer-denite '.join(s:denite_option_array, ' ').'')

nnoremap <silent> ;r :<C-u>Dgrep<CR>
```

### 9 auto close tag, auto pair, and nice jsx format

[vim-jsx-pretty](https://github.com/MaxMEllon/vim-jsx-pretty)

```
'maxmellon/vim-jsx-pretty'
```

[auto-pairs](https://github.com/jiangmiao/auto-pairs)

```
'jiangmiao/auto-pairs'
```

[vim-closetag](https://github.com/alvan/vim-closetag)

```
'alvan/vim-closetag'
```

### 10 Basic thing while editing with Vim

Move cursor in insert mode
```
cnoremap <S-h> <Left>
inoremap <S-h> <Left>
inoremap <S-j> <Down>
inoremap <S-k> <Up>
inoremap <S-l> <Right>
```

```
ctr h to left
ctr l to right
ctr k to up
ctr j to down
```

Setup

```
ironemap <A-w> <C-o> w
ironemap <A-b> <C-o> b
```

```
alt w
alt b
```

Another way is to ctr o to back to normal mode for one command

Move cursor in normal mode

```
b to back one word
w to forward one word
^ to go to begining of a line
$ to go to end of a line
```

Delete multiple line

```
normal mode shift v, then x
```

Edit multiple line

```
normal mode ctr v, edit, then esc
```

Copy a line

```
normal mode yy then p
```

Search and jump by key word

```
:/keyword then n - next, N - previous
```

Open a file in a new tab

```
:tabe to open new tabe
:tabn move to next tab
:tabp move to prev tab
```

Split screen vertically

```
ctr w and v
ctr w and l
ctr w and r
```

move line up and dow

```
shift up arrow
shift down arrow
:m+2
```

Search file by denite

```
:Denite file/rec
```

String subtitue current line

```
:.s/old/new/g
```

String subtitue entire file

```
:%s/old/new/g
```
Copy to clipboard 
```
shift v or ctrl v to visual mode 
: ! xclip -selection clipboard
```

Copy from cursor to the end 
```
: .,$yank
```
Copy from cursor to next x line 
```
:start,endyank
```


### 11 remove plug in and reinstall

Delete dein directory

```
rm -rf ~/.cache/dein
```

Resintall dein

```
curl https://raw.githubusercontent.com/Shougo/dein.vim/master/bin/installer.sh > installer.sh
sh ./installer.sh ~/.cache/dein
```

Open vim and reinstall plugins

```
call dein#install()
```

### 12 estlint fix on save

Need to open **coc-setting.json** and edit </br>
Reference [here](https://vimawesome.com/plugin/coc-eslint) and [here](https://github.com/neoclide/coc-eslint)

```
:CocConfig
```

```
eslint.autoFixOnSave: true
```

### 13 Debug

Check is eslint server running well or not

```
:CocCommand: showOutputChannel
```

Might need to install eslint version in package.json

```
"eslint": "7.32.0"
```

Show coc-setting.json

```
:CocConfig
```

Content of coc-setting.json

```
{
  "eslint.format.enable": true,
  "eslint.autoFixOnSave": true,
  "eslint.enable": true
}
```

# Setup eslint and prettier for NextJS

**07 DEC 2201**

### Init project with npm

This will create package.json

```
npm init -y
```

### Update package.json

```
npm i @chakra-ui/react @emotion/react @emotion/styled framer-motion next react react-dom react-table react-icons chakra-ui/icons
```

```
{
  "name": "haitran-homepage",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "keywords": [],
  "author": "Hai Tran",
  "license": "MIT",
  "dependencies": {
    "@chakra-ui/icons": "^1.1.1",
    "@chakra-ui/react": "^1.7.2",
    "@emotion/react": "^11.6.0",
    "@emotion/styled": "^11.6.0",
    "framer-motion": "^5.3.1",
    "next": "^12.0.4",
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "react-icons": "^4.3.1",
    "three": "^0.134.0"
  },
  "devDependencies": {
    "eslint": "^7.32.0",
    "eslint-config-next": "^12.0.4",
    "prettier": "^2.4.1"
  }
}
```

### First page

```
./pages/index.js
```

```
function HomePage() {
  return <div>Welcome to Next.js!</div>
}

export default HomePage
```

### Eslint and prettier configuration

```
npm i -D eslint eslint-config-next prettier
```

.eslintrc.json

```
{
    "root": true,
    "extends": "next",
    "rules": {
      "quotes":[
        "error",
        "single"
      ],
      "indent":[
        "error",
        2
      ],
      "no-unused-vars": [
        "error",
        {
          "argsIgnorePattern": "^_",
          "varsIgnorePattern": "^_"
        }
      ],
      "react/display-name": 0
    }
  }
```
eslint ignore un-used-var 
```
"varsIgnorePattern": [
          "^_",
          "colorMode"
        ]
``` 

eslint disable one line 
```
// eslint-disable-next-line no-use-before-define
```

prettier.config.js

```
const options = {
    arrowParens: 'avoid',
    singleQuote: true,
    bracketSpacing: true,
    endOfLine: 'lf',
    semi: false,
    tabWidth: 2,
    trailingComma: 'none'
  }

  module.exports = options
```

### Vim basic things
15 DEC 2201 <br /> 
i-insert mode, n-normal mode, v-visual mode <br />
combine number with <h, i, j, k> to move <br />
go to begining of a line type 0 <br/>
go to first line type gg <br /> 
go to last line type <S-g> <br />
go to a line number 2 <S-g> <br />
move word by word type w <br /> 
move end of word type e <br />
move by 5 words type 5w <br /> 
delete a character type x <br /> 
delete two characters type 2x <br /> 
delete a word type dw <br />
delete a line type dd <br />
delete 10 lines type 10dd <br />
paste by <p> <br /> 
replace a character by <r> <new> <br />
delete a word and into insert <c> <e> <br />
delete a line and into isnert mode <c> <$> <br /> 
