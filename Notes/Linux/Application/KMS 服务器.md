# KMS 服务器

GitHub 上下载二进制包，解包运行
```bash
# download
wget https://ghproxy.com/https://github.com/Wind4/vlmcsd/releases/download/svn1113/binaries.tar.gz
# decompress
tar -zvxf binaries.tar.gz
# Linux intel
cd binaries/Linux/intel/static

# run
./vlmcsd-x64-musl-static -L 0.0.0.0:16886

# default
#$ ./vlmcs-x64-musl-static
$ ./vlmcs-x64-musl-static 0.0.0.0:16882
Connecting to 0.0.0.0:16886 ... successful
Sending activation request (KMS V6) 1 of 1  -> 03612-00206-566-464396-03-1103-14393.0000-2672021 (3A1C049600B60076)
```

Office 激活步骤
```bash
# vol versin Office
C:\Program Files\Microsoft Office\Office16

# open PowerShell here with administrator, and execute command
# GVLK key
cscript ospp.vbs /inpkey:XQNVK-8JYDB-WJ9W3-YJ8YR-WFG99
# server ip
cscript ospp.vbs /sethst:42.193.139.78
# server port
cscript ospp.vbs /setprt:16886
# activate
cscript ospp.vbs /act
# status
cscript ospp.vbs /dstatus
```

something  msg

```bash
PS C:\Program Files\Microsoft Office\Office16> cscript ospp.vbs /act
Microsoft (R) Windows Script Host Version 5.812
Copyright (C) Microsoft Corporation. All rights reserved.

---Processing--------------------------
---------------------------------------
Installed product key detected - attempting to activate the following product:
SKU ID: d450596f-894d-49e0-966a-fd39ed4c4c64
LICENSE NAME: Office 16, Office16ProPlusVL_KMS_Client edition
LICENSE DESCRIPTION: Office 16, VOLUME_KMSCLIENT channel
Last 5 characters of installed product key: WFG99
<Product activation successful>
---------------------------------------
---------------------------------------
---Exiting-----------------------------
PS C:\Program Files\Microsoft Office\Office16> cscript ospp.vbs /dstatus
Microsoft (R) Windows Script Host Version 5.812
Copyright (C) Microsoft Corporation. All rights reserved.

---Processing--------------------------
---------------------------------------
PRODUCT ID: 00339-10000-00000-AA718
SKU ID: d450596f-894d-49e0-966a-fd39ed4c4c64
LICENSE NAME: Office 16, Office16ProPlusVL_KMS_Client edition
LICENSE DESCRIPTION: Office 16, VOLUME_KMSCLIENT channel
LICENSE STATUS:  ---LICENSED---
ERROR CODE: 0x4004F040 (for information purposes only as the status is licensed)
ERROR DESCRIPTION: The Software Licensing Service reported that the product was activated but the owner should verify the Product Use Rights.
REMAINING GRACE: 179 days  (258642 minute(s) before expiring)
Last 5 characters of installed product key: WFG99
Activation Type Configuration: ALL
        DNS auto-discovery: KMS name not available
        KMS machine registry override defined: 42.193.139.78:16886
        Activation Interval: 120 minutes
        Renewal Interval: 10080 minutes
        KMS host caching: Enabled
---------------------------------------
---------------------------------------
---Exiting-----------------------------
```