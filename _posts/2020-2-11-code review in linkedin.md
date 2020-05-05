https://thenewstack.io/linkedin-code-review/

LinkedIn recently passed the milestone of having conducted one million code reviews. The head of the social networking service’s tooling shared a few learned lessons along the way.



Reading and reviewing code is something every engineer does on a daily basis. A formal code review process, however, is a bit different — it requires every code change to be officially reviewed by another team member before the code goes to production. At LinkedIn, code reviews have been a mandatory part of our development process since 2011. Our goal in requiring code reviews was to scale our rapidly-growing engineering team as smoothly as possible. Good code reviews with meaningful, useful comments can really help with leveling up an entire engineering organization. At LinkedIn, these reviews have become an essential part of quality assurance and knowledge sharing. Embracing code reviews has changed our whole engineering culture for the better in several key ways.


One of the biggest benefits of implementing company-wide code reviews has been increased standardization in our development workflow. Every team at LinkedIn uses the same tools and process for doing code reviews, which means that anyone can help with reviewing or contributing code for another team’s project. This eliminates problems like “I could fix the bug in their code, but how would I build that code and submit the fix?” This, in turn, helps increase collaboration across different teams in the engineering organization.



By making code reviews a mandatory process, we’ve also helped foster a healthy feedback culture at the company: engineers are open to giving and receiving feedback in all areas of work, not just in coding because it’s become a routine component of the job. Rather than viewing code reviews as critical or negative, our engineers use both giving and receiving code reviews as opportunities to grow professionally. In fact, high-quality code reviews are an important part of LinkedIn’s promotion process because they provide objective evidence of engineering skill.


Over the years, we’ve honed several best practices and tips for how to give truly great reviews. Below are some guidelines, in the form of questions, that we suggest asking to help make sure the reviewer and reviewee are getting the most value possible out of a code review.



#### Do I Understand the “Why”?
To facilitate the best review possible and help your team scale, every code change submission should include a design overview that briefly explains the motivation behind the change. It is really hard to offer a high-quality code review when the rationale needs to be inferred from the code change itself. It is fair to ask and expect the submitter to explain their motivation before attempting the code review. This also encourages the submitter to have an explanation in their commit message, increasing the quality of code documentation.


#### Am I Giving Positive Feedback?
In an organization full of smart people, clean code and neat test coverage can be taken for granted. As a result, code review feedback tends to focus only on problems and issues found in the code. This is very unfortunate because most people need positive feedback to feel engaged and motivated — and engineers are no exception. When a reviewer sees good stuff in the code, they should call it out and give positive feedback. This helps improve team dynamics, and often such positive feedback is contagious. As with all code review comments (more on this below), any positive feedback should be specific, explaining why that particular code is well-written.


#### Is My Code Review Comment Explained Well?
Whether the feedback is positive or negative, any code review comment should be self-explanatory. What might seem obvious to the reviewer can be unclear to an engineer who receives poorly-explained code review comments. When in doubt, it is better to over-explain than to provide terse feedback that yields more questions and the need for more back-and-forth communication. Explanations can be as simple as “reduces duplication,” “improves coverage,” or “makes code easier to test.” In addition to making reviewers’ comments clearer, these types of explanations also help reinforce the design principles that the team aspires to meet.

#### Do I Appreciate the Submitter’s Effort?
Hard work always needs to be appreciated, regardless of the outcome — this fosters strong, highly motivated teams. Some code changes are not of the highest quality and will need to be reworked. In those situations, it’s important to still acknowledge the effort that the author put into the changes, even if their code needs reworking. The best way to show appreciation is to put effort into your code review by giving high-quality feedback with decent explanations, acknowledging the good ideas (there are always good things in every code submission!), and using “thank you.”



#### Would This Review Comment Be Useful to Me?
Asking this question is an easy and powerful way to validate if the code review comment is necessary. At the end of the day, engineers should view code reviews as helpful development tools, and not sources of unimportant busywork. If you don’t think a particular review comment would be useful to you, then remove it. A classic example of unhelpful code review comments are ones related to code formatting. Code style and formatting should be validated by automated tools, not engineers.


#### Is the “Testing Done” Section Thorough Enough?
At LinkedIn, every code change submission has a mandatory “testing done” section that needs to be filled out. In the open source world, using GitHub as an example, engineers can submit “testing done” information in the pull request description. What should be in “testing done” depends on the gravity of the changes and the current level of test coverage. If the change contains new or altered conditional complexity, it is fair to expect it to be covered in unit tests. Some changes may require running manual testing if the integration test coverage is inadequate. In those cases, “testing done” should include information about the test scenarios and the outputs. When changes alter the output of the program, it is very useful to include the new output in the “testing done” section.


#### Am I Too Pedantic in My Review?
Some code reviews have so many comments that important issues — ones that really need to be fixed — are lost among less important suggestions. Reviews that are too heavy on details for a given team can slow down the review cycle and cause friction for both the reviewers and reviewee. Having clear review expectations, example reviews, and a positive, inviting review culture are great ways to avoid lengthy, exhausting review cycles.


In summary, having a formal code review process helps improve code quality, team learning, and knowledge sharing. The quality of work increases when every engineer on the team realizes two important things: someone else will read my code, so it better be good, and I’ll have to address any review comments I receive, so I should try to make my code good the first time around to save myself effort later on. When code reviews become an everyday habit, the team practices giving and receiving feedback on a daily basis. This is key for growth and improvement.


At LinkedIn, we’ve learned a lot from the past one million code reviews, and we aspire to learn even more from the next million. The more effort that’s put into code reviews, the better a team gets at giving great code reviews, the higher the quality of code that is checked in, and the higher the quality of the product that’s built. High-quality code reviews are contagious!


The author thanks James Miller, Oscar Bonilla, Joshua Olson, Andrew Macleod, Scott Meyer, and Deep Majumder for insightful feedback on this article.

---
### https://thenewstack.io/linkedin-code-review/

在LinkedIn，代码评审最近达成了100万行的里程碑。这家社交网络工具的领头羊，分享了一路走来获得的收获。

代码阅读和检视是工程师每天都要做的事。正式的代码评审流程，有一点点不同——每一次代码更新，在部署到正式环境之前，都需要经过其他团队成员正式的评审。自2011年以来，代码评审在LinkedIn已经变成了开发流程中必须的一部分。它的目标是尽可能平滑的衡量快速成长的工程队伍。

有意义的代码评审反馈，对提升整个工程组织的水平很有帮助。在LinkedIn，评审已经成了质量保证和知识分享中的核心部分。拥抱代码评审让整个工程文化在某些关键方面变得更好。

在公司层面实施代码评审，一个最大好处是提升了开发流程的标准。每个团队用的是同一套工具和流程，意味着每个人都能帮助别的项目团队评审或贡献代码。能消除诸如“我可以修复那段代码中的bug，但我怎么构建并提交修复呢？”之类的问题。 并且有助于增进组织中不同团队之间的协作。

把代码评审作为一个必需的流程，有助于促成健康的反馈文化：工程人员愿意提出或接收工作中各方面的反馈，不仅仅是代码，因为这已经变成了日常工作的一部分。

工程师并没有把代码评审看成是批评或负面的，而是把它作为一个提升专业的机会。事实是，高质量的代码评审是linkedIn晋升流程的一个重要部分，因为它提供了衡量工程技术的客观证据。

多年来，对于如何真正做好评审，我们打磨了一些最佳实践和技巧。下面给出一些指导方针，以问题的形式来展现。在代码评审中问一问自己，可以帮助双方获得最大的价值。

### 我理解“为什么要这么做”吗？
为了促进最大可能的评审，并帮助团队衡量，每次代码变更的提交都需要包含整体设计，用来简单解释变更背后的动机。如果需要从代码本身去推断实现原理，就很难做到高质量的代码评审。 

在评审之前，期望提交人解释下代码变更的动机，这样的要求也是很合理的。同时也是鼓励提交者在提交时写好注释， 提高代码文档的质量。

### 我提供了积极的反馈吗？
在一个都是聪明人的组织里，干净的代码和整洁的测试覆盖率被认为是理所当然的。因此，代码评审仅会关注代码中的问题。

非常不幸的是，大部分人需要获取积极的反馈，才能让自己有参与感或者被激励——工程人员也不例外。 
当评审者看到代码中好的方面时，应该说出来，给予积极的反馈。这有利于改善团队动力，经常性的正面反馈会有感染力。

正如代码评审的评语（后面会详细介绍）一样，任何积极的反馈内容需要非常明确，指出为什么那段代码写的特别好。

### 我的评审反馈解释清楚了吗？
无论反馈是积极的还是负面的，任何评语应该是自解释的。不怎么完善的评语评审者自己能看懂，但对于代码提交者来说，却不会那么清楚。当有疑问时，解释充分点会比较好，太简洁反而会掩盖更多问题，并需要来回多次的沟通。 

当然，解释也可以很简单，比如使用“减少重复”，“完善覆盖率”，或者“让代码更容易测试”这样的重构术语。不但让评论更清晰，也会帮助团队进一步巩固设计原则，

### 我感激提交者的努力吗？
无论结果怎样，努力工作总是值得赞赏——这会增强团队的积极性。有些代码变更的质量不是很高，需要返工。即使这样，仍然要承认提交者的努力，这点很重要。

展示感激的最好办法是在代码评审中给出高质量的反馈，得体的解释，承认做的好的部分（任何提交的代码中总有好的一面），以及说声“谢谢你”。

### 代码评审的评语对我有用吗？
在验证评审评语是否必要时，这样问自己一下，简单又有效。

最终，工程师会把代码评审看成利于开发的工具，而不是无关紧要的额外工作。如果你认为某条评语没用，就删掉。无用评语很典型的例子就是关于代码格式的。代码风格和格式应该由自动化工具来验证，不是工程人员。

### “测试完成” 区足够充分吗？
在LinkedIn，每次代码变更提交都必须填写“测试完成”区。在开源领域，比如GitHub，工程师能够在拉取请求（pull request）的描述中填写“测试完成”信息。 

测试完成区里填写的信息取决于变更的比重以及当前的测试覆盖水平。假如变更包含新的或者更改的条件复杂度，应当有相对应的单元测试。如果集成测试覆盖率并不充分，则需要运行手工测试。

以上的情况下，”测试完成”区应该包括测试场景以及输出结果。当变更改变了程序的输出时，把新的输出结果放在“测试完成”区是非常有用的。

### 在评审中我是否太学究派了？
有些代码评审评语太多，使得真正重要的问题（确实需要解决的）被淹没在不重要的建议中。评审太关注于细节，会拖慢团队的评审循环，让评审双方人员产生摩擦。为避免冗长，令人疲惫的评审循环，需要有清晰的评审期望，评审案例，以及积极的，吸引人的评审文化。

总之，正式的代码评审流程帮助团队改善代码质量，团队学习和知识分享。当团队的每个成员意识到以下2个要点时，工作质量将得到提升：

别人会读我的代码，因此最好写的好点。还有，我需要处理收到的评审反馈，为节省以后修改的时间，我应该第一时间就把代码写好。

当代码评审变成了每个人的习惯， 团队每天都在实践反馈的输出输入。这是成长和改善的关键。

在LinkedIn，我们从过去100万行的代码评审中学到了很多，我们也期待从下一个100万中学到更多。

代码评审中投入的精力越多，团队在评审中就会给出更好的反馈，提交的代码质量也更高，从而构建的产品质量也更好。高质量的代码评审具有传染性。

The author thanks James Miller, Oscar Bonilla, Joshua Olson, Andrew Macleod, Scott Meyer, and Deep Majumder for insightful feedback on this article.
