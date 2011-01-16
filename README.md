Tinysync -- A simple directory uploader
=======================================

(C) 2011 Kevin Mehall <km@kevinmehall.net>  
Revised BSD License  
https://github.com/kevinmehall/tinysync  

About
-----

Tinysync is a minimal tool to incrementally upload a local directory to 
a dumb SFTP server. It uses a manifest file to determine which files 
have changed, and only uploads the necessary files. It was developed as 
a replacement for [`bzr-upload`](https://launchpad.net/bzr-upload) when 
I switched to `git`.

Compared to
-----------

### bzr-upload

  * `tinysync` is VCS-agnostic (flip-side: not VCS integrated)
 
### Rsync
 
  * `tinysync` has native SFTP support, without requiring rsync on the server or sshfs 
  * `tinysync` uses a remote manifest file and does not require traversing the entire remote directory tree
  * `tinysync` incremental upload assumes no other changes have been made to the remote directory

Examples
--------

### Setup

Upload the current directory to a remote location:

    tinysync -l sftp://user@host/location

The location is remembered in `.tinysync-config` and does not have to be 
specified after first use:

    tinysync

It can remember multiple locations. The default location is `default`

    tinysync production -l sftp://user@prod/var/www -n
    tinysync testing -l sftp://user@test/var/www -n

    tinysync production
    tinysync testing

When run, it outputs a change summary:

    Synchronized 22 files. (2 added; 3 updated; 0 deleted)

The `-d` option sets `tinysync`'s source directory

    tinysync -d site

`-n` makes no changes, but summarizes what would be done (dry run)

    tinysync -n

Files can be excluded from upload. `.git`, `.bzr`, `.gitignore` and 
`.bzrignore` are excluded by default. Remember to use quotes around 
glob-syntax wildcards so they are not expanded by the shell

    tinysync --exclude '*.bak'

To remove destination locations or exclude rules, modify 
`.tinysite-config` in the source directory



Usage
-----

    tinysync [location_name] [options]

### Options

#### -h, --help

show this help message and exit

#### -l URI, --uri=URI

Set the destination URI for the chosen location name

#### -n, --dry-run

Perform a trial run and make no changes

#### -d DIR, --dir=DIR

Work in DIR instead of the current directory

#### -f, --full

Ignore the remote digest and force full upload

#### --exclude=RULE

Add an exclude rule (glob syntax)

#### --mark-updated

Update the remote digest without syncing files. Use only if remote dir 
is known to be in sync
