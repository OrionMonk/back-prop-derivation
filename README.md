
## Understanding and Deriving the Backpropagation algorithm in a simple neural network
Let us try to understand the Backpropagation algorithm using an overly simplified Neural Network with just one hidden layer.

It has a bit of linear algebra and calculus involved but anyone who has atleast the basic knowledge about matrix multiplication and the chain rule of differentiation, should be able to follow through most of what will be discussed. 

We shall not delve too deep into proving the algorithm itself, but at the end, it will be very clear how the same principles could be used to intuitively undertand even larger networks with much more hidden layers and units.

## The model
For the sake of simplicity, we will be using a Neural Network with two inputs, two outputs and one hidden layer with 3 nodes. 

<img src="https://github.com/mizimo/Backprop-Derivation/raw/master/images/nn.jpg"/>

## Notations
Let us represent <img src="https://latex.codecogs.com/svg.latex?\overline&space;x" title="\overline x" /> as the input, <img src="https://latex.codecogs.com/svg.latex?\overline&space;a^{(1)}" title="\overline a^{(1)}" /> as the activation of the hidden layers, <img src="https://latex.codecogs.com/svg.latex?\overline&space;a^{(2)}" title="\overline a^{(2)}" /> as the output and <img src="https://latex.codecogs.com/svg.latex?\overline&space;b^{(1)}" title="\overline b^{(1)}" /> and <img src="https://latex.codecogs.com/svg.latex?\overline&space;b^{(2)}" title="\overline b^{(2)}" /> as the biases in the hidden layer and the output layer respectively.

<img src="https://latex.codecogs.com/svg.latex?\large&space;\overline&space;x&space;=&space;\begin{bmatrix}x_1\\x_2\end{bmatrix},\hspace{0.3cm}&space;\overline&space;a^{(1)}&space;=&space;\begin{bmatrix}a_1^{(1)}\\a_2^{(1)}\\a_3^{(1)}\\&space;\end{bmatrix},\hspace{0.3cm}&space;\overline&space;a^{(2)}&space;=&space;\begin{bmatrix}a_1^{(2)}\\a_2^{(2)}\end{bmatrix}" title="\large \overline x = \begin{bmatrix}x_1\\x_2\end{bmatrix},\hspace{0.3cm} \overline a^{(1)} = \begin{bmatrix}a_1^{(1)}\\a_2^{(1)}\\a_3^{(1)}\\ \end{bmatrix},\hspace{0.3cm} \overline a^{(2)} = \begin{bmatrix}a_1^{(2)}\\a_2^{(2)}\end{bmatrix}" />

<img src="https://latex.codecogs.com/svg.latex?\large&space;\hspace{0.3cm}&space;\overline&space;b^{(1)}&space;=&space;\begin{bmatrix}b_1^{(1)}\\b_2^{(1)}\\b_3^{(1)}\end{bmatrix}&space;,\hspace{0.3cm}&space;\overline&space;b^{(2)}&space;=&space;\begin{bmatrix}b_1^{(2)}\\b_2^{(2)}\end{bmatrix}" title="\large \hspace{0.3cm} \overline b^{(1)} = \begin{bmatrix}b_1^{(1)}\\b_2^{(1)}\\b_3^{(1)}\end{bmatrix} ,\hspace{0.3cm} \overline b^{(2)} = \begin{bmatrix}b_1^{(2)}\\b_2^{(2)}\end{bmatrix}" />


And let us represent weight matrices as <img src="https://latex.codecogs.com/svg.latex?\overline&space;\theta^{(1)}" title="\overline \theta^{(1)}" /> and <img src="https://latex.codecogs.com/svg.latex?\overline&space;\theta^{(2)}" title="\overline \theta^{(2)}" />.

<img src="https://latex.codecogs.com/svg.latex?\large&space;\overline&space;\theta^{(1)}&space;=&space;\begin{bmatrix}\theta_{11}^{(1)}&\theta_{12}^{(1)}\\&space;\theta_{21}^{(1)}&\theta_{22}^{(1)}\\&space;\theta_{31}^{(1)}&\theta_{32}^{(1)}\\&space;\end{bmatrix},\hspace{0.3cm}&space;\overline&space;\theta^{(2)}&space;=&space;\begin{bmatrix}\theta_{11}^{(2)}&\theta_{12}^{(2)}&\theta_{13}^{(2)}\\&space;\theta_{21}^{(2)}&\theta_{22}^{(2)}&\theta_{23}^{(2)}\\&space;\end{bmatrix}" title="\large \overline \theta^{(1)} = \begin{bmatrix}\theta_{11}^{(1)}&\theta_{12}^{(1)}\\ \theta_{21}^{(1)}&\theta_{22}^{(1)}\\ \theta_{31}^{(1)}&\theta_{32}^{(1)}\\ \end{bmatrix},\hspace{0.3cm} \overline \theta^{(2)} = \begin{bmatrix}\theta_{11}^{(2)}&\theta_{12}^{(2)}&\theta_{13}^{(2)}\\ \theta_{21}^{(2)}&\theta_{22}^{(2)}&\theta_{23}^{(2)}\\ \end{bmatrix}" />

Matrix multplication (Dot Product) will be represented using the <img src="https://latex.codecogs.com/svg.latex?\large&space;\times" title="\large \times" /> symbol, whereas element wise multiplication (Hadamard Product) will be represented using the <img src="https://latex.codecogs.com/svg.latex?\large&space;\odot" title="\large \odot" /> symbol. 

We shall use a general notation <img src="https://latex.codecogs.com/svg.latex?\large&space;\sigma" title="\large \sigma" /> for all the activation functions, and the symbol <img src="https://latex.codecogs.com/svg.latex?\large&space;\sigma'" title="\large \sigma'" /> for it's gradient.

## The forward Pass

Forward propagation in a neural network is nothing but a bunch of matrix calculations, and a few activation functions, applied at each layer. 
### Hidden Layer Values
For the hidden layer, we get the activation values <img src="https://latex.codecogs.com/svg.latex?\large&space;\overline&space;a^{(1)}" title="\large \overline a^{(1)}" /> using the following equations -

<img src="https://latex.codecogs.com/svg.latex?\large&space;\overline&space;a^{(1)}&space;=&space;\sigma(\overline&space;z^{(1)})&space;=&space;\sigma(\overline&space;\theta^{(1)}&space;\times&space;\overline&space;x&space;&plus;&space;\overline&space;b^{(1)})" title="\large \overline a^{(1)} = \sigma(\overline z^{(1)}) = \sigma(\overline \theta^{(1)} \times \overline x + \overline b^{(1)})" />

or more elaborately -

<img src="https://latex.codecogs.com/svg.latex?\large&space;\overline&space;a^{(1)}&space;=&space;\begin{bmatrix}a_1^{(1)}\\a_2^{(1)}\\a_3^{(1)}\\&space;\end{bmatrix}&space;=&space;\begin{bmatrix}\sigma(z_1^{(1)})\\\sigma(z_2^{(1)})\\\sigma(z_3^{(1)})\\&space;\end{bmatrix}&space;=&space;\sigma(\begin{bmatrix}z_1^{(1)}\\z_2^{(1)}\\z_3^{(1)}\\&space;\end{bmatrix})&space;=&space;\sigma(\overline&space;z^{(1)})" title="\large \begin{bmatrix}a_1^{(1)}\\a_2^{(1)}\\a_3^{(1)}\\ \end{bmatrix} = \sigma(\begin{bmatrix}z_1^{(1)}\\z_2^{(1)}\\z_3^{(1)}\\ \end{bmatrix}) = \begin{bmatrix}\sigma(z_1^{(1)})\\\sigma(z_2^{(1)})\\\sigma(z_3^{(1)})\\ \end{bmatrix}" />

<img src="https://latex.codecogs.com/svg.latex?\overline&space;z^{(1)}&space;=&space;\begin{bmatrix}z_1^{(1)}\\z_2^{(1)}\end{bmatrix}&space;=&space;\begin{bmatrix}\theta_{11}^{(1)}&\theta_{12}^{(1)}\\\theta_{21}^{(1)}&\theta_{22}^{(1)}\\\theta_{31}^{(1)}&\theta_{32}^{(1)}\\\end{bmatrix}&space;\times&space;\begin{bmatrix}x_1\\x_2\end{bmatrix}&space;&plus;&space;\begin{bmatrix}b_1^{(1)}\\b_2^{(1)}\\b_3^{(1)}&space;\end{bmatrix}&space;=&space;\overline&space;\theta^{(1)}&space;\times&space;\overline&space;x&space;&plus;&space;\overline&space;b^{(1)}" title="\overline z^{(1)} = \begin{bmatrix}z_1^{(1)}\\z_2^{(1)}\end{bmatrix} = \begin{bmatrix}\theta_{11}^{(1)}&\theta_{12}^{(1)}\\\theta_{21}^{(1)}&\theta_{22}^{(1)}\\\theta_{31}^{(1)}&\theta_{32}^{(1)}\\\end{bmatrix} \times \begin{bmatrix}x_1\\x_2\end{bmatrix} + \begin{bmatrix}b_1^{(1)}\\b_2^{(1)}\\b_3^{(1)} \end{bmatrix} = \overline \theta^{(1)} \times \overline x + \overline b^{(1)}" />

Note that the activation function <img src="https://latex.codecogs.com/svg.latex?\large&space;\sigma" title="\large \sigma" /> is used both as an <img src="https://latex.codecogs.com/svg.latex?\large&space;\mathbb{R}^{n}\rightarrow&space;\mathbb{R}^{n}" title="\large \mathbb{R}^{n}\rightarrow \mathbb{R}^{n}" /> function as well as an <img src="https://latex.codecogs.com/svg.latex?\large&space;\mathbb{R}&space;\rightarrow&space;\mathbb{R}" title="\large \mathbb{R} \rightarrow \mathbb{R}" /> function, depending on the context, only to make it easier to understand that the two functions perform very similar operation to both a number as well as a vector of numbers.

### Output Layer Values
The values of the output layer is calculated in a similar way using the three output values from the hidden layer units.


<img src="https://latex.codecogs.com/svg.latex?\large&space;\overline&space;a^{(2)}&space;=&space;\sigma(\overline&space;z^{(2)})&space;=&space;\sigma(\overline&space;\theta^{(2)}&space;\times&space;\overline&space;a^{(1)}&space;&plus;&space;\overline&space;b^{(2)})" title="\large \overline a^{(2)} = \sigma(\overline z^{(2)}) = \sigma(\overline \theta^{(2)} \times \overline a^{(1)} + \overline b^{(2)})" />

or 

<img src="https://latex.codecogs.com/svg.latex?\large&space;\overline&space;a^{(2)}&space;=&space;\begin{bmatrix}a_1^{(2)}\\a_2^{(2)}\end{bmatrix}&space;=&space;\begin{bmatrix}\sigma(z_1^{(2)})\\\sigma(z_2^{(2)})\end{bmatrix}&space;=&space;\sigma(\begin{bmatrix}z_1^{(2)}\\z_2^{(2)}\end{bmatrix})&space;=&space;\sigma(\overline&space;z^{(2)})" title="\large \overline a^{(2)} = \begin{bmatrix}a_1^{(2)}\\a_2^{(2)}\end{bmatrix} = \begin{bmatrix}\sigma(z_1^{(2)})\\\sigma(z_2^{(2)})\end{bmatrix} = \sigma(\begin{bmatrix}z_1^{(2)}\\z_2^{(2)}\end{bmatrix}) = \sigma(\overline z^{(2)})" />

<img src="https://latex.codecogs.com/svg.latex?\overline&space;z^{(2)}&space;=&space;\begin{bmatrix}z_1^{(2)}\\z_2^{(2)}\end{bmatrix}&space;=&space;\begin{bmatrix}\theta_{11}^{(2)}&\theta_{12}^{(2)}&\theta_{13}^{(2)}\\\theta_{21}^{(2)}&\theta_{22}^{(2)}&\theta_{23}^{(2)}\\\end{bmatrix}&space;\times&space;\begin{bmatrix}a_1^{(1)}\\a_2^{(1)}\\a_3^{(1)}\end{bmatrix}&space;&plus;&space;\begin{bmatrix}b_1^{(2)}\\b_2^{(2)}&space;\end{bmatrix}&space;=&space;\overline&space;\theta^{(2)}&space;\times&space;\overline&space;a^{(1)}&space;&plus;&space;\overline&space;b^{(2)}" title="\overline z^{(2)} = \begin{bmatrix}z_1^{(2)}\\z_2^{(2)}\end{bmatrix} = \begin{bmatrix}\theta_{11}^{(2)}&\theta_{12}^{(2)}&\theta_{13}^{(2)}\\\theta_{21}^{(2)}&\theta_{22}^{(2)}&\theta_{23}^{(2)}\\\end{bmatrix} \times \begin{bmatrix}a_1^{(1)}\\a_2^{(1)}\\a_3^{(1)}\end{bmatrix} + \begin{bmatrix}b_1^{(2)}\\b_2^{(2)} \end{bmatrix} = \overline \theta^{(2)} \times \overline a^{(1)} + \overline b^{(2)}" />

## Backpropagation
By back-propagating across the layers, we caculate the error in the cost function with respect to each each weight and bias variable, so that we can correct the weights to minimize the cost function.

In our case, the values that we need to find during backpropagation are -

<img src="https://latex.codecogs.com/svg.latex?\large&space;\frac{\partial&space;C}{\partial&space;\overline&space;\theta^{(1)}},\hspace{0.2cm}\frac{\partial&space;C}{\partial&space;\overline&space;\theta^{(2)}},\hspace{0.2cm}&space;\frac{\partial&space;C}{\partial&space;\overline&space;b^{(1)}}\hspace{0.2cm}&space;and&space;\hspace{0.2cm}\frac{\partial&space;C}{\partial&space;\overline&space;b^{(2)}}" title="\large \frac{\partial C}{\partial \overline \theta^{(1)}},\hspace{0.2cm}\frac{\partial C}{\partial \overline \theta^{(2)}},\hspace{0.2cm} \frac{\partial C}{\partial \overline b^{(1)}}\hspace{0.2cm} and \hspace{0.2cm}\frac{\partial C}{\partial \overline b^{(2)}}" />

where, <img src="https://latex.codecogs.com/svg.latex?\large&space;C" title="\large C"/> is the Cost Function.

### Weights of the Output Layer
Let us work on the weights of the output layer first.

<img src="https://latex.codecogs.com/svg.latex?\large&space;\overline&space;\theta^{(2)}&space;=&space;\begin{bmatrix}&space;\theta_{11}^{(2)}&&space;\theta_{12}^{(2)}&&space;\theta_{13}^{(2)}\\&space;\theta_{21}^{(2)}&&space;\theta_{22}^{(2)}&&space;\theta_{23}^{(2)}\\&space;\end{bmatrix}" title="\large \overline \theta^{(2)} = \begin{bmatrix} \theta_{11}^{(2)}& \theta_{12}^{(2)}& \theta_{13}^{(2)}\\ \theta_{21}^{(2)}& \theta_{22}^{(2)}& \theta_{23}^{(2)}\\ \end{bmatrix}" />

Since, C is a scalar function, its partial differential w.r.t. to the matrix <img src="https://latex.codecogs.com/svg.latex?\large&space;\overline\theta^{(2)}" title="\large \overline\theta^{(2)}" /> is given by -

<img src="https://latex.codecogs.com/svg.latex?\large&space;\frac{\partial&space;C}{\partial&space;\overline&space;\theta^{(2)}}&space;=&space;\begin{bmatrix}&space;\frac{\partial&space;C}{\partial\theta_{11}^{(2)}}&&space;\frac{\partial&space;C}{\partial\theta_{12}^{(2)}}&&space;\frac{\partial&space;C}{\partial\theta_{13}^{(2)}}\\&space;\frac{\partial&space;C}{\partial\theta_{21}^{(2)}}&&space;\frac{\partial&space;C}{\partial\theta_{22}^{(2)}}&&space;\frac{\partial&space;C}{\partial\theta_{23}^{(2)}}\\&space;\end{bmatrix}" title="\large \frac{\partial C}{\partial \overline \theta^{(2)}} = \begin{bmatrix} \frac{\partial C}{\partial\theta_{11}^{(2)}}& \frac{\partial C}{\partial\theta_{12}^{(2)}}& \frac{\partial C}{\partial\theta_{13}^{(2)}}\\ \frac{\partial C}{\partial\theta_{21}^{(2)}}& \frac{\partial C}{\partial\theta_{22}^{(2)}}& \frac{\partial C}{\partial\theta_{23}^{(2)}}\\ \end{bmatrix}" />

Using chain rule of differentiation, 

<img src="https://latex.codecogs.com/svg.latex?\large&space;\frac{\partial&space;C}{\partial&space;\overline&space;\theta^{(2)}}&space;=&space;\begin{bmatrix}&space;\frac{\partial&space;C}{\partial&space;z_1^{(2)}}\frac{\partial&space;z_1^{(2)}}{\partial\theta_{11}^{(2)}}&&space;\frac{\partial&space;C}{\partial&space;z_1^{(2)}}\frac{\partial&space;z_1^{(2)}}{\partial\theta_{12}^{(2)}}&&space;\frac{\partial&space;C}{\partial&space;z_1^{(2)}}\frac{\partial&space;z_1^{(2)}}{\partial\theta_{13}^{(2)}}\\&space;\frac{\partial&space;C}{\partial&space;z_2^{(2)}}\frac{\partial&space;z_2^{(2)}}{\partial\theta_{21}^{(2)}}&&space;\frac{\partial&space;C}{\partial&space;z_2^{(2)}}\frac{\partial&space;z_2^{(2)}}{\partial\theta_{22}^{(2)}}&&space;\frac{\partial&space;C}{\partial&space;z_2^{(2)}}\frac{\partial&space;z_2^{(2)}}{\partial\theta_{23}^{(2)}}\\&space;\end{bmatrix}" title="\large \frac{\partial C}{\partial \overline \theta^{(2)}} = \begin{bmatrix} \frac{\partial C}{\partial z_1^{(2)}}\frac{\partial z_1^{(2)}}{\partial\theta_{11}^{(2)}}& \frac{\partial C}{\partial z_1^{(2)}}\frac{\partial z_1^{(2)}}{\partial\theta_{12}^{(2)}}& \frac{\partial C}{\partial z_1^{(2)}}\frac{\partial z_1^{(2)}}{\partial\theta_{13}^{(2)}}\\ \frac{\partial C}{\partial z_2^{(2)}}\frac{\partial z_2^{(2)}}{\partial\theta_{21}^{(2)}}& \frac{\partial C}{\partial z_2^{(2)}}\frac{\partial z_2^{(2)}}{\partial\theta_{22}^{(2)}}& \frac{\partial C}{\partial z_2^{(2)}}\frac{\partial z_2^{(2)}}{\partial\theta_{23}^{(2)}}\\ \end{bmatrix}" />

Since, 

<img src="https://latex.codecogs.com/svg.latex?\large&space;\frac{\partial&space;z_i^{(2)}}{\partial&space;\theta_{ij}^{(2)}}&space;=&space;a_j^{(1)},\hspace{0.2cm}&space;\forall\hspace{0.2cm}&space;i\hspace{0.2cm}&space;\epsilon\hspace{0.2cm}&space;1,2" title="\large \frac{\partial z_i^{(2)}}{\partial \theta_{ij}^{(2)}} = a_j^{(1)},\hspace{0.2cm} \forall\hspace{0.2cm} i\hspace{0.2cm} \epsilon\hspace{0.2cm} 1,2" />

the partial differentiation can be written after expansion as,

<img src="https://latex.codecogs.com/svg.latex?\large&space;\frac{\partial&space;C}{\partial&space;\overline&space;\theta^{(2)}}&space;=&space;\begin{bmatrix}&space;\frac{\partial&space;C}{\partial&space;z_1^{(2)}}a_1^{(1)}&&space;\frac{\partial&space;C}{\partial&space;z_1^{(2)}}a_2^{(1)}&&space;\frac{\partial&space;C}{\partial&space;z_1^{(2)}}a_3^{(1)}\\&space;\frac{\partial&space;C}{\partial&space;z_2^{(2)}}a_1^{(1)}&&space;\frac{\partial&space;C}{\partial&space;z_2^{(2)}}a_2^{(1)}&&space;\frac{\partial&space;C}{\partial&space;z_2^{(2)}}a_3^{(1)}\\&space;\end{bmatrix}" title="\large \frac{\partial C}{\partial \overline \theta^{(2)}} = \begin{bmatrix} \frac{\partial C}{\partial z_1^{(2)}}a_1^{(1)}& \frac{\partial C}{\partial z_1^{(2)}}a_2^{(1)}& \frac{\partial C}{\partial z_1^{(2)}}a_3^{(1)}\\ \frac{\partial C}{\partial z_2^{(2)}}a_1^{(1)}& \frac{\partial C}{\partial z_2^{(2)}}a_2^{(1)}& \frac{\partial C}{\partial z_2^{(2)}}a_3^{(1)}\\ \end{bmatrix}" />

<img src="https://latex.codecogs.com/svg.latex?\large&space;\frac{\partial&space;C}{\partial&space;\overline&space;\theta^{(2)}}&space;=&space;\begin{bmatrix}&space;\frac{\partial&space;C}{\partial&space;z_1^{(2)}}\\&space;\frac{\partial&space;C}{\partial&space;z_2^{(2)}}\\&space;\end{bmatrix}&space;\times&space;\begin{bmatrix}&space;a_1^{(1)}&&space;a_2^{(1)}&&space;a_3^{(1)}\\&space;\end{bmatrix}" title="\large \frac{\partial C}{\partial \overline \theta^{(2)}} = \begin{bmatrix} \frac{\partial C}{\partial z_1^{(2)}}\\ \frac{\partial C}{\partial z_2^{(2)}}\\ \end{bmatrix} \times \begin{bmatrix} a_1^{(1)}& a_2^{(1)}& a_3^{(1)}\\ \end{bmatrix}" />

Let us denote the partial differentiation of the cost function with respect to i*th* unit in the output layer by <img src="https://latex.codecogs.com/svg.latex?\large&space;\delta_i^{(2)}" title="\large \delta_i^{(2)}" />. 

<img src="https://latex.codecogs.com/svg.latex?\large&space;\frac{\partial&space;C}{\partial&space;\overline&space;\theta^{(2)}}&space;=&space;\begin{bmatrix}&space;\delta_1^{(2)}\\&space;\delta_2^{(2)}\\&space;\end{bmatrix}&space;\times&space;\begin{bmatrix}&space;a_1^{(1)}\\&space;a_2^{(1)}\\&space;a_3^{(1)}\\&space;\end{bmatrix}^\intercal" title="\large \frac{\partial C}{\partial \overline \theta^{(2)}} = \begin{bmatrix} \delta_1^{(2)}\\ \delta_2^{(2)}\\ \end{bmatrix} \times \begin{bmatrix} a_1^{(1)}\\ a_2^{(1)}\\ a_3^{(1)}\\ \end{bmatrix}^\intercal" />

Which can be denoted as -

<img src="https://latex.codecogs.com/svg.latex?\large&space;\frac{\partial&space;C}{\partial&space;\overline&space;\theta^{(2)}}&space;=&space;\overline\delta^{(2)}&space;\times&space;(\overline&space;a^{(1)})^\intercal" title="\large \frac{\partial C}{\partial \overline \theta^{(2)}} = \overline\delta^{(2)} \times (\overline a^{(1)})^\intercal" />

Here, <img src="https://latex.codecogs.com/svg.latex?\large&space;\overline\delta^{(2)}" title="\large \overline\delta^{(2)}" /> is called the error vector of the output layer. It is basically the partial differentiation of the cost function w.r.t. the output layer.

### Biases of the Output Layer
We could similarly prove that the partial differentiation of the cost function w.r.t. the biases of the output layer is, 

<img src="https://latex.codecogs.com/svg.latex?\large&space;\frac{\partial&space;C}{\partial&space;\overline&space;b^{(2)}}&space;=&space;\overline\delta^{(2)}" title="\large \frac{\partial C}{\partial \overline b^{(2)}} = \overline\delta^{(2)}" />

This is because there is no multiplication factor for biases, and the derivation is almost the same as we did for the weights of the layer.

### Weights of Hidden Layer
The weights of hidden layer are

<img src="https://latex.codecogs.com/svg.latex?\large&space;\overline&space;\theta^{(1)}&space;=&space;\begin{bmatrix}&space;\theta_{11}^{(1)}&&space;\theta_{12}^{(1)}\\&space;\theta_{21}^{(1)}&&space;\theta_{22}^{(1)}\\&space;\theta_{31}^{(1)}&&space;\theta_{32}^{(1)}\\&space;\end{bmatrix}" title="\large \overline \theta^{(1)} = \begin{bmatrix} \theta_{11}^{(1)}& \theta_{12}^{(1)}\\ \theta_{21}^{(1)}& \theta_{22}^{(1)}\\ \theta_{31}^{(1)}& \theta_{32}^{(1)}\\ \end{bmatrix}" />


Hence, the partial differentiation of the cost function w.r.t. the weights of the hidden layer is

<img src="https://latex.codecogs.com/svg.latex?\large&space;\frac{\partial&space;C}{\partial&space;\overline&space;\theta^{(1)}}&space;=&space;\begin{bmatrix}&space;\frac{\partial&space;C}{\partial\theta_{11}^{(1)}}&&space;\frac{\partial&space;C}{\partial\theta_{12}^{(1)}}\\&space;\frac{\partial&space;C}{\partial\theta_{21}^{(1)}}&&space;\frac{\partial&space;C}{\partial\theta_{22}^{(1)}}\\&space;\frac{\partial&space;C}{\partial\theta_{31}^{(1)}}&&space;\frac{\partial&space;C}{\partial\theta_{32}^{(1)}}\\&space;\end{bmatrix}" title="\large \frac{\partial C}{\partial \overline \theta^{(1)}} = \begin{bmatrix} \frac{\partial C}{\partial\theta_{11}^{(1)}}& \frac{\partial C}{\partial\theta_{12}^{(1)}}\\ \frac{\partial C}{\partial\theta_{21}^{(1)}}& \frac{\partial C}{\partial\theta_{22}^{(1)}}\\ \frac{\partial C}{\partial\theta_{31}^{(1)}}& \frac{\partial C}{\partial\theta_{32}^{(1)}}\\ \end{bmatrix}" />

Using Chain Rule, 

<img src="https://latex.codecogs.com/svg.latex?\large&space;\frac{\partial&space;C}{\partial&space;\overline&space;\theta^{(1)}}&space;=&space;\begin{bmatrix}&space;\frac{\partial&space;C}{\partial&space;z_1^{(1)}}\frac{\partial&space;z_1^{(1)}}{\partial\theta_{11}^{(1)}}&&space;\frac{\partial&space;C}{\partial&space;z_1^{(1)}}\frac{\partial&space;z_1^{(1)}}{\partial\theta_{12}^{(1)}}\\&space;\frac{\partial&space;C}{\partial&space;z_2^{(1)}}\frac{\partial&space;z_2^{(1)}}{\partial\theta_{21}^{(1)}}&&space;\frac{\partial&space;C}{\partial&space;z_2^{(1)}}\frac{\partial&space;z_2^{(1)}}{\partial\theta_{22}^{(1)}}\\&space;\frac{\partial&space;C}{\partial&space;z_3^{(1)}}\frac{\partial&space;z_2^{(1)}}{\partial\theta_{31}^{(1)}}&&space;\frac{\partial&space;C}{\partial&space;z_3^{(1)}}\frac{\partial&space;z_2^{(1)}}{\partial\theta_{32}^{(1)}}\\&space;\end{bmatrix}" title="\large \frac{\partial C}{\partial \overline \theta^{(1)}} = \begin{bmatrix} \frac{\partial C}{\partial z_1^{(1)}}\frac{\partial z_1^{(1)}}{\partial\theta_{11}^{(1)}}& \frac{\partial C}{\partial z_1^{(1)}}\frac{\partial z_1^{(1)}}{\partial\theta_{12}^{(1)}}\\ \frac{\partial C}{\partial z_2^{(1)}}\frac{\partial z_2^{(1)}}{\partial\theta_{21}^{(1)}}& \frac{\partial C}{\partial z_2^{(1)}}\frac{\partial z_2^{(1)}}{\partial\theta_{22}^{(1)}}\\ \frac{\partial C}{\partial z_3^{(1)}}\frac{\partial z_2^{(1)}}{\partial\theta_{31}^{(1)}}& \frac{\partial C}{\partial z_3^{(1)}}\frac{\partial z_2^{(1)}}{\partial\theta_{32}^{(1)}}\\ \end{bmatrix}" />

Since, the weights have multilication factors of only inputs w.r.t. the <img src="https://latex.codecogs.com/svg.latex?\large&space;\overline&space;z^{(1)}" title="\large \overline z^{(1)}" />, as seen previously in the output layer,

<img src="https://latex.codecogs.com/svg.latex?\large&space;\frac{\partial&space;C}{\partial&space;\overline&space;\theta^{(1)}}&space;=&space;\begin{bmatrix}&space;\frac{\partial&space;C}{\partial&space;z_1^{(1)}}x_1&&space;\frac{\partial&space;C}{\partial&space;z_1^{(1)}}x_2\\&space;\frac{\partial&space;C}{\partial&space;z_2^{(1)}}x_1&&space;\frac{\partial&space;C}{\partial&space;z_2^{(1)}}x_2\\&space;\frac{\partial&space;C}{\partial&space;z_3^{(1)}}x_1&&space;\frac{\partial&space;C}{\partial&space;z_3^{(1)}}x_2\\&space;\end{bmatrix}" title="\large \frac{\partial C}{\partial \overline \theta^{(1)}} = \begin{bmatrix} \frac{\partial C}{\partial z_1^{(1)}}x_1& \frac{\partial C}{\partial z_1^{(1)}}x_2\\ \frac{\partial C}{\partial z_2^{(1)}}x_1& \frac{\partial C}{\partial z_2^{(1)}}x_2\\ \frac{\partial C}{\partial z_3^{(1)}}x_1& \frac{\partial C}{\partial z_3^{(1)}}x_2\\ \end{bmatrix}" />

<img src="https://latex.codecogs.com/svg.latex?\large&space;\frac{\partial&space;C}{\partial&space;\overline&space;\theta^{(1)}}&space;=&space;\begin{bmatrix}&space;\frac{\partial&space;C}{\partial&space;z_1^{(1)}}\\&space;\frac{\partial&space;C}{\partial&space;z_2^{(1)}}\\&space;\frac{\partial&space;C}{\partial&space;z_3^{(1)}}\\&space;\end{bmatrix}&space;\times&space;\begin{bmatrix}&space;x_1&&space;x_2\\&space;\end{bmatrix}" title="\large \frac{\partial C}{\partial \overline \theta^{(1)}} = \begin{bmatrix} \frac{\partial C}{\partial z_1^{(1)}}\\ \frac{\partial C}{\partial z_2^{(1)}}\\ \frac{\partial C}{\partial z_3^{(1)}}\\ \end{bmatrix} \times \begin{bmatrix} x_1& x_2\\ \end{bmatrix}" />

denoting partial differentiation of cost function w.r.t. i*th* unit in the hidden layer as <img src="https://latex.codecogs.com/svg.latex?\large&space;\delta_i^{(1)}" title="\large \delta_i^{(1)}" />,

<img src="https://latex.codecogs.com/svg.latex?\large&space;\frac{\partial&space;C}{\partial&space;\overline&space;\theta^{(1)}}&space;=&space;\begin{bmatrix}&space;\delta_1^{(1)}\\&space;\delta_2^{(1)}\\&space;\delta_3^{(1)}\\&space;\end{bmatrix}&space;\times&space;\begin{bmatrix}&space;x_1\\&space;x_2\\&space;\end{bmatrix}^\intercal" title="\large \frac{\partial C}{\partial \overline \theta^{(1)}} = \begin{bmatrix} \delta_1^{(1)}\\ \delta_2^{(1)}\\ \delta_3^{(1)}\\ \end{bmatrix} \times \begin{bmatrix} x_1\\ x_2\\ \end{bmatrix}^\intercal" />

which can be represented as -

<img src="https://latex.codecogs.com/svg.latex?\large&space;\frac{\partial&space;C}{\partial&space;\overline&space;\theta^{(1)}}&space;=&space;\overline\delta^{(1)}&space;\times&space;\overline&space;x^\intercal" title="\large \frac{\partial C}{\partial \overline \theta^{(1)}} = \overline\delta^{(1)} \times \overline x^\intercal" />

<img src="https://latex.codecogs.com/svg.latex?in\hspace{0.2cm}general:&space;\frac{\partial&space;C}{\partial&space;\overline\theta^{(n)}}&space;=&space;\overline\delta^{(n)}\times(\overline&space;a^{(n-1)})^\intercal" title="in\hspace{0.2cm}general: \frac{\partial C}{\partial \overline\theta^{(n)}} = \overline\delta^{(n)}\times(\overline a^{(n-1)})^\intercal" />

### Biases of the hidden layer
Similarly for the biases of the hidden layer, the partial differentiation is - 

<img src="https://latex.codecogs.com/svg.latex?\large&space;\frac{\partial&space;C}{\partial&space;\overline&space;b^{(1)}}&space;=&space;\overline\delta^{(1)}" title="\large \frac{\partial C}{\partial \overline b^{(1)}} = \overline\delta^{(1)}" />

<img src="https://latex.codecogs.com/svg.latex?in\hspace{0.2cm}general:&space;\frac{\partial&space;C}{\partial&space;\overline&space;b^{(n)}}&space;=&space;\overline\delta^{(n)}" title="in\hspace{0.2cm}general: \frac{\partial C}{\partial \overline b^{(n)}} = \overline\delta^{(n)}" />

## How do we calculate the error/delta vector for each layer?
One can easily notice that once we get hold of the delta/error vector <img src="https://latex.codecogs.com/svg.latex?\large&space;\overline\delta^{(i)}" title="\large \overline\delta^{(i)}" /> for each layer i, it is very easy to calculate the error in the weights and biases of each layer.
But is there a more systematic way to calculate these delta values? Do we really need to recalculate the partial differentiation of the cost function w.r.t. each layer or parameter? 
This is where backpropagation steps in. Remember what <img src="https://latex.codecogs.com/svg.latex?\large&space;\overline\delta^{(2)}" title="\large \overline\delta^{(2)}" /> actually is? 

<img src="https://latex.codecogs.com/svg.latex?\large&space;\overline\delta^{(2)}&space;=&space;\begin{bmatrix}&space;\delta_1^{(2)}\\&space;\delta_2^{(2)}\\&space;\end{bmatrix}&space;=&space;\begin{bmatrix}&space;\frac{\partial&space;C}{\partial&space;z_1^{(2)}}\\&space;\frac{\partial&space;C}{\partial&space;z_2^{(2)}}\\&space;\end{bmatrix}" title="\large \overline\delta^{(2)} = \begin{bmatrix} \delta_1^{(2)}\\ \delta_2^{(2)}\\ \end{bmatrix} = \begin{bmatrix} \frac{\partial C}{\partial z_1^{(2)}}\\ \frac{\partial C}{\partial z_2^{(2)}}\\ \end{bmatrix}" />

Using Chain Rule, 

<img src="https://latex.codecogs.com/svg.latex?\large&space;\overline\delta^{(2)}&space;=&space;\begin{bmatrix}&space;\frac{\partial&space;C}{\partial&space;a_1^{(2)}}\frac{\partial&space;a_1^{(2)}}{\partial&space;z_1^{(2)}}\\&space;\frac{\partial&space;C}{\partial&space;a_2^{(2)}}\frac{\partial&space;a_2^{(2)}}{\partial&space;z_2^{(2)}}\\&space;\end{bmatrix}&space;=&space;\begin{bmatrix}&space;\frac{\partial&space;C}{\partial&space;a_1^{(2)}}\\&space;\frac{\partial&space;C}{\partial&space;a_2^{(2)}}\\&space;\end{bmatrix}&space;\odot&space;\begin{bmatrix}&space;\frac{\partial&space;a_1^{(2)}}{\partial&space;z_1^{(2)}}\\&space;\frac{\partial&space;a_2^{(2)}}{\partial&space;z_2^{(2)}}\\&space;\end{bmatrix}" title="\large \overline\delta^{(2)} = \begin{bmatrix} \frac{\partial C}{\partial a_1^{(2)}}\frac{\partial a_1^{(2)}}{\partial z_1^{(2)}}\\ \frac{\partial C}{\partial a_2^{(2)}}\frac{\partial a_2^{(2)}}{\partial z_2^{(2)}}\\ \end{bmatrix} = \begin{bmatrix} \frac{\partial C}{\partial a_1^{(2)}}\\ \frac{\partial C}{\partial a_2^{(2)}}\\ \end{bmatrix} \odot \begin{bmatrix} \frac{\partial a_1^{(2)}}{\partial z_1^{(2)}}\\ \frac{\partial a_2^{(2)}}{\partial z_2^{(2)}}\\ \end{bmatrix}" />

Now since, 

<img src="https://latex.codecogs.com/svg.latex?\large&space;\overline&space;a^{(2)}&space;=&space;\sigma(\overline&space;z^{(2)})" title="\large \overline a^{(2)} = \sigma(\overline z^{(2)})" />

<img src="https://latex.codecogs.com/svg.latex?\large&space;\frac{\partial\overline&space;a^{(2)}}{\partial\overline&space;z^{(2)}}&space;=&space;\sigma'(\overline&space;z^{(2)})&space;=&space;\sigma'(&space;\begin{bmatrix}&space;z_1^{(2)}\\&space;z_2^{(2)}&space;\end{bmatrix}&space;)&space;=&space;\begin{bmatrix}&space;\sigma'(z_1^{(2)})\\&space;\sigma'(z_2^{(2)})&space;\end{bmatrix}" title="\large \frac{\partial\overline a^{(2)}}{\partial\overline z^{(2)}} = \sigma'(\overline z^{(2)}) = \sigma'( \begin{bmatrix} z_1^{(2)}\\ z_2^{(2)} \end{bmatrix} ) = \begin{bmatrix} \sigma'(z_1^{(2)})\\ \sigma'(z_2^{(2)}) \end{bmatrix}" />

So, the delta vector for the output layer is,

<img src="https://latex.codecogs.com/svg.latex?\large&space;in\hspace{0.2cm}short:\overline\delta^{(2)}&space;=&space;\frac{\partial&space;C}{\partial&space;\overline&space;a^{(2)}}&space;\odot&space;\sigma'(\overline&space;z^{(2)})" title="\large in\hspace{0.2cm}short:\overline\delta^{(2)} = \frac{\partial C}{\partial \overline a^{(2)}} \odot \sigma'(\overline z^{(2)})" />

<img src="https://latex.codecogs.com/svg.latex?&space;in\hspace{0.2cm}general:\overline\delta^{(N)}&space;=&space;\frac{\partial&space;C}{\partial&space;\overline&space;a^{(N)}}&space;\odot&space;\sigma'(\overline&space;z^{(N)})" title="in\hspace{0.2cm}general:\overline\delta^{(N)} = \frac{\partial C}{\partial \overline a^{(N)}} \odot \sigma'(\overline z^{(N)})" />

### Delta or Error Vectors for Subsequent Layers
The error vectors can be calculated by propagating through each layer backwards one by one, hence **Backpropagation**. 
Let us try to calculate <img src="https://latex.codecogs.com/svg.latex?\large&space;\overline\delta^{(1&space;)}" title="\large \overline\delta^{(1 )}" /> using <img src="https://latex.codecogs.com/svg.latex?\large&space;\overline\delta^{(2)}" title="\large \overline\delta^{(2)}" />

<img src="https://latex.codecogs.com/svg.latex?\large&space;\overline\delta^{(1)}&space;=&space;\begin{bmatrix}&space;\frac{\partial&space;C}{\partial&space;z_1^{(1)}}\\&space;\frac{\partial&space;C}{\partial&space;z_2^{(1)}}\\&space;\frac{\partial&space;C}{\partial&space;z_3^{(1)}}\\&space;\end{bmatrix}&space;=&space;\begin{bmatrix}&space;\frac{\partial&space;C}{\partial&space;a_1^{(1)}}\frac{\partial&space;a_1^{(1)}}{\partial&space;z_1^{(1)}}\\&space;\frac{\partial&space;C}{\partial&space;a_2^{(1)}}\frac{\partial&space;a_2^{(1)}}{\partial&space;z_2^{(1)}}\\&space;\frac{\partial&space;C}{\partial&space;a_3^{(1)}}\frac{\partial&space;a_3^{(1)}}{\partial&space;z_3^{(1)}}\\&space;\end{bmatrix}&space;=&space;\begin{bmatrix}&space;\frac{\partial&space;C}{\partial&space;a_1^{(1)}}\\&space;\frac{\partial&space;C}{\partial&space;a_2^{(1)}}\\&space;\frac{\partial&space;C}{\partial&space;a_3^{(1)}}\\&space;\end{bmatrix}&space;\odot&space;\begin{bmatrix}&space;\frac{\partial&space;a_1^{(1)}}{\partial&space;z_1^{(1)}}\\&space;\frac{\partial&space;a_2^{(1)}}{\partial&space;z_2^{(1)}}\\&space;\frac{\partial&space;a_3^{(1)}}{\partial&space;z_3^{(1)}}\\&space;\end{bmatrix}" title="\large \overline\delta^{(1)} = \begin{bmatrix} \frac{\partial C}{\partial z_1^{(1)}}\\ \frac{\partial C}{\partial z_2^{(1)}}\\ \frac{\partial C}{\partial z_3^{(1)}}\\ \end{bmatrix} = \begin{bmatrix} \frac{\partial C}{\partial a_1^{(1)}}\frac{\partial a_1^{(1)}}{\partial z_1^{(1)}}\\ \frac{\partial C}{\partial a_2^{(1)}}\frac{\partial a_2^{(1)}}{\partial z_2^{(1)}}\\ \frac{\partial C}{\partial a_3^{(1)}}\frac{\partial a_3^{(1)}}{\partial z_3^{(1)}}\\ \end{bmatrix} = \begin{bmatrix} \frac{\partial C}{\partial a_1^{(1)}}\\ \frac{\partial C}{\partial a_2^{(1)}}\\ \frac{\partial C}{\partial a_3^{(1)}}\\ \end{bmatrix} \odot \begin{bmatrix} \frac{\partial a_1^{(1)}}{\partial z_1^{(1)}}\\ \frac{\partial a_2^{(1)}}{\partial z_2^{(1)}}\\ \frac{\partial a_3^{(1)}}{\partial z_3^{(1)}}\\ \end{bmatrix}" />

<img src="https://latex.codecogs.com/svg.latex?\large&space;\overline\delta^{(1)}&space;=&space;\begin{bmatrix}&space;\frac{\partial&space;C}{\partial&space;z_1^{(2)}}\frac{\partial&space;z_1^{(2)}}{\partial&space;a_1^{(1)}}&space;&plus;&space;\frac{\partial&space;C}{\partial&space;z_2^{(2)}}\frac{\partial&space;z_2^{(2)}}{\partial&space;a_1^{(1)}}\\&space;\frac{\partial&space;C}{\partial&space;z_1^{(2)}}\frac{\partial&space;z_1^{(2)}}{\partial&space;a_2^{(1)}}&space;&plus;&space;\frac{\partial&space;C}{\partial&space;z_2^{(2)}}\frac{\partial&space;z_2^{(2)}}{\partial&space;a_2^{(1)}}\\&space;\frac{\partial&space;C}{\partial&space;z_1^{(2)}}\frac{\partial&space;z_1^{(2)}}{\partial&space;a_3^{(1)}}&space;&plus;&space;\frac{\partial&space;C}{\partial&space;z_2^{(2)}}\frac{\partial&space;z_2^{(2)}}{\partial&space;a_3^{(1)}}\\&space;\end{bmatrix}&space;\odot&space;\sigma'(\overline&space;z^{(1)})" title="\large \overline\delta^{(1)} = \begin{bmatrix} \frac{\partial C}{\partial z_1^{(2)}}\frac{\partial z_1^{(2)}}{\partial a_1^{(1)}} + \frac{\partial C}{\partial z_2^{(2)}}\frac{\partial z_2^{(2)}}{\partial a_1^{(1)}}\\ \frac{\partial C}{\partial z_1^{(2)}}\frac{\partial z_1^{(2)}}{\partial a_2^{(1)}} + \frac{\partial C}{\partial z_2^{(2)}}\frac{\partial z_2^{(2)}}{\partial a_2^{(1)}}\\ \frac{\partial C}{\partial z_1^{(2)}}\frac{\partial z_1^{(2)}}{\partial a_3^{(1)}} + \frac{\partial C}{\partial z_2^{(2)}}\frac{\partial z_2^{(2)}}{\partial a_3^{(1)}}\\ \end{bmatrix} \odot \sigma'(\overline z^{(1)})" />

which, upon expansion gives -

<img src="https://latex.codecogs.com/svg.latex?\large&space;\overline\delta^{(1)}&space;=&space;(&space;\begin{bmatrix}&space;\frac{\partial&space;z_1^{(2)}}{\partial&space;a_1^{(1)}}&\frac{\partial&space;z_2^{(2)}}{\partial&space;a_1^{(1)}}\\&space;\frac{\partial&space;z_1^{(2)}}{\partial&space;a_2^{(1)}}&\frac{\partial&space;z_2^{(2)}}{\partial&space;a_2^{(1)}}\\&space;\frac{\partial&space;z_1^{(2)}}{\partial&space;a_3^{(1)}}&\frac{\partial&space;z_2^{(2)}}{\partial&space;a_3^{(1)}}\\&space;\end{bmatrix}\times&space;\begin{bmatrix}&space;\frac{\partial&space;C}{\partial&space;z_1^{(2)}}\\&space;\frac{\partial&space;C}{\partial&space;z_2^{(2)}}\\&space;\end{bmatrix})&space;\odot&space;\sigma'(\overline&space;z^{(1)})" title="\large \overline\delta^{(1)} = ( \begin{bmatrix} \frac{\partial z_1^{(2)}}{\partial a_1^{(1)}}&\frac{\partial z_2^{(2)}}{\partial a_1^{(1)}}\\ \frac{\partial z_1^{(2)}}{\partial a_2^{(1)}}&\frac{\partial z_2^{(2)}}{\partial a_2^{(1)}}\\ \frac{\partial z_1^{(2)}}{\partial a_3^{(1)}}&\frac{\partial z_2^{(2)}}{\partial a_3^{(1)}}\\ \end{bmatrix}\times \begin{bmatrix} \frac{\partial C}{\partial z_1^{(2)}}\\ \frac{\partial C}{\partial z_2^{(2)}}\\ \end{bmatrix}) \odot \sigma'(\overline z^{(1)})" />

Since, <img src="https://latex.codecogs.com/svg.latex?\large&space;\overline&space;z^{(2)}&space;=&space;(\overline\theta^{(2)}\times\overline&space;a^{(1)})&space;&plus;&space;\overline&space;b^{(2)}" title="\large \overline z^{(2)} = (\overline\theta^{(2)}\times\overline a^{(1)}) + \overline b^{(2)}" />, we can rewrite the partial differentiation of <img src="https://latex.codecogs.com/svg.latex?\large&space;\overline&space;z^{(2)}" title="\large \overline z^{(2)}" /> w.r.t. <img src="https://latex.codecogs.com/svg.latex?\large&space;\overline&space;a^{(1)}" title="\large \overline a^{(1)}" /> as the transpose of the weight matrix for the output layer. Using this and the definition of <img src="https://latex.codecogs.com/svg.latex?\large&space;\delta_i^{(j)}" title="\large \delta_i^{(j)}" />, we have

<img src="https://latex.codecogs.com/svg.latex?\large&space;\overline\delta^{(1)}&space;=&space;(&space;\begin{bmatrix}&space;\theta_{11}^{(2)}&\theta_{21}^{(2)}\\&space;\theta_{12}^{(2)}&\theta_{22}^{(2)}\\&space;\theta_{13}^{(2)}&\theta_{23}^{(2)}\\&space;\end{bmatrix}\times&space;\begin{bmatrix}&space;\delta_1^{(2)}\\&space;\delta_2^{(2)}\\&space;\end{bmatrix})&space;\odot&space;\sigma'(\overline&space;z^{(1)})" title="\large \overline\delta^{(1)} = ( \begin{bmatrix} \theta_{11}^{(2)}&\theta_{21}^{(2)}\\ \theta_{12}^{(2)}&\theta_{22}^{(2)}\\ \theta_{13}^{(2)}&\theta_{23}^{(2)}\\ \end{bmatrix}\times \begin{bmatrix} \delta_1^{(2)}\\ \delta_2^{(2)}\\ \end{bmatrix}) \odot \sigma'(\overline z^{(1)})" />

<img src="https://latex.codecogs.com/svg.latex?\large&space;\overline\delta^{(1)}&space;=&space;(&space;\begin{bmatrix}&space;\theta_{11}^{(2)}&\theta_{12}^{(2)}&\theta_{13}^{(2)}\\&space;\theta_{21}^{(2)}&\theta_{22}^{(2)}&\theta_{32}^{(2)}\\&space;\end{bmatrix}^\intercal&space;\times&space;\begin{bmatrix}&space;\delta_1^{(2)}\\&space;\delta_2^{(2)}\\&space;\end{bmatrix})&space;\odot&space;\sigma'(\overline&space;z^{(1)})" title="\large \overline\delta^{(1)} = ( \begin{bmatrix} \theta_{11}^{(2)}&\theta_{12}^{(2)}&\theta_{13}^{(2)}\\ \theta_{21}^{(2)}&\theta_{22}^{(2)}&\theta_{32}^{(2)}\\ \end{bmatrix}^\intercal \times \begin{bmatrix} \delta_1^{(2)}\\ \delta_2^{(2)}\\ \end{bmatrix}) \odot \sigma'(\overline z^{(1)})" />

<img src="https://latex.codecogs.com/svg.latex?\large&space;in\hspace{0.2cm}short:\overline\delta^{(1)}&space;=&space;(&space;(\overline&space;\theta^{(2)})^\intercal&space;\times&space;\overline\delta^{(2)})&space;\odot&space;\sigma'(\overline&space;z^{(1)})" title="\large in\hspace{0.2cm}short:\overline\delta^{(1)} = ( (\overline \theta^{(2)})^\intercal \times \overline\delta^{(2)}) \odot \sigma'(\overline z^{(1)})" />

<img src="https://latex.codecogs.com/svg.latex?&space;in\hspace{0.2cm}general:&space;\overline\delta^{(n-1)}&space;=&space;(&space;(\overline&space;\theta^{(n)})^\intercal&space;\times&space;\overline\delta^{(n)})&space;\odot&space;\sigma'(\overline&space;z^{(n-1)})" title="in\hspace{0.2cm}general: \overline\delta^{(n-1)} = ( (\overline \theta^{(n)})^\intercal \times \overline\delta^{(n)}) \odot \sigma'(\overline z^{(n-1)})" />
