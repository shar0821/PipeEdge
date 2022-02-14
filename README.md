# EdgePipe

## Prerequisites

Install dependencies with with:

```sh
pip install -r requirements.txt
```

Download weight files from [Google Cloud](https://console.cloud.google.com/storage/browser/vit_models;tab=objects?pageState=(%22StorageObjectListTable%22:(%22f%22:%22%255B%255D%22))&prefix=&forceOnObjectsSortingFiltering=false), e.g.:

```sh
wget https://storage.googleapis.com/vit_models/imagenet21k%2Bimagenet2012/ViT-B_16-224.npz
wget https://storage.googleapis.com/vit_models/imagenet21k%2Bimagenet2012/ViT-L_16-224.npz
wget https://storage.googleapis.com/vit_models/imagenet21k/ViT-H_14.npz
```


## Usage

For full usage help, run:

```sh
python runtime.py -h
```

To run with default parameters (using ViT-Base) on a single node:

```sh
python runtime.py 0 1
```

To run on multiple nodes, e.g., with 2 stages and even partitioning, on rank 0:

```sh
python runtime.py 0 2 -pt 1,24,25,48
```

and on rank 1:

```sh
python runtime.py 1 2 -pt 1,24,25,48
```

### Partitioning

For example, the ViT-Base model has 12 layers, so the range is [1, 12*4] = [1, 48].

An even partitioning for 2 nodes is:
```
partition = [1,24,25,48]
```

An uneven partitioning for 2 nodes could be:
```
partition = [1,47,48,48]
```

A partitioning for 4 nodes could be:
```
partition = [1,4,5,8,9,20,21,48]
```



## TODO

- [x] Remove unneccessary code
- [x] Build layers in the node and reload the weight 
- [x] Combine the function for different number of nodes
- [x] Test for correctness
- [x] Test for multiple nodes 
- [x] Test for vit-large model
- [x] Test for vit-huge model
- [x] Support fine-grained partitioning
- [x] Add baseline.py   
- [x] Operator-level partition method
- [x] Edit profile script
- [x] Create a simulator 
- [x] Import profile to Partition
- [x] Edit Partition script
- [ ] Import Partition to Runtime
- [ ] Solve the memory leak problem ([RPC Framework Problem](https://github.com/pytorch/pytorch/issues/61920#issuecomment-886345414))


