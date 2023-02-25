# vinyl-ripping-workflow container

This repository contains image definition files for a container that augments my personal vinyl recording digitization process.
The container can be built and run using [apptainer](https://apptainer.org/) (formerly known as Singularity).

# Using the container

In order to build the container, run `sudo apptainer build image.sif image.def`.
After the build has completed, you can run [wavbreaker](https://wavbreaker.sourceforge.io/) using `apptainer exec image.sif wavbreaker`.

# Troubleshooting

An attempt to run a GTK application might result in the following error message:
> Authorization required, but no authorization protocol specified

In this case, it is necessary to alter the access control list of X11 by running `xhost + local:`.

If you notice that GTK applications do not follow the system theme, it is advised to add the following snippet to `/etc/apptainer.conf`:
```
# BIND PATH: [STRING]
# DEFAULT: Undefined
# Define a list of files/directories that should be made available from within
# the container. The file or directory must exist within the container on
# which to attach to. you can specify a different source and destination
# path (respectively) with a colon; otherwise source and dest are the same.
# NOTE: these are ignored if apptainer is invoked with --contain except
# for /etc/hosts and /etc/localtime. When invoked with --contain and --net,
# /etc/hosts would contain a default generated content for localhost resolution.
#bind path = /etc/apptainer/default-nsswitch.conf:/etc/nsswitch.conf
#bind path = /opt
#bind path = /scratch
bind path = /etc/localtime
bind path = /etc/hosts
bind path = /usr/share/themes
bind path = /usr/share/icons
```

This will ensure that the application has access to host GTK themes.
