# Checking traces

The test checker takes the traces from executing the test scripts and
checks them against the SibylFS model. The results are checked traces,
in various formats. The command to check traces is: `fs_test check`. To
see the manpage:

    fs_test check --help

An example invocation is:

    fs_test check linux_spec 2015-07-10_KvD 2> check_summary.err.md
    
Where `2015-07-10_KvD` is the path to the traces generated previously.

