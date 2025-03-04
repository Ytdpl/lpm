# LFS Package Manager (highly customizable maybe)

LFS包管理器的某种构想。

An English version of README.md will be provided later on. 

## 构想

目前的想法十分简单。整个项目正常运行的目录树大概长这样：

```
/
├── etc
│   ├── lpm.conf
│   └── lpm.d
├── usr
│   └── src
│       └── lpm
│           └── <some packages>
└── var
    └── lpm
        ├── local
        │   └── (pakcage name)
        │       └── x.y.z-abc
        │           ├── buildlog
        │           ├── errorlog
        │           └── pkginfo
        └── pkglist
```

目前觉得`lpm.d`没啥用，不过先留着当未来拓展接口也行。

其中的`pkginfo`提供包的基本信息和一般构建方法，基本会参考`Arch Linux`的`PKGBUILD`文件编写；`buildlog`记录编译过程中的全部记录供参考，`errorlog`将编译过程中的错误消息单独记录下来。

`/usr/src`下的`lpm`文件夹是可选的，可以合并或者为`lpm`单独设置源码夹。

`/var/lpm`下设置`local`的想法是可能之后会开一个新的仓库，每天更新包信息，所以可以再设置类似`remote`或者`sync`之类的夹。但是这样是否偏离LFS的主旨太多还有待考虑。

目标还包括高度可定制。LFS Book没有提供包管理器的另一原因是包管理器各有优劣，不能满足所有人的需求。所以对于[LFS Book提到的各种包管理技术](https://linuxfromscratch.org/lfs/view/stable-systemd/chapter08/pkgmgt.html)（除了用脑子记，这诗人啊）我都希望给出某种配置方式以满足不同需求。

## 接口

```
lpm   -h,-? --help                    打印此信息
      -b --build  <file>              从文件提供的pkginfo编译并安装包
      -c --check                      读取本地已安装的包并更新本地数据库
      -Q --query  <pkgname>           查询本地包数据
      -R --remove <pkgname> [pkgver]  删除某包[的某个版本]
      -u --update [pkgname]           检查本地包更新，若有更新则下载、编译并安装
         --update-check               只检查更新而不下载
         --update-download            只检查并下载而不编译安装
      -v --verbose                    详细输出
      -V --version                    打印lpm版本信息
```

## 起因

起因没有按照逻辑顺序放最前面是因为点进来看的大部分对起因没啥兴趣。但是记录在这里也保持了完整性。权作跋。

最近花了大量的时间在构建LFS上。虽然LFS Book非常详细地给出了命令，我还是踩了不少次坑，特别是为UEFI安装GRUB的时候。主书一个个地给出了编译安装的顺序，但是在BLFS中，所有的包依赖关系都要自己处理，一层一层的依赖真是头疼。想当时手动安装Arch Linux的时候，网络条件正常的情况下最快都只要半个小时就能完成桌面系统的安装，LFS光为了能引导就耗了我不下6个小时。（当然应该也有部分原因是我不太熟悉构建过程，以及那几天整天不务正业醒着的时间全拿来搞这个导致最后已经成为麻木的代码搬运工）很怀念`pacman`和各种`aur helper`。于是（很自然地）有了这个想法。

不过本人学业压力也不小，所以很可能一鸽就像许多其他项目一样好几年都没进展（
而且不是计科专业的，所以代码可能写的依托，还请见谅。