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

## Introduction
The "Hi Planner" refers to a hierarchical or high-level planning system. It's a sophisticated hierarchical planning system designed to offer an innovative approach to task organization and strategy development. Unlike traditional sequential planners in _LangChain_ and _SemanticKernel_, which operate linearly on given set of provided functions, the _Hi Planner_ specializes in the generative aspect of planning—crafting the blueprint for task completion without delving into the actual execution. This research introduces several novel concepts that set the _Hi Planner_ apart from its sequential counterparts:

- __Task Execution Planning as a Distinct Process:__ The Hi Planner treats the planning of task execution as a standalone process, independent of the actual execution phase. This separation allows for more intricate and considered planning, ensuring that the blueprint is robust before any action is taken.

- __Provision for Assistance Requests:__ Acknowledging the complexity of certain tasks, the __Hi Planner__ is equipped with the capability to solicit external assistance. Whether it's a call for more information or a need for additional resources, this feature ensures that the planner remains effective even in the face of unforeseen challenges.

- __Generation of Additional Functionalities:__ The __Hi Planner__ is not just reactive but also proactive in its strategy, capable of generating additional functionalities as needed. This adaptability enables the system to tailor its planning process to the specific demands of each task.

- __Decomposition into Sub-Tasks:__ Breaking down complex tasks into more manageable sub-tasks is a core feature of the Hi Planner. This methodical segmentation allows for a more organized approach to planning and facilitates the distribution of work among different components or teams. 

Each of these elements contributes to a planning system that is not only more flexible and responsive to the nuances of task management but also more efficient in orchestrating complex workflows.

## Distinctive Features of Sequential and Hierarchical Planners
Sequential and hierarchical planners operate from the same starting point, requiring a detailed task description and an inventory of available skills. The path they take after receiving these inputs, however, significantly diverges:

-__Sequential Planner Outcomes:__ A sequential planner operates on a linear trajectory. It processes the input and attempts to map out a step-by-step plan based on the skill set provided. The result is binary: the planner will either successfully formulate a complete plan or halt with an error if it encounters a scenario outside its capabilities.

-__Hi Planner's Adaptive Planning:__ In contrast, the Hi Planner embodies a more dynamic approach. Should it encounter a gap in its ability to construct a plan, it doesn't stop at an error. Instead, it explores alternative routes: it can autonomously create rudimentary functions to bridge the skills gap, or it can actively seek further information or additional functions. This not only prevents a standstill in the planning process but also enriches the planner's capability to handle complex, multifaceted tasks.

The comparison highlights the Hi Planner's adaptability and resilience in planning, traits that are essential for managing intricate and evolving tasks.

<p align="center">
    <img src="/assets/sequential_vs_hi_planners.jpg" alt="hi planner vs sequential" width="50%">
</p>


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

<p align="center">
    <img src="/assets/create_plan.jpg" alt="decision block" width="70%">
</p>

### Skill Creation
Initially, we use simple code generation or prompts, but this can certainly be expanded upon. Looking at progress in different projects like GptEnginer and recent demo from GitHub Autopilots - generating working functions looks totally feasible

### Task Split
This step is akin to creating a sequential plan; however, it involves generating descriptions of functions necessary for plan formulation.

<p align="center">
    <img src="/assets/task_split.jpg" alt="Task splitting process" width="70%">
</p>


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

- Whether to develop a new skill using various approaches like Python coding, LLM prompting, shell scripting, etc.
- Whether to break down the task into more manageable subtasks
- When to request assistance, either due to a lack of information or skills (including newly generated ones)
Decision-making process

Key considerations include:

- Shifting from solving tasks to more general function generation
- Detecting loops, where the task splits but the subtasks are too similar to the original (see task split loop)
- Utilizing LLM as an expert system rather than rigidly adhering to hardcoded rules

## Conclusion and Future Work
This project was initiated as part of a larger endeavor to develop a Workforce Agent. An interactive and capable planner is vital for this venture.

### Integrating Domain Knowledge
By designing the decision block as described, we can enhance it with various tool creation strategies and incorporate domain knowledge. This knowledge could relate to both task execution and planning for execution. This knowdge can be in the form of text to be incorporated into prompts thus making possible to educate mnodel without both LLM change and hardcoded parts rewritten. 
Additional information is similar to RAG (Retrival Augmnented Generation), however knowledge on planning strategies and tools geenration is closer to representing knowledge (not information) in expert systems, howerver similar tools might be used.

### Isolating Prompts to a Separate Repository
Since all logic currently relies on prompt engineering, it could be isolated to a dedicated repository. This separation would allow for distinct development cycles between the main codebase and AI prompting strategies.

### Environment aware
Anylizing tasks in [Evaluating Language-Model Agents on Realistic Autonomous Tasks](https://evals.alignment.org/blog/2023-08-01-new-report/) there is clear difference in environment agents work in
- cli for cloud provider, linux shell etc. 
- working with external API to get information
- interact with people vie different medias
- etc.
Current implemntation of hi planner has no enviroment awareness, this can be improved thus making planner even more flexible. 

Additional research and development will refine these concepts to deliver a planner that is not only more interactive but also more capable of handling complex tasks.

## Conclusion and Future Work
The inception of this project is a strategic step toward creating a Workforce Agent. At the heart of this system lies an interactive and proficient planner, integral to the framework's success.

### Integrating Domain Knowledge
The decision block's architecture is deliberately designed to be expansive, paving the way for the integration of specialized tool creation strategies and domain-specific knowledge. This knowledge, extending beyond mere data, can be textually integrated into prompts, thus educating the model without necessitating direct changes to the LLM or overhauling hardcoded components. It moves toward a model reminiscent of RAG (Retrieval Augmented Generation), focusing on embedding expertise akin to that found in expert systems, possibly utilizing similar methodologies.

### Isolating Prompts to a Separate Repository
Given the reliance of the current logic on prompt engineering, compartmentalizing it into a standalone repository is a logical progression. This would facilitate independent development cycles, allowing for the main codebase and AI prompting techniques to evolve asynchronously.

### Environment Awareness
Evaluating agents within diverse operational contexts, as seen in Evaluating Language-Model Agents on Realistic Autonomous Tasks, reveals a pronounced variance in the environments they engage with, such as:

- Command-line interfaces for cloud services or Linux shell tasks.
- Utilization of external APIs for information retrieval.
- Interaction with individuals across various communication platforms.

The current __Hi Planner__ iteration lacks this environmental sensitivity, a dimension that could be vastly improved to enhance the planner's versatility.
Continued research and innovation are key to evolving these constructs, aiming to craft a planner that excels in interactivity and adeptly manages the complexities of diverse tasks.