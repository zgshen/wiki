
激活步骤参考 [KMS 服务器](../Linux/Application/KMS%20服务器.md)

密钥管理服务 (KMS) 客户端激活和产品密钥
https://docs.microsoft.com/zh-cn/windows-server/get-started/kms-client-activation-keys

上面的没找到 WindowsThin 的 KMS 客户端产品密钥，查了别人提供的
```
Windows 7 Embedded Standard : XGY72-BRBBT-FF8MH-2GG8H-W7KCW
```

### Windows 激活
```
slmgr /ipk XGY72-BRBBT-FF8MH-2GG8H-W7KCW
slmgr /skms 42.193.139.xxx:16886
slmgr /ato
```

没激活成功，参照 https://www.bilibili.com/read/mobile?id=12835398 ， 停掉服务删除 slui 好了
- 打开服务，停止并禁用Software Protection服务
- 打开C:\Windows\System32，找到 slui
- 邮件 slui，Properties-security-Edit（to change permission）
- 修改当前用户权限为完全控制，ok保存后，当前用户就有权限删除 slui 了
- 删除 slui 之后可以看到系统的status和product id都是 not available 不可用，其实可用还没烦人激活提醒了
![](Assets/Pasted%20image%2020220401183145.png)


### Office 2016激活
服务器地址是 42.193.139.xxx，端口16886
```
cd C:\Program Files\Microsoft Office\Office16
cscript ospp.vbs /inpkey:XQNVK-8JYDB-WJ9W3-YJ8YR-WFG99
cscript ospp.vbs /sethst:42.193.139.7
cscript ospp.vbs /setprt:16886
cscript ospp.vbs /act
```

执行输出记录
```
C:\Windows\system32>cd C:\Program Files\Microsoft Office\Office16

C:\Program Files\Microsoft Office\Office16>cscript ospp.vbs /inpkey:XQNVK-8JYDB-WJ9W3-YJ8YR-WFG99
Microsoft (R) Windows Script Host Version 5.8
Copyright (C) Microsoft Corporation. All rights reserved.

---Processing--------------------------
---------------------------------------
<Product key installation successful>
---------------------------------------
---Exiting-----------------------------

C:\Program Files\Microsoft Office\Office16>cscript ospp.vbs /sethst:42.193.139.7
8
Microsoft (R) Windows Script Host Version 5.8
Copyright (C) Microsoft Corporation. All rights reserved.

---Processing--------------------------
---------------------------------------
Successfully applied setting.
---------------------------------------
---Exiting-----------------------------

C:\Program Files\Microsoft Office\Office16>cscript ospp.vbs /setprt:16886
Microsoft (R) Windows Script Host Version 5.8
Copyright (C) Microsoft Corporation. All rights reserved.

---Processing--------------------------
---------------------------------------
Successfully applied setting.
---------------------------------------
---Exiting-----------------------------

C:\Program Files\Microsoft Office\Office16>cscript ospp.vbs /act
Microsoft (R) Windows Script Host Version 5.8
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

C:\Program Files\Microsoft Office\Office16>
```