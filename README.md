# Control-CLI
**Controlling a device by interacting with its Command Line Interface(CLI)**

A Command Line Interface (CLI) is an interface where the user is presented with a command prompt and has to enter ASCII commands to drive or control or configure that device.
That interface could be the shell on a Unix system or some other command interpreter on a device such as an ethernet switch or an IP router or some kind of security appliance.
This module is useful to control/automate/script a remote device by interacting with its CLI interface remotely over any type of connection which can be used to reach the CLI interface of that remote device.
Control::CLI supports any of Telnet, SSHv2 (using an interactive shell channel) and Serial Port connections.

Much of the functionality of this module is in fact similar (and in some cases identical) to that offered by the popular Net::Telnet module. The real benefit of this module is that connection and basic I/O can be performed in a consistent manner regardless of the underlying connection type thus allowing CLI based scripts to be easily converted between or operate over any of Telnet, SSH or Serial port connection. Furthermore this module is capable of operating in a non-blocking fashion for all its methods and is thus able to drive multiple hosts simultaneusly without resorting to Perl threads.

To do so, Control::CLI relies on these underlying modules:

- Net::Telnet for Telnet access
- Net::SSH2 for SSH access
- IO::Socket::IP for IPv6 support as well as non-blocking TCP socket setup (IPv4 and IPv6)
- Win32::SerialPort or Device::SerialPort for Serial port access respectively on Windows and Unix systems

Since all of the above are Perl standalone modules (which do not rely on external binaries) scripts using Control::CLI can easily be ported to any OS platform (where either Perl is installed or by simply packaging the Perl script into an executable with PAR::Packer's pp). This is a big advantage for portability to Windows platforms where using Expect scripts is usually not possible.

Each of the above modules is optional (they are not required to install Control::CLI), however if one of the modules is missing then no access of that type will be available. For instance if Win32::SerialPort is not installed (on a Windows system) but both Net::Telnet and Net::SSH2 are, then Control::CLI will be able to operate over both Telnet and SSH, but not Serial port. There has to be, however, at least one of the Telnet/SSH/SerialPort modules installed, otherwise Control::CLI's constructor will throw an error.

Net::Telnet and Net::SSH2 both natively use IO::Socket::INET which only provides IPv4 support; if however IO::Socket::IP is installed, this class will use it as a drop in replacement to IO::Socket::INET and allow both Telnet and SSH connections to operate over IPv6 as well as IPv4.

For both Telnet and SSH this module allows setting of terminal type (e.g. vt100 or other) and windows size, which are not easy to achieve with Net::Telnet as they rely on Telnet option negotiation. Being able to set the terminal type is important with some devices which might otherwise refuse the connection if a specific virtual terminal type is not negotiated from the outset.

Note that Net::SSH2 only supports SSHv2 and this class will always and only use Net::SSH2 to establish a channel over which an interactive shell is established with the remote host. This is typically the only way that SSH is implemented on ethernet switches and IP routers and other appliances. Authentication methods supported are 'publickey', 'password' and 'keyboard-interactive'.

# INSTALLATION

This module was built using Module::Build.

If you have Module::Build already installed, to install this module run the following commands:

	perl Build.PL [TESTPORT=<DEVICE>]
	./Build
	./Build test
	./Build install

Or, if you're on a platform (like DOS or Windows) that doesn't require the "./" notation, you can do this:

	perl Build.PL [TESTPORT=<DEVICE>]
	Build
	Build test
	Build install


If instead you are relying on ExtUtils::MakeMaker then run the following commands:

	perl Makefile.PL [TESTPORT=<DEVICE>]
	make
	make test
	make install

If Win32::SerialPort or Device::SerialPort are installed on the target system, the optional [TESTPORT=<DEVICE>] allows a serial port name to be manually specified for testing the Serial port constructor of Control::CLI, where <DEVICE> can be any of COM1, /dev/ttyS0, etc..
If no serial port is specified via the optional [TESTPORT=<DEVICE>] (e.g. if installing via cpan shell) and Win32::SerialPort or Device::SerialPort are installed on the target system, then the test script will attempt to automatically detect an available serial port to use; however if this detection fails then the serial port constructor tests will simply be skipped. 

Once installed, you can perform tests in interactive mode like this:

	perl t/control-cli.t

To perform interactive tests before installing the module, after build or make, run the test script like this:

	perl -Mblib t/control-cli.t

The test script also accepts command line arguments with these syntaxes:

 	control-cli.t telnet|ssh [username:password@]host ["cmd"] [blocking]
 	control-cli.t <COM-port-name> <baudrate> [username:password] ["cmd"] [blocking]

	where:
		"cmd" = CLI test command to send once connected (quoted if it contains spaces)
		blocking = 0 (test non-blocking mode) or 1 (use regular blocking mode) 

# SUPPORT AND DOCUMENTATION

After installing, you can find documentation for this module with the
perldoc command.

    perldoc Control::CLI

You can also look for information at:

[RT, CPAN's request tracker](http://rt.cpan.org/NoAuth/Bugs.html?Dist=Control-CLI)

[AnnoCPAN, Annotated CPAN documentation](http://annocpan.org/dist/Control-CLI)

[CPAN Ratings](http://cpanratings.perl.org/d/Control-CLI)

[Search CPAN](http://search.cpan.org/dist/Control-CLI/)


# LICENSE AND COPYRIGHT

Copyright (C) 2024 Ludovico Stevens

This program is free software; you can redistribute it and/or modify it
under the terms of either: the GNU General Public License as published
by the Free Software Foundation; or the Artistic License.

See http://dev.perl.org/licenses/ for more information.
