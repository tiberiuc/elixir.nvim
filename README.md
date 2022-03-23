# elixir.nvim

`elixir.nvim` provides a nice experience for writing Elixir applications with [Neovim](https://github.com/neovim/neovim).

> Note: This plugin does not provide autocompletion, I recommend using [nvim-cmp](https://github.com/hrsh7th/nvim-cmp).

## Install

```lua
use({"mhanberg/elixir.nvim", requires = { "neovim/nvim-lspconfig" }})
```

## Getting Started

You will need to install the [ElixirLS](https://github.com/elixir-lsp/elixir-ls) language server manually and provide a path to the `language_server.sh` script.

### Minimal Setup

```lua
local elixir = require("elixir")

elixir.setup({
  -- required option, reminder this is a table
  cmd = { vim.fn.expand("path/to/language_server.sh") },
})
```

### Advanced Setup

While the plugin works with a minimal setup, it is much more useful if you add some personal configuration.

```lua
local elixir = require("elixir")

elixir.setup({
  -- required option, reminder this is a table
  cmd = { vim.fn.expand("path/to/language_server.sh") },

  -- default settings, use the `settings` function to override settings
  settings = elixir.settings({
    dialyzerEnabled = true,
    fetchDeps = false,
    enableTestLenses = false,
    suggestSpecs = false,
  }),

  on_attach = function(client, bufnr)
    -- run the codelens under the cursor
    vim.keymap.set("n", "<space>r",  vim.lsp.codelens.run,                             { buffer = true, noremap = true })
    -- remove the pipe operator
    vim.keymap.set("n", "<space>fp", elixir.from_pipe(client),                         { buffer = true, noremap = true })
    -- add the pipe operator
    vim.keymap.set("n", "<space>tp", elixir.to_pipe(client),                           { buffer = true, noremap = true })

    -- standard lsp keybinds
    vim.keymap.set("n", "df",        "<cmd>lua vim.lsp.buf.formatting_seq_sync()<cr>", { buffer = true, noremap = true })
    vim.keymap.set("n", "gd",        "<cmd>lua vim.diagnostic.open_float()<cr>",       { buffer = true, noremap = true })
    vim.keymap.set("n", "dt",        "<cmd>lua vim.lsp.buf.definition()<cr>",          { buffer = true, noremap = true })
    vim.keymap.set("n", "K",         "<cmd>lua vim.lsp.buf.hover()<cr>",               { buffer = true, noremap = true })
    vim.keymap.set("n", "gD",        "<cmd>lua vim.lsp.buf.implementation()<cr>",      { buffer = true, noremap = true })
    vim.keymap.set("n", "1gD",       "<cmd>lua vim.lsp.buf.type_definition()<cr>",     { buffer = true, noremap = true })
    -- keybinds for fzf-lsp.nvim: https://github.com/gfanto/fzf-lsp.nvim
    -- you could also use telescope.nvim: https://github.com/nvim-telescope/telescope.nvim
    -- there are also core vim.lsp functions that put the same data in the loclist
    vim.keymap.set("n", "gr",        ":References<cr>",                                { buffer = true, noremap = true })
    vim.keymap.set("n", "g0",        ":DocumentSymbols<cr>",                           { buffer = true, noremap = true })
    vim.keymap.set("n", "gW",        ":WorkspaceSymbols<cr>",                          { buffer = true, noremap = true })
    vim.keymap.set("n", "<leader>d", ":Diagnostics<cr>",                               { buffer = true, noremap = true })


    -- keybinds for vim-vsnip: https://github.com/hrsh7th/vim-vsnip
    vim.cmd([[imap <expr> <C-l> vsnip#available(1) ? '<Plug>(vsnip-expand-or-jump)' : '<C-l>']])
    vim.cmd([[smap <expr> <C-l> vsnip#available(1) ? '<Plug>(vsnip-expand-or-jump)' : '<C-l>']])

    -- update capabilities for nvim-cmp: https://github.com/hrsh7th/nvim-cmp
    require("cmp_nvim_lsp").update_capabilities(capabilities)
  end
})
```

## Features

### Run Tests

ElixirLS provides a codelens to identify and run your tests. If you configure `enableTestLenses = true` in the settings table, you will see the codelens as virtual text in your editor and can run them with `vim.lsp.codelens.run()`.

![elixir-test-lens](https://user-images.githubusercontent.com/5523984/159722637-ef1586d5-9d47-4e1a-b68b-6a90ad744098.gif)

### Manipulate Pipes

TODO: description

TODO: gif
