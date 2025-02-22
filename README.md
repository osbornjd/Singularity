# Singularity container for sPHENIX and EIC-sPHENIX

Singularity container for sPHENIX and EIC-sPHENIX allow collaborators to run sPHENIX RCF/SDCC environment with the nightly builds on your local computers or on external high-performance computing clusters. 

This repository includes the instruction and local update macro for this Singularity container.

**Validations:** `updatebuild.sh --build=new` 
[![Build Status](https://web.racf.bnl.gov/jenkins-sphenix/buildStatus/icon?job=sPHENIX%2Fsingularity-download-validation)](https://web.racf.bnl.gov/jenkins-sphenix/job/sPHENIX/job/singularity-download-validation/) , 
`--build=root5`
[![Build Status](https://web.racf.bnl.gov/jenkins-sphenix/buildStatus/icon?job=sPHENIX%2Fsingularity-download-validation-root5)](https://web.racf.bnl.gov/jenkins-sphenix/job/sPHENIX/job/singularity-download-validation-root5/)

[![Macros](https://img.shields.io/badge/standard%20macros-git-green.svg)](https://github.com/sPHENIX-Collaboration/macros)
[![Tutorials](https://img.shields.io/badge/tutorials-git-green.svg)](https://github.com/sPHENIX-Collaboration/tutorials)
[![Doxygen](https://img.shields.io/badge/code%20reference-Doxygen-green.svg)](https://www.phenix.bnl.gov/WWW/sPHENIX/doxygen/html/)
[![Last Commit](https://img.shields.io/github/last-commit/sPHENIX-Collaboration/Singularity.svg)](https://github.com/sPHENIX-Collaboration/Singularity/commits/master)

# How to download

sPHENIX and EIC-sPHENIX software can be obtained to your local computing environment in two ways: 

1. Option 1, **Mount sPHENIX CVMFS**: sPHENIX container, software and builds are distribute on [CVMFS](https://www.racf.bnl.gov/docs/services/cvmfs/info) since Nov 2018. Like RCF/SDCC computing cluster at BNL, external collaborating computing center could also mount the `/cvmfs/sphenix.sdcc.bnl.gov/` CVMFS repository, which automatically obtain, buffer and update all sPHENIX build files.
2. Option 2, **Download sPHENIX build via HTTPS archive**: one can also directly download the files for sPHENIX build to a local folder via [the nightly refreshed HTTPS archive](https://www.phenix.bnl.gov/WWW/publish/phnxbld/sPHENIX/Singularity/). 

The advantage of **Mount sPHENIX CVMFS** is that it mounts all sPHENIX builds and software and perform automatic caching and updates. This would be suitable for the case of a computing center or server environment. However, it requires constant network connection to function. Therefore, if you wish to use sPHENIX/EIC-sPHENIX software on a laptop during travel, **Downloading sPHENIX build via HTTPS archive** would work best. All download instructions are the same for sPHENIX and EIC-sPHENIX. 

## Option 1: Mount sPHENIX CVMFS

1. On your local system, install [Singularity v2.5](https://www.sylabs.io/guides/2.5/user-guide/quick_start.html#installation). 

    - *Note 1: the current RCF image is built under Singularity v2.5.0. Newer version of Singularity may be incompatible to load this image.*
    - *Note 2: Singularity installation may require host to support local compilation. E.g. on Ubuntu, it can be obtained via `sudo apt-get install libtool m4 automake`*

2. Install [CVMFS from CERN](https://cernvm.cern.ch/portal/filesystem/quickstart). CERN support build packages under (various Linux distribution and MAC)[https://cernvm.cern.ch/portal/filesystem/downloads].

3. Copy these three configuration and key files to your local computer from RCF (e.g. from any interactive RCF computer nodes):

```
/etc/cvmfs/keys/sdcc.bnl.gov/sphenix.sdcc.bnl.gov.pub
/etc/cvmfs/config.d/sphenix.sdcc.bnl.gov.local
/etc/cvmfs/domain.d/sdcc.bnl.gov.local
```

   Add `sphenix.sdcc.bnl.gov` to `CVMFS_REPOSITORIES` in `/etc/cvmfs/default.local`. e.g. 
```
CVMFS_REPOSITORIES=sphenix.sdcc.bnl.gov
```
   You may also need to specify `CVMFS_HTTP_PROXY` in `/etc/cvmfs/default.local` from your local computing to access the `http://cvmfs.sdcc.bnl.gov:8000/` server. For direct network access, `CVMFS_HTTP_PROXY=DIRECT` would work. 

   After completing step 2 and 3, please confirm you can read local path `/cvmfs/sphenix.sdcc.bnl.gov/`, which should show same content as that on RCF interactive nodes. 
   
4. launch singularity container for sPHENIX and EIC-sPHENIX with following command

```
singularity shell -B /cvmfs:/cvmfs /cvmfs/sphenix.sdcc.bnl.gov/singularity/rhic_sl7_ext.simg
source /opt/sphenix/core/bin/sphenix_setup.sh -n   # setup sPHENIX environment in the singularity container shell. Note the shell is bash by default
root # give a test
```
*Please note the slight difference in singularity shell commands for option 1 and option 2*


## Option 2: Download sPHENIX build via HTTPS archive

1. On your local system, install [Singularity v2.5](https://www.sylabs.io/guides/2.5/user-guide/quick_start.html#installation). 

    - *Note 1: the current RCF image is built under Singularity v2.5.0. Newer version of Singularity may be incompatible to load this image.*
    - *Note 2: Singularity installation may require host to support local compilation. E.g. on Ubuntu, it can be obtained via `sudo apt-get install libtool m4 automake`*

2. Download this repository:

```
git clone git@github.com:sPHENIX-Collaboration/Singularity.git
cd Singularity/
```

3. Run the download/update macro [updatebuild.sh](./updatebuild.sh).

```
./updatebuild.sh
```

This macro download the current release of sPHENIX/EIC-sPHENIX Singularity container and nightly build libs. The total download size is about 5 GB  and decompressed disk usage is about 10 GB. Two build versions are supported with default as the `new` build, as well as the `root5` build with `./updatebuild.sh --build=root5`.

4. Start the container with 

```
singularity shell -B cvmfs:/cvmfs cvmfs/sphenix.sdcc.bnl.gov/singularity/rhic_sl7_ext.simg
source /opt/sphenix/core/bin/sphenix_setup.sh -n   # setup sPHENIX environment in the singularity container shell. Note the shell is bash by default
root # give a test
```
*Please note the slight difference in singularity shell commands for option 1 and option 2*

5. To get daily build update, run the download/update macro [updatebuild.sh](./updatebuild.sh) to sync build files again. 


# What's next

After entering the Singularity container, you can source sPHENIX environment and interact with it in the same way as on RCF: 

```
computer:~/> singularity shell <options depending on which of the two downloading options above>
Singularity: Invoking an interactive shell within container...
Singularity rhic_sl7_ext.simg:~/> source /opt/sphenix/core/bin/sphenix_setup.sh -n
Singularity rhic_sl7_ext.simg:~/> lsb_release  -a         # Verify same environment shows up as that on RCF
LSB Version:	:core-4.1-amd64:core-4.1-ia32:core-4.1-noarch
Distributor ID:	Scientific
Description:	Scientific Linux release 7.3 (Nitrogen)
Release:	7.3
Codename:	Nitrogen
```

This bring up a shell environment which is identical to sPHENIX RCF. Meanwhile, it use your local file system for non-system files, e.g. it directly work on your code or data directories. Singularity container also support running in the [command mode or background mode](https://www.sylabs.io/guides/2.5.1/user-guide/quick_start.html#interact-with-images). 

Next, please try [the sPHENIX simulation tutorial](https://github.com/sPHENIX-Collaboration/macros). 

Please discuss on [sPHENIX software email list](https://lists.bnl.gov/mailman/listinfo/sphenix-software-l) and [meeting](https://indico.bnl.gov/categoryDisplay.py?categId=88) regarding any question or suggestion.

# Troubleshooting

## MAC installation

Singularity runs under linux OS. But in macOS, it require another layer of virtual machine to generate a linux environment first ([see Singularity docs](https://www.sylabs.io/guides/2.5/user-guide/quick_start.html#installation)). For Mac users, Here is [a more detailed guild on the macOS installation of sPHENIX container](./OSX_installationguide.md).

## 3D accelerated Graphics

The container is built for batch computing. It could be tricky to bring up 3D-accelerated graphics for Geant4 display. 
* On Linux, binding local X IPC socket folder to the singularity container could help enabling local hardware 3D acceleration, e.g. `singularity -B /tmp/.X11-unix:/tmp/.X11-unix ....` followed with `setenv DISPLAY unix:0.0` in the container. 
* On MAC John H. have developed [a note on how to get the 3D graphics working on MAC](https://indico.bnl.gov/event/4046/contributions/25558/attachments/21219/28796/singularity_mac_haggerty_20181217.pdf). 

## Clean download in Option-2

Occasionally, local download catch cab become corrupt, e.g. after an interrupted `updatebuild.sh` call. If you encounter a problem executing container, it is always useful to first try clean up the local download buffer by removing the `./cvmfs` folder and download again. Or you can run `./updatebuild.sh --clean <other options>` which force a clean download (default is incremental updates). 

If you have RCF credentials, you can also compare your local output with the daily test runs of the default simulation macro in the container following these links: `updatebuild.sh --build=new` 
[![Build Status](https://web.racf.bnl.gov/jenkins-sphenix/buildStatus/icon?job=sPHENIX%2Fsingularity-download-validation)](https://web.racf.bnl.gov/jenkins-sphenix/job/sPHENIX/job/singularity-download-validation/) , 
`--build=root5`
[![Build Status](https://web.racf.bnl.gov/jenkins-sphenix/buildStatus/icon?job=sPHENIX%2Fsingularity-download-validation-root5)](https://web.racf.bnl.gov/jenkins-sphenix/job/sPHENIX/job/singularity-download-validation-root5/)
