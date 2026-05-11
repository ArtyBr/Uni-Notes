Recall Latent Factor Models from PCA slides
- 'There is a lot of data out there', and the practical hypothesis behind most ML applications is that **data** was **generated** with some **structure**
- There is some **governing process** or **system** which generates the data
- And we would like to **understand** that **structure**
- PCA is one of the methods by which we do this
$$f(W,Z)=\sum\limits^{n}_{i=1}||W^{T}z_{i}-x_{i}||^2$$
What we're trying to do in PCA is to find the **best latent model** which can **reproduce the data** that we see

---

Instead, with recommender systems we would like to:
- **Predict** users' interests to **recommend products** that have a **high probability** to be of interest to them

**Two** main approaches:
1. **Collaborative** filtering
	- *Unsupervised* learning
		- We have a **label matrix $Y$** but *no features*
		- Entries of $Y$ have '*labels*' $y_{ij}$

We have a user-item **matrix** $Y$ of $n$ rows (users) and $d$ columns (items)
- However, the matrix is very **sparse**
	- **Lots** of items **haven't been rated** by users
	- There might be some kind of underlying structure though?

![[Pasted image 20260418171008.png]]

Suppose two users have very similar tastes.
- As a result, even though a lot of items haven't been rated by one user, you could try to use the results from a similar user's taste

*Each user* may have some **representation** - some short(er) **vector** (less than total number of features)
- Matrix $Z$ - contains **vector representation** of *each user* in the database

LFM for entries in matrix $Y$
$$Y\approx W^TZ$$
$$y_{ij}\approx (w^j)^Tz_{i}$$
So in the end, the matrix $Y$ can be approximated as a **combination** a **user-feature** matrix $Z$ and an **item-feature** matrix $W$
- We assume that the relationship is *linear* (matrix product)

Now, we define a **loss function** that accounts for **all available ratings $R$**: (find the matrices)
$$f(W,Z)=\sum\limits_{i,j\in R}((w^{j})^{T}z_{i}-y_{ij})^{2}+\frac{\lambda_{1}}{2}||Z||^{2}_{F}+\frac{\lambda_{2}}{2}||W||^{2}_{F}$$
- Basically this is a sum of:
	- The **square distance** from the product $W^TZ$
	- The **size** of $Z$ (*regularisation* - trying to make it smaller/simpler)
	- The **size** of $W$ (*regularisation* - trying to make it smaller/simpler)
- This is basically the L2-regularised PCA on the **available entries** of $Y$

Once we have the $W$ and $Z$ solutions to the previous equation, we just need to **plug them in** to the solution (multiply them together) $Y\approx W^TZ$ and we can fill up the entire $Y$ matrix with some close approximation!

There may also be some biases that we might want to add, depending on certain users or other environmental factors that may additionally affect the likelihood of ratings
- Some **users** may **rate items higher**, on *average*, than other users
- Some **items** may be **rated higher**, on *average*, than other items
- **Adding biases** can help to **balance** *users* (raters) and *ratings*:
$$\hat{y_{ij}}\approx(w^{j})^Tz_{i}+\beta+\beta_{i}+\beta_{j}$$
- Where $\beta$ could be a **global bias** (e.g. for users of certain country)
- $\beta_{i}$ could be a **user-specific bias** (e.g. user rates films higher across the board)
- $\beta_{j}$ could be an **item-specific bias** (e.g. a certain genre of items is rated higher by users across the board)
	- These biases can be *learned* during training

2. **Content-based** filtering
	- *Supervised* learning
		- Use **features** $x_{ij}$ of users and items to build a **model** to **predict rating** $y_{ij}$ **given** $x_{ij}$
			- *Note*: $x_{ij}$ is a vector of features for the **user** $i$ and **item** $j$

With content-based filtering, we can base it on a **simple linear model**:
$$\hat{y_{ij}}=w^{T}x_{ij}$$
Note: $x_{ij}$ is now a **vectors** of **features** for the user $i$ and item $j$
- Usual supervised learning framework:
	- **Vector** $y$ contains all the $y_{ij}$s
	- **Matrix $X$** contains the $x_{ij}$s as rows
	- Can *predict* on **new users/items** but *cannot learn* about **each user/item**

---

Typically we would like to **combine both ideas** in a *hybrid approach*
- We have some data (content-based) about users but we would also like to approximate/discover new information (collaborative)
$$\hat{y_{ij}}=(w^j)^{T}z_{i}=\beta+\beta_{i}+\beta_{j}+w^Tx_{ij}$$

![[Pasted image 20260418175138.png]]

Great, we have a loss function for $w^j$ and $z$ and $w$, but how do we actually **solve** for these variables?

A common approach is **stochastic gradient descent** (SGD)
- Choose **random user $i$** and **random movie** $j$
- Update $\beta, \beta_{i}, \beta_{j}, w^{j}, z_{i}, w$ based on their **gradient** for this user-movie