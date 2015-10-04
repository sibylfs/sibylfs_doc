# Executing test scripts to obtain traces

**WARNING!!! Executing test scripts directly on your machine has the
potential to destroy your data.** For example, as superuser, you
probably do not want to run a test which successfully removes the root
directory of your filesystem. To avoid such unfortunate events, the
tools run all tests in a chroot environment, so you should be reasonably
safe. However, because the test environment's isolation measures may
have defects, it is recommended that you run all tests in virtual
machines or installations which you can afford to lose.

## Prerequisites

The following assumes you have installed SibylFS, and that the
executable `fs_test` is available on your path.

## Executing the scripts

The test executor executes the test scripts and records the calls and
return values at the libc interface. To see the manpage:

    fs_test exec --help

To run a short sequence of `mkdir` tests, on Linux ext4, execute the
following in directory `sibylfs_test_suite/`:

    fs_test exec --suites test-suite --suite=mkdir --model ext4 2> summary.err.md
    
You need to be root to run this in order to have the ability to mount
file systems and create and destroy temporary users and groups. As a
non-superuser, you could run:

    sudo sh -c "env \"PATH=$PATH:\$PATH\" fs_test exec --suites test-suite --suite=mkdir --model ext4 2> summary.err.md"

If this executes successfully, there should be a directory named
something like `2015-07-10_KvD` (the date, followed by a random string
of 3 letters which serves as a unique identifier). This will be owned by
root, so you need to type:

    sudo chown -R username:group 2015-07-10_KvD

Replacing `username` and `group` with your username and personal group,
respectively.

Then you should be able to see the trace files by typing `find 2015-07-10_KvD/`:

```
2015-07-10_KvD/
2015-07-10_KvD/ext4_loop
2015-07-10_KvD/ext4_loop/mkdir
2015-07-10_KvD/ext4_loop/mkdir/adhoc_mkdir_link_count-int.trace
2015-07-10_KvD/ext4_loop/mkdir/mkdir___mkdir_empty_dir1_____0777-int.trace
2015-07-10_KvD/ext4_loop/mkdir/mkdir___mkdir_empty_dir1___0777-int.trace
[...]
2015-07-10_KvD/ext4_loop/mkdir/results
2015-07-10_KvD/ext4_loop/mkdir/results/exec_adhoc_mkdir_link_count-int.trace
2015-07-10_KvD/ext4_loop/mkdir/results/exec_adhoc_mkdir_link_count-int.trace.err
[...]
2015-07-10_KvD/ext4_loop/mkdir/results/exec_mkdir___mkdir_nonexist_dir__nonexist_3___0777-int.trace.err
2015-07-10_KvD/index.traces
```

Note the directory structure (`.../mkdir/` which contains the original
scripts and `.../mkdir/results/` which contains the executed traces) and
existence of the file `index.traces`.
