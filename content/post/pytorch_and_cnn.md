# What is a CNN

From my understanding, Convolutional neural network (ConvNets or CNNs) is
basically a neural network where Convolution is used to extract features and
neural network is used to do the classification.

A typical CNN using several layers of convolution (including ReLU and Pooling)
to extract features and then flat the output to vectors as inputs to the FC
(Full connected network) which is basically the average neural network.

Refer to the
[link](https://medium.com/@RaghavPrabhu/understanding-of-convolutional-neural-network-cnn-deep-learning-99760835f148)
for more detailed reading.

# Number of Input channel and Number of output could be different

Please note that for example following python (torch) code create a Convolution
layer, the first parameter `1` is the number of input channels, while `6` is
number of the output channels, How does it work? 

```python
self.conv1 = nn.Conv2d(1, 6, 3)
```

It means that there are 6 filters to do convolution on the same location of the
image, so that one channel of image could outputs 6 different channels of
images.

Refer to
[input and output channels](https://discuss.pytorch.org/t/convolution-input-and-output-channels/10205)
for the discussion.

# size reduced after convolution

Following example create a convolution function `conv2d_test`, which takes 1
channel and output another channel with kernel size 3 that is 3 x 3.

```
>>> conv2d_test = nn.Conv2d(1,1,3)
>>> x=torch.rand(1,1,5,5)
>>> conv2d_test(x)
tensor([[[[ 0.1419,  0.0967, -0.1615],
          [-0.1023,  0.2995, -0.0513],
          [ 0.0512,  0.1443, -0.0801]]]], grad_fn=<MkldnnConvolutionBackward>)
```

We could see that the output dimension is 3 x 3.

Input size 5 - Kernel Size 3 + 1 = Output size 3

# understand an example

In https://pytorch.org/tutorials/beginner/blitz/neural_networks_tutorial.html

The examples define a network:

One channel 32 x 32 image input 
-> (convolution with kernel size 5) 6 Channels 28 x 28 images  (32 - 3 + 1 = 30)
-> (ReLU and apply 2 x 2 pooling, that is down sampling), 6 Channels 14 x 14 images
-> (convolution with kernel size 3) 16 Channels 12 x 12 images 
-> (ReLU and apply 2 x 2 pooling, that is down sampling), 16 Channels 6 x 6 images

# what is the purpose to "accumulate" the gradient?

Refer to [github](http://cs231n.github.io/optimization-2/)

```
Gradients add up at forks. The forward expression involves the variables x,y
multiple times, so when we perform backpropagation we must be careful to use +=
instead of = to accumulate the gradient on these variables (otherwise we would
overwrite it). This follows the multivariable chain rule in Calculus, which
states that if a variable branches out to different parts of the circuit, then
the gradients that flow back to it will add.
```

To make it easy, it allows the same parameters to be used multiple times, so
the gradient is added up from the different path of back-propagation.

That is to say in each training loop, you have to zero the gradients before
performing back-propagation. Refer to end of
[tutorial](https://pytorch.org/tutorials/beginner/blitz/neural_networks_tutorial.html).


# Different models on ImageNet

Refer to https://medium.com/@RaghavPrabhu/cnn-architectures-lenet-alexnet-vgg-googlenet-and-resnet-7c81c017b848

# ResNet

```

不做图像相关处理，研究不多，看过网络结构图和highway
network那篇文章，说说自己的直观理解。我觉得是在解决
不同层级的特征组合问题或者说浅层信息的远距离传输问题。卷积网络都是从低阶特征抽象出高阶特征的，从点线面到简单结构到复杂形状，而每一层网络能表示的往往是相近复杂度的特征（或者说相近感受野范围内的特征）。另一方面，对不同的图片，相似的特征并不总是处在同一层，比如同样的抽象出一个眼睛，小点的图像可能在第3层就抽象出来了，但是对同样内容的一个大尺度图片，可能要到第8层才抽象出来（因为要这么深感受野才覆盖得到）。图像任务利用的特征基本都不是单一层级的，而是多个层级的组合，比如眉毛部分的局部特征+整个脸部特征来判断一个人，但是这些特征并不都在一个层级。不加res连接，意味着低阶的特征要起作用需要经过多层网络来传递，这个过程中是会有信息损益的，可能第三层抽象出来的特征，传到最后就不知道是什么东西了。加了res层保证了各个层级的特征可以随意的组合，而且可以高保真的让各层级的有用特征传递到最终的特征层。

作者：pymars
链接：https://www.zhihu.com/question/64494691/answer/325751092
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

# What is Deep learning?

https://www.youtube.com/watch?v=zacsrmK03B0

CNN layers could be designed different, a lot of design was introduced. In the
early days, people proposed AlexNet, VGG, GoogleNet which has tens of CNN
layers which is already "deep" at that time. Later ResNet was introduced which
has 50-200 layers.

Hence the "deep" looks like means the number of layers is large, so features
could be extracted in different layers.

## What state-of-art deep CNN model architecture to use? 

Refer to
https://towardsdatascience.com/review-inception-v4-evolved-from-googlenet-merged-with-resnet-idea-image-classification-5e8c339d18bc

Inception V4 is kind of combination of ResNet and Inception.

## Transfer Learnning

These state of art model actually has generic capability to recognize an image.
The pre-trained model contains generic knowledge, which could be used to do
image recognition task for other purposes. The method is called "transfer
learning".

Refer to
https://towardsdatascience.com/transfer-learning-with-convolutional-neural-networks-in-pytorch-dd09190245ce

``` 

The basic premise of transfer learning is simple: take a model trained on a
large dataset and transfer its knowledge to a smaller dataset. For object
recognition with a CNN, we freeze the early convolutional layers of the network
and only train the last few layers which make a prediction. The idea is the
convolutional layers extract general, low-level features that are applicable
across images — such as edges, patterns, gradients — and the later layers
identify specific features within an image such as eyes or wheels.

```
