---
title: PIL example neurodataWeek1 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'neurodataWeek1'

Functions in program: 
* `def tensorsToImage(tensor, denormalize=True):`

Modules used in program: 
* `import aiohttp`
* `import asyncio`
* `import asyncio`
* `import concurrent.futures`
* `import tqdm`
* `import json`
* `import requests`
* `import time`
* `import os`
* `import random`
* `import pickle`
* `import io`
* `import os`
* `import PIL`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import torchvision.transforms as transforms`
* `import torchvision`
* `import torch.optim as optim`
* `import torch.nn.functional as F`
* `import torch.nn as nn`
* `import torch`

## python neurodataWeek1

Python PIL example: neurodataWeek1

```python
import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
from torch.utils.data import Dataset, DataLoader
import torchvision
import torchvision.transforms as transforms
from PIL import Image
from IPython.display import display
import numpy as np
import matplotlib.pyplot as plt
from PIL import features as PILFeatures
print("Using libjpeg_turbo? : ", PILFeatures.check_feature('libjpeg_turbo')) # speeds up
import PIL
print(PIL.__version__) # PIL-SIMD for max speed (pip install)

import os
import io
import pickle
import random
import os
import time
import requests
import json
import tqdm

from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor
import concurrent.futures
import asyncio
from google.cloud import storage
from gcloud.aio.storage import Storage
import asyncio
import aiohttp


device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
%matplotlib inline
plt.style.use("ggplot")
%config InlineBackend.figure_format = 'svg'

# Next time, should make live scraper->downloader->augmenter pipeline!

class GoogleStorageMassDownloader:

    # Arguments:
    #   filenamesBlob: a "gs://..." path to a blob, that when unpickled, is a list of paths relative to the specified bucket

    def __init__(self, downloadDirectory, bucketName, allBlobPathsBlob, numProcesses=16, maxConnPerProcess=64, convertImageType=None):
        self.maxConnPerProcess = maxConnPerProcess
        self.downloadDirectory = downloadDirectory
        self.numProcesses = numProcesses
        self.convertImageType = convertImageType
        self.bucketName = bucketName
        storageClient = storage.Client()
        file = io.BytesIO()
        storageClient.download_blob_to_file(allBlobPathsBlob, file)
        file.seek(0)
        self.allBlobPaths = pickle.load(file)

    async def downloadItem(self, name, storageSession):
        filename = name.replace("/", "-")
        fullPath = os.path.join(self.downloadDirectory, filename)
        if not os.path.exists(fullPath):
            try:
                item = await storageSession.download(self.bucketName, name, timeout=60*5)
            except:
                return await self.downloadItem(name, storageSession)
            with open(fullPath, "wb") as f: #slower if use aiofiles
                f.write(item)
        return filename
    async def downloadAllItems(self, storageSession, names, httpSession):
        filenames = await asyncio.gather(*[self.downloadItem(name, storageSession) for name in names], return_exceptions=True)
        await httpSession.close()
        return filenames
    def runDownloadLoop(self, names):
        connector = aiohttp.TCPConnector(limit=self.maxConnPerProcess)
        httpSession = aiohttp.ClientSession(connector=connector)
        storageSession = Storage(session=httpSession)
        filenames = self.downloadAllItems(storageSession, names, httpSession)
        return filenames
    def download(self, names):
        loop = asyncio.new_event_loop()
        asyncio.set_event_loop(loop)
        random.shuffle(names)
        filenames = loop.run_until_complete(self.runDownloadLoop(names))
        return filenames

    def run(self):
        executor = ProcessPoolExecutor(self.numProcesses)
        numSplits = self.numProcesses
        nameSplits = [l.tolist() for l in np.array_split(self.allBlobPaths, numSplits)]

        print("Starting Download")
        try:
            filenameLists = list(executor.map(self.download, nameSplits))
        except Exception as e:
            executor.shutdown()
            print("Failed Download")
            raise e
        print("Finished Download")
        executor.shutdown()

        filenames = []
        for l in filenameLists:
            filenames += l
        return filenames

class ImageNet1000(Dataset):

    def __init__(self, directory, filenamesIn, classLabel=None):
        super().__init__()

        # global filenames # so that forked processes get this w/o issue
        # filenames = filenamesIn
        self.classLabel = classLabel
        self.directory = directory
        classMapURL = "http://files.fast.ai/models/imagenet_class_index.json"
        self.classTable = requests.get(classMapURL).content.decode("utf-8")
        self.classTable = json.loads(self.classTable)

        self.categorizedFilepaths = []
        categories = [self.classTable[str(i)][0] for i in range(1000)]
        executor = ProcessPoolExecutor(16)
        for result in executor.map(ImageNet1000.loadCategoryFilenames, categories):
            self.categorizedFilepaths.append(result)
        executor.shutdown()

    @staticmethod
    def loadCategoryFilenames(category):
        return list(filter(lambda x: category in x, filenames))

    def __len__(self):
        length = np.sum([len(files) for files in self.categorizedFilepaths])
        return length

    def preloadImageData(self): # can't do this, because then os.fork() will throw mem error. I think this => 0.05s instead of 0.075s per batch
        self.categorizedImageData = []
        for categoryFilepaths in tqdm.tqdm(self.categorizedFilepaths):
            categoryImageData = []
            for filename in categoryFilepaths:
                fullPath = os.path.join(self.directory, filename)
                file = open(fullPath, "rb")
                bytes = file.read()
                file.close()
                categoryImageData.append(io.BytesIO(bytes))
                os.remove(fullPath)
            self.categorizedImageData.append(categoryImageData)

    def __getitem__(self, idx):
        if (self.classLabel is not None):
            categoryIdx = self.classLabel
        else:
            categoryIdx = idx % len(self.categorizedFilepaths)
        randomIdx = torch.randint(len(self.categorizedFilepaths[categoryIdx]),size=(1,)).item()

        imagePath = self.categorizedFilepaths[categoryIdx][randomIdx]
        img = Image.open(os.path.join(self.directory, imagePath)).convert("RGB")
        # img = Image.open(self.categorizedImageData[categoryIdx][randomIdx])

        shorterSideLength = np.min(img.size)
        if shorterSideLength <= 224:
            newShorterSideLength = 224
        else:
            newShorterSideLength = np.random.randint(224, shorterSideLength)
        img = torchvision.transforms.Resize(newShorterSideLength)(img)
        img = torchvision.transforms.RandomHorizontalFlip(0.5)(img)
        img = torchvision.transforms.RandomCrop((224, 224))(img)
        torchImg = torchvision.transforms.ToTensor()(img)
        img.close()
        torchImg = torchvision.transforms.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225])(torchImg)
        return (torchImg, categoryIdx)

# mount the ssd at /mnt/ramdisk
fileNamesBlob = "gs://unitousdata-central1/imagenet/train/allTrainBlobNames.pkl"
downloader = GoogleStorageMassDownloader("/mnt/ramdisk", "unitousdata-central1", fileNamesBlob, numProcesses=16, maxConnPerProcess=64, convertImageType=None)
filenames = downloader.run() # takes about 8 minutes.

#------------------------------ End Data Loading ------------------------------

classMapURL = "http://files.fast.ai/models/imagenet_class_index.json"
classTable = json.loads(requests.get(classMapURL).content.decode("utf-8"))
classTable["99"]

dataset = ImageNet1000("/mnt/ramdisk", filenames, 1)
# 0.055s/batch is possible, see distillationExperiments.
dataloader = DataLoader(dataset, batch_size=64, shuffle=True, num_workers=12, )

def tensorsToImage(tensor, denormalize=True):
    std = np.array([0.229, 0.224, 0.225])[:, np.newaxis, np.newaxis]
    mean = np.array([0.485, 0.456, 0.406])[:, np.newaxis, np.newaxis]
    image = tensor.cpu().detach().numpy()
    if denormalize:
        image = std*image + mean
    image = image.transpose(-2, -1, -3).clip(0, 1)
    image = (image*255).astype(np.uint8)
    return image

vgg16 = torchvision.models.vgg16(pretrained=True).to(device)

for i in range(10):
    idx = 16
    testImage = dataset[idx][0]
    display(Image.fromarray(tensorsToImage(testImage)))

class Generator(nn.Module):
	def __init__(self):
		super().__init__()
		self.conv1 = nn.ConvTranspose2d(100, 512, 5, stride=2) #output 512x5x5
		self.bnorm1 = nn.BatchNorm2d(512)
		self.conv2 = nn.ConvTranspose2d(512, 256, 5, stride=2) #output 256x13x13
		self.bnorm2 = nn.BatchNorm2d(256)
		self.conv3 = nn.ConvTranspose2d(256, 128, 5, stride=2) #ouput 128x29x29
		self.bnorm3 = nn.BatchNorm2d(128)
		self.conv4 = nn.ConvTranspose2d(128, 64, 5, stride=2) #output 64x61x61
		self.bnorm4 = nn.BatchNorm2d(64)
		self.conv5 = nn.ConvTranspose2d(64, 32, 5, stride=2) #output 32x125x125
		self.bnorm5 = nn.BatchNorm2d(32)
		self.conv6 = nn.ConvTranspose2d(32, 3, 5, stride=2) #output 3x253x253

	def forward(self, x):
		x = x.view(-1, 100, 1, 1)
		x = self.bnorm1(nn.LeakyReLU(0.1)(self.conv1(x)))
		x = self.bnorm2(nn.LeakyReLU(0.1)(self.conv2(x)))
		x = self.bnorm3(nn.LeakyReLU(0.1)(self.conv3(x)))
		x = self.bnorm4(nn.LeakyReLU(0.1)(self.conv4(x)))
		x = self.bnorm5(nn.LeakyReLU(0.1)(self.conv5(x)))
		x = nn.Tanh()(self.conv6(x))
		return x

generator = Generator().to(device)
"{:,}".format(np.sum([np.prod(p.shape) for p in generator.parameters()]))

batchSize = 16
optim = torch.optim.Adam(generator.parameters(), lr=0.001)

display(Image.fromarray(tensorsToImage(genImages[0])))

# Optimize generator params for predicted labels of vgg16.
for i in range(100):
    optim.zero_grad()
    noise = torch.randn(batchSize, 100)
    genImages = generator(noise.to(device))
    predictedLabels = vgg16(genImages)
    targetLabels = torch.ones(batchSize, dtype=torch.long) * 1
    error = torch.nn.CrossEntropyLoss()(predictedLabels, targetLabels.to(device)) # 5.9 and 6.9 minimums at 16, 32 batchsize
    error.backward()
    optim.step()
    print(error.item())

torch.argmax(predictedLabels, axis=1)

# for low layers: subpatches, subpatches with jitter, download layer 10 max

# Optimize for layer activations
vgg16=torchvision.models.vgg16(pretrained=True).to(device)
subnetwork = vgg16.features[:20]
blackVal = (np.array([0,0,0]) - np.array([0.485, 0.456, 0.406])) / np.array([0.229, 0.224, 0.225])
whiteVal = (np.array([1,1,1]) - np.array([0.485, 0.456, 0.406])) / np.array([0.229, 0.224, 0.225])
meanVal = np.array([0.485, 0.456, 0.406])
jitterAmount=32

images = []
for idx in range(20):
    image = torch.zeros(1, 3, 224+jitterAmount,  224+jitterAmount) + torch.FloatTensor(meanVal).unsqueeze(1).unsqueeze(1)
    image = image.to(device)
    # image = torch.randn(1, 3, 224, 224).to(device).clamp(0, 1)
    image.requires_grad = True
    optim = torch.optim.SGD([image], lr=1)
    for i in range(1000):
        jitter = torch.randint(0, jitterAmount, size=(2,))
        subImage = image[:, :, jitter[0]:(224+jitter[0]), jitter[1]:(224+jitter[1])]
        optim.zero_grad()
        activations = subnetwork(subImage)
        # error = -torch.mean(torch.pow(activations[0,idx], 2)) #MSE leads to cool visuals even on 3x3 kernel but looks wrong
        error = -torch.mean(torch.pow(activations[0,idx][0, 0], 1))
        error.backward()
        optim.step()
        if i % 999 == 0:
            print(error.item())
    images.append(Image.fromarray(tensorsToImage(image[0])).resize((224*2, 224*2)))
    display(Image.fromarray(tensorsToImage(image[0])).resize((224*2, 224*2)))
for img in images:
    display(img)

imgArrays = []
for img in images:
    imgArrays.append(np.array(img))
imgStack = np.concatenate(imgArrays, axis=0)
Image.fromarray(imgStack).save("vgg16layer3filterPointMaximallyActivatingUNTRAINED.png")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
