[![Crates.io](https://img.shields.io/crates/d/rust-htslib.svg)](https://crates.io/crates/rust-htslib)
[![Crates.io](https://img.shields.io/crates/v/rust-htslib.svg)](https://crates.io/crates/rust-htslib)
[![Crates.io](https://img.shields.io/crates/l/rust-htslib.svg)](https://crates.io/crates/rust-htslib)
[![docs.rs](https://docs.rs/rust-htslib/badge.svg)](https://docs.rs/rust-htslib)
![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/rust-bio/rust-htslib/rust.yml?branch=master&label=tests)
[![Coverage Status](https://coveralls.io/repos/github/rust-bio/rust-htslib/badge.svg?branch=master)](https://coveralls.io/github/rust-bio/rust-htslib?branch=master)

# HTSlib bindings for Rust

This library provides HTSlib bindings and a high level Rust API for reading and writing BAM files.

To clone this repository, issue

```shell
$ git clone --recursive https://github.com/rust-bio/rust-htslib.git
```

ensuring that the HTSlib submodule is fetched, too.
If you only want to use the library, there is no need to clone the repository. Go on to the **Usage** section in this case.

## Requirements

rust-htslib comes with pre-built bindings to htslib for Mac and Linux. You will need a C toolchain compatible with the `cc` crate. The build script for this crate will automatically build a link htslib.


### MUSL build
To compile this for MUSL crate you need docker and cross:

```shell
$ cargo install cross
$ cross build 				              # will build with GNU GCC or LLVM toolchains
```

If you want to run rust-htslib code on AWS lambda, [you'll need to statically compile it with MUSL](https://github.com/awslabs/aws-lambda-rust-runtime/issues/17#issuecomment-577490373) as follows:

```shell
$ cross build --target x86_64-unknown-linux-musl      # will build with MUSL toolchain
```

Alternatively, you can also install it locally by installing the development headers of zlib, bzip2 and xz. For instance, in Debian systems one needs the following dependencies:

```shell
$ sudo apt-get install zlib1g-dev libbz2-dev liblzma-dev clang pkg-config
```

We provide Dockerfile bases that provide these dependencies. Refer to the [docker](https://github.com/rust-bio/rust-htslib/tree/master/docker) directory in this repository for the latest instructions, including LLVM installation.

On OSX:

```shell
$ brew install FiloSottile/musl-cross/musl-cross
$ brew install bzip2 zlib xz curl-openssl
```

## Usage

Add this to your `Cargo.toml`:
```toml
[dependencies]
rust-htslib = "*"
```

By default `rust-htslib` links to `bzip2-sys` and `lzma-sys` for full CRAM support. If you do not need CRAM support, or you do need to support CRAM files
with these compression methods, you can deactivate these features to reduce you dependency count:

```toml
[dependencies]
rust-htslib = { version = "*", default-features = false }
```

`rust-htslib` has optional support for `serde`, to allow (de)serialization of `bam::Record` via any serde-supported format.

Http access to files is available with the `curl` feature.

Beta-level S3 and Google Cloud Storge support is available with the `s3` and `gcs` features.

`rust-htslib` can optionally use `bindgen` to generate bindings to htslib. This can slow down the build substantially. Enabling the `bindgen` feature will 
cause `hts-sys` to use a create a binding file for your architecture. Pre-built bindings are supplied for Mac and Linux. The `bindgen` feature on Windows is untested - please file a bug if you need help.



```toml
[dependencies]
rust-htslib = { version = "*", features = ["serde_feature"] }
```

For more information, please see the [docs](https://docs.rs/rust-htslib).

# Alternatives

There's [noodles](https://github.com/zaeleus/noodles) by [Michael Macias](https://github.com/zaeleus) which implements a large part of htslib's C functionality in pure Rust (still experimental though).

# Authors

* [Johannes Köster](https://github.com/johanneskoester)
* [Christopher Schröder](https://github.com/christopher-schroeder)
* [Patrick Marks](https://github.com/pmarks)
* [David Lähnemann](https://github.com/dlaehnemann)
* [Manuel Holtgrewe](https://github.com/holtgrewe)
* [Julian Gehring](https://github.com/juliangehring)

For other contributors, see [here](https://github.com/rust-bio/rust-htslib/graphs/contributors).

## License

Licensed under the MIT license https://opensource.org/licenses/MIT. This project may not be copied, modified, or distributed except according to those terms.
Some test files are taken from https://github.com/samtools/htslib.
