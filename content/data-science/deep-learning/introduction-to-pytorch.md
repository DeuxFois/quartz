```python
# Importing both packages
import torch.nn as nn
import torch.optim as optim
```

## nn.Module
```Python
class MySimpleMLP(nn.Module):

def __init__(self, in_size, hidden_units, out_size):
	super(MySimpleMLP, self).__init__()
	# Let us now define the linear layers we need:
	self.fc1 = nn.Linear(in_size, hidden_units)
	self.fc2 = nn.Linear(hidden_units, hidden_units)
	self.fc3 = nn.Linear(hidden_units, out_size)
	
	# We have also to define what is the forward of this module:
	def forward(self, x):
		h1 = nn.functional.relu(self.fc1(x))	
		h2 = nn.functional.relu(self.fc2(h1))
		out = self.fc3(h2)
		return out
```
```Python
# Instantiate a 3 layers MLP (with 10 hidden neurons in each layer) that computes a scalar quantity from a scalar input.
my_net = MySimpleMLP(1, 10, 1)

  

# We usually give batches of values to nn modules, where the first dimension is the dimension of the batch while
#the others must represent your data (here a scalar).
x = torch.arange(-2, 2, .1).unsqueeze(1)

  

# Detach is used to detach the tensor from its computation graph, it is required to
#be able to convert the tensor as numpy matrix (which is implicitely made when you plot a tensor).
y = my_net(x).detach()
plt.plot(x, y)
```

## nn.Sequential
```Python
class MyElegantSimpleMLP(nn.Module):
def __init__(self, in_size, hidden_units, out_size):
	super(MyElegantSimpleMLP, self).__init__()
	
	self.net = nn.Sequential(nn.Linear(in_size, hidden_units), nn.ReLU(),
							nn.Linear(hidden_units, hidden_units), nn.ReLU(),
							nn.Linear(hidden_units, out_size))

# We have also to define what is the forward of this module:
	def forward(self, x):
		out = self.net(x)

return out
```

## Optimizer
```Python
# We create an object from the class SGD that will make the updates for us.
sgd_optimizer = optim.SGD(params=my_net.parameters(), lr=.001)

  

# Let's do some learning steps with randomly generated x values:

for i in range(5000):
	x = torch.randn(100, 1)
	y = x**2
	y_pred = my_net(x)
	
	# We have to set all the grad values of the parameters of our net to zero, we can use zero_grad instruction
	sgd_optimizer.zero_grad()
	# Let's compute the loss and the gradients with respect to it.
	loss = ((y - y_pred)**2).mean()
	loss.backward()
	# And now we update the parameters:
	sgd_optimizer.step()
```

## Real example
```Python
net = nn.Sequential(nn.Linear(2, 50), nn.ReLU(),
					nn.Linear(50, 50), nn.ReLU(),
					nn.Linear(50, 1), nn.Sigmoid())
optimizer = optim.Adam(net.parameters(), lr=.01)

def loss_func(y_hat, y):
	return nn.BCELoss()(y_hat, y)

train_loss = [] # where we keep track of the loss
train_accuracy = [] # where we keep track of the accuracy of the model
iters = 1000 # number of training iterations

Y_train_t = torch.FloatTensor(Y_train).reshape(-1, 1) # re-arrange the data to an appropriate tensor

for i in range(iters):
	X_train_t = torch.FloatTensor(X_train)
	y_hat = net(X_train_t) # forward pass
	loss = loss_func(y_hat, Y_train_t) # compute the loss
	loss.backward() # obtain the gradients with respect to the loss
	optimizer.step() # perform one step of gradient descent
	optimizer.zero_grad() # reset the gradients to 0
	y_hat_class = np.where(y_hat.detach().numpy()<0.5, 0, 1) # we assign an appropriate label based on the network's prediction
	accuracy = np.sum(Y_train.reshape(-1,1)==y_hat_class) / len(Y_train) # compute final accuracy
	train_accuracy.append(accuracy)
	train_loss.append(loss.item())

```