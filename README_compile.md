


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

