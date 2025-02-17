# buildah-copy "1" "March 2017" "buildah"

## NAME
buildah\-copy - Copies the contents of a file, URL, or directory into a container's working directory.

## SYNOPSIS
**buildah copy** *container* *src* [[*src* ...] *dest*]

## DESCRIPTION
Copies the contents of a file, URL, or a directory to a container's working
directory or a specified location in the container.  If a local directory is
specified as a source, its *contents* are copied to the destination.

## OPTIONS

**--add-history**

Add an entry to the history which will note the digest of the added content.
Defaults to false.

Note: You can also override the default value of --add-history by setting the
BUILDAH\_HISTORY environment variable. `export BUILDAH_HISTORY=true`

**--chmod** *permissions*

Sets the access permissions of the destination content. Accepts the numerical format.

**--chown** *owner*:*group*

Sets the user and group ownership of the destination content.

**--contextdir**

Build context directory. Specifying a context directory causes Buildah to
chroot into a the context directory. This means copying files pointed at
by symbolic links outside of the chroot will fail.

**--ignorefile**

Path to an alternative .containerignore (.dockerignore) file. Requires \-\-contextdir be specified.

**--quiet**, **-q**

Refrain from printing a digest of the copied content.

## EXAMPLE

buildah copy containerID '/myapp/app.conf' '/myapp/app.conf'

buildah copy --chown myuser:mygroup containerID '/myapp/app.conf' '/myapp/app.conf'

buildah copy --chmod 660 containerID '/myapp/app.conf' '/myapp/app.conf'

buildah copy containerID '/home/myuser/myproject.go'

buildah copy containerID '/home/myuser/myfiles.tar' '/tmp'

buildah copy containerID '/tmp/workingdir' '/tmp/workingdir'

buildah copy containerID 'https://github.com/containers/buildah' '/tmp'

buildah copy containerID 'passwd' 'certs.d' /etc

## FILES

### .containerignore/.dockerignore

If the .containerignore/.dockerignore file exists in the context directory,
`buildah copy` reads its contents. If both exist, then .containerignore is used.

When the \fB\fC\-\-ignorefile\fR option is specified Buildah reads the
content to exclude files and directories from the source directory, when
copying content into the image.

Users can specify a series of Unix shell globals in a ignore file to
identify files/directories to exclude.

Buildah supports a special wildcard string `**` which matches any number of
directories (including zero). For example, **/*.go will exclude all files that
end with .go that are found in all directories.

Example .containerignore/.dockerignore file:

```
# here are files we want to exclude
*/*.c
**/output*
src
```

`*/*.c`
Excludes files and directories whose names ends with .c in any top level subdirectory. For example, the source file include/rootless.c.

`**/output*`
Excludes files and directories starting with `output` from any directory.

`src`
Excludes files named src and the directory src as well as any content in it.

Lines starting with ! (exclamation mark) can be used to make exceptions to
exclusions. The following is an example .containerignore/.dockerignore file that uses this
mechanism:
```
*.doc
!Help.doc
```

Exclude all doc files except Help.doc from the image.

This functionality is compatible with the handling of .dockerignore files described here:

https://docs.docker.com/engine/reference/builder/#dockerignore-file

## SEE ALSO
buildah(1)
