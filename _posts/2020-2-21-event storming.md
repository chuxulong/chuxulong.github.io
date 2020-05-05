https://developer.ibm.com/technologies/java/tutorials/reactive-in-practice-1/#

Reactive in practice, Unit 1: Event storming the stock trader domain

DDD事件风暴（上）——原理篇

### 前言
范钢老师在devops社区的系列分享第一篇-《快速交付》之设计篇，以电商中的购物环节为例，利用领域驱动设计方法来建模，介绍了事件风暴，聚合，聚合根，命令等一系列概念。作为DDD的入门者，这些概念还是比较模糊的。在IBM Developer找了一篇文章，详细介绍了DDD的基本概念和实践，翻译出来与大家分享。

以我个人的经验，很少能看完公众号上长篇的文章，基本翻几页就放弃了，或者先收藏着，然后从来也不会去打开收藏。所以还是分两次发布，上篇以股票领域为背景，介绍DDD的基本概念，下篇介绍如何在股票交易领域实践DDD。

等不及的朋友可以直接跳转到原文链接。

### 为什么采用事件风暴和领域驱动设计？
事件风暴和领域驱动设计的目标是创建一种技术无关的语言，达成对业务需求和流程的详细了解。让业务领域专家——这些最熟悉股票交易领域和业务角色的人——能够和团队的其他人分享领域知识。建模工作坊需要参与的干系人，可能包括技术专家、项目管理人员、用户体验专家、质量保证分析师，以及任何在项目运作中涉及的其他人；当然，最重要的人是业务领域专家。

同步系统依赖于请求/响应式语义；比如，我们调用一个方法或者Restful端点，并期待得到响应。 调用者收到响应前会一直等待。 如果响应延迟或失败了，会由于超时而导致资源无法获取。最起码，来自第三方系统任何的延迟都会导致服务明显的时延。

与之相反，我们将演示一个异步的，没有阻塞的系统开发。 因此，我们需要一种能够适应这种设计和开发风格的建模方法。

事件风暴和DDD允许我们以异步的方式建模，这很适合响应式，云原生系统分析和设计。

### 事件风暴介绍
事件风暴关注于召集项目所有的干系人，让大家对业务领域和身边的问题达成一致的理解，并且与技术无关。这使得解决方案建立在合适的业务上下文基础上，确保领域专家和技术专家在构建系统前达成一致的理解。事件风暴是一个高度协作的流程，经常能发现相当多的知识点。否则，它们只会留在个人和团队心中。

在事件风暴中，我们用橘黄色便签代表事件。起步很容易：干系人想一想，在桔色便签上写下感兴趣的业务事件，并把便签粘在某个固体表面上（比如墙上的空白纸，事后可以方便的卷起来）。在开始的10-10分钟时间里，或许有一些有趣的事件被识别出来。一旦我们有足够的事件，就需要创建流：事件流就是根据时间顺序从左到右排列的事件集合。


Why event storming and domain-driven design?
The goal of event storming and domain-driven design (DDD) is to establish a technology-independent language and detailed understanding of the business needs and processes. This will allow the business domain experts — those most familiar with the stock trading domain and the role our business has in it — to communicate their domain knowledge with the rest of the team. Stakeholders involved in a modeling workshop may include technology experts, project management, user experience specialists, quality assurance analysts, and anyone else involved in the execution of this project; however, the most important people to include are the business domain experts.

Synchronous systems rely on request/response semantics; as in, we invoke a method or RESTful endpoint and expect a response. The caller blocks until receiving a response. If the response is delayed or fails, this can lead to resource starvation due to accumulating timeouts. At the very minimum, any delay when integrating with third-party systems can cause noticeable latency in our services.

In contrast, we will be demonstrating an asynchronous, non-blocking style of systems development throughout this series. For this reason, we need an approach to modeling that is complementary to this style of design and development.

Event storming and DDD allow us to model our system in an asynchronous way, which is suitable for reactive, cloud-native systems analysis and design.

Event storming introduction
Event storming involves gathering all stakeholders of a project to align on a technology-neutral understanding of the business domain and the problem at hand. This grounds our solution in the appropriate business context, helping to ensure that the business domain experts and technology experts arrive at a common understanding before constructing a system. It is a highly collaborative process, and can often surface quite a bit of knowledge that would otherwise be siloed away within individuals and teams.

In event storming, we represent events with orange sticky notes. Getting started is easy: stakeholders simply begin to think of and write down interesting business events on orange stickies and affix them to a modeling surface (typically paper on a wall that can easily be rolled up when finished). Within 10-15 minutes of beginning an event storming session, we will likely already have a bunch of interesting events identified! Once we have enough events, we need to create flows: a flow of events is simply events in order from left to right, the order representing time.

An event storming workshop is a very dynamic experience. We may begin with a very simple flow of events. But shortly after a few events emerge on our workspace, a more detailed discussion is bound to emerge. Unlike more ‘formal’ modeling techniques, such as UML, communication and discussion are the most critical outcomes that emerge from this exercise. That’s not to say that we can’t keep the output of the workshop in the form of diagrams or other artifacts, but typically, we don’t want to turn valuable conversations into high-fidelity representations of those discussions right away. Event storming is all about the free flowing of communications.

Domain-driven design introduction
Notice that we are starting to not only identify events, but also to surface a set of terms with specific business context. In the stock trader domain, we will use terms such as ‘stock,’ ‘quote,’ and ‘order.’ These will form the ubiquitous language of our business. We need to keep this consistent for the benefit of communication.

Domain-driven design gives us the blueprints to transform the output of an event storming session into models that are formal enough to use for architecting and building a real-world system.

We’ll now assume that readers have completed the prerequisites before continuing.

Getting started: The basics
We’ll work through the modeling process to first establish a technology agnostic understanding of the stock trading business domain, using our business experts as the source of that knowledge. We’ll then translate those domain concepts directly into domain logic in our code, using the appropriate domain language. This will enable our technology experts and our business experts to stay closely aligned throughout the entire design and implementation of Reactive Stock Trader.

Through this early design process, we will use event storming to:
Model the event and command flow of our stock trading system
Assign commands and events into various subdomains that can be implemented by different teams
Establish the interfaces between subdomains

Events
We start the modeling process by identifying the important events in the system we’re modeling. An event is a factual statement of occurrence (for example, Stock Purchased). In other words, a set of events is a historical record of things that have happened in our system (or business). We will always express events in the past tense to reflect this.


In theory, we can model an entire business, no matter how complex, as a flow of events. In practice, however, it would be virtually impossible to gather tens of thousands of stakeholders from a large company in the same room in order to make any meaningful progress. For this reason, we must limit an event storming session to a targeted area of the business — not a targeted area of the technology — and further break down the process flows into domains and subdomains.

Typically, we kick off an event storming session by having all participants begin affixing events onto our workspace — any event that they find interesting enough to discuss. In our stock trading domain, a few interesting events are:

A client opened a new portfolio
A client purchased some shares of a stock for their portfolio
A client sold some shares of a stock from their portfolio
A client closed their portfolio


As we begin to think of new events, we may notice that our event storming session is resulting in a pile of unordered events stuck to our workspace. As a more complete picture of the flow emerges, we will need to organize these events into a linear sequence. This can help to surface gaps in the business process.

For example, we might create a sequence ‘client opened a portfolio,’ ‘client purchased shares of stock.’ This might prompt us to ask, “What did they purchase the shares with?” Which we might respond to by adding a new domain event between those two events labeled ‘client adds cash to portfolio.’


We always start with events. Events are the natural foundation of a system, as they are the closest representation of the business processes involved, and entire discussions with subject matter experts can be formed around them. Other building blocks, such as commands, help us to elaborate on the processes and form a mental model of the asynchronous nature of the system.

Remember that event storming is an iterative process, so it may take many iterations between events and commands before we have modeled a process flow to some degree of accuracy, based on the business requirements.

Commands
Commands are often the trigger for a sequence of one or more events, added as blue sticky notes to signal intent. While events are irrefutable statements about the past, commands express our intent for something to happen in the future.

However, commands can be refuted. For example, a command to ‘sell stock’ might be rejected if the client’s portfolio doesn’t hold the number of shares they wish to sell.

Commands are always present tense intentions, whereas events are always past tense facts.

Aggregates
We will naturally begin to identify some aggregates, such as a Portfolio.

A Portfolio may contain some cash and also contain shares of various stocks. A Portfolio is an entity — that is, it has an intrinsic identity — so that even if your portfolio and my portfolio have the exact same amount of money and the exact same shares, they are still distinct from each other. For example, if I add some shares to my portfolio, it will still be the ‘same’ portfolio, even though the contents are different.

Entities, aggregates, and aggregate roots
Before we continue, let’s cover some additional theory about the terms we’ve covered, namely entities, aggregates, and aggregate roots.

It’s often convenient to blur the distinction between an entity, an aggregate, and the aggregate’s root. This is similar to the way we might blur the distinction between an object graph and the root of the object graph. For example, a linked list can be thought of as a collection of linked nodes, or as a reference to the first node in the list. If we held a reference to the first node in that list, we wouldn’t hesitate to say that we held a reference to “the list.” If that list represented a queue of work to do, and we updated our work queue reference by following the reference between nodes, we would be thinking of the work queue as an entity, which changes state as we perform work on it. In this example, if we talk about the ‘work queue,’ do we mean the reference to the head of the list (entity), the head of the list (aggregate root), or an ordered collection of items in that list (aggregate)?

In practice, we often omit the distinction. At the level of detail we want to achieve during event storming, we’re typically concerned about state purely from a business context. If we keep that in mind, and ensure that our conversations remain accessible and engaging to non-technical stakeholders, we’ll likely be capturing the right level of detail. There will be time during more technical solutioning exercises to make the distinction with more rigor.

Modeling aggregates
For aggregates, we use pale yellow sticky notes. Aggregates represent the state within our system. For example, a Portfolio is a natural aggregate. So is a Bank Account.


Aggregates are usually represented using persistent entities in Lagom. We will cover terminology in much more depth throughout the series.
Aggregates are all about state, so we’ll need a way to change state. A command is directed to an aggregate and the only way that an aggregate can begin the process of changing state. This is an important distinction: commands do not force a state change. Commands kick off a series of operations that may or may not result in a state change. Aggregates can choose to change state, if the command is able to be applied, or ignore the command altogether.

As soon as an aggregate decides to change state, it emits an event that represents the change of state. For example, the ‘sell shares’ command would be applied to the entity that represents the client’s portfolio, and may lead to an event ‘shares sold’ and a change in the number of shares held by that portfolio. Applying the ‘shares sold’ event to the aggregate is the state change itself, and then that event is also emitted to subscribers.


Events may also be emitted if a state change is not possible. For example, an event called ‘sell order rejected’ may be emitted if a sale cannot be completed due to insufficient shares available.

Changes to the state of an entity happen only as a result of events. In order to recreate the state of the entity, we can start from the beginning of time and reapply all events that have ever happened in the aggregate boundary, which can be considered a transactional boundary. This is the core concept of event sourcing, which we will cover in depth throughout this series.
During event storming, the events should be descriptive enough to paint a clear picture of any state changes that may or may not happen. The descriptive naming of events is the key to capture relevant business logic behind state changes, and also capture all possible outcomes of a command.

A general pattern that we’ll see is:

A command expresses intent for an entity to change state (for example, sell shares)
An event expressing that the intended action has occurred is created (for example, shares sold)
The entity’s state is updated based on the event
The event is published for subscribers to consume

Actors and reactions
Events happen because of a reason, such as user-initiated commands or machine-initiated commands.

The most common trigger of an event are user-initiated commands. Users are called actors in event storming parlance (not to be confused with Akka actors or the actor model) and attached to a command as a small yellow sticky. The main reason to use actor notation is that it makes planning and estimation much easier in later phases of modeling; any command with an actor associated with it will automatically be of interest to UI and UX teams, who can easily visualize the scope of their work.

Reactions (also known as policies) help to model system-initiated commands in response to events happening. For instance, we may want to initiate a command when something else happens. It’s this ‘when, then‘ notation that makes reactions very handy to use in modelling workshops. By convention, reactions are represented using lavender sticky notes. We can insert them between an event or a command, which makes it easy to model policies, with multiple commands being triggered from one event, or to attach the lavender sticky directly to a command, where a policy only has a single command associated with it.


The above flow is an example of an ecommerce process for adding products to a shopping cart. We leverage both actors and policies to paint a complete picture of our business requirements. We can see that a user is able to add an item to their shopping cart. We can also see that when the shopping cart is abandoned, we wish to send a follow-up email.
上图是电商中一个加入购物车的流程。 我们运用角色和策略，画了一个完整的业务需求图。可以看到，用户可以把商品加入购物车，当取消购物车时，会发出一封跟进邮件。

Event storming summary
If you want to be an event storming legend, don’t forget your Event Storming Legend! Ours at RedElastic has been affixed to many walls and now requires much tape, but it does the trick.
Event storming gives us a complete and simple (but expressive) modeling language to use during the initial phases of modeling, when collaboration is important, flexible, and fast-paced. We have introduced the bare minimum event storming vocabulary for you to get started using this technique in your own modeling sessions.

We highly recommend affixing both a legend and a sample flow in a highly visible place during your event storming workshops. This will give your teams a reference point as they collaborate.



-----------------------------------------------------------

Now that we have some basics of event storming covered, let’s apply what we’ve learned to the stock trader domain.

Modeling the stock trader domain
Now that we have some basics of event storming covered, we’ll apply what we’ve learned to the stock trader domain. Rather than attempt to photograph or diagram the output of our event storming session in a high amount of graphical detail, we’ll instead work backwards and cover the structure of the system we’re going to build from the top down by documenting the output of event storming as pairs of commands and events. We arrived at all of these details through event storming, and then documented the details in the format you’ll see below.

Practice your event storming skills with your team by reverse-engineering the following bounded contexts! Hold a mock event storming workshop, and compare your output to our description below.
This is a typical approach to event storming. Typically, models coming out of an event storming session are not turned into high-fidelity diagrams, but rather feed into design documentation and other planning artifacts. Remember, the value of event storming is communication, so there’s no way to substitute for actual participation. Attempting to capture the output of an event storming session in high-fidelity is an anti-pattern. Rather, we use event storming as a collaborative learning tool that makes our other processes, like domain-driven design, more effective and accurate.

Bounded contexts
Our main unit of high-level structure is bounded contexts, which are categorizations of functionality that group related entities together.

In a real-world enterprise development scenario, a bounded context is often a team-level separation, with each bounded context being maintained by a team.

A good rule of thumb is that a bounded context should be comprehensible by an entire team, whereas an aggregate should be sized accordingly, so that it is easily understood by a single person. We’ll expand upon these concepts throughout this series.

For each bounded context, we will describe the commands and queries accepted, along with the events produced. Typically, bounded contexts are the last structures that are defined. However, in the case of this series, we’ll discuss them first to ground readers in context as they learn about the business domain.

Note: The following is a reasonable level of detail to capture after an event storming session to share with the team. We typically produce ‘design documents’ at this level of detail after a modeling session to share with stakeholders and communicate to a wider audience.

During event storming, we identified three bounded contexts within the stock trader domain:

Portfolio – business logic for customer portfolios
Wire transfer – adapters for third-party wire transfers
Broker – adapters to obtain stock quotes and execute trades
Portfolio
A Portfolio represents stock and cash holdings.

We will now describe the aggregate per in more detail. Recall that an aggregate represents state in our system, with commands acting on that state, and then events being emitted to communicate the details of state transitions to subscribers.

Portfolio aggregate
The Portfolio is the most obvious aggregate. Here we’ll have stock holdings and cash funds to either withdraw or to use to buy additional shares.

We have identified the following command/event associations:

Command	Event
OpenPortfolio	PortfolioOpened
ClosePortfolio	PortfolioClosed
PlaceOrder	OrderPlaced
SharesDebited
ProcessTrade	SharesCredited
FundsDebited
FundsCredited
TradeProcessed
AcknowledgeOrderFailure	OrderFailed
ReceiveFunds	FundsCredited
SendFunds	FundsDebited
AcceptRefund	RefundAccepted
For each command, a Portfolio may produce one or more of the corresponding events in response.

The PlaceOrder command is not an atomic transaction resulting in the sale or purchase of shares for the portfolio. It produces the OrderPlaced (and possibly the SharesDebited) event, which is be handled elsewhere by the broker service.

In trading, there are a number of order types. For the sake of the stock trader application, we will demonstrate market orders, limit orders, and stop orders. The key difference between the types is that market orders will execute once there is a willing buyer or seller for the order, which means that the order may execute in near real-time. However, it’s possible that the order will never fulfill, in the event that the stock is illiquid. Limit and stop orders place additional conditions around the execution of the order, which gives us a good opportunity to showcase the dynamics of a reactive system. An order may stick around for days, weeks, or years before a limit is hit.

The portfolio is not responsible to actually fulfill orders directly, but rather to emit events that represent the orders that have been placed. The brokerage bounded context will be responsible for fulfilling orders, which we will cover shortly.

Summary
This gives us the basic buildings blocks for our Portfolio bounded context. Next, we will cover wire transfers and how to move cash in and out of a portfolio.

Wire transfers
Our next bounded context represents the cashflow through our system in the form of wire transfers. This will handle money in and out of portfolios. The wire transfer bounded context only contains a single aggregate, a wire transfer repository.

Wire transfer aggregate
Our Portfolio subscribes to fund transfer events from the wire transfer aggregate and publishes its own acknowledgement events in response. This allows us to model wires to and from the portfolio. In the real world, transferring funds would typically be accomplished through a third-party wire transfer service. For this reason, we don’t want to couple wire transfers directly to our portfolio, but rather, we should consider a new aggregate boundary that is essentially an adapter in front of a wire transfer service. This allows us to interact with the third-party system using pub/sub semantics.

We have identified the following command/event pairs:

Command	Event
RequestFunds	FundsRequested
FundsSecured
FundsRequestFailed
SendFunds	FundsSent
FundsDelivered
FundDeliveryFailed
RefundSender	SenderRefunded
RefundFailed
Summary
If the transfer from our Wire Transfer aggregate to a third-party wire system fails for any reason, we can simply issue a compensating transaction to add the funds back to the portfolio. In a real-world architecture, we would likely event storm this with business stakeholders in greater detail to understand how to compensate from failure. But, for now, this is an appropriate level of detail to get started with a reference architecture.

Broker
Clients hold a portfolio of stocks. They can purchase or sell stocks. This bounded context will represent the source of truth for stock holdings, and, in our case, interface with external systems to place trades themselves. We’re going to leverage a third-party brokerage service to perform quotes and trades.

In an enterprise context, this may be a system within your organization, but we’ll treat it as a third-party system (perhaps a completely separate bounded context outside of our control). Within our control (in othere words, our bounded context), we will create another adapter that gives us complete control of interactions.

Order aggregate
This represents an order for a particular quantity of a particular stock symbol. We will attempt to fill this order and debit the portfolio balance. It is possible that purchase cost of the order may exceed the balance available in the account, in which case, the account is overdrawn. At some point in the future, we may want to limit the order based on available funds and perhaps holdings value. Each purchase incurs a commission payment; the commission amount is dependent on the loyalty level of the portfolio.

We have identified the following command/event pairs:

Command	Event
PlaceOrder	OrderReceived
CompleteOrder	OrderFulfilled
OrderFailed
CancelOrder	OrderCanceled
The order can represent a market, limit, or stop, and be either a buy or a sell request.

Quote service
The quote service isn’t necessarily an aggregate, but it’s worth mentioning here, as we’ll need to integrate with it on the server side in order to compute portfolio balance.

The reason that a quote is not an aggregate is because it isn’t stateful. A quote is effectively a wrapper around a third-party web service.

Trade service
The trade service simulates the custodian service that we would integrate with, if this were a real-world stock trading system.

Reactive Stock Trader won’t hold stocks directly, but rather act as an interface to a custodian system. A custodian system may be an internal ‘book of record’ system in a large bank, or a third-party custodian. The trade service will simulate the actual fulfilment of orders or the cancellation of orders due to user input or failure.

Summary & next steps
We hope that this unit has demonstrated a new way of thinking about systems architecture. Why might we benefit from learning this new approach to building systems, and why choose a reactive approach?

Suppose we have an existing stock trading system. We’ve likely learned many things about the business of stock trading and what it takes to run a stock trading platform. Now assume our platform has achieved so much success that we’re starting to run into two specific technical challenges: elasticity and resilience.

As we described at the beginning of this unit, the aim of reactive systems development is to create completely responsive systems. In order to do this, they need to be elastic and resilient. We achieve elasticity and resilience through asynchronous messaging in the form of commands and events. As we work through this series, the power of these simple principles will become more clear.

In this unit, we have covered the building blocks of designing such as a system, and as we move through the rest of this series, we will showcase how to build and deploy robust reactive systems that are responsive at runtime and responsive to the needs of your business.