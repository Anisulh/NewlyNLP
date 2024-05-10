# Newsly NLP Service

![Python](https://img.shields.io/badge/python-3.11-blue.svg)
![HuggingFace](https://img.shields.io/badge/HuggingFace-Transformers-red.svg)
![Kaggle](https://img.shields.io/badge/Data-Kaggle-20BEFF.svg)
![Build](https://img.shields.io/badge/build-passing-brightgreen.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)

The Newsly NLP Service is designed to categorize and tag news articles, enhancing personalized news recommendations. Leveraging a fine-tuned DistilBERT model, this service processes articles to categorize them under several distinct topics such as Science, Technology, and Politics, inspired by TikTok's recommendation algorithm.

## Features

- **Custom Fine-Tuned BERT**: Utilizes a version of DistilBERT adapted to accurately categorize texts into specific news-related categories.
- **Data-Driven**: Trained and fine-tuned on diverse datasets including BBC, CNN, AG News, and more, ensuring robustness and accuracy.
- **Efficient Preprocessing**: Integrates BART for summarizing texts, accommodating the token limits of DistilBERT without losing crucial information.

## Model Evolution

- `v1`: Initial model using AG News Dataset.
- `v2`: Enhanced with Text Classification Dataset.
- `v3`: Expanded with BBC News Dataset.
- `v4`: Refined with CNN Dataset, representing the final and most sophisticated version.

## Setup and Installation

Ensure you have Python 3.8 or later installed, then run:

```bash
pip install -r requirements.txt
```

## Usage

To train and fine-tune the model, execute the following scripts in order:

```bash
python src/model_base_training.py       # Train initial model on AG News
python src/model_v1_training.py         # Train v1 on Text Classification Dataset
python src/model_v2_training.py         # Train v2 on BBC Dataset
```

## Data Description

- **AG News Dataset (HuggingFace)**: Used for initial training, categorizing news into four classes.
- **Text Document Classification Dataset ([Kaggle](https://www.kaggle.com/datasets/sunilthite/text-document-classification-dataset))**: Employed for fine-tuning with additional classes.

## File Structure

```plaintext
.
├── data
│   ├── ag_news.csv                # Initial dataset
│   └── custom_dataset.csv         # Dataset for fine-tuning
├── models
│   ├── saved_model                # Initial model
│   └── saved_model_fine-tuned     # Fine-tuned model
├── src
│   ├── preprocessing
│   │    ├── bbc_preprocessing.py  # BBC data preprocessing
│   │    └── text_classification_preprocessing.py
│   ├── model_base_training.py     # Base model training script
│   ├── model_v1_training.py       # v1 training script
│   └── model_v2_training.py       # v2 training script
```

## Discussion

Due to distilBERT input size being 512 tokens, all texts (except for AG News Dataset) were summarized using BART to reduce the total text size without truncating the texts too much. However truncating was still required as the BART model had a limit of 1024 token input size.

Below are the categories in each dataset and what they map to. Then a global map is implemented
```bash
    # AG News Dataset Classes

    World = 0
    Sports = 1
    Business = 2
    Science/Technology = 3

    # Text Document Classification Dataset Classes
    
    Politics = 0
    Sport = 1
    Technology = 2
    Entertainment = 3
    Business = 4

    # Relabelled Text Document Classification Dataset Classes
    Sports = 1    # common
    Business = 2  # common
    Politics = 4
    Technology = 5
    Entertainment = 6

    # BBC Dataset Classes
    Sports = "sport"    
    Business = "business" 
    Politics = "politics"
    Technology = "tech"
    Entertainment = "entertainment"


    # Relabelled BBC Dataset Classes
    Sports = 1    # common
    Business = 2  # common
    Politics = 4  # common
    Technology = 5  # common
    Entertainment = 6 # common


    # New Dataset Classes
    World = 0
    Sports = 1
    Business = 2
    Science = 3  # Science/Technology renamed to science since technology is a separate class
    Politics = 4 
    Technology = 5 
    Entertainment = 6 

```
As such the model is expected to differentiate science from technology and politics from world.

The model was further trained on the BBC News Dataset, which had a similar classes to the Text Classification Dataset, to improve it classification abilities on larger texts.
`{'train_runtime': 758.0574, 'train_samples_per_second': 11.741, 'train_steps_per_second': 0.739, 'train_loss': 0.10026595762797764, 'epoch': 5.0}` 

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
