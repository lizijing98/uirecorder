UIRecorder PC标准入门
===================

如何录制正常网页操作步骤？
-------------------

1. 首先安装UIRecorder，这个请参考项目介绍首页：[https://github.com/alibaba/uirecorder](https://github.com/alibaba/uirecorder)
2. 创建新目录并进入，然后初始化UIRecorder工程目录：`uirecorder init`， 根据具体情况输入相关的参数，几个回车后，我们的准备工作就结束了，这里详细介绍下几个参数分别是什么作用

    1. `Path扩展属性配置,除id,name,class之外`: 此参数，用来定义网页DOM中哪些属性可以用来定位我们的控件，默认值已经包括了主流的属性，一般情况下不需要修改
    2. `属性值黑名单正则`：网页中当某些属性值是随机或者不稳定的时候，我们可以通过这个配置来忽略那些属性值，从而保证我们的自动化脚本稳定性，例如我们可以这样配置：`/attr_name_\d+/`，如果需要配置多个属性，可以这么写：`/attr1_\d+|attr2_\d+|/`，详细更多的正则教程请自行搜索
    3. `class值黑名单正则`：某些场景下，class属性的值会干扰自动化录制，比如当鼠标移到某个控件上时会动态的添加一个class样式名称，就可能会被录制到PATH中，从而导致自动化无法稳定重放。此时就可以将那个class名称加到黑名单中，例如:`/class_name/`，详细值的配置请参考属性值黑名单
    4. `断言前隐藏`：某些业务场景下，会在最上层显示一层透明的div，用来实现一些高级的富应用需求，会导致无法直接断言透明div下方的控件。此种场景下，可以将透明div的PATH配置在此属性中，即可实现在断言时自动移除透明的div，断言后再自动恢复
    5. `WebDriver域名或IP`：WebDriver执行机的IP地址，默认是指向本地，如果有部署grid执行机集群，可以指向对应的执行机集群IP
    6. `WebDriver端口号`：WebDriver端口号，一般情况下是4444端口号，不需要修改
    7. `需要同时测试的浏览器列表`： 需要同时测试的浏览器列表，例如：chrome, firefox, ie 11，可以根据本地或远程执行机集群所支持的浏览器类型自行输入

3. 输入`uirecorder`，会要求输入相关的几个参数， 我们这里介绍下几个参数的意义

    1. `测试脚本文件名`: 录制后保存到哪个文件，支持直接输入目录名，例如：test/aaa.spec.js，我们要求脚本文件名必需是xxx.spec.js，否则会导致无法定位到
    2. `打开同步校验浏览器？`：校验浏览器是用来实现边录边跑，同步的校验录制的步骤是否能够稳定的重现，避免录制后进行大量的调试。从而实现一次录制，即可稳定重现，极大的提高成功率和稳定性。默认值已经是打开，按回车即可。
    3. `浏览器大小`：默认值是maximize，也即是最大化，如果对于浏览器窗口大小没有特殊要求，用默认值回车即可。如果某些场景要求必需特殊的大小，那么可以自己指定特殊的大小，例如：1026 x 768

    当以上4个参数确认后，我们将看到一个黑背影的开始界面。此时，我们仅需输入需要测试的目标网址，即可正式开始我们的录制之旅。

常规录制说明：

1. 对于常规的网页操作行为，我们没有任何学习门槛，按照正常的网页操作步骤进行即可，UIRecorder会在后台默默的记录并生成自动化脚本
2. 除了鼠标的悬停操作，我们需要通过录制面板，在目标控件上手工添加一个悬停。例如，二级菜单就需要鼠标移到一级菜单后，才能在二级菜单进行点击操作。
3. 如何录制三级甚至四级的多级菜单？

    按住`Ctrl/Command键`，然后点击`添加悬停`按钮，即可进入持续悬停模式。进入此模式后，每次添加悬停后，会持续处于事件拦截状态，不用担心鼠标移动会导致二级菜单缩回，然后就可以持续的添加更多的悬停操作。

4. 如何在悬停后断言？

    进入`持续悬停模式`后，同时也可以解决悬停后的断言问题。如果希望结束`持续悬停模式`，请再次点击`结束悬停`按钮，即可退出此模式。

如何添加断言？
-------------------

当进行一系列操作后，往往我们需要添加一系列的断言，用来判断操作后的结果是不是正确的。

在操作录制后，我们仅需点击录制面板上的`添加断言`按钮即可，点击按钮后，我们可以发现有以下几个选项：

1. `延迟时间`：用来设置本次断言延迟一定的时间后才执行，一般用来解决某些异步操作时间不确定的问题。
2. `断言类型`：我们提供子丰富的断言类型，不同的控件类型要选择合适的断言类型，这里针对每个断言类型进行介绍

    1. `val`: 断言输入框的值
    2. `text`: 断言文本的内容
    3. `displayed`: 断言控件是否处于显示状态
    4. `enabled`: 断言当前控件是否可用(没有禁用)
    5. `selected`: 断言当前控件是否打勾选中了
    6. `attr`: 断言当前DOM的属性值
    7. `css`: 断言当前DOM的CSS值
    8. `url`: 断言当前网页的URL地址
    9. `title`: 断言当前网页的title标题
    10. `cookie`: 断言当前网页的cookie值
    11. `localStorage`: 断言当前网页的localStorage
    12. `sessionStorage`: 断言当前网页的sessionStorage
    13. `alert`: 断言弹出的alert窗口的提示文本
    14. `jscode`: 在浏览器端执行自定义的JS代码，断言JS代码的返回值
    15. `count`: 断言控件匹配的数量
    16. `imgdiff`: 断言当前控件的图片差异，可以自定义图片差异的百分比

3. `断言DOM`：当前DOM控件的PATH，录制时自动生成，也可以自己进行修改，以在某些特殊场景下进行微调
4. `比较方式`: 针对读取到的值，如何进行断言，我们解释下每个比较方式

    1. `equal`: 相等
    2. `notEqual`： 不相等
    3. `contain`: 包含，目标值包含另外一个值
    4. `notContain`： 不包含
    5. `above`: 大于，用于断言数值大于某个值
    6. `below`: 小于，用于断言数值小于某个值
    7. `match`: 匹配正则，一般用于高级断言，例如：/aaa\d+bbb/
    8. `notMatch`: 不匹配正则

5. `断言结果`: 判断的目标值

其它录制功能介绍
--------------------

1. `属性开关`：对于某些特殊场景，可以通过灵活的开启或关闭属性项，不同步骤选择不同的PATH生成策略，例如：某些控件不太适合用文本定位，就可以临时先关闭`text`属性项
2. `属性值黑名单`: 可以即时配置属性值黑名单，立即生效，用来解决随机属性值的问题，修改后记得按一下回车键，让变更立即生效，格式如下：`/aaa_bbb_\d+/`
3. `执行JS`: 可以用来在浏览器端执行一些扩展功能，例如：`document.title=123;`
4. `添加延迟`：某些操作后，会发起异步请求，异步请求的完成时间不太确定时，可以通过添加延迟时间提升稳定性，时间单位是毫秒
5. `脚本跳转`：用来跳到新的URL，或者跳到另外一个脚本，一般情况下用来引用公共脚本，例如登录操作

如何部署WebDriver服务？
----------------

1. 本地部署Selenium standalone server:

    新开控制台窗口然后执行

    > `npm run server`

2. Selenium Grid: [https://github.com/SeleniumHQ/selenium/wiki/Grid2](https://github.com/SeleniumHQ/selenium/wiki/Grid2)
3. F2etest: [https://github.com/alibaba/f2etest](https://github.com/alibaba/f2etest)

录制后如何运行脚本？
--------------------

1. 运行所有脚本: `source run.sh` ( Linux|Mac ) 或 `run.bat` ( Windows )
2. 运行单个脚本: `source run.sh sample/test.spec.js` ( Linux|Mac ) 或 `run.bat sample/test.spec.js` ( Windows )

如何临时基于本地浏览器调试脚本?
----------------

对于常态基于远程执行机跑的场景下，出现问题时，往往需要在本机进行调试，我们可以通过设置环境变量，临时基于本机的浏览器来调试。

1. `export webdriver=127.0.0.1:4444` 或 `set webdriver=127.0.0.1:4444` (Windows)

提示：端口号是非必填项，例如：`export webdriver=127.0.0.1`