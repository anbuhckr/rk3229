# Build OP-TEE, U-Boot and Linux for RK3229

### Introduction

This documentation details my experiences with building and installing *mainline versions of OP-TEE, U-Boot and Linux* for systems based on Rockchip's RK3229. The primary focus of my efforts has been, and remains, on the MXQ 4K / MXQ-Pro 4K.

If you decide to use any of the information presented here - think for yourself and acknowledge that you, alone, take full responsibility for your own actions.


### Documentation

- #### Build

     - [Compile OP-TEE, U-Boot and Linux for RK3229](COMPILE.md)


- #### Full Installation

     This approach does not rely on any software that was shipped by the vendor.

     *Installation to onboard flash on devices that feature NAND instead of an eMMC will not work, as neither mainline U-Boot nor mainline Linux have support for the Rockchip NAND controller. For such devices, follow the installation procedure for SD/MMC instead.*

     - [U-Boot + Linux Installation (eMMC)](EMMC-INSTALL.md)
     - [U-Boot + Linux Installation (SD/MMC)](SDMMC-INSTALL.md)


### Device Findings

- #### MXQ 4K / MXQ-Pro 4K
    | Procedure                            | Status         | Remarks                                       |
    |--------------------------------------|----------------|-----------------------------------------------|
    | U-Boot + Linux Installation (eMMC)   | Pending        | -                                             |
    | U-Boot + Linux Installation (SD/MMC) | OK             | -                                             |


### Hardware Support

- #### CPUs
    - All 4 cores brought up via PSCI.

- #### Ethernet
    - 10/100Mbps Full Duplex.

- #### GPU
    - Hardware rendering via lima. Be sure to use upstream libdrm and mesa master branches respectively.

- #### HDMI
    - Works well if the HDMI connector is forced into the enabled state, and an EDID firmware override is used to compensate for problematic EDID responses from the HDMI sink or for DRM issues.
    - Unfortunately, an unhandled IRQ request message is generated by the kernel when the display is powered down but this does not hinder further use.

- #### MMC
    - eMMC and SD/MMC cards.

- #### USB
    - Host and OTG modes.

- #### Wireless
    - RTL8723BS attached via SDIO works using the r8723bs staging driver.


### Acknowledgement

Thank you to the following people for their assistance in making this process possible.

- [Justin Swartz](https://github.com/jhswartz/rk3229)
- Heiko Stübner, for answers and recommendations
- Fabio Bassa, for testing
