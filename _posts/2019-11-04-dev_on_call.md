---
layout: post
title: why developers should also be on call

---

##### 一切都围绕反馈环
很多开发人员都熟悉持续交付管道，它是一个相互协作的过程，反馈环存在于过程的每个阶段。 产品、开发和QA一起定义新功能，开发人员利用TDD完成功能开发。TDD本身就是一个反馈环，通过不断的测试来指导开发进程。代码在主干上编译，运行单元测试。 接着应用会部署到测试环境，进行一系列验收测试。所有这些反馈不断的为生产环境部署提供信心。测试完成后，开发人员把部署工作交接给运维，然后开始新一轮的功能开发。

看起来没毛病，很符合我们的思维习惯，但并不正确。问题在于开发人员过早从交付管道中退出，把工作扔给了墙外的团队，反馈因此中断了。

##### 把墙拆掉
要构建可靠、贴心的应用，获取应用在部署/运行中的反馈是非常重要的。从管道构建开始，我们一直都依赖反馈环，但是应用一旦部署成功，反馈环似乎被丢弃了。 代码的价值只有被终端用户使用才能体现出来，用户的反馈是所有反馈中最重要的，但是我们作为开发者，却没有收到。

可能是组织级的原因导致了隔离：传统的做法都是开发写代码，运维维护服务。 Devops提出了一种新的文化，让开发和运维走的更近。比如，开发能及时获取生产环境的反馈。 生产日志分析是开发获取反馈的一种方式。通过错误信息能洞察系统的薄弱环节，并持续完善。

洞察系统的另一种方式是建立指标采集服务。指标可以来自系统本身，也可以根据业务需求来定。 比如，信用卡和借记卡数量比这个指标，数据很容易采集，可用来解答这样的问题：“从特定的api端点上接收了多少请求，是否可以取消？”（其实我也没看懂！）。指标以百分比的方式采集，除异常点，让反馈专注于主要的用例，而不仅仅是告警。

这些自动记录的反馈几乎可以实时的获取，要完善系统的心智模式，还有很长的路要走。离完全拥有应用也还有一定差距。

##### 更努力，更好，更快，更强

开发人员对自己开发的应用有完全的控制权，并且直接负责应用的运行。提供了另一种视角来审视“什么是重要的”，“需要关注什么，哪些不需要关注”。开发过程中看起来微不足道的问题，到了运维阶段，可能会影响你的个人生活，特别是当你的电话响起时。 偶尔的内存泄漏在白天不是什么问题，只要重启服务就可。但如果出现在晚上，就完全是另一回事了。 都知道显示错误页面是一个很不好的用户体验，由于简单的重启能解决，从而忽视了从根源上解决问题。现在，到你还债的时候了，不得不优先处理这些运维问题。

开发人员轮值是指标驱动开发的演进，当应用发生错误时，开发人员通常是最清楚如何来解决的。 随着应用上云和硬件基础架构的成熟，应用错误会比基础设施问题更多。 当微服务普及，消息成了服务之间的粘合剂，就需要开发人员的系统知识来定位根源，让应用尽快的恢复正常。

驱动力有三大要素：自主、专精、目的。拥有完全的自主权，才会有目的，并驱使自己精通相关知识让系统变得更好，自主决定修复优先级。我相信，这样的自主权，让我们对构建的应用感到自豪。

-----------------------------------------

It’s all about Feedback Loops
The Continuous Delivery Pipeline is familiar to most developers.  It’s a collaborative process built upon loops of feedback at every stage.  A new feature will defined between a Product Manager, a Developer and a Quality Analyst.  A pair of Developers will work together on the implementation following a Test Driven Development approach using the tests as feedback to guide progress.  The new code is compiled with the master branch and all of the unit tests are run.  The application may be deployed to a test environment and a series of Acceptance Tests will be run.  All of this feedback gives the confidence to proceed to a Production deployment at which point Ops take over and the developers pick up a new change.

This is wrong. The Developers’ involvement stops too soon.  The change is thrown over the wall to another team and the feedback cycles stop.


Bring down the wall
The feedback from the deployment itself and from the running application are vital for building a reliable and fit for purpose application.  In all other parts of the process we rely on feedback loops, but there is a tendency to drop this once the code is deployed.  It is only in Production, when the code is in the hands of the users, that any code matters.  It is this feedback which is the most important of all and yet, we as Developers, are not receiving it.


There could be organisational reasons for this separation – traditionally Developers write code and Operations maintain running services.  We are all aware that DevOps is a cultural shift to bring these two areas closer together and part of this includes Developers being involved in the feedback from Production. Collating and aggregating log files is a way of getting some of this feedback to the Developers.  Log data gives an insight into errors that improves debugging and highlights weak areas in the application.

Adding a metrics collection service provides another insight into the running application.  Data points can be sent from the any part of the application and can be written around business requirements, for example the number of credit card payments vs debit card.  They are simple to add and can be used to answer questions such as “how many requests to we receive to a particular API endpoint, and can we deprecate it?”. Metrics values can be rolled up into percentiles and outliers smoothed away allowing feedback to focus on the majority use cases, not just alerts. 



All of this telemetry feedback can be received in near real-time and goes a long way to improving the mental models we’ve built up of the production system.  But, it is still a step removed from fully owning the application.

Harder, Better, Faster, Stronger

Owning and being directly responsible for the successful execution of something you build gives a different perspective as to what is important, what needs focus and what does not.  Issues that seem trivial in development become far more important to you personally when it’s your phone which rings.  Random memory leaks that require app reboots are easy to ignore when they occur during the day, but not so much when they happen during the night.  You’ve always known that this affects the users – that they have a terrible experience when they see the error page – but it was always difficult to track down and service so easily restored.  But now you feel the pain too and prioritise the operational fixes.

Developers On Call is an evolution of Metrics Driven Development
When it comes to application errors it is the Developers who generally have the greatest context for fixing things.  As applications move to the cloud and on-premise infrastructure matures, over time application errors become more frequent than infrastructure ones.  As microservices become more common and messaging becomes the glue, it will take the Developers’ system knowledge to isolate the cause and get things running again as quickly as possible.

It is this level of ownership which brings the purpose in the driving forces of Autonomy, Mastery and Purpose. Combined with the desire to master the knowledge needed to make the application successful and the autonomy to prioritise operational fixes.  I believe it is this ownership which enables us to have pride in the systems we build.

