# Startup Time

An attempt to determine which languages are practical (or impractical) to use for command-line interface (CLI) programs.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Description](#description)
- [Example Output](#example-output)
- [Prerequisites](#prerequisites)
- [Setup](#setup)
- [Tasks](#tasks)
  - [Run the tests](#run-the-tests)
  - [Clean up temporary files](#clean-up-temporary-files)
- [Contributing](#contributing)
- [Further Reading](#further-reading)
- [See Also](#see-also)
- [Author](#author)
- [Copyright and License](#copyright-and-license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

This benchmark game tests how long it takes to execute ["Hello, world!"](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program) programs written in various languages. It records the fastest time for each program and prints a sorted table of the times after each run. With the exception of the [prerequisites](#prerequisites) listed below, it does not require any of the tested languages to be installed: if a compiler/interpreter is not available, the test is skipped.

## Example Output

    Test          Time (ms)
    C (gcc)            0.37
    LuaJIT             0.59
    Go                 0.78
    Kotlin Native      0.95
    Lua                1.64
    Perl               1.72
    Haskell (GHC)      1.84
    Rust               1.91
    Crystal            1.93
    C++ (g++)          2.31
    Bash               3.62
    Python 2 -S        4.66
    Python 2          12.09
    Python 3 -S       13.74
    Python 3          20.42
    Ruby              41.49
    Java              55.18
    Node.js           66.48
    Kotlin           105.51
    Scala            797.23

## Prerequisites

* Ruby >= 1.9.3
* [tty](https://github.com/peter-murach/tty#installation)

## Setup

    gem install tty

## Tasks

### Run the tests

    rake

Or:

    rake rounds=20 # change the number of times each program is executed (default: 10)

### Clean up temporary files

    rake clean

## Contributing

Patches are welcome, but please make sure the test runs without warnings or errors if the compiler/interpreter is not installed. See the Rakefile for examples.

## Further Reading

* [Response Times: The 3 Important Limits](https://www.nngroup.com/articles/response-times-3-important-limits/)
* [100 milliseconds](http://cogsci.stackexchange.com/questions/1664/what-is-the-threshold-where-actions-are-perceived-as-instant)
* [The Great Startup Problem](http://mail.openjdk.java.net/pipermail/mlvm-dev/2014-August/005866.html)

## See Also

* [gnustavo/startup-times](https://github.com/gnustavo/startup-times "A script to investigate the startup times of several programming languages")
* [jwiegley/helloworld](https://github.com/jwiegley/helloworld "A comparison of Hello, world startup times in various languages")

## Author

[chocolateboy](mailto:chocolate@cpan.org)

## Copyright and License

Copyright © 2015-2017 by chocolateboy

This benchmark game is free software; you can redistribute it and/or modify it under the
terms of the [Artistic License 2.0](http://www.opensource.org/licenses/artistic-license-2.0.php).
