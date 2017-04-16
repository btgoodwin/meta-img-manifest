# Manifest for meta-img

This repository sets up a build environment for building a Yocto-based file system for the MIPSfpga.  This readme is similar to all of the other readme's for projects using Google Repo for Yocto builds.

## Getting Started

1.  Install Repo.

    Download the Repo script.

        $ curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > repo

    Make it executable.

        $ chmod a+x repo

    Move it on to your system path.

        $ sudo mv repo /usr/local/bin/

2.  Initialize a Repo client.

    Create an empty directory to hold your working files.

        $ mkdir <working directory>
        $ cd <working directory>

    Tell Repo where to find the manifest

        $ repo init -u <git repo> -b <branch> -m <manifest XML name>
    
    A successful initialization will end with a message stating that Repo is
    initialized in your working directory. Your client directory should now
    contain a .repo directory where files such as the manifest will be kept.
    
    > **NOTE:** The `-m` option lets you specify a different manifest file other than the `default.xml`.

    To learn more about the repo manifest format, here is the [documentation](https://gerrit.googlesource.com/git-repo/+/master/docs/manifest-format.txt).

3.  Fetch all the repositories.

        $ repo sync

    This may take upwards of 20 minutes depending on your network connection.

4.  Initialize the OpenEmbedded Environment. This assumes you created the oe-core directory
    in your home directory.

        $ TEMPLATECONF=`pwd`/poky/meta-img/conf source ./poky/oe-init-build-env ./build ./poky/bitbake


    This copies default configuration information into the build/conf*
    directory and sets up some environment variables for OpenEmbedded.  You may
    wish to edit the configuration options at this point.
    
5.  Build an image.

    This process downloads potentially several hundred megabytes of source code and then proceeds to
    compile several artifacts for each package, so make sure you have plenty of space (25GB
    minimum). Also, it takes a considerable amount of time and RAM (try to have at least 8 GB).

        $ export MACHINE="xilfpga"
        $ bitbake mipsfpga-soc-initramfs

6.  At the end of the build, in the above case, you should have a `tmp/deploy/images/xilfpga/vmlinux-initramfs-xilfpga.bin`, which is the kernel plus a RAM-based file system.  Also in that path are the file system and the kernel, separated.
