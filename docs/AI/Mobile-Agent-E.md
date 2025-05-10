



概念
MLLM  是多模态大语言模型（Multimodal Large Language Model）的缩写。这种模型不仅可以处理文本数据，还能够理解和生成其他类型的数据，如图像、音频和视频等。这使得多模态大语言模型可以在更加丰富的上下文中进行信息处理和生成，从而在广泛的应用场景中发挥更大的作用，例如图像生成与描述、视频内容分析与理解等。

模态在人工智能和计算机科学中通常指的是数据的不同表现形式或类型，例如文本、图像、音频、视频等。每一种模态代表一种独特的信息传递方式和理解方式。以下是一些常见的模态：

文本模态：包括书面语言、文字信息等。

视觉模态：涉及图像和视频数据。

音频模态：包括语音和音乐。

触觉模态：涉及通过触觉反馈传递的信息。

多模态系统可以结合这些不同的模态，以更加综合和丰富的方式来理解和处理信息。例如，多模态大语言模型可以同时处理文本和图像，从而提供更全面的分析和解释。
















角色划分
Manger 规划与任务拆分

responsible for devising overall plans by breaking down complex tasks into subgoals
负责通过将复杂任务分解为子目标来制定总体计划。
Perceptor 视觉感知

fine-grained visual perception 细粒度视觉感知
Operator 行动决策

immediate action execution 立即执行操作
Action Reflector 结果验证

error verification “错误校验；错误核实”。
Notetaker 信息聚合

information aggregation 信息聚合 
selfevolution module  自我进化模块

long-term memory 长期记忆

Tips 

general guidance on effective interactions and lessons learned from previous trail-and-error experiences.
之前任务中总结出的关于如何有效与环境交互的通用指导和经验教训。
Shortcuts 

reusable, executable functions that contains sequences of atomic operations tailored to efficiently complete recurring subroutines under specific preconditions.
可重用的可执行功能，包含为在特定前提下高效完成重复子例程而量身定制的原子操作序列
Experience Reflectors 经验反射器

After completing each task, the Experience Reflectors are triggered to update the Tips and propose new Shortcuts based on the interaction history. These are then fed to the Manager and Operator, enabling improved planning and action decision-making in future tasks.
在完成每个任务后，经验反射器被触发以更新提示并根据互动历史提出新的捷径。这些信息随后被提供给管理者和操作员，从而在未来任务中改善规划和行动决策






















MobileAgentE 流程总结
每一轮迭代都可以分为规划阶段、执行阶段、反馈评估阶段、信息记录阶段、经验反思阶段

PlantUML 代码标识任务执行主要流程

@startuml
title run_single_task 主要流程


start


:初始化日志目录和信息池;
:初始化感知器和智能体;


:第一次感知获取屏幕信息;


while (是否继续?) is (yes)
  :Manager进行高层规划;
  
  if (是否完成任务?) then (yes)
    :经验反思更新Tips和Shortcuts;
    :保存日志;
    stop
  endif
  
  :Operator执行动作决策;
  :执行具体动作;
  :Perceptor获取新的屏幕信息;
  
  :ActionReflector评估动作结果;
  
  if (动作是否成功?) then (yes)
    :Notetaker记录重要内容;
  else (no)
    :记录错误信息;
  endif
  
  if (是否达到终止条件?) then (yes)
    :记录终止原因;
    stop
  endif
  
  :等待下一轮迭代;
endwhile


stop


legend right
  终止条件包括:
  * 达到最大迭代次数(max_itr)
  * 连续失败次数过多(max_consecutive_failures)
  * 重复动作次数过多(max_repetitive_actions)
endlegend


@enduml









主要流程说明：
初始化阶段：

创建日志目录

初始化信息池(InfoPool)

初始化各种智能体

主循环阶段：

Manager进行高层规划

Operator执行具体动作

Perceptor感知环境变化

ActionReflector评估动作结果

Notetaker记录重要信息

终止条件：

任务完成("Finished")

达到最大迭代次数(max_itr)

连续失败次数过多(max_consecutive_failures)

重复动作次数过多(max_repetitive_actions)

结束阶段：

经验反思更新Tips和Shortcuts

保存日志

清理资源





论文总结
“Mobile-Agent-E: Self-Evolving Mobile Assistant for Complex Tasks”由王振海龙等人提出，介绍了一种用于复杂任务的自主进化移动助手Mobile-Agent-E，该助手具有分层多智能体框架和自进化模块，能够有效处理现实世界中的复杂任务，并在学习经验中不断改进自身性能。文章还介绍了新的基准测试Mobile-Eval-E及相关评估指标，实验结果表明Mobile-Agent-E相比之前的先进方法有显著提升。
1. 研究背景
智能手机在现代生活中不可或缺，但操作复杂的移动设备任务仍令人沮丧。

基于大型多模态模型（LMM）的移动助手虽有进展，但仍存在局限性： 

与实际人类需求存在差距，难以处理需要深入推理、长周期规划和探索的复杂任务。

缺乏从先前经验中学习和改进的能力，每次都像首次执行任务一样分配资源并重复错误。

2. Mobile-Agent-E介绍
2.1分层多智能体框架
整体架构 

包括一个Manager（管理者）和四个下属智能体：Perceptor（感知器）、Operator（操作器）、Action Reflector（动作反射器）和Notetaker（笔记员）。

Manager负责制定总体计划，将复杂任务分解为子目标；其他智能体分别处理视觉感知、动作执行、结果验证和信息聚合。

框架通过分离高层规划和低层动作决策，提高了长期规划和错误恢复能力。

各智能体详细功能 

Manager 

输入包括任务查询、当前屏幕截图、之前的总体计划、子目标、进度状态、长期记忆中的快捷方式和重要笔记等。

根据这些信息更新总体计划并确定下一个立即要实现的子目标。当可能陷入错误循环时，根据额外的错误信息从更高层面调整计划或子目标。

Perceptor 

进行细粒度的视觉感知，检测和识别手机状态的丰富信息，如图标和文本。

由OCR模型、图标定位模型和图标字幕模型组成，根据当前屏幕截图生成文本和图标列表及其坐标。

Operator 

根据任务查询、总体计划、当前子目标、之前的进度状态、重要笔记、动作历史和错误历史以及长期记忆中的提示等决定具体的动作。

考虑提示作为指导，同时利用感知器提供的细粒度感知结果和屏幕截图来确定动作参数。

Action Reflector 

验证每个动作的结果，跟踪进度并提供错误反馈。

当连续出现k个失败动作时，向Manager发出错误升级标志，触发错误处理机制。

Notetaker 

在导航过程中聚合重要信息。

2.2自进化模块
长期记忆 

包含Tips（一般指导和从过去经验中学习的教训）和Shortcuts（针对特定子例程的可重用可执行操作序列）。

经验反射器 

包括用于Shortcuts的经验反射器（AES）和用于Tips的经验反射器（AET）。

在每个任务完成后，经验反射器根据交互历史更新Tips并提出新的Shortcuts，然后将它们提供给Manager和Operator，以改进未来的任务规划和动作决策。

3. Mobile-Eval-E基准测试及评估指标
基准测试介绍 

设计用于评估复杂的现实世界移动任务，包含比之前基准测试更多的预期操作，并且更多任务需要跨多个应用程序交互。

评估指标 

引入满意度得分（Satisfaction Score），基于人工编写的规则计算，考虑里程碑完成和探索性行为等因素。

提出满意度得分与步骤（SSS）曲线，用于评估和可视化移动代理的效率。

4. 实验结果
性能提升 

在三种不同的基础模型骨干上，Mobile-Agent-E相比之前的先进方法平均绝对提升了22.1%。

自进化机制带来了6.5%的绝对性能提升，且Shortcuts减少了计算开销，实现了与先前模型相当的速度，同时性能显著提高。

自我进化行为 

在性能和效率方面展示了自我进化行为，验证了自进化模块的有效性。

