# cp.nvim

neovim plugin for cp

# Features

- Problem parser
- Multiple problem
- Extensive multitest
- Hotkey submit
- Stresstest
- Terminal intergration
- GDB support

# Config

.vimrc example

```vim
"/ cp.nvim config
if argv(0) == 'cp'
py3 << EOF
with open('/tmp/cp.conf', 'w') as f: f.write("""
cur = ''
compileCmd = 'g++ -O2 -Wall'
codePath = expanduser('~') + f'/code'
templatePath = f'~/code/templates/multi.cpp'
regrexLink = [
  r'.*://codeforces.com/(?:gym|contest)/([1-9][0-9]*)/problem/(0|[A-Z][1-9]?)',
  r'.*://codeforces.com/problemset/problem/([1-9][0-9]*)/([A-Z][1-9]?)',
  r'.*://atcoder.jp/contests/([a-z]*[0-9]*)/tasks/([a-z]*[0-9]*_[a-z][1-9]?)'
]
""")
EOF
for i in range(0, 9)
  execute "noremap <A-" . i . "> :call CpCmd('tab(" . i . ")')<CR>"
endfor
noremap <A-t> :2wincmd w \| term<CR>i
noremap <A-S> :wa \| call CpCmd('threading.Thread(target = problem[cur].submit).start()')<CR>
noremap <A-c> :wa \| call CpCmd('threading.Thread(target = problem[cur].compile).start()')<CR>
noremap <A-r> :wa \| call CpCmd('threading.Thread(target = problem[cur].run(problem[cur].curTest).start())')<CR><CR>
noremap <A-a> :wa \| call CpCmd('threading.Thread(target = problem[cur].compile_run).start()')<CR><CR>
noremap <A-d> :call CpCmd('problem[cur].remove(problem[cur].curTest)')<CR>
noremap <A-s> :call CpCmd('problem[cur].hide_show(problem[cur].curTest)')<CR>
noremap <A-m> :call CpCmd('problem[cur].show_all()')<CR>
noremap <A-o> :call CpCmd('problem[cur].hide("AC")')<CR>
noremap <A-i> :call CpCmd('problem[cur].invert()')<CR>
noremap t :<C-u>call CpTest(v:count, 1, 'l', 0)<CR>
function CpAdd(...)
  call CpCmd('problem[cur].add(' . v:count .')')
endfunction
noremap A :<C-u>call CpAdd()<CR>
" Define colors
hi HD ctermfg=White ctermbg=none guifg=#ffffff guibg=none
hi NA ctermfg=White ctermbg=DarkGray guifg=#ffffff guibg=#ABB2BF
hi PD ctermfg=White ctermbg=Gray guifg=#C678DD guibg=#ffffff
hi AC ctermfg=White ctermbg=Green guifg=#ffffff guibg=#98C379
hi WA ctermfg=White ctermbg=Red guifg=#ffffff guibg=#E06C75
hi RE ctermfg=White ctermbg=Blue guifg=#ffffff guibg=#61AFEF
hi TL ctermfg=White ctermbg=Yellow guifg=#ffffff guibg=#E5C07B
" Black fg for Sel
hi fHD ctermfg=Black ctermbg=none guifg=#000000 guibg=none
hi fPD ctermfg=Black ctermbg=Gray guifg=#000000 guibg=#ABB2BF
hi fNA ctermfg=Black ctermbg=DarkGray guifg=#000000 guibg=#ABB2BF
hi fAC ctermfg=Black ctermbg=DarkGreen guifg=#000000 guibg=#98C379
hi fWA ctermfg=Black ctermbg=Red guifg=#000000 guibg=#E06C75
hi fRE ctermfg=Black ctermbg=Blue guifg=#000000 guibg=#61AFEF
hi fTL ctermfg=Black ctermbg=Yellow guifg=#000000 guibg=#E5C07B
hi TabLineFill guibg=none
endif
"\
```
