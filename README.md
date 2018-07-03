# How-to-install-Ubuntu-18.04-on-Alienware-15-R4
## Overview
This guide descibes how to set up a dual-boot Alienware 15 R4 laptop with Windows 10 and Ubuntu 18.04 The system has the following drives:

|Purpose|Size|Type|Form|Source|Description|
|-------|----|----|----|------|-----------|
|Windows|256GB|SSD NVMe|2.5in|Standard with 15R4|Toshiba THNSN5256GPUK NVMe/PCIe M.2 PCIe|
|Ubuntu|240GB|SSD NVMe|2242|Optional: [Amazon](https://www.amazon.com/gp/product/B07DD4FWRL)|Toshiba RC100 THN-RC10Z2400G8 M.2 PCIe|
|Shared|1TB|SATA III|2280|Standard with 15R4|HGST Travelstar 7K1000 7200 RPM 32MB Cache|
## Acknowledgement
This guide borrows heavily from [alienware15r3_ubuntu14 guide](https://github.com/awesomebytes/alienware15r3_ubuntu14). Thanks [awesomebytes](https://github.com/awesomebytes). A new guide was needed as the ubuntu version, laptop model and some steps have changed. 
## Ubuntu 18.04 set up
1. Download `Ubuntu 18.04 LTS` ISO from the [Ubuntu download site](https://www.ubuntu.com/download/desktop)
2. Download `Rufus` from the [Rfus download site](https://rufus.akeo.ie/)
3. Use Rufus to create a `bootable USB stick` by:
* 
