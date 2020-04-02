---
title: pil example dataloader (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'dataloader'


## python dataloader

Python pil example: dataloader

```python


class DataLoader(object):
    def __init__(self, options, data_path, load_fkt, shuffle, return_paths):
        self.pairedData = None
        self.initialize(options, load_fkt, data, shuffle, return_paths)

    def initialize(self, options, load_fkt, data_path, shuffle, return_paths):
        pass

    def load_data(self):
        """
        Function to load one dataPair
        :return: Paired data
        """
        return self.pairedData

    @staticmethod
    def name():
        """
        Function to get class name
        :return: class name 8string)
        """
        return 'BaseDataLoader'

    def __len__(self):
        pass


class AlignedDataLoader(DataLoader):
    def __init__(self, options, data_path, load_fkt, shuffle=True, return_paths=True):
        self.dataset = None
        super(AlignedDataLoader, self).__init__(options, data_path, load_fkt, shuffle, return_paths)
        self.initialize(options, load_fkt, shuffle, return_paths)

    def initialize(self, options, load_fkt, data_path, shuffle, return_paths):
        if options.inputNc == 1:
            norm = transforms.Normalize([0.5], [0.5])
        else:
            norm = transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5))
        transform = transforms.Compose([transforms.ToTensor(), norm])
        dataset = ImageFolder(data_path, loader=load_fkt, transform=transform, return_paths=return_paths)

        data_loader = data.DataLoader(dataset, batch_size=options.batchSize, shuffle=shuffle, num_workers=0)
        self.dataset = dataset
        self.pairedData = AlignedPairedData(data_loader, return_paths)

    @staticmethod
    def name():
        return 'AlignedDataLoader'

    def __len__(self):
        return len(self.dataset)


class UnalignedDataLoader(DataLoader):
    """Class to handle data load process of a dataset"""

    def __init__(self, options, data_path, load_fkt=image_load_fkt_pillow_unaligned, shuffle=True, return_paths=True):
        """
        Function to create class variables
        :param options: class containing options (args of BaseOptions or subclass)
        :param data_path: path containing the dataset
        :param load_fkt: function to load the data
        :param shuffle: True if random item of dataset should be loaded, False otherwise
        :param return_paths: True if paths should be returned alongside data, False otherwise
        """
        self.datasetA = None
        self.datasetB = None
        super(UnalignedDataLoader, self).__init__(options, data_path, load_fkt, shuffle, return_paths)
        self.initialize(options, load_fkt, data_path, shuffle, return_paths)

    def initialize(self, options, load_fkt, data_path, shuffle, return_paths):
        """
        Function to initialize class variables
        :param options: class containing options (args of BaseOptions or subclass)
        :param data_path: path containing the dataset
        :param load_fkt: function to load the data
        :param shuffle: True if random item of dataset should be loaded, False otherwise
        :param return_paths: True if paths should be returned alongside data, False otherwise
        :return None
        """
        if options.inputNc == 1:
            norm = transforms.Normalize([0.5], [0.5])
        else:
            norm = transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5))
        transform = transforms.Compose([transforms.ToTensor(), norm])

        datasetA = ImageFolder(data_path + "/A", options, loader=load_fkt, transform=transform,
                               return_paths=return_paths)
        datasetB = ImageFolder(data_path + "/B", options, loader=load_fkt, transform=transform,
                               return_paths=return_paths)

        data_loader_a = data.DataLoader(dataset=datasetA, batch_size=options.batchSize, shuffle=shuffle, num_workers=0)
        data_loader_b = data.DataLoader(dataset=datasetB, batch_size=options.batchSize, shuffle=shuffle, num_workers=0)

        self.datasetA = datasetA
        self.datasetB = datasetB

        self.pairedData = UnalignedPairedData(data_loader_a, data_loader_b, return_paths=return_paths)

    @staticmethod
    def name():
        """
        Function to get class name
        :return: class name 8string)
        """
        return 'UnalignedDataLoader'

    def __len__(self):
        """
        Function to get the maximum number of items in the datasets
        :return: number of items
        """
        return max(len(self.datasetA), len(self.datasetB))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
