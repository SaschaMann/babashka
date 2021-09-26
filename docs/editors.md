# Babashka text editor integrations

## nREPL

### Emacs: [CIDER](https://cider.mx/)

This is the one that started it all, and its [Orchard](https://metaredux.com/posts/2018/11/09/ciders-orchard-the-heart.html) ecosystem powers many of the other plug-ins.

1. Install [Emacs](https://www.gnu.org/software/emacs/download.html) for your platform.

2. Choose an existing emacs distribution ([Prelude](https://prelude.emacsredux.com/), [Spacemacs](https://www.spacemacs.org/) and [Doom Emacs](https://github.com/hlissner/doom-emacs) are all great) or add this to your `init.el`:

```elisp
(unless (package-installed-p 'cider)
  (package-install 'cider))
  
(add-hook 'clojure-mode-hook #'cider-mode)
```

Summarized from the [CIDER docs](https://docs.cider.mx/cider/platforms/babashka.html):

Start the nREPL server from the project directory:

```bash
$ bb --nrepl-server
```

Create or open any Clojure file (`.clj`) with `C-x C-f` and connect to it using `C-c C-x c j` (cider-connect-clj).

The [basic CIDER keybindings](https://docs.cider.mx/cider/usage/cider_mode.html#key-reference):

* `cider-load-buffer` - `C-c C-k`
* `cider-eval-last-sexp` - `C-x C-e` or `C-c C-e`
* `cider-eval-defun-at-point` - `C-M-x` or `C-c C-c`

### VSCode: [Calva](https://github.com/BetterThanTomorrow/calva)

1. Install [VSCode](https://code.visualstudio.com/).

2. Install [Calva](https://marketplace.visualstudio.com/items?itemName=betterthantomorrow.calva).

Currently, Calva understands your source code to be in the context of a folder, i.e. a Clojure project. A nice way to create one is with [clj-new](https://github.com/seancorfield/clj-new):

```bash
$ clj -A:new lib myorg/bbtest
Generating a project called bbtest based on the 'lib' template.
The lib template is intended for library projects, not applications.
```
3. Start the nREPL server from the project's directory:

```bash
$ cd bbtest
bbtest$ bb --nrepl-server 1667
Started nREPL server at localhost:1667
For more info visit https://github.com/borkdude/babashka/blob/master/doc/repl.md#nrepl.
```

Open the project folder in VSCode (Ctrl+K Ctrl+O) or from another terminal:

```bash
$ cd bbtest
bbtest$ code .
```

4. In VSCode, run the Calva command `Connect to a running REPL` (ctrl+alt+c ctrl+alt+c). Choose `Generic` project when prompted, and enter `localhost:1667`.

REPL commands:

* `Load Current File and Dependencies` ctrl+alt+c enter
* `Evaluate Current Form Inline` ctrl+alt+c e
* `Evaluate Current Top Level Form (defun)` ctrl+alt+c space

### IntelliJ: [Cursive](https://cursive-ide.com/)

1. Download IntelliJ [here](https://www.jetbrains.com/idea/download/index.html).
2. Install Cursive by following the [user guide](https://cursive-ide.com/userguide/) and [configure deps](https://cursive-ide.com/userguide/deps.html).
3. See [Cursive: Working with Babashka](https://cursive-ide.com/userguide/babashka.html) for more info.

## Socket REPL

### Atom: [chlorine](https://github.com/mauricioszabo/atom-chlorine)

1. Install [atom](https://flight-manual.atom.io/getting-started/sections/installing-atom/)

2. Install the `chlorine` package

3. Start socket REPL:

 ```
$ bb --socket-repl 1666
Babashka socket REPL started at localhost:1666
 ```

4. Open Atom command palette (ctrl+shift+p) and execute command `Connect Clojure Socket REPL` with `localhost` `1666`

5. Edit key bindings in keymap.cson via `Edit` menu -> `Keymap...`:

```
'atom-text-editor[data-grammar="source clojure"]':
  'ctrl-; y':       'chlorine:connect-clojure-socket-repl'
  'ctrl-; e':       'chlorine:disconnect'
  'ctrl-; k':       'chlorine:clear-console'
  'ctrl-shift-enter':       'chlorine:load-file'
  'ctrl-enter':       'chlorine:evaluate-block'
  'shift-enter':       'chlorine:evaluate-top-block'
  'ctrl-; i':       'chlorine:inspect-block'
  'ctrl-; I':       'chlorine:inspect-top-block'
  'ctrl-; s':       'chlorine:evaluate-selection'
  'ctrl-; c':       'chlorine:break-evaluation'
  'ctrl-; S':       'chlorine:source-for-var'
  'ctrl-; d':       'chlorine:doc-for-var'
  'ctrl-; x':       'chlorine:run-tests-in-ns'
  'ctrl-; t':       'chlorine:run-test-for-var'
```

### Emacs: [inf-clojure](https://github.com/clojure-emacs/inf-clojure)

For Emacs, install [inf-clojure](https://github.com/clojure-emacs/inf-clojure) by adding this to your `init.el`:

```
(unless (package-installed-p 'inf-clojure)
  (package-refresh-contents)
  (package-install 'inf-clojure))

(add-hook 'clojure-mode-hook #'inf-clojure-minor-mode)
```

Start the socket REPL as above, and in emacs execute `M-x inf-clojure-connect` (C-c M-c).

Evaluate forms with C-c C-e. For a list of all available commands in inf-clojure-mode (a.k.a. the REPL) and inf-clojure-minor-mode you can either invoke C-h f RET inf-clojure-mode and C-h f RET inf-clojure-minor-mode or simply browse their menus.

### Vim: [vim-iced](https://github.com/liquidz/vim-iced)

Download the [Neovim appimage](https://github.com/neovim/neovim/releases/download/v0.4.3/nvim.appimage) and alias it to `vim`.

* Install [vim-plug](https://github.com/junegunn/vim-plug):

```
$ curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

Create `~/.config/nvim/init.vim`:

```
call plug#begin('~/.vim/plugged')

Plug 'liuchengxu/vim-clap'
Plug 'guns/vim-sexp',    {'for': 'clojure'}
Plug 'liquidz/vim-iced', {'for': 'clojure'}

call plug#end()

" Enable vim-iced's default key mapping
" This is recommended for newbies
let g:iced_enable_default_key_mappings = v:true
```

Open Neovim and run the command `:PlugInstall`.

Add the `iced` command to the $PATH:

`$ export PATH=$PATH:~/.vim/plugged/vim-iced/bin`

* Start the socket REPL server, open source file and execute `:IcedConnectSocketRepl 1666`.

Default REPL commands (default `<Leader>` is `\`):

- `<Leader>ei` : Evaluate inner element
- `<Leader>ee` : Evaluate outer list
- `<Leader>et` : Evaluate outer top list
