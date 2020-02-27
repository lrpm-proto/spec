[![Build Status](https://travis-ci.org/lrpm-proto/spec.svg)](https://travis-ci.org/lrpm-proto/spec)

# LRPM Protocol Specification

**CURRENTLY IN DEVELOPMENT**

The LRPM Protocol (**Lightweight RPC and PubSub Message Protocol**) is inspired by WAMP (Web Application Messaging Protocol),
however is heavily stripped down to be more generic with more application 
specific details.

## Definitions

Messages, types and standard values are defined in `src/definitions.toml` for use in both code and documentation generation.

## Developing the Documentation

First install **mdBook** and the **Tera preprocessor**.

```text
cargo install mdbook mdbook-tera
```

Then run `mdbook build` or `mdbook serve` within this project.
