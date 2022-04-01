因为近期开始UNITY移植NS过程，积累了一些坑，这方面中文又资料太少，故还是写一些吧。

## SDK安装和硬件连接
我们是基于
*UNITY2018.4.36LTS
*NX 13.1.0

第一步肯定是拿到https://developer.nintendo.com/ 的账号，我们作为开发者，最开始写了申请，任天堂就驳回了，说，你们不行，必须发行来申请，后来我们还是通过发行才拿到了账号。
进来从Download下载最新Nintendo Dev Interface 
<img width="950" alt="1648802652(1)" src="https://user-images.githubusercontent.com/3361015/161228627-76730ff0-a103-4566-8c5c-d5619e52e9a6.png">


然后就是运行，输入账号。选择你要安装什么
<img width="419" alt="1648802756(1)" src="https://user-images.githubusercontent.com/3361015/161228762-9e940ede-accf-4ee3-a510-badf24205871.png">
我第一眼的时候犹豫很久选了第一个看起来最像的。
<img width="328" alt="1648802770(1)" src="https://user-images.githubusercontent.com/3361015/161228845-a4e93e26-07f0-4915-8203-ae4eb0b0252b.png">
然后就是继续第一个。
<img width="422" alt="1648802806(1)" src="https://user-images.githubusercontent.com/3361015/161228905-f51cf107-6085-44c4-975e-43c41ba39e52.png">
选环境，我们嘛，unity
<img width="512" alt="1648802838(1)" src="https://user-images.githubusercontent.com/3361015/161228994-57519c23-f5bb-4fbf-888a-6fb6009cf719.png">
这块，不同版本都有，直接搜索安装就行。
这块后来我觉得很神奇。因为不同版本都有它的sample.还都是可以撒欢跑的。这每一次SDK升级所有unity版本都要做一遍，这个工作量也有点太大了……
<img width="501" alt="1648802983(1)" src="https://user-images.githubusercontent.com/3361015/161229418-0114db20-97ba-430e-81f7-6fe7af6ddcda.png">
默认安装一堆东西。还非常之大……

然后就是安装。。。其实还挺快的。
然后安装完就可以连接开发机试试
![image](https://user-images.githubusercontent.com/3361015/161232061-b3882689-163a-44ba-b1d2-39308e7ef1aa.png)

<img width="139" alt="1648803950(1)" src="https://user-images.githubusercontent.com/3361015/161232247-f4b8c0e9-557f-4f49-9cc7-0e4582a1aca2.png">
这两个都可以，我用的1没有用2.
不过如果连接不上,可以从Nintendo Dev Interface  里
<img width="194" alt="1648804026(1)" src="https://user-images.githubusercontent.com/3361015/161232492-c8c0b763-a35d-4ba4-ab24-61c2d7862457.png">
试试EDEV模式。

正常情况就能从Nintendo Target Manager里边看到设备了。
![image](https://user-images.githubusercontent.com/3361015/161245411-778f8156-8680-44a9-86cb-472c81fb5fec.png)



## 开发者后台设置

主要需要在这里添加一个产品获得一个APPID
![image](https://user-images.githubusercontent.com/3361015/161246239-cf2ac718-64a9-4808-a0da-ab90cc94ec41.png)


## unity部分

先安装unity的switch支持
就在上边选的那个目录里
<img width="174" alt="1648806070(1)" src="https://user-images.githubusercontent.com/3361015/161238587-b8c9f2f6-f8ff-4c8b-a53b-2da5e322dd74.png">
UnityForNintendoSwitch里边
<img width="356" alt="1648806086(1)" src="https://user-images.githubusercontent.com/3361015/161238636-ffd5894c-2897-464d-84e3-ff766b3a77bf.png">
基本就知道安装哪个了。

到unity里边切到switch环境
![image](https://user-images.githubusercontent.com/3361015/161245609-037d8154-1e88-4b4d-900b-904f77e6064f.png)
主要就是这些设置。
APPID就是上边获得的，然后下班还有两处复制就行。


### 关于存档

这块我参考了https://zhuanlan.zhihu.com/p/86218073
以及上文的安装目录里的UnityForNintendoSwitch\Samples\NintendoSDKPlugin\FsSaveData 这个demo

Save data Size和Journal Size需要设置成16384的倍数。
最大不能超过4MB
那些代码基本可用。不过就是需要主要
代码里的saveDataSize = 458752;要按上文设置成unity里边的减去32*1024

第一次使用的时候记得初始化，按照demo里的Initialize函数那么做就行

然后挂载目录
```C#
nn.Result result = nn.fs.SaveData.Mount(mountName, userId);
```

写文件还比较简单
```C#
 byte[] dataByteArray;
  using (MemoryStream stream = new MemoryStream(saveDataSize)) // the stream size must be less than or equal to the save journal size
  {
      BinaryWriter binaryWriter = new BinaryWriter(stream);
      binaryWriter.Write(GameData);
      stream.Close();
      dataByteArray = stream.GetBuffer();
  }
 ```     
因为我会往这个目录里塞很多文件，所以还需要拼接目录，再执行操作，这块我之前还范蠢把Savefilename里边带了一个 "/"符号，不要带
```C#
filePath = string.Format("{0}:/{1}", mountName, Savefilename);
result = nn.fs.File.Create(filePath, saveDataSize);
result.abortUnlessSuccess();

result = nn.fs.File.Open(ref fileHandle, filePath, nn.fs.OpenFileMode.Write);
result.abortUnlessSuccess();

result = nn.fs.File.Write(fileHandle, 0, dataByteArray, dataByteArray.LongLength, nn.fs.WriteOption.Flush);
result.abortUnlessSuccess();

nn.fs.File.Close(fileHandle);
result = nn.fs.FileSystem.Commit(mountName);

result.abortUnlessSuccess();
```
结束所有操作再
```C#
nn.fs.FileSystem.Unmount(mountName);
```


## 关于读档

读档相对简单，按照demo，判断有没有初始化，然后按照DEMO谢就行。
这块因为我是每次存档都会挂载卸载目录，读档这里也是这么做。你如果一直挂在那，也可以不用再挂载。

读取的直接我因为比较大，就读出来字符串然后，再转化成JSON
```C#
byte[] datafile = new byte[fileSize];
        result = nn.fs.File.Read(fileHandle, 0, datafile, fileSize);
        result.abortUnlessSuccess();
        nn.fs.File.Close(fileHandle);
        string str;
        using (MemoryStream stream = new MemoryStream(datafile))
        {
            BinaryReader reader = new BinaryReader(stream);
            str = reader.ReadString();
            Debug.Log(str);
        }
```

基本就完事。


## 关于操作
这块我们目前比较粗暴
放入SDK
UnityForNintendoSwitch\Plugins\NintendoSDKPlugin\Libraries\NintendoSDKPlugin.unitypackage

这些放头
```C#
private nn.hid.NpadState npadState;
private nn.hid.NpadId[] npadIds = { nn.hid.NpadId.Handheld, nn.hid.NpadId.No1 };
```    
这些放start
```C#
nn.hid.Npad.Initialize();
nn.hid.Npad.SetSupportedStyleSet(nn.hid.NpadStyle.Handheld | nn.hid.NpadStyle.JoyDual);
nn.hid.Npad.SetSupportedIdType(npadIds);
npadState = new nn.hid.NpadState();
 ``` 
 
然后把这些扔进Update
```C#
   for (int i = 0; i < npadIds.Length; i++)
        {
            nn.hid.Npad.GetState(ref npadState, npadIds[i], nn.hid.Npad.GetStyleSet(npadIds[i]));
            if ((npadState.buttons & nn.hid.NpadButton.Y) != 0)
            {
            }
            else if ((npadState.buttons & nn.hid.NpadButton.B) != 0)
            {
                TestBox.SetActive(false);
            }
            else if ((npadState.buttons & nn.hid.NpadButton.A) != 0)
            {
                TestBox.SetActive(true);
            }
        }
```
就对应按键啦……
后期肯定要改，先跑起来再说吧

## 其他要说的

还要一些配置环境，也需要考虑一下然后安装，因为我这个版本有个bug就是无法build。后来发现是.net core sdk没安装，可以
在NintendoSDK\Tools\SdkEnvironmentChecker里边有个SdkEnvironmentChecker.exe 可以运行看看
<img width="263" alt="1648810367(1)" src="https://user-images.githubusercontent.com/3361015/161249939-e868fefb-a9d3-4fae-a88d-78b196156e97.png">
这个东西也不用都安装，我这还有几个报红，也都跑起来正常…

另外我做过版本的unity有个bug就是真机会紫屏。这个好像在2018d的unity是通病。
https://developer.nintendo.com/html/online-docs/g1kr9vj6-en/Packages/middleware/UnityForNintendoSwitch/Documents/contents-en/Pages/Page_417572333.html
官方有各个版本的已知bug列表。
问题解决也比较简单.
安装正常版vs（不是我常用的vscode）,
里边安装个
`On the Workloads tab, select Universal Windows Platform development.
On the Individual Components tab, select Graphics debugger and GPU profiler for DirectX.`
也就好了




