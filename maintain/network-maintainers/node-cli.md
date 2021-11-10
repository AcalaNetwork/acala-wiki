
## Acala’s Command Line Interface flags option list

**Always refer to the client's help `acala --help` for the most up-to-date information.**

### Common

| Flags | Descriptions |
| :--- | :--- |
| `--base-path <PATH>` | Specifies a directory where Acala should store all the data related to this chain. If this value is not specified, a default path will be used. If the directory does not exist it will be created for you. If other blockchain data already exists there you will get an error. Either clear the directory or choose a different one. |
| `--chain <CHAIN_SPEC>` | Specifies which chain specification to use. There are a few prepackaged options including `karura`, `acala`, `mandala` and `dev` but generally one specifies their own chain spec file. We'll specify our own file in a later step. |
| `--name <NAME>` | The human-readable name for this node. |
| `--alice` | Puts the predefined Alice keys \(both for block production and finalization\) in the node's keystore. Generally one should generate their own keys and insert them with an RPC call. We'll generate our own keys in a later step. This flag also makes Alice a validator. |
| `--port 30333` | Specifies the port that your node will listen for p2p traffic on. `30333` is the default and this flag can be omitted if you're happy with the default. If Bob's node will run on the same physical system, you will need to explicitly specify a different port for it. |
| `--ws-port 9945` | Specifies the port that your node will listen for incoming WebSocket traffic on. The default value is `9944`. This example uses a custom web socket port number \(`9945`\). |
| `--rpc-port 9933` | Specifies the port that your node will listen for incoming RPC traffic on. `9933` is the default, so this parameter may be omitted. |
| `--node-key` | The Ed25519 secret key to use for `libp2p` networking. The value is parsed as a hex-encoded Ed25519 32 byte secret key, i.e. 64 hex characters. WARNING: Secrets provided as command-line arguments are easily exposed. Use of this option should be limited to development and testing. |
| `--telemetry-url` | Tells the node to send telemetry data to a particular server. The one we've chosen here is hosted by Parity and is available for anyone to use. You may also host your own \(beyond the scope of this article\) or omit this flag entirely. |
| `--validator` | Means that we want to participate in block production and finalization rather than just sync the network. |


### RPC node

| Flags | Descriptions |
| :--- | :--- |
| `--pruning=archive` | Specify the state pruning mode, a number of blocks to keep or 'archive'.  Default is to keep all block states if the node is running as a validator (i.e. 'archive'), otherwise is only kept for the last 256 blocks. |
| `--rpc-cors=all` | Specify browser Origins allowed to access the HTTP & WS RPC servers. A comma-separated list of origins (protocol://domain or special `null` value). Value of `all` will disable origin validation. Default is to allow localhost and <https://polkadot.js.org> origins. When running in --dev mode the default is to allow all origins. |
| `--ws-max-connections=1000` | Maximum number of WS RPC server connections |

### Collator node

| Flags | Descriptions |
| :--- | :--- |
| `--collator` | Run node as collator. |
| `--unsafe-ws-external` | Listen to all Websocket interfaces.  Same as `--ws-external` but doesn't warn you about it. |
| `--unsafe-rpc-external` | Listen to all RPC interfaces.  Same as `--rpc-external`. |
| `--rpc-methods=unsafe` | RPC methods to expose. - `Unsafe`: Exposes every RPC method. - `Safe`: Exposes only a safe subset of RPC methods, denying unsafe RPC methods.  - `Auto`: Acts as `Safe` if RPC is served externally, e.g. when `--{rpc,ws}-external` is passed, otherwise acts as `Unsafe`. [default: Auto]  [possible values: Auto, Safe, Unsafe |

### Bootnode

| Flags | Descriptions |
| :--- | :--- |
| `--node-key-file=/path/to/file` | The file from which to read the node's secret key to use for libp2p networking.  The contents of the file are parsed according to the choice of `--node-key-type` as follows: `ed25519`: The file must contain an unencoded 32 byte or hex encoded Ed25519 secret key.  If the file does not exist, it is created with a newly generated secret key of the chosen type. |
| `--in-peers=100` | Specify the maximum number of incoming connections we're accepting [default: 25] |
| `--listen-addr=/ip4/0.0.0.0/tcp/30333` | Listen on this multiaddress.  By default: If `--validator` is passed: `/ip4/0.0.0.0/tcp/<port>` and `/ip6/[::]/tcp/<port>`. Otherwise: `/ip4/0.0.0.0/tcp/<port>/ws` and `/ip6/[::]/tcp/<port>/ws`. |