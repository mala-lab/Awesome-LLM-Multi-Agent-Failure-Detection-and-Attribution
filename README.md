# Awesome-LLM-Multi-Agent-Failure-Detection-and-Attribution

# Awesome-LLM-Multi-Agent-Failure-Detection-and-Attribution

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-green)

A curated collection of papers on **failure detection, failure attribution, recovery, and learning from failures in LLM-based multi-agent systems (MAS)**.

This list also includes related agent debugging, diagnostic guardrails, and experience-based self-improvement. These broader settings are explicitly marked.

**Last reviewed:** October 19, 2026. **Coverage:** 20 unique papers. This is a focused reading list, not an exhaustive survey.

Contributions of relevant papers, author-provided resources, and corrections are welcome.

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
  - Empirical MAS failure analysis with a taxonomy covering system design, inter-agent misalignment, and task verification. Useful for defining error categories and studying failure patterns.

- **[Zhang2025] Who&When.** Which Agent Causes Task Failures and When? On Automated Failure Attribution of LLM Multi-Agent Systems. *ICML*, 2025. [\[paper\]](https://arxiv.org/abs/2505.00212) [\[code\]](https://github.com/ag2ai/Agents_Failure_Attribution) [\[data\]](https://huggingface.co/datasets/Kevin355/Who_and_When)
  - Defines agent-and-step attribution and introduces annotated MAS failures. Includes all-at-once, step-by-step, and binary-search attribution baselines.

- **[Deshpande2025] TRAIL.** TRAIL: Trace Reasoning and Agentic Issue Localization. *arXiv*, 2025. [\[paper\]](https://arxiv.org/abs/2505.08638) [\[data\]](https://huggingface.co/datasets/PatronusAI/TRAIL)
  - Human-annotated traces for error categorization and localization. Includes both single-agent and multi-agent workflows; its scope is broader than MAS-only attribution.

- **[Kong2026] Aegis / AEGIS-Bench.** Aegis: Automated Error Generation and Attribution for Multi-Agent Systems. *ICLR*, 2026. [\[paper\]](https://arxiv.org/abs/2509.14295) [\[code\]](https://github.com/kfq20/AEGIS) [\[data\]](https://huggingface.co/datasets/Fancylalala/AEGIS)
  - Produces labeled MAS failures through controlled, context-aware error injection. Supports learning and evaluation with faulty-agent and error-mode annotations. First released as a preprint in 2025.

- **[Liu2026] Who&When Pro.** Who&When Pro: Can LLMs Really Attribute Failures in AI Agents? *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2607.09996)
  - Extends attribution evaluation across multiple modalities and task benchmarks using a controlled successful-prefix replay and failure-injection pipeline.

- **[Chen2026] TraceElephant.** Seeing the Whole Elephant: A Benchmark for Failure Attribution in LLM-based Multi-Agent Systems. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2604.22708)
  - Studies failure attribution with full execution traces and reproducible environments, emphasizing the importance of observing inputs and context alongside agent outputs.

## Failure Detection

### Representation-Based Monitoring

- **[Shen2025] MASC.** Metacognitive Self-Correction for Multi-Agent System via Prototype-Guided Next-Execution Reconstruction. *arXiv*, 2025. [\[paper\]](https://arxiv.org/abs/2510.14319) [\[author-linked anonymous code\]](https://anonymous.4open.science/r/MASC-7194)
  - **Setting:** Online step monitoring.
  - **Signal:** History-conditioned next-step embedding reconstruction and a learned normal-step prototype prior.
  - **Response:** A detected deviation can trigger a correction agent before the message propagates downstream.

### Online Auditing and Message Checking

- **[Zhang2026] AgentForesight.** AgentForesight: Online Auditing for Early Failure Prediction in Multi-Agent Systems. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2605.08715) [\[code\]](https://github.com/ZBox1005/AgentForesight) [\[project\]](https://zbox1005.github.io/agent-foresight/) [\[data\]](https://huggingface.co/datasets/ZBox008003/AFTraj)
  - **Setting:** Prefix-only online auditing.
  - **Signal:** An RL-trained auditor produces continue/alarm decisions and localizes the decisive agent and step.
  - **Resource:** AFTraj-2K includes safe and unsafe trajectories for studying online alarms.

- **[Wang2026a] MASRubric.** MASRubric: Auditing Information Flow in Multi-Agent Systems with Failure-Distilled Pitfall Rubrics. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2602.23258) [\[author-linked repository\]](https://github.com/TonySY2/MASRubric)
  - **Setting:** Online checking of intermediate messages.
  - **Signal:** Retrieves applicable checking criteria from a rubric bank distilled from historical failures.
  - **Response:** Routes a message to broadcast, revision, or withholding based on the audit.

### Proactive Failure Forecasting

- **[Zhao2026] ProMAS.** ProMAS: Proactive Error Forecasting for Multi-Agent Systems Using Markov Transition Dynamics. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2603.20260)
  - **Setting:** Prefix-based proactive error analysis.
  - **Signal:** Semantic changes are represented through Markov transitions, with a prediction head and risk-jump detection.
  - **Focus:** Earlier warning with reduced trajectory observation; forecasting and observed-step detection should be evaluated separately.

## Failure Attribution

### Agent and Step Localization

See **[Who&When](#failure-benchmarks-and-taxonomies)** for the task formulation, benchmark, and three basic localization approaches. These baselines analyze a failed trajectory rather than providing a prefix-only online alarm protocol.

### Hypothesis Verification

- **[Qiao2026] VerifyMAS.** VerifyMAS: Hypothesis Verification for Failure Attribution in LLM Multi-Agent Systems. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2605.17467) [\[code\]](https://github.com/mala-lab/VerifyMAS) [\[project\]](https://hezheqiao2022.github.io/VerifyMAS/)
  - **Setting:** Full-trajectory failure attribution.
  - **Mechanism:** Verifies candidate error hypotheses against the trajectory, then identifies responsible agents.
  - **Focus:** Error-first verification for local and global failure patterns, including cross-agent inconsistencies.

### Dependency-Based Attribution

- **[Deng2026] DeFA.** DeFA: Dependency-Guided Failure Attribution for LLM Agents. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2610.01256)
  - **Setting:** Trajectory-based attribution, including multi-agent evaluations.
  - **Mechanism:** Builds event-dependency and failure-propagation graphs to identify the decisive step, responsible agent, and error category.
  - **Improvement link:** Evaluates diagnostic feedback as input to subsequent skill evolution.

### Intervention-Based Debugging

- **[Ma2025] DoVer.** DoVer: Intervention-Driven Auto Debugging for LLM Multi-Agent Systems. *arXiv*, 2025. [\[paper\]](https://arxiv.org/abs/2512.06749)
  - **Setting:** Debugging with access to execution interventions.
  - **Mechanism:** Tests failure hypotheses by editing messages or plans and observing subsequent execution.
  - **Focus:** Whether an intervention repairs the task or produces progress, rather than assuming one uniquely correct agent-step attribution.

### Related General-Agent Diagnosis

- **[Zhu2025] AgentDebug.** Where LLM Agents Fail and How They can Learn From Failures. *arXiv*, 2025. [\[paper\]](https://arxiv.org/abs/2509.25370) [\[code\]](https://github.com/ulab-uiuc/AgentDebug)
  - **Scope:** General LLM agents; relevant to MAS, but not exclusively a coordination-failure method.
  - **Mechanism:** Combines module-level error analysis with critical-error identification and corrective feedback.
  - **Resources:** AgentErrorTaxonomy and AgentErrorBench cover failures in memory, reflection, planning, action, and system operations.

## Failure Mitigation and Recovery

### Attribution-Guided Reflection

- **[Wang2026b] DoCtOR.** Finding Where the Buck Stops: An Automated Failure Attribution-Based Reflection Framework for Multi-Agent Collaboration. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2608.28264)
  - **Mechanism:** Attributes a failure, constructs a counterfactual corrected decisive step, and directs reflection to the responsible agent.
  - **Focus:** Avoids making every agent learn a lesson from a failure that only some agents caused.

### Protocol Verification and Repair

- **[Xia2026] TraceFix.** TraceFix: Repairing Agent Coordination Protocols with TLA+ Counterexamples. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2605.07935)
  - **Mechanism:** Generates coordination protocols, repairs them using model-checker counterexamples, and enforces topology constraints during execution.
  - **Scope boundary:** Protocol compliance and coordination properties; not a general verifier of factual or semantic correctness at every reasoning step.

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
  - **Scope:** General agent skill learning.
  - **Feedback:** Execution experience, recurring pitfalls, and workarounds.
  - **Update:** Consolidates experience into reusable skills and operating procedures without parameter updates. It is not primarily an online MAS failure detector.

### Recursive Self-Improvement through Memory

- **[Zhu2026] RSIAgent.** RSIAgent: Autonomous Exploration for Recursive Self-improvement in New Environments. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2609.15364) [\[code\]](https://github.com/razor-ai/rsiagent) [\[project\]](https://aetherlabsai.github.io/RSIAgent/)
  - **Scope:** Multi-agent exploration with curriculum, actor, and verifier roles.
  - **Feedback:** Actual execution and verifier feedback during broad-then-deep exploration.
  - **Update:** Stores reusable environment knowledge and failure lessons in memory. Model weights remain fixed; memory is frozen for downstream evaluation.

### Recursive Self-Improvement through Code

- **[Srikanth2026] AIDE².** Recursive self-improvement of AI research agents. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2609.26457)
  - **Scope:** General AI research agents, not specifically MAS failure attribution.
  - **Feedback:** Benchmarked performance of candidate agent implementations on development tasks and hidden evaluations.
  - **Update:** Changes the agent's own code; this differs from reflecting on one failed trajectory or updating a memory bank.

### Diagnostic Safety Guardrails

- **[Liu2026b] AgentDoG.** AgentDoG: A Diagnostic Guardrail Framework for AI Agent Safety and Security. *arXiv*, 2026. [\[paper\]](https://arxiv.org/abs/2601.18491) [\[code\]](https://github.com/AI45Lab/AgentDoG)
  - **Scope:** General agent safety and security.
  - **Signal:** Taxonomy-based diagnosis of risk sources, failure modes, and consequences.
  - **Scope boundary:** Safety diagnosis is related to, but does not replace, task-failure attribution in MAS. The linked repository also hosts later AgentDoG-family work; this entry refers to the original paper.

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

Dataset names, label taxonomies, observation access, and task scopes differ. Results across these resources should not be compared as though they were the same detection or attribution task.

## Contributing

Please include:

1. Full paper title, authors, year, and a canonical paper URL.
2. The most appropriate category and whether the setting is MAS-specific or general-agent.
3. Whether inference uses a prefix, a full trajectory, external verification, or re-execution.
4. A short explanation of the failure signal and any recovery or learning mechanism.
5. Author-provided code/data links when available.

Use one full entry per paper, with cross-references for additional capabilities. Label papers as preprints unless the venue is supported by a publication or author source. Do not equate memory updates, code changes, and parameter training.

**Entry template**

```markdown
- **[AuthorYear] Method.** Full Paper Title. *Venue or arXiv*, Year. [\[paper\]](URL) [\[code\]](URL)
  - **Scope / setting:** MAS or general-agent; online or post-hoc.
  - **Mechanism:** What evidence determines failure or responsibility?
  - **Response / update:** Alarm, attribution, correction, memory, skill, code, or parameters.
```

## Acknowledgment

The category-and-entry presentation was inspired by [Awesome-LLM-LVLM-Hallucination-Detection-and-Mitigation](https://github.com/mala-lab/Awesome-LLM-LVLM-Hallucination-Detection-and-Mitigation).

