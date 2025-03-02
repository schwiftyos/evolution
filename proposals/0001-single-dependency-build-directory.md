# Single dependency build directory

* Proposal: [SE-0001](0001-single-dependency-build-directory.md)
* Authors: [RandomHashTags](https://github.com/RandomHashTags)
* Status: **Awaiting implementation**

## Introduction

TBD

## Motivation

Project compilation performance is a core feature of development. The problem is _where_ the build output is stored.

Currently:
- build output is stored in the project's local `.build` directory and only ever accessed by the individual project
- fetching and compilation of every dependency is required for every Swift project (located in the project's local `.build` directory)

## Proposed solution

Create a single system-wide dependency directory that contains all the dependency build outputs ever fetched and compiled.

## Detailed design

Proposed build directory path: `/build/` or `/usr/build/`

Swift shared objects (dynamic products) can be put in `/usr/lib/` when it would be useful to do so.

The `swift package clean` will behave the same (cleaning the local `.build` directory), however we will need to introduce new commands to further clean dependency build outputs in the proposed build directory and remove stale output.

We would need to prioritize the local build output over the proposed build directory because anyone can declare compilation flags which can alter the behavior of the built results. The proposed build directory will contain the default output if it were built as-is (taking into account `Package.swift`).

### Patches

#### [SwiftLang](https://github.com/swiftlang/swift)

<details>
  
<summary>Files of interest</summary>

- https://github.com/swiftlang/swift/blob/main/utils/build_swift/build_swift/constants.py

</details> 

#### [SwiftPM](https://github.com/swiftlang/swift-package-manager)

<details>
<summary>Files of interest</summary>

- https://github.com/swiftlang/swift-package-manager/blob/main/Sources/Workspace/Workspace%2BConfiguration.swift
- https://github.com/swiftlang/swift-package-manager/blob/main/Sources/_InternalTestSupport/SwiftPMProduct.swift

</details>

#### [Swiftly](https://github.com/swiftlang/swiftly)

<details>
<summary>Files of interest</summary>

- https://github.com/swiftlang/swiftly/blob/main/Tools/build-swiftly-release/BuildSwiftlyRelease.swift

</details> 

## Source compatibility

TBD

## ABI compatibility

This proposal is purely an extension of the ABI of the
standard library and does not change any existing features.

## Implications on adoption

This proposal changes how the build system fetches and links dependencies.

Existing products should be unaffected by these changes.

## Future directions

### Linking improvements

A potential future direction would allow shared objects (and additional components) to be distributed between systems, further eliminating fetching and compilation of dependencies.

## Alternatives considered

None
