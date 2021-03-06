# **CS231N : [Convolutional Neural Networks for Visual Recognition](http://cs231n.stanford.edu/index.html)**
## **Lecture 1** : 
An overview of Computer Vision and problems everyone is trying to solve.

## **Lecture 2 : K-Nearest Neighbor Classifier** <br>
EDIT!

## **Lecture 3 : Loss Functions and Optimization**
A Loss function, in essence, is a measure of **how bad the learned weights of a model are.**<br>
The loss function includes model **prediction** and then a **comparison** between the predicted value and the true value. <br>
Depending on the output of your loss function, you would know if your model is any good. <br>

L = 

The learning part is introduced with the loss function. While training a model, you would always want to reduce your loss by optimizing your model after each iteration. This is where optimization comes in. As you would like to minimize your model loss, the optimization is always performed in the direction of the highest gradient(or slope.)

  1. Hinge Loss : &nbsp;&nbsp;&nbsp;&nbsp;<img src="https://i.upmath.me/svg/Loss%20%3D%20%5Csum_%7Bj%5Cneq%20y_i%7Ds_j%20-%20s_%7By_i%7D%20%2B%20%5Cdelta" alt="Loss = \sum_{j\neq y_i}s_j - s_{y_i} + \delta" />  
  
  2. Softmax Loss : &nbsp;&nbsp;&nbsp;&nbsp; <img src="https://i.upmath.me/svg/Loss%20%3D%20%5Csum%20-log(p_%7By_i%7D)%20%5C%20where%20%5C%20p_%7By_i%7D%20%3D%20%7B%5Cexp(s_%7By_i%7D)%20%5Cover%20%5Csum_%7Bj%7D%5Cexp(s_j)%20%20%7D" alt="Loss = \sum -log(p_{y_i}) \ where \ p_{y_i} = {\exp(s_{y_i}) \over \sum_{j}\exp(s_j)  }" />

## **Lecture 4/5 : Introduction to Neural Networks, Convolutional Neural Networks** <br>
*


## **Lecture 6 : Training Networks**
  1. **Choice of Activation functions for your network**:
      | Activation Function | f(x) <img></img> | Pros | Cons |
      | :---: |---| :--- | :--- |
      | Sigmoid | 1 / (1 + e<sup>-x</sup>) | -> None. Don't use this | -> Output isn't zero centered. <br> -> Kills gradient flow as a result of saturation at a high or low x-value |
      | Tanh Activation function | tanh(x) | -> Zero Centered output. Optimization is efficient compared to sigmoid <br>  | -> Saturation kills the gradient flow(high or low x-value) |
      | Rectified Linear Unit (RELU) | max(0, x) | -> No saturation in the positive region. <br> -> Efficient computation and faster convergence | -> Non zero centered. <br> -> Dead Relu's if x < 0 (Can happen because of bad initialization of weights or if the learning rate is too high) <br>|
      | Leaky RELU | max(0.01x, x) | -> All pros of Relu's <br> -> No dying Relu's here because of a gradient in the negative direction too. | n |
      | Parametric RELU | max(αx, x) | α is a hyperparameter that needs to be optimized | -> |
      | Exponential LU | = x  if x > 0 <br> = α * (e<sup>x</sup> - 1) if x <= 0 | -> All RELU benfits hold for this. <br> -> Almost zero centered output. <br> -> Negative region saturates for a large negative value. Provides Noise robustness.| -> |
      |<img width=200/>|<img width=375/>| | |
  
  
  2. **Data Pre-processing(Zero Centered Data)** :
      Why is zero mean data required? Assume a scenario where all the inputs(X) is positive. You forward propagate through the network and obtain a loss value.<br>
      During backprop at this layer, the upstream gradient can either be positive or negative. The local gradient of W is X.<br>
      Therefore, the gradient of W is either all positive or all negative. This leaves us with only two directions of optimizations, which may not be optimal.<br>
      By zero centering your data, you would have an equal number of poisitve and negative inputs(almost), thereby providing multiple directions for updates.<br>
      
      **X_new = (X - Mean(X)) / StdDev(X)**
  
  
  3. **Initialization of Weights for robustness of network** : <br>
      Assume a scenario where you try to train a deep neural network, with all of the weights in every layer initialized with zeros. During back-propagation, successive matrix multiplications with the weight matrix is carried out until the first layer is reached. This makes it very important for the weights to be initialized correctly. It helps in mitigating the **Vanishing Gradient** problem.
  
  
  4. **Batch Normalization to prevent dead units in your network**
  
  
  5. **Training Process** :
      1. Always start your training process with a small subset of the data from your training set.
      2. Start with Zero Regularization and ensure that the value for the loss(or cost) is plausible.
      3. Update the learning rate to a small value(say 1e-6) and check how this affects the network loss.
      4. Similarly try out a small value for the regularization and check how this affects the loss. You should expect a slight increase in loss.
      5. Optimization in the log space is easier. Use the random.uniform function in the numpy library(10**uniform(l, h)).
      6. A random search for these hyper-parameters works out better than the grid search.
      7. Look at all the training and Validation accuracies achieved by the network and try to optimize in areas of high accuracy.      
      8. Monitor plots of training, validation accuracies as well as loss curves:          
         A big difference between Train and Val accuracy(Overfitting) - Increase Regularization; Small difference - Model saturated. Increase model capacity.
         Monitor loss curves for information about the learning rate effectiveness.
         
## **Lecture 7 : Training your Network** <br>
*Optimization Algorithms :*<br><br>
  A Number of optimization methods are defined. The simplest one among these is the Stochastic Gradient descent. Although its a good way to optimize the parameters of your model, the algorithm has its own drawbacks, which leads to inefficient optimization.<br>
  These are : <br>
  1. If the sensitivity of the parameters are different along different directions, it results in sub-optimal optimization(zig-zag) instead of going directly along the direction of highest gradient descent.
  2. Stochasticity introduces its own problems, where in the mini-batch that is used to update the parameters in an iteration might lead to noisy gradients, thereby resulting in a wrong update. The updates, thereby, are jittery.
  3. Sub-optimal minima or Saddle points may be reached by the function. In regions around these saddle points, the gradients are close to zero, which would reduce the rate of update and inturn, increase the training time. 
  
  Inorder to resolve these issues with the optimization algorithm, a number of other techniques are defined : <br>
   1. **SGD with Momentum** : This is similar to the SGD but parameters are updated, not using the gradient, but with a 'velocity' value. This 'velocity' value is defined by using the gradient. The accumulated velocity is used as the gradient. The intuition here is that if you reach a saddle point, the momentum accumulated would help you cross the saddle point. <br>
       ----> **V<sub>t+1</sub> = beta * V<sub>t</sub> + grad** <br>
       ----> **X -= learning_rate * V<sub>t+1</sub>**
   2. **AdaGrad Optimizer** : Here, a Gradient squared term is accumulated with the square of the gradient. This is used later during optimization.
    The intuition here is that if the gradient along a particular direction is large, the denominator in the formula would be large too, thereby slowing down the update along that direction. <br>
       ----> **gradSquared += grad * grad**
       ----> **X -= learning_rate * grad / (sqrt(gradSquared) + eps)**  
   3. **RMS Prop** : This is similar to AdaGrad Optimizer. Here, the gradSquared term is decayed using a decay_rate.
   Decay Rate is a hyperparameter(0.9 or 0.99) <br>
       ----> **gradSquared = decay_rate * grdaSquared + (1 - decay_rate) * grad * grad** <br>
       ----> **X -= learning_rate * grad / (sqrt(gradSquared) + eps)**   
   4. **Adam Optimizer** : Adam Optimizer combines the best properties of SGD with momentum and RMS Prop. 
       ----> **f_m, s_m = 0, 0** <br>
       ----> **f_m = beta1 * f_m + (1 - beta1) * grad** (Momentum term)<br>
       ----> **s_m = beta2 * s_m + (1 - beta2) * grad * grad** (Grad Squared Term)<br>
       ----> **X -= learning_rate * f_m / sqrt(s_m) + eps**<br>
       But there's a problem here. If we start with **s_m = 0**, during the first iteration the denominator **sqrt(s_m) + eps** is very small. This would increase the rate of update. We would be taking a very large step during the first few iterations as a result of this bias.<br>
       Therefore, bias correction is defined :<br>
       ----> **f_b = f_m / (1 - beta1 ^ t)**<br>
       ----> **s_b = s_m / (1 - beta2 ^ t)**<br>
       ----> **X -= learning_rate * f_b / sqrt(s_b) + eps**<br>   
    
  In addition to these optimization algorithms, some learning rate decay techniques are also defined which would be helpful if the loss curves tend to plateau after a few iterations of training. <br>
    1. Exponential Decay : **α = α<sub>0</sub> * e<sup>-Kt</sup>** (K - Decay rate)<br>
    2. 1 / t Decay : **α = α<sub>0</sub> / (1 + Kt)** (K - Decay rate) <br>
    3. Decay in phases : Depnding on the total number of iterations, decay the learning rate after a certain count is reached.
    
*Reducing the gap between the Train and Validation error :*  <br>
During training, if you motice that the gap between the train and validation error is high, it means that the model is being overfit to the training data.<br>
When this happens, you would want to increase the model loss so that it prevents the model from fitting the train data too well. 
There are many ways to prevent overfitting :
  1. **Model Ensembles** : Train multiple networks. FInal results on the test set are averaged to get the best result. <br>  
  2.   
  3. **Regularization** :<br>
  As we have already seen, we use some amount of regularization(add a regularization loss to the data loss). While learning, this would tell the model not to fit the train data too well. If you use L2 regularization, it would mean that you want to spread your parameters across the whole range rather than having them concentrated in certain regions. If L2 Reg is used, you would want to concentrate on certain features only.<br>
  4. **Dropout Layer** :<br>
  Instead of training multiple models(model ensembles), Dropout is something we can use to get the same effect. What happens in dropout layers is that certain randomly chosen activations of a particular layer are set to zero. <br>
  The intuition here is that, by blocking a random set of activations, the model would learn the most optimized feature set to classify an example correctly.<br>
  At train time : Block a random number of activations(**keep probability p**). Back-Propagate only to these nodes. <br>
  At test time : Multiply predicted output by keep probability p. This is done because you wouldn't want randomness in your model while predicting new unseen data. <br>
  
  Instead of this you can also use an **"Inverted Dropout"**.  At train time, divide the input by the keep probability p, at test time no changes are required. 
    
## **Lecture 8 : Deep Learning Software** <br>
----> CPU vs GPU : <br>
A **CPU** has lesser number of cores, but these have much higher clock sppeds compared to GPU cores, and hence are better preferred for sequential tasks. Memory shared with the system. <br>
A **GPU** has thousands of cores, but have small clock speeds, and are only used for parallel processing. They also have their own memory. <br>  
*Why using a GPU is useful for Deep Learning* : At the core of Deep Learning, there is a lot of matrix multiplication(or you could say repetitive tasks). The whole process of multiplying two matrices can be divided into sub-tasks which can be parallelized. Therefore, it would be much faster using a GPU for this task, rather than using a CPU. 
In a CPU, all of these tasks happen sequentially, and with exploding sizes of matrices, the amount of time these computations take might be a lot. <br>
Model parameters are stored in the GPU memory, whereas the training data is stored on the hard drive. This might create a bottleneck on data access by the GPU. Best possible solutions are to, either read all data into the system RAM, use a SSD or running CPU threads in the background to fetch data into the GPU. <br>
*Why Deep Learning Software is preferred* : <br>
  1. Lets you concentrate on developing the forward pass of your model. Gradients are computed by the software. <br>
  2. Provides you the option to run the computations either using the CPU or the GPU. <br>
  
**Static vs Dynamic Graphs** : <br>
In Tensorflow, we create the computational graph first and then run it multiple times. These are static graphs. <br>
While using PyTorch, the graph is created multiple times(created within the loop). These are Dynamic graphs. <br>

| Static Graphs | Dynamic Graphs |
| :---: | :---: |
| Graph created once. Run Multiple times | Graphs created multiple times |
| As the graph is created once, the code to create the graph is not required anymore if the graph itself is stored| The code to create the graph is required everytime you want to process new data |
| Need to learn new constructs to actually create a graph that represents the full model(eg. cond constructs, etc) | Can code the graph easily in numpy code. No need of new constructs |

**Using Tensorflow**: <br>
  1. **Using Tensorflow with the Low Level APIs** : Creating a computational graph first(with __tf.nn__) and then using __tf.GradientTape__ to record gradients of model parameters. Not very          useful for large networks.
  2. **Tensorflow Keras SubClassing API** : Creating a new class for your model. Inheriting all properties of the Keras.Model in your class. <br>
     This used the super method for inheritance. Once you define your computational graph(in the __init__) function, you need to have an explicit __call__ function
     which will use up your input tensors, and feed it to the graph. Additionally, an optimization function is required, so as to optimize all the parameters of the model. <br>
  3. **Tensorflow Keras Sequential API** : You could also use __tf.keras.Sequential__ to create an instance of your model. <br>
     Then, just add layers on your model. This is very useful in case your model is sequential. But, if you have multiple inputs from different sources(or multiple outputs),          using the Sequential API is not very flexible.
  4. **Keras Functional API** : 
  
**Using PyTorch** : The computational graph is always created inside the loop(dynamic graph)<br>
  1. **Using PyTorch with low level APIs** : Using autograd function to compute gradients on relevant tensors. Set requires_grad to True if gradient has to be computed.
  2. **Using the PyTorch Module API** : Inherit properties of the nn.module(use super().init() function for that), and initialize all layers in the model. Additionally create a __forward()__ function to call all layers with required input tensors.
  3. **Using PyTorch with the Sequential API** : Using nn.Sequential to stack sequential layers on top of each other to create a model. If a user defined layer is required to be used, create a new class for the layer(point 2) and use the class instead. This way of using PyTorch is user-friendly, but it isnt very flexible, if the model is not sequential.

## **Lecture 9 : CNN Architectures**

----> [**AlexNet**](https://github.com/abhijeetknayak/Deep-Learning/blob/master/CS-231N/Material/AlexNet.png) : <br>[[Conv] - [MaxPool] - [Normalization]] * 2 - [Conv] - [Conv] - [Conv] - [Pool] - [FC] * 3] <br>
**Conv1** : 96 11 * 11 filters, with stride = 4 ----> Output Dimension : __55 * 55 * 96__; Learnable parameters : __11 * 11 * 3 * 96__ <br>
**MaxPool1** : 3 * 3 filters, with stride = 2   ----> Output Dimension : __27 * 27 * 96__; Learnable parameters : __0__ <br>
**Norm1** : Output Dimension : __27 * 27 * 96__ <br><br>
**Conv2** : 256 5 * 5 filters, with stride = 1, pad = 2 ----> Output Dimension : __27 * 27 * 256__; Learnable parameters : __5 * 5 * 96 * 256__ <br>
**MaxPool2** : 3 * 3 filters, with stride = 2   ----> Output Dimension : __13 * 13 * 256__; Learnable parameters : __0__<br>
**Norm2** : Output Dimension : __13 * 13 * 256__ <br><br>
**Conv3** : 384 3 * 3 filters, with stride = 1, pad = 1 ----> Output Dimension : __13 * 13 * 384__; Learnable parameters : __3 * 3 * 256 * 384__ <br>
**Conv4** : 384 3 * 3 filters, with stride = 1, pad = 1 ----> Output Dimension : __13 * 13 * 384__; Learnable parameters : __3 * 3 * 384 * 384__ <br>
**Conv5** : 256 3 * 3 filters, with stride = 1, pad = 1 ----> Output Dimension : __13 * 13 * 256__; Learnable parameters : __11 * 11 * 3 * 96__ <br>
**MaxPool3** : 3 * 3 filters, with stride = 2   ----> Output Dimension : __6 * 6 * 256__; Learnable parameters : __0__ <br><br>
**Dense or FC** : 4096 units; Learnable parameters : __6 * 6 * 256 * 4096__ <br>
**Dense or FC** : 4096 units; Learnable parameters : __4096 * 4096__ <br>
**Dense or FC** : 4096 units; Learnable parameters : __num_classes * 4096__ <br>

ReLU, Dropout(0.5), BatchNorm, Learning rate Decay(when Val Accuracy Plateaus). <br>
----> **ZFNet** : <br>
Same architecture as AlexNet, but different filter sizes. Better hyperparameter optimization. <br>
----> **VGG Net(16/19)** : <br>
[[Conv] - [Conv] - [Pool]] * 3 - [[Conv] - [Conv] - [Conv] - [Pool]] * 2 - [FC] * 3 <br>
VGG proposed the use of 3 * 3 filter for all convolutions, and introduced many more layers. <br>
  *Why are smaller filters preferred?* <br>
    1. The intuition here is that the receptive field is the same for 3 3 * 3 conv layers and a 7 * 7 conv layer. This means that the output of the third convolutional is              affected by a range of 7 * 7 inputs from the input layer, which is similar to a single 7 * 7 Conv layer. Therefore, the number of feature maps collected would be higher          when using smaller filters and more number of layers. <br>
    2. Moreover, the number of learnable parameters would be much lesser in 3 3 * 3 Conv Layers, as compared to a 7 * 7 Conv layer. <br>
       In VGGNet, the Conv layers always preserve the dimensions, whereas the pooling reduces dimensions by half. As we go deeper, the number of filters are increased so as to          learn more feature maps. <br>
  Memory Used : ~96Mb / image; Total number of parameters : 138M <br>
----> **GoogLeNet** : <br>
    1. Creation of an **"Inception"** module, which is used repeatedly. <br>
    2. No Fully Connected Layers used. As we've seen in VGG, most of the parameters are in FC layers. Therefore, the number of learnable parameters is greatly reduced in                GoogLeNet. <br>
    The inception module performs **different sets of convolution and pooling**, but still maintains the same dimensions. The depth after each convolution varies.<br>
    At the end, all the outputs from the different sets of **filters are aggregated** to get a much larger depth. As a result, the depth can only increase.
    This presents some computational bottleneck if the depth at the input is high. <br>
    To prevent this, a 1 * 1 convolution is used on the input(with a smaller number of filters) so as to reduce the number of computations. This is also applied after pooling to      further reduce the depth of the pooling layer. This dimension reduction layers are called [**BottleNeck Layers**](https://github.com/abhijeetknayak/Deep-Learning/blob/master/CS-231N/Material/Dimension-Reduction-Inception.png)<br>
    As a result of these operations, some information(feature maps) may be lost, but this dimension reduction helps in learning the most relevant features, thereby reducing         redundancies in the model. <br>
    Auxiliary Outputs from lower layers - To keep the gradient flowing even in the lower layers(prevents Vanishing Gradient Problem). These are fed to FC Layers <br>
    No Fully Connected Layers at the end. <br>
----> **ResNet : Residual Networks**
Training a Deep Network is an optimization problem. Optimizing a deeper network is much harder than optimizing a shallow network.<br>
The intuition to building a resnet architecture is that the deep network should perform at least as good as the shallow network. If the ouput from the shallow layers is just mapped as an input to the top layers, the model should be able to learn weights for the top layers such that the deep model performs at least as good as the shallow layers. This mapping is termed as the **residual mapping**. The model can also learn zero weights for the top layers so that all the activations are zero, and the input from the shallow layer is just passed to the output. <br>
**BottleNeck Layer(1 * 1 Convolution)** can be used in ResNets as well to reduce the number of computations. <br>
Hyperparameters used are shown [here](https://github.com/abhijeetknayak/Deep-Learning/blob/master/CS-231N/Material/ResNet-Training-Hyperparameters.png) <br>
**Other Architectures** :
  1. [**Network in Network**](https://github.com/abhijeetknayak/Deep-Learning/blob/master/CS-231N/Material/Network-in-Network.png) - Each convolution layer uses an mlpconv layer
  2. [**ResNeXt**](https://github.com/abhijeetknayak/Deep-Learning/blob/master/CS-231N/Material/ResNeXt.png) - The width of residual blocks is increased(32 paths in one block)
  3. [**ResNet with Stochastic Depth**](https://github.com/abhijeetknayak/Deep-Learning/blob/master/CS-231N/Material/ResNet-Dropout.png) - Some residual blocks are randomly dropped during train time. Identical to Dropout
  4. [**DenseNet**](https://github.com/abhijeetknayak/Deep-Learning/blob/master/CS-231N/Material/Dense-Net.png) - Dense layers, where outputs are concatenated
  5. [**FractalNet**](https://github.com/abhijeetknayak/Deep-Learning/blob/master/CS-231N/Material/Fractal-Net.png) - Arranged as fractals. Gets rid of residual connections
  6. [**SqueezeNet**](https://github.com/abhijeetknayak/Deep-Learning/blob/master/CS-231N/Material/SqueezeNet.png) - "Fire" modules with "Squeeze" Conv layers(1 * 1 Conv) and "Expand" Layers(1 * 1, 3 * 3 and so on). Increases efficiency

## **Lecture 10 : [Recurrent Neural Networks](https://github.com/abhijeetknayak/Deep-Learning/blob/master/CS-231N/Material/RNN-State.png)** <br>
All networks until now had a single input and generated an output. Recurrent Networks are used in applications where the number of inputs and outputs varies, depending on the intended application. You could have 'one-to-many', 'many-to-one', or 'many-to-many' recurrent networks.<br>
Image Captioning, Sentiment Classification, Video Analytics, Text translation, etc are applications where RNNs are used. <br>
RNN forward Step :    __h<sub>t</sub> = tanh(x.Wx + h<sub>t-1</sub>.Wh + b)__ <br>
RNNs have a acertain number of hidden layers, which correspond to a particular tinme step. These are temporal networks(sequences) <br>
All the 'layers' are provided the same weight and bias parameters. <br>
__RNNs have a problem though__ : If the number of layers is very high(or hidden dimension is high), during backprop, the weight matrix is continuously multiplied over and over again to produce the downstream gradient. Unless values in W is almost 1, this would always lead to exploding or vanishing gradients, and the model wouldn't learn.<br>
To prevent this, you can either clip gradients to a threshold, or change the architecture. This is where LSTMs come in! <br>

**[Long Short Term Memory(LSTM)](https://github.com/abhijeetknayak/Deep-Learning/blob/master/CS-231N/Material/LSTM.png)** : <br>
Similar to the RNN, an LSTM keeps track of the hidden state of the cell. In addition to this, a cell state is maintained.<br>
LSTM maintains four different gates which determines if and how much of the input and the output should be passed on to the next timestep. <br>
The gates are as follows:
  1. **i Gate** - Input gate. Determines how much of the input should be stored in the cell state. Sigmoid Activation(between 0 and 1)
  2. **g Gate** - Determines what inputs should be stored in the cell state. Tanh activation(-1, 1) <br>
  3. **f gate** - Forget gate. Determines how much of the previous cell state should be erased. Sigmoid Activation(0, 1) <br>
  4. **o Gate** - Output gate. Determines how much of the current cell state should be sent as output. Sigmoid Activation(0, 1)<br>

The forward pass of an [LSTM cell](https://github.com/abhijeetknayak/Deep-Learning/blob/master/CS-231N/Material/LSTM-Diagram.png) : <br>
  ------>    __C<sub>t</sub> = C<sub>t-1</sub> * f + i * g__ -----> Element wise multiplications<br>
  ------>    __h<sub>t</sub> = o * tanh(C<sub>t</sub>)__ ----> Again, Element wise multiplication<br>
  
  The advantage of this architecture is that, during backpropagation an element wise multiplication with the gates determines the downstream gradients. <br>
  As all of these gates are results of activations functions that output values in a small range, there is no way the gradients can explode. <br>
  This architecture helps in provinding a gradient flow from the last cell to the first, which elinminates the vanishing gradients problem too. <br>
<br>
**Lecture 11 : Detection and Segmentation** <br>
*Different Applications in which Deep Learning is used(in Visual Recognition)*:
  1. **Semantic Segmentation** : Using a set of downsampling layers + a set of Upsampling Layers to make sure the output is the same size as that of the input. Upsampling is achieved by performing Transposed Convolution. <br>
  2. **Classification + Localization** : Network predicts class + (x, y, w, h). Two different FC Layers(2 Losses, but weighted addition depending on the accuracy you need).
  This is called Multi-Task loss, and uses a weighting hypr-parameter. This parameter defines the loss function itself, so optimizing can be difficult.
  (Pose Estimation, Bounding box generation, etc) <br>
  3. **Object Detection** : Tricky because you don't know how many objects to expect. Sliding window approach is too inefficient. Region proposals work better!
  **[R-CNN]()** uses a region proposal algorithm to generate crops that most likely contain objects.(Transform regions to same size -> Pass through network)
  As R-CNN is slow during training, as well as while inferring, Fast R-CNN is used. It does exactly the same region proposals as R-CNN, but on a feature map generated by a CNN.
  Fast R-CNN has its bottleneck as the region proposals! [Faster R-CNN]() saves the day! It uses a region proposal network to learn where regions are most likely to be. Multiple losses again! <br>
  A parallel stream : *[Detection without proposals]()*<br>
  4. **Instance Segmentation** : Instances of similar objects are seperated. [Mask R-CNN]() additionally predicts a mask for the object along with the class and the bounding box dimensions.<br>
  
## **Lecture 12 : Visualizing network outputs** <br>
  1. Weights of the first layer of a CNN can be visualized. As the inner product is maximized when the image(or a part of it) matches the weight filters, visualizing the weights can give us a rough idea of what the filter is looking for in the image. <br>
  2. Intermediate layer weights can also be visualized this way, but they're not very insightful. As these weights have a different number of channels, we would have to visualize them as sets of grayscale images.<br>
  3. Fully Connected layer outputs can also be visualized. Nearest Neighbor visualizations can be used here. Nearest neighbor approach in pixel space may be ambiguous. Computing nearest neighbors in feature space results in images that have similar features. <br>
  4. FC Layer - Dimensionality Reduction using t-SNE(T Distributed Stochastic Neighbor embeddings). Using the n-Dimensional output of the Fc layer, t-SNE reduces it to a dimensional value(a coordinate). When these coordinates are visualized, clusters of images are created according to their class. <br>
  5. Visualizing activation maps can help us understand which part of the image causes a certain kind of activation in a particular layer of the CNN. <br>
  6. Maximally Activating patches : For any channel in an activation volume, find image patches that cause maximum activations in that channel. If an activation from a higher layer is used, it has a larger receptive field. The image patch generated would be bigger in this case. <br>
  7. Occlusion Experiments : Occlude parts of images, and then create a probability distribution over the range of the entire image. Parts of the image which are important for the image to be correctly classified will have low probabilities in the new probability distribution.<br>
  8. Saliency maps : Tells us which parts of the image are under consideration to make a correct classification decision. <br>
  9. Guided backpropagation : <br>
  10. Class Visualization with Gradient ascent : Start with a zero image and find image gradient that maximizes a class score. Then, perform a gradient ascent so as to maximize the score. This generates a new image that maximizes the class score. The regularization makes the image look relatively natural. Other implicit regularizers used are gaussian blurring, image jitter(shift the whole image by some pixels), etc. <br>
  11. Fooling Images : Maximizing the class score for a particular target class and performing gradient ascent so as to generate an image which is classified as the target class. <br> 
## **Lecture 13 : [Generative Models](https://github.com/abhijeetknayak/Machine-Learning/blob/master/CS-231N/Material/Generative-Models.png) : Unsupervised Learning Techniques** <br>
* **PixelRNN and PixelCNN** : Sequentially generates each pixel of the image. This is an explicit density function, where p(x) can actually be obtained. Its very slow though because of the sequential process of generation
* **Variational Autoencoders**
  * [Autoencoders](https://github.com/abhijeetknayak/Machine-Learning/blob/master/CS-231N/Material/Autoencoder.png) : Encoder-Decoder combination that derives features from the input and then reconstructs the inputs data via the decoder.
  * By defining a latent variable Z, a [lower bound](https://github.com/abhijeetknayak/Machine-Learning/blob/master/CS-231N/Material/VAE-Math.png) is obtained on the log likelihood. The intractable terms are always >= 0
  * While generating new data, sample Z from the prior and pass this through the decoder network. Sample data from p(x|z). Variations in the latent variable 'Z' results in variations in the generated data
  * Pros and Cons [here](https://github.com/abhijeetknayak/Machine-Learning/blob/master/CS-231N/Material/VAE-Pros-Cons.png)
* **[Generative Adversarial Networks](https://github.com/abhijeetknayak/Machine-Learning/blob/master/CS-231N/Material/GANs.png)** :
  * Implicit Density estimation. Learn through a min-max game
  * GANs use a Generator network that tries to fool the discriminator by generating real-looking images. The discriminator tries to distinguish between the real and fake images
  * The [objective function](https://github.com/abhijeetknayak/Machine-Learning/blob/master/CS-231N/Material/GAN-Objective-Function.png) of a GAN is : 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://i.upmath.me/svg/min_%7B%5Ctheta_g%7D%20max_%7B%5Ctheta_d%7D%5BE_%7Bx%20%5C%20%3C-%20%5C%20p_%7Bdata%7D%7Dlog(D_%7B%5Ctheta_d%7D(x))%20%5C%20%2B%20%5C%20E_%7Bz%20%5C%20%3C-%20%5C%20p(z)%7Dlog(1%20-%20D_%7B%5Ctheta_d%7D(G_%7B%5Ctheta_g%7D(z)))%5D" alt="min_{\theta_g} max_{\theta_d}[E_{x \ &lt;- \ p_{data}}log(D_{\theta_d}(x)) \ + \ E_{z \ &lt;- \ p(z)}log(1 - D_{\theta_d}(G_{\theta_g}(z)))]" />
  * While [training a GAN](https://github.com/abhijeetknayak/Machine-Learning/blob/master/CS-231N/Material/Gan-Training.png), the discriminator is trained on noise(for a few steps). Then Gradient Ascent is applied on the generator as shown [here](https://github.com/abhijeetknayak/Machine-Learning/blob/master/CS-231N/Material/GAN-grad-Ascent.png).

## **Lecture 14 : Deep Reinforcement Learning**
* Markov Decision Process : Formalizing the RL problem

  

  







  

      
      
