# Obtaining test scripts

The test scripts are very numerous which causes problems for git. As a
result, the test scripts are stored in a separate repository. 

First, clone the repository: 

    git clone https://github.com/sibylfs/sibylfs_test_suite

Once you have downloaded/cloned the test scripts, you should have a
directory `sibylfs_test_suite/test-suite/`, under which are various
subdirectories such as `check`, `file_descriptors`, and `open`. In the
following, paths starting `test-suite/` refer to this directory.


## Regenerating the test scripts

Along with `fs_test.sh`, SibylFS contains an executable `tgen` used to
generate tests. You need to have this on your path. Then in
`test-suite/` there is a shell script `gentests.sh` which uses
`tgen`. Running this script generates the tests. Type: `./gentests.sh`
