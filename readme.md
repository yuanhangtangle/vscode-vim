# Building a Vscode-Vim Hybrid Editor

This is a testing message from vscode github integration

> Disclaimer 1: I am not sure whether it is generally a good idea to use a 'hybrid' editor, 
but recently I am happy with it. 

> Disclaimer 2: This article does NOT aim to provide a thorough tutorial for vscode or vim,
but to introduce my effort of building a Vscode-Vim hybrid editor.

> Disclaimer 3: Please refer to LLMs (like GPT-4 or Claude-3.5) if I do not make things clear.

> Disclaimer 4: I use MacOS, but the settings should also work in Ubuntu or Windows with minor modifications.

In this article, I will explain why I migrated from vscode to vim and then back to vscode.
I will explain the pros & cons and show how to build a hybrid editor by combining the pros of both editors.

If you are not interested in my story with vscode and vim, refer to [Configure Vim Extension](#configure-vim-extension) and [Configure File Explorer Keybindings](#configure-file-explorer-keybindings)

## Table of Contents

- [My Story with Vscode and Vim](#my-story-with-vscode-and-vim)
    - [From Vscode to Vim](#from-vscode-to-vim)
    - [From Vim to Vscode](#from-vim-to-vscode)
- [For Vim Beginners](#for-vim-beginners)
- [Configure Vim Extension](#configure-vim-extension)
    - [Install Necessary Extensions](#install-necessary-extensions)
    - [Setup Vim Keybindings](#setup-vim-keybindings)
    - [Different Groups of Keybindings](#different-groups-of-keybindings)
- [Configure File Explorer Keybindings](#configure-file-explorer-keybindings)
- [Vim Keybindings in `vscodevim-settings.json`](#vim-keybindings-in-vscodevim-settingsjson)
- [File Explorer Keybindings in `explorer-keybindings.json`](#file-explorer-keybindings-in-explorer-keybindingsjson)

## My Story with Vscode and Vim

### From Vscode to Vim

I guess most programmers and learners use vscode as their first code editor,
and probably, vscode is the most popular code editor.
I used vscode for around 2 years before migrating to vim.
Vscode is a GUI-based editor with tons of powerful plugins 
which help you with almost everything in programming.

I was pretty satisfied with vscode until one of my friends reminded me to try vim,
a 'weird' editor which uses hjkl to move the cursor and discards the mouse completely.
It was painful at the beginning when I tried to use vim to do my job,
but after getting through that period,
everything made sense.
It saves time to use hjkl and to keep all the fingers in the area of A-Z.
Vim is also equipped with powerful plugins 
and can do the same amount of work as vscode by controlling the editor fully with a keyboard.

The most valuable lesson vim teaches me is that 
shortcuts save time and doing (almost) all things with shortcuts saves a lot of time.
Well, you may not agree with this if you work with GUI-based items, 
but I would say it is generally true if you work with only text files like a programmer.
I used to use the mouse to do almost everything in vscode.
Open the explorer by clicking the sidebar,
find the file by clicking the folder,
and run the code by clicking the 'run' button.
You don't need to learn a *How To Use Mouse* lesson and you can do many things easily with a mouse by clicking something.
But it's slow.
And it is generally faster to hit one or two keys 
if the key combination is mapped to the desired functionality.
For example, if you map F5 to *Run Active File*,
then you can run a file almost immediately each time you want to run it.

I was so satisfied with vim that I discarded vscode completely.
I used tmux to manipulate terminal windows,
vim to code and shell scripts to run codes.
Altogether, they worked pretty well.

But then, I went back to vscode.

### From Vim to Vscode

Why did I go back to vscode although I was satisfied with neovim's full-keyboard control?
Essentially, it is because of remote development and graphic support.
Vim is a great editor but it lacks native remote development or image rendering support,
and I failed to find plugins that do these things for me.
Remote development is common for GPU users and charts are necessary for data analysis.
If you only work on the same server all the time,
it is straightforward to install neovim and all the plugins on the server
and you can enjoy the same shortcuts as that of your local machine.
But I need to switch between servers depending on the GPU availability.
Of course, I can install neovim on every server I work on if I am extremely patient.
However, neovim depends on specific versions of some fundamental software (e.g. glibc),
but I can't upgrade or downgrade these software versions freely because they also affect other users.
It is even impossible to install a new enough version of neovim if the dependencies are old versions.
Besides the annoying remote development experience, 
viewing images created by `matplotlib` is also troublesome in a terminal shell.
I need to open `finder` in my MacOS, find the image, view it and then close all these software before going back to neovim.
The use of the cursor is almost inevitable to view an image.
This is even more cumbersome if I work on a server via SSH.
I need to download the image first and then view it locally.
Compared to neovim, vscode makes remote development easy and elegant.
You can even work inside a docker on the server and the coding experience remains the same.
Although I was a vimer, 
I did agree that vscode is a better remote editor.

So I started to think of migrating back to vscode,
but it was not easy to give up the `hjkl`.
As an old saying (maybe 2-seconds-old), 'Vim: never or forever'.

Things became different after I discovered the full power of the Vim extension.
And combining the power of vscode and vim is the topic of the remaining parts of this article.

## For Vim Beginners

This article is about combining vscode and vim.
I won't include a vim tutorial in this article which is not the purpose of this article.
There have been lots of resources online and for Chinese readers,
I recommend Chenhao's vim tutorial in CoolShell:
[简明 Vim 练级攻略](https://coolshell.cn/articles/5426.html)

## Configure Vim Extension

We mainly rely on the VscodeVim extension to configure a vscode-vim hybrid editor
which combines the advantages of both vscode and vim.
Here are the steps:

1. Install necessary extensions
2. Setup keybindings and settings

### Install Necessary Extensions

We rely on the following extensions:

1. **Vim**, by vscodevim. It is the very magic that bridges vscode and vim.

<img src="images/vim.png" alt="Example Image" width="400">

2. **Vscode text objects**, by Rodrigo Scola. It provides treesitter-like text objects.

<img src="images/vscode-text-objects.png" alt="Example Image" width="400">

3. **Open file**, by Frank Stuetzer. It mimics the behavior of `gf` in vim.

<img src="images/open-file.png" alt="Example Image" width="400">

4. **fuzzy-search**, by jacobdufault. It is similar to `current_buffer_fuzzy_find` of `telescope.nvim`.

<img src="./images/fuzzy-search.png" alt="Example Image" width="400">

Other extensions are also helpful, but for the purpose of this article, 
the above ones are sufficient.

You may find many keys disabled or mapped to 'weird' behaviors after installing the Vim extension.
Don't worry. 
It means that the Vim extension is successfully installed,
and the keys are taken over by Vim.

For vim beginners, see [For Vim Beginners](#for-vim-beginners).

### Setup Vim Keybindings

Along with this file,
you should find `vscodevim-settings.json` in the same repository,
which lists my configuration of the vim extension.
Most of the settings are vim keybindings 
which are similar to that of vscode keybindings but only work within text areas.
You can append these settings to your `settings.json` 
and save it to make it work immediately.
The default keybinding to open `preferences` is `cmd+,`.
By default, it shows the GUI 
and you can switch between the GUI and the json file (the `settings.json` file) 
by clicking the <img src="images/convert-icon.png" alt="Example Image" style="width:25px; height:auto; vertical-align:middle;"> icon.

> It should be emphasized that the vim keybindings only work in the 'text areas' 
where `textEditorFocus` is true (ask LLMs if you are not familiar with this condition) 
and when vim extensions are active.
This means that when the cursor focuses on the sidebar (`sidebarFocus`) or somewhere other than the text editor,
these keybindings won't work.
This is different from vim where keybindings listed in `init.lua` or `init.vim` are global.
But fortunately,
we focus on the text editor most of the time,
so these keybindings usually help.

### Different Groups of Keybindings

Now, there are 3 groups of keybindings in your vscode:

1. Keybindings of vscode, defined in `keybindings.json`. The default keybinding to open this file is `cmd+k cmd+s` (holding the `cmd` key, press `k` and then `s`)
2. Default keybindings provided by Vim extension, which keep aligned with that of vim. For example, use `hjkl` to move the cursor and `i` to trigger the `insert` mode.
3. User-defined keybindings of Vim extension. Vim extension allows mapping arbitrary key combinations to vim shortcuts or vscode commands. 

For me, the user-defined keybindings help a lot.
It allows using `space` or `g` as the leader key in the normal or visual mode,
which is much more convenient than commonly used `cmd` or `ctrl`.
The keybindings are listed in `vim.normalModeKeyBindingsNonRecursive` and `vim.visualModeKeyBindingsNonRecursive` in the `settings.json`.

I like using `space` as the `<leader>`, so many of my keybindings start with `space`.
Feel free to modify the keybindings in `settings.json` to fit your own needs.

I will append a list of all vim keybindings defined in `settings.json`,
which is generated by GPT-4 and manually checked.

## Configure File Explorer Keybindings

Vscode provides an integrated file explorer，
which does what `nvim-tree` did for me.
But I like to use `hjkl` to navigate the file tree,
so I add some keybindings that work when `filesExplorerFocus` is true.
You can find these keybindings in `./explorer-keybindings.json` and append them in your `keybindings.json` file.
The default keybinding to open `keybindings.json` is `cmd+k cmd+s`
(holding `cmd`, press `k` and then `s`).
By default, it shows the GUI 
and you can switch between the GUI and the json file (the `settings.json` file) 
by clicking the <img src="images/convert-icon.png" alt="Example Image" style="width:25px; height:auto; vertical-align:middle;"> icon.

## Vim Keybindings in `vscodevim-settings.json`

Here are (possibly) all the keybindings defined in the `vscodevim-settings.json`.

| Keybindings | Normal | Visual | Functionality |
|-------------|--------|--------|---------------|
| `,` | Yes | No | Fuzzy search active text editor |
| `f` | Yes | Yes | Easy motion search |
| `<leader>g` | Yes | No | Global search |
| `<leader>h` | Yes | No | Go to symbol in the active text editor |
| `<leader>t` | Yes | No | Go to symbol in the project |
| `<leader>f` | Yes | No | Quick open |
| `<leader>rn` | Yes | No | Rename symbols |
| `gr` | Yes | No | Go to references |
| `gd` | Yes | No | Go to definition |
| `[g` | Yes | No | Go to previous problem |
| `]g` | Yes | No | Go to next problem |
| `<leader>ci` | Yes | No | Toggle comment line |
| `<leader>nn` | Yes | No | Toggle sidebar visibility |
| `<leader>nf` | Yes | No | Show active file in explorer |
| `<leader>l` | Yes | No | Open last edited editor |
| `<leader>0` | Yes | No | Open last editor in group |
| `<leader>1` | Yes | No | Open editor at index 1 |
| `<leader>2` | Yes | No | Open editor at index 2 |
| `<leader>3` | Yes | No | Open editor at index 3 |
| `<leader>4` | Yes | No | Open editor at index 4 |
| `\]` | Yes | No | Next editor |
| `\[ ` | Yes | No | Previous editor |
| `<leader>w` | Yes | No | Focus next group, convenient for left hand |
| `<leader>o` | Yes | No | Focus next group, convenient for right hand |
| `U` | Yes | No | Redo |
| `C-j` | Yes | No | Cursor page down |
| `C-k` | Yes | No | Cursor page up |
| `<leader>j` | Yes | Yes | Cursor page down |
| `<leader>k` | Yes | Yes | Cursor page up |
| `-` | Yes | Yes | Move to end of the last word in this line |
| `0` | Yes | Yes | Move to beginning of the first word in this line |
| `^` | Yes | Yes | Move to beginning of line |
| `<leader><leader>` | Yes | No | Unfold editor |
| `<leader>i` | Yes | No | Toggle fold |
| `<leader>m` | Yes | No | Fold all |
| `<leader>e` | Yes | No | Unfold all |
| `daf` | Yes | No | Delete next outer function |
| `vaf` | No | Yes | Select next outer function |
| `vif` | No | Yes | Select next inner function |
| `vac` | No | Yes | Select next outer class |
| `vic` | No | Yes | Select next inner class |
| `]f` | Yes | No | Go to next outer function |
| `[f` | Yes | No | Go to previous outer function |
| `]c` | Yes | No | Go to next outer class |
| `[c` | Yes | No | Go to previous outer class |

## File Explorer Keybindings in `explorer-keybindings.json`

| Keybindings | Functionality |
|-------------|---------------|
| `y` | Copy file |
| `r` | Rename file |
| `a` | Create new file |
| `c` | Copy file |
| `p` | Paste file |
| `shift+y` | Copy relative file path |
| `j` | Move focus down |
| `k` | Move focus up |
| `backspace` | Collapse folder |
| `/` | Find in list |
| `x` | Cut file |
| `tab` | Open file and preserve focus |
| `o` | Open file and pass focus |
| `d` | Delete file |