### gen_serial: generic serial port driver for Erlang

[gen_serial](https://github.com/tomszilagyi/gen_serial) is a generic
serial port driver written by [Tom Szilagyi](https://github.com/tomszilagyi)
and [Shawn Pearce](http://blog.spearce.org/2004/02/genserial-01-released.html)
to allow Erlang applications to use the host's RS-232 serial ports
through a single cross platform API.

[This repo](https://github.com/igorclark/gen_serial) just packages up
the POSIX version as a `rebar3` lib with a new `rebar.config`,
`gen_serial.app.src` and a slightly modified `posix/Makefile` so it can
be built as a `rebar` OTP app and source dependency, rather than as an
Erlang system library. These notes have been modified to reflect the
removal of Windows support and the new build/installation mechanism.

Each serial port is managed in its own host OS process, as a C port,
ensuring that the stability of the Erlang node is not compromised by
the `gen_serial` driver.

Currently this driver and API has been tested on Linux. It should also
work on any POSIX compatible (UNIX-like) system.

A simple test module is also provided as `gen_serial_test`. This has some
test cases which can be run in one Erlang node (using a loopback serial
cable with a null modem installed and two serial ports on the host) or
between two networked Erlang nodes (using a serial cable with a null
modem to connect the two hosts).

Building gen_serial on UNIX platforms is pretty simple:

	rebar3 compile

This will compile the Erlang code using `rebar3`, and invoke the
Makefile in `c_src/posix` to compile the POSIX driver backend.

To add this app as a source dependency to your OTP app, add this to
your app's `rebar.config`:

```
{ deps, [
    { gen_serial, { git, "https://github.com/igorclark/gen_serial.git", { tag, "0.1.0" } } }
] }.
```

The backend (native code) is under `c_src` and can be compiled on UNIX.

See [docs](doc/gen_serial.html) for API documentation.

