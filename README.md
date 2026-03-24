# ai-user-research

Author: Ivone <ivone@nibbly.cn>  
License: MIT

`ai-user-research` is an evidence-grounded research skill for independent developers, product managers, and early-stage founders.

It helps you:
- define the research task
- plan evidence sources
- generate 20+ personas
- use public evidence plus optional persona CSV baselines
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
3. use my persona CSV if relevant
4. generate 20+ personas
5. output structured conclusions
```

## How the CSV is used

A user-provided persona CSV is treated as a persona baseline library, not as proof of demand.

It can help with:
- persona coverage
- archetype retrieval
- segmentation seeds
- better deep-dive realism

Current reviewed sample:
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
