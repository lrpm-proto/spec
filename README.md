[![Build Status](https://travis-ci.org/lrpm-proto/spec.svg)](https://travis-ci.org/lrpm-proto/spec)

# LRPM Protocol Specification

**CURRENTLY IN DEVELOPMENT**

The LRPM Protocol (**Lightweight RPC and PubSub Message Protocol**) is inspired by WAMP (Web Application Messaging Protocol),
however is heavily stripped down to be more generic with more application 
specific details.


## Developing the Documentation

First download `mdBook` and the Tera preprocessor.

```text
cargo install mdbook
cargo install mdbook-tera
```

Then run `mdbook build` or `mdbook serve` within this project.
