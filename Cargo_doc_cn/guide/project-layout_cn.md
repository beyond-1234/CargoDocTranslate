{==+==}
## Package Layout
{==+==}
## 包的布局
{==+==}

{==+==}
Cargo uses conventions for file placement to make it easy to dive into a new
Cargo [package][def-package]:
{==+==}
Cargo 有一系列文件布局的约定，从而可以简便地创建新的包:
{==+==}

{==+==}
```text
.
├── Cargo.lock
├── Cargo.toml
├── src/
│   ├── lib.rs
│   ├── main.rs
│   └── bin/
│       ├── named-executable.rs
│       ├── another-executable.rs
│       └── multi-file-executable/
│           ├── main.rs
│           └── some_module.rs
├── benches/
│   ├── large-input.rs
│   └── multi-file-bench/
│       ├── main.rs
│       └── bench_module.rs
├── examples/
│   ├── simple.rs
│   └── multi-file-example/
│       ├── main.rs
│       └── ex_module.rs
└── tests/
    ├── some-integration-tests.rs
    └── multi-file-test/
        ├── main.rs
        └── test_module.rs
```
{==+==}
```text
.                                       项目根目录
├── Cargo.lock                          存储构建成功后的准确版本信息
├── Cargo.toml                          包和构建需要的信息
├── src/                                源码目录
│   ├── lib.rs                          库 crate 的根
│   ├── main.rs                         二进制 cate 的根
│   └── bin/                            其他二进制crate
│       ├── named-executable.rs         命名的可执行crate根
│       ├── another-executable.rs       另一个可执行crate根
│       └── multi-file-executable/      多文件可执行crate根目录，目录名即可执行文件名
│           ├── main.rs                 crate 的根
│           └── some_module.rs          crate 的模块
├── benches/                            性能测试        与bin/目录结构相同
│   ├── large-input.rs                  
│   └── multi-file-bench/               
│       ├── main.rs                     
│       └── bench_module.rs             
├── examples/                           库的实例                  与bin/目录结构相同
│   ├── simple.rs
│   └── multi-file-example/
│       ├── main.rs
│       └── ex_module.rs
└── tests/                              集成测试                  与bin/目录结构相同
    ├── some-integration-tests.rs
    └── multi-file-test/
        ├── main.rs
        └── test_module.rs
```
{==+==}


{==+==}
* `Cargo.toml` and `Cargo.lock` are stored in the root of your package (*package
  root*).
* Source code goes in the `src` directory.
* The default library file is `src/lib.rs`.
* The default executable file is `src/main.rs`.
    * Other executables can be placed in `src/bin/`.
* Benchmarks go in the `benches` directory.
* Examples go in the `examples` directory.
* Integration tests go in the `tests` directory.
{==+==}
*  `Cargo.toml` 和 `Cargo.lock` 保储在包的根目录。
* 源码在 `src` 目录。
* 默认的库 crate 根文件是 `src/lib.rs` 。
* 默认的可执行 crate 根文件是 `src/main.rs` 。
    * 其他可执行 crate 文件可以放在 `src/bin/` 目录。
* 性能测试放在 `benches` 目录。
* 示例放在 `examples` 目录.
* 集成测试文件放在 `tests` 目录.
{==+==}


{==+==}
If a binary, example, bench, or integration test consists of multiple source
files, place a `main.rs` file along with the extra [*modules*][def-module]
within a subdirectory of the `src/bin`, `examples`, `benches`, or `tests`
directory. The name of the executable will be the directory name.
{==+==}
如果一个binary、example、bench 或集成测试具有多个源文件，把 `main.rs` 和其他模块([*modules*][def-module])一起放在目录( `src/bin` 、`examples` 、`benches` 或 `tests` )中的一个子目录下，可执行crate的名字就是该子目录的名字。
{==+==}


{==+==}
You can learn more about Rust's module system in [the book][book-modules].
{==+==}
你可以在 [the book][book-modules] 中了解更多关于Rust模块系统的知识。
{==+==}


{==+==}
See [Configuring a target] for more details on manually configuring targets.
See [Target auto-discovery] for more information on controlling how Cargo
automatically infers target names.
{==+==}
参阅 [Configuring a target] 一节，有更多关于手动设置编译目标的细节。
参阅 [Target auto-discovery] 一节，有更多关于控制Cargo如何自动推断目标名的知识。
{==+==}


{==+==}
[book-modules]: ../../book/ch07-00-managing-growing-projects-with-packages-crates-and-modules.html
[Configuring a target]: ../reference/cargo-targets.md#configuring-a-target
[def-package]:           ../appendix/glossary.md#package          '"package" (glossary entry)'
[def-module]:            ../appendix/glossary.md#module           '"module" (glossary entry)'
[Target auto-discovery]: ../reference/cargo-targets.md#target-auto-discovery
{==+==}

{==+==}
