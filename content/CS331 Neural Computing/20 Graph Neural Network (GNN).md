We motivate this by the example of the **Node classification problem**
- This is a semi-supervised learning problem

Given a graph structure, we have some labels for some nodes.
- We also have some unlabelled nodes.
- We would like to predict which label we should give to the unlabelled nodes

![[Pasted image 20260318123144.png]]

The GNN establishes the **relationship** between the **graph** and the **embedding space**, helping us to classify the nodes

![[Pasted image 20260318123401.png]]

So how can we **map** a node vector $x_v$ to a low-dimensional vector $z_v$?
- A GNN uses **Message Passing** **Layers**, which are actually an encoder for this problem

So, what's inside the MPL?

![[Pasted image 20260318123557.png]]

A GNN is slightly different to a CNN - it operates on **graphs** rather than **images**

![[Pasted image 20260318123850.png]]

How do we **propagate** information across the graph to **compute node features**?
- A node's **neighbourhood** defines a **computation graph**
- We can **generate node embeddings** based on **local neighbourhoods**

![[Pasted image 20260318124014.png]]

Nodes **aggregate** information from their neighbours using **neural networks**
- e.g. GCN averages messages from neighbours, and applied Neural network

GNNs can be of **arbitrary depth**
- Nodes have **embeddings** at **each layer**
- Layer 0 embedding of node $v$ is its **input feature** $x_v$
- Layer $k$ embedding **gets information** from **nodes** that are $k$ **hops away**

![[Pasted image 20260318124139.png]]

How do we train the GCN to **generate embeddings?**

![[Pasted image 20260318124251.png]]

