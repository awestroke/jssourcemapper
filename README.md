# js-source-mapper

A rust library for consuming JavaScript source maps with a focus on performance.
Supports [Source Map revision 3](https://docs.google.com/document/d/1U1RGAehQwRypUTovF1KRlpiOFze0b-_2gc6fAH0KY0k/edit).

[![Build Status](https://travis-ci.org/awestroke/js-source-mapper.svg?branch=master)](https://travis-ci.org/awestroke/js-source-mapper)
[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](./LICENSE)

## Usage

```toml
[dependencies]
js-source-mapper = "0.1.0"
```

```rust
extern crate js_source_mapper;

use js_source_mapper::{Cache, consume};

fn main() {
  let cache = consume(r#"{
    "version": 3,
    "file": "foo.js",
    "sources": ["source.js"],
    "names": ["name1", "name1", "name3"],
    "mappings": ";EAACA;;IAEEA;;MAEEE",
    "sourceRoot": "http://example.com"
  }"#).unwrap();

  let mapping = cache.mapping_for_generated_position(2, 2);
  assert!(mapping.original.line == 1);
  assert!(mapping.original.column == 1);
  assert!(mapping.original.source == "source.js".into());
  assert!(mapping.original.name == "name1".into());
}
```
