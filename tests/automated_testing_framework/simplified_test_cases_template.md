# PowerAutomation 简化测试用例范例

基于Excel文件的测试用例简化设计，分为操作型和API型两大类。

## 操作型测试用例范例

### 测试用例 1: 蓝牙页面半关切换功能测试

**测试类型**: 操作型测试  
**业务模块**: BSP_Bluetooth  
**测试ID**: BT_OP_001  

**测试描述**:  
验证蓝牙设置页面中半关状态与全关/全开状态之间的切换功能，确保用户界面操作正确响应，状态转换符合预期行为。

**测试目的**:  
- 验证蓝牙状态切换的用户界面交互正确性
- 确保蓝牙半关、全关、全开三种状态转换的稳定性
- 测试重复操作的一致性和可靠性

**环境前置条件**:
```yaml
硬件环境:
  - 设备类型: Android手机
  - Android版本: >=10.0
  - 蓝牙支持: 必须
  - 内存: >=4GB

软件环境:
  - ADB版本: >=1.0.41
  - 截图工具: uiautomator2
  - 测试框架: pytest>=6.0

网络环境:
  - WiFi连接: 稳定
  - 网络延迟: <100ms

权限要求:
  - ADB调试权限: 开启
  - 截图权限: 开启
  - 系统应用访问: 允许
```

**测试前置条件**:
- 设备已开机并解锁进入主界面
- 蓝牙功能正常可用且初始状态为全开
- 控制中心可正常下拉访问
- 蓝牙设置页面可正常进入

**测试步骤与截图检查点**:

1. **步骤1**: 下拉控制中心，点击蓝牙图标切换至半关状态
   - **操作**: 从屏幕顶部下拉 → 找到蓝牙图标 → 点击切换
   - **📸 检查点1**: 截图验证控制中心蓝牙图标为半亮/半透明状态
   - **验证**: 图标应显示特殊的半关标识（如半透明或特殊颜色）

2. **步骤2**: 进入蓝牙设置页面
   - **操作**: 设置 → 蓝牙 → 进入蓝牙设置页面
   - **📸 检查点2**: 截图验证蓝牙设置页面正确显示
   - **验证**: 页面标题显示"蓝牙"，开关控件可见且为半关状态

3. **步骤3**: 点击蓝牙开关按钮切换为全关
   - **操作**: 点击蓝牙设置页面的开关按钮
   - **📸 检查点3**: 截图验证蓝牙状态切换为全关
   - **验证**: 开关显示为OFF状态，相关蓝牙选项变灰不可用

4. **步骤4**: 返回控制中心，点击蓝牙切换至全开状态
   - **操作**: 下拉控制中心 → 点击蓝牙图标
   - **📸 检查点4**: 截图验证控制中心蓝牙图标为完全点亮状态
   - **验证**: 图标完全点亮，无透明效果，颜色为正常激活色

5. **步骤5**: 再次点击切换至半关状态
   - **操作**: 在控制中心再次点击蓝牙图标
   - **📸 检查点5**: 截图验证蓝牙状态切换为半关
   - **验证**: 图标恢复半透明状态

6. **步骤6**: 进入蓝牙页面，点击"允许新连接"按钮
   - **操作**: 进入蓝牙设置 → 点击"允许新连接"选项
   - **📸 检查点6**: 截图验证按钮状态和界面响应
   - **验证**: 按钮高亮激活，蓝牙状态自动切换为全开

7. **步骤7**: 重复测试验证稳定性
   - **操作**: 重复步骤1-6共5次
   - **📸 检查点7**: 每次重复截图记录状态变化
   - **验证**: 所有重复测试结果一致，无异常或卡顿

**预期结果**:
- **检查点1**: 蓝牙图标呈现半透明或带有特殊半关标识
- **检查点2**: 蓝牙设置页面正常显示，开关为半关状态
- **检查点3**: 开关显示为关闭状态，蓝牙相关选项全部变灰
- **检查点4**: 蓝牙图标完全点亮，显示正常激活状态
- **检查点5**: 图标恢复半透明的半关状态
- **检查点6**: "允许新连接"按钮激活，蓝牙自动切换为全开
- **检查点7**: 5次重复测试结果完全一致，操作流畅无异常

**失败判断标准**:
- 任何状态切换不符合预期
- 界面显示异常或卡顿
- 重复测试结果不一致
- 截图检查点验证失败

---

## API型测试用例范例

### 测试用例 2: 网络定位NLP权限管理API测试

**测试类型**: API型测试  
**业务模块**: BSP_GNSS  
**测试ID**: GNSS_API_001  

**测试描述**:  
通过ADB命令和系统API验证网络位置服务的权限管理功能，确保权限配置正确显示和API调用返回预期结果。

**测试目的**:  
- 验证网络定位服务权限API的正确性
- 确保权限信息通过系统接口正确获取
- 测试权限管理界面与API数据的一致性

**环境前置条件**:
```yaml
硬件环境:
  - 设备类型: Android手机
  - Android版本: >=10.0
  - GPS/GNSS支持: 必须
  - 网络连接: 必须

软件环境:
  - ADB版本: >=1.0.41
  - Python版本: >=3.8
  - 测试库: requests, subprocess
  - 截图工具: adb shell screencap

网络环境:
  - 网络连接: 稳定
  - 基站信号: 良好
  - WiFi连接: 可选但推荐

权限要求:
  - ADB调试权限: 开启
  - 开发者选项: 开启
  - USB调试: 开启
```

**测试前置条件**:
- 设备通过USB连接并被ADB识别
- 网络位置服务已安装且可访问
- 设备具有基本的定位权限
- 系统设置应用可正常访问

**测试步骤与截图检查点**:

1. **步骤1**: 执行ADB命令获取权限属性
   - **操作**: `adb shell getprop | grep location`
   - **📸 检查点1**: 截图命令行输出结果
   - **API调用**: 获取系统定位相关属性配置
   - **验证**: 命令成功执行，返回定位服务配置信息

2. **步骤2**: 查询网络位置服务包信息
   - **操作**: `adb shell pm list packages | grep location`
   - **📸 检查点2**: 截图包查询结果
   - **API调用**: 通过包管理器API查询定位服务包
   - **验证**: 返回网络位置服务相关包信息

3. **步骤3**: 获取网络位置服务权限详情
   - **操作**: `adb shell dumpsys package com.android.location | grep permission`
   - **📸 检查点3**: 截图权限详情输出
   - **API调用**: 通过系统服务API获取权限配置
   - **验证**: 返回完整的权限列表和状态

4. **步骤4**: 通过UI验证权限管理界面
   - **操作**: 设置 → 应用管理 → 搜索"网络位置服务" → 权限管理
   - **📸 检查点4**: 截图权限管理界面
   - **API调用**: 系统设置界面调用权限管理API
   - **验证**: 界面正确显示权限项目

5. **步骤5**: 验证具体权限项目显示
   - **操作**: 在权限管理界面查看各项权限详情
   - **📸 检查点5**: 截图权限详情页面
   - **API调用**: 权限详情API返回具体权限说明
   - **验证**: 权限说明内容与API返回数据一致

6. **步骤6**: 测试权限状态API查询
   - **操作**: `adb shell cmd appops get com.android.location FINE_LOCATION`
   - **📸 检查点6**: 截图权限状态查询结果
   - **API调用**: AppOps API查询具体权限状态
   - **验证**: 返回正确的权限授予状态

**预期结果**:
- **检查点1**: 命令返回包含location相关的系统属性配置
- **检查点2**: 成功查询到网络位置服务包，包名正确
- **检查点3**: 权限详情包含以下项目：
  ```
  - android.permission.ACCESS_FINE_LOCATION
  - android.permission.ACCESS_COARSE_LOCATION
  - android.permission.READ_PHONE_STATE
  - android.permission.ACCESS_NETWORK_STATE
  ```
- **检查点4**: 权限管理界面显示"网络位置服务"标题和权限列表
- **检查点5**: 权限说明界面显示：
  ```
  位置（获取基站信息、WLAN信息、位置信息、用来为您提供定位服务）
  获取电话相关信息（获取基站变化信息，用于改善定位服务）
  文件和文档（访问设备上的照片、视频、音频、文件）
  ```
- **检查点6**: 权限状态查询返回"allow"或具体的权限级别

**API响应格式验证**:
```json
{
  "service_name": "网络位置服务",
  "package_name": "com.android.location",
  "permissions": [
    {
      "name": "android.permission.ACCESS_FINE_LOCATION",
      "status": "granted",
      "description": "获取精确位置信息"
    },
    {
      "name": "android.permission.READ_PHONE_STATE", 
      "status": "granted",
      "description": "获取基站变化信息"
    }
  ]
}
```

**失败判断标准**:
- ADB命令执行失败或返回错误
- 权限信息不完整或不正确
- UI界面与API数据不一致
- 权限状态查询返回异常

---

## 测试用例格式说明

### 标准化字段
- **测试类型**: 操作型测试 / API型测试
- **业务模块**: 对应Excel中的业务模块分类
- **测试ID**: 唯一标识符，格式为 [模块]_[类型]_[序号]
- **测试描述**: 简明扼要说明测试内容
- **测试目的**: 明确测试要达到的目标
- **环境前置条件**: YAML格式的环境要求
- **测试前置条件**: 测试开始前需要满足的状态
- **测试步骤与截图检查点**: 详细的操作步骤和验证点
- **预期结果**: 每个检查点的具体期望
- **失败判断标准**: 明确的失败条件

### 截图检查点规范
- 使用 📸 符号标识截图点
- 每个关键操作都有对应的截图验证
- 截图文件命名格式: `[测试ID]_checkpoint_[序号].png`
- 截图应包含时间戳和设备信息

### 环境配置标准
- 使用YAML格式定义环境要求
- 分为硬件、软件、网络、权限四个维度
- 版本要求使用语义化版本号
- 支持环境变量和配置文件

这个范例将作为后续Python脚本生成的标准模板。

