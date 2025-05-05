## Resource Efficient Deep Learning Operators
These article takes notes of a series of resource efficient deep learning operators, which are capable of deploying on resource constraint hardwares.


<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Resource Efficient Deep Learning Operators](#resource-efficient-deep-learning-operators)
  - [Depthwise Separable Convolutions](#depthwise-separable-convolutions)
    - [Resources](#resources)
    - [Explanations](#explanations)
    - [Comparison](#comparison)
    - [Implementations](#implementations)

<!-- /code_chunk_output -->



### Depthwise Separable Convolutions
#### Resources
- [An explanation of depthwise separable convolutions](https://paddlepedia.readthedocs.io/en/latest/tutorials/CNN/convolution_operator/Separable_Convolution.html)

#### Explanations
A typical convolution operator will perform convolution operations on all input channels and increase the output channels all at once. The depthwise separable convolution operator firstly perform the convolution on each input channel (depthwise), and then it uses the pointwise operator to increase the output channels (increase the feature dimensions). The depthwise separable convolution operator can significantly reduce the parameters and MACs (Multiply-Accumulate Operators). The following section will compare these two metrics for typical convolution operator and depthwise separable convolution operator:

Input data size
$$H_{in} \times W_{in} \times C_{in}$$

Output data size
$$H_{out} \times W_{out} \times C_{out}$$

**For typical convolution kernel:**

Typical convolution kernel
$$K \times K \times C_{in} \times C_{out}$$

Typical convolution kernel MACs
$$K \times K \times H_{out} \times W_{out} \times C_{in} \times C_{out}$$

Typical convolution kernel parameters
$$K \times K \times \times C_{in} \times C_{out}$$

**For depthwise seperable convolution kernel:**
*Depthwise MACs*
$$K \times K \times H_{out} \times W_{out}\times C_{in}$$
*Pointwise MACs*
$$ H_{out} \times W_{out}\times C_{in}\times C_{out}$$
*All MACs*
$$K \times K \times H_{out} \times W_{out}\times C_{in} + H_{out} \times W_{out}\times C_{in}\times C_{out}$$

*Depthwise parameters*
$$K \times K \times C_{in}$$
*Pointwise parameters*
$$C_{in} \times C_{out}$$
*All parameters*
$$K \times K \times C_{in} + C_{in} \times C_{out}$$

#### Comparison
For the MACs, the depthwise separable convolution can reduce the MACs in the number of
$$\frac{1}{C_{out}} + \frac{1}{K^2}$$

For the parameters, the depthsize seperable convolution can reduce the parameters in the number of
$$\frac{1}{C_{out}} + \frac{1}{K^2}$$

#### Implementations
- [Keras 3 DepthwiseConv2D](https://keras.io/api/layers/convolution_layers/depthwise_convolution2d/)