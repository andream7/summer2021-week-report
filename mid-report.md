### 项目名称

工作流级别任务空跑（后端）

### 方案描述

#### 1. 修改数据表

在以下数据表中增加空跑字段。

1. t_ds_command
1. t_ds_error_command
1. t_ds_process_instance
1. t_ds_task_instance

#### 2. api模块的接口修改

1. ExecutorController：startProcessInstance接口增加空跑DryRun参数
   1. ExecutorService：execProcessInstance方法增加空跑参数
   1. ExecutorServiceImpl：execProcessInstance：增加形参并修改逻辑
   1. ExecutorServiceImpl：createCommand：增加形参并修改逻辑
2. TaskInstanceController：在list-paging的返回值中增加空跑参数，所以只需在Mapper.xml中增加查询的字段值
3. ProcessInstanceController：在list-paging的返回值中增加空跑参数，只需要在对于xml文件的queryProcessInstanceListPaging增加查询参数
4. 测试类：修改ExecutorControllerTest及ExecutorService2Test中的测试方法，通过测试

#### 3. dao模块修改

1. entity：在以下实例类中增加dryRun字段，相应的Getter和Setter，修改构造方法、toString等
   1. Command
   1. ErrorComand
   1. ProcessInstance
   1. TaskInstance
2. mapper：相关查询中增加空跑字段
   1. CommandMapper.xml
   1. ProcessInstanceMapper.xml
   1. TaskInstanceMapper.xml

#### 4. server模块修改

1. master包：
   1. MasterExecThread：createTaskInstance：把空跑字段由processInstance传递到taskInstance
2. worker包：
   1. TaskExecuteThread：**跳过空跑的实际逻辑位置**，根据空跑标记跳过实际执行部分，直接把状态修改为成功

#### 5. service模块修改

1. process包：
   1. ProcessService：调用Command构造方法的地方增加空跑字段
   1. 测试类ProcessServiceTest：增加空跑字段使测试通过

#### 6. 容错处理

![image.png](https://cdn.nlark.com/yuque/0/2021/png/1498431/1628928407912-f8b6f5ec-395b-4863-8d39-11f2a7dda25c.png#clientId=u985adb37-367c-4&from=paste&height=54&id=u6f43534f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=64&originWidth=916&originalType=binary&ratio=1&size=34191&status=done&style=none&taskId=u69fdfbed-255d-488a-8a1f-56d984367d6&width=772)
实际上就是在上述两个.xml文件中，在查询时增加空跑字段，就能分别保证master和worker容错。（已经在server模块中进行了修改）

### 时间规划

| 时间（周）        | 要完成的工作                                                 |
| ----------------- | ------------------------------------------------------------ |
| 1                 | 搭建本地开发环境，建立本地数据库，与导师交流沟通项目细节     |
| 2                 | 完成增加相关api的空跑字段并进行功能测试                      |
| 3                 | 完成流程定义到流程实例，流程实例到任务实例空跑标记的传递并进行测试 |
| 4                 | 完成流程、任务实例增加空跑字段后持久化到数据库、任务执行时跳过实际执行 |
| 5                 | 完成master和worker的容错设计并测试                           |
| 6（中期审核8.15） | 完成前期安排中未完成的任务                                   |
| 7                 | 与前端交接，进行接口和功能测试，修复bug                      |
| 8                 | 与前端交接，进行接口和功能测试，修复bug                      |
| 9                 | 编写整体流程的测试，修复bug                                  |
| 10                | 整理源码并提交pull Request                                   |
| 11-12             | 进一步参与社区其他issue问题的修复                            |

## 项目进度

### 已完成工作

目前后端部分的代码已经全部编写完成，可以成功运行一个空跑工作流。

![image.png](https://cdn.nlark.com/yuque/0/2021/png/1498431/1628929155998-714d5eff-0b40-482d-80de-3353aeae4511.png#clientId=u985adb37-367c-4&from=paste&height=227&id=u18a1dd4f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=454&originWidth=1589&originalType=binary&ratio=1&size=84449&status=done&style=none&taskId=uc1112f33-0b6a-4d92-aba0-5340af5837b&width=794.5)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1498431/1628929175965-d37fcb90-1e53-402a-b66b-c03ae5af5150.png#clientId=u985adb37-367c-4&from=paste&height=203&id=u32a3c9e2&margin=%5Bobject%20Object%5D&name=image.png&originHeight=405&originWidth=1606&originalType=binary&ratio=1&size=68504&status=done&style=none&taskId=u41e64ebe-a088-4b00-a9b8-9c0e533dbe4&width=803)