---
title: 'A visual and intuitive explanation of the Gradient'
date: 2026-11-19
permalink: /posts/gradient_visual
tags:
  - Multivariable Calculus
  - Optimization
---
In this post I explain the basics of the gradient. All visualizations have been constructed using MATLAB. You can find the MATLAB-code [here](https://JasperVisterin.github.io/files/gradient_visualizer.mlx).

Imagine standing on a mountain. You can see the surrounding terrain, and your goal is to find the top. Then a logical question to ask is which direction should you walk if you want to climb as steeply as possible?

Some directions will take you uphill, others downhill, and some might leave your altitude almost unchanged. How would you determine the steepest direction without physically trying every possible direction?

The gradient can help you with figuring this out. Yet, despite being one of the most fundamental concepts in multivariable calculus, the gradient is often introduced as little more than a vector of partial derivatives. So, let's start by building some intuition.

## A function as a landscape

Consider a function \\(f\\) of two variables:

$$ z=f(x,y).$$

You can interpret this function as a landscape: every point \\((x,y)\\) in the horizontal plane is assigned a height \\(z\\) . For example, consider 

$$ f(x,y)=x^2+2y^2.$$

<br/><img src='/images/Pasted image 20261119122402.png'>

This describes an elliptic paraboloid, a bowl-shaped surface. Suppose we are standing at a particular point \\((x_0,y_0)\\). We want to understand how the height changes when we move away from this point.

There are infinitely many possible directions in which we could move. Fortunately, we do not need to investigate them independently. We can start by looking at the partial derivatives.

## Partial derivatives: looking in one direction at a time

The partial derivative with respect to \\(x\\) measures how the function changes when we move in the \\(x\\) direction while keeping \\(y\\) fixed.

In mathematical terms, this gives

$$ \frac{\partial f}{\partial x} = \lim_{h\to0} \frac{f(x+h,y)-f(x,y)}{h}.$$

Similarly,

$$ \frac{\partial f}{\partial y} = \lim_{h\to0} \frac{f(x,y+h)-f(x,y)}{h}. $$

These derivatives tell us the local slope of the landscape in two perpendicular directions. For our example, 

$$ f(x,y)=x^2+2y^2,$$

we obtain

$$ \frac{\partial f}{\partial x}=2x, \qquad \frac{\partial f}{\partial y}=4y.$$

At the point \\((1,1)\\), this gives

$$ \frac{\partial f}{\partial x}(1,1)=2, \qquad \frac{\partial f}{\partial y}(1,1)=4.$$

What does this mean?

- If we move a small distance in the positive \\(x\\) direction, our height increases approximately twice as much as the distance travelled.
- If we move a small distance in the positive \\(y\\) direction, our height increases approximately four times as much as the distance travelled.   

The following figure shows the function value for the points \\((1,1)\\) , \\((1.1 \,, 1)\\) and \\((1,\,1.1)\\). 

<br/><img src='/images/Pasted image 20261119122452.png'>

So we know how the landscape behaves in two particular directions. But what if we want to move diagonally? Or what if we want to find the direction that produces the largest possible increase in height? 

## Enter the gradient

The gradient simply collects all the partial derivatives into one vector: 

$$ \nabla f(x,y)= \begin{pmatrix} \frac{\partial f}{\partial x}\\[2mm] \frac{\partial f}{\partial y} \end{pmatrix}.$$

For a function of three variables, we obtain

$$ \nabla f(x,y,z)= \begin{pmatrix} \frac{\partial f}{\partial x}\\ \frac{\partial f}{\partial y}\\ \frac{\partial f}{\partial z} \end{pmatrix}. $$


And in general, for a function \\(f:\mathbb R^n\to\mathbb R\\), 

$$ \nabla f= \begin{pmatrix} \frac{\partial f}{\partial x_1}\\ \vdots\\ \frac{\partial f}{\partial x_n} \end{pmatrix}. $$

For our example, at \\((1,1)\\),

$$ \nabla f(1,1)= \begin{pmatrix} 2\\4 \end{pmatrix}. $$

<br/><img src='/images/Pasted image 20261119122733.png'>

But why should this particular vector point in the steepest direction of increase? After all, it is just a vector containing partial derivatives. What makes it special? To answer that question, we need to introduce one more concept: the directional derivative.

## The directional derivative

Suppose we want to move in some direction described by a unit vector

$$ \mathbf \beta= \begin{pmatrix} \beta_1\\\beta_2 \end{pmatrix}, \qquad \|\mathbf \beta\|=1. $$

The unit-length condition means that we measure the change in height per unit of distance travelled. Starting at a point \\(\mathbf p\\), we can describe our movement by

$$ \mathbf \gamma(t)=\mathbf p+t\mathbf \beta. $$

Here, \\(t\\) represents the distance travelled in direction \\(\beta\\). The height along this path is 

$$ g(t)=f(\mathbf p+t\mathbf \beta) = f(p_1+t\beta_1,p_2+t\beta_2) .$$

Using the chain rule (DONT FORGET IT 🐈), we can differentiate with respect to \\(t\\) :

$$  g'(t)= \frac{\partial f}{\partial x} (p_1+t\beta_1) \beta_1+ \frac{\partial f}{\partial y} (p_2+t\beta_2) \beta_2= \nabla f(\mathbf p +t\mathbf \beta)\cdot\mathbf \beta. $$

Evaluating this at \\(t=0\\), we obtain 

$$ g'(0)= D_{\mathbf \beta}f(\mathbf x) = \nabla f(\mathbf x)\cdot\mathbf \beta. $$

This is the directional derivative. It is a scalar that tells us exactly how quickly the function changes when we move in a particular direction. Or in other words, how steep the landscape is in a given direction. 

To further experiment with the directional derivative, I refer to a [desmos-applet](https://www.desmos.com/3d/pm4ghaap0r?lang=nl).

Now, we are ready to answer the great mystery of why the gradient points in the steepest direction.

## Why does the gradient point in the steepest direction?

Recall the geometric interpretation of the dot product (also known as scalar product):

$$ \mathbf a\cdot\mathbf b = \|\mathbf a\|\|\mathbf b\|\cos(\theta), $$

where \\(\theta\\) is the angle between the two vectors. Applying this to the directional derivative gives

$$ D_{\mathbf \beta}f(\mathbf x) = \|\nabla f(\mathbf x)\| \|\mathbf \beta\|\cos(\theta). $$

Since \\(\mathbf \beta\\) is a unit vector, \\(\|\mathbf \beta\|=1\\). Therefore,

$$ D_{\mathbf \beta}f(\mathbf x) = \|\nabla f(\mathbf x)\|\cos(\theta).$$

Now consider what happens when we change our direction of movement.

- If \\(\theta=0\\), then \\(\cos(\theta)=1\\) which means that the directional derivative is as large as possible. The vector \\(\beta\\) then fully aligns with the gradient.
- If \\(\theta=90^\circ\\), then \\(\cos(\theta)=0\\) meaning that the directional derivative is zero. The vector \\(\beta\\) is then orthogonal to \\(\beta\\).
- If \\(\theta=180^\circ\\), then \\(\cos(\theta)=-1\\) and the directional derivative is as negative as possible. The vector \\(\beta\\) points in the exact opposite direction of the gradient.

The maximum possible directional derivative is therefore

$$ \max_{\|\mathbf \beta\|=1}D_{\mathbf \beta}f(\mathbf x) = \|\nabla f(\mathbf x)\|. $$

This maximum is attained when \\(\mathbf \beta= \frac{\nabla f(\mathbf x)} {\|\nabla f(\mathbf x)\|}.\\) This should make sense because this is the case where \\(\beta\\) and the gradient align in the same direction.

This result means that the gradient points in the direction of steepest increase[^1], and its magnitude tells us the maximum rate of increase per unit distance.

Notice that the norm of the gradient gives the corresponding maximum rate of ascent and that the gradient itself is not necessarily a unit vector.

If the gradient is zero, there is no uniquely defined steepest direction. Every directional derivative is zero at that point. Or in other words,

$$ D_{\mathbf \beta}f(\mathbf x) = \|\nabla f(\mathbf x)\| \|\mathbf \beta\|\cos(\theta) = 0. $$

**A Small Example**

Let's return to our function

$$ f(x,y)=x^2+2y^2$$

We want to find the steepest direction of increase at the point \\(P=(1,1).\\) We already calculated that

$$ \nabla f(1,1)= \begin{pmatrix} 2\\4 \end{pmatrix}.$$

The direction of steepest ascent is therefore

$$  \mathbf \beta_{\max} = \frac{1}{\sqrt{2^2+4^2}} \begin{pmatrix}2\\4\end{pmatrix} = \frac{1}{\sqrt5} \begin{pmatrix}1\\2\end{pmatrix}.$$

So we should move diagonally, with twice as much movement in the \\(y\\) direction as in the \\(x\\) direction. The maximum rate of increase is

$$ \|\nabla f(1,1)\| = \sqrt{2^2+4^2} = \sqrt{20}.$$

This means that if we move one unit of distance in the direction of steepest ascent, the height initially increases at a rate of \\(\sqrt{20}\\). For comparison, consider moving directly along the positive \\(x\\) axis:

$$ \mathbf \beta_x= \begin{pmatrix}1\\0\end{pmatrix}.$$The directional derivative becomes
$$ D_{\mathbf \beta_x}f(1,1) = \begin{pmatrix}2\\4\end{pmatrix} \cdot \begin{pmatrix}1\\0\end{pmatrix} =2.$$

This makes sense as this is essentially the same as the partial derivative with respect to \\(x\\) we have computed earlier.

Moving along the positive \\(y\\) axis considering

$$\mathbf \beta_y= \begin{pmatrix}0\\1\end{pmatrix}$$

gives

$$ D_{\mathbf \beta_y}f(1,1)=4.$$

which is the partial derivative with respect to \\(y\\) we have computed earlier.

And moving in the direction of the gradient gives

$$ D_{\mathbf \beta_{\max}}f(1,1)=\sqrt{20} = \|\nabla f(1,1)\|$$

The diagonal direction is indeed steeper than either coordinate direction individually. 

This is also illustrated in the figure below where we take a step of length \\(h=0.1\\) in both the direction of \\(x\\), \\(y\\) and the direction of the gradient \\(\nabla f (1,1)\\). When stepping in the direction of the gradient which is the white line, we end up in a larger value when compared to the green and the red line.

<br/><img src='/images/Pasted image 20261119123145.png'>

This example also illustrates why calculating partial derivatives separately is not enough to understand the behaviour of a multivariable function. The gradient combines all directional information into a single object.
## Contour lines and level sets

There is even another way to understand the gradient, which is particularly useful when studying surfaces. Suppose we draw contour lines of a function:

$$f(x,y)=c$$

where \\(c\\) is a constant.

Each contour line connects points that have the same function value. In our landscape interpretation, these are lines of equal altitude. This means that walking along one of these contour lines will change your position but your height remains constant.

Let \\(\gamma(t)\\) describe a path along a contour line. Since the function value remains constant,

$$ f(\gamma(t))=c.$$

Differentiating with respect to \\(t\\) , using the chain rule 🐱🐈, gives

$$ \frac{d}{dt}f(\gamma(t)) = \nabla f(\gamma(t))\cdot\gamma'(t) = \frac{d}{dt}(c)=0. $$

This means that the gradient is perpendicular to the direction of movement along the contour line. Since this holds for every tangent direction to the contour line, we conclude that the gradient is perpendicular to the level curves.

<br/><img src='/images/Pasted image 20261119123524.png'>

In three dimensions, the same reasoning applies to level surfaces. For a surface described implicitly by 

$$ F(x,y,z)=c,$$

the gradient 

$$\nabla F= \begin{pmatrix} F_x\\F_y\\F_z \end{pmatrix}$$

is normal to the surface, provided that the gradient is nonzero. This is precisely why the gradient appears when calculating tangent planes!

<br/><img src='/images/Pasted image 20261119141827.png'>

At a point \\(P_0=(x_0,y_0,z_0)\\), the tangent plane is described by

$$ \nabla F(P_0)\cdot \begin{pmatrix} x-x_0\\ y-y_0\\ z-z_0 \end{pmatrix}=0. $$

The gradient is the normal vector of the tangent plane. We have therefore encountered three closely related interpretations:

- The gradient points in the direction of steepest ascent.
- The gradient is perpendicular to the level curves or level surfaces.
- The gradient is the normal vector of a regular level surface.

These are not three unrelated properties. They are different consequences of the same mathematical structure.

## What if the gradient is zero?

Another common misconception is that

$$ \nabla f(\mathbf x)=0$$

automatically means that we have found a minimum or maximum. This is not generally true. If we found a minimum or maximum then yes, the gradient is equal to the zero vector however, it is not necessarily the case. Remember that

$$ f'(x)=0$$

also does not necessarily mean that we have found a minimum or maximum. Consider the simple function

$$ f(x,y)=x^2-y^2.$$

Its gradient is 

$$ \nabla f(x,y)= \begin{pmatrix} 2x\\-2y \end{pmatrix}.$$

At the origin, 

$$ \nabla f(0,0)= \begin{pmatrix}0\\0\end{pmatrix}.$$

But is the origin a minimum?  Along the \\(x\\)-axis, we see the surface going up.

$$ f(x,0)=x^2\geq0. $$

Along the \\(y\\)-axis, we see the surface of \\(f\\) going down. 

$$ f(0,y)=-y^2\leq0.$$

This means that the function \\(f\\) increases in one direction and decreases in another. The origin is therefore a saddle point, not a minimum or maximum.

<br/><img src='/images/Pasted image 20261119123627.png'>

<br/><img src='/images/Pasted image 20261119123822.png'>

To distinguish between minima, maxima and saddle points, we need to investigate more than just the gradient. The Hessian matrix is one of the main tools for doing this. For a function \\(f(x,y)\\) with two variables the Hessian is

$$ \nabla^2f(x,y)=\left[\begin{array}{ccc}
\frac{\partial^2 f}{\partial x^2} & \frac{\partial^2 f}{\partial x \partial y}  \\
\frac{\partial^2 f}{\partial y \partial x} & \frac{\partial^2 f}{\partial y^2} 
\end{array}\right]$$

Suppose that we found a point for which the gradient is zero, then the point is called a stationary point. For a twice continuously differentiable function, the Hessian matrix is always symmetric. 

A positive definite Hessian at a stationary point guarantees a strict local minimum. A negative definite Hessian guarantees a strict local maximum. An indefinite Hessian indicates a saddle point. If the Hessian is only semidefinite, the second-derivative test may be inconclusive.

For the function \\(f(x,y)= x^2 - y^2\\) the Hessian is equal to 

$$\nabla^2f(x,y)=\left[\begin{array}{ccc}
1 & 0 \\
0 & -1
\end{array}\right]$$

this matrix has eigenvalues \\(1\\) and \\(-1\\) and thus is the Hessian indefinite indicating a saddle point which is in accordance with our earlier reasoning. 

This is in essence the multi-variable second-derivative test that extends the univariate test for which holds that for a point for which \\(f^{\prime}(x^*) = 0\\) that the second derivative \\(f^{\prime\prime}(x^*) = 0\\) will the determine whether it is a minimum, maximum or neither.

# Why is the gradient useful in engineering?

At this point, we have developed quite a bit of mathematical theory. But where does the gradient actually show up in engineering?

Well, a better question is to ask where it does not show up. It is present almost everywhere. Whenever we have a quantity that depends on several variables and we want to understand how it changes, the gradient is a natural tool. 

Gradients are essential in numerical mathematics as they appear whenever one wants to optimize any objective function. 

For instance, one of the most widespread applications of the gradient is in training machine learning models. Consider a model that predicts the price of a house based on its surface area. The model has parameters that determine its predictions, and we want to choose these parameters such that the predictions are as accurate as possible. Then by following the gradient we can actually tune the model and make it so that its predictions become better.

In many large-scale problems, explicitly calculating every partial derivative is computationally expensive. For instance, when optimizing a function \\(f\\) with millions of parameters, computing the gradient efficiently can be the difference between a feasible computation and an impractical one.
# Conclusion

The important thing to remember is that the gradient is a local object. It tells us how a function changes around a particular point and how quickly we can expect it to change. It does not provide a complete description of the global behaviour of the function. A consequence of this is that the gradient points in the direction of steepest ascent, not necessarily towards the global maximum. There is a lot of research going on to find good strategies of optimizing a functions. Often, the gradient plays an important role in this.

The magnitude of the gradient matters too. A large gradient indicates a large maximum rate of change per unit distance. A small gradient indicates that the function is locally relatively flat, although this does not necessarily mean we are close to an optimum.

I hope you now have a better understanding of the gradient and its use in engineering.

[^1]: The gradient we introduced is based on the standard Euclidean geometry. In this setting, we measure distances using the familiar Euclidean norm, and the gradient identifies the direction of steepest increase per unit of Euclidean distance. However, this is not the only possible way to measure distances. Consider an engineering optimization problem in which we adjust several parameters with very different physical units or scales. Treating a change of one unit in every parameter as equally significant might not be appropriate. By introducing a different notion of distance, we effectively change what we mean by the steepest direction. This leads to a more general concept of the gradient.