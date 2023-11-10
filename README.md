> Code availability is forthcoming, as it is presently undergoing extensive cleanup and refactoring. Stay tuned for updates or request access if needed urgently.

__To-Do:__

[] Incorporate an example of a task split loop

## Introduction
"Hi Planner" refers to a hierarchical or high-level planning system. This research aims to introduce an alternative to the prevalent sequential planners such as LangChain and SemanticKernel, with a focus exclusively on the plan generation phase, omitting the execution phase.
Nowel ideas compared to sequential planner 

- task execution planning extarcted as separate activity
- ability to ask for help
- additional functionality generation 
- task split into smaller sub-tasks

## Comparison of sequential and hierarchical planners
Both sequential and hierarchical planners begin with identical inputs: a task description and a skill set. The Sequential planner may yield a plan or an error. In contrast, the Hi Planner can generate basic functions or request additional information or functions if it cannot construct a plan.

![hi planner vs sequential](/assets/sequential_vs_hi_planners.png)

## Example
Consider the task: ```Summarize an input, translate it to French, and email it to John Doe.``` For this task, the following functions are available:
```YAML
- SummarizePlugin.Summarize
- WriterPlugin.Translate
- email.GetEmailAddress
- email.SendEmail
```
Both __sequential__ and __hi planners__ would output the same sequence of actions:

```YAML
Steps:
  - SummarizePlugin.Summarize input='$INPUT' => SUMMARY
  - WriterPlugin.Translate input='$SUMMARY' => TRANSLATED_SUMMARY
  - email.GetEmailAddress input='John Doe' => EMAIL_ADDRESS
  - email.SendEmail input='$TRANSLATED_SUMMARY' email_address='$EMAIL_ADDRESS'
```

If given the same objective _without any_ available functions, the sequential planner would return an error due to the inability to create a plan. Meanwhile, the hi planner would provide a more detailed response:

```YAML
Functions created:
    - Summarize
    - Translate
    - SendEmail
Information request:
    - Email address of John Doe
```
## Algorithm
The planning creation process encompasses various facets:

- Sequential planning - conventional approach
- Skill creation using Python, LLM, or other methods
- Task decomposition into subtasks
- Decision-making block to determine the subsequent steps (new skill creation, task splitting, or information/functionality request)

![decision block](/assets/create_plan.png)

### Skill Creation
Initially, we use simple code generation or prompts, but this can certainly be expanded upon.

### Task Split
This step is akin to creating a sequential plan; however, it involves generating descriptions of functions necessary for plan formulation.

![Task splitting process](/assets/task_split.png)


[Click for full version](/docs/task_split_example_1.md)

```yaml
Copy code
Task: Summarize, translate, and email text
Inputs:
    - Name: input_text
      Value: "This is a sample text that needs to be summarized, translated, and emailed."
Functions: 
    - Name: summarize_text
      Description: This function summarizes text using an algorithm.
      Verification: Ensures the output is a concise yet comprehensive version of the input.
      Parameters:
          - Name: text
            Description: The text to be summarized
      Outputs:
          - Description: The summarized text
    ...

Steps:
    - Name: Summarize the text
    ...
    - Name: Translate the text
    ...
    - Name: Email the text
```

Subsequent processing will focus on implementing these functions, disregarding the steps described.

### Decision Block
Here, based on the task, available skills, and steps already taken, we decide:

Whether to develop a new skill using various approaches like Python coding, LLM prompting, shell scripting, etc.
Whether to break down the task into more manageable subtasks
When to request assistance, either due to a lack of information or skills (including newly generated ones)
Decision-making process

Key considerations include:

- Shifting from solving tasks to more general function generation
- Detecting loops, where the task splits but the subtasks are too similar to the original (see task split loop)
- Utilizing LLM as an expert system rather than rigidly adhering to hardcoded rules

## Conclusion and Future Work
This project was initiated as part of a larger endeavor to develop a Workforce Agent. An interactive and capable planner is vital for this venture.

### Integrating Domain Knowledge
By designing the decision block as described, we can enhance it with various tool creation strategies and incorporate domain knowledge. This knowledge could relate to both task execution and planning for execution.

### Isolating Prompts to a Separate Repository
Since all logic currently relies on prompt engineering, it could be isolated to a dedicated repository. This separation would allow for distinct development cycles between the main codebase and AI prompting strategies.

Additional research and development will refine these concepts to deliver a planner that is not only more interactive but also more capable of handling complex tasks.