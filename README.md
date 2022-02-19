# tarfs
> Backwards-compatible extension for tar archives

## Introduction
This repository describes a backwards-compatible extension to the tar file format,
which can be used to speedup reading specific members from a tar file,
especially if the file is read from a linear medium such as tape as
long as the medium is seekable.

## Details
``tarfs`` provides an index for a given tar archive which holds information
about a its members or a subset of its members.
This information includes the offset of the member in the tar archive and
thus can be used to directly seek to the position of an indexed member
instead of the need to process all preceding members of the archive.

For new tar archives this index can be included as the *first* member
in the archive, or the index can be added later by replacing the first
member(s) of the archive with the index while appending the replaced members
at the end of the archive.
If the tar archive cannot or should not be modified the index can still
be used by providing it via a file external to the archive.

A ``tarfs`` archive is simply a tar archive,
in which the first member has the name ``.tarfs`` and its data is a valid
index for the remaining archive.
Programs supporting the ``tarfs``-extension can use the index in order
to list and directly extract all members which are listed in the index.
Programs which do not support the ``tarfs``-extension simply extract the
index as a regular file called ``.tarfs``.
Using this technique a ``tarfs``-file present on a tape can be mounted like a filesystem by first saving the index into memory or unto another filesystem.
