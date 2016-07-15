# Installation

The repository for the project is <https://github.com/sibylfs/sibylfs_src>.

Nix is a package manager available on Linux (x86-64) and Mac (Darwin,
x86-64) (and possibly others e.g. Linux i686; unfortunately it is not
supported under FreeBSD). SibylFS is also available via nix (see
[Installing with nix](#nix-install)). Nix is likely to be more stable
than opam (ie, you are more likely to be able to build using Nix).

N.B. 2016-07-15 nix changed its hashing function recently;
unfortunately there are two hashing functions now in existence; if you
get a message similar to:

```
output path ‘/nix/store/77hcsyrhmr70dp720jdhxqfvzfcqyvlr-lem-5b4a168’ has r:sha256 hash ‘19642yadi089p8si87n0rbn0mwwxyvzjrv33g848gaqy811i0zak’ when ‘0z1chfa2q4h7rdzabyjjh3zhgdafjhjhxyna5hd6q11ns88v7xds’ was expected
```

then you are probably running into this problem; the fix is on branch
`2016-07-14_fix_nix` from github.

OPAM is the OCaml Package Manager which manages sets of complete OCaml
package development environments per user. It's available on Linux, OS
X, and FreeBSD. See [Installing with OPAM](#opam-install) and [Developing
with OPAM](#opam-develop) for instructions.

On Linux, the simplest way to install is using prebuilt binaries via nix
(see [Installing binaries with nix](#nix-binaries)).

<!-- Potentially quicker, but fragile, is to
install "plain binaries".  -->

## <a name="opam-install"></a> Installing with OPAM

  - Install *opam*

    - [On OS X](http://opam.ocaml.org/doc/Install.html#OSX)

    - [On Ubuntu](http://opam.ocaml.org/doc/Install.html#Ubuntu)

    - [On other operating systems](http://opam.ocaml.org/doc/Install.html)

  - Ensure you have loaded OPAM's configuration into `bash` with


    eval `opam config env`


  - Or another shell with


    eval `opam config env --shell=[sh|csh|zsh|fish]`


  - Run


    opam install sibylfs


## <a name="opam-develop"></a> Developing with OPAM

  - Run


    git clone https://github.com/sibylfs/sibylfs_src
    opam pin add sibylfs sibylfs_src


## <a name="nix-install"></a> Installing with nix

  - Install nix following https://nixos.org/nix/ : type 
  

    curl https://nixos.org/nix/install | sh
    
    
  This requires
  root access; alternatives without root are described at
  <https://nixos.org/wiki/How_to_install_nix_in_home_(on_another_distribution)>
      
  - Use git to clone the SibylFS source code:
  
  
    git clone https://github.com/sibylfs/sibylfs_src
    
  - This will create a directory `sibylfs_src`. In the following, paths starting
    with `sibylfs_src/` refer to this directory.

  - Change to the `sibylfs_src/` directory: 
  
  
    cd sibylfs_src/
    
  - Build SibylFS (this may take a long time) by typing: 
  
  
    nix-build
    
  - Install SibylFS by typing:
  
  
    nix-env -i ./result
    


An example install is as follows:

[![asciicast](https://asciinema.org/a/c4nxhmnn1ctsi1w1615wzgrrf.png)](https://asciinema.org/a/c4nxhmnn1ctsi1w1615wzgrrf)


## <a name="nix-binaries"></a> Installing prebuilt binaries via nix (Linux, Mac)

  - Install nix as above
    
  - Follow the instructions to make sure your environment is set up: 
  

    . <your home directory>/.nix-profile/etc/profile.d/nix.sh

  - Download the prebuilt binaries for your platform. Go to the binary releases page <https://github.com/sibylfs/sibylfs_binaries/releases>, select the newest release, and download the closure file that corresponds to your architecture:
    - Linux 64-bit binaries: `sibylfs_nix_linux_amd64.closure`
    - Mac OS X 10.10.3 (and maybe others) 64-bit binaries: `sibylfs_nix_mac_amd64.closure`
    
  - Place the binaries in the nix store: 


    nix-store --import <   [path to downloaded binay]
    
   - This will print various entries; the last entry will be a path to sibylfs, something like:
    `/nix/store/f77518afhrq1jsih7rgr5y2gc9v8zsi3-fs_test`. Now install the binaries into your path: 


    nix-env -i /nix/store/f77518afhrq1jsih7rgr5y2gc9v8zsi3-fs_test  # N.B. replace this with the path from the import!



## Installing plain binaries (Linux)

***This option is not really supported***, but is probably the
quickest way to install (if it works). Go to
<https://github.com/sibylfs/sibylfs_binaries/releases/>, select the
latest release, and download the `.tar.gz` file corresponding to your OS
and architecture (eg `sibylfs_linux_amd64.tar.gz` for Linux, 64
bit). Unpack this file:

    tar xvzf <file you downloaded>
    
This should create a directory `sibylfs/bin` containing the
executables. You should place these on your path (or execute from
inside that directory).


## After installation

SibylFS provides several command-line tools. `fs_test --help` displays a
man page showing the various subcommands available (check, clean, exec
etc.). Each subcommand also has online help available. For instance, for
the check subcommand usage information is available via `fs_test check --help`.



