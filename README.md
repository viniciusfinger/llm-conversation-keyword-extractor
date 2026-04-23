# LLM Conversation Keyword Extractor

Simple keyword extraction from conversation JSON files using spaCy.

## Overview

This project extracts the most frequent keywords from chat-style conversations.
It reads messages from a JSON file, filters messages by `role`, cleans the text, and returns the top keywords with frequencies.

Main features:

- Role-based filtering (`user`, `assistant`, `tool`, `system`)
- Optional lemmatization
- Stopword and punctuation removal
- Simple output: keyword + frequency

## Input Format

Your input file must follow this structure:

```json
{
  "messages": [
    { "role": "user", "content": "Hello, My name is Vinicius Finger." },
    { "role": "assistant", "content": "Hello, Vinicius!" }
  ]
}
```

Put your JSON file inside the `data/` folder.

Reference for the `messages` structure (`role` + `content`) used in chat conversations:
- [OpenAI Chat Completions - Messages](https://platform.openai.com/docs/guides/text-generation/chat-completions-api)

## Setup

Requirements:

- Python 3.12+

Install dependencies (using `uv`):

```bash
uv sync
```

If you want to use another language:

1. Replace the spaCy model dependency in `pyproject.toml` (for example, change `en_core_web_sm` to your target language model).
2. Update the model loaded in `keyword-extractor.ipynb`:

```python
nlp = spacy.load("en_core_web_sm", disable=["ner"])
```

Change `"en_core_web_sm"` to the desired model name.

## How to Use

1. Add your conversation JSON file to `data/`.
2. Open `keyword-extractor.ipynb`.
3. In the usage cell, change the file path from:
   - `"data/sample.json"`
   - to your file, for example: `"data/my-conversation.json"`
4. Run all notebook cells.

Current usage example in the notebook:

```python
keywords = extract_top_keywords(
    "data/sample.json",
    roles=[Role.USER, Role.ASSISTANT],
    lemmatize=False
)
```

## Role Filter

Use the `roles` argument to control which messages are included in the analysis.

Available roles:

- `Role.USER`
- `Role.ASSISTANT`
- `Role.TOOL`
- `Role.SYSTEM`

Example:

- `roles=[Role.USER]` -> analyze only user messages
- `roles=[Role.USER, Role.ASSISTANT]` -> analyze user + assistant messages

## Lemmatization Option

The `lemmatize` argument enables or disables lemmatization:

- `lemmatize=False`: keeps tokens as they appear
- `lemmatize=True`: converts words to their base form (lemma)

### What is lemmatization?

Lemmatization reduces inflected words to a common base form.
Examples:

- `running`, `ran` -> `run`
- `cars` -> `car`

Why it helps:

- Groups word variations together
- Produces cleaner keyword counts
- Reduces duplicated meanings across forms

## Output

The result is a list of dictionaries:

```json
[
  { "keyword": "nlp", "frequency": 14 },
  { "keyword": "keyword", "frequency": 48 }
]
```

You can control output size with `top_n` (default: `20`).
