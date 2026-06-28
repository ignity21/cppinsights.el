# cppinsights.el

[![EMACS](https://img.shields.io/badge/Emacs-28.1-922793?logo=gnu-emacs&logoColor=b39ddb&.svg)](https://www.gnu.org/savannah-checkouts/gnu/emacs/emacs.html)
![GitHub License](https://img.shields.io/github/license/chrischen3121/cppinsights.el)
[![MELPA](https://melpa.org/packages/cppinsights-badge.svg)](https://melpa.org/#/cppinsights)


An Emacs package that integrates with [C++ Insights](https://cppinsights.io/), a tool that transforms C++ source code into its expanded form, revealing the details the compiler sees after applying language features like templates, operator overloading, and lambda functions.

## Description

This package allows you to run C++ Insights directly from within Emacs, displaying the transformed code in a separate buffer. It helps C++ developers understand how the compiler interprets their code, making it easier to debug complex C++ features and learn about the language's inner workings.

## Installation

### Prerequisites

Before using this package, you need to install the C++ Insights command-line tool:

#### Windows
1. Install [WSL](https://docs.microsoft.com/en-us/windows/wsl/install) (Windows Subsystem for Linux)
2. Follow the Ubuntu instructions below within WSL

#### macOS
Using Homebrew:
```bash
brew install cmake llvm
git clone https://github.com/andreasfertig/cppinsights.git
cd cppinsights
mkdir build && cd build
cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=ON -DCMAKE_CXX_COMPILER=clang++ ..
make
sudo make install
```

#### Ubuntu/Debian
```bash
sudo apt-get install cmake clang libclang-dev llvm
git clone https://github.com/andreasfertig/cppinsights.git
cd cppinsights
mkdir build && cd build
cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=ON ..
make
sudo make install
```

#### Arch Linux
Using AUR:
```bash
pamac install cppinsights
```

### Package Installation

#### With Straight
``` elisp
(use-package cppinsights
  :straight (:host github :repo "chrischen3121/cppinsights.el")
  :commands cppinsights-run
  :custom
  ;; Customize variables as needed
  (cppinsights-program "insights")  ;; Path to the insights binary
  (cppinsights-clang-opts '("-std=c++17"))  ;; Additional arguments to pass to internal clang
  :bind
  ;; Add keybinding for cppinsights-run
  (:map c++-mode-map
        ("C-c c i" . cppinsights-run)))

```

#### With Doom Emacs
In `packages.el`:
``` elisp
(package! cppinsights
  :recipe (:host github :repo "chrischen3121/cppinsights.el"))
```

In `config.el`:
``` elisp
(use-package! cppinsights
  :commands cppinsights-run
  :custom
  (cppinsights-program "insights")  ;; Path to the insights binary
  (cppinsights-clang-opts '("-O0" "-std=c++17"))  ;; Additional arguments to pass to internal clang
  :init
  ;; Add keybinding for cppinsights-run
  (map! :map c++-mode-map
        :desc "Run C++ Insights" "C-c i" #'cppinsights-run))
```

#### With `package-vc-install` (Emacs 30+ built-in)
``` elisp
(package-vc-install '(cppinsights :url "https://github.com/chrischen3121/cppinsights.el"))
```

## Usage

1. Open a C++ file in Emacs
2. Run `M-x cppinsights-run` to process the current file
3. A new buffer will open showing the transformed code

You can customize the package by:
- `M-x customize-group RET cppinsights RET`

## Key Bindings

You can add a key binding to your Emacs configuration:

```elisp
(keymap-set c++-mode-map "C-c c i" #'cppinsights-run)
```
