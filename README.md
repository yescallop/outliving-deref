# borrow-or-share

Traits for either borrowing or sharing data.

[![crates.io](https://img.shields.io/crates/v/borrow-or-share.svg)](https://crates.io/crates/borrow-or-share)
[![license](https://img.shields.io/github/license/yescallop/borrow-or-share?color=blue)](/LICENSE)

See below for a basic usage of the crate.
See the [documentation](https://docs.rs/borrow-or-share) for a detailed walkthrough.

## Basic usage

Suppose that you have a generic type that either owns some data or holds a reference to them.
You can use this crate to implement on this type a method taking `&self` that either borrows from `*self`
or from behind a reference it holds:

```rust
use borrow_or_share::BorrowOrShare;

struct Text<T>(T);

impl<'i, 'o, T: BorrowOrShare<'i, 'o, str>> Text<T> {
    fn as_str(&'i self) -> &'o str {
        self.0.borrow_or_share()
    }
}

// The returned reference is borrowed from `*t`.
fn owned_as_str(t: &Text<String>) -> &str {
    t.as_str()
}

// The returned reference is borrowed from `*t.0`.
fn borrowed_as_str<'a>(t: &Text<&'a str>) -> &'a str {
    t.as_str()
}
```

## Credit

Credit goes to [@beepster4096](https://github.com/beepster4096) for figuring out a safe version of the code.
