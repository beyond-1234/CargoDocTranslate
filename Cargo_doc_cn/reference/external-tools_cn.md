{==+==}
## External tools
{==+==}
## 插件
{==+==}


{==+==}
One of the goals of Cargo is simple integration with third-party tools, like
IDEs and other build systems. To make integration easier, Cargo has several
facilities:
{==+==}
Cargo的目标之一是能与第三方工具简单集成，如IDE和其他构建系统。为了使集成更容易，Cargo有几个工具:
{==+==}


{==+==}
* a [`cargo metadata`] command, which outputs package structure and dependencies
  information in JSON,
{==+==}
* [`cargo metadata`]命令，以JSON格式输出包结构和依赖信息。
{==+==}


{==+==}
* a `--message-format` flag, which outputs information about a particular build,
  and
{==+==}
* `--message-format` 标志，用于输出关于特定构建的信息，以及
{==+==}


{==+==}
* support for custom subcommands.
{==+==}
* 支持自定义子命令。
{==+==}


{==+==}
### Information about package structure
{==+==}
### 关于包结构信息
{==+==}


{==+==}
You can use [`cargo metadata`] command to get information about package
structure and dependencies. See the [`cargo metadata`] documentation
for details on the format of the output.
{==+==}
你可以使用 [`cargo metadata`] 命令来获取包结构和依赖的信息。有关输出格式的详细信息，请参见[`cargo metadata`]文档。
{==+==}


{==+==}
The format is stable and versioned. When calling `cargo metadata`, you should
pass `--format-version` flag explicitly to avoid forward incompatibility
hazard.
{==+==}
该格式是稳定的，有相应版本。当调用 `cargo metadata` 时，你应该明确传递 `--format-version` 标志以避免向前不兼容的风险。
{==+==}


{==+==}
If you are using Rust, the [cargo_metadata] crate can be used to parse the
output.
{==+==}
如果你使用的是Rust，可以使用[cargo_metadata] crate来解析输出。
{==+==}


{==+==}
[cargo_metadata]: https://crates.io/crates/cargo_metadata
[`cargo metadata`]: ../commands/cargo-metadata.md
{==+==}

{==+==}


{==+==}
### JSON messages
{==+==}
### JSON 消息
{==+==}


{==+==}
When passing `--message-format=json`, Cargo will output the following
information during the build:
{==+==}
当传递 `---message-format=json` 时，Cargo会在构建时输出以下信息:
{==+==}


{==+==}
* compiler errors and warnings,

* produced artifacts,

* results of the build scripts (for example, native dependencies).
{==+==}
* 编译器错误和警告。

* 产生的制品。

* 构建脚本的结果(例如，本地依赖)。
{==+==}


{==+==}
The output goes to stdout in the JSON object per line format. The `reason` field
distinguishes different kinds of messages.
{==+==}
在标准输出以每行的JSON对象格式输出。`reason` 字段区分不同种类的信息。
{==+==}


{==+==}
The `--message-format` option can also take additional formatting values which
alter the way the JSON messages are computed and rendered. See the description
of the `--message-format` option in the [build command documentation] for more
details.
{==+==}
`--message-format` 选项也可以采用额外的格式化值，改变JSON信息的计算和显示方式。
更多细节见[build command documentation]中对 `--message-format` 选项的描述。
{==+==}


{==+==}
If you are using Rust, the [cargo_metadata] crate can be used to parse these
messages.
{==+==}
如果你使用的是Rust，可以使用 [cargo_metadata] crate来解析这些信息。
{==+==}


{==+==}
[build command documentation]: ../commands/cargo-build.md
[cargo_metadata]: https://crates.io/crates/cargo_metadata
{==+==}

{==+==}


{==+==}
#### Compiler messages
{==+==}
#### 编译消息
{==+==}


{==+==}
The "compiler-message" message includes output from the compiler, such as
warnings and errors. See the [rustc JSON chapter](../../rustc/json.md) for
details on `rustc`'s message format, which is embedded in the following
structure:
{==+==}
"compiler-message" 消息包括来自编译器的输出，如警告和错误。关于 `rustc` 的消息格式，请参见 [rustc JSON 章节](.../.../rustc/json.md)，它被嵌入到以下结构中。
{==+==}


{==+==}
```javascript
{
    /* The "reason" indicates the kind of message. */
    "reason": "compiler-message",
    /* The Package ID, a unique identifier for referring to the package. */
    "package_id": "my-package 0.1.0 (path+file:///path/to/my-package)",
    /* Absolute path to the package manifest. */
    "manifest_path": "/path/to/my-package/Cargo.toml",
    /* The Cargo target (lib, bin, example, etc.) that generated the message. */
    "target": {
        /* Array of target kinds.
           - lib targets list the `crate-type` values from the
             manifest such as "lib", "rlib", "dylib",
             "proc-macro", etc. (default ["lib"])
           - binary is ["bin"]
           - example is ["example"]
           - integration test is ["test"]
           - benchmark is ["bench"]
           - build script is ["custom-build"]
        */
        "kind": [
            "lib"
        ],
{==+==}
```javascript
{
    /* "reason" 表示信息的种类。*/
    "reason": "compiler-message",
    /* 包的ID，是指包的唯一标识。 */
    "package_id": "my-package 0.1.0 (path+file:///path/to/my-package)",
    /*  包配置清单的绝对路径。 */
    "manifest_path": "/path/to/my-package/Cargo.toml",
    /* 产生该消息的Cargo目标(lib、bin、example等等)。 */
    "target": {
        /* 目标种类数组。
           - lib目标列出配置清单中的 `cate-type` 值，如 "lib"、"rlib"、"dylib"、"proc-macro" 等(默认["lib"])。
           - 二进制是 ["bin"]
           - 实例是 ["example"]
           - 综合测试是 ["test"]
           - 性能测试是 ["bench"]
           - 构建脚本是 ["custom-build"]
        */
        "kind": [
            "lib"
        ],
{==+==}


{==+==}
        /* Array of crate types.
           - lib and example libraries list the `crate-type` values
             from the manifest such as "lib", "rlib", "dylib",
             "proc-macro", etc. (default ["lib"])
           - all other target kinds are ["bin"]
        */
        "crate_types": [
            "lib"
        ],
        /* The name of the target. */
        "name": "my-package",
        /* Absolute path to the root source file of the target. */
        "src_path": "/path/to/my-package/src/lib.rs",
        /* The Rust edition of the target.
           Defaults to the package edition.
        */
        "edition": "2018",
        /* Array of required features.
           This property is not included if no required features are set.
        */
        "required-features": ["feat1"],
        /* Whether or not this target has doc tests enabled, and
           the target is compatible with doc testing.
        */
        "doctest": true
    },
    /* The message emitted by the compiler.

    See https://doc.rust-lang.org/rustc/json.html for details.
    */
    "message": {
        /* ... */
    }
}
```
{==+==}
        /* crate类型数组。
           - lib和示例库列出配置清单中的 `crate-type` 值，如 "lib"、"rlib"、"dylib"、"proc-macro" 等(默认为["lib"])。
           - 所有其他目标类型是 ["bin"]
        */
        "crate_types": [
            "lib"
        ],
        /* 目标名称。 */
        "name": "my-package",
        /* 目标的根源文件的绝对路径。 */
        "src_path": "/path/to/my-package/src/lib.rs",
        /* 目标的Rust版本。默认为包版本。 */
        "edition": "2018",
        /* 所需特性的数组。如果没有设置必要的特性，则不包含此属性。 */
        "required-features": ["feat1"],
        /* 该目标是否启用了文档测试，以及该目标是否与文档测试兼容。 */
        "doctest": true
    },
    /* 由编译器发出的信息。 参阅 https://doc.rust-lang.org/rustc/json.html 。 */
    "message": {
        /* ... */
    }
}
```
{==+==}


{==+==}
#### Artifact messages
{==+==}
#### 制品消息
{==+==}


{==+==}
For every compilation step, a "compiler-artifact" message is emitted with the
following structure:
{==+==}
对于每一个编译步骤，都会发出 "compiler-artifact" 消息，其结构如下:
{==+==}


{==+==}
```javascript
{
    /* The "reason" indicates the kind of message. */
    "reason": "compiler-artifact",
    /* The Package ID, a unique identifier for referring to the package. */
    "package_id": "my-package 0.1.0 (path+file:///path/to/my-package)",
    /* Absolute path to the package manifest. */
    "manifest_path": "/path/to/my-package/Cargo.toml",
    /* The Cargo target (lib, bin, example, etc.) that generated the artifacts.
       See the definition above for `compiler-message` for details.
    */
    "target": {
        "kind": [
            "lib"
        ],
        "crate_types": [
            "lib"
        ],
        "name": "my-package",
        "src_path": "/path/to/my-package/src/lib.rs",
        "edition": "2018",
        "doctest": true,
        "test": true
    },
{==+==}
```javascript
{
    /* "reason" 表示信息的种类。 */
    "reason": "compiler-artifact",
    /* 包的ID，是指包的唯一标识。 */
    "package_id": "my-package 0.1.0 (path+file:///path/to/my-package)",
    /* 包配置清单的绝对路径。 */
    "manifest_path": "/path/to/my-package/Cargo.toml",
    /* 产生制品的Cargo目标(lib、bin、example等等)。详见上面 `compiler-message` 的定义。 */
    "target": {
        "kind": [
            "lib"
        ],
        "crate_types": [
            "lib"
        ],
        "name": "my-package",
        "src_path": "/path/to/my-package/src/lib.rs",
        "edition": "2018",
        "doctest": true,
        "test": true
    },
{==+==}


{==+==}
    /* The profile indicates which compiler settings were used. */
    "profile": {
        /* The optimization level. */
        "opt_level": "0",
        /* The debug level, an integer of 0, 1, or 2. If `null`, it implies
           rustc's default of 0.
        */
        "debuginfo": 2,
        /* Whether or not debug assertions are enabled. */
        "debug_assertions": true,
        /* Whether or not overflow checks are enabled. */
        "overflow_checks": true,
        /* Whether or not the `--test` flag is used. */
        "test": false
    },
    /* Array of features enabled. */
    "features": ["feat1", "feat2"],
    /* Array of files generated by this step. */
    "filenames": [
        "/path/to/my-package/target/debug/libmy_package.rlib",
        "/path/to/my-package/target/debug/deps/libmy_package-be9f3faac0a26ef0.rmeta"
    ],
    /* A string of the path to the executable that was created, or null if
       this step did not generate an executable.
    */
    "executable": null,
    /* Whether or not this step was actually executed.
       When `true`, this means that the pre-existing artifacts were
       up-to-date, and `rustc` was not executed. When `false`, this means that
       `rustc` was run to generate the artifacts.
    */
    "fresh": true
}

```
{==+==}
    /* 该简介表明使用了哪些编译器设置。 */
    "profile": {
        /* 优化级别。 */
        "opt_level": "0",
        /* 调试级别，是0、1或2的整数。如果 "null"，它意味着rustc的默认值为0。 */
        "debuginfo": 2,
        /* 是否启用了调试断言。 */
        "debug_assertions": true,
        /* 是否启用了溢出检查。 */
        "overflow_checks": true,
        /* 是否使用`--test` 标志。*/
        "test": false
    },
    /* 启用特性数组。 */
    "features": ["feat1", "feat2"],
    /* 该步骤产生的文件的数组。*/
    "filenames": [
        "/path/to/my-package/target/debug/libmy_package.rlib",
        "/path/to/my-package/target/debug/deps/libmy_package-be9f3faac0a26ef0.rmeta"
    ],
    /* 创建的可执行文件的路径的字符串，如果该步骤没有生成可执行文件，则为空。 */
    "executable": null,
    /* 这个步骤是否被实际执行。当 `true` 时，这意味着预先存在的制品是最新的，并且 `rustc` 没有被执行。当 `false` 时，这意味着运行 `rustc` 以生成制品。 */
    "fresh": true
}

```
{==+==}


{==+==}
#### Build script output
{==+==}
#### 构建脚本输出
{==+==}


{==+==}
The "build-script-executed" message includes the parsed output of a build
script. Note that this is emitted even if the build script is not run; it will
display the previously cached value. More details about build script output
may be found in [the chapter on build scripts](build-scripts.md).
{==+==}
"build-script-executed" 消息包括构建脚本的解析输出。注意，即使构建脚本没有运行，也会发出这个消息；
它将显示之前缓存的值。关于构建脚本输出的更多细节可以在[构建脚本章节](build-scripts.md)中找到。
{==+==}


{==+==}
```javascript
{
    /* The "reason" indicates the kind of message. */
    "reason": "build-script-executed",
    /* The Package ID, a unique identifier for referring to the package. */
    "package_id": "my-package 0.1.0 (path+file:///path/to/my-package)",
    /* Array of libraries to link, as indicated by the `cargo:rustc-link-lib`
       instruction. Note that this may include a "KIND=" prefix in the string
       where KIND is the library kind.
    */
    "linked_libs": ["foo", "static=bar"],
    /* Array of paths to include in the library search path, as indicated by
       the `cargo:rustc-link-search` instruction. Note that this may include a
       "KIND=" prefix in the string where KIND is the library kind.
    */
{==+==}
```javascript
{
    /* "reason" 表示信息的种类。 */
    "reason": "build-script-executed",
    /* 包的ID，是指包的唯一标识。 */
    "package_id": "my-package 0.1.0 (path+file:///path/to/my-package)",
    /* 要链接的库的数组，如 `cargo:rustc-link-lib` 指令所指示。注意，这可能包括字符串中的 "KIND=" 前缀，其中KIND是库的种类。 */
    "linked_libs": ["foo", "static=bar"],
    /* 包含在库搜索路径中的路径数组，如 `cargo:rustc-link-search` 指令所指示。注意，这可能包括字符串中的 "KIND=" 前缀，其中KIND是库的种类。 */
{==+==}


{==+==}
    "linked_paths": ["/some/path", "native=/another/path"],
    /* Array of cfg values to enable, as indicated by the `cargo:rustc-cfg`
       instruction.
    */
    "cfgs": ["cfg1", "cfg2=\"string\""],
    /* Array of [KEY, VALUE] arrays of environment variables to set, as
       indicated by the `cargo:rustc-env` instruction.
    */
    "env": [
        ["SOME_KEY", "some value"],
        ["ANOTHER_KEY", "another value"]
    ],
    /* An absolute path which is used as a value of `OUT_DIR` environmental
       variable when compiling current package.
    */
    "out_dir": "/some/path/in/target/dir"
}
```
{==+==}
    "linked_paths": ["/some/path", "native=/another/path"],
    /* 要启用的cfg值数组，如 `cargo:rustc-cfg` 指令所示。 */
    "cfgs": ["cfg1", "cfg2=\"string\""],
    /* 要设置的环境变量的 [KEY, VALUE] 数组，如 `cargo:rustc-env` 指令所示。 */
    "env": [
        ["SOME_KEY", "some value"],
        ["ANOTHER_KEY", "another value"]
    ],
    /* 一个绝对路径，在编译当前包时作为 `OUT_DIR` 环境变量的值。 */
    "out_dir": "/some/path/in/target/dir"
}
```
{==+==}


{==+==}
#### Build finished
{==+==}
#### 构建完成
{==+==}


{==+==}
The "build-finished" message is emitted at the end of the build.
{==+==}
构建结束后会发出 "build-finished" 消息。
{==+==}


{==+==}
```javascript
{
    /* The "reason" indicates the kind of message. */
    "reason": "build-finished",
    /* Whether or not the build finished successfully. */
    "success": true,
}
````
{==+==}
```javascript
{
    /* "reason" 表示信息的种类。 */
    "reason": "build-finished",
    /* 构建是否成功完成。 */
    "success": true,
}
````
{==+==}


{==+==}
This message can be helpful for tools to know when to stop reading JSON
messages. Commands such as `cargo test` or `cargo run` can produce additional
output after the build has finished. This message lets a tool know that Cargo
will not produce additional JSON messages, but there may be additional output
that may be generated afterwards (such as the output generated by the program
executed by `cargo run`).
{==+==}
这条信息对工具来说很有帮助，可以知道何时停止读取JSON信息。
诸如 `cargo test` 或 `cargo run` 之类的命令在构建完成后可能会产生额外的输出。
这条消息让工具知道Cargo不会产生额外的JSON消息，但之后可能会有额外的输出(比如 `cargo run` 执行的程序产生的输出)。
{==+==}


{==+==}
> Note: There is experimental nightly-only support for JSON output for tests,
> so additional test-specific JSON messages may begin arriving after the
> "build-finished" message if that is enabled.
{==+==}
> 注意: 对测试的JSON输出有实验性的每日支持，所以如果启用了该功能，额外的测试专用JSON消息可能会在 "build-finished" 消息之后。
{==+==}


{==+==}
### Custom subcommands
{==+==}
### 自定义子命令
{==+==}


{==+==}
Cargo is designed to be extensible with new subcommands without having to modify
Cargo itself. This is achieved by translating a cargo invocation of the form
cargo `(?<command>[^ ]+)` into an invocation of an external tool
`cargo-${command}`. The external tool must be present in one of the user's
`$PATH` directories.
{==+==}
Cargo被设计成可以扩展新的子命令，而不需要修改Cargo本身。
这是通过将 cargo `(?<command>[^ ]+)` 形式的调用转换成插件 `cargo-${command}` 的调用来实现的。
该插件必须存在于用户的 `$PATH` 目录中。
{==+==}


{==+==}
When Cargo invokes a custom subcommand, the first argument to the subcommand
will be the filename of the custom subcommand, as usual. The second argument
will be the subcommand name itself. For example, the second argument would be
`${command}` when invoking `cargo-${command}`. Any additional arguments on the
command line will be forwarded unchanged.
{==+==}
当Cargo调用一个自定义子命令时，子命令的第一个参数将是自定义子命令的文件名，像往常一样。
第二个参数将是子命令名称本身。例如，调用 "cargo-${command}" 时，第二个参数将是 "${command}" 。
命令行上的任何其他参数将被转发，不做任何改变。
{==+==}


{==+==}
Cargo can also display the help output of a custom subcommand with `cargo help
${command}`. Cargo assumes that the subcommand will print a help message if its
third argument is `--help`. So, `cargo help ${command}` would invoke
`cargo-${command} ${command} --help`.
{==+==}
Cargo也可以用 `cargo help ${command}` 显示自定义子命令的帮助输出。
如果子命令的第三个参数是 `--help`，Cargo会假定该子命令会打印帮助信息。
因此，`cargo help ${command}` 会调用 `cargo-${command} ${command} --help` 。
{==+==}


{==+==}
Custom subcommands may use the `CARGO` environment variable to call back to
Cargo. Alternatively, it can link to `cargo` crate as a library, but this
approach has drawbacks:
{==+==}
自定义子命令可以使用 `CARGO` 环境变量来回调Cargo。
另外，它也可以作为一个库链接到 `cargo` crate ，但这种方法有缺点。
{==+==}


{==+==}
* Cargo as a library is unstable: the  API may change without deprecation
* versions of the linked Cargo library may be different from the Cargo binary
{==+==}
* 作为一个库，Cargo是不稳定的：API可能会改变而未淘汰。
* 链接的Cargo库的版本可能与Cargo二进制文件不同。
{==+==}


{==+==}
Instead, it is encouraged to use the CLI interface to drive Cargo. The [`cargo
metadata`] command can be used to obtain information about the current project
(the [`cargo_metadata`] crate provides a Rust interface to this command).
{==+==}
相反，我们鼓励使用CLI接口来驱动Cargo。[`cargo metadata`] 命令可以用来获取当前项目的信息([`cargo_metadata`] crate为该命令提供了Rust接口)。
{==+==}


{==+==}
[`cargo metadata`]: ../commands/cargo-metadata.md
[`cargo_metadata`]: https://crates.io/crates/cargo_metadata
{==+==}

{==+==}