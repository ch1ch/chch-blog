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
就是记得挂载目录前先
```C#
nn.Result result = nn.fs.SaveData.Mount(mountName, userId);
```
挂载后再
```C#
nn.fs.FileSystem.Unmount(mountName);
```








