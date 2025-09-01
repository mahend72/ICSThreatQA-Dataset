# ICSThreatQA-Dataset

## Overview

The ICSThreatQA dataset is a meticulously crafted collection of 620 question-answer (QA) pairs designed to assist security teams in understanding and addressing cybersecurity threats in Industrial Control Systems (ICS). These questions are categorized into four distinct types: _factual, contrastive, inferential, and opinion-based_. The dataset was created through a human-in-the-loop approach, ensuring that the QA pairs are both diverse and contextually rich. Each question is carefully constructed to reflect ICS security's complexity and align with the needs of security teams working to defend against cyber threats targeting critical infrastructure.

The QA pairs in the dataset were developed by defining question intents based on a comprehensive threat knowledge base. This knowledge base includes entities, attributes, and their relationships within the ICS domain, making the dataset highly relevant and practical for real-world security teams. The dataset aims to provide a well-rounded resource for various analytical approaches, including fact-checking, comparisons, drawing inferences, and seeking expert opinions by focusing on diverse question categories.

##  Questions type

The 620 QA pairs in the ICSThreatQA dataset are divided into four major question categories, each serving a different purpose in terms of information retrieval, threat analysis, and decision-making.

- _Factual Questions_: These questions focus on seeking clear, specific, and verifiable information about a particular topic or entity within ICS security. The goal is to provide concise facts about the functionality or attributes of security-related entities.

  _Example (Factual Query)_: What is the primary function of the Stuxnet malware in ICS environments?

- _Contrastive Questions_: These questions aim to compare and contrast two or more entities, focusing on their differences and similarities. These types of questions are designed to help security professionals understand how different malware, attacks, or security measures compare in terms of their impact, behavior, and characteristics.

  _Example (Contrastive Query)_: How does the Triton malware differ from the Industroyer malware in their impact on ICS?

- _Inferential Questions_: These questions require the respondent to make inferences based on available information or data. They are more analytical, prompting the security team to think critically about potential threats, preventive measures, or mitigation strategies.

  _Example (Inferential Query)_: Given the increase in ICS-targeted attacks, what preventive measures can be implemented to protect against unauthorized command injection attacks?

- _Opinion-Based Questions_: These questions seek subjective responses based on expert knowledge or personal judgment. They are intended to gauge the perspectives or evaluations of security professionals regarding specific security practices, policies, or tools.

  _Example (Opinion-Based Query)_: Do you think the implementation of multi-factor authentication (MFA) is sufficient to secure remote access in ICS environments?


## Evaluation of the Dataset

The evaluation of the ICSThreatQA dataset is designed to ensure the quality, diversity, and relevance of the generated QA pairs. The process includes the following key evaluation criteria:

- _Relevance_: The dataset must ensure that the questions and their corresponding answers are directly aligned with the needs of ICS security teams. This ensures the questions are meaningful, contextually appropriate, and can be used effectively in real-world security contexts.

- _Diversity_: Given the varying nature of cybersecurity threats in ICS environments, the dataset includes a broad spectrum of questions that cover different topics, question types, and problem-solving approaches. This diversity ensures that the dataset is useful for a wide range of scenarios, from basic information retrieval to complex threat analysis.

- _Accuracy_: The dataset relies on expert knowledge and a thorough understanding of ICS security, ensuring that all answers are factually accurate and up-to-date with current cybersecurity standards and best practices. This is especially important for factual and contrastive queries.

- _Human-in-the-Loop Validation_: To improve the diversity and relevance of the questions, the dataset creation process employed a human-in-the-loop approach. This ensured that the dataset contained unique and creative questions that go beyond simple question classification and capture the complexities of real-world ICS security concerns.

- _Usability_: The dataset is designed to be a practical resource for security teams, offering insights into various threats, security measures, and attack scenarios. Each question type is crafted to be useful for different stages of threat analysis, from gathering factual data to forming expert opinions.

Overall, the ICSThreatQA dataset is a robust tool for enhancing the capabilities of cybersecurity professionals working in ICS environments. The combination of factual, contrastive, inferential, and opinion-based questions ensures that it provides a comprehensive resource for tackling a wide range of security challenges.


cite this paper 

@article{rani2025icsthreatqa,
  title={ICSThreatQA: A Knowledge-Graph Enhanced Question Answering Model for Industrial Control System Threat Intelligence},
  author={Rani, Ruby and Kumar, Mahender and Epiphaniou, Gregory and Maple, Carsten},
  journal={Expert Systems with Applications},
  pages={130180},
  year={2025},
  publisher={Elsevier},
}