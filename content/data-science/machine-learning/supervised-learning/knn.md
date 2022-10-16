---
tags:
  - supervised-learning
  - discriminative
  - discrete value
---
The K-Nearest Neighbours (KNN) algorithm is one of the simplest supervised machine learning algorithms that is used to solve both classification and regression problems.  
KNN is also known as an **instance-based model** or a lazy learner because it doesn’t construct an internal model.


# KNN Classification

Let’s learn how to classify data using the knn algorithm. Suppose we have two classes circle and triangle.

Below is the representation of data points in the feature space.

![](https://miro.medium.com/max/679/1*avsW4gXeRsCwT4GYMbNWZQ.png)

Now we have to predict the class of new query point (star shape shown in the figure). We have to predict whether the new data point (star shape) belongs to class circle or traingle.

![](https://miro.medium.com/max/700/1*9PjV-hIWQvk65HuyAet1yQ.png)


First, we have to determine k value. k denotes the number of neighbors.  
Second, we have to determine the nearest k neighbors based on distance.

This algorithm finds the k nearest neighbor, and classification is done based on the majority class of the k nearest neighbors.  
Here in this example, I have shown the nearest neighbors inside the blue oval shape. So the majority class belongs to “Circle”, so the query point belongs to class circle.

![](https://miro.medium.com/max/691/1*JXYc7StDsGQsoLe-kXgnGQ.png)

Predicting the class of new query point [Image by Author]

Now, comes the important point.  
1. How to find the optimum k value?  
2. How to find the k nearest neighbors?

# How to find the optimum k value?

Choosing the k value plays a significant role in determining the efficacy of the model.

1.  If we choose k =1 means the algorithm will be sensitive to outliers.
2.  If we choose k= all (means the total number of data points in the training set), the majority class in the training set will always win. Since knn classifies class based on majority voting mechanism. So all the test records will get the same class which is the majority class in the training set.
3.  Generally, k gets decided based on the square root of the number of data points.
4.  Always use k as an odd number. Since KNN predicts the class based on the majority voting mechanism, the chances of getting into a tie situation will be minimized.
5.  We can also use an error plot or accuracy plot to find the most favorable K value. Plot possible k values against error and find the k with minimum error and that k value is chosen as the favorable k value.

# How to find the k nearest neighbors?

There are different techniques to find the k nearest neighbors.

-   Euclidean distance
-   Manhattan distance
-   Minkowski distance

One of the most used techniques is the euclidean distance.