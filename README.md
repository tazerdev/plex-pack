# plex-repack

A small playbook to convert a PleX Debian package to an RPM for the aarch64 platform.

## Motivation

I run Fedora on my Raspberry PIs and prefer not to run PleX Media Server as a container. Since PleX doesn't distribute RPMs for the aarch64 platform I have to roll my own. This playbook does that by downloading both the aarch64 Debian package, the x86_64 RPM, unpacking both, generating an rpmspec and then building the aarch64.rpm.

It's not extensively tested but it works for me.

## Usage

By default this will use '/tmp/plex-repack' as a temporary workspace and will place the final rpm there. All downloaded files and generated artifacts will be located in that directory. You can specify the directory using the extra var 'build_dir':

```Bash
ansible-playbook -i localhost, -c local main.yml -e "build_dir=/tmp/workdir"
```

You must specify the verison of PleX you wish to rebuild using the 'plex_version' extra var. This must include the version hash:

```Bash
ansible-playbook -i localhost, -c local main.yml -e "plex_version=1.25.5.5492-12f6b8c83"
```

```Bash
# ls -la /tmp/workdir
total 237240
-rw-r--r-- 1 root   root    72391756 Feb 13 14:01 plexmediaserver-1.25.5.5492-12f6b8c83.aarch64.rpm
-rw-r--r-- 1 root   root    67228702 Feb 13 12:06 plexmediaserver_1.25.5.5492-12f6b8c83_arm64.deb
-rw-r--r-- 1 root   root   102973688 Feb 13 12:06 plexmediaserver-1.25.5.5492-12f6b8c83.x86_64.rpm
-rw-r--r-- 1 root   root      329348 Feb 13 13:58 plexmediaserver.spec
```
