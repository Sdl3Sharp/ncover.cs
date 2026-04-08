# ncover.cs

`ncover.cs` is a minimal, file‑based C# tool for comparing native symbol coverage between a Windows PE (portable executable) and a .NET assembly. It checks which symbols exported by a native library are imported by a managed assembly, reports coverage statistics, and flags potential issues using a built‑in rule engine. It was originally built for use with [Sdl3Sharp](https://github.com/Sdl3Sharp/) but works with any native/.NET library pair that follows similar P/Invoke patterns.

## Requirements

- .NET 10 SDK must be installed  
- The `dotnet` CLI must be available in your PATH  

## Running the tool

You can run `ncover.cs` in several ways:

**Normal invocation:**

```shell
dotnet run ncover.cs -- native.dll managed.dll
```

**Unix‑like systems (shell script):**

```shell
./ncover.sh native.dll managed.dll
```

If you get a permission error, run:

```shell
chmod +x ncover.sh
```

**PowerShell:**

```shell
./ncover.ps1 native.dll managed.dll
```

**Windows CMD:**

```shell
ncover.cmd native.dll managed.dll
```

The wrapper scripts simply forward arguments to the tool.

If you have issues running `ncover.cs`, try running `dotnet clean ncover.cs` followed by `dotnet restore ncover.cs`. Sometimes you have to do that twice for it to work.

## Arguments

`ncover.cs` takes two positional arguments — a native Windows PE and a .NET assembly — in either order. One must be a native PE (containing exported symbols) and the other must be a .NET assembly (containing imported symbols).

Alternatively, you can use the `--package` option to automatically resolve and download both files from a NuGet package.

**Examples:**

```shell
# Compare local files
./ncover.sh SDL3.dll Sdl3Sharp.Core.dll

# Compare from a NuGet package (latest stable version)
./ncover.sh --package Sdl3Sharp

# Compare from a NuGet package with a version range
./ncover.sh --package "Sdl3Sharp@1.0.*-*"
```

## Options

### Input options

| Option | Short | Description |
| --- | --- | --- |
| `FIRST-PE` | | Path to the first PE file. Either a native PE or a .NET assembly. |
| `SECOND-PE` | | Path to the second PE file. Either a native PE or a .NET assembly. |
| `--package` | `-p` | A NuGet package ID with an optional version range (e.g., `Sdl3Sharp@1.0.*-*`). Can be used instead of providing `FIRST-PE` and `SECOND-PE`. |
| `--prefer-x64` | `-x64` | When using `--package`, prefer x64 RID‑specific packages over x86. Default: `true`. |
| `--nuget-source` | | NuGet feed URL to use when looking up packages. Default: `https://api.nuget.org/v3/index.json`. |

> [!NOTE]
> You must specify either `--package` or both positional arguments. They cannot be combined.

### Filtering options

| Option | Short | Description |
| --- | --- | --- |
| `--exclude-file` | `-x` | Path to a file containing symbol names to exclude (one per line). Lines support `#` comments. |
| `--exclude` | `-X` | Symbol names to exclude. Can be specified multiple times. |
| `--min-severity` | `-m` | Minimum severity level to include in the report. One of: `All`, `Slight`, `Warn`, `Error`, `None`. Default: `All`. |

### Severity options

| Option | Short | Description |
| --- | --- | --- |
| `--warn-as-error` | `-e` | Treat warnings as errors. |
| `--slight-as-warn` | `-w` | Treat slight issues as warnings. |

### Output options

| Option | Short | Description |
| --- | --- | --- |
| `--verbosity` | `-v` | Verbosity level: `None`, `Default`, `Verbose`, or `Json`. Default: `Default`. |
| `--json-output` | `-j` | Path to a file where the report will be additionally saved in JSON format. |
| `--pretty` | | Pretty‑print JSON output (applies to `--json-output` and `--verbosity Json`). |
| `--no-unicode` | `-u` | Use ASCII symbols instead of Unicode in standard output. |
| `--no-ansi` | `-a` | Disable ANSI escape codes (colored output) in standard output. |

## License

Licensed under the MIT License. See [LICENSE.md](LICENSE.md) for details.
