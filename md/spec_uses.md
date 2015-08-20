# Possible uses of the spec (motivation)

(Meta: what should the structure of the following uses be?)

This section is to record possible uses of the spec.

  * Determinized, can be used as a posix filesystem, or a Linux or Mac filesystem e.g. under FUSE
  
  * Can be used as a formal component when proving facts about applications that use the filesystem

  * Can be used as a specification for a verified filesystem implementation
  
  * Maybe could be used as a POSIX compatibility layer on top of e.g. windows
    * or on top of other non-traditional filesystem implementations



  * Could be used to identify application call sequences that may
    behave differently on different platforms (without the developer
    needing access to those platforms)
  
  * Could identify non-POSIX call sequences from applications (i.e. a
    special state is encountered)
  
  * Could dynamically check behaviour from an underlying filesystem
  
  
    
  * Could be used in crash testing, to identify post-crash states that
    do not correspond to a state of the spec (probably this needs a
    dependency model)



  * As a formal counterpart, or possibly (with much annotation) replacement for (parts of) POSIX
    * as a readable? counterpart to posix
    * as a supplement to eg Stevens, showing the actual real-world
      behaviour (Stevens is simplified considerably)
    



  * As a source for identifying and categorizing weird real-world (Linux, Mac) behaviours
  
  * For assessing the divergence between given platforms (combinations of libc, os and fs)
    * but note this doesn't really need the spec - testing would establish this
    * but the spec can allow a more sophisticated analysis of the divergences
  
  * To identify a core, portable fs api (such that the behaviour is
    "the same" across all known platforms; the problem here is that
    such a thing would probably not be very useful since the feature
    set would be so limited eg no symlinks, restricted open flags etc)



  * For assessing the coverage eg of the opengroup test suite (or other test suite)

  * As a filesystem test suite, for assessing the POSIX-compliance of
    new/existing filesystems/systems



  * To inform development of a "better" filesystem interface

  * To verify that applications behave the same
    * For example, a version of the linux spec could be used to
      simulate building the linux kernel, and a version of the posix
      spec could be used to simulate the same thing; the end results
      could be compared to check that the end results were the same;
      this would identify whether eg whether any non-posix features
      were used (eg undefined behaviours etc)


  * Mechanical spec means that, in principle, it is possible to
    mechanically derive desired variants of the spec, or to
    mechanically confirm or refute (probably driven by the user) any
    stated property of the spec
    * eg a spec covering the common intersection of posix, linux and
      mac (which api uses result in the same behaviour on all
      systems); or the intersection "up to different errors" (or some
      equivalence relation on errors)
    * eg possible to show that for a command such as link on mac os x,
      the two paths are resolved in different ways (contra expectation
      that path resolution depends just on the command, and not which
      path is being resolved)
      
