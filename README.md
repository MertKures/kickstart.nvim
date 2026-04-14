# kickstart.nvim

A starting point for Neovim that is:

* Small
* Single-file
* Completely Documented

**NOT** a Neovim distribution, but instead a starting point for your configuration.

This fork adds the following on top of the original kickstart.nvim:
- **Python LSP** via `pyright` (autocompletion, type checking, go-to-definition)
- **Python formatter** via `ruff` (`<leader>f`)
- **C/C++ LSP** via `clangd`
- **C/C++ formatter** via `clang-format`
- **Arduino language server**
- **Markdown support** via `markdown.nvim`
- **Fold settings** (indent-based, all folds open by default)
- **`<A-z>`** — toggle line wrap
- **`<leader>e`** — open diagnostic float
- **`<leader>f`** — format current buffer

## Installation

### Install Neovim

Kickstart.nvim targets *only* the latest
['stable'](https://github.com/neovim/neovim/releases/tag/stable) and latest
['nightly'](https://github.com/neovim/neovim/releases/tag/nightly) of Neovim.
If you are experiencing issues, please make sure you have at least the latest
stable version.

### Install External Dependencies

External Requirements:
- Basic utils: `git`, `make`, `unzip`, C Compiler (`gcc`)
- [ripgrep](https://github.com/BurntSushi/ripgrep#installation)
- Clipboard tool (`xclip` / `xsel` / `win32yank` depending on platform)
- A [Nerd Font](https://www.nerdfonts.com/): optional, provides various icons
  - if you have it set `vim.g.have_nerd_font = true` in `init.lua`

> [!NOTE]
> Unlike the upstream kickstart, this fork uses `nvim-treesitter` **master** branch
> which downloads pre-compiled parsers — **`tree-sitter-cli` is NOT required**.

### Install Kickstart

> [!NOTE]
> [Backup](#faq) your previous configuration (if any exists)

```sh
git clone https://github.com/MertKures/kickstart.nvim.git "${XDG_CONFIG_HOME:-$HOME/.config}"/nvim
```

### Install clangd and clang-format

#### Ubuntu 22.04

```sh
sudo apt-get install clangd-12
sudo update-alternatives --install /usr/bin/clangd clangd /usr/bin/clangd-12 100
```

```sh
sudo apt install clang-format-12
sudo update-alternatives --install /usr/bin/clang-format clang-format /usr/bin/clang-format-12 100
```

If using arm64 (aka aarch64) architecture, run below to make nvim see clangd:

```sh
ln -s /usr/bin/clangd-12 ~/.local/share/nvim/mason/bin/clangd
mkdir ~/.local/share/nvim/mason/packages/clangd
```

### Install arduino-language-server

#### Install go

##### AMD64

```sh
wget https://go.dev/dl/go1.25.3.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.25.3.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' >> $HOME/.bashrc
```

##### ARM64

```sh
wget https://go.dev/dl/go1.25.3.linux-arm64.tar.gz
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.25.3.linux-arm64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' >> $HOME/.bashrc
```

#### Install language server

```sh
go install github.com/arduino/arduino-language-server@latest
echo 'export PATH=$PATH:$HOME/go/bin' >> $HOME/.bashrc
```

### Post Installation

Start Neovim:

```sh
nvim
```

That's it! Lazy will install all the plugins you have. Use `:Lazy` to view
the current plugin status. Hit `q` to close the window.

On first launch, `nvim-treesitter` will automatically download and compile
the required parsers (bash, c, cpp, lua, python, etc.) — this may take a moment.

LSP servers and tools (`pyright`, `ruff`, `stylua`, `clangd`, `arduino_language_server`)
are managed by **Mason** and installed automatically. You can check the status with `:Mason`.

### Key Bindings (Custom)

| Keymap | Action |
| :--- | :--- |
| `<leader>f` | Format current buffer |
| `<A-z>` | Toggle line wrap |
| `<leader>e` | Open diagnostic float |
| `<leader>sh` | Search help tags |
| `<leader>sf` | Search files |
| `<leader>sg` | Search by grep |
| `<leader>th` | Toggle inlay hints |

### FAQ

* What should I do if I already have a pre-existing Neovim configuration?
  * You should back it up and then delete all associated files.
  * This includes your existing init.lua and the Neovim files in `~/.local`
    which can be deleted with `rm -rf ~/.local/share/nvim/`
* Can I keep my existing configuration in parallel to kickstart?
  * Yes! You can use [NVIM_APPNAME](https://neovim.io/doc/user/starting.html#%24NVIM_APPNAME)`=nvim-NAME`
    to maintain multiple configurations. For example you can install the kickstart
    configuration in `~/.config/nvim-kickstart` and create an alias:
    ```
    alias nvim-kickstart='NVIM_APPNAME="nvim-kickstart" nvim'
    ```
* What if I want to "uninstall" this configuration:
  * See [lazy.nvim uninstall](https://lazy.folke.io/usage#-uninstalling) information

### Linux Install

<details><summary>Unsupported older glibc release install steps</summary>

```
sudo apt update
sudo apt install make gcc ripgrep unzip git xclip curl

curl -LO https://github.com/neovim/neovim-releases/releases/download/v0.11.4/nvim-linux-x86_64.tar.gz
sudo rm -rf /opt/nvim-linux-x86_64
sudo mkdir -p /opt/nvim-linux-x86_64
sudo chmod a+rX /opt/nvim-linux-x86_64
sudo tar -C /opt -xzf nvim-linux-x86_64.tar.gz

sudo ln -sf /opt/nvim-linux-x86_64/bin/nvim /usr/local/bin/
```
</details>

<details><summary>Ubuntu Install Steps</summary>

```
sudo add-apt-repository ppa:neovim-ppa/unstable -y
sudo apt update
sudo apt install make gcc ripgrep unzip git xclip neovim
```
</details>

<details><summary>Debian Install Steps</summary>

X86
```
sudo apt update
sudo apt install make gcc ripgrep unzip git xclip curl

# Now we install nvim
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux-x86_64.tar.gz
sudo rm -rf /opt/nvim-linux-x86_64
sudo mkdir -p /opt/nvim-linux-x86_64
sudo chmod a+rX /opt/nvim-linux-x86_64
sudo tar -C /opt -xzf nvim-linux-x86_64.tar.gz

# make it available in /usr/local/bin, distro installs to /usr/bin
sudo ln -sf /opt/nvim-linux-x86_64/bin/nvim /usr/local/bin/
```

ARM64
```
sudo apt update
sudo apt install make gcc ripgrep unzip git xclip curl

# Now we install nvim
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux-arm64.tar.gz
sudo rm -rf /opt/nvim-linux-arm64
sudo mkdir -p /opt/nvim-linux-arm64
sudo chmod a+rX /opt/nvim-linux-arm64
sudo tar -C /opt -xzf nvim-linux-arm64.tar.gz

# make it available in /usr/local/bin, distro installs to /usr/bin
sudo ln -sf /opt/nvim-linux-arm64/bin/nvim /usr/local/bin/
```
</details>

<details><summary>Arch Install Steps</summary>

```
sudo pacman -S --noconfirm --needed gcc make git ripgrep fd unzip neovim
```
</details>
