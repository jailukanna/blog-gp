---
title: pil example scaleai5 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'scaleai5'

Functions in program: 
* `def get_async_dataloaders(gcs_data_root: str, batch_size: int = 8, transform: Callable = None,`
* `def generate_stream(items: List[str]) -> Generator[str, None, None]:`

Modules used in program: 
* `import aiohttp`
* `import asyncio`
* `import random`

## python scaleai5

Python pil example: scaleai5

```python
import random
import asyncio

import aiohttp
from janus import Queue
from gcloud.aio.storage import Storage


def generate_stream(items: List[str]) -> Generator[str, None, None]:
    while True:
        # Python's randint has inclusive upper bound
        index = random.randint(0, len(items) - 1)
        yield items[index]


class AsyncImageDataset(torch.utils.data.IterableDataset):
    def __init__(self, data_root: str, items: List[str], transform: Callable = None, concurrency: int = 64):
        self.data_root = data_root
        self.items = items
        self.transform = transform
        self.worker_initialized = False
        self.loop_thread = None
        self.q = None
        self.creds = os.environ["GOOGLE_APPLICATION_CREDENTIALS"]
        self.concurrency = concurrency
        self.stream = generate_stream(self.items)

    async def run(self, loop, session):
        for item in self.stream:
            try:
                image_gs = urlparse(os.path.join(self.data_root, "images", item + ".jpg"))
                label_gs = urlparse(os.path.join(self.data_root, "labels", item + ".png"))
                aio_storage = Storage(service_file=self.creds, session=session)
                blobs = await asyncio.gather(
                    aio_storage.download(image_gs.netloc, image_gs.path[1:]),
                    aio_storage.download(label_gs.netloc, label_gs.path[1:]),
                    loop=loop
                )
                image = Image.open(io.BytesIO(blobs[0]))
                label = Image.open(io.BytesIO(blobs[1])).convert("RGB")
                await self.q.async_q.put((image, label))
            except aiohttp.ClientError as e:
                logging.debug(e)
            except TimeoutError:
                pass
            except Exception as e:
                logging.exception(e)

    def init_worker(self):
        loop = asyncio.new_event_loop()
        session = aiohttp.ClientSession(loop=loop, connector=aiohttp.TCPConnector(limit=0, loop=loop),
                                        raise_for_status=True)
        self.q = Queue(self.concurrency, loop=loop)

        # Spin up workers
        for _ in range(self.concurrency):
            loop.create_task(self.run(loop, session))

        def loop_in_thread(loop):
            asyncio.set_event_loop(loop)
            loop.run_forever()

        self.loop_thread = Thread(target=loop_in_thread, args=(loop,), daemon=True)
        self.loop_thread.start()
        self.worker_initialized = True

    def __iter__(self):
        while True:
            if not self.worker_initialized:
                self.init_worker()

            image, label = self.q.sync_q.get()
            if self.transform is not None:
                image, label = self.transform((image, label))

            yield image, label


def get_async_dataloaders(gcs_data_root: str, batch_size: int = 8, transform: Callable = None,
                          test_ratio: float = 0.1, num_workers: int = 8) -> Dict[str, torch.utils.data.DataLoader]:
    # Async Streaming
    streamed_items = load_items_gcs(os.path.join(gcs_data_root, "images"))
    train_indices, test_indices = consistent_train_test_split(streamed_items, test_ratio)
    train_items = [streamed_items[i.item()] for i in train_indices]
    train_dataset = AsyncImageDataset(gcs_data_root, train_items, transform=transform, concurrency=128)
    test_items = [streamed_items[i.item()] for i in test_indices]
    test_dataset = AsyncImageDataset(gcs_data_root, test_items, transform=transform, concurrency=128)

    return {
        "train": torch.utils.data.DataLoader(train_dataset, batch_size=batch_size, worker_init_fn=worker_init_fn,
                                num_workers=num_workers),
        "test": torch.utils.data.DataLoader(test_dataset, batch_size=batch_size, worker_init_fn=worker_init_fn,
                               num_workers=num_workers)
    }

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
