# NLP for a Low-Resource Language: Jamaican Patois and English

**Analyzing NLP model performance on high-resource and low-resource language pairs**

Independent research project, Dartmouth College (2025). I tested whether NLP models can transfer across a linguistically related language pair: English (high-resource) and Jamaican Patois (low-resource, English-lexified Creole). The study covers three foundational tasks: part-of-speech (POS) tagging, dependency parsing, and constituency parsing.

## Motivation

Most NLP models are built for languages with large annotated datasets, which leaves speakers of low-resource languages poorly served by tools like voice assistants and autocorrect. Because Patois draws heavily on English vocabulary, it is a good test of whether a model's knowledge of one language helps with a closely related one.

## Data

I built the Patois datasets myself, since no annotated resources existed.

- **Sources:** dancehall and reggae lyrics (via Genius, Spotify, and YouTube) and the Jamaican Bible (New Testament).
- **Size:** about 200 sentences annotated for POS tagging and dependency parsing, plus a smaller 135-sentence constituency treebank (Penn Treebank style), since constituency annotation was the slowest to do by hand.
- **Processing:** removed repeated lines, tokenized, and manually annotated POS tags, dependencies, and constituency trees.
- **English comparison data:** the English Web Treebank.

## Methods

- **POS tagging and dependency parsing:** fine-tuned a [RoBERTa-based model](https://huggingface.co/KoichiYasuoka/xlm-roberta-base-english-upos) for token classification (17 POS labels, 37 dependency labels), with hyperparameter tuning.
- **Constituency parsing:** used the [SuPar](https://github.com/yzhangcs/parser) CRF parser with a RoBERTa encoder. I trained one model from scratch on Patois and fine-tuned one pretrained on English.
- **Cross-lingual testing:** each model was evaluated on its own language and on the other language, then compared.

## Results

Accuracy for POS tagging and dependency parsing:

| Model | Test data | POS | Dependency |
| --- | --- | --- | --- |
| Patois | Patois | 0.854 | 0.632 |
| Patois | English | 0.819 | 0.612 |
| English | English | 0.991 | 0.921 |
| English | Patois | 0.491 | 0.424 |

For constituency parsing (unlabeled F1 on the Patois test set), the English-pretrained model scored 60.8 and the Patois-only model scored 38.7. The standard English baseline on the Penn Treebank scores 94.8.

## Key Findings

- Models trained on Patois held up on English (POS accuracy 0.82), while English-trained models dropped sharply on Patois (0.99 to 0.49).
- In constituency parsing, the Patois-only model failed to generalize, labeling nearly every span as a noun phrase. Starting from the English-pretrained model helped considerably, supporting the idea that a related high-resource language offers a useful starting point.
- Even so, no model matched its English performance on Patois. I attribute this to creolization (for example, spelling that does not map cleanly onto English words, and grammatical forms like "dem" and "im" with no direct English match) and to limited generalization from small data (unseen words, loanwords, tokenization).

## Limitations and Future Work

The datasets are small and come from two genres (religious text and dancehall/reggae), which limits how representative they are. Future work includes building larger and more diverse annotated datasets, testing other high-resource and low-resource language pairs (such as Haitian Creole and French), and extending to other NLP tasks.

## Repository Contents

- [`Ling_Final_Project___A_McPherson.pdf`](./Ling_Final_Project___A_McPherson.pdf): paper on the constituency parsing portion
- [`Analyzing_NLP_model_performance_on_high-resource_and_low-resource_language_pairs.pdf`](./Analyzing_NLP_model_performance_on_high-resource_and_low-resource_language_pairs.pdf): presentation slides covering the POS tagging and dependency parsing portions

The code is not included yet. It is being cleaned up and may be added later.

## Author

Angelic McPherson ([@Angelic-Work](https://github.com/Angelic-Work))
