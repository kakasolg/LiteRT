# LiteRT (SPM fix fork)

This branch is a minimal, Swift-Package-Manager-consumable snapshot of
[google-ai-edge/LiteRT](https://github.com/google-ai-edge/LiteRT).

## Why this exists

The upstream `Package.swift` declares:

```swift
.binaryTarget(name: "CLiteRT", path: "prebuilt/CLiteRT.xcframework.zip")
.binaryTarget(name: "CLiteRT_mac", path: "prebuilt/CLiteRT_mac.xcframework.zip")
```

but `prebuilt/CLiteRT.xcframework.zip` has never existed in any upstream tag
or release, so `swift package resolve` fails at checkout for any project
that adds the upstream repo via SPM
(see [google-ai-edge/LiteRT#8521](https://github.com/google-ai-edge/LiteRT/issues/8521)).

This branch fixes that by:

1. Actually committing the built `CLiteRT.xcframework.zip` /
   `CLiteRT_mac.xcframework.zip` binaries at the path `Package.swift` expects.
2. Trimming the tree to just what the Swift package needs
   (`Package.swift`, `LICENSE`, `prebuilt/*.xcframework.zip`,
   `litert/swift/Sources`, `litert/swift/Tests`). The full monorepo contains
   git-lfs-tracked Android/other native binaries that are irrelevant to the
   Swift package and that SwiftPM's local-mirror checkout mechanism cannot
   smudge from git-lfs, which breaks `swift package resolve` even when the
   objects exist upstream. Dropping those files avoids that failure mode
   entirely for this package.

## Usage

```swift
.package(url: "https://github.com/kakasolg/LiteRT.git", revision: "<tag>")
```

See tags matching `vX.Y.Z-spm-fix.N` for pinned snapshots built from the
corresponding upstream release.
