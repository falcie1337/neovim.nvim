# neovim.nvim

https://github.com/kdheepak/kickstart.nvim/assets/1813121/f3ff9a2b-c31f-44df-a4fa-8a0d7b17cf7b

### Introduction

*This is a fork of [dam9000/kickstart-modular.nvim](https://github.com/dam9000/kickstart-modular.nvim), which aims to be my personal minimal neovim config.* 

kickstart-modular.nvim in of itself is:

* Small
* Documented
* Modular

That is why i chose it as my starting point, and you probably should too. I preferred giving a try to the kickstart-modular.nvim instead of kickstart.nvim because i already saw the INCREDIBLE on-going [typecraft](https://twitter.com/typecraft_dev) video guide on configuring neovim from scratch prior to it, so i didn't wanted to go through the hassle to migrate the original kickstart.nvim to a modular config since we already have that with the dam9000 fork. 

I do NOT recommend you directly fork and clone my configuration into yours for daily driving purposes (even though you can if you want to), but definitely give a look at the source code to get some inspiration and learn some thing or other with me! If you also want to give me any insights and contribute to me, feel free to open a issue on my repo! 

This README will not be exhaustely updated, however i will try to make it up-to-date when i think i've made some good changes and progressed enough on my config file. (and i will definitely change the overall look and some of the content of the README too, since it is too based off kickstart-modular.nvim as of now, which is only because i do not have the will or need to completely reformulate it *yet* xD)

### Step-by-step if you STILL want to install my config:

> **NOTE** 
> [Backup](#FAQ) your previous configuration (if any exists)

Requirements:
* Make sure to review the readmes of the plugins if you are experiencing errors. In particular:
  * [ripgrep](https://github.com/BurntSushi/ripgrep#installation) is required for multiple [telescope](https://github.com/nvim-telescope/telescope.nvim#suggested-dependencies) pickers.
* See [Windows Installation](#Windows-Installation) if you have trouble with `telescope-fzf-native`

Neovim's configurations are located under the following paths, depending on your OS:

| OS | PATH |
| :- | :--- |
| Linux | `$XDG_CONFIG_HOME/nvim`, `~/.config/nvim` |
| MacOS | `$XDG_CONFIG_HOME/nvim`, `~/.config/nvim` |
| Windows (cmd)| `%userprofile%\AppData\Local\nvim\` |
| Windows (powershell)| `$env:USERPROFILE\AppData\Local\nvim\` |

Clone kickstart.nvim:

- on Linux and Mac
```sh
git clone https://github.com/falcie1337/neovim.nvim.git "${XDG_CONFIG_HOME:-$HOME/.config}"/nvim
```

- on Windows (cmd)
```
git clone https://github.com/falcie1337/neovim.nvim.git %userprofile%\AppData\Local\nvim\
```

- on Windows (powershell)
```
git clone https://github.com/falcie1337/neovim.nvim.git $env:USERPROFILE\AppData\Local\nvim\
```


### Post Installation

Start Neovim

```sh
nvim
```

The `Lazy` plugin manager will start automatically on the first run and install the configured plugins - as can be seen in the introduction video. After the installation is complete you can press `q` to close the `Lazy` UI and **you are ready to go**! Next time you run nvim `Lazy` will no longer show up.

If you would prefer to hide this step and run the plugin sync from the command line, you can use:

```sh
nvim --headless "+Lazy! sync" +qa
```

### Getting Started

You can see [Effective Neovim: Instant IDE](https://youtu.be/stqUbv-5u2s), covering the previous version. Note: The install via init.lua is outdated, please follow the install instructions in this file instead. They say an updated video is coming soon, however it hasn't come out yet.

I also recommend watching the [typecraft's playlist](https://www.youtube.com/playlist?list=PLsz00TDipIffreIaUNk64KxTIkQaGguqn) on configuring neovim from scratch, it's nice, simple and straight to the point, it will give you some basic knowledge of how things should work when working on your neovim configuration, and he covers from getting a custom colorscheme all the way to implementing a debugger. I definitely think it's worth checking it out.

If you want to get even deeper into it, i suggest giving a look into The Primeagen's [secondary channel](https://www.youtube.com/@TheVimeagen), at the moment i'm writing this, he posted some long (40 min+) vods configuring neovim from scratch with the lazy.nvim package manager, and he also just talks a lot about Vim and Neovim, so why not checking the man out? ;) 

### Recommended Steps

[Fork](https://docs.github.com/en/get-started/quickstart/fork-a-repo) kickstart-modular.nvim (so that you have your own copy that you can modify) and then installing you can install to your machine using the methods above.

> **NOTE**  
> Your fork's url will be something like this: `https://github.com/<your_github_username>/kickstart-modular.nvim.git`

### Configuration And Extension

* Inside of your copy, feel free to modify any file you like! It's your copy!
* Feel free to change any of the default options in `init.lua` to better suit your needs.
* For adding plugins, there are 3 primary options:
  * Add new configuration in `lua/custom/plugins/*` files, which will be auto sourced using `lazy.nvim` (uncomment the line importing the `custom/plugins` directory in the `lua/lazy-plugins.lua` file to enable this)
  * Modify `init.lua` with additional plugins.
  * Include the `lua/kickstart/plugins/*` files in your configuration.

You can also merge updates/changes from the kickstart repo back into your fork, to keep up-to-date with any changes for the default configuration.

#### Example: Adding an autopairs plugin

In the file: `lua/custom/plugins/autopairs.lua`, add:

```lua
-- File: lua/custom/plugins/autopairs.lua

return {
  "windwp/nvim-autopairs",
  -- Optional dependency
  dependencies = { 'hrsh7th/nvim-cmp' },
  config = function()
    require("nvim-autopairs").setup {}
    -- If you want to automatically add `(` after selecting a function or method
    local cmp_autopairs = require('nvim-autopairs.completion.cmp')
    local cmp = require('cmp')
    cmp.event:on(
      'confirm_done',
      cmp_autopairs.on_confirm_done()
    )
  end,
}
```


This will automatically install [windwp/nvim-autopairs](https://github.com/windwp/nvim-autopairs) and enable it on startup. For more information, see documentation for [lazy.nvim](https://github.com/folke/lazy.nvim).

#### Example: Adding a file tree plugin

In the file: `lua/custom/plugins/filetree.lua`, add:

```lua
-- Unless you are still migrating, remove the deprecated commands from v1.x
vim.cmd([[ let g:neo_tree_remove_legacy_commands = 1 ]])

return {
  "nvim-neo-tree/neo-tree.nvim",
  version = "*",
  dependencies = {
    "nvim-lua/plenary.nvim",
    "nvim-tree/nvim-web-devicons", -- not strictly required, but recommended
    "MunifTanjim/nui.nvim",
  },
  config = function ()
    require('neo-tree').setup {}
  end,
}
```

This will install the tree plugin and add the command `:Neotree` for you. You can explore the documentation at [neo-tree.nvim](https://github.com/nvim-neo-tree/neo-tree.nvim) for more information.

### Contribution

Issues are welcome. The goal of this repo is not to be a Neovim distro that tries to replace kickstart.nvim, but to offer some examples on how you can try to structure your neovim configuration and give inspiration for people who are still conflicted on how their editor should look like. My config also aims to be minimal, but functional, so keep that in mind. Some things to take into account: 

* I can't warrant any help with exclusively personal issues you encounter with the config while you use it. If you want to drastically change it, you should do it yourself (or just use the kickstart/kickstart-modular.nvim template as a starting point), i myself am a beginner at programming and i don't know much about Lua yet, so if you ask for ask to me, i probably won't know jackshit, and it's even worse if you are trying to do any big changes to it, so keep that in mind.
* Please refrain for treating this repository as THE exceptional config example (i know it's hard to think of it like that, but it's always good to remind people), always take everything here with a grain of salt even if you were to implement something i did to your configuration if you are a beginner like i am. If you do something you saw here, this doesn't mean i can help you out if you get to any problems, you can try to reach out here if you want, but i don't consider myself as capacitated enough to troubleshoot someone elses' files. You do your things at your own risk.

I will accept any issue, but this is a small-scope personal project, so it doesn't mean i will try to get every issue closed and resolved asap. Even more so if the issue you open is related to one of the above cited. However, i can try to tackle into some of them in my spare time. The best kind of issues someone can open on my repo are simple suggestions that improve the usabilty of the config and doesn't add many lines to the codebase and aren't too hard to implement. 

### FAQ (This FAQ is related to Kickstart, i will update it some time later to relate to my personal fork)

* What should I do if I already have a pre-existing neovim configuration?
  * You should back it up, then delete all files associated with it.
  * This includes your existing init.lua and the neovim files in `~/.local` which can be deleted with `rm -rf ~/.local/share/nvim/`
  * You may also want to look at the [migration guide for lazy.nvim](https://github.com/folke/lazy.nvim#-migration-guide)
* Can I keep my existing configuration in parallel to kickstart?
  * Yes! You can use [NVIM_APPNAME](https://neovim.io/doc/user/starting.html#%24NVIM_APPNAME)`=nvim-NAME` to maintain multiple configurations. For example you can install the kickstart configuration in `~/.config/nvim-kickstart` and create an alias:
    ```
    alias nvim-kickstart='NVIM_APPNAME="nvim-kickstart" nvim'
    ```
    When you run Neovim using `nvim-kickstart` alias it will use the alternative config directory and the matching local directory `~/.local/share/nvim-kickstart`. You can apply this approach to any Neovim distribution that you would like to try out.
* What if I want to "uninstall" this configuration:
  * See [lazy.nvim uninstall](https://github.com/folke/lazy.nvim#-uninstalling) information
* Why is the kickstart `init.lua` a single file? Wouldn't it make sense to split it into multiple files?
  * The main purpose of kickstart is to serve as a teaching tool and a reference
    configuration that someone can easily `git clone` as a basis for their own.
    As you progress in learning Neovim and Lua, you might consider splitting `init.lua`
    into smaller parts. *This is the fork of the original project that splits the configuration into smaller parts.*
    The original repo that maintains the exact
    same functionality in a single `init.lua` file is available here:
    * [kickstart.nvim](https://github.com/dam9000/kickstart-modular.nvim)
  * Discussions on this topic can be found here:
    * [Restructure the configuration](https://github.com/nvim-lua/kickstart.nvim/issues/218)
    * [Reorganize init.lua into a multi-file setup](https://github.com/nvim-lua/kickstart.nvim/pull/473)

### Windows Installation

Installation may require installing build tools, and updating the run command for `telescope-fzf-native`

See `telescope-fzf-native` documentation for [more details](https://github.com/nvim-telescope/telescope-fzf-native.nvim#installation)

This requires:

- Install CMake, and the Microsoft C++ Build Tools on Windows

```lua
{'nvim-telescope/telescope-fzf-native.nvim', build = 'cmake -S. -Bbuild -DCMAKE_BUILD_TYPE=Release && cmake --build build --config Release && cmake --install build --prefix build' }
```

