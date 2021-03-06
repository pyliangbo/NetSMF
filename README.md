# NetSMF

NetSMF: Large-Scale Network Embedding as Sparse Matrix Factorization

# HOWTO

## How to install
```
sudo apt-get install cmake
sudo apt-get install libgflags-dev
sudo apt-get install liblog4cxx-dev
sudo apt-get install libomp-dev
sudo apt-get install libeigen3-dev
https://github.com/xptree/NetSMF.git
cd NetSMF
mkdir build
./configure
cd build
make
```

## How to run

### Input

Support undirected networks with edgelist format.

For unweighted networks, each edge should appear twice `a b` and `b a`.

For weighted networks, each edge should appear twice `a b w` and `b a w`.

You may want to use `example/mat2edge.py` to translate mat to edgelist.

`.mat` files can be downloaded here:

* BlogCatalog [Source](http://socialcomputing.asu.edu/datasets/BlogCatalog3) [Preprocessed](http://leitang.net/code/social-dimension/data/blogcatalog.mat)
* Protein-Protein Interaction [Source](http://thebiogrid.org/download.php) [Preprocessed](http://snap.stanford.edu/node2vec/Homo_sapiens.mat)
* [Flickr](http://leitang.net/code/social-dimension/data/flickr.mat)
* [YouTube](http://leitang.net/code/social-dimension/data/youtube.mat)



### Run NetSMF

For unweighted networks, see `example/blog.sh` for an example.

`blog.sh` takes three arguments, the first one indicates the input edgelist file, the second one indicates the output file, the third one indicating the origin `.mat` file containing network and labels.

For exmaple, runing `./blog.sh blogcatalog.edgelist blogcatalog.netsmf blogcatalog.mat` will

* check if `blogcatalog.edgelist` is a valid file. If not, it calls `mat2edge.py` to translate mat file `blogcatalog.mat` to edgelist `blogcatalog.edgelist`.
* call NetSMF algorithm, and store the 128-dim embedding at `blogcatalog.netsmf_128.npy`.
* call `predict.py` to evaluate NetSMF at the label classification task.

You can use `-weight` to switch to weighted networks and use `-noweight` to switch to unweighted network (default unweighted).

### About truncated logarithm

We propose to use truncated logarithm in our WWW'19 paper.

In the code, we provide a new option `log1p`, i.e., `log(1+x)`. You can use  `-log1p` to turn it on and `-nolog1p` to turn it off (default off). Empirically speaking, `log1p` sometimes achieves better performance, for example in wiki dataset.


## Acknowledgement

The implementation of randomized singular value decomposition is by [redsvd](https://code.google.com/p/redsvd/) and [HPCA](https://github.com/idiap/hpca).
