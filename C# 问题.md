<center><h1>C# 问题</h1></center>

- 无法向会话状态服务器发出会话状态请求

  - 解决：

    - 1 ：在服务中开启  **`ASP.NET State Service`** 服务

    - 2 ：如果第一步没用==>注册表没有改：--`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\aspnet_state\Parameters\AllowRemoteConnection`

      `AllowRemoteConnection` ,0仅能本机使用,1可以供其他机器使用.

    - 3 ：防火墙 :**防火墙全部关闭掉**

--------

- 谷歌浏览器**`chrome`**不再支持**`showmodaldialog`**的解决方案
  - [解决](https://www.cnblogs.com/wangfuyou/p/5044194.html)

------

- VS **当前不会命中断点 还没有为该文档加载任何符号**

  - 解决 ： 菜单栏中找到“项目”->“项目属性页”,从左边展开“配置属性”,点击“生成”,将“生成调试信息”前面的勾勾上就OK了。

  - **当点击添加依赖项的时候报，会出现循环依赖项错误**。

    - ![img](https://img-blog.csdn.net/20170422103144109?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZHV5dXNlYW4=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

    - 解决 ： 我想要做的是在LoginUI中引用LoginBLL，但此时LoginBLL中已经引用了LoginUI，所以只需要将LoginBLL中的LoginUI引用删除掉就OK了。

---------

- 使用注册表，写入电脑唯一标识；

  ```c#
          private string IsRegeditKeyExit()
          {
              string info;
              string[] sKeyNameColl;
              RegistryKey Key = Registry.LocalMachine;
              //RegistryKey localKey = RegistryKey.OpenRemoteBaseKey(Microsoft.Win32.RegistryHive.LocalMachine, Mac);
              if (Environment.ProcessorCount==64)
                  Key = RegistryKey.OpenRemoteBaseKey(RegistryHive.LocalMachine,"64");
              else
                  Key = RegistryKey.OpenRemoteBaseKey(RegistryHive.LocalMachine, "32");
              RegistryKey software = Key.OpenSubKey(@"SOFTWARE", true);
              if (software != null)
              {
                  sKeyNameColl = software.GetSubKeyNames();
              }
              else
              {
                  return "d";
              }
              try 
              { 
                  sKeyNameColl = software.GetSubKeyNames();
              }
              catch (InvalidCastException ex)
              {
                  Console.WriteLine(ex.Message);
              }
              sKeyNameColl = software.GetSubKeyNames();
              foreach (string keyName in sKeyNameColl)  //遍历整个数组
              {
  
                  if (keyName == "YourSoftware") //判断子项的名称
                  {
                      info = GetRegedit();
                      break;
                  }
  
              }
  
              //if (product.GetValue("IPaddressComputer") == null)
              //{
              //    info = CreateRegedit();
              //    return info;
              //}
              Key.Close();
              info = CreateRegedit();
              return info;
  
          }
  
          //创建注册表
          private string CreateRegedit()
          {
              RegistryKey key = Registry.LocalMachine;
              RegistryKey software = key.OpenSubKey("SOFTWARE", true);
              RegistryKey product = software.CreateSubKey("YourSoftware");  //创建注册表
              key.Close();
              string info = SetRegedit();
              return info;
          }
  
          //创建键值
          private string SetRegedit()
          {
              RegistryKey key = Registry.LocalMachine;
              RegistryKey software = key.OpenSubKey("YourSoftware", true); //该项必须已存在 true,代表可以编辑；
              software.SetValue("IPaddressComputer", Guid.NewGuid().ToString("N"));//创建一个GUID标识
              key.Close();
              string info = GetRegedit();
              return info;
          }
  
          //读取键值
          private string GetRegedit()
          {
              string info = "";
              RegistryKey Key = Registry.LocalMachine;
              RegistryKey software = Key.OpenSubKey("SOFTWARE", false);
              RegistryKey product = software.OpenSubKey("YourSoftware", false);
              info = product.GetValue("IPaddressComputer").ToString();
              Key.Close();
              return info;
          }
  	 
  
  ```

  - [ 32位，64位注册表的区别](https://www.rhyous.com/2011/01/24/how-read-the-64-bit-registry-from-a-32-bit-application-or-vice-versa/)

  - [32位程序64位程序读\写注册表的区别](https://www.cnblogs.com/mingmingruyuedlut/archive/2011/01/20/1940371.html)
  - [32位程序访问64位注册表](https://www.cnblogs.com/TaiYangXiManYouZhe/p/5087248.html)
  - [关于64位操作系统使用C#访问注册表失败的问题](https://www.cnblogs.com/TaiYangXiManYouZhe/p/5086974.html)

------

- **获取 电脑的唯一mac地址**（`mac不是唯一的`）

  - 第一种方法 ：

    ```c#
    using System.Net.NetworkInformation;
    
    public string GetMacAddress()
            {
                string physicalAddress = "";
                NetworkInterface[] nice = NetworkInterface.GetAllNetworkInterfaces();
                foreach (NetworkInterface adaper in nice)
                {
                    if (adaper.Description == "en0")
                    {
                        physicalAddress = adaper.GetPhysicalAddress().ToString();
                        break;
                    }
                    else
                    {
                        physicalAddress = adaper.GetPhysicalAddress().ToString();
                        if (physicalAddress != "")
                        {
                            break;
                        };
                    }
                }
                return physicalAddress;
            }
    ```

  - 第二种方法：

    ```c#
    //通过IP地址获取MAC地址的方法（可跨网段获取）       
    string GetMac(string IP)
     {
         string dirResults = "";
         ProcessStartInfo psi = new ProcessStartInfo();
         Process proc = new Process();
         psi.FileName = "nbtstat";
         psi.RedirectStandardInput = false;
         psi.RedirectStandardOutput = true;
         psi.Arguments = "-A " + IP;
         psi.UseShellExecute = false;
         proc = Process.Start(psi);
         dirResults = proc.StandardOutput.ReadToEnd();
         proc.WaitForExit();
         dirResults = dirResults.Replace("\r", "").Replace("\n", "").Replace("\t", "");
         Regex reg = new Regex("Mac[ ]{0,}Address[ ]{0,}=[ ]{0,}(?<key>((.)*?))__MAC", RegexOptions.IgnoreCase | RegexOptions.Compiled);
         Match mc = reg.Match(dirResults + "__MAC");
     
         if (mc.Success)
         { return mc.Groups["key"].Value; }
         else
         {
             reg = new Regex("Host not found", RegexOptions.IgnoreCase | RegexOptions.Compiled);
             mc = reg.Match(dirResults);
             if (mc.Success)
             {
                 return "Host not found!";
             }
             else
             { return ""; }
         }
     }
    
    //没试过，网上找到  跨网段获取mac地址
    ```

    [跨网段获取mac地址](https://www.cnblogs.com/linyc/archive/2011/04/02/2002849.html)

  - 第三种方法 ：

    ```html
    //通过HTML标签获取IP和mac地址
    <HTML>
    <HEAD>
    <TITLE>WMI Scripting HTML</TITLE>
    <META http-equiv=Content-Type content="text/html; charset=gb2312">
    <SCRIPT language=JScript event="OnCompleted(hResult,pErrorObject, pAsyncContext)" for=foo>
    document.forms[0].txtMACAddr.value=unescape(MACAddr);
    document.forms[0].txtIPAddr.value=unescape(IPAddr);
    document.forms[0].txtDNSName.value=unescape(sDNSName);
    </SCRIPT>
    
    <SCRIPT language=JScript event=OnObjectReady(objObject,objAsyncContext) for=foo>
    if(objObject.IPEnabled != null && objObject.IPEnabled != "undefined" && objObject.IPEnabled == true)
    {
    if(objObject.MACAddress != null && objObject.MACAddress != "undefined")
    MACAddr = objObject.MACAddress;
    if(objObject.IPEnabled && objObject.IPAddress(0) != null && objObject.IPAddress(0) != "undefined")
    IPAddr = objObject.IPAddress(0);
    if(objObject.DNSHostName != null && objObject.DNSHostName != "undefined")
    sDNSName = objObject.DNSHostName;
    }
    </SCRIPT>
    
    <META content="MSHTML 6.00.2800.1106" name=GENERATOR></HEAD>
    <BODY>
    <OBJECT id=locator classid=CLSID:76A64158-CB41-11D1-8B02-00600806D9B6 VIEWASTEXT></OBJECT>
    <OBJECT id=foo classid=CLSID:75718C9A-F029-11d1-A1AC-00C04FB6C223></OBJECT>
    <SCRIPT language=JScript>
    var service = locator.ConnectServer();
    var MACAddr ;
    var IPAddr ;
    var DomainAddr;
    var sDNSName;
    service.Security_.ImpersonationLevel=3;
    service.InstancesOfAsync(foo, ''Win32_NetworkAdapterConfiguration'');
    </SCRIPT>
    
    <FORM id=formfoo name=formbar action=NICPost.asp method=post>
    <INPUT value=00:05:5D:0E:C7:FA name=txtMACAddr> 
    <INPUT value=192.168.0.2 name=txtIPAddr> 
    <INPUT value=typ name=txtDNSName> 
    </FORM>
    </BODY>
    </HTML>
    ```

  

  - 因有跨网段的电脑登录系统所以，获取mac不现实，但获取IP会随时变换所以想着如果能存住最开始的IP不就OK了

  - **C# cookie 持久化**

    ```C#
    //设置
    HttpCookie cookie = new HttpCookie("cookieName"); 
    cookie.Value = "name1"
    HttpContext.Current.Response.Cookies.Add(cookie); 
    //读取
    HttpContext.Current.Request.Cookies["cookieName"].Value
    //检测cookie是否存在
    if(HttpContext.Current.Request.Cookies["cookieName"]==null){
    //do something
    }
    //设置cookie有效期
    cookie.Expires = DateTime.Now.AddDays(1);
    ```

    [C# 设置cookie](https://www.cnblogs.com/cpcpc/archive/2012/09/10/2679098.html)

  - **JavaScript Cookie 持久化**

    ```javascript
    var username=document.cookie.split(";")[0].split("=")[1];
    //JS操作cookies方法!
    //写cookies
    function setCookie(name,value)
    {
        var Days = 30;
        var exp = new Date();
        exp.setTime(exp.getTime() + Days*24*60*60*1000);
        document.cookie = name + "="+ escape (value) + ";expires=" + exp.toGMTString();
    }
    //读取cookie
    function getCookie(name)
    {
        var arr,reg=new RegExp("(^| )"+name+"=([^;]*)(;|$)");
        if(arr=document.cookie.match(reg))
        return unescape(arr[2]);
        else
        return null;
    }
    //删除cookie
    function delCookie(name)
    {
        var exp = new Date();
        exp.setTime(exp.getTime() - 1);
        var cval=getCookie(name);
        if(cval!=null)
        document.cookie= name + "="+cval+";expires="+exp.toGMTString();
    }
    //使用示例
    setCookie("name","hayden");
    alert(getCookie("name"));
    //如果需要设定自定义过期时间
    //那么把上面的setCookie　函数换成下面两个函数就ok;
    //程序代码
    function setCookie(name,value,time)
    {
        var strsec = getsec(time);
        var exp = new Date();
        exp.setTime(exp.getTime() + strsec*1);
        document.cookie = name + "="+ escape (value) + ";expires=" + exp.toGMTString();
    }
    function getsec(str)
    {
        alert(str);
        var str1=str.substring(1,str.length)*1;
        var str2=str.substring(0,1);
        if (str2=="s")
        {
        	return str1*1000;
        }
        else if (str2=="h")
        {
        	return str1*60*60*1000;
        }
        else if (str2=="d")
        {
        	return str1*24*60*60*1000;
        }
    }
    //这是有设定过期时间的使用示例：
    //s20是代表20秒
    //h是指小时，如12小时则是：h12
    //d是天数，30天则：d30
    setCookie("name","hayden","s20");
    ```

    [JS  设置Cookie 持久化](https://www.cnblogs.com/endv/p/8089506.html)

---------

- **通过JS检测本地是否存在文件（file API）**行不通  `文件读取只能用户主动触发`

  ```javascript
  //判断文件是否存在
  function isExistFile(url)  
      {      
          var xmlHttp ;  
          if (window.ActiveXObject)  
           {  
            xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");  
           }  
           else if (window.XMLHttpRequest)  
           {  
            xmlHttp = new XMLHttpRequest();  
           }   
          xmlHttp.open("post",url,false);  
          xmlHttp.send();  
          if(xmlHttp.readyStatus==4){
              if(xmlhttp.status==200)return true;//url存在
              else if(xmlhttp.status==404)return false;//url不存在
              else return false;//其他状态
          }  
          return false;  
          else  
          return true;  
      }
      $(function(){
          alert(isExistFile("./svg/svgFile/主页模板.svg"));
      })
  ```

  - [W3C FileAPI](https://w3c.github.io/FileAPI/)

------

- **SQL 中使用正则表达 匹配数据**
  - [SQL语句与正则表达式](https://www.cnblogs.com/renzaijianghu/p/5666750.html)

- **C# 打印机 预览**
  - [C#实现打印与打印预览功能](<https://www.cnblogs.com/xiaofengfeng/p/3473479.html>)

