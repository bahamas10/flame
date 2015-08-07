flame(1)
========

Generate a flamegraph the easy way with DTrace and stackvis

Dependencies
------------

The system you are on must support DTrace, and `stackvis` must be installed with

    npm install -g stackvis

Example
-------

Generate a flamegraph and write the output to an HTML file

    # flame zoneadm list -cv > flamegraph.html
    > program is 'zoneadm' - using ustack stack function
    > calling dtrace: "-n" "profile-97 /pid == $target/ { @[ustack(80, 8192)] = count(); }" "-o" "/tmp/flame-dtrace-out.27416.XXXXXXX.txt" "-c" "/tmp/flame-dtrace-prog.27416.XuDa0yn /tmp/flame-dtrace-prog-args.27416.XxDa2yn"
    dtrace: description 'profile-97 ' matched 1 probe
    dtrace: pid 27423 has exited
    > cleaning temporary files
    # scp flamegraph.html webserver:/var/www

Generate a flamegraph and share it on manta

    # flame zoneadm list -cv | stackvis share
    > program is 'zoneadm' - using ustack stack function
    > calling dtrace: "-n" "profile-97 /pid == $target/ { @[ustack(80, 8192)] = count(); }" "-o" "/tmp/flame-dtrace-out.28054.XXXXXXX.txt" "-c" "/tmp/flame-dtrace-prog.28054.Xe3aYSn /tmp/flame-dtrace-prog-args.28054.Xh3a0Sn"
    dtrace: description 'profile-97 ' matched 1 probe
    dtrace: pid 28062 has exited
    > cleaning temporary files
    http://us-east.manta.joyent.com/bahamas10/public/stackvis/e9541153-c253-4b19-bafd-eb4a13ff262b/index.htm

(see http://us-east.manta.joyent.com/bahamas10/public/stackvis/e9541153-c253-4b19-bafd-eb4a13ff262b/index.htm)

Trace `rsyslogd` for 10 seconds and create a flame graph

    # flame -p "$(pgrep rsyslogd)" -t 10 > graph.html

Usage
-----

    $ flame -h
    Usage: flame [-o] [-p pid] [-s stack] [-t secs] prog args

    Generate a flamegraph easily using DTrace and optionally stackvis.
    A program can be specified as the operands to be executed, or an
    already running program can be given by pid with -p.

    Examples

      Generate a flamegraph for zoneadm list and write the HTML to file
        # flame zoneadm list -cv > graph.html

      Generate a flamegraph and share it to Manta for rsyslog for 20 seconds
        # flame -p "$(pgrep rsyslogd)" -t 20 | stackvis share

      Dump unmodified DTrace output to stdout for a node program (raw mode)
        # flame -r node foo.js

    Options

      -h           print this message and exit
      -p <pid>     trace the pid given
      -r           raw mode, don't run stackvis just print what dtrace outputs
      -s <stack>   stack function to use, defaults to stack() or jstack() depend on execname
      -t <secs>    seconds to trace when run with "-p <pid>", defaults to 30

License
-------

MIT License
