# VRust
Automated Vulnerability Detector for Solana Smart Contracts

## Build
First, install rustup, and install a nightly version rustc:
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
rustup install nightly-2022-02-24
rustup default nightly-2022-02-24
rustup component add rust-src
rustup component add rustc-dev
rustup component add llvm-tools-preview
cargo install xargo
```
(Rust nightly changes dramastically, so make sure you used the version specified above)

Go to vrust folder, run:
```
cargo build
```
If you see error, please set RUSTFLAGS env variable to the toolchain lib path, or you can add a config.toml
under ~/.cargo folder, with the below content (replace the path to your own one): 
```
[build]
rustflags = ["-L", "/home/USERNAME/.rustup/toolchains/nightly-2021-12-14-x86_64-unknown-linux-gnu/lib"]
```
## Run
Refer to README in examples/ to use VRust automatically through run.py script, or continue to run on your own.

Add an Xargo.toml file under smart contract source directory with the below content:
```
[dependencies.std]
features = []

[target.x86_64-unknown-linux-gnu.dependencies.term]
stage = 1

[target.x86_64-unknown-linux-gnu.dependencies.proc_macro]
stage = 2
```

Run:

```
RUST_LOG=debug RUSTFLAGS="-Zalways-encode-mir -Zsymbol-mangling-version=v0 -C panic=abort" xargo build -v
cargo clean
```

Then add .cargo/config.toml under source code directory with the below content (change the path to your own
path , remember to build it first):
```
[build]
rustc = "/PATH_TO_VRUST/target/debug/vrust"
```

Finally run xargo again:
```
RUST_LOG=debug RUSTFLAGS="-Zalways-encode-mir -Zsymbol-mangling-version=v0 -C panic=abort" xargo build -v
```

## Research Paper

For more information, please refer to our [VRust Paper](https://dl.acm.org/doi/abs/10.1145/3548606.3560552) accepted [CCS'2022](https://www.sigsac.org/ccs/CCS2022/).


