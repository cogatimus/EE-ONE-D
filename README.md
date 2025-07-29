# Lexical, Fuzzy, and Semantic Search (LFSS)

This project implements a comprehensive search pipeline that combines lexical, fuzzy, and semantic search techniques to provide robust and intelligent search capabilities. It leverages pre-trained language models, edit distance algorithms, and Milvus for efficient vector similarity search.

## Project Structure

The project is organized into several modules, each responsible for a specific aspect of the search functionality:

- `attention.py`: Implements `AttentionModel` for extracting important tokens from a query using BERT.

- `base.py`: Defines the abstract base class `SearchClass` for all search components.

- `fuzzy_search.py`: Provides `FuzzySearch` for finding approximate string matches using Levenshtein distance.

- `lexical_fuzzy_semantic_search.py`: The main entry point, `LFSS`, orchestrating the search pipeline.

- `logging.conf`: Configuration file for logging.

- `models.py`: Handles interaction with the Milvus vector database for semantic search.

- `pipeline.py`: Implements a generic `SearchPipeline` to chain different search functionalities.

- `semantic_search.py`: Implements `SemanticSearch` for finding semantically similar text using embeddings.

- `typographical_search.py`: Implements `TypographicalNeighbors` for finding words with small edit distances.

- `nuke_db.py`: A utility script to drop all collections in the Milvus database.

For more detailed explanation check the `/docs` folder for module wise information.

## Features

- **Attention-based Token Importance**: Identify key terms in your query using the `AttentionModel`.

- **Fuzzy Matching**: Find relevant results even with typos or variations using `FuzzySearch`.

- **Semantic Search**: Discover semantically similar content, understanding the meaning behind words, powered by `SemanticSearch` and Milvus.

- **Typographical Neighbors**: Explore words with minor spelling differences using `TypographicalNeighbors`.

- **Configurable Pipeline**: Easily combine and reorder different search modules using the `SearchPipeline`.

- **Logging**: Comprehensive logging for debugging and monitoring.

## Setup and Installation

### Prerequisites

- Python 3.8+

- `pip` or `Poetry`

- Milvus (vector database) - [Installation Guide](https://milvus.io/docs/install_milvus.md)

### Installation Steps

1. **Clone the repository:**

   ```bash
   git clone https://github.com/cogatimus/EE-ONE-D
   cd EE-ONE-D
   ```

2. **Create a virtual environment (recommended):**

   **Using pip:**

   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: `venv\Scripts\activate`
   ```

   **Using Poetry:**

   ```bash
   poetry install
   ```

3. **Install dependencies:**

   **Using pip:**

   ```bash
   pip install -r requirements.txt
   ```

   **Using Poetry:**
   Poetry will manage dependencies automatically upon `poetry install`.

   (Note: You might need to create a `requirements.txt` file based `pyproject.toml` if using pip. For Poetry, ensure you have a `pyproject.toml` and `poetry.lock` file.)

4. **Download NLTK data:**

   ```python
   import nltk
   nltk.download('punkt')
   nltk.download('stopwords')
   nltk.download('wordnet')
   nltk.download('words')
   ```

5. **Start Milvus:**
   Follow the Milvus installation guide to start your Milvus instance. By default, the `models.py` expects Milvus to be running on `localhost:19530`. We used docker to easily install milvus locally.

## Usage

### Example: Lexical, Fuzzy, and Semantic Search (LFSS)

The `LFSS` class in `lexical_fuzzy_semantic_search.py` demonstrates how to combine the different search functionalities.

```python
from lexical_fuzzy_semantic_search import LFSS

# Example document
document = [
    "The loyal dog waited patiently by the door.",
    "She enjoyed taking her furry companion for long walks.",
    "The puppy's playful antics brought joy to the family.",
    "He trained his canine friend to do impressive tricks.",
    "The barking of the neighbor's dog could be heard in the distance.",
    "The sun was setting over the horizon, painting the sky in hues of orange and pink.",
    "She sipped her coffee slowly, savoring the rich aroma.",
    "The students eagerly listened to the professor's lecture on quantum mechanics.",
    "The scent of fresh flowers wafted through the open window.",
    "The city bustled with activity as people hurried to their destinations.",
    "The old oak tree stood majestic against the backdrop of the clear blue sky.",
    "The soothing sound of rain pattering on the roof lulled her to sleep.",
    "The aroma of freshly baked bread filled the quaint bakery.",
    "The children laughed and played in the park, their voices ringing with joy.",
    "The bookshelf was filled with a collection of novels spanning various genres.",
]

# Example query
input_string = "dog"

# Initialize LFSS with desired search components and arguments
# init_arg_dict: Initialization arguments for each class in the pipeline
# call_arg_dict: Call arguments for each class in the pipeline
lfss = LFSS(
    init_arg_dict=[{}, {}, {}], # TypographicalNeighbors, SemanticSearch, FuzzySearch
    call_arg_dict=[{}, {"limit": 3}, {"limit": 3}], # Arguments for __call__ method
    query=input_string,
    document=document,
    use_attention=False, # Set to True to incorporate attention model for query processing
)

# Run the search pipeline
results = lfss()

# Print results
for result in results:
    print(f"\nIn sentence: '{result['sentence']}'")
    print(f"\nFound: '{result['word']}'(Distance: {result['distance']})")
    print(f"\nContext: '{result['context']}'")
```

### Running individual modules

You can also run individual modules by executing their respective `if __name__ == "__main__":` blocks. Refer to the `Usage` section within each Python file for specific examples.

### Nuke Database

To clear all collections in your Milvus database (useful for development and testing):

```bash
python nuke_db.py <database_name>
```

Replace `<database_name>` with the actual name of your Milvus database (e.g., `eeoned`).

## Project setup for pip users

```bash
pip install -r requirements.txt
```

## Poetry Project Setup (for Poetry users)

For a simpler set up you can use poetry to install all the dependencies.

## Logging

The project uses a `logging.conf` file for configuring logging. By default, logs are printed to the console. You can modify `logging.conf` to change logging levels, add file handlers, etc.

## Contributing

Contributions are welcome! Please feel free to open issues or submit pull requests.

## License

[GPL-V3]
