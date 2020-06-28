# FTP server for lwip and tinyfatfs

This repository contains the ftp-server for lwip Copyright (c) 2002 Florian
Schulze and an API translation layer Copyright (c) 2013 Philipp Tölke.

The translation layer translates between the vfs-API the ftp server uses and
the well known tinyfatfs by ChaN, which can be downloaded at
http://elm-chan.org/fsw/ff/00index_e.html .

The translation-layer

* is not reentrant and it is possible that multiple
  concurrent ftp-connections are *not* supported.
* relies on malloc(). It would be easy to change this to use a pooled allocator
* does not use long filenames for any function apart from getcwd().

All code in this repository is licensed under a 3-clause BSD license

Patches, comments and pull-requests are welcome.

## To build a docker container for testing

```
docker build -f test-in-docker\Dockerfile -t lwip-ftpd-test .
```

It will start lwip running on IP `172.17.0.5`, be sure to change that if it is not your docker network. You can then access ftp by dialing that address. It uses a file called `/app/data` as backing store, you can mount that in, if needed. The tester will `mkfs` when there is no file-system in that file.
