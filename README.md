# OpenHarmony Community
欢迎来到OpenHarmony社区！

## 1. 仓的分类说明1

为了支持按照不同类型下载代码，OpenHarmony为每个代码仓定义了以下类别：

| 分类           | 分类说明                                                     | group         |
| -------------- | ------------------------------------------------------------ | ------------- |
| **轻量系统仓** | 适用于轻量系统的代码仓                                       | ohos:mini     |
| **小型系统仓** | 适用于小型系统的代码仓                                       | ohos:small    |
| **标准系统仓** | 适用于标准系统的代码仓                                       | ohos:standard |
| 系统组件仓     | 标准系统中与硬件无关的代码仓，构建产物都部署在/system目录下。 | ohos:system   |
| 芯片组件仓     | 标准系统中与芯片或硬件相关的仓，构建产物部署在/vendor或/chipset目录下。 | ohos:chipset  |

一个仓可以归属于多个group，如下代码所示，groups中多个group以","连接在一起。

```xml
<project name="inputmethod_imf" path="base/miscservices/inputmethod" groups="ohos:standard,ohos:system"/>

<project name="ai_engine" path="foundation/ai/ai_engine" groups="ohos:mini,ohos:small,ohos:standard,ohos:system"/>
```

每个仓可以适用于mini,small或standard系统，如果适用于standard系统，同时需要明确是ohos:system类型还是ohos:chipset类型。

每个仓都需要加入默认的default组。

## 2. 平台仓和芯片仓

OpenHarmony会支持越来越多的芯片平台，每个芯片平台会在device和vendor目录下创建相应的仓；为了区分，我们把这类仓称为芯片仓，其它的仓称为平台仓。平台仓和芯片仓具有不同的生命周期，芯片仓可能会随着硬件的演进而逐渐废弃，而平台仓相对与具体的硬件关系不大，生命周期相对更长。

平台仓都组织在**manifests/ohos/ohos.xml**文件中，而芯片仓都组织在**manifests/chipsets/** 目录下。全量的代码仓组织形式如下所示：

```
manifest
    ├── default.xml
    │   └── ohos
    │   └── ohos.xml
    └── chipsets
        ├── all.xml
        ├── chipset1.xml
        ├── chipset2.xml
        ├── chipsetN.xml
        ├── chipset1
        │   └── chipset1-detail.xml
        ├── chipset2
        │   └── chipset2-detail.xml
        └── chipsetN
            └── chipsetN-detail.xml
```

- default.xml由ohos/ohos.xml和chipsets/all.xml组成，是所有平台仓和芯片仓的集合；通过此方式可以下载所有代码仓。

- chipsets/chipsetN/chipsetN-detail.xml是单个芯片平台所引入的仓集合，chipsets/chipsetN.xml是由ohos/ohos.xml和chipsetN-detail.xml仓组合而成，用于下载该芯片平台的全量仓。

- chipsets/all.xml则是所有芯片平台xxx/xxx-detail.xml的仓组合，用于汇总所有芯片仓。

  > 注意：
  >
  > 每个开发板的chipsets/chipsetN/chipsetN-detail.xml里主要包括device/soc，device/board以及vendor相关仓。多个开发板可能共用device/soc仓或vendor仓，此时需要注意防止chipsets/all.xml里重复包含共用的仓。
  >
  > 例如：
  >
  > chipsets/bearpi_hm_nano.xml和chipsets/bearpi_hm_micro.xml中直接包含了chipsets/hispark/hispark.xml，并额外增加了vendor_bearpi和device_board_bearpi；而这两个仓在chipsets/bearpi_hm_micro/bearpi_hm_micro.xml中已经包含，因此chipsets/all.xml里不需要额外包含bearpi_hm_nano开发板的仓。

按照上述分类，可支持各种代码下载方式：

> 下表中的URL默认为https://gitee.com/openharmony/manifest.git或git@gitee.com:openharmony/manifest.git，详细内容可参考[获取源码](https://gitee.com/openharmony/docs/blob/master/zh-cn/device-dev/get-code/sourcecode-acquire.md)页。

| **统分类** | **代码下载方式** | **下载命令**                                                 | **说明**                                            |
| ---------- | ---------------- | ------------------------------------------------------------ | --------------------------------------------------- |
| 所有系统   | 全量代码         | repo init -u *URL* -b master                                 | 默认下载OpenHarmony的全量代码（兼容已有下载命令）。 |
| 轻量系统   | 全量代码         | repo init -u *URL* -b master **-g ohos:mini**                | 下载轻量系统全量代码。                              |
| 轻量系统   | 指定芯片代码     | repo init -u *URL* -b master **-m chipsets/chipsetN.xml -g ohos:mini** | 下载轻量系统指定芯片的代码。                        |
| 小型系统   | 全量代码         | repo init -u *URL* -b master **-g ohos:small**               | 下载小型系统全量代码。                              |
| 小型系统   | 指定芯片代码     | repo init -u *URL* -b master **-m chipsets/chipsetN.xml -g ohos:small** | 下载小型系统指定芯片的代码。                        |
| 标准系统   | 全量代码         | repo init -u *URL* -b master **-g ohos:standard**            | 下载标准系统的全量代码。                            |
| 标准系统   | 指定芯片代码     | repo init -u *URL* -b master **-m chipsets/chipsetN.xml -g ohos:standard** | 下载标准系统指定芯片的代码。                        |
| 标准系统   | 系统组件代码     | repo init -u *URL* -b master **-g ohos:system**              | 下载标准系统系统组件的代码。                        |
| 标准系统   | 芯片组件代码     | repo init -u *URL* -b master **-g ohos:chipset**             | 下载芯片组件的代码（所有芯片平台）。                |
| 标准系统   | 指定芯片组件代码 | repo init -u *URL* -b master **-m chipsets/chipsetN.xml -g ohos:chipset** | 下载指定芯片chipsetN的芯片组件代码。                |




## 3. matrix_product.csv文件说明
[matrix_product.csv](https://gitee.com/openharmony/manifest/blob/master/matrix_product.csv)用于管理OpenHarmony代码仓对应的编译形态，以及每个设备形态下面对应的测试用例。


## 修改/新增代码仓
matrix_product.csv每一行对应一个代码仓，新增的代码仓需要新加一行。
A,B两列分别填入对应的仓名和组件名，C列往后是编译形态和对应测试形态(Test_{编译形态})，编译形态填写Y/N，测试形态填写测试用例。

## 门禁规则
1、PR精准构建需要代码仓责任田维护仓与各编译形态的关系，该文件采用CSV格式保存，存放在manifest代码仓。
新增仓、修改仓涉及manifest及Matrix.csv的，责任田自行修改，不更新manifest以及Matrix.csv的子系统默认不进行构建。

2、TDD测试用例由责任田提供 ，并维护Matrix.csv中的对应编译形态的测试套列表，责任田没有提供编译形态测试用例的，门禁不做测试验证。

3、组件依赖关系由责任田维护，不提供依赖关系的只测试当前组件，提供依赖关系会解析三层组件的总测试用例集用于门禁测试。

## 验证方法
评论start build是否可以正常触发门禁，如提示默认不跑门禁，则需填写[matrix_product.csv](https://gitee.com/openharmony/manifest/blob/master/matrix_product.csv)。
