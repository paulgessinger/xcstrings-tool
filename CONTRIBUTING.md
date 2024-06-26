# Contributing

## Overview

Contributions to XCStrings Tool are welcome!

- For ideas and questions: [Visit the Discussions](https://github.com/liamnichols/xcstrings-tool/discussions)

- For bugs: [Open an Issue](https://github.com/liamnichols/xcstrings-tool/issues/choose)

- For contributions: [Open a Pull Request](https://github.com/liamnichols/xcstrings-tool/compare)

## Testing your contributions

To build and run all test cases open XCStrings Tool with Xcode and hit ⌘U (command + U).

### Snapshot tests in XCStrings Tool

The majority of tests for XCStrings Tool are implemented using snapshot tests. Snapshot tests compare a "snapshot" of the output generated by a previous version of the software with the output generated by a subsequent version. The test is passed if the outputs are the same.  

Snapshot testing is handled by the [`GenerateTests`](https://github.com/liamnichols/xcstrings-tool/blob/main/Tests/XCStringsToolTests/GenerateTests.swift) class. When the snapshot tests are run, XCStrings Tool generates Swift code using the Strings Catalogs in the [__Fixtures__](https://github.com/liamnichols/xcstrings-tool/tree/main/Tests/XCStringsToolTests/__Fixtures__) directory. These files are then compared against the previous snapshots in the [__Snapshots__](https://github.com/liamnichols/xcstrings-tool/tree/main/Tests/XCStringsToolTests/__Snapshots__) directory.

### Dealing with failed snapshot tests

If there is a discrepancy between a generated Swift file and its corresponding snapshot, the relevant snapshot test will fail. This means that either:

* A bug has been introduced; or

* The changes that have been made are expected to generate an "expected diff" (i.e. a new Swift file). In this case you will want to re-record the snapshot by setting `record` to `true`:

`record: Bool = false,` [(source)](https://github.com/liamnichols/xcstrings-tool/blob/main/Tests/XCStringsToolTests/GenerateTests.swift#L55)


### Viewing the generated Swift files

If you want to view the Swift files generated by the snapshot tests:

1. Comment out the following line in the trailing closure taken by `addTearDownBlock` in GenerateTests.swift:

`try? fileManager.removeItem(at: outputURL)` [(source)](https://github.com/liamnichols/xcstrings-tool/blob/main/Tests/XCStringsToolTests/GenerateTests.swift#L102)

2.  Rerun the tests using command + U in Xcode.

This will prevent the generated Swift files from being removed after the tests are run. The filepath for these files will be printed in the console.

> [!NOTE]
> By commenting out this line you will be generating potentially unwanted Swift files every time you run the snapshot tests.

### Benchmarking

Benchmarks are setup using the [package-benchmark](https://github.com/ordo-one/package-benchmark) package but they are excluded by default. To run benchmarks locally, you must ensure that `BENCHMARK_PACKAGE=true` and `BENCHMARK_DISABLE_JEMALLOC=true` are set in the environment. 

```bash
$ BENCHMARK_PACKAGE=true BENCHMARK_DISABLE_JEMALLOC=true swift package benchmark
```

Thanks for your contributions!
