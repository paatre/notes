# Language Server Protocol (LSP)

Language Server Protocol (LSP) works with a client-server archictecture.

## LSP Client

Example: Neovim.

LSP client is responsible for sending requests (like "hover", "format", "autocomplete") and receiving responses from the LSP server.

### Capabilities

LSP client declares its capabilities when initializing the connection with the LSP server. It tells the server what features the client supports and what text representation it is capable of handling. Usually the capabilities is a JSON object.

## LSP Server

Example: Pyright.

LSP server is a separate process that runs in the background, maintaining an abstract syntax tree (AST) of your project and providing all the language intelligence.
