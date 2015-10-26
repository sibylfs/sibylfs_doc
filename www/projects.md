# Possible projects using SibylFS

(Based on an email from dsheets)

There are a number of interesting projects which could be done with
SibylFS and combined in powerful ways. Among them are:

  * Test case reduction

  You would use SibylFS's checked trace model to reduce the number of
  state transitions required to exhibit the "same" model deviation
  through repeated application of the model.

  * Test case translation

  You would translate the transition system of the model into a C
  program (or programs) which would either produce the same output as
  the present script interpreter or would provide a predicate command
  that would exit 0 on a 'correct' system and 1 on a deviant
  system. The simple cases for this are trivial and the complicated
  cases involve managing users and processes. It would be nice for the
  test cases to stand-alone without requiring linking to a runtime
  system (but that may be too hard/annoying/unwieldy or simply
  including the relevant bits of the runtime in the test case).

  * Race test generation without instrumentation

  You would translate transitions which have overlapping call/return
  pairs into one or more C programs which include
  multi-thread/multi-process races that have an interference
  distribution which gives "good" coverage. Feedback into further race
  generation and instrumentation to validate the interference
  distribution would be useful. Combined with 1 and 2, this could lead
  to a very powerful file system race reproduction case generator.

  * Test case generation

  You would use the specification model or develop a property-based
  model to generate test cases for transitions that have already been
  specified but may not have been tested adequately. The difficulty
  here is understanding which non-model properties may be relevant to
  test case generation and ensuring that the number of generated test
  cases does not explode (or somehow culling them).

  * Specification parameterization

  You would work with Tom to parameterize at run-time the Lem
  specification against filesystems, libcs, and features. Currently,
  we only parameterize across operating system VFS layers at
  run-time. We would really like to be able to indicate that we want
  to model ext4 on Linux or unknown filesystem without symlinks (and
  instead a consistent errno) on OS X. Combined with 1 and 2, this
  could lead to interesting file system fingerprinting tools.

  * Specify a new class of syscalls like the *at commands (linkat, mkdirat, etc)

  You would make a major specification coverage contribution and help
  find file system bugs in obscure but important implementations

There are surely other interesting projects in this area but nothing else that I can recall at the moment. Do feel free to reach out if you have any questions or want to discuss these further.