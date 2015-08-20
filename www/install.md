# Installation

The repository for the project is <https://github.com/sibylfs/sibylfs_src>
On Linux, the simplest way to install is probably using prebuilt
binaries via nix (see below). 

<!-- Potentially quicker, but fragile, is to
install "plain binaries".  -->

The most reliable method is probably
"installing from source via nix".


## Installing prebuilt binaries via nix

Nix is a package manager available on Linux (x86-64) and Mac (Darwin,
x86-64) (and possibly others e.g. Linux i686).

  - Install nix following https://nixos.org/nix/ : type 
  

    curl https://nixos.org/nix/install | sh
    
    
  This requires
  root access; alternatives without root are described at
  <https://nixos.org/wiki/How_to_install_nix_in_home_(on_another_distribution)>
    
    
  - Follow the instructions to make sure your environment is set up: 
  

    . <your home directory>/.nix-profile/etc/profile.d/nix.sh

  - Download the prebuilt binaries for your platform. Go to the binary releases page <https://github.com/sibylfs/sibylfs_binaries/releases>, select the newest release, and download the closure file that corresponds to your architecture:
    - Linux 64-bit binaries: `sibylfs_nix_linux_amd64.closure`
    - Mac OS X 10.9.3 64-bit binaries (may work on other platforms): `FIXME_nix_mac.closure`
    
  - Place the binaries in the nix store: 


    nix-store --import <   [path to downloaded binay]
    
   - This will print various entries; the last entry will be a path to sibylfs, something like:
    `/nix/store/f77518afhrq1jsih7rgr5y2gc9v8zsi3-fs_test`; now install the binaries into your path: 


    nix-env -i /nix/store/f77518afhrq1jsih7rgr5y2gc9v8zsi3-fs_test  # N.B. replace this with the path from the import!


## Installing from source via nix

  - Install nix as above
  
  - Use git to clone the SibylFS source code:
  
  
    git clone https://github.com/sibylfs/sibylfs_src
    
  - This will create a directory `sibylfs_src`. In the following, paths starting
    with `sibylfs_src/` refer to this directory.

  - Change to the `sibylfs_src/` directory: 
  
  
    cd sibylfs_src/
    
  - Build (this may take a long time) and install SibylFS by typing: 
  
  
    nix-env -f ./default.nix -i sibylfs
    

If you want to inspect the build result, type `nix-build` . This
should create a directory `result` which contains the spec build, the
tools build, and the executables. To install the result, type `nix-env
-i ./result`

An example install is as follows (this uses `nix-build` to build, followed by `nix-env -i ./result` to install):

[![asciicast](https://asciinema.org/a/c4nxhmnn1ctsi1w1615wzgrrf.png)](https://asciinema.org/a/c4nxhmnn1ctsi1w1615wzgrrf)



## Installing plain binaries (2015-08-20 FIXME not working)

***This option is not really supported***, but is probably the
quickest way to install (if it works). Go to
<https://github.com/sibylfs/sibylfs_binaries/releases/tag>, select the
latest tag, and download the `.tar.gz` file corresponding to your OS
and architecture (eg `sibylfs_linux_amd64.tar.gz` for Linux, 64
bit). Unpack this file:

    tar xvzf <file you downloaded>
    
This should create a directory `sibylfs/bin` containing the
executables. You should place these on your path (or execute from
inside that directory).




## Installing from source using OCaml, ocamlfind and opam

***Because of the large number of dependencies, this method is quite
   fragile. ***

First, you need to install ocaml and ocamlfind and opam. Perhaps the
easiest is to install opam (which includes ocaml and ocamlfind)
following <https://opam.ocaml.org/doc/Install.html> using the binary
installer. Any version of ocaml >= 4.01.0 should work. For example:

  * Download `opam_installer.sh` 
  
  * Type `sh ./opam_installer.sh /usr/local/bin 4.02.1` to install opam. 
  
  * Type `opam install ocamlfind` to install ocamlfind


The SibylFS specification is written in Lem. To build Lem:

  * Clone the Lem repository, branch fs, using git: `git clone -b fs
    https://tomridge@bitbucket.org/tomridge/lem.git` . This will create
    a directory `lem`. In the following paths starting `lem/` refer to
    this directory.
    
  * Change to the lem directory: `cd lem/` 
  
  * Build Lem and the OCaml libraries: `make lem ocaml-libs`

<!-- FIXME ref to repo in following -->
Now you need to get the SibylFS source.

  * Use git to clone the SibylFS source: `git clone
    https://github.com/sibylfs/sibylfs_src`. This will create a directory
    `sibylfs_src`. In the following, paths starting with `sibylfs_src/` refer to this
    directory.

The `sibylfs_src/` directory contains two particular subdirectories: `fs_spec`
(the specification) and `fs_test` (the test tools). To build `fs_spec` you need the following:

  * ocaml tool cppo: `opam install cppo`
  
  * ocaml libraries sha, sexplib: `opam install sha sexplib`
  
  * Lem: link `lem/` under `fs/src_ext`: `cd fs/src_ext && ln -s <path to lem/> ./lem`

To build `fs_test` you need: 

  * ocaml tools camlp4 and menhir: `opam install camlp4 menhir`
  
  * ocaml libraries cmdliner, fd-send-recv, cow: `opam install cmdliner fd-send-recv cow`

If you have opam, you can install ocamlfind, sha, cppo, cmdliner,
fd-send-recv, camlp4, sexplib, menhir and all of their transitive
dependencies with `make dep` in the `sibylfs_src/` directory.

Having installed these dependencies, you should now be able to build SibylFS:

  * In `sibylfs_src/` type: `make`

When this is complete, you need to add the executables in `sibylfs_src/fs_test`
to your path, e.g. by typing:

    export PATH=<absolute path to sibylfs_src>/fs_test:$PATH

<!-- FIXME test these source-compile instructions -->


## After installation

SibylFS provides several command-line tools. These are typically
accessed indirectly by the top-level `fs_test.sh` script. For example,
`fs_test.sh --help` displays a man page showing the various
subcommands available (check, clean, exec etc.). Further information
is provided eg for the check subcommand by typing `fs_test.sh check
--help`.





