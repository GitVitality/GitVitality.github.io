```flow
st=>start: Start
e=>end: End
notId=>condition: 没有任务id?
isCopy=>condition: 复制任务？
newTask=>operation: 新建任务
copyTask=>operation: 复制任务
editTask=>operation: 编辑任务
taskDetail=>operation: 任务详情

campaignType=>inputoutput: 计划类型
productType=>inputoutput: 推广目标类型

locationLevelId=>inputoutput: 广告版位

keepSession=>inputoutput: 重新加载

taskCrowdType=>condition: 人群定向
materialMainSub=>condition: 创意
taskTime=>condition: 时间

st->notId
notId(no)->isCopy
notId(yes)->newTask
newTask->keepSession->campaignType->productType->locationLevelId->e
copyTask->locationLevelId->e
editTask->taskCrowdType(yes)->locationLevelId
editTask->taskCrowdType(no)->materialMainSub
materialMainSub(yes)->locationLevelId
materialMainSub(no)->taskTime
taskTime(yes)->locationLevelId
locationLevelId->e
isCopy(yes)->copyTask
isCopy(no)->editTask
```