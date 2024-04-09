


- 1. install perl
- 2. install qdep

	`pip install qdep`

- 3. make 会提示运行命令:
    windows: `qdep prfgen --qmake C:/Qt/Qt5.14.2/5.14.2/mingw73_32/bin/qmake.exe`

    Ubuntu: `sudo ~/.local/bin/qdep prfgen --qmake /opt/Qt5.14.2/5.14.2/gcc_64/bin/qmake`

- 4. 保证网络可以访问 github.com, 会根据 .pro 中的 QDEP_DEPENDS 配置，去下载对应的依赖项目:
```
QtMvvm\src\mvvmcore\mvvmcore.pro:88
    QDEP_DEPENDS += \
        Skycoder42/QPluginFactory@1.5.0

QtMvvm\src\mvvmdatasynccore\mvvmdatasynccore.pro:55
	android: QDEP_DEPENDS += Skycoder42/AndroidContentDevice@1.1.0

QtMvvm\src\mvvmquick\mvvmquick.pro:37
    QDEP_DEPENDS += \
        Skycoder42/QUrlValidator@1.2.0

QtMvvm\src\mvvmwidgets\mvvmwidgets.pro:53
    QDEP_DEPENDS += \
        Skycoder42/DialogMaster@1.4.0 \
        Skycoder42/QUrlValidator@1.2.0

QtMvvm\src\settingsconfig\settingsconfig.pri:16
    QDEP_DEPENDS += \
        Skycoder42/QXmlCodeGen@1.4.1
```

windows 上下载完之后，会放在: `C:\Users\Admin\AppData\Local\qdep\qdep\Cache`
ubuntu 上放在: `~/.cache/qdep/`

- 5.  `qmake qtmvvm.pro && make 2>&1 | tee .log` 就可以编译出核心库, 放在 `./lib` 目录中,

- 6. 编译 examples : `cd examples && qmake examples.pro && make 2>&1 | tee .log` , 
     即可编译出, 例如可执行文件: `examples/mvvmwidgets/SampleWidgets/SampleWidgets`

运行:

```shell
export LD_LIBRARY_PATH="${LD_LIBRARY_PATH}:${PWD}/lib/"

./examples/mvvmwidgets/SampleWidgets/SampleWidgets
./examples/mvvmquick/SampleQuick/SampleQuick
```

examples app:
```shell
rg 'TEMPLATE = app' ./examples 
./examples/mvvmdatasyncwidgets/DataSyncSampleWidgets/DataSyncSampleWidgets.pro
1:TEMPLATE = app

./examples/mvvmwidgets/SampleWidgets/SampleWidgets.pro
1:TEMPLATE = app

./examples/mvvmquick/SampleQuick/SampleQuick.pro
1:TEMPLATE = app

./examples/mvvmdatasyncquick/DataSyncSampleQuick/DataSyncSampleQuick.pro
1:TEMPLATE = app

./examples/mvvmcore/DroidSettings/DroidSettings.pro
1:TEMPLATE = app

```



# 注意

> 因为 `https://github.com/Skycoder42/QtDataSync/blob/master/src/messages/messages.pro` 中依赖 `Skycoder42/CryptoQQ@2.0.3`,
>
> 而当前没有 `github.com/Skycoder42/CryptoQQ` 仓库了, 导致 `QtDataSync` 无法编译, 
>
> 从而导致 `src/mvvmdatasynccore/` 无法编译 , `src/mvvmdatasyncquick/ src/mvvmdatasyncwidgets/` 依赖 `mvvmdatasynccore` 同样会无法编译，
>
>  最后 examples 中的 `mvvmdatasynccore  mvvmdatasyncquick  mvvmdatasyncwidgets` 都无法编译.

