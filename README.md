# Empathetic AI Agent – Breast Cancer Support

## Solution Summary

### Part 1 - Structured Interaction

A structured conversational flow was implemented using a predefined set of five questions:
- First name
- Age
- Diagnosis
- Treatment
- Support system

An internal evaluation layer (Judge Layer), based on prompt engineering with GPT-4o, was used to assess the clarity of the user's responses.

The Judge Layer handles vague, partial, or emotional responses and returns either:
- `CLEAR|[answer]` – for a valid, structured answer
- `FOLLOWUP|[empathetic rephrased question]` – for unclear or incomplete answers
- 
If the user explicitly refuses or avoids answering more than twice, the system acknowledges the difficulty and moves on to the next question. The skipped question is revisited later in the flow with gentle and empathetic phrasing.

### Part 2 - Dynamic Personalized Interaction

The user is asked to describe their condition and treatment plan in free text.  
GPT-4o is used with one-shot prompt engineering to extract structured information without assuming a fixed schema.

Based on this extracted profile and the ongoing chat history, the system generates five dynamic follow-up questions using GPT-4o.  
Each question:
- Is emotionally sensitive
- Avoids repetition
- Focuses on relevant aspects of the user's journey

The answers are stored in the profile using keys like `dynamic_0_emotion_support`, etc.  
The Judge Layer continues to operate during this stage.

### Architectural Considerations

LangChain’s agent abstraction was initially considered, but was not used due to:
- Unnecessary complexity
- Reduced control over the prompt flow
- Increased development and runtime cost

Instead, the solution is implemented using:
- Direct calls to GPT-4o
- A modular Python structure
- LangChain's `ConversationBufferMemory` for maintaining context

This approach offered a simpler, more stable, and efficient solution suitable for the scope of the assignment.

### Future Improvements and Production Considerations

* **Model Fine-Tuning**  
  Fine-tuning GPT-4o on a dedicated dataset of annotated patient conversations to enhance empathy and specificity.

* **Agent Chaining Architecture**  
  Breaking the system into smaller dedicated agents (e.g., conversation manager, evaluator, extractor, generator) to improve modularity and scalability.

* **Retrieval-Augmented Generation (RAG)**  
  Connecting to a verified medical knowledge base to enrich answers with accurate clinical information.

* **Monitoring and Control Layer**  
  Adding response logging, quality control, and issue tracking to support production deployment and reliability.
  
* **Dynamic Acknowledgment Responses**
  Replacing the static "Thank you for sharing." with varied, context-aware acknowledgments based on the content or emotional tone of the user's answer.
  This would improve conversational flow and make interactions feel more natural and emotionally attuned.


## Technologies Used

- Python 3
- OpenAI GPT-4o (`openai` SDK)
- LangChain (`ConversationBufferMemory`)
- Google Colab (for execution and testing)

## Output Example

A final structured user profile is printed to the console. Sample output:
Name: Sarah
Age: 45
Diagnosis: Triple-Negative breast cancer
Stage: IIb
Treatment_started: AC-T chemotherapy
Upcoming_tests: Genetic testing and breast MRI
How are you feeling about starting your ac-t chemotherapy and the upcoming genetic testing and breast mri?: Terrified
As you prepare for the upcoming genetic testing and breast mri, is there anything specific you’re hoping to learn or understand better about your diagnosis?: No
What are some ways you find comfort or strength when you're feeling overwhelmed during this journey?: Sport helps them find comfort and strength.
How do you feel sport has helped you emotionally during this challenging time?: It clears my mind.
