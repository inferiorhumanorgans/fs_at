# At calls for Rust

The Rust standard library does not (yet) offer at filesystem calls as a core
feature. For instance `mkdirat`. These calls are essential for writing race-free
filesystem code, since otherwise the state of the filesystem path that
operations are executed against can change silently, leading to TOC-TOU race
conditions. For Unix these calls are readily available in the libc crate, but
for Windows some more plumbing is needed, which this crate provides.

## Usage

See the crate [docs](https://docs.rs/fs_at).

## Vs other crates

### openat

[openat](https://docs.rs/openat) is a nice wrapper around the Unix *at
facilities. It doesn't offer Windows support, and it also requires adoption of a
new Dir struct which owns the fd - which adds friction for interop with the rest
of std.

### cap_std

[cap_std](https://docs.rs/cap-std) is a lovely rethink of many system
interactions as operations on capabilities. Even more than openat, it steps away
from the familiar std APIs and instead provides its own comprehensive ecosystem.

Unfortunately that doesn't use the full capabilities of the underlying OS - it
layers on top of Rust's own IO stack in some cases (e.g. Windows, some
non-Linux), leading to TOCTOU concerns. That is obviously fixable over time - if
you want a high level API that will make insecure usage hard, I think cap-std is
perfect.

The goal of fs_at isn't to reframe how we do IO though - but just to surface
these important calls in an ergonomic way. Perhaps cap_std could layer on fs_at
when it is finished.

## Contributing

PR's as normal on Github.

## Code of conduct

Please note that this project is released with a Contributor Code of Conduct. By
participating in this project you agree to abide by its terms.
