# 

# 


SparkContext
runJob
  * rdd: RDD[T]
  * func: (TaskContext, Iterator[T]) => U,
  * partitions: Seq[Int],
  * resultHandler: (Int, U) => Unit): Unit = {

调用路径大致如下
1. sc.runJob -> dagScheduler.runJob -> submitJob
2. DAGScheduler::submitJob会创建JobSummitted的event发送给内嵌类eventProcessActor
3. eventProcessActor在接收到JobSubmmitted之后调用processEvent处理函数
4. job到stage的转换，生成finalStage并提交运行，关键是调用submitStage
5. 在submitStage中会计算stage之间的依赖关系，依赖关系分为宽依赖和窄依赖两种
6. 如果计算中发现当前的stage没有任何依赖或者所有的依赖都已经准备完毕，则提交task
7. 提交task是调用函数submitMissingTasks来完成
8. task真正运行在哪个worker上面是由TaskScheduler来管理，也就是上面的submitMissingTasks会调用TaskScheduler::submitTasks
9. TaskSchedulerImpl中会根据Spark的当前运行模式来创建相应的backend,如果是在单机运行则创建LocalBackend
10. LocalBackend收到TaskSchedulerImpl传递进来的ReceiveOffers事件
11. receiveOffers->executor.launchTask->TaskRunner.run



DAGScheduler::submitStage






SparkContext::runJob
 |---DAGScheduler::runJob
      |---DAGScheduler::submitJob
           |---new JobWaiter(...) extends JobListener
           |---post JobSubmitted DAGSchedulerEvent to DAGSchedulerEventProcessLoop
                |---DAGSchedulerEventProcessLoop::onReceive match JobSubmitted
                |---DAGSchedulerEvent::handleJobSubmitted
                     |---DAGSchedulerEvent::newResultStage -> new ActiveJob() 
                     |---DAGSchedulerEvent::submitStage
                          |---DAGSchedulerEvent::submitMissingTasks
                               |---TaskScheduler::submitTasks
                                    |---LocalBackend::reviveOffers
                                         |---LocalEndpoint::reviveOffers
                                              |---Executor::launchTask
                                                   |--- new TaskRunner()
                                              
                                              
                                
                                              



