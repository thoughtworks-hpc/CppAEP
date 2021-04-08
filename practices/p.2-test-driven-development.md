# 测试驱动开发

## 价值

TDD 实践能提供的价值比较多，但核心只有一个：反馈。

### 反馈

反馈是XP的核心价值之一，Kent Beck 是这样描述的：

> 没有哪个固定方向是长久有效的，不论我们在谈论软件开发细节、系统需求还是系统架构。超越经验而确定出来的方向，它的半衰期尤其短。变化是不可能避免的，变化产生了对反馈的需要。
>
> ...
>
> XP团队致力于在尽可能快的情况下产生尽可能多的反馈。尽量将反馈的周期缩短为分钟或小时，而不是星期或月。知道得越早，你就可以调整得越早。
>
> -- Kent,Beck,Cynthia,Andres. 解析极限编程拥抱变化（原书第2版） (软件工程技术丛书) (Chinese Edition) . 机械工业出版社. 

而 TDD 核心就是在代码层面给开发者提供重要反馈的方法，TDD 通过测试向开发者反馈代码的实现是否正确，以及通过测试向团队反馈需求理解是否正确：

> TDD is an awareness of the gap between decision and **feedback** during programming, and techniques to control that gap.
>
> -- Kent Beck (2002-11-08). Test-Driven Development by Example. ISBN 978-0-321-14653-3.

TDD 不是获取这项反馈的唯一手段，但其它的手段往往不够快速（比如 QA 测试，Scrum 中的 Desk check，版本 Show case等），正如 Kent Beck 对于为什么开发者需要自己写测试的回答是：”等别人写太久了“。

其它外围价值都是在 Feedback 的延伸，核心价值如果没有做好，外围价值都会大打折扣甚至不复存在。主要有五个：

### 提升信心

信心指对解决复杂需求的自信心。TDD通过测试来驱动开发者对问题的认知提升，帮助开发者面对复杂需求时建立自信心

### 简化设计

TDD 会让开发者避免过度设计引入不必要的复杂性。结合 Simple Design 实践可以得到刚刚好的代码。

### 容易维护（扩展、修bug、找错）

TDD 促使开发者设计出具有高可测试性，模块化、低耦合、高内聚的实现代码。同时健全的自动化测试让维护（重构、增加功能、修复问题）更安全及高效。

### 提升质量 

TDD提升质量的作用毋庸置疑，虽然TDD的测试并不能取代传统的测试，但是它要求开发者对实现的所有代码（100%代码覆盖率）进行验证，能明显提升代码交付的质量。

### 提升效率

很多人认为，TDD还需要写更多的代码会导致他的效率低下。

短期地看，站在整体的软件开发活动上分析你会发现，编码在整个软件开发的过程当中只是（甚至是相当少的）一部分。比如从活动来看：

编码活动前工程师其实花了可观的时间在思考复杂的功能应该如何设计，编码活动中还有相当大部分的时间是在做调试，缺乏自动化测试的编码后还会有非常可观时间投入到返工。

如果拉长软件开发周期来看，TDD 能降低团队人员变化、功能叠加、问题回归等问题带来的演进成本。 大多数项目都需要长期维护，一个绝妙的比喻是软件的整个开发周期像是一场接力赛，关注长期的效率应该更关注接力棒的交接效率而不是选手的效率。

一个有趣的现象是作为软件工程师个人，其实更关注于短期的效率，作为项目的管理和组织者更关注的其实是长期的效率，这里面存在着因为身份不同导致的政治矛盾。 

同时 TDD 促使开发者提前思考和确认需求验收条件，避免不必要的返工。

> 注意：虽然有学者的研究表明 TDD 对效率提升有所提升，参考[论文](https://web.archive.org/web/20141222180731/http://nparc.cisti-icist.nrc-cnrc.gc.ca/npsi/ctrl?action=shwart&index=an&req=5763742&lang=en)，但是由于效率是一个综合且复杂的问题，难以通过理论证明，因此在业界并没有达成共识。

## 方法

TDD 的实践方法可以归纳为三个周期和五个步骤。让开发变成一项富有节奏的活动而不是埋头苦干。

### 三周期

三个周期分别是“红”，“绿”，“重构”，分别是：

1. Write a test.

	写一个测试（“红”）； 

2. Make it run

	让测试通过（“绿”）； 

3. Make it right

	优化设计（“重构”）。

意图通过每个周期开发者只做一件事来降低心智负担：“红”周期专注于（业务）需求的正确理解，“绿”周期专注于代码能不能正确工作，“重构”周期专注于更好的代码（Clean Code），正如 Kent Beck 解释的这是分治法的一种体现：

> Divide and conquer, baby. First we'll solve the "that works" part of the problem. Then we'll solve the "clean code" part. This is the opposite of architecture-driven development, where you solve "clean code" first, then scramble around trying to integrate into the design the things you learn as you solve the "that works" problem.
>
> -- Kent Beck (2002-11-08). Test-Driven Development by Example. ISBN 978-0-321-14653-3.

因此严格要求开者必须确切地知道当前所处的周期，并专注当前周期的职责，只解决当前周期应该解决的问题。

### 五步骤

五个步骤实际上是指导三个周期相互的流转的方法，分别是：

1. 快速增加一项测试
2. 运行所有的测试并看到新增加的测试失败
3. 进行很小的改动
4. 运行所有的测试并看到全部成功
5. 重构以消除重复

可以参考流程图：

![img](https://upload.wikimedia.org/wikipedia/commons/0/0b/TDD_Global_Lifecycle.png)

在这五步之前还需要有一个准备的步骤，暂且称为步骤0:

0. 对任务进行拆分并以 AC 的方式输出列表

在 ThoughtWorks 内部通常被称为 Tasking，在 《Test-Driven Development by Example》 一书中也使用到了一项测试列表，然而两者并不能完全等价。ThoughtWorks 内部的 Tasking 不仅仅是开者的测试列表，还是开发者跟 BA、QA 等角色核对需求理解的重要实践，因此会倾向于任务的拆解需要有业务含义，使用纵切而不是横切。在 ThoughtWorks 内部之所以会产生这样的实践，我们认为是因为 ThoughtWorks 大多项目使用 Scrum 而不是 XP，XP 在需求及任务分解上有更多的活动层次支撑，能通过一系列的实践把任务拆分到分钟、甚至秒钟的细颗粒度：

![img](https://upload.wikimedia.org/wikipedia/commons/8/84/Extreme_Programming.svg)

而应用 Scrum 的项目，在开发者使用 TDD 进行开发时，需求很可能还停留在 Story 的层次，颗粒度可能长达数天，如果任务对于开发者来说过于庞大，则需要额外的活动来进一步拆解任务，以达到快速循环的 TDD 节奏。

### 法则

为执行好每一个步骤，有一些法则需要遵守，以下是一些法则的梳理，供实践参考：

#### 1. 快速增加一项测试 

##### 测试行覆盖率 100%

对代码的测试行覆盖率要求应该是 100%，因为所有的实现代码应该都是由测试代码驱动出来的，必然要做要 100 %。

> *Statement coverage* certainly is not a sufficient measure of test quality, but it is a starting place. TDD followed religiously should result in 100 percent statement coverage. 
>
> -- Kent Beck (2002-11-08). Test-Driven Development by Example.

##### 测试行为而非方法

测试 public 方法而非 private 方法，但这只是一种很狭隘的理解，或者应该在更高的层次测试 : 优先测试代码行为而非方法

##### 减少测试与实现的耦合

##### 测试即文档

使用测试描述代码的行为，测试的文档功能是TDD的附属产物。为了确保在单元测试中的投入得到良好的回报，必须保证其他人能够很容易地理解测试，否则你的测试就是在浪费他们的时间。

##### 保持简单

一个测试用例只关注一种情况

##### 等价替换

测试用例的设计不需要枚举所有可能的输入，等价的输入可以由一个测试用例来替代。

##### 测试优先级

Uncle Bob 设计了一个体系：变换优先级假设（Transformation Priority Premise，TPP），用于将每步变换分类。所有的变换按从最简单（最高优先级）到最复杂（最低优先级）安排优先级顺序。你要做的就是选择一个最高优先级的变换，然后为此变换写一个测试。按照变换优先级增量地进行，就能得到一个理想的测试顺序。参考：

https://en.wikipedia.org/wiki/Transformation_Priority_Premise

#### 2. Run all tests and see the new one fail. 

##### 写实现代码前必须先得要一个失败的测试

> Write a failing automated test before you write any code
>
> -- Kent Beck (2002-11-08). Test-Driven Development by Example.

必须运行测试以检查是否存在提前通过（premature passes）的现象。 下列原因可能会让测试提前通过： 

- 运行了错误的测试 
- 测试了错误的代码
- 不当的测试规范 
- 对系统的无效假设 
- 不佳的测试顺序 
- 相关联的产品代码 
- 过度编码 
- 确定性测试

#### 3. 进行很小的改动 

只编写能刚好让一个失败测试通过的产品代码。快速让测试通过的三个方法：

> The three approaches to making a test work cleanly - fake it, triangulation, and obvious implementation 
>
> ###### Fake It
>
> Return a constant and gradually replace constants with variables until you have the real code. 
>
> ###### Obvious Implementation
>
> Type in the real implementation. 
>
> ###### Triangulation 
>
> -- Kent Beck (2002-11-08). Test-Driven Development by Example.

#### 4. 运行所有的测试并看到全部成功 

##### 完成的定义

> Didn't mark a test as done because the duplication had not been eliminated
>
> -- Kent Beck (2002-11-08). Test-Driven Development by Example.

一项 Task 的完成不应该止步于实现代码通过了测试，而应该检查新增的实现代码是否存在重复，如果答案是“是”，应该进入步骤5消除重复。

#### 5. Refactor to remove duplication. 

##### 简单设计

步骤5中的重构主要以消除重复为目的，也可以运用简单设计的四法则来作为判断依据：

1. 通过测试
2. 揭示意图
3. 消除重复
4. 最少元素

参考：https://martinfowler.com/bliki/BeckDesignRules.html

#### 其它法则

##### 十分钟反馈

每个周期（红、绿、重构）应该在十分钟完成，如果限定的时间到了，那么考虑丢掉你之前的结果然后重新来过，而不是使用调试器对代码进行调查。因为十分钟之前的代码是好的，而好的版本控制工具（如Git）让回滚变得容易、迅速、有效，而且很安全。回滚后考虑采取更小的步伐。

##### 禁用测线用例

当某个测试用例不正当地成为你的 TDD 循环的障碍时，注释掉测试代码是可以的，但更好的方法是把测试标记为禁用。因为标记为禁用时，测试框架在测试报告中通常会输出提示，以避免被渐渐遗忘，而这个往往就是被注释掉的后果。

##### The Three Laws of TDD

Uncle Bob 为 TDD 提出了三条法则，可以作为补充：

1. You are not allowed to write any production code unless it is to make a failing unit test pass.
2. You are not allowed to write any more of a unit test than is sufficient to fail; and compilation failures are failures.
3. You are not allowed to write any more production code than is sufficient to pass the one failing unit test.

参考：http://butunclebob.com/ArticleS.UncleBob.TheThreeRulesOfTdd

### 扩展知识

关于TDD的理解业界有不同的声音，Kent Beck 提出的 TDD 原先只关注较局部的任务，而不是复杂系统中端到端的需求，这点可以从 XP 的实践及快速反馈的要求中看出来，而 ATDD 及 BDD 就是解决复杂系统中端到端的业务需求的实践，因此有些声音认为TDD应该指广义的TDD，包含 UTDD（或者 Developer TDD）与 ATDD、BDD 之类的实践，参考：

http://[www.netobjectives.net/files/pdfs/TestDrivenDevelopment_ATDDandUTDD.pdf](http://www.netobjectives.net/files/pdfs/TestDrivenDevelopment_ATDDandUTDD.pdf)

http://www.agiledata.org/essays/tdd.html#Documentation
