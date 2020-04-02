---
title: pil example scaleai4 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'scaleai4'

Functions in program: 
* `def get_streamed_dataloaders(gcs_data_root: str, batch_size: int = 8, transform: Callable = None,`
* `def load_items_gcs(path: str) -> List[str]:`
* `def gcs_pil_loader(uri: str) -> ImageType:`

Modules used in program: 
* `import io`

## python scaleai4

Python pil example: scaleai4

```python
import io
from urllib.parse import urlparse

from PIL import Image
from google.cloud import storage
from google.api_core.retry import Retry


@Retry()
def gcs_pil_loader(uri: str) -> ImageType:
    uri = urlparse(uri)
    client = storage.Client()
    bucket = client.get_bucket(uri.netloc)
    b = bucket.blob(uri.path[1:], chunk_size=None)
    image = Image.open(io.BytesIO(b.download_as_string()))
    return image.convert("RGB")


@Retry()
def load_items_gcs(path: str) -> List[str]:
    uri = urlparse(path)
    client = storage.Client()
    bucket = client.get_bucket(uri.netloc)
    blobs = bucket.list_blobs(prefix=uri.path[1:], delimiter=None)
    return sorted(os.path.splitext(os.path.basename(blob.name))[0] for blob in blobs)


def get_streamed_dataloaders(gcs_data_root: str, batch_size: int = 8, transform: Callable = None,
                             test_ratio: float = 0.1, num_workers: int = 8) -> Dict[str, torch.utils.data.DataLoader]:
    # Streaming
    streamed_items = load_items_gcs(os.path.join(gcs_data_root, "images"))
    dataset = ImageDataset(gcs_data_root, streamed_items, loader=gcs_pil_loader, transform=transform)

    # Identical for both local and streamed
    # This is handy for CrossValidation, use consistent hashing
    train_indices, test_indices = consistent_train_test_split(streamed_items, test_ratio)
    return {
        "train": torch.utils.data.DataLoader(dataset, batch_size=batch_size,
                                             sampler=torch.utils.data.SubsetRandomSampler(train_indices),
                                             num_workers=num_workers),
        "test": torch.utils.data.DataLoader(dataset, batch_size=batch_size, 
                                            sampler=torch.utils.data.SubsetRandomSampler(test_indices),
                                            num_workers=num_workers)
    }

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
