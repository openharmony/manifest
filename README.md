# 仓库职责

* 统一管理用来组织个SIG组代码的manifest文件

* 为开发者提供统一下载各SIG组的代码的入口

# 下载方式

* 目前manifest仓的管理思路是一个开发板对应一个.xml文件，所以开发者想要下载指定代码的时候，repo 命令中需要添加 -m  来指定.xml文件

方式一（推荐）：通过repo + ssh 下载（需注册公钥，请参考[码云帮助中心](https://gitee.com/help/articles/4191)）。

```shell
repo init -u ssh://git@gitee.com/openharmony-sig/manifest.git -b master --no-repo-verify -m xxxx.xml
repo sync -c
repo forall -c 'git lfs pull'
```

方式二：通过repo + https 下载。

```shell
repo init -u https://gitee.com/openharmony-sig/manifest.git -b master --no-repo-verify -m xxxx.xml
repo sync -c
repo forall -c 'git lfs pull'
```

# 执行prebuilts

在源码根目录下执行脚本，安装编译器及二进制工具。

```
bash build/prebuilts_download.sh
```

下载的prebuilts二进制默认存放在与代码文件夹同目录下的OpenHarmony_2.0_canary_prebuilts中。

