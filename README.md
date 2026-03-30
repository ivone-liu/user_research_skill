# ai-user-research

Author: Ivone <ivone@nibbly.cn>  
License: MIT

`ai-user-research` is an evidence-grounded research skill for independent developers, product managers, and early-stage founders.

It helps you:
- define the research task
- plan evidence sources
- generate 20+ personas
- use public evidence plus an embedded or user-provided persona CSV baseline
- partially reuse relevant persona seeds when appropriate
- generate missing personas when the library is incomplete
- ignore mismatched seeds while still reusing the CSV structure
- run persona deep dives
- update persona states
- produce structured decision outputs

## How to use

1. Describe the product and the validation goal.
2. Provide public evidence if you have it.
3. Optionally upload a persona CSV library.
4. Ask for 20+ personas.
5. Deep-dive selected personas.
6. Update persona states.
7. Ask for structured conclusions.

### Suggested prompt

```text
We are starting a new user research project.

Product name:
One-line definition:
Problem solved:
Target users:
Usage scenario:
Competitors / alternatives:
What I most want to validate:

Please:
1. structure the task
2. build a source plan
3. selectively use my persona CSV if relevant
4. generate 20+ personas
5. fill missing personas yourself if needed
6. output structured conclusions
```

## How the CSV is used

A persona CSV is treated as a **candidate archetype pool**, not as proof of demand.

The skill should choose one of three modes:

- partial reuse mode
- structure reuse mode
- public-sample dominant mode

This means:
- if some seeds are relevant, use only those
- if the content is weak but the structure is useful, keep the structure and generate new personas
- if the CSV is not relevant, rely mostly on public-sample synthesis

Embedded sample included in this package:
- rows: 3125
- columns: id, sys_platform, uuid, bstudio_create_time, name, avatar, profile, tag

Top tags:
- 自由职业者: 300
- 白领: 289
- 基层管理者: 280
- 大学生: 269
- 新房业主: 250
- 车主: 230
- 宝妈: 190
- 小城打工人: 190
- 新婚夫妇: 189
- 中小学生家长: 180
- 蓝领: 170
- 平台博主: 170
