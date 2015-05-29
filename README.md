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

### Build a project ie the Wanboard-quad ###
source the environment

    $: . ./setup-environment builds/Freescale/wandboard-quad

Run Bitbake

    $: bitbake fsl-image-multimedia-full


## And thats all there is to it ##
## EXCEPT!!          ##

Raspberrypi fails to build with **yocto dizzy**, so we need **yocto daisy**

### Create the BSP directory for YOCTO 1.7 daisy ###
    
    $: PATH=${PATH}:~/bin
    $: mkdir yocto-daisy
    $: cd yocto-daisy

#### Initialise the repositories for POKY daisy ####

    $: repo init -u https://github.com/noeldiviney/yocto-repo -b daisy 

#### Download yocto, poky, openembedded and all BSP metadata layers ####

    $: repo sync

once this has completed you should have a complete development environment

### Build a project ie the raspberrypi ###
source the environment

    $: . ./setup-environment builds/Raspberrypi/model-b+

Run Bitbake

    $: bitbake rpi-hwup-image
    