# vmware_vcenter_install

1：安装系统、安装TOOLS、设置IP、破解2008、破除复杂密码、修改机器名、禁IPV6 、增加进入规则、dcpromo (安装AD、DNS）
2：安装系统、安装TOOLS、设置IP、破解2008、破除复杂密码、修改机器名、禁IPV6 、增加进入规则、DB机器加入域，装DB
3：安装系统、安装TOOLS、设置IP、破解2008、破除复杂密码、修改机器名、禁IPV6 、增加进入规则、VC机器加入域，装VC

====IPV6====================
开始 -> 运行 - > 输入 Regedit 进入注册表编辑器
定位到：  
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip6\Parameters]  
右键点击 Parameters，选择新建 -> DWORD (32-位)值  
命名值为 DisabledComponents，然后修改值为 ffffffff (16进制)
=====进入规则===================
all - 本地IP (172.168.1.0/24)
=======安装DB
1:服务管理器--功能--添加功能
   安装 .net Framework 3.5 SP1
2:安装完DB,建库,后要加条防火墙入站规则.

=========VC配置DB==
在VC上安装 ****/x64/setup/x64/sqlncli
建ODBC,系统DSN--添加 SQL SERVER NATIVE CLIENT 10.0
开始安装VC
1：SSO  域名:vsphere.local  用户名: administrator  密码:Liguanghai9?

administrator@vshpere.local
<https://xcvc.zjxcvm.com:7444/lookupservice/sdk>

完全限制域名 xcvc.zjxcvm.com

<https://xcvc.zjxcvm.com:10443>

==配置32位ODBC 为update manager
C:\Windows\SysWOW64\odbcad32.exe
