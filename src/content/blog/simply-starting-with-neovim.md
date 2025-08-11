---
title: Simple neovim starter setup (using kickstart.nvim)
description: I explain how I'd use kickstart.nvim to quickly bootstrap an IMO minimal usable neovim setup.
publishDate: 2025-08-04
tags:
  - neovim
draft: false
language: English
comment: false
heroImage:
  src: "./post1-banner.png"
---
There have been many good write ups talking up *vim* and *neovim* that go in-depth on why and how using vim increases one's productivity and makes text editing enjoyable (e.g. a great one [here](https://www.ssp.sh/blog/why-using-neovim-data-engineer-and-writer-2023/) ). Instead of repeating that, here I'll give a quick overview on how I would set up an (almost) minimal and easy-to-use **neovim** environment from scratch. 

I can't talk about minimal neovim setups without mentioning [LazyVim](https://www.lazyvim.org/). If you search online for tips on starting with neovim, you will likely get it recommended as an easy and a minimal starting point, which is exactly what happened to me as well. However, even though the plugins LazyVim's setup comes with are considered *essential* and the setup *minimal* by many veteran neovim users, I was not happy to start with a bunch of tools that I don't know anything about, and generally prefer an even more minimal setup that I can slowly extend with specific plugins that I need and understand.

(Note: LazyVim is also a package manager that is actually quite easy to use, and we'll still be using that, just not LazyVim's suggested starting setup)

So as an alternative to LazyVim on one end, and starting fully from scratch on another, I found [kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim), a project whose idea is to provide quite a minimal setup that can be a starting point for one's personal config. I like the fact that it comes with less plugins than many alternatives, and is easier to pick up and thus slowly understand how plugin management works. It is also superbly commented, comes with a tutorial that you can follow inside neovim (try `:Tutor`) and is all-in-all a joy to use.


## Installation

Here's how to quickly set it up (assuming you are on a Debian based OS, otherwise finding your OS alternative shouldn't be hard).

Install the latest stable neovim from snap:
```
sudo snap install neovim --classic
```

Add an alias to your `.bashrc` for `vim` to open Neovim:
```
alias vim='nvimalias vim='nvim''
```


### Install external dependencies


```
# ripgrep - line-oriented search tool that recursively searches the current directory for a regex pattern - https://github.com/BurntSushi/ripgrep#installation
sudo apt-get install ripgrep

## fd - tool to find entries in your filesystem https://github.com/sharkdp/fd#installation
sudo apt-get install fd-find

## xclip - command line clipboard tool (xsel/win32yank should also do the job on other platforms)
sudo apt-get install xclip
```

Optionally, install a [NerdFont](https://www.nerdfonts.com/font-downloads) on your system. Not necessary, but things tend to look nicer with it. There are many ooptions to choose from, I for example like the `Hack Nerd Font`. After you download a zip for this font, install it on your Debian-like OS like this:
```
mkdir -p ~/.fonts && unzip -o ~/Downloads/Hack.zip -d ~/.fonts && fc-cache -fv
```

## Setup `kickstart.nvim`

```
# Remove (or backup if you want) old nvim config
rm -rf ~/.config/nvim

# Clone `kickstart.nvim`
git clone https://github.com/nvim-lua/kickstart.nvim.git  ~/.config/nvim

# Remove .git folder from the cloned started
rm -rf ~/.config/nvim/.git && rm -rf ~/.config/nvim/.github

# Start nvim once so it downloads and setups the plugins from the config
nvim # (or just `vim` if you set up an alias)
```

And that's it. You can already start using neovim with a `vim` alias, without caring too much about the more *interactive* plugins. In case you want to start using neovim as more than just a text editor, I recommend going through the config file (under `~/.config/nvim/init.lua`), which should tell you how to use the plugins and which plugins you now have installed.

### A few (imo) essential edits (~/.config/nvim/init.lua)

1. Install [`leap.nvim`](https://github.com/ggandor/leap.nvim)

To me, this plugin alone is worth switching over from vim to neovim and is absolutely essential.
It basically lets you jump with around 3 keystrokes to any location you currently see on the screen.

```
# Add this to your `init.lua` after other plugins (inside the `require('lazy').setup({` block
{
    'ggandor/leap.nvim',
    enabled = true,
    keys = {
      { 's', mode = { 'n', 'x', 'o' }, desc = 'Leap Forward to' },
      { 'S', mode = { 'n', 'x', 'o' }, desc = 'Leap Backward to' },
      { 'gs', mode = { 'n', 'x', 'o' }, desc = 'Leap from Windows' },
    },
    config = function(_, opts)
      local leap = require 'leap'
      for k, v in pairs(opts) do
        leap.opts[k] = v
      end
      leap.add_default_mappings(true)
      vim.keymap.del({ 'x', 'o' }, 'x')
      vim.keymap.del({ 'x', 'o' }, 'X')
    end,
  }
```
2. Enable nerd font, if installed earlier:
```
# Change the existing line 
vim.g.have_nerd_font = true
```
3. Enable relative line numbers (to help with jumping):
```
# Uncomment line
vim.o.relativenumber = true
```
4. Enable showing the percentage of file before the current location of the cursor
```
# Find this section in the `init.lua`:

statusline.section_location = function()
        return '%2l:%-2v'
      end

# Modify the return line to be

return '%2l:%-2v %p%%'
```

## Setup details

Your leader key is `<space>` (used to interact with plugins).

Your plugin manager is [lazy.nvim](https://github.com/folke/lazy.nvim).

#### Installed plugins list

Here I'll list most of the plugins that you now have installed, with some quick info on what they do. I might write a separate post going over some more interesting ones and explaining how I use them. I'll update this page with links to those if and when it happens.

I also rank them by usefulness to me, so if you'd like to slowly start using more that **neovim** has to offer, starting with the ones from the top of the list could be a good idea.

1. [telescope.nvim](https://github.com/nvim-telescope/telescope.nvim) - fuzzy finder, helps you find files on your file system, words in your file, grep contents
2. [which-key.nvim](https://github.com/folke/which-key.nvim)  - helps you remember Neovim keymaps by showing available keybindings in a popup as you type
3. [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter) - helps highlight edit and navigate code
4. [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig) - default setup for further LSP setups
5. [lazydev.nvim] - LSP (language server) for lua
6.  [conform.nvim](https://github.com/stevearc/conform.nvim) - lightweight formatter
7. [blink.cmp] - autocomplete tool using LSP
8. [todo-comments.nvim](https://github.com/folke/todo-comments.nvim) -highlight and search for TODO comments
9. [mini.nvim](https://github.com/echasnovski/mini.nvim) a collection of over 40(!) small plugins that make your experience nicer
10. [gitsigns.nvim](https://github.com/lewis6991/gitsigns.nvim) - adds git related signs to the bottom of the window
l
