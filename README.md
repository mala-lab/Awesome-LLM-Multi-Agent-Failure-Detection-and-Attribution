
# Awesome-LLM-Multi-Agent-Failure-Detection-and-Attribution

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-green)

A curated collection of papers on **failure detection, failure attribution, recovery, and learning from failures in LLM-based multi-agent systems (MAS)**.

This list also includes related agent debugging, diagnostic guardrails, and experience-based self-improvement. These broader settings are explicitly marked. Contributions of relevant papers, author-provided resources, and corrections are welcome.

## Contents

- [Scope and Taxonomy](#scope-and-taxonomy)
- [Failure Benchmarks and Taxonomies](#failure-benchmarks-and-taxonomies)
- [Failure Detection](#failure-detection)
  - [Representation-Based Monitoring](#representation-based-monitoring)
  - [Online Auditing and Message Checking](#online-auditing-and-message-checking)
  - [Proactive Failure Forecasting](#proactive-failure-forecasting)
- [Failure Attribution](#failure-attribution)
  - [Agent and Step Localization](#agent-and-step-localization)
  - [Hypothesis Verification](#hypothesis-verification)
  - [Dependency-Based Attribution](#dependency-based-attribution)
  - [Intervention-Based Debugging](#intervention-based-debugging)
  - [Related General-Agent Diagnosis](#related-general-agent-diagnosis)
- [Failure Mitigation and Recovery](#failure-mitigation-and-recovery)
  - [Attribution-Guided Reflection](#attribution-guided-reflection)
  - [Protocol Verification and Repair](#protocol-verification-and-repair)
  - [Cross-Listed Detection-to-Repair Methods](#cross-listed-detection-to-repair-methods)
- [Related Learning and Safety Methods](#related-learning-and-safety-methods)
  - [Learning Skills from Execution Experience](#learning-skills-from-execution-experience)
  - [Recursive Self-Improvement through Memory](#recursive-self-improvement-through-memory)
  - [Recursive Self-Improvement through Code](#recursive-self-improvement-through-code)
  - [Diagnostic Safety Guardrails](#diagnostic-safety-guardrails)
- [Method Comparison](#method-comparison)
- [Dataset and Resource Index](#dataset-and-resource-index)
- [Contributing](#contributing)

## Scope and Taxonomy

| Category | Main question | Typical output |
| --- | --- | --- |
| **Detection** | Is an observed execution step or message erroneous or risky? | Error score, audit verdict, or alarm |
| **Forecasting** | Does the observed execution prefix indicate an impending failure? | Predicted risk or an early warning |
| **Attribution** | Which agent, step, or error type explains a failure? | Responsible agent, decisive step, or failure category |
| **Mitigation / Recovery** | How can a faulty run or coordination protocol be corrected? | Revised message, intervention, repaired protocol, or retry |
| **Learning from Experience** | How can execution feedback improve subsequent attempts or tasks? | Reflection, memory, skills, code changes, or parameter updates |
| **Benchmarks / Taxonomies** | What failures should be annotated and evaluated? | Datasets, failure categories, and evaluation protocols |

**Reading conventions**

- **Online** methods inspect the available execution prefix or current message. This does not automatically mean they predict an error before it occurs.
- **Post-hoc** attribution usually diagnoses an already observed failure using a completed trajectory. Attribution methods should not be described as online detectors without checking their information access.
- **Recovery** during a run, **memory/skill updates** across tasks, and **model/code updates** are different forms of improvement.
- **General-agent** papers are relevant to MAS research but are not necessarily designed for multi-agent coordination.
- **Safety diagnosis** and **task-correctness diagnosis** have overlapping techniques but different target labels.
- Each paper has one full bibliographic entry. Cross-listings indicate additional capabilities without counting a paper twice.
- Unless a venue is explicitly supported by the linked author resource, the entry is labeled *arXiv*. Year-only arXiv entries use the first public release year; revised versions may be newer.
- Paper and resource links are provided individually. An omitted code link means it was not established in this review, not that no implementation exists.

## Failure Benchmarks and Taxonomies

- **[Cemri2025] MAST / MAST-Data.** Why Do Multi-Agent LLM Systems Fail? *arXiv*, 2025. [\[paper\]](https://arxiv.org/abs/2503.13657) [\[code/data\]](https://github.com/multi-agent-systems-failure-taxonomy/MAST)
- **[Zhang2025] Who&When.** Which Agent Causes Task Failures and When? On Automated Failure Attribution of LLM Multi-Agent Systems. *ICML*, 2025. [\[paper\]](https://arxiv.org/abs/2505.00212) [\[code\]](https://github.com/ag2ai/Agents_Failure_Attribution) [\[data\]](https://huggingface.co/datasets/Kevin355/Who_and_When)
- **[Deshpande2025] TRAIL.** TRAIL: Trace Reasoning and Agentic Issue Localization. *arXiv*, 2025. [\[paper\]](https://arxiv.org/abs/2505.08638) [\[data\]](https://huggingface.co/datasets/PatronusAI/TRAIL)
- **[Kong2026] Aegis / AEGIS-Bench.** Aegis: Automated Error Generation and Attribution for Multi-Agent Systems. *ICLR*, 2026. [\[paper\]](https://arxiv.org/abs/2509.14295) [\[code\]](https://github.com/kfq20/AEGIS) [\[data\]](https://huggingface.co/datasets/Fancylalala/AEGIS)
- **[Liu2026] Who&When Pro.** Who&When Pro: Can LLMs Really Attribute Failures in AI Agents? *ICMLW*, 2026. [\[paper\]](https://arxiv.org/abs/2607.09996)
- **[Chen2026] TraceElephant.** Seeing the Whole Elephant: A Benchmark for Failure Attribution in LLM-based Multi-Agent Systems. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2604.22708)
  
## Failure Detection

### Representation-Based Monitoring

- **[Shen2025] MASC.** Metacognitive Self-Correction for Multi-Agent System via Prototype-Guided Next-Execution Reconstruction. *arXiv*, 2025. [\[paper\]](https://arxiv.org/abs/2510.14319) [\[author-linked anonymous code\]](https://anonymous.4open.science/r/MASC-7194)


### Online Auditing and Message Checking

- **[Zhang2026] AgentForesight.** AgentForesight: Online Auditing for Early Failure Prediction in Multi-Agent Systems. *NeurIPS*, 2026. [\[paper\]](https://arxiv.org/abs/2605.08715) [\[code\]](https://github.com/ZBox1005/AgentForesight) [\[project\]](https://zbox1005.github.io/agent-foresight/) [\[data\]](https://huggingface.co/datasets/ZBox008003/AFTraj)
- **[Wang2026a] MASRubric.** MASRubric: Auditing Information Flow in Multi-Agent Systems with Failure-Distilled Pitfall Rubrics. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2602.23258) [\[author-linked repository\]](https://github.com/TonySY2/MASRubric)

### Proactive Failure Forecasting

- **[Zhao2026] ProMAS.** ProMAS: Proactive Error Forecasting for Multi-Agent Systems Using Markov Transition Dynamics. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2603.20260)

## Failure Attribution

### Representation-Based Monitoring

- **[Li2026] AFANet.** Beyond LLM-Based Reasoning: Lightweight GNNs for Agent Failure Attribution. *NeurIPS*, 2026. [\[paper\]](https://arxiv.org/abs/2608.18575)
- **[Deng2026] DeFA.** DeFA: Dependency-Guided Failure Attribution for LLM Agents. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2610.01256)
- **[Liao2026] EMFA.** Error-Propagation Modeling for Failure Attribution in LLM-Based Multi-Agent Systems. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2610.11600)
- **[Yeh2026] OAT.** Tracing Agentic Failure from the Flow of Success. *NeurIPS*, 2026. [\[paper\]](https://arxiv.org/abs/2607.12747) [\[code\]](https://github.com/deeplearning-wisc/OAT)
  
### Hypothesis Verification

- **[Qiao2026] VerifyMAS.** VerifyMAS: Hypothesis Verification for Failure Attribution in LLM Multi-Agent Systems. *NeurIPS*, 2026. [\[paper\]](https://arxiv.org/abs/2605.17467) [\[code\]](https://github.com/mala-lab/VerifyMAS) [\[project\]](https://hezheqiao2022.github.io/VerifyMAS/)

### Intervention-Based Debugging

- **[Ma2025] DoVer.** DoVer: Intervention-Driven Auto Debugging for LLM Multi-Agent Systems. *arXiv*, 2025. [\[paper\]](https://arxiv.org/abs/2512.06749)

### Related General-Agent Diagnosis

- **[Zhu2025] AgentDebug.** Where LLM Agents Fail and How They can Learn From Failures. *arXiv*, 2025. [\[paper\]](https://arxiv.org/abs/2509.25370) [\[code\]](https://github.com/ulab-uiuc/AgentDebug)



## Failure Mitigation and Recovery

### Attribution-Guided Reflection

- **[Wang2026b] DoCtOR.** Finding Where the Buck Stops: An Automated Failure Attribution-Based Reflection Framework for Multi-Agent Collaboration. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2608.28264)

### Protocol Verification and Repair

- **[Xia2026] TraceFix.** TraceFix: Repairing Agent Coordination Protocols with TLA+ Counterexamples. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2605.07935)

### Cross-Listed Detection-to-Repair Methods

| Method | Failure signal | Intervention or subsequent improvement |
| --- | --- | --- |
| MASC | Representation anomaly | Revises the flagged agent output |
| MASRubric | Failure-derived checking criteria | Returns a message for revision or withholds it |
| DoVer | Intervention outcomes | Tests and retains task-repairing changes |
| AgentDebug | Error and root-cause diagnosis | Provides targeted corrective feedback |
| DeFA | Dependency-guided diagnosis | Supplies feedback for skill evolution |

Full paper entries appear above; these are additional capabilities, not additional papers.

## Related Learning and Safety Methods

### Learning Skills from Execution Experience

- **[Ni2026] Trace2Skill.** Trace2Skill: Distill Trajectory-Local Lessons into Transferable Agent Skills. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2603.25158)

### Recursive Self-Improvement through Memory

- **[Zhu2026] RSIAgent.** RSIAgent: Autonomous Exploration for Recursive Self-improvement in New Environments. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2609.15364) [\[code\]](https://github.com/razor-ai/rsiagent) [\[project\]](https://aetherlabsai.github.io/RSIAgent/)

### Recursive Self-Improvement through Code

- **[Srikanth2026] AIDE².** Recursive self-improvement of AI research agents. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2609.26457)

### Diagnostic Safety Guardrails

- **[Liu2026b] AgentDoG.** AgentDoG: A Diagnostic Guardrail Framework for AI Agent Safety and Security. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2601.18491) [\[code\]](https://github.com/AI45Lab/AgentDoG)

## Method Comparison

The categories below are editorial groupings based on each paper's stated task and mechanism. They are not a standardized benchmark ranking.

| Method | Main scope | Information or feedback | Main output / update |
| --- | --- | --- | --- |
| MASC | MAS detection + recovery | Current step, history, learned normality | Error score; revised output |
| AgentForesight | MAS online auditing | Observed trajectory prefix | Continue/alarm; agent-step localization |
| MASRubric | MAS message auditing | Current message and retrieved pitfalls | Broadcast/revise/withhold |
| ProMAS | MAS forecasting | Observed semantic transitions | Risk-based early warning |
| VerifyMAS | MAS attribution | Full trajectory and error hypotheses | Validated errors and responsible agents |
| DeFA | Agent/MAS attribution | Step content and dependency graphs | Agent, decisive step, error category |
| DoVer | MAS debugging | Logs, interventions, re-execution | Tested task repairs |
| AgentDebug | General-agent diagnosis | Module-level trajectory analysis | Critical errors and corrective feedback |
| DoCtOR | MAS reflection | Attribution and counterfactual correction | Responsible-agent reflection |
| TraceFix | MAS protocol repair | Formal counterexamples and runtime operations | Repaired protocol; coordination enforcement |
| Trace2Skill | General-agent learning | Execution experience | Transferable skills |
| RSIAgent | Multi-agent exploration | Environment execution and verifier feedback | Persistent memory |
| AIDE² | Research-agent self-improvement | Evaluation of candidate implementations | Agent code changes |
| AgentDoG | Agent safety diagnosis | Agent trajectory and risk taxonomy | Risk labels and diagnostic rationale |

## Dataset and Resource Index

| Resource | Main use | Link |
| --- | --- | --- |
| MAST / MAST-Data | MAS failure taxonomy and annotated traces | [Repository](https://github.com/multi-agent-systems-failure-taxonomy/MAST) |
| Who&When | Responsible-agent and decisive-step attribution | [Dataset](https://huggingface.co/datasets/Kevin355/Who_and_When) |
| Who&When Pro | Multimodal failure attribution | [Paper](https://arxiv.org/abs/2607.09996) |
| AEGIS | Controlled error generation and attribution | [Dataset](https://huggingface.co/datasets/Fancylalala/AEGIS) |
| TRAIL | Trace error categorization and localization | [Dataset](https://huggingface.co/datasets/PatronusAI/TRAIL) |
| TraceElephant | Full-observability attribution | [Paper](https://arxiv.org/abs/2604.22708) |
| AFTraj-2K | Safe/unsafe trajectories for online auditing | [Dataset](https://huggingface.co/datasets/ZBox008003/AFTraj) |
| AgentErrorBench | General-agent errors and debugging | [Project repository](https://github.com/ulab-uiuc/AgentDebug) |
| ATBench | Agent safety evaluation and diagnosis | [Dataset](https://huggingface.co/datasets/AI45Research/ATBench) |


