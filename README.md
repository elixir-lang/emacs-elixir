[![License GPL 3][badge-license]](http://www.gnu.org/licenses/gpl-3.0.txt)
[![Build Status](https://travis-ci.org/elixir-editors/emacs-elixir.svg?branch=master)](https://travis-ci.org/elixir-editors/emacs-elixir)
[![MELPA Stable](http://stable.melpa.org/packages/elixir-mode-badge.svg)](http://stable.melpa.org/#/elixir-mode)
[![MELPA](http://melpa.org/packages/elixir-mode-badge.svg)](http://melpa.org/#/elixir-mode)

# Elixir Mode

Provides font-locking, indentation and navigation support for the
[Elixir programming language.](http://elixir-lang.org/)

- [Installation](#installation)
  - [Via package.el](#via-packageel)
  - [Via el-get](#via-el-get)
  - [Manual](#manual)
- [Usage](#usage)
  - [Interactive Commands](#interactive-commands)
  - [Configuration](#configuration)
  - [Keymapping](#keymapping)
- [Notes](#notes)
- [Elixir Tooling Integration](#elixir-tooling-integration)
- [Elixir Format](#elixir-format)
- [History](#history)
- [Contributing](#contributing)
- [License](#license)

## Installation

### Via package.el

`package.el` is the built-in package manager in Emacs.

`elixir-mode` is available on the two major community maintained repositories -
[MELPA STABLE](melpa-stable.milkbox.net) and [MELPA](http://melpa.milkbox.net).

You can install `elixir-mode` with the following command:

<kbd>M-x package-install [RET] elixir-mode [RET]</kbd>

or by adding this bit of Emacs Lisp code to your Emacs initialization file
(`.emacs` or `init.el`):

```el
(unless (package-installed-p 'elixir-mode)
  (package-install 'elixir-mode))
```

If the installation doesn't work try refreshing the package list:

<kbd>M-x package-refresh-contents [RET]</kbd>

Keep in mind that MELPA packages are built automatically from
the `master` branch, meaning bugs might creep in there from time to
time. Never-the-less, installing from MELPA is the recommended way of
obtaining `Elixir-Mode`, as the `master` branch is normally quite stable and
"stable" (tagged) builds are released somewhat infrequently.

With the most recent builds of Emacs, you can pin `Elixir-Mode` to always
use MELPA Stable by adding this to your Emacs initialization:

```el
(add-to-list 'package-pinned-packages '(elixir-mode . "melpa-stable") t)
```

### Via el-get

[el-get](https://github.com/dimitri/el-get) is another popular package manager for Emacs. If you're an el-get
user just do <kbd>M-x el-get-install [RET] elixir-mode [RET]</kbd>.

### Manual

You can install `Elixir-Mode` manually by placing `Elixir-Mode` on your `load-path` and
`require` ing it. Many people favour the folder `~/.emacs.d/vendor`.

```el
(add-to-list 'load-path "~/.emacs.d/vendor")
(require 'elixir-mode)
```

## Usage

### Interactive Commands

<table>
    <tr>
        <th>Command (For the <code>M-x</code> prompt.)</th>
        <th>Description</th>
    </tr>
    <tr>
        <td><code>elixir-mode</code></td>
        <td>Switches to elixir-mode.</td>
    </tr>
    <tr>
        <td><code>elixir-mode-open-github</code></td>
        <td>Open the GitHub page for Elixir.</td>
    </tr>
    </tr>
    <tr>
        <td><code>elixir-mode-open-elixir-home</code></td>
        <td>Go to Elixir README in the browser.</td>
    </tr>
    <tr>
        <td><code>elixir-mode-open-docs-master</code></td>
        <td>Open the Elixir documentation for the master.</td>
    </tr>
    <tr>
        <td><code>elixir-mode-open-docs-stable</code></td>
        <td>Open the Elixir documentation for the latest stable release.</td>
    </tr>
    <tr>
        <td><code>elixir-mode-show-version</code></td>
        <td>Print version info for elixir-mode.</td>
    </tr>
</table>

### Configuration

Any file that matches the glob `*.ex[s]` or `*.elixir` is
automatically opened in elixir-mode, but you can change this
functionality easily.

```lisp
;; Highlights *.elixir2 as well
(add-to-list 'auto-mode-alist '("\\.elixir2\\'" . elixir-mode))
```

### Keymapping

Keymaps can be added to the `elixir-mode-map` variable.

### Pairing

[Smartparens](https://github.com/Fuco1/smartparens) has direct support for Elixir.

Alternatively, if you want to use `ruby-end-mode`, you can add the following to your `elixir-mode-hook`:

```lisp
(add-to-list 'elixir-mode-hook
             (defun auto-activate-ruby-end-mode-for-elixir-mode ()
               (set (make-variable-buffer-local 'ruby-end-expand-keywords-before-re)
                    "\\(?:^\\|\\s-+\\)\\(?:do\\)")
               (set (make-variable-buffer-local 'ruby-end-check-statement-modifiers) nil)
               (ruby-end-mode +1)))
```

## Elixir Tooling Integration

If you looking for elixir tooling integration for Emacs, check: [alchemist.el](https://github.com/tonini/alchemist.el)

You can use [web-mode.el](http://web-mode.org) to edit elixir templates (eex files).


## Elixir Format

### Setup of elixir-format
Customize the elixir and mix paths

In Emacs, run following command to customize option
``` elisp
M-x customize-option

Customize-variable: elixir-format-elixir-path
```
and set your elixir executable path there. After that run:
``` elisp
M-x customize-option

Customize-variable: elixir-format-mix-path
```
and set your mix executable path there.

Your machine's elixir and mix executable paths can be found with `which` command as shown below

``` shell
$ which elixir
/usr/local/bin/elixir

$ which mix
/usr/local/bin/mix
```
Alternavively you can define variables as below

``` elisp
(setq elixir-format-elixir-path "/usr/local/bin/elixir")
(setq elixir-format-mix-path "/usr/local/bin/mix")
```

### Use it

``` elisp
M-x elixir-format
```

### Add elixir-mode hook to run elixir format on file save

``` elisp
;; Create a buffer-local hook to run elixir-format on save, only when we enable elixir-mode.
(add-hook 'elixir-mode-hook
          (lambda () (add-hook 'before-save-hook 'elixir-format nil t)))
```

To use a `.formatter.exs` you can either set `elixir-format-arguments` globally to a path like this:

``` elisp
(setq elixir-format-arguments (list "--dot-formatter" "/path/to/.formatter.exs"))
```

or you set `elixir-format-arguments` in a hook like this:

``` elisp
(add-hook 'elixir-format-hook (lambda ()
                                 (if (projectile-project-p)
                                     (setq elixir-format-arguments (list "--dot-formatter" (concat (projectile-project-root) "/.formatter.exs")))
                                   (setq elixir-format-arguments nil))))
```

In this example we use [Projectile](https://github.com/bbatsov/projectile) to get the project root and set `elixir-format-arguments` accordingly.


## History

This mode is based on the
[Emacs mode by secondplanet](https://github.com/secondplanet/elixir-mode).

## Contributing

Please read [CONTRIBUTING.md](https://github.com/elixir-lang/emacs-elixir/blob/master/CONTRIBUTING.md) for guidelines on how to contribute to this project.

## License

Copyright © 2011-2017 Samuel Tonini, Matt DeBoard, Andreas Fuchs, secondplanet and
[contributors](https://github.com/elixir-lang/emacs-elixir/contributors).

Distributed under the GNU General Public License, version 3

[badge-license]: https://img.shields.io/badge/license-GPL_3-green.svg
