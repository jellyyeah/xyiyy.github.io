---
title: standford-cs231n-assignment1
date: 2017-02-27 16:54:08
tags: cs231n
categories:
- CNN

---

作业一前面的课程视频和notes早就看完了，这次抽空把assignment1做了，感觉作用挺大的。

完整代码[见此](https://github.com/xyiyy/cs231n_assignment/tree/master/assignment1)


<!--more-->


## KNN


> *Complete and hand in this completed worksheet (including its outputs and any supporting code outside of the worksheet) with your assignment submission. For more details see the [assignments page](http://vision.stanford.edu/teaching/cs231n/assignments.html) on the course website.*
>
> The kNN classifier consists of two stages:
>
> - During training, the classifier takes the training data and simply remembers it
> - During testing, kNN classifies every test image by comparing to all training images and transfering the labels of the k most similar training examples
> - The value of k is cross-validated
>
> In this exercise you will implement these steps and understand the basic Image Classification pipeline, cross-validation, and gain proficiency in writing efficient, vectorized code.

首先是knn的部分，这里用knn其实就相当于拿到一张新的待预测的图片，然后去和训练集中的所有的已有的图片去进行比较，然后差异最小的k张图片中，频次出现最高的label作为本次预测的label。  

这部分的代码也比较简单，对于计算差值的部分，就是分别写一个两重，一重循环，以及不用循环的计算loss的函数，然后另外再补充一个predict的函数。  

其中，在不用循环的那个loss函数，还是有些trick的，第一次接触到，考虑到那个式子每行每列的平方项提出，就会发现其基本的部分就是一个乘法而已
```python
#########################################################################
# TODO:                                                                 #
# Compute the l2 distance between all test points and all training      #
# points without using any explicit loops, and store the result in      #
# dists.                                                                #
#                                                                       #
# You should implement this function using only basic array operations; #
# in particular you should not use functions from scipy.                #
#                                                                       #
# HINT: Try to formulate the l2 distance using matrix multiplication    #
#       and two broadcast sums.                                         #
#########################################################################
dists = -2 * np.dot(X,self.X_train.T)
dists = dists + np.sum(X**2,axis=1,keepdims=True)
dists = dists + np.sum(self.X_train**2,axis = 1)

#########################################################################
#                         END OF YOUR CODE                              #
#########################################################################
```

***

## SVM

> *Complete and hand in this completed worksheet (including its outputs and any supporting code outside of the worksheet) with your assignment submission. For more details see the [assignments page](http://vision.stanford.edu/teaching/cs231n/assignments.html) on the course website.*
>
>In this exercise you will:
>
>- implement a fully-vectorized **loss function** for the SVM
>- implement the fully-vectorized expression for its **analytic gradient**
>- **check your implementation** using numerical gradient
>- use a validation set to **tune the learning rate and regularization** strength
>- **optimize** the loss function with **SGD**
>- **visualize** the final learned weights

这个部分主要是要对svm的loss函数进行一个求导的步骤，其余部分都很简单。

首先，这里svm的loss函数的定义如下：
$$L=\underbrace{\frac{1}{N} \sum\_{i}^{N}L\_{i}}\_{data\ loss} + \underbrace{\frac{\lambda}{2} \sum\_{k} \sum\_{l}w\_{kl}^2}\_{regularization\ loss}$$
$$ L\_{i} = \sum\_{j \neq y\_{i}} max\left(0, w\_{j}^{T}x\_{i} - w\_{y\_{i}}^{T}x\_{i}+\Delta \right) $$  
实际中我们一般将X展成了行向量的形式，所以，其实在实际的式子中
$$L\_{i} = \sum\_{j \neq y\_{i}} max   \left(0,\sum\_{k}{x\_{ik}w\_{kj}}-\sum\_{k}{x\_{ik}w\_{ky\_{i}}+\Delta}\right)$$  
整体分析一下，对这个式子求导还是非常简单的，首先对于正则化的部分，就不提了，二次方直接求就好。对于data loss部分，对于小于0的，那部分对应的导数是0，其次，剩下部分的导数，每次都是作用在对应的整列上，为导数是\\(x\_{ik}\\)。然后最后把data loss和regularization loss两部分的导数求个和即可。最终得到如下的代码
```python
  #############################################################################
  # TODO:                                                                     #
  # Implement a vectorized version of the structured SVM loss, storing the    #
  # result in loss.                                                           #
  #############################################################################
  num_train = X.shape[0]
  scores = X.dot(W)
  margin = np.maximum(0,scores - scores[np.array(range(num_train)),y].reshape(num_train,1) + 1)
  margin[np.array(range(num_train)),y] = 0
  loss = np.sum(margin) / num_train
  loss += 0.5 * reg * np.sum(W ** 2)
  #############################################################################
  #                             END OF YOUR CODE                              #
  #############################################################################
  #############################################################################
  # TODO:                                                                     #
  # Implement a vectorized version of the gradient for the structured SVM     #
  # loss, storing the result in dW.                                           #
  #                                                                           #
  # Hint: Instead of computing the gradient from scratch, it may be easier    #
  # to reuse some of the intermediate values that you used to compute the     #
  # loss.                                                                     #
  #############################################################################
  margin[margin > 0] = 1.0
  sum_row = np.sum(margin,axis=1)
  margin[np.array(range(num_train)),y] -= sum_row
  dW += np.dot(X.T,margin) / num_train + reg * W
  #############################################################################
  #                             END OF YOUR CODE                              #
  #############################################################################
```

***

## Softmax
>*Complete and hand in this completed worksheet (including its outputs and any supporting code outside of the worksheet) with your assignment submission. For more details see the [assignments page](http://vision.stanford.edu/teaching/cs231n/assignments.html) on the course website.*
>
>This exercise is analogous to the SVM exercise. You will:
>
>- implement a fully-vectorized **loss function** for the Softmax classifier
>- implement the fully-vectorized expression for its **analytic gradient**
>- **check your implementation** with numerical gradient
>- use a validation set to **tune the learning rate and regularization** strength
>- **optimize** the loss function with **SGD**
>- **visualize** the final learned weights

难的主要还是在向量化，求导的地方。其loss function如下：
$$L=\frac{1}{N}\sum\_{i}^{N}{L\_{i}}+\frac{\lambda}{2}\sum\_{j}\sum\_{k}w\_{jk}$$
$$L\_{i}
＝ln\frac{e^{S\_{iy\_{i}}}}{\sum\_{j}{e^{S\_{ij}}}}\\ \\
=-S\_{iy\_{i}}+ln\sum\_{j}{e^{S\_{ij}}}$$
$$S\_{ij}=\sum\_{k}{x\_{ik}w\_{kj}}$$
这里求导的话，regularization loss部分就不讲了。

$$\frac{\partial L}{\partial S\_{ij}}=
\begin{cases}
\frac{1}{N}\frac{e^{S\_{ij}}}{\sum\_{j}{e^{S\_{ij}}}}, & \text{if} \ j \neq y\_{i} \\\\
\frac{1}{N}\left(\frac{e^{S\_{ij}}}{\sum\_{j}{e^{S\_{ij}}}} - 1\right), & \text{if} \ j = y\_{i}
\end{cases}$$
这个式子里的减1的判断，实现向量化还是很容易的。然后是剩下的部分
$$\begin{align}
\frac{\partial S}{\partial W}  
& = \frac{\partial tr(XW)}{\partial W} \\\\
& = \frac{\partial tr(WX)}{\partial W} \\\\
& = X^{T}
\end{align}
$$
从而，可以得到，对于data loss部分，其导数为
$$
\frac{\partial L}{\partial W}
= \frac{\partial S}{\partial W}\frac{\partial L}{\partial S}
$$
Softmax_loss_vectorized部分的代码如下所示
```python
#############################################################################
# TODO: Compute the softmax loss and its gradient using no explicit loops.  #
# Store the loss in loss and the gradient in dW. If you are not careful     #
# here, it is easy to run into numeric instability. Don't forget the        #
# regularization!                                                           #
#############################################################################
scores = X.dot(W)
num_train = X.shape[0]
num_classes = W.shape[1]
scores = scores - np.max(scores,axis=1,keepdims=True)
row_sum = np.sum(np.exp(scores),axis=1,keepdims=True)
loss += np.sum(-np.log(np.exp(scores[range(num_train),y]) / row_sum.T))
loss = loss / num_train
loss += 0.5 * reg * np.sum(W ** 2)
scores = np.exp(scores) / row_sum
scores[range(num_train),y] -= 1
dW = np.dot(X.T,scores)/num_train + reg * W
#############################################################################
#                          END OF YOUR CODE                                 #
#############################################################################
```

***
***

## two layer net
>In this exercise we will develop a neural network with fully-connected layers to perform classification, and test it out on the CIFAR-10 dataset.  

forward没什么好说的
```python
#############################################################################
# TODO: Perform the forward pass, computing the class scores for the input. #
# Store the result in the scores variable, which should be an array of      #
# shape (N, C).                                                             #
#############################################################################
X1 = X.dot(W1) + b1
X2 = np.maximum(X1,0)
scores = X2.dot(W2) + b2
#############################################################################
#                              END OF YOUR CODE                             #
#############################################################################
```
主要还是bp求梯度的部分。这里要求使用的是Softmax classification loss function。所以每层的输出及其对应的公式如下：
$$X\_{1} = XW\_{1} + b\_{1} \\\\
X\_{2} = max(X\_{1},0) \\\\
scores = X\_{2}W\_{2}+b\_{2}
$$
利用Softmax里推过的式子
$$\frac{\partial L}{\partial S\_{ij}}=
\begin{cases}
\frac{1}{N}\frac{e^{S\_{ij}}}{\sum\_{j}{e^{S\_{ij}}}}, & \text{if} \ j \neq y\_{i} \\\\
\frac{1}{N}\left(\frac{e^{S\_{ij}}}{\sum\_{j}{e^{S\_{ij}}}} - 1\right), & \text{if} \ j = y\_{i}
\end{cases}$$
可以直接得到\\(W\_{2}\\)，\\(b\_{2}\\)以及\\(X\_{2}\\)的梯度
$$\frac{\partial L}{\partial W\_{2}} = \frac{\partial S}{\partial W\_{2}}\frac{\partial L}{\partial S}
=X\_{2}^{T}\frac{\partial L}{\partial S} \\\\
\frac{\partial L}{\partial X\_{2}} = \frac{\partial L}{\partial S}\frac{\partial S}{\partial X\_{2}}=\frac{\partial L}{\partial S}W\_{2}^{T}\\\\  
\frac{\partial L}{\partial b\_{2}}=\underbrace{[1,...,1]}\_{N个1}\frac{\partial L}{\partial S}
$$
利用chain rule，然后向前推,对于max函数，小的那个是0，大的偏导是1。也就是将\\(\frac{\partial L}{\partial X\_{2}}\\)中与\\(X\_{1}<0\\)下标对应的值置为0，其余不变赋值给\\(\frac{\partial L}{\partial X\_{1}}\\)
最后再对\\(X\_{1}\\)的式子求一下导，得到
$$
\frac{\partial L}{\partial W\_{1}}
=\frac{\partial X\_{1}}{\partial W\_{1}}\frac{\partial L}{\partial X\_{1}}
=X^{T}\frac{\partial L}{\partial X1}\\\\
\frac{\partial L}{\partial b\_{1}}
=\frac{\partial X\_{1}}{\partial b\_{1}}\frac{\partial L}{\partial X\_{1}}
=\underbrace{[1,...,1]}\_{N个1}\frac{\partial L}{\partial X\_{1}}
$$

```python
#############################################################################
# TODO: Finish the forward pass, and compute the loss. This should include  #
# both the data loss and L2 regularization for W1 and W2. Store the result  #
# in the variable loss, which should be a scalar. Use the Softmax           #
# classifier loss. So that your results match ours, multiply the            #
# regularization loss by 0.5                                                #
#############################################################################
num_train = X.shape[0]
num_class = scores.shape[1]
scores -= np.max(scores,axis=1,keepdims=True)
loss = np.sum(-np.log(np.exp(scores[range(num_train),y]) / np.sum(np.exp(scores),axis=1))) / num_train
loss += 0.5 * reg * (np.sum(W1 ** 2) + np.sum(W2 ** 2))
#############################################################################
#                              END OF YOUR CODE                             #
#############################################################################

# Backward pass: compute gradients
grads = {}
#############################################################################
# TODO: Compute the backward pass, computing the derivatives of the weights #
# and biases. Store the results in the grads dictionary. For example,       #
# grads['W1'] should store the gradient on W1, and be a matrix of same size #
#############################################################################
dW2 = np.zeros(W2.shape)
dW1 = np.zeros(W1.shape)
row_tot = np.sum(np.exp(scores),axis=1,keepdims=True)
dL = np.exp(scores) / row_tot
dL[range(num_train),y] -= 1
dL /= num_train
dW2 = np.dot(X2.T,dL) + reg * W2
db2 = np.dot(np.ones((1,num_train)),dL)

dL_dX2 = np.dot(dL,W2.T)
dL_dX1 = dL_dX2
dL_dX1[X1 <= 0] = 0
dX1_dW1 = X.T
dW1 = np.dot(dX1_dW1,dL_dX1) + reg * W1
db1 = np.dot(np.ones((1,num_train)),dL_dX1)

grads['W1'] = dW1
grads['W2'] = dW2
grads['b1'] = db1
grads['b2'] = db2
#############################################################################
#                              END OF YOUR CODE                             #
#############################################################################
```
调参部分
```python
best_net = None # store the best model into this

#################################################################################
# TODO: Tune hyperparameters using the validation set. Store your best trained  #
# model in best_net.                                                            #
#                                                                               #
# To help debug your network, it may help to use visualizations similar to the  #
# ones we used above; these visualizations will have significant qualitative    #
# differences from the ones we saw above for the poorly tuned network.          #
#                                                                               #
# Tweaking hyperparameters by hand can be fun, but you might find it useful to  #
# write code to sweep through possible combinations of hyperparameters          #
# automatically like we did on the previous exercises.                          #
#################################################################################
batch_size = [1000]
learning_rate = [1.4e-3,1.5e-3,1.6e-3]
reg = [1e-4,1e-3,1e-2]
hidden_size = 300
best_acc = -1
for bs in batch_size:
    for lr in learning_rate:
        for rg in reg:
            net = TwoLayerNet(input_size, hidden_size, num_classes)
            stats = net.train(X_train,y_train,X_val,y_val,num_iters=2000,batch_size=bs,learning_rate=lr,
                             learning_rate_decay=0.95,reg=rg,verbose=True)
            val_acc = (net.predict(X_val) == y_val).mean()
            print "bs : %f , lr  :  %f , rg : %f ==> %f" %(bs,lr,rg,val_acc)
            if val_acc > best_acc:
                best_net = net
                best_acc = val_acc

#################################################################################
#                               END OF YOUR CODE                                #
#################################################################################
```

***

## features
神经网络部分的参数如下
```python
################################################################################
# TODO: Train a two-layer neural network on image features. You may want to    #
# cross-validate various parameters as in previous sections. Store your best   #
# model in the best_net variable.                                              #
################################################################################
regularization = [2.5e-4,5e-4,7.5e-4,1e-3,1.25e-3,1.5e-3]
learning_rate = [2e-1,3e-1,4e-1]
best_acc = -1
params = [(x,y)for x in learning_rate for y in regularization]
for lr,reg in params:    
    net = TwoLayerNet(input_dim,hidden_dim,num_classes)
    net.train(X_train_feats,y_train,X_val_feats,y_val,num_iters=1500,learning_rate=lr,reg=reg,batch_size=300)
    train_acc = np.mean(net.predict(X_train_feats) == y_train)
    val_acc = np.mean(net.predict(X_val_feats) == y_val)
    print "lr : %f reg : %f ===>  ta : %f  va : %f" %(lr,reg,train_acc,val_acc)
    if val_acc > best_acc:
        best_acc,best_net = val_acc,net

################################################################################
#                              END OF YOUR CODE                                #
################################################################################
```
