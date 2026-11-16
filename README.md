# LiteRT (SwiftPM fix fork) — archived

> **No longer needed.** Google now publishes an official Swift package without Git LFS files:
> [google-ai-edge/LiteRT-Swift](https://github.com/google-ai-edge/LiteRT-Swift). Use it instead of this fork:
>
> ```swift
> .package(url: "https://github.com/google-ai-edge/LiteRT-Swift", from: "2.3.0")
> ```
>
> The product names are the same (`LiteRT`, `LiteRT_static`, `LiteRT_dynamic`, `LiteRtMetalAccelerator`, `CLiteRT_static`,
> `TensorFlowLite`); refer to them as `.product(name: "LiteRT", package: "LiteRT-Swift")`. See
> [google-ai-edge/LiteRT#8521](https://github.com/google-ai-edge/LiteRT/issues/8521) and the
> [LiteRT Swift README](https://github.com/google-ai-edge/LiteRT/blob/main/litert/swift/README.md#adding-litert-to-your-app).
> LiteRT-Swift 2.3.0 downloads the same `v2.3.0` xcframework release assets that this fork's `2.3.2-spm-fix.1` points at.
> This repository is archived and kept read-only so existing `2.3.2-spm-fix.1` pins still resolve.


A minimal snapshot of [google-ai-edge/LiteRT](https://github.com/google-ai-edge/LiteRT) that Swift Package Manager
can resolve. Personal workaround, not affiliated with the LiteRT team.

## Why this exists

Adding the upstream repository as a SwiftPM dependency fails at checkout
([google-ai-edge/LiteRT#8521](https://github.com/google-ai-edge/LiteRT/issues/8521)). The monorepo stores
Android/desktop native binaries (`litert/prebuilt/*/*.so`, `.dylib`, ...) in Git LFS. They have nothing to do with
the Swift package, but SwiftPM still has to check them out:

- without git-lfs, current SwiftPM stops with `error: Git LFS is not installed` (checked with upstream `2.3.2`,
  Swift 6.4, 2026-11);
- with git-lfs, the checkout goes through SwiftPM's local bare-mirror cache, where the LFS smudge step can fail.

This snapshot keeps only what the Swift package needs, so there is nothing for LFS to fetch.

## What changed from upstream

Nothing in the files themselves. This tag is upstream `v2.3.2` trimmed to `Package.swift`, `LICENSE` and
`litert/swift/`, plus this README.

Since LiteRT 2.3.0, upstream publishes the xcframeworks as release assets and `Package.swift` downloads them by URL
with a checksum, so this snapshot contains no binaries. (Upstream `v2.3.2`'s `Package.swift` points at the
`v2.3.0` release assets; that is upstream's choice and is kept as is.) All five checksums were verified against the
downloaded assets with `swift package compute-checksum`.

Older tags (`v2.2.0-spm-fix.*`) predate the release assets and commit the xcframework zips directly.

## Usage

```swift
.package(url: "https://github.com/kakasolg/LiteRT.git", exact: "2.3.2-spm-fix.1")
```

Products are the same as upstream: `LiteRT`, `LiteRT_static`, `LiteRT_dynamic`, `LiteRtMetalAccelerator`,
`CLiteRT_static`, `TensorFlowLite`.

## License

Apache 2.0, same as upstream (see `LICENSE`).
