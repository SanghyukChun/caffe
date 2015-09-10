# Caffe development version by SanghyukChun

This is repository for Caffe development by [@SanghyukChun](https://github.com/SanghyukChun/).

Now currently working on implementing batch normalization based on [PR#1965](https://github.com/BVLC/caffe/pull/1965/).

### File changes for implementation of batch normalization
* [common layer header](https://github.com/SanghyukChun/caffe/blob/master/include/caffe/common_layers.hpp) and [proto file](https://github.com/SanghyukChun/caffe/blob/master/src/caffe/proto/caffe.proto)
* bn layer [cpp](https://github.com/SanghyukChun/caffe/blob/master/src/caffe/layers/bn_layer.cpp) and [cu](https://github.com/SanghyukChun/caffe/blob/master/src/caffe/layers/bn_layer.cu)
* BN modles based on [AlexNet](https://github.com/SanghyukChun/caffe/tree/master/models/sanghyuk_alexnet_bn) and [GoogleNet](https://github.com/SanghyukChun/caffe/tree/master/models/sanghyuk_googlenet_bn) in model zoo

### Implemetation of BN
In [PR#1965](https://github.com/BVLC/caffe/pull/1965/), there are 2 unresolved problems:

1. In the PR, shuffling is implemented for only encoded data.
2. Mean/variance for inference is not implemented. Therefore, we cannot classify test data with batch size 1.

In this dev repository, I resolve the problems as followings:

1. Instead of using shuffling code from [PR#1965](https://github.com/BVLC/caffe/pull/1965/), I use shuffle param in ImageDataLayer. It is still not implemented for every layers but I decide to use ImageDataLayer because it is uniform shuffling and not that slow then DataLayer.
2. I apply moving average strategy duriung training phase and use it for inference.

Some approaches I am now considering:

1. Shuffling via random skip as mentioned in [the comment in PR#1965](https://github.com/BVLC/caffe/pull/1965#issuecomment-133727821). Then we can use every type of data layer for shuffling. However, my experiments say shuffling is not critical issue for BN network
2. Implement completely independent batch normalization inference module as like [lim6060's](https://github.com/lim0606/caffe-dev/blob/master/tools/test_bn.cpp). However, my my experiments say inference rule is not critical issue for BN network

## License and Citation

Caffe is released under the [BSD 2-Clause license](https://github.com/BVLC/caffe/blob/master/LICENSE).
The BVLC reference models are released for unrestricted use.

Please cite Caffe in your publications if it helps your research:

    @article{jia2014caffe,
      Author = {Jia, Yangqing and Shelhamer, Evan and Donahue, Jeff and Karayev, Sergey and Long, Jonathan and Girshick, Ross and Guadarrama, Sergio and Darrell, Trevor},
      Journal = {arXiv preprint arXiv:1408.5093},
      Title = {Caffe: Convolutional Architecture for Fast Feature Embedding},
      Year = {2014}
    }
