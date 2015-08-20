# Bugs found

This section gathers up bugs we have found in the POSIX spec, and also
in implementations.

## Bugs in POSIX

  * "open(O_CREAT), directories, and EISDIR" - spec accepts our suggestion concerning EISDIR
    * <http://thread.gmane.org/gmane.comp.standards.posix.austin.general/9503> - email thread
    * <http://austingroupbugs.net/view.php?id=847> - Austin Group bug tracker

  * "Question about mkdir and ENOENT" was accepted as an error, but no issue was opened yet
    * <http://thread.gmane.org/gmane.comp.standards.posix.austin.general/9499>
    * FIXME Austin Group bug tracker?

  * "Assistance in understanding the spec" was accepted that link should have ENOENT or ENOTDIR clause 
    * <http://thread.gmane.org/gmane.comp.standards.posix.austin.general/9182> - email thread
    * <http://austingroupbugs.net/view.php?id=822> - Austin Group bug tracker

  * "Does lstat update files timestamps?"
    * POSIX said: "The lstat() function is not required to update the
      time-related fields if the named file is not a symbolic link.
      The remainder was correct for POSIX.1-2001/SUSv3 but is out of
      date in the current standard, as we decided in the 2008 revision
      to require meaningful data for symbolic links in all stat
      structure fields except for the permission bits of st_mode.  It
      was an oversight that we didn't remove this rationale at the
      same time."
    * POSIX mailing list said: "I suspect that the double-negative in
    the first sentence was unintentional.  It should have said "only
    required" or "is a symbolic link"."
    * <http://thread.gmane.org/gmane.comp.standards.posix.austin.general/10075> - email thread
    * <http://austingroupbugs.net/view.php?id=889> - Austin Group bug tracker

## Real-world non-POSIX behaviour on Linux

The latest version of this list is in `../fast_2015/fs.mng`, for the
`open` command (should extend this list to include all commands).

## Real-world non-POSIX behaviour on Mac

FIXME
