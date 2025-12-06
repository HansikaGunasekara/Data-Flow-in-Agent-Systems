# Data Flow in Agent Workflows
Here you can learn more about how real-world agentic AI systems pass data between components. This will be a collection of learning materials for the most important tools used to build LLM powered applications where every step is structured, validated and ready to plug into any workflow.

In general LLMs produce free form text. When building a LLM into a larger software system, the output of one LLM
would become an input to another LLM or a function. It is important to get the output of LLM in a structured way.

Example:
![Customer Support Application](https://github.com/HansikaGunasekara/Data-Flow-in-Agent-Systems/blob/main/customer_support_application.jpg)

Imagine you have a system where the user fills out a form with **Name**,**Email** & **How can we help you?** fields. You can then construct the following prompt asking the LLM to analyze the user query.

```python
prompt = """
Analyze this user query: {user_query} and provide a response in properly formatted JSON in the format of this example:
      {
      "name": "Example User",
      "email": "user@example.com",
      "query": "I ordered a new computer monitor and it arrived with the screen cracked. I need to exchange it        for a new one",
      "priority": "high",
      "category": "refund_request",
      "is_complaint": true,
      "tags": ["monitor", "support", "exchange"]
      }
"""
```

When you pass that prompt into an LLM you get a response like follows:
```JSON
{
  "name": "Joe User",
  "email": "joe.user@example.com",
  "query": "I'm really dissapointed! I ordered a remote control plane kit but it arrived missing key parts. I   need to send it back.",
  "priority": "high",
  "category": "refund_request",
  "is_complaint": true,
  "tags": ["return", "order_issue"]

}
```

This JSON structure is similar to a python dictionary. With this structured data the system can automatically create a support ticket or call a function tool. But this strucured response is not always perfect.

- It might contain additionaly text in the response
```JSON
Here's the JSON output you requested!

{
  "name": "Joe User",
  "email": "joe.user@example.com",
  "query": "I'm really dissapointed! I ordered a remote control plane kit but it arrived missing key parts. I   need to send it back.",
  "priority": "high",
  "category": "refund_request",
  "is_complaint": true,
  "tags": ["return", "order_issue"]

}
```
- It might add some other formatting like the tripple backtick markdown formatting
```json
Here's the JSON output you requested!
```json
{
  "name": "Joe User",
  "email": "joe.user@example.com",
  "query": "I'm really dissapointed! I ordered a remote control plane kit but it arrived missing key parts. I   need to send it back.",
  "priority": "high",
  "category": "refund_request",
  "is_complaint": true,
  "tags": ["return", "order_issue"]

}```
```
- It might not give you all the fields requested
- It might give certain fields in a wrong format

This unpredictability in the LLM response format is not reliable to get a stuructured output from a LLM. With Pydantic we can ensure an LLM to give the data in an expected format.

The Pydantic data model supports LLMs to have structured outputs and function calls.

![Customer Support Application](https://github.com/HansikaGunasekara/Data-Flow-in-Agent-Systems/blob/main/LLM_Data_Validation_framework.jpg)

