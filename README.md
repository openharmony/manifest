# OpenHarmony Community
欢迎来到OpenHarmony社区！


## [matrix_product.csv文件](https://gitee.com/openharmony/manifest/blob/master/matrix_product.csv)说明
matrix_product.csv用于管理OpenHarmony代码仓对应的编译形态，以及每个设备形态下面对应的测试用例。


## 修改/新增代码仓
matrix_product.csv每一行对应一个代码仓，新增的代码仓需要新加一行。
A,B两列分别填入对应的仓名和组件名，C列往后是编译形态和对应测试形态(Test_{编译形态})，编译形态填写Y/N，测试形态填写测试用例。

## 门禁规则
1、PR精准构建需要代码仓责任田维护仓与各编译形态的关系，该文件采用CSV格式保存，存放在manifest代码仓。
新增仓、修改仓涉及manifest及Matrix.csv的，责任田自行修改，不更新manifest以及Matrix.csv的子系统默认不进行构建。

2、TDD测试用例由责任田提供 ，并维护Matrix.csv中的对应编译形态的测试套列表，责任田没有提供编译形态测试用例的，门禁不做测试验证。

3、组件依赖关系由责任田维护，不提供依赖关系的只测试当前组件，提供依赖关系会解析三层组件的总测试用例集用于门禁测试。

## 验证方法
评论start build是否可以正常触发门禁，如提示默认不跑门禁，则需填写matrix_product.csv。
