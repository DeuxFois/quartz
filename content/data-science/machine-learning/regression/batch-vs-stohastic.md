# Batch vs Stochastic
![](_resources/Pasted%20image%2020220701233150.png)

# Batch Gradient Descent

> BGD is a variation of the gradient descent algorithm that calculates the error for each eg in the training datasets, but only updates the model after all training examples have been evaluated.

![](https://miro.medium.com/max/640/1*Ouc8p_YbjY5m2mMIzOgnLw.png)

One cycle through entire training datasets is called a training epoch. Therefore, it is often said that BGD performs model updates at the end of each training epoch.

**_Advantages_**:

-   It is more computationally efficient.
-   It is a learnable parameter : whenever we are trying to calculate a new weight, we are trying to consider all the data which is available to us based on the summation of the loss. So, we are trying to find out or derive the new value of the weight / bias , which is a learnable parameter.

**_Disadvantages_**:

-   **Memory consumption is too high**
-   If memory consumption is too high, we can say that thr computation will be high and calculation will be very slow and so the optimization will be slower as compared to any other optimizer.

# Mini Batch Gradient descent (MGD)

> MGD is a variation of the gradient descent algorithm that splits the training datasets into small batches that are used to calculate model error and update model coefficients.
![](https://miro.medium.com/max/671/1*_ctmL9Ya0DpppDFbiYa7VQ.png)

**_Advantages_**:

-   The model update frequency is higher than BGD: In MGD, we are not waiting for entire data, we are just passing 50 records or 200 or 100 or 256, then we are passing for optimization.
-   The batching allows both efficiency of not having all training data in memory and algorithms implementations. We are controlling memory consumption as well to store losses for each and every datasets.
-   The batches updates provide a computationally more efficient process than SGD.

**_Disadvantages_**:

-   No guarantee of convergence of a error in a better way.
-   Since the 50 sample records we take , are not representing the properties (or variance) of entire datasets. Do, this is the reason that we will not be able to get an convergence i.e., we won’t get absolute global or local minima at any point of a time.
-   While using MGD, since we are taking records in batches, so, it might happen that in some batches, we get some error and in dome other batches, we get some other error. So, we will have to control the learning rate by ourself , whenever we use MGD. If learning rate is very low, so the convergence rate will also fall. If learning rate is too high, we won’t get an absolute global or local minima. So we need to control the learning rate.

**Note**:If the batch size = total no. of data, then in this case, BGD = MGD.

# Stochastic Gradient Descent

> SGD is a variation of the gradient descent that calculates the error and updates the model for each record in the training datasets.


**_Points_**: Since, in SGD records are send one by one so, if talking about minima, we will be able to get multiple minima points as there will be minima for each records and it will look like this as shown below:

![](https://miro.medium.com/max/700/1*tLTWgad8BUisFKtXB8MgCQ.png)

It keeps on fluctuating. And we will fall inside a local minima at any point of time.


**_Advantages_**:

-   For every record, we are updating the weights, so we are learning
-   Weight updates is faster.
-   Loss function is not suppose to wait for the entire datasets to calculate itself.
-   Even optimizer is not suppose to wait for entire datasets to calculate itself.
-   Memory consumptions will also be low.
-   SGD is faster than MGD and BGD.



**_Disadvantages_**:

-   It is having huge oscillation. So, SGD will always vary from one point to another for each and every datasets. Hence, its tough to get an absolute minima. And we will end up getting a multiple minima points.
-   We need to control the learning rate: if learning rate is too high, it may be possible that some other dataset may not show you the same properties, again, learning rate effect in SGD will be little but lesser as compare to the BGD and MGD.

# Comparison 
If we compare all three optimizer, then every optimizer has its own advantages and disadvantages ,we can’t come to conclusions which optimizer is best, it totally depends on datasets.


![](https://miro.medium.com/max/700/1*9calCrrqS9opiytuA--7AA.png)