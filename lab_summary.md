In this lab, I built and tested version 1 prompts for sentiment analysis, product description generation, and data extraction, then iteratively improved them into version 3 prompts using clearer task instructions, stronger output structure, few-shot examples, and Chain-of-Thought style reasoning where it helped most. The final sentiment v3 prompt asked the model to classify `{input_text}` into only Positive, Neutral, or Negative, which improved consistency most when I tightened the label-only output and removed extra explanation; the final product description v3 prompt used a fixed format with Title, Description, Features, and Price plus a few-shot example, and this combination worked best because the structured template greatly reduced variation and kept the tone stable across different product inputs; the final data extraction v3 prompt used explicit field names and careful step-by-step reasoning, which helped accuracy and made the outputs easier to compare, especially for edge cases where earlier versions drifted or added commentary. Across the process, the biggest failures were inconsistent formatting, extra text outside the required output, prompt anchoring to examples, and weaker generalization on task variations, while the techniques that helped most were clear delimiters for `{input_text}`, fixed output schemas, few-shot examples for style control, and concise reasoning instructions for extraction tasks. If I repeated this lab, I would make every prompt fully parameterized from the start, separate test inputs from prompt templates more cleanly, and use a consistent evaluation table from the beginning so comparisons between versions are easier and more reliable.
Final version prompts:
sentiment_prompt_v3 = """
You are a customer support analyst.
Classify the sentiment of the customer message below.
Return only one label:
Positive, Neutral, or Negative.
The format should be: Sentiment: [Positive/Negative/Neutral]
2 words maximum for the output.
Follow the examples below for formatting and style:
Example 1:
Sentiment: Positive
Example 2:
Sentiment: Negative
Example 3:
Sentiment: Neutral
Customer message:
{input_text}
"""
product_prompt_v3 = """
You are a professional copywriter.
Write a product description in this exact format:
Title: ...
Description: ...
Features (only 3, in bullet points):
- ...
- ...
- ...
Price: ...
The style should be sober and persuasive, targeting tech-savvy consumers looking for affordable accessories.
The output should be no more than 40 words.
Product request:
{input_text}
"""
extraction_prompt_v3 = """
You are a precise information extraction assistant.
Extract the requested fields from the text below.
Analyze the input text step by step to identify all relevant facts, but only return the final extracted data in the exact format below.
- Order Number: [Extracted order number]
- Order Date: [Extracted order date]
- Delivery Feedback: [Extracted delivery feedback]
- Packaging Feedback: [Extracted packaging feedback]
All responses should start with a capital letter and be 2 words maximum.
There should be no additional text or labels, just the extracted information formatted as described.
Text:
{input_text}
"""