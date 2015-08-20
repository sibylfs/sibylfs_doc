# Thomas' permissions documentation

## General Overview

File access permissions are highly implementation specific. In section
4.4 the standard states:

    ### 4.4 File Access Permissions

    The standard file access control mechanism uses the file permission bits, 
    as described below.

    Implementations may provide additional or alternate file access
    control mechanisms, or both. An additional access control mechanism
    shall only further restrict the access permissions defined by the file
    permission bits. An alternate file access control mechanism shall:
     
      - Specify file permission bits for the file owner class, file group 
        class, and file other class of that file, corresponding to the 
        access permissions.
     
      - Be enabled only by explicit user action, on a per-file basis 
        by the file owner or a user with appropriate privileges.

      - Be disabled for a file after the file permission bits are changed 
        for that file with chmod(). The disabling of the alternate mechanism
        need not disable any additional mechanisms supported by 
        an implementation.
     
    Whenever a process requests file access permission for read,
    write, or execute/search, if no additional mechanism denies
    access, access shall be determined as follows:
     
      - If a process has appropriate privileges:
     
          * If read, write, or directory search permission is requested,
            access shall be granted.

          * If execute permission is requested, access shall be granted
            if execute permission is granted to at least one user by the
            file permission bits or by an alternate access control
            mechanism; otherwise, access shall be denied.
     
      - Otherwise:
     
          * The file permission bits of a file contain read, write, and
            execute/search permissions for the file owner class, file group
            class, and file other class.

          * Access shall be granted if an alternate access control
            mechanism is not enabled and the requested access permission
            bit is set for the class (file owner class, file group class,
            or file other class) to which the process belongs, or if an
            alternate access control mechanism is enabled and it allows
            the requested access; otherwise, access shall be denied.



    A.4.4 File Access Permissions

    A process should not try to anticipate the result of an attempt to
    access data by a priori use of these rules. Rather, it should make the
    attempt to access data and examine the return value (and possibly
    errno as well), or use access(). An implementation may include other
    security mechanisms in addition to those specified in POSIX.1, and an
    access attempt may fail because of those additional mechanisms, even
    though it would succeed according to the rules given in this
    section. (For example, the user's security level might be lower than
    that of the object of the access attempt.) The supplementary group IDs
    provide another reason for a process to not attempt to anticipate the
    result of an access attempt.

    Since the current standard does not specify a method for opening a
    directory for searching, it is unspecified whether search permission
    on the fd argument to openat() and related functions is based on
    whether the directory was opened with search mode or on the current
    permissions allowed by the directory at the time a search is
    performed. When there is existing practice that supports opening
    directories for searching, it is expected that these functions will be
    modified to specify that the search permissions will be granted based
    on the file access modes of the directory's file descriptor identified
    by fd, and not on the mode of the directory at the time the directory
    is searched.

So, implementations may both provide *additional* and *alternate* file
access control mechanisms. Moreover access depends on whether a process
has *appropriate privileges*.

    3.20 Appropriate Privileges

    An implementation-defined means of associating privileges with a
    process with regard to the function calls, function call options, and
    the commands that need special privileges. There may be zero or more
    such means. These means (or lack thereof) are described in the
    conformance document.

Despite being completely implementation-defined, appropriate privileges
are a very important concept. Most operations demand that the process
has appropriate privileges and as stated in section 4.4 file permission
checking relies on appropriate privileges as well. Important concepts
like superusers (as stated in section A.3) is implemented via
appropriate privileges.

    Superuser*

    This concept, with great historical significance to UNIX system 
    users, has been replaced with the notion of appropriate privileges.

In the end, we don’t know much. A lot is implementation defined.
However, Posix defines permission bits as the minimal and as the
fallback permission model. Posix specifies in `sys/stat.h` the following
permissions:

 Name    | Value | Description                               
 --------|-------|------------
`S_IRWXU`| 0o0700| Read, write, execute/search by owner.    
`S_IRUSR`| 0o0400| Read permission, owner.                  
`S_IWUSR`| 0o0200| Write permission, owner.                 
`S_IXUSR`| 0o0100| Execute/search permission, owner.        
`S_IRWXG`| 0o0070| Read, write, execute/search by group.    
`S_IRGRP`| 0o0040| Read permission, group.                  
`S_IWGRP`| 0o0020| Write permission, group.                 
`S_IXGRP`| 0o0010| Execute/search permission, group.        
`S_IRWXO`| 0o0007| Read, write, execute/search by others.   
`S_IROTH`| 0o0004| Read permission, others.                 
`S_IWOTH`| 0o0002| Write permission, others.                
`S_IXOTH`| 0o0001| Execute/search permission, others.       
`S_ISUID`| 0o4000| Set-user-ID on execution.                
`S_ISGID`| 0o2000| Set-group-ID on execution.               
`S_ISVTX`| 0o1000| On directories, restricted deletion flag.


So, essentially there are *read*, *write* and *execute/search*
permissions for the owner, the group and others as well as 3 special
permissions.

The problem is, how to interpret these permissions. Every process has a
*real*, an *effective* and a `saved` user ID. The *real* user ID is the
ID the process really has. The *effective* one the one actually used and
the *saved* one one that it is - without special priviledges - allowed
to switch to. Normally all 3 user ids are the same, the difference only
important for processes that switch user id.

As far as I understand it, the effective user ID is used with the file
permission bits. That seems to be the behavior indicated by the
description of for example `access`. However, I could not find a direct
explanation in the Posix Spec so far. As I understand it the effective
user id is used to also determine the groups the process belongs to.
With both a user id and a set of group ids, one can use the permission
bits to determine, whether a process has a certain permission.

Each process also has a *real*, an *effective* and a `saved` group ID.
As far as I understand it, these are normally not important for
permission checking, but e.g. when creating new files and setting their
permission bits.

