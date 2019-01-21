# file_fingerprint

What is this project?
======

Find possible duplicates / local copies of files across machines and directories.

The idea is that I have a lot of files that are somewhat backed up amongst multiple machines - over the years there have been a lot of "Ah, going on a trip, need to be sure to save this file somewhere else" and then... these files disappear into the ether, only rarely to surface again.

In order to get more organized, this project aims to do the following:

* Characterize files regardless of directory or filename. "Characterize" means "Recognize the same file, regarless of what a system stat call says".
* Do this as performantly as possible (for instance, in parallel across the fs).
* Identify the same characterization across filesystems - that is, a bitwise transfer between filesystems of the same file (same bytes) means the file is the same
* Have the ability to sync across the data between filesystems in a performant way...
    * But local runs of the system should not need to sync.


The goal here is to have a system to identify how many copies, and where they are, of a file exist on any of the computers in the network.

The approach will be to use Python to build local databases of files based on a hash of the file contents. The hash will be computed along the first N bytes, where N is the lesser of "The whole file" and "However long an arbitrary limit of bytes is that keeps us from having hash collisions, but identifies the file well". This will be configurable.

Many file formats will store data in a header or a tail section that might change the header or tail. Due to this, **TEST THIS** A match is made on both the head and the tail for those N bytes.



The files will then be stored in a SQL database backed by either mysql or sqlite (for local / portable use).
Hash collisions where:
* The files collide,
* They have nearly the same size (within, say, 10% of the same size)

