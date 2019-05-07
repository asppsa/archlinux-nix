archlinux-nix
=============

This is a script that helps set up [Nix][1] on [Archlinux][2].

There are two main functions:

1. creating a build group and set of build users; and
2. setting up a sandbox for builds.


Why?
----

If you want to use Nix as a non-root user, you need to set up
[multi-user mode][3].  This requires creating build users, which is a
tad menial to do over and over, so here's a script to do it for you.

The other thing you will find using Nix on Arch Linux (and probably
other linuces for that matter) is that the presence of libraries in
/usr can cause problems when compiling some programs in Nixpkgs.  The
solution to this is to use [sandboxing][4], which is another thing
that is a bit of a pain to set up; so this script can also take care
of that for you.


Installation
------------

There is no AUR package yet...

```
install archlinux-nix /usr/local/bin
```


Usage
-----

## Status

```
archlinux-nix status
```

Displays some info about whether nix is installed, etc.


## Install nix

```
archlinux-nix install
```

This code (mostly pilfered from the [official install script][5]) will
download nix in binary format, install it into `/nix` and add environment
setup to `/etc/profile.d`.


## Set-up build users

```
archlinux-nix set-build-group
```

This will:

1. create a group called `nixbld`, and a set of ten system users,
   `nixbld{1..10}`;
2. add a `build-users-group` line to `nix.conf`;
3. kill the `nix-daemon` if it's running (so that it can pick up the
   new settings); and
3. fix the ownership on the nix store to be writable by the build
   users.

If you don't like `nixbld`, you can specify a different name:

```
archlinux-nix set-build-group mynixbuild
```

This would create a group called `mynixbuild`, and users
`mynixbuild{1..10}`.

## Get rid of build users

```
archlinux-nix delete-build-group
```

This will:

1. determine the name of the group used in nix.conf;
2. remove the `build-users-group` line from `nix.conf`;
3. kill the nix-daemon, if it's running;
4. remove the ten users associated with the group; and
5. remove the group itself.

You can also specify a group name:

```
archlinux-nix delete-build-group mygroup
```

This will skip step 1 in the above series.


## Setup sandboxing

```
archlinux-nix set-sandbox
```


## Stop sanboxing

```
archlinux-nix delete-sandbox
```


License
-------

Licenced under the Apache License, Version 2.0.

[1]: https://nixos.org/nix/
[2]: https://www.archlinux.org/
[3]: https://nixos.org/nix/manual/#ssec-multi-user
[4]: https://nixos.org/nix/manual/#conf-build-sandbox-paths
[5]: https://nixos.org/nix/download.html
