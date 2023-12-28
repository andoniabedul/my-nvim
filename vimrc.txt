

"############################
"######## THEME #############
"############################

set termguicolors
set background=dark
colorscheme solarflare-contrast-customized

" Avoid fold code
set nofoldenable

" Disable compatibility with vi which can cause unexpected issues.
set nocompatible

" Enable type file detection. Vim will be able to try to detect the type of file in use.
filetype on

" Enable plugs and load plug for the detected file type.
filetype plugin on

" Load an indent file for the detected file type.
filetype indent on
syntax on
set number

" Highlight cursor line underneath the cursor horizontally.
set cursorline

" Highlight cursor line underneath the cursor vertically.
set cursorcolumn

" Set shift width to 4 spaces.
set shiftwidth=2

" Set tab width to 4 columns.
set tabstop=2

" Use space characters instead of tabs.
set expandtab

" Do not save backup files.
set nobackup

" Do not let cursor scroll below or above N number of lines when scrolling.
set scrolloff=10

" Do not wrap lines. Allow long lines to extend as far as the line goes.
set nowrap

" While searching though a file incrementally highlight matching characters as you type.
set incsearch

" Ignore capital letters during search.
set ignorecase

" Override the ignorecase option if searching for capital letters.
" This will allow you to search specifically for capital letters.
set smartcase

" Show partial command you type in the last line of the screen.
set showcmd

" Show the mode you are on the last line.
set showmode

" Show matching words during a search.
set showmatch

" Use highlighting when doing a search.
set hlsearch

" Set the commands to save in history default number is 20.
set history=1000

" Enable auto completion menu after pressing TAB.
set wildmenu

" Make wildmenu behave like similar to Bash completion.
set wildmode=list:longest

" There are certain files that we would never want to edit with Vim.
" Wildmenu will ignore files with these extensions.
set wildignore=*.docx,*.jpg,*.png,*.gif,*.pdf,*.pyc,*.exe,*.flv,*.img,*.xlsx

set wrap

" Optimize navigation
set lazyredraw 

" Open vs o vnew on split right
set splitright

set hid

set hidden

set virtualedit=onemore

set showtabline=2

" PLUGINS ---------------------------------------------------------------- {{{
" Plugins code goes here.

call plug#begin('~/.vim/plugged')
  Plug 'dense-analysis/ale'
  Plug 'preservim/nerdtree'
  Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
  Plug 'junegunn/fzf.vim'
  Plug 'neoclide/coc.nvim', {'branch': 'release'}
  " Plug 'Xuyuanp/nerdtree-git-plug'
  Plug 'scrooloose/nerdcommenter'
  Plug 'prettier/vim-prettier', { 'do': 'yarn install' } "
  Plug 'morhetz/gruvbox'
  Plug 'jremmen/vim-ripgrep'
  Plug 'vim-airline/vim-airline'
  Plug 'vim-airline/vim-airline-themes'
  Plug 'pangloss/vim-javascript'
  Plug 'sheerun/vim-polyglot'
  set encoding=UTF-8
call plug#end()

  " }}}

" MAPPINGS --------------------------------------------------------------- {{{

" Mappings code goes here.
let mapleader = "\<Space>"

inoremap jk <ESC>

" NERDTree
nmap <C-e> :NERDTreeToggle<CR>
vmap ++ <plug>NERDCommenterToggle
nmap ++ <plug>NERDCommenterToggle

" j/k will move virtual lines (lines that wrap)
noremap <silent> <expr> j (v:count == 0 ? 'gj' : 'j')
noremap <silent> <expr> k (v:count == 0 ? 'gk' : 'k')

" Save with Ctrl + s
nnoremap <C-s> :w<CR>
" Save and quit with Ctrl` + Q
nnoremap <C-Q> :wq<CR>

" fzf with ctrl + p
noremap <C-p> :GFiles<CR>
" rimgrep find in in files with ctrl + f
noremap <C-f> :Rg -i --smart-case --follow --ignore-file ~/.ignore <C-R><C-W><CR>

" Show tabs opened
noremap <C-t> :W<CR>

" shift+arrow selection
nmap <S-Up> v<Up>
nmap <S-Down> v<Down>
nmap <S-Left> v<Left>
nmap <S-Right> v<Right>
vmap <S-Up> <Up>
vmap <S-Down> <Down>
vmap <S-Left> <Left>
vmap <S-Right> <Right>
imap <S-Up> <Esc>v<Up>
imap <S-Down> <Esc>v<Down>
imap <S-Left> <Esc>v<Left>
imap <S-Right> <Esc>v<Right>

vmap <C-c> y<Leader>i
vmap <C-x> d<Leader>i
map <C-v> pi
imap <C-v> <Esc>pi

" Moving between tabs
nnoremap <Leader>tn :tabnew<CR>
nnoremap <Leader>th :tabp<CR>
nnoremap <Leader>tl :tabn<CR>
nnoremap <Leader>tc :tabc<CR>
nnoremap <Leader>tp :tablast<CR>

" Copying file paths
" Relative path
nnoremap <Leader>cp :let @+=expand("%")<CR>
" Absolute path
nnoremap <Leader>cfp :let @+=expand("%:p")<CR>

" Saving with leader + s
nnoremap <Leader>s :w<CR>

" Using coc.vim to jump to definition
nmap <silent> <leader>rd <Plug>(coc-definition)
nmap <silent> <leader>rt <Plug>(coc-type-definition)
nmap <silent> <leader>ri <Plug>(coc-implementation)
nmap <silent> <leader>rr <Plug>(coc-references)

"This unsets the "last search pattern" register by hitting return
nnoremap <CR> :noh<CR><CR>

set cindent

" }}}

" VIMSCRIPT -------------------------------------------------------------- {{{

" This will enable code folding.
" Use the marker method of folding.
augroup filetype_vim
    autocmd!
    autocmd FileType vim setlocal foldmethod=marker
augroup END


" sync open file with NERDTree
" Check if NERDTree is open or active
function! IsNERDTreeOpen()
  return exists("t:NERDTreeBufName") && (bufwinnr(t:NERDTreeBufName) != -1)
endfunction

let g:NERDTreeIgnore = ['^node_modules$']
let g:ale_completion_enabled = 0
let g:ale_linters = {'python': ['flake8', 'pylint'], 'javascript': ['eslint']}
let g:ale_enabled = 0

let g:NERDTreeDirArrowExpandable="+"
let g:NERDTreeDirArrowCollapsible="~"

" vim-airline
let g:airline#extensions#tabline#enabled = 1
let g:airline_theme='tomorrow'
let g:fzf_action = { 'enter': 'tab split' }
let g:rg_derive_root=1

" Returns true if the color hex value is light
function! IsHexColorLight(color) abort
  let l:raw_color = trim(a:color, '#')

  let l:red = str2nr(substitute(l:raw_color, '(.{2}).{4}', '1', 'g'), 16)
  let l:green = str2nr(substitute(l:raw_color, '.{2}(.{2}).{2}', '1', 'g'), 16)
  let l:blue = str2nr(substitute(l:raw_color, '.{4}(.{2})', '1', 'g'), 16)

  let l:brightness = ((l:red * 299) + (l:green * 587) + (l:blue * 114)) / 1000

  return l:brightness > 155
endfunction

" }}}

" ALIAS --------------------------------------------------------------- {{{



" }}}

" STATUS LINE ------------------------------------------------------------ {{{

" Status bar code goes here.
" }}}

