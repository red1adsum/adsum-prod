# Getting Started: 

Our goal is to allow device developers to focus on what they do best,
i.e. their applications and use cases software, while we provide them
the necessary tool sets that abstract the complexity of the SoCs
networking and allow the performance predictability necessary for their
industrial-grade applications.

Adsum SDK offers multiple components to applications developers, you can
include the components you need to monitor your application performance.
Our SDK is designed to be lightweight , so it has a minimal impact on
your application and SoC performance.

We provide you step by step guides on how to add the SDK to your
application, and how to publish data to your tenant on Adsum cloud-based
APM.

NRF-Architecture : NRF Connect SDK Integration Guide \<Link to a
separate document>

Adsum Modules and Mesh Models:

\<We describe here the modules list that are part of our SDK and how
Adsum Mesh Models work, <u>we need to define what level of details to
share in our public documentation</u>\>

nRF Connect SDK Integration Guide

This guide provides a walkthrough of the required steps to integrate
Adsum SDK lib files into your project using nRF Connect SDK. This
integration has been validated against nNRF Connect SDK releases ranging
from vXXX to vYYY

Integration steps :

*Note:*

*This guide assumes that you are using a qualified nRF SoC, check the
list of qualified nRF SoCs \<include link of Qualified nRF SoCs and SDK
versions>*

Download Adsum SDK Lib files :

The first step is to download recent library files from Adsum SDK
repository on GitHub, please visit Adsum github site \<add URL> and
download the folder “adsum_sdk_lib”.

Include Adsum SDK Library files in your project build :

1- Place the downloaded folder into your project repository as explained
in the structure below :

\<YOUR_PROJECT REPOSITORY>

├── **adsum_sdk_lib**

│ ├──**lib**

│ │ │ #precompiled library file

│ │ └─ adsum_lib.a

│ │

│ │

│ ├── **include**

│ │ │ # Configuration headers

│ │ └─ adsum_models.h

2- Define Adsum Models Header into the main.c file of your project :

#include "../adsum_sdk_lib/include/adsum_models.h"

3- Update your Mesh Elements definition in the main.c to include
Adsum_models, as you will notice, we are using element id 77, so make
sure this is unique to Adsum.

static struct bt_mesh_elem elements\[\] = {

BT_MESH_ELEM(0, models, BT_MESH_MODEL_NONE),

BT_MESH_ELEM(77, BT_MESH_MODEL_NONE, Adsum_models), // Adsum Model Def

};

4- Add the adsum_model_init function to the main.c file, it serves for
Adsum Model initiationalization. This function needs to be added after
Mesh initialization is complete. You can place it either in the main.c,
or in the function responsible to initialize your mesh network.

adsum_model_init(&comp); // initial adsum model

5- Update your project CMakeLists.txt file to include the below
instructions and targets required for Adsum SDK integration.

**# Adsum SDK Lib additions**

**set(adsum_lib_dir ${CMAKE_CURRENT_SOURCE_DIR}/adsum_sdk_lib)**

**set(ADSUMLIB_LIB_DIR ${adsum_lib_dir}/lib)**

**add_library(adsum_lib STATIC IMPORTED GLOBAL)**

**set_target_properties(adsum_lib PROPERTIES IMPORTED_LOCATION
${ADSUMLIB_LIB_DIR}/adsum_lib.a)**

**target_link_libraries(app PUBLIC adsum_lib)**

5- Update your project Kernel Configuration file “prj.conf” to include
the appropriate Adsum recommended parameters below.

We do recommend fine-tuning these values based on the scale of your
network. Please visit the Adsum Mesh Models config guide \<Include a
link> where all the parameters are explained along with recommended
values based on the scale of your network.

If you are new to Bluetooth Mesh, we encourage you to read our Bluetooth
Mesh primer \<Add link to Adsum Bluetooth Primer> to understand the BLE
mesh technology and check our Adsum terminology page \<Add link to Adsum
Terminology>.

**# ADSUM MODELS CONFIG**

**CONFIG_BT_MESH_CRPL=128**

**CONFIG_BT_MESH_MSG_CACHE_SIZE=128**

**CONFIG_BT_MESH_ADV_BUF_COUNT=128**

**CONFIG_BT_MESH_SEG_BUFS=256**

**CONFIG_BT_MESH_RX_SEG_MAX=10**

**CONFIG_BT_MESH_TX_SEG_MAX=10**

**CONFIG_BT_MESH_RX_SEG_MSG_COUNT=10**

**CONFIG_BT_MESH_TX_SEG_MSG_COUNT=10**

**CONFIG_BT_MESH_LOOPBACK_BUFS=32**

**CONFIG_BT_MESH_FRIEND=n**

**CONFIG_BT_MESH_LOW_POWER=n**

**CONFIG_BT_MESH_MODEL_EXTENSIONS=n**

**#Use this parameter to enable UART logging if needed**

**CONFIG_UART_NRFX=y**
