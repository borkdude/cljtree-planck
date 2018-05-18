
# cljtree-planck

A ClojureScript version of `tree` in Planck.
Ported from https://github.com/borkdude/cljtree-graalvm.

## Credits

This repo is inspired by:

- https://github.com/lambdaisland/birch

## Usage

![Usage](cljtree.gif)

Note that the GIF was recorded using the GraalVM version which runs much faster,
but the tool works exactly the same.

## Options

- `--color` or `-c`: colorize output
- `--edn` or `-E`: output EDN

The path argument is optional and will default to the current directory.

## Run

- Install [planck](https://github.com/planck-repl/planck)
- Clone this repo
- Run `./cljtree`
