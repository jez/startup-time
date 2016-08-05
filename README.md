# Startup Time

An attempt to determine which languages are practical (or impractical) to use for CLI commands.

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
- [Links](#links)
- [Author](#author)
- [Copyright and License](#copyright-and-license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Description

This benchmark game executes a series of programs which print the line "Hello, world!". It records the fastest time for each program and prints a sorted table of the times after each run.

## Example Output

    Test        Time (ms)
    C                0.45
    LuaJIT           0.72
    Go               0.78
    Rust             0.82
    Crystal          1.53
    Perl             1.70
    Bash             3.23
    Python 2 -S      4.69
    Python 2        11.55
    Python 3 -S     14.94
    Python 3        31.35
    Ruby            45.99
    Java            54.60
    Node.js         60.12
    Kotlin          92.58
    Scala          323.81

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

## Links

* [100 milliseconds](http://cogsci.stackexchange.com/questions/1664/what-is-the-threshold-where-actions-are-perceived-as-instant)
* [The Great Startup Problem](http://mail.openjdk.java.net/pipermail/mlvm-dev/2014-August/005866.html)

## Author

[chocolateboy](mailto:chocolate@cpan.org)

## Copyright and License

Copyright © 2015-2016 by chocolateboy

This benchmark game is free software; you can redistribute it and/or modify it under the
terms of the [Artistic License 2.0](http://www.opensource.org/licenses/artistic-license-2.0.php).
