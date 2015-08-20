# Generating HTML output

The output from checking is not very user friendly. To generate HTML, run the following:

    fs_test.sh html ./2015-07-10_KvD
    
This will print a message similar to:

    Browse to file:///mnt/sdb1/tom/bitbucket/fs/./2015-07-10_KvD/html/index.html
    
Opening this link should display a page similar to <http://sibylfs.github.io/sibylfs_examples/2015-07-10_KvD/html/index.html>

For a single filesystem (say, ext4 on Linux), a full check run with HTML is about 1.2GB. An example full run is <http://sibylfs.github.io/sibylfs_examples/2015-07-24_y2I/html/>
