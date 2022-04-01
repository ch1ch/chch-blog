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


## unity部分


