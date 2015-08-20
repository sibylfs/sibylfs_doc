# Testing

## Test generation (`testgen`)

### Testing simple commands such as `link`, `rename`

We want to exhaustively check the behaviour of a command such as
`mkdir` command. This command takes a single path parameter:

    mkdir(path)
    
However, as a function we know that there are several hidden parameters:

    mkdir(pid,s0,path)
    
Of course, we cannot hope to exhaustively check the behaviour for all
states, and we shall ignore the `pid` argument for now. How can we
exhaustively test the path argument? Clearly the behaviour depends on
the state. Let's assume that we have some state `s0` such that it is
possible to exhaustively check all "essentially different" behaviours
by varying the path. (Not clear whether a single state suffices: one
might imagine that whether path resolution starts in the root, or a
subdir of root, might well make a difference; best to start not in the
root, on the basis that absolute paths immediately resolve to the root
anyway.) What paths should we use to check `mkdir(s0,path)`?

Factors that might influence the behaviour of the command are as follows:

- whether a path ends in a slash or not
- whether a path starts with a single slash
- whether a path starts with two slashes
- whether a path starts with more than two slashes
- whether the path is empty
- whether the path is "/"
- whether the path identifies something existing or not; if existing,
  whether the something is a file or dir (or symlink); if existing
  dir, whether the dir is empty or not
- whether a path component is a symlink or not (and if so, if broken,
  absolute, relative, empty... effectively all the factors that are
  relevant to a path are relevant to a link)
- also the presence of .. and . in paths might make a difference

For two arguments, the possible interactions are:

- whether f1 is equal to f2
- whether f1 is a proper prefix of f2 (or f2 is a prefix of f1); NB if
  f1 is a proper prefix of f2, if f2 exists then f1 exists; (prefix is
  based on lists of names ie there is no relation between a/alf and
  a/alfie, but there is a relation between a/alf and a/alf/bert)
- if f1 < f2, and f2 exists, then f1 exists; if f1 < f2 and f1 doesn't
  exist, then f2 doesn't exist
- the other thing that might matter is whether the number of links is
  1 or 2 or more; in general the hard-link structure might matter
- another thing that might make a difference (it did in our faulty
  spec!) is if the source and target are different files in the same
  dir (in a correct implementation this shouldn't make a difference)
- another thing that may matter: if a dir contains 1 file (so, 0 1 2
  and 2+ are probably the things that matter)
- links may matter, eg if two different paths have the same underlying
  inode
- the fact that two paths have a common prefix shouldn't matter eg
  /a/b/c.txt and /a/d/e.txt should behave as /b/c.txt and /d/e.txt
  (after all, both have / as a common prefix); principle: from a
  subdir things look the same (behave the same) as from the root;
- for a file /a/b/c.txt, if there is a file /a/d.txt it shouldn't
  affect things - commands act "pointwise", and contents of
  directories on the route to a file shouldn't affect things



## Running tests as normal user

## Running tests as root

## Running `permissions_root` tests, docker

We used to run tests as a "normal user". Recently David's test
infrastructure runs as root because eg it needs to create and mount
filesystems. 

The following text is likely only an approximation to the truth.

The tests under `permissions_root` are from the time when we ran tests
as a "normal user". The `permissions_root` tests typically exercise
the behaviour of many processes, with different ids, group membership
etc. The test-executor typically runs as a single process. In order to
simulate multiple processes with different ids, the test-executor
initially runs as root, and changes to a particular id before
executing a command.

At the moment (2014-10-08) it is likely that these tests are not
executing as expected, because it is likely that various users and
groups need to be present on the system before a test can run. Thomas
had constructed a docker environment in which to run the tests, and
this is hosted on pc1008. Unfortunately there is not much
documentation. I have two emails from Thomas that are relevant:

---

~~~
Subject: testing perms
Date: 2014-07-01

Hi,

all that can be done without root permission is in the test-suites.
The rest exists, but I have not run these tests for a few weeks now
and need to work a bit on the infrastructure to run them decently.

Best



Thomas

On 01/07/14 13:16, Tom Ridge wrote:
> Hi Thomas,
>
> What is the state of permission testing?
>
> Thanks
>
> T
~~~

---

~~~
Subject: How to run root-tests
Date: 2014-07-10

Hi Tom,

I started implementing the permission test that need root permission.
Essentially it is still as yesterday, except that now the results end up
in directory "/home/fsuser/results". So

1) ssh fsuser@pc1008
2) ./run_fstests
3) check state in the virtual session, e.g. have a look at the files in /tmp
4) close docker session
5) copy logs from /home/fsuser/results to where you want them


To get different behaviour, modify "docker.sh"

Best
~~~

---

The next task is to get these test files and docker setup into the
repository, and try to reproduce the testing Thomas was doing.


### Docker instructions

We run the main `fs_test` testing command as root. Typically we run
this in a vm so it doesn't destroy our host filesystem.

Docker provides a container, similar to a vm. We can use docker to run
some of our tests. Because docker is not a proper vm, it is not clear
that the tests run on docker give the same results as running in a vm.

The approach is as follows:

  - set up host system to support docker
  - create a docker container, following <https://github.com/tomjridge/dockertest/tree/master/ubuntu_ocaml>
    - this mounts the host `fs/` directory into the docker container
  - type `make run_image` to run the docker container
  - change to `fs_test/docker` directory and run `docker.sh` to run the `permissions_root` tests
  - check the results to see what happened
