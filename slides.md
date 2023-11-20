---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: images/green-ai.png
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
# page transition
transition: fade-out
# use UnoCSS
css: unocss
download: https://louis-sanna-eki.github.io/gen-ai-retex-slides/export/slides-export.pdf
favicon: images/eki-favicon.png
---

# Exploring Generative AI: Four Innovative Projects

A journey through diverse applications of generative AI, showcasing innovation and versatility.

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: image-right
image: /images/table-content.png  # Replace with the actual path to the image
---

# Table of Contents

1. **UNESCO AI Chatbot**: Exploring AI-driven communication.
2. **Connect the Dots**: Integration of AI in team collaboration.
3. **AnythingQ&A**: Versatile AI-powered question-answering.
4. **TarotGPT**: Custom AI application in fortune telling.

Explore the innovative applications of generative AI across various domains.

---
layout: two-cols
---

# Project 1: UNESCO AI Chatbot

- **Description**: A Gradio chatbot powered by RAG, deployed on Microsoft Azure.
- **Key Insights**:
  - Code quality is crucial for reliability.
  - Flexibility to adapt to client needs (like unplanned demos).
  - Custom UI, using UNESCO Design System, enhances user engagement.
- **Demo Link**: [UNESCO AI Chatbot](https://unescoai-dev-webapp-01.azurewebsites.net/)

::right::

<img src="/images/unesco.png" class="h-80 rounded shadow" />

---
layout: two-cols
---

# Project 2: Connect the Dots

- **Description**: Teams app featuring a chatbot enriched with SQL and web search capabilities. 
- **Highlights**:
  - Utilizes GPT for classification and Named Entity Recognition (NER).
  - Demonstrates the efficacy of instruct models in NER tasks.

::right::

<img src="/images/ctd.png" class="h-80 rounded shadow" />

---
---

# Classifier with GPT-3.5 Turbo Instruct

```python
def classify(features, labels):
    print("features", features)
    prompt = (
        f"This is a classification task.\\n\\n"
        f"The possible labels are {labels}.\\n\\n"
        f"The features are {features}.\\n\\n"
        "Your answer MUST be of the form (one line per feature): `FEATURE=>LABEL\\n`"
    )

    completion_str = invoke_llm(prompt)
    completion_pairs = [line.split("=>") for line in completion_str.strip().split("\\n")]
    valid_pairs = [
        (feature, label)
        for feature, label in completion_pairs
        if len(feature) > 0 and len(label) > 0
    ]

    corrected_pairs = filter_and_correct_labels(valid_pairs, labels, labels[0])

    return dict(corrected_pairs)
```


---
layout: two-cols
---

# Project 3: AnythingQ&A

- **Description**: Fork of climateQ&A, adapted for broader question-answering.
- **Features**:
  - Chroma for efficient in-memory indexing.
  - LangChain enabling easy swapping of AI providers.
- **Demo Link**: [AnythingQ&A](https://huggingface.co/spaces/LouisSanna/anything-question-answering)

::right::

<img src="/images/anyqa.png" class="h-80 rounded shadow" />

---
layout: two-cols
---

# Project 4: TarotGPT

- **Description**: Custom GPT model for insightful tarot readings.
- **Implementation**:
  - Actions integrated via HTTP endpoints using OpenAPI.
  - FastAPI for streamlined process and deployment on Modal.
- **Demo Link**: [TarotGPT](https://chat.openai.com/g/g-EoTBgBWwS)

::right::

<img src="/images/tarotgpt.png" class="h-80 rounded shadow ml-16" />

---
layout: center
---

<img src="/images/dog.png" with="100%" height="100%" position="absolute" top="0" left="0"/>
<div position="relative" color="#15293B" top="-200px">
</div>

