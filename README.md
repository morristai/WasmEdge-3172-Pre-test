Pre Test for LFX WasmEdge [3172](https://github.com/WasmEdge/WasmEdge/issues/3172)
---

<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->

- [1. Burn Framework Build and Execution](#1-burn-framework-build-and-execution)
    * [Init Project](#init-project)
    * [Burn Example Code](#burn-example-code)
    * [Build and Run](#build-and-run)
- [2. Build WasmEdge Rustls Plug-in](#2-build-wasmedge-rustls-plug-in)
    * [Prerequisites](#prerequisites)
    * [Build and Install](#build-and-install)
- [3. Example with WasmEdge Rustls Plug-in](#3-example-with-wasmedge-rustls-plug-in)
    * [WasmEdge Hyper Demo](#wasmedge-hyper-demo)
        + [Prerequisites](#prerequisites-1)
        + [Build and Run](#build-and-run-1)

<!-- TOC end -->

<!-- TOC --><a name="1-burn-framework-build-and-execution"></a>
# 1. Burn Framework Build and Execution

> Ref: https://burn.dev/book/getting-started.html

<!-- TOC --><a name="init-project"></a>
## Init Project

```shell
cargo new burn_playground
cd burn_playground
cargo add burn --features wgpu
```
![init_project.png](static%2Finit_project.png)

<!-- TOC --><a name="burn-example-code"></a>
## Burn Example Code

Update `./burn_playground/src/main.rs` with the following code:

```rust
use burn::tensor::Tensor;
use burn::backend::Wgpu;

// Type alias for the backend to use.
type Backend = Wgpu;

fn main() {
    let device = Default::default();
    // Creation of two tensors, the first with explicit values and the second one with ones, with the same shape as the first
    let tensor_1 = Tensor::<Backend, 2>::from_data([[2., 3.], [4., 5.]], &device);
    let tensor_2 = Tensor::<Backend, 2>::ones_like(&tensor_1);

    // Print the element-wise addition (done with the WGPU backend) of the two tensors.
    println!("{}", tensor_1 + tensor_2);
}
```

<!-- TOC --><a name="build-and-run"></a>
## Build and Run

```shell
cargo build
cargo run
```
![run_playground.png](static%2Frun_playground.png)

<!-- TOC --><a name="2-build-wasmedge-rustls-plug-in"></a>
# 2. Build WasmEdge Rustls Plug-in

<!-- TOC --><a name="prerequisites"></a>
## Prerequisites

```bash
# Cmake
brew install cmake
# WasmEdge runtime (https://wasmedge.org/docs/start/install/)
curl -sSf https://raw.githubusercontent.com/WasmEdge/WasmEdge/master/utils/install.sh | bash
source $HOME/.wasmedge/env
```

<!-- TOC --><a name="build-and-install"></a>
## Build and Install

```shell
# fork branch: hydai/0.13.5_ggml_lts
git clone https://github.com/morristai/WasmEdge
cd WasmEdge/plugins/wasmedge_rustls
cargo build --release
```
![build_plugin.png](static%2Fbuild_plugin.png)

```shell
# Install the plugin
cp target/release/libwasmedge_rustls.dylib ~/.wasmedge/plugin/
```

<!-- TOC --><a name="3-example-with-wasmedge-rustls-plug-in"></a>
# 3. Example with WasmEdge Rustls Plug-in

<!-- TOC --><a name="wasmedge-hyper-demo"></a>
## WasmEdge Hyper Demo
> Ref: https://github.com/WasmEdge/wasmedge_hyper_demo

<!-- TOC --><a name="prerequisites-1"></a>
### Prerequisites

```shell
# Install wasm/wasi target 
rustup target add wasm32-wasi
```

<!-- TOC --><a name="build-and-run-1"></a>
### Build and Run

```shell
cargo build --target wasm32-wasi --release
# Run the client example
wasmedge target/wasm32-wasi/release/wasmedge_hyper_client.wasm
```

- Install pre-requisites and build the client example.
  ![build_demo.png](static%2Fbuild_demo.png)
- The client sends a GET request to `http://eu.httpbin.org/get?msg=Hello` and receives the response.
  ![run_client.png](static%2Frun_client.png)
