# DSPy-Qdrant

[Qdrant](https://qdrant.tech/) powered custom retriever module for [DSPy](https://dspy.ai).

## Installation

```bash
pip install dspy-qdrant
```

## Usage

```python
from dspy_qdrant import QdrantRM
```

The `QdrantRM` class enables semantic search by retrieving the top-k most relevant documents from a Qdrant collection using a sentence embedding model.

### Parameters

| Name                 | Type                      | Description                                                                                  | Default                     |
|----------------------|---------------------------|----------------------------------------------------------------------------------------------|-----------------------------|
| `qdrant_collection_name` | `str`                    | Name of the Qdrant collection used for retrieval.                                             | **Required**                |
| `qdrant_client`         | `QdrantClient`            | An initialized instance of `qdrant_client.QdrantClient`.                                      | **Required**                |
| `k`                    | `int`                     | Number of top documents to retrieve per query.                                                | `3`                         |
| `document_field`       | `str`                     | Field in the Qdrant payload that contains the raw document content.                           | `"document"`                |
| `vectorizer`           | `BaseSentenceVectorizer`  | Embedding model for vectorizing queries. Uses `FastEmbedVectorizer` if not provided.          | `None`                      |
| `vector_name`          | `str`                     | Name of the vector field in Qdrant collection to use for search. Defaults to the first found. | `None`                      |

### Use in a Module's `forward()` Function

```python
import dspy

from qdrant_client import QdrantClient
from dspy_qdrant import QdrantRM

qdrant_client = QdrantClient()

class MyModule(dspy.Module):
    def __init__(self, num_passages: int = 5):
        super().__init__()
        self.num_passages = num_passages

    def forward(self, question: str):
        retrieve = QdrantRM(
            qdrant_collection_name="my_collection_name",
            qdrant_client=qdrant_client,
            k=self.num_passages
        )
        # Do something with results...
```

## 📚 See Also

- [Qdrant Documentation](https://qdrant.tech/documentation/)
- [DSPy GitHub](https://github.com/stanfordnlp/dspy)
