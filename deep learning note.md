# deep learning note



## 1th week

### 1.1 different types of functions.  

| method             | output              |
| ------------------ | ------------------- |
| regression         | scalar              |
| classification     | one or more classes |
| structure learning | Learn to create     |



### 1.2 common process of machine learning 

- create a model ,such as y=b+wx.

- define **Loss**(**L($\theta$)**) from training data (**Loss**：![](https://latex.codecogs.com/svg.latex?\color{white}|y-\hat{y}|,(y-\hat{y})^2),cross-entropy......）.

- optimization : find the lowest best ![](https://latex.codecogs.com/svg.latex?\color{white}w^*,b^*=\arg\min_{w,b}L).

   (define your own hyperparameters and use **Gradient Descent**,a process of training)

- prediction.	



### 1.3 Sigmoid and Relu function (consist the neuron network)

- the key to predict the trend of  the problems we researched is making a line that consist of many known lines ,and now we use basic **Sigmoid** line to simulate the trend of problems.

	![](https://latex.codecogs.com/svg.latex?\color{white}sigmoid:y=c\frac{1}{1+e^-(b+wx)})
	

- from some perspectives,maybe all the trips come from this function,and relu function.

#### 1.3.1 constructure the basic trend line (1 feature)

for a example ,we wanna predict the relationsip between  the square  and the house price.we can easily discover the relationship is y(price)=b+wx(square).But many problems are more difficult because it doesn't easily fit the direct proportional relationship.

So,as mentioned above, we use many many sigmoid function to simulate the line.

![WechatIMG125](/picture/1week/WechatIMG125.jpg)

in 1 feature rela,we find  the real line = sum of a set of sigmoid +constant.

​               ![](https://latex.codecogs.com/svg.latex?\color{white}y=b+\sum_{i}sigmoid(b_i+w_ix_1))

- i is a hyperparameter up to yourself.

the picture followed can describe the process:

![WechatIMG126](/picture/1week/WechatIMG126.jpg)

then, we define the loss function and use **Gradient Descent** to find the lowest ![](https://latex.codecogs.com/svg.latex?\color{white}w^*,b^*),and it represents the best fitted result predicting the trend.



#### 1.3.2 more features

(1) firstly, we make a basic direct proportional function with more features like ![](https://latex.codecogs.com/svg.latex?\color{white}y=b+$\sum_jw_jx_j$).

| 1 feature | more features        |
| --------- | -------------------- |
| y=b+wx    | ![](https://latex.codecogs.com/svg.latex?\color{white}y=b+$\sum_jw_jx_j) |

for each sigmoid,we input every reslut of b+$\sum_j w_j x_j$,we define this result as r.

also,we input each sigmoid function every r.

the equation is here:

​                                ![](https://latex.codecogs.com/svg.latex?\color{white}y=b+\sum_{i}c_isigmoid(b_j+\sum_{j}w_{ij}x_j))

we can describe this equation as picture as followed:

![WechatIMG127](/picture/1week/WechatIMG127.jpg)

We can easily descibe this formula:

​                                             y=b+**$c^T$** · $\sigma$( r )

- $\sigma(r)$ is sigmoid(r)



(2) define a **LOSS** function ,and make **Gradient Descent**.



