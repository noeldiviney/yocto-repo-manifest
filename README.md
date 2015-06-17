## yocto-repo-manifest ##


**yocto-repo-manifest** is a Yocto meta repository manifest to allow the use of Google's "repo" to manage a number of Yocto repositories.  
If you are unfamiliar with repo, you can read up on it [here](http://source.android.com/source/version-control.html).  
  
The **yocto** repositories catered for are contained in the "default.xml" manifest file

    <?xml version="1.0" encoding="UTF-8"?>
    <manifest>

      <default sync-j="4" revision="master"/>

      <remote  fetch="git://git.yoctoproject.org"                name="yocto"/>
      <remote  fetch="git://github.com/Freescale"                name="freescale"/>
      <remote  fetch="git://git.openembedded.org"                name="oe"/>
      <remote  fetch="git://github.com/noeldiviney"              name="tnd"/>
      <remote  fetch="git://github.com/meta-qt5"                 name="qt5"/>
      <remote  fetch="git://git.yoctoproject.org"                name="raspberrypi"/>
      <remote  fetch="git://github.com/dv1"                      name="gstreamer"/>
      <remote  fetch="git://github.com/Xilinx"                   name="xilinx"/>
      <remote  fetch="git://github.com/nathanrossi"              name="parallella"/>
      <remote  fetch="git://git.rocketboards.org"                name="altera"/>
      <remote  fetch="git://git.linaro.org/openembedded"         name="linaro"/>
      <remote  fetch="git://git.yoctoproject.org"                name="ti"/>
      <remote  fetch="git://github.com/MarkusEich"               name="odroid"/>
      <remote  fetch="git://github.com/linux4sam"                name="CORE9G25"/>
    
      <!--
      -->
    
      <project remote="yocto"       revision="dizzy"    name="poky"                    path="poky"/>
      <project remote="yocto"       revision="dizzy"    name="meta-fsl-arm"            path="poky/meta-fsl-arm"/>
      <project remote="oe"          revision="dizzy"    name="meta-openembedded"       path="poky/meta-openembedded"/>
      <project remote="freescale"   revision="dizzy"    name="fsl-community-bsp-base"  path="poky/base">
                  <copyfile dest="poky/README" src="README"/>
                  <copyfile dest="poky/setup-environment" src="setup-environment"/>
      </project>
      <project remote="freescale"   revision="dizzy"    name="meta-fsl-arm-extra"       path="poky/meta-fsl-arm-extra"/>
      <project remote="freescale"   revision="dizzy"    name="meta-fsl-demos"           path="poky/meta-fsl-demos"/>
      <project remote="freescale"   revision="dizzy"    name="Documentation"            path="poky/Documentation"/>
      <project remote="tnd"         revision="master"   name="meta-builds"              path="poky/builds"/>
      <project remote="qt5"         revision="dizzy"    name="meta-qt5"                 path="poky/meta-qt5"/>
      <project remote="raspberrypi" revision="master"   name="meta-raspberrypi"         path="poky/meta-raspberrypi"/>
      <project remote="gstreamer"   revision="master"   name="meta-gstreamer1.0"        path="poky/meta-gstreamer1.0"/>
      <project remote="xilinx"      revision="dizzy"    name="meta-xilinx"              path="poky/meta-xilinx"/>
      <project remote="parallella"  revision="master"   name="meta-parallella"          path="poky/meta-parallella"/>
      <project remote="altera"      revision="master"   name="meta-altera"              path="poky/meta-altera"/>
      <project remote="linaro"      revision="dizzy"    name="meta-linaro"              path="poky/meta-linaro"/>
      <project remote="ti"          revision="master"   name="meta-ti"                  path="poky/meta-ti"/>
      <project remote="odroid"      revision="master"   name="meta-odroid"              path="poky/meta-odroid"/>
      <project remote="CORE9G25"    revision="dizzy"    name="meta-atmel"               path="poky/meta-atmel"/>
    
      <!--
      -->
    </manifest>

###Repo installs the above structure such that the Yocto build environment is as follows: ##

    yocto-dizzy/
        poky/
            base/                holds base layers for Freescale
            bitbake              holds the bitbake build tool stuff
            builds/
                Atmel/            holds the different Atmel builds         ie at91sam9c5ek etc
                Altera/           holds the different Altera builds        ie sockit etc
                Freescale/        holds the different Freescale builds     ie wandboard-quad etc
                Raspberrypi/      holds the different Raspberrypi builds   ie model-b+ etc
                Xilinx/           holds the different Xilinx builds        ie parallella etc
            documentation/        holds yocto manuals
            meta/
            meta-altera/
            meta-atmel/
            meta-fsl-arm/
            meta-fsl-arm-extra/
            meta-fsl-demos/
            meta-gstreamer1.0/
            meta-linaro/
            meta-odroid/
            meta-openembedded/
            meta-parallella/
            meta-qt5/
            meta-raspberrypi/
            meta-selftest/
            meta-skeleton/
            meta-ti/
            meta-xilinx/
            meta-yocto/
            meta-yocto-bsp/
            meta-scripts/
            tarballs-altera/        holds already downloaded tarballs. Speeds up subsequent builds
            tarballs-atmel/         holds already downloaded tarballs. Speeds up subsequent builds
            tarballs-freescale/     holds already downloaded tarballs. Speeds up subsequent builds
            tarballs-raspberrypi/   holds already downloaded tarballs. Speeds up subsequent builds
            tarballs-xilinx/        holds already downloaded tarballs. Speeds up subsequent builds
            .repo                   Git stuff
            oe-init-build-env       openembedded script for initialising build environment
            oe-init-build-env-memres
            README                  Readme file
            setup-environment       Sources script for initialising a build environment.

### Download and Install Google's repo utility ###

The BSP is based on the Yocto Project, which consists of a number of applicable metadata 'layers'. These are managed by the repo utility.

    $: mkdir ~/bin
    $: curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
    $: chmod a+x ~/bin/repo 


### Create the BSP directory for YOCTO 1.7 dizzy ###
    
    $: PATH=${PATH}:~/bin
    $: mkdir yocto-dizzy
    $: cd yocto-dizzy

#### Initialise the repositories for POKY dizzy ####

    $: repo init -u https://github.com/noeldiviney/yocto-repo -b dizzy 

#### Download yocto, poky, openembedded and all BSP metadata layers ####

    $: repo sync

once this has completed you should have a complete development environment



### Build the Freescale Wanboard-quad project ###

source the environment

    $: . ./setup-environment builds/Freescale/wandboard-quad

Run Bitbake

    $: bitbake fsl-image-multimedia-full



### Build the Atmel Core9g25 project ###

source the environment

    $: . ./setup-environment builds/Atmel/Core0g25

Run Bitbake

    $: bitbake core-image-minimal



### Build the raspberrypi model-b+ project ###

source the environment

    $: . ./setup-environment builds/Raspberrypi/model-b+

Run Bitbake

    $: bitbake rpi-hwup-image
    


### Build the Adapteva parallella project ###

source the environment

    $: . ./setup-environment builds/Xilinx/parallella

Run Bitbake

    $: bitbake core-image-minimal

