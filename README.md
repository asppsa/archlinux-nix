archlinux-nix
=============

This is a script that helps set up [Nix][nix] on [Arch Linux][arch]. It supports
two distinct ways of installing Nix:

1. Via [AUR][aur], using the [nix package][aur-nix]; or
2. Directly via Nix (i.e. Nix is "self-hosted").

In the case of the latter, this script helps to install Nix.  You can also use
the [official installer][nix-official-install].

Other scripts

1. creating a build group and set of build users; and
2. setting up a sandbox for builds.
3. launching the nix-daemon.


Installation
------------

This script is available via the Arch Linux [AUR][aur].  You can install either
the [archlinux-nix package][archlinux-nix] or the
[archlinux-nix-git package][archlinux-nix-git] if you want the bleeding edge.

Alternatively, if you attempting to use this script elsewhere, you can clone
this repo and do the following:

```
install archlinux-nix /usr/local/bin
```


Usage
-----

## Basic usage

If you are installing Nix from AUR, this script is called automatically, so you
shouldn't need to execute this script at all.  If you want to do a
"self-hosted" install, you can execute the following (as root):

```
archlinux-nix boostrap
```

This will "intelligently" execute various commands (described below) with
sensible defaults to get you up and running.

## Status

```
archlinux-nix status
```

Displays some info about whether Nix is installed, etc.


## Install "self-hosted" Nix

```
archlinux-nix install
```

This code (mostly pilfered from the [official install script][nix-official-install]) will
download nix in binary format, install it into `/nix` and add environment
setup to `/etc/profile.d`.


## Set-up build users

```
archlinux-nix setup-build-group
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
archlinux-nix setup-build-group mynixbuild
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

You can also specify a group name to delete a group of users that are not
specified in nix.conf:

```
archlinux-nix delete-build-group mygroup
```

This will skip step 1 in the above series.


## Setup sandboxing

Note that by default sandboxing is enabled in Nix.  For a self-hosted Nix
install, no additional configuration is needed.  If you installed Nix via
AUR, sandboxind is setup automatically using the following command:

```
archlinux-nix install-sandbox
```

This creates a Nix profile at `/nix/var/nix/profiles/arch-system/build-sandbox`
that includes the essential scripts required to build nix expressions
(bash, tar, etc.), and the references these in `/etc/nix/nix.conf`.

## Stop sandboxing

```
archlinux-nix delete-sandbox
archlinux-nix disable-sandbox
```

The former command will do the opposite of `install-sandbox` (i.e. remove the
`build-sandbox` profile and any mention of it from `nix.conf`); the latter
command will disable sanboxing entirely by adding a line to that effect to
`nix.conf`.

## Stopping/starting nix-daemon

```
archlinux-nix enable-nix-daemon
```

This will link (if required) and launch the `nix-daemon` systemd service/socket.

```
archlinux-nix disable-nix-daemon
```

This will do the opposite.

License
-------

Licenced under the Apache License, Version 2.0.

[nix]: https://nixos.org/nix/
[arch]: https://www.archlinux.org/
[nix-official-install]: https://nixos.org/nix/download.html
[archlinux-nix]: https://aur.archlinux.org/packages/archlinux-nix
[archlinux-nix-git]: https://aur.archlinux.org/packages/archlinux-nix-git
[aur]: https://aur.archlinux.org/
[aur-nix]: https://aur.archlinux.org/packages/nix/
