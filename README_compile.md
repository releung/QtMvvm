


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



# 使用例子

- `https://github.com/releung/QtMvvm_notes` - 使用 `qml` 实现 UI 的



# 理解

## QTMVVM_REGISTER_CORE_APP

> `https://metaso.cn/search/8465988560465297408?q=QTMVVM_REGISTER_CORE_APP`

`QTMVVM_REGISTER_CORE_APP`宏用于将自定义的CoreApp类注册为QtMvvm库中的CoreApp。这一步骤是使用QtMvvm框架时必须进行的，因为它允许你的应用程序从核心部分驱动MVVM应用程序。这个宏要求你使用一个扩展了`QtMvvm::CoreApp`的类1。这意味着你需要在你的核心项目中创建一个自定义的App类，并且让它继承自`QtMvvm::CoreApp`3。这样做的目的是为了执行应用程序的设置并管理ViewModels，以便能够呈现它们3。

此外，`QTMVVM_REGISTER_CORE_APP(T)`宏的定义表明，它期望传入的类型T必须是`QtMvvm::CoreApp`的基类或其派生类1。这是确保你的应用程序能够正确地与QtMvvm框架集成的关键一步。通过这种方式，你可以利用QtMvvm提供的功能来构建和管理你的MVVM应用程序。

总结来说，`QTMVVM_REGISTER_core_APP`是一个宏，用于将自定义的CoreApp类注册为QtMvvm框架的一部分，这是使用QtMvvm进行MVVM开发的一个重要步骤。你需要确保你的CoreApp类继承自`QtMvvm::CoreApp`，以满足这个宏的要求13。

#### 如何创建一个继承自`QtMvvm::CoreApp`的自定义CoreApp类？

要创建一个继承自`QtMvvm::CoreApp`的自定义CoreApp类，首先需要理解QtMvvm库的主要特性。QtMvvm的一个主要特点是它将UI和逻辑分离，允许开发者创建一个包含应用程序逻辑的核心库（core library），以及在此基础上创建多个UI项目8。这意味着自定义CoreApp类应该能够继承并扩展`QtMvvm::CoreApp`的功能，同时保持这种分离性。

具体步骤如下：

1. **创建自定义CoreApp类**：首先，你需要创建一个新的类文件，这个类文件将继承自`QtMvvm::CoreApp`。这可以通过在Qt Creator中创建一个新的类项目来完成，确保在向导中选择合适的基类为`QtMvvm::CoreApp`。
2. **修改项目配置**：为了使你的自定义CoreApp类能够在Qt Creator中正确识别和使用，你可能需要修改项目的`.pro`文件或向导的`.xml`文件，以包含你的新类。这可能涉及到添加对新头文件和源文件的引用，以及任何必要的编译指令或链接设置7。
3. **实现自定义逻辑**：在你的自定义CoreApp类中，你可以添加新的方法或重写现有的方法来自定义应用程序的行为。例如，你可能想要添加新的事件处理逻辑，或者修改核心应用的默认行为。重要的是要保持UI和逻辑的分离，确保所有的业务逻辑都在核心应用中处理，而UI控制器（ViewModels）则负责展示数据和用户交互8。
4. **测试和调试**：最后，不要忘记对你的自定义CoreApp类进行彻底的测试和调试，以确保它按预期工作，并且与QtMvvm库中的其他组件兼容。

通过遵循上述步骤，你可以创建一个继承自`QtMvvm::CoreApp`的自定义CoreApp类，从而扩展和定制你的应用程序的核心逻辑部分。记得利用QtMvvm提供的分离式架构来优化你的应用开发过程。

#### 在QtMvvm框架中，如何管理ViewModels以实现MVVM架构？

在QtMvvm框架中，管理ViewModels以实现MVVM架构主要涉及以下几个方面：

1. **使用依赖注入框架**：通过依赖注入框架来处理ViewModel的依赖关系，这有助于更轻松地管理ViewModel之间的依赖，并提高代码的可维护性和可扩展性10。
2. **数据与组件的双向绑定**：MVVM模式的核心之一是通过ViewModel层实现Model层和View层的双向绑定。这意味着当Model层的数据发生变化时，View层也会相应地更新；反之亦然9。
3. **共用ViewModel以降低设计复杂度**：如果页面中的多个组件都只用于本页面，优先选择共用同一个ViewModel。这样做可以减少设计的复杂度。如果担心某个组件后续可能需要独立出来，也应优先考虑共用同一个ViewModel，待真正需要拆分时再进行重构13。
4. **利用QtMvvm库**：QtMvvm是一个基于MVVM设计模式的库，专门用于创建结构清晰的Qt应用程序。它支持并行创建Widget和Quick项目，有助于开发者高效地实现MVVM架构1218。
5. **理解MVVM架构思想**：MVVM架构思想的核心在于将应用程序分为三个部分：模型（Model）、视图（View）和视图模型（ViewModel）。这三个部分分别承担不同的职责，实现了解耦和职责分离，从而提高了代码的可维护性和可测试性14。

通过上述方法，在QtMvvm框架中管理ViewModels以实现MVVM架构，不仅可以提高开发效率，还能确保应用程序的结构清晰、易于维护和扩展。

#### `QTMVVM_REGISTER_CORE_APP`宏在QtMvvm中的作用和重要性是什么？

`QTMVVM_REGISTER_CORE_APP`宏在QtMvvm中的作用和重要性主要体现在它用于注册核心应用程序。虽然我搜索到的资料并没有直接提到`QTMVVM_REGISTER_CORE_APP`宏，但我们可以根据Qt框架的一般知识来推断其作用。

因此，可以推测`QTMVVM_REGISTER_core_APP`宏的作用类似于在QtMvvm架构下注册一个核心应用程序，确保QtMvvm能够正确地初始化和管理应用程序的核心逻辑。

此外，由于QtMvvm是一个基于MVVM（Model-View-ViewModel）架构的UI框架，它旨在简化用户界面的开发，通过提供数据绑定、命令、观察者等功能来提高开发效率。在这种架构下，注册核心应用程序对于确保应用程序的启动、状态管理和组件通信至关重要。

`QTMVVM_REGISTER_core_APP`宏的重要性在于它为QtMvvm框架提供了一个标准化的方式来注册和初始化核心应用程序，这对于构建高效、可维护的应用程序架构非常重要。通过这种方式，开发者可以更加专注于业务逻辑的实现，而不是花费大量时间在底层的初始化和配置上。

虽然我搜索到的资料没有直接说明`QTMVVM_REGISTER_core_APP`宏的具体作用，但根据Qt框架和MVVM架构的一般知识，我们可以推断出该宏在QtMvvm中的作用和重要性。

#### 使用`QTMVVM_REGISTER_CORE_APP`宏注册CoreApp后，对QtMvvm应用程序有哪些具体影响？

使用`QTMVVM_REGISTER_CORE_APP`宏注册CoreApp后，对QtMvvm应用程序的具体影响主要体现在以下几个方面：

1. **UI与逻辑的分隔**：QtMvvm的设计初衷之一就是实现UI（用户界面）与应用程序逻辑之间的有效分隔。通过使用`QTMVVM_REGISTER_CORE_APP`宏注册CoreApp，开发者可以创建一个核心库，这个核心库包含了应用程序的所有逻辑以及UI控制器（即ViewModels），从而实现了UI与逻辑的分离2021。
2. **支持并行创建多个UI项目**：在QtMvvm中，一旦有了一个包含应用程序逻辑及ViewModels的核心库，开发者就可以在这个基础上并行地创建多个UI项目。这意味着开发者可以为不同的平台或目标设备开发不同的UI版本，而无需重复编写相同的业务逻辑代码。这种能力极大地提高了开发效率和项目的可维护性2021。
3. **灵活性和扩展性**：通过将应用程序逻辑封装在ViewModels中，并利用`QTMVVM_REGISTER_CORE_APP`宏进行注册，QtMvvm应用程序变得更加灵活和易于扩展。开发者可以根据需要添加、修改或删除ViewModels，而不会影响到其他部分的代码。这种设计使得QtMvvm应用程序能够更好地适应不断变化的需求和技术环境2021。

使用`QTMVVM_REGISTER_CORE_APP`宏注册CoreApp后，QtMvvm应用程序能够实现UI与逻辑的有效分隔，支持并行创建多个UI项目，同时提高了应用程序的灵活性和扩展性。这些特点使得QtMvvm成为开发复杂、多平台应用程序的理想选择。