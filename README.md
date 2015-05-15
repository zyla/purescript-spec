# purescript-spec

purescript-spec is a simple testing framework for Purescript using NodeJS. It's
inspired by [hspec](http://hspec.github.io/).

<img src="https://raw.githubusercontent.com/owickstrom/purescript-spec/master/example.png" width="400" />

## Usage

```bash
bower install purescript-spec
```

### Example

```purescript
module Main where

import Control.Monad.Aff
import Test.Spec (describe, pending, it)
import Test.Spec.Node
import Test.Spec.Assertions
import Test.Spec.Reporter.Console
import Test.QuickCheck

additionSpec =
  describe "Addition" do
    it "does addition" do
      (1 + 1) `shouldEqual` 2
    it "fails as well" do
      (1 + 1) `shouldEqual` 3

main = runNode [consoleReporter] do
  describe "Math" do
    additionSpec
    describe "Multiplication" do
      pending "will do multiplication in the future"
  describe "Async" do
    it "asserts in the future" do
      res <- later' 100 $ return "Alligator"
      res `shouldEqual` "Alligator"
```

*The last test demonstrates how you can use [Aff](https://github.com/slamdata/purescript-aff)
to write async tests.*

### Embedding Specs

In the example `additionSpec` is embedded into the `Math` specification. This
is useful if you want to split specifications into multiple files and combine
them in `Main`.

```purescript
main = suite do
  mathSpec
  stringsSpec
  arraySpec
  ...
```

### Reporters

Reporters can plugged in to the runner, e.g.
`runNode [reporter1, ..., reporterN] spec`. More reporters are planned to be
added in the future. For now there's only [`Test.Spec.Reporter.Console.consoleReporter`](API.md#consolereporter).

### Running Tests

Run the test suite using `psc-make` and NodeJS. Note that `$TESTS`, `$SRC`
and `$LIB` contain all the Purescript source paths needed.

```bash
psc-make -o output/tests $TESTS $SRC $LIB
NODE_PATH=output/tests node -e "require('Main').main();"
```

## QuickCheck

You can use [QuickCheck](https://github.com/purescript/purescript-quickcheck)
together with the [`purescript-spec-quickcheck`](https://github.com/owickstrom/purescript-spec-quickcheck)
adapter to get nice output formatting for QuickCheck tests.

## API

See [API](API.md).

## Build

```bash
# Make the library
make
# Run tests
make run-tests
# Generate docs
make docs
```

### Generate Example

Generating the `example.png` requires:

* phantomjs
* aha
* imagemagick

```
make example.png
```

## CTags

```bash
make ctags
```

## Changelog

* **0.5.0**
  * Make reporters pluggable.
* **0.4.0**
  * Add async support in `it` using `Aff`.

## Contribute

If you have any issues or possible improvements please file them as
[GitHub Issues](https://github.com/owickstrom/purescript-spec/issues). Pull
requests requests are encouraged.

## License

[MIT License](LICENSE.md).
