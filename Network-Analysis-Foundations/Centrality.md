

# Centrality

by chew lu, 10/28/2018



Make some notes about the centrality indexes. answer the following questions:

1. what is that
2. what is the background/ideas/application of that
3. how to impletement it

###  Contents

* What is centrality
  * basic example
* Centralities based on shortest path
  * motivation
  * some indexes
  * algorithms
    * exploitation of the recursive nature of a graph
    * random approximative algorithm
* Centralities based on Vitality
  * motivation
  * some indexes
  * algorithms
    * exploitation of the recursive nature of a graph
* Current Flow and random process
  * motivation
  * adaption
* Feedback
  * motivation
  * Katz
  * Eigenvector
  * PageRank
* General concepts
  * normalization
  * personalization
  * how to design the centrality for your own application
  * Stability and sensitivity
* End: So, what is centrality? - The Axiomatization Efforts



### What is Centrality



It is the intuitive description about a element of a network and it describe how important the element is. And its specific definition is depended on the context. 

It roots in the field of social network analysis, and it is used to describe how important a person is in the social network. Basically, the more central the person is in the network, the more important it is.

#### Basic example

Let's say the network is a friendship network, that is to say if two nodes are friends, then they share a edge. And we assume that the more friend a person has, the more import it is. For example, if someone is ill and lay in the hospital and its friend will visit it. A important person of course have many visitors. We can estimate the number of visitors by counting its degree. In formula, it is:

$$C(v) = d(v)$$

where *C* stands for the centrality, *d* for degree and *v* for vertex.

And this is called degree centrality, a most simple one. And it is one kind of **Structural Index** for the reason that it is just the same for the same vertex no matter how you draw the graph.



### Centralities based on shortest path

#### Motivation

If it is only 5 minutes before the class start and you don't want to be late for the class, then you may probably choose a shortest route to attend the class in time.

It is often observed that the the shortest the path is, the more users go on it. And sometimes, things may only be loaded on the fastest/shortest way, for example, the router almost always put the datagram into the best way it knows in the route table which captures the shortest way.

It is reasonable to assume that, shortest path has something to do with the importance of its vertexes and edges.



#### Some Indexes

Those indexes is adaptable to non-shortest-path-based indexes, or we can say they are generic because they provide the ideas of measuring the centrality in difference context.

* Eccentricity

  ​	The longest length of the shortest paths started with the vertex. 

  ​	The vertexes with the minimum eccentricity are called the center of a graph, and the minimum eccentricity is called the radius of the graph.

* Closeness Centrality

  ​	The sum over the length of the shortest paths ended/started with the vertex.

  ​	To generalize, we have:	

  ​	![general_closeness_def](.\img\general_closeness_def.PNG)

  ​	if $$a_k=k$$, then we get the narrow definition of closeness centrality.

  ​	The vertexes with the minimum closeness are called the median of a graph.

* Centroid Value 

  ​	The minimal difference of the number of customers which the salesman gains or loses if he selects *u* and a competitor chooses an appropriate vertex *v* different from *u*, assuming that the customers will go the closest salesman's location.

  ​	u is the centroid of the graph.

* Stress Centrality

  ​	The numbers of shortest paths passed over that vertex.

* Betweenness Centrality

  ​	The sum of  the rushes  passed over that vertex.  An concept is called **rush** which is defined at Chapter 3. “The rush in an element is the total flow through that element, resulting from a flow between each pair of vertices”. 

  ​	Difference against stress centrality is the context. If things goes like rushes, then we measure the centrality using this centrality. For example, we want to measure the control over a vertex, then this centrality is more suitable because control can be passes over more than one way.

* Reach

  ​	The reach of v in G, r(v,G) is the maximum value of r(v,Q) over all least-cost paths Q in G containing v, where the reach of v on P is defined as r(v,P) := min{m(s, v, P),m(v, t, P)}, where m(s, v, P), is the distance between s and v alone path P. 

  ​	Reach is used for computing the shortest path is some hierarchical network.

* Traversal Set

  ​	![travel_set_fig](.\img\travel_set_fig.PNG)

  (a, b) is call a traversal of the edge (y, z) if there is a shortest path between a and b and contains (y, z), and such (a, b) consists of a traversal set of edge (y, z).  The bigger the size a traversal set has, the more busy (important) it is.

#### Algorithm

Let's look at the betweenness centrality, its definition is:

$$C{_b}(c) = \sum_{v \ne s\ne t \in V} \frac{\sigma_{st} (v)}{\sigma_{st} } $$

and $$\sigma_{st} $$ stands for the number of shortest path between vertex *s* and *t*,  and $$\sigma_{st} (v)$$ stands for the number of the shortest path that pass through the vertex *v*.

We have two ways to calculate this simple centrality:

* derive the centrality from its definition directly.
* derive the centrality by exploiting some properties of these centrality.

The first approach has two phases. First, Uses SSSP algorithm to calculate the shortest paths for every nodes and record the predecessor sets, then we can get the number of shortest paths between every pair of vertexes, and these can be done by modifying Dijkstra’s algorithm ; Second, we can fix a pair of vertexes and find out that how many path passing a particular vertex by verifying the predecessor sets, and we sum over it, and these also can be done by scanning the predecessor table. And if you implement these naive approach, you will be grateful for Dijkstra, for his algorithm guarantee you a lot of conveniences.

The second approach exploits this discovery:

![Brandes](.\img\Brandes.PNG)

Thus, we can calculate **the dependency of a source vertex** recursively, and we just need to modify the second step of the first naive approach into these recursive style. For the fact that,  the dependency of a source vertex and be recorded and don't have to compute again, then the efficiency it better than the first approach, just like the time we save by storing the Fibonacci numbers into an array. For more optimizations, we use shortest path tree to guide the orders by which we calculate the dependency of a source vertex. We computer the dependency of a source vertex from the leaves of the shortest path tree backward to the root --- s, which has the value we exactly aim to get. The reason for such order hides in Theorem 4.2.1(especially, " $$w:v\in pred(s,w)$$" ), and first kind of values we can get is the value of the leaves, which always yields an integer, and the second kind of values we can get the must be the value of a node that its out edge only connect to the leaves. 

And what can we do with the dependency of  a source vertex? Here is the useful equation:

$$C_b(v) = \sum_{v\ne s \in V}\delta_{s·}({v}) $$

**To fasten these fastened algorithm**, the idea of **random approximative algorithm** can be applied. It is that when we spanning a shortest path tree, instead of selection all the shortest path pairs, we select only a subset of it, and then calculate the betweenness centrality the same way. It is mathematically proved that it can reduce the time to call the SSSP form O(n) to O(log(n)) with an user-defined acceptable error rate. 



### Centralities based on Vitality

>  It would be empty without me.
>
> ---  *Without Me*, a famous rap song by Eninem

#### Motivation

"Is it vital?"

How do you answer this question?

Or, let's ask, "Is drinking water vital to human being?"

A common answer would be "Of course it is vital, we would have already died deadly without water!"

Exactly, that is the way we put up with to measure the centrality of something. That is to say, Let's think about what would be like without such a element.

What would it be like without you? Well, you can image the lost, and discover that, only you are in some kind of social networks, then you would be of a little bit importance. 

#### Definition

![vitality_def](.\img\vitality_def.PNG)

The definition is the under the idea of VARIANT, which is a classic idea of science. We use the variant to observe the essence. And by the way, using No proposition, we can get a conclusion that, if the essence is not change, then the variant would not be observed. That is to say, to define a essence quality, we may just define it is invariant under the same essence.

####Some indexes

* shortcut value

  ​	Maximum increase in distance between any two vertices if e = (u, v) is removed from the graph. It is not fit in the definition of vitality index, but the idea behind it is similar.

* flow betweenness vitality

  ​	The changes of maximum flow if a vertex u is removed. Divided by the original maximum flow value.

* centrality vertexes that adapted from other catalog of centrality such as the centrality based on shortest path, such as:

  * from closeness to Wiener Index: the sum over the distances of all vertex pairs. And Wiener Index acts as the f(G) in the definition.
  * from stress to the total number of shortest path that acts as the f(G) in the definition.

#### Algorithms

Let's look at shortcut value of all the edges.

There is also two approaches, the naive brute force one and the one that exploits some properties.

First, the naive method is pretty simple, just call the ASSP on every edge, and do a element-level subtraction and select the maximum difference. It takes m times ASSP.

The complex one has a similar idea to the one that calculates the betweenness centrality. It exploits a recursive property of the network. And I think the reason why these two algorithms are all recursive is **shortest paths recursion**, in fact, if we want to know the shortest distance from *s* to *t* in a recursive style, we need to recursive find out all the shortest distances from *s* to all the nodes that point to *t*. That is a recursive property of the shortest path.

The complex algorithm defines three variables in order to express the recursion we can exploit. Let's fix a vertex *u*, and we want to find out all the shortcut values of the edge out of *u*.

$$\alpha_{i}$$ : the distance of the plan A, which means if no edge is removed, the distance we need to travel to vertex *i* from *u*. 

$$\tau _{i}$$:  the first vertex we would meet if we travel as plan A.

$$\beta_{i}$$:  the plan B, which means if we cannot travel as plan A due to $$\tau_{i}$$ is removed and unreachable, and we need to find out another fastest plan to go to vertex *i*, and just as $$\alpha_{i}$$, the $$\beta_{i}$$ is the distance if we travel as plan B.

Notes that:

$$\tau _{i}$$ can be $$\bot $$, which means that in plan A, we can go the that way first or another way first, because they have the same distance and would travel as the same fast.

$$\beta_{i}$$ can be $$\infty$$, because sometimes, there is no feasible plan B:

Then, here comes the recursion:

**The Bottom Condition** :

$$\alpha_{u}=0, ~~\tau_{u}=\O, ~~\beta_{u}=\infty$$

**The Recursive Expression** :

![shortcut_rercursive_expression](.\img\shortcut_rercursive_expression.PNG)

Here I don't want to write the equation myself, and my explanations are attached on the right to help understand better. The mathematical expressions are quite straightforward and need no explanation. And I think my analogue ($$\beta$$ as plan B) helps a lot to understand the last expression, it is exactly the way you looking and the map and find out another fastest way, and this expression share the same idea as the shortest path recursion.

And we can see that the recursion expression of $$\beta$$ is similar to the Dijkstra's execution. Let's imagine an animation that show the value of plan A and plan B are being returned back after the bottom condition is hit, and we will see a similar pattern as the Dijkstra's, that is, spanning out as a tree.

So the complexity of the algorithm is n times SSSP.



### Current Flow and Random Process

#### Motivation

All the centrality relating with shortest path have the same assume that everything love shortcut and they do go alone with the shortest path. But certainly it is rather a ideal situation than a practical reality.

For example, consider the deadlock caused by the cars that only run on its shortest path and consider the electronic current will not all rush alone the wires that has the minimum resistance (otherwise we could not even charge our cellphone).

And sometimes, it is just not practical to compute the shortest paths, and we may need another way to approximate the centrality based on shortest path. There is two idea, one is to replace the shortest path distance with the **mean first passage time **to approximate the distance, and another is to reduce the computation by random sampling.

Thus, two approaches are proposed to model the situation where the shortest path assume does not stand. They are:

* Instead of assuming that things go along the shortest path, we assume that things goes like electronic current flow. You may think it is bullshit like CNN which processes the image like human's eyes, but it may work just like CNN works. This is called the Current flow.
* Instead of assuming that things go along the shortest path, we just assume that things just go randomly and let's face a world of road nerd.  This is called random process.

By the way, if I were a quantum physicist, I might put up with the quantum centrality. 



#### Adaption

Is it easy to modify the centrality based on shortest path we have about into current flow centrality and random centrality? Well, it is easy. All we need to do is to modified the definition of shortest path and the corresponding distance.

For current flow, the rushes of betweenness centrality which is based on shortest path (more precisely, the fraction of shortest paths) can be replaced by the current on the edge, and the closeness of a vertex can be adapted into current-flow closeness centrality just by changing the concept of the distance of two vertexes into the difference between the potential of the two vertexes. 

For random process, the rushes of betweenness centrality is received by counting the number of 'encounters' of some random travelers in the graph, who are just load nerd that starting from *s* and aiming at *t*, random walking and enjoy the time. Then we get the random-walk betweenness centrality. And, it we replace the distance in the closeness centrality with the mean first passage time, then we get exactly the random-walk closeness centrality. The mean first passage time means the time it take averagely for a source vertex *s* to pass a massage to a target vertex *t*, and the massage walks randomly.

Adaption is not everything. Although adaption is always a good idea to try to extend the knowledge, it should not limit the uniqueness of a new model, should not limit the possibility to have a original centrality of that model. Just like the centrality respecting feedback gives a lot of unique and original centrality index that make a huge difference.



### Feedback

> For to every one who has will more be given, and he will have abundance; but from him who has not, even what he has will be taken away.
>
> — [Matthew](https://en.wikipedia.org/wiki/Gospel_of_Matthew) 25:29, [RSV](https://en.wikipedia.org/wiki/Revised_Standard_Version).



#### Motivation

The more important a node is, the more important its neighbors are. The more important its neighbors is, the more important it is. It is a common observation in the real world.

How to capture the power that comes from relationship? How to compute the a academic paper's SCI with respecting the importance of the papers that cite this paper? And is the person with the largest number of friends is the most powerful person? And how to quantify this person's centrality?

To answer these question, we need some mathematics to guarantee the convergence, just as the two Stanford student did.



#### Katz

Simply, **A** is the adjacency matrix, and the Katz status is given below, where $$\alpha$$ is a number that ensures the convergence.

![katz](.\img\katz.png)

To understand this formula, recollect two pieces of knowledge from high school and discrete mathematics (or graph theory, precisely).

The first piece of knowledge is geometric sequence from high school, here $$\alpha$$ is less than 1 so the sum over the $$\sum^\infin_{k=i}\alpha^k$$ is a certain real number. And the starting number *i* is depended on the second piece of knowledge, that is the multiplication of an adjacency matrix. If a matrix multiple itself then we get a adjacency matrix that represent a graph that is derived from the original graph by adding the edges between the vertexes that have a distance 2.

The idea of Katz status is the longer the distance, the weaker the support a vertex would give to its friend. 



#### Eigenvector

A loose definition of feedback centrality would be this abstract formula:

$$c(v_i) = f(c(v_1), ... , c(v_n))$$

Yep, because the neighbors matters and play as parameters, so it is recursive, and that is why we need to ensure the convergence. And the great work Larry Page and Sergey Brin did is to proof the convergence.

We can write the formula above in a matrix style, the centralities are stored in the resulting vector *v*:

$$v = f(v)$$

And, rethink about the Katz status, it use the adjacency matrix, so, we can revise the abstract formula above into:

$$v = f(A,v)$$

So, we can open observe a pattern like that

$$v^{k+1} = f(A,v^k)$$

And, think about the Katz status, think about its $$\alpha$$, then we get:

$$\lambda v^{k+1} =  f(A,v^k)$$

And we know that life is short so we always want to make things easier so we open observe a pattern like that:

$$ \lambda v^{k+1} = A*v^k$$, which is proved to be convergent, then we have:

$$ \lambda v = A * v$$

So that the the calculation of feedback centrality is usually reduced to the calculation of eigenvector and eigenvalue. And someone call the feedback centrality as eigenvector centrality.



#### PageRank

I can bet that the most familiar centrality you known before you read anything about Network analysis or Element-level centrality is PageRank, and the most unfamiliar centrality you know after you read the book Network Analysis is PageRank.

The idea is simple and is demonstrated below, the righter vertex has a higher rank when we search "java".

![PageRank_visualization_1](C:\Users\lu zhao\Desktop\img\PageRank_visualization_1.png)

And the gray line show the contribution from the left vertex to the right vertex, which have the feedback property.

And continue the discussion of eigenvector problem, for PageRank we have:

$$ c_{PR}^{k+1} = dPc_{PR}^k+(1-d)1_n$$

where d is the dumping value which is used to give every vertex the same ranking score that would be give to the its neighbors and the given ranking score are control by the dumping value.

And P is the transition matrix. and $$p_{ij} = ((i,j) \in E)? \frac{1}{d^+(j)}:0$$.

Sure, there are a lot of topics is not covered here, and the interested reader can refer to the book and even refer to the book' reference.



### General Concepts

Although we cannot have a general centrality, there are some general concepts that works on the centrality we have talk about.

#### Normalization

How to compare the two student with their GPA? One student from WHU and another from ZJU.

The full GPA in WHU is 4.0 and the full GPA in ZJU is 5.0.

The answer is simple --- normalize them by dividing a student's GPA by its full GPA.

The same idea applies to centralities. That is, find a fair enough divisor. To compare the nodes intra- graph, we may find Euclidean Normalized Form helpful, in fact, *p*-norm form of the centrality vector are widely use as the divisor. And to compare the nodes extra-graph, we may need to select a value that is big enough to act as a divisor (or, we don't want to find one, just use the theoretical maximum value of the unnormalized centrality value). But sometimes, we cannot have a node that has the centrality value that can be the divisor. For example, there is no a single PageRank or Katz status can be a common divisor, due to the fact that damping value is not the same in each graph.

And be aware of the information distortion caused by normalization. For example:

![BCvsStressC](.\img\BCvsStressC.PNG)

As the picture above demonstrates, the stress centrality is suitable to measure the stress that is put onto each vertex, but not suitable to evaluate how much communication control a vertex has.

But something really ridiculous is that, if we chose the maximum value to normalize these centrality, then each gray node would have a value 1 in their picture, which is misleading.

#### Personalization

As Confucius said: teaching according to the student's talent, it would be better to give the user the information that meets the users taste. Personalization is the process to return a personalized result.

It is done by two approaches: change the weights according to the user's preference; select a 'rootset' that are relatively important to the user.  And these method can be combined together.

The first approach of personalization applies a weight vector ***v*** to **V** , **E**, or to the transition matrix of the random surfer model in the case of the Web centralities. To give Web Centrality, let's say, PageRank, the personalization ability, we must consider the time it take to generate a personalized query result to a user. To reduce the time, there are two strategies, one is to divide the computation into offline phase and online phase, another is approximation with less expensive computation. The offline phase is to compute the basis matrix of personalized PageRank which is used in the online phase to generate the query result. 

Rootset are the set of vertexes that are preferred by the user. And the vertexes that are related to them will be treat specially. For example we can generate a special betweenness centrality that only consider the paths that 1) source in R, 2) target in R, 3) end in R. Here R is the shorthand for rootset. And modification of the definition of the centrality may need to change, for example, the definition of betweenness centrality is modified slightly as:

$$C_{b|P}(v) = \sum_{v\ne s \in V}\delta_{s·|p}({v}),~ where~\delta{st|P} = \frac{\sigma_{st}}{|P(s,t)|} ,~$$

$$where ~|P(s, t)| ~is~ any~ well~ defined~ paths~ from ~s ~to~ t$$

That means, the divisor is no longer rigidly the number of shortest path from *s* to *t*, it can be the number of path that from *s* to *t* via a vertex in R, while the definition of $$\sigma_{st}$$ changes correspondingly, such as the number of path that from *s* to *t* via *v* and via a vertex in R also.

#### How to design the centrality for you own application

First thing to be claim is that there is no silver bullet for different application, and the only general lecture can be learn from the centrality indexes we have discussed so far is that it does matter which precise definition of centrality one uses in a concrete application.

Here, the book proposes a workflow for choosing, adapting and design the centrality index for your own application, and it is just a loose description rather than a strict schema and it doesn't works with all centrality indexes that are published.



![workflow_design_centrality_index](.\img\workflow_design_centrality_index.png)

Let's walk through the chart. The first thing to do is to specify and analyze the application, and it is the application that determine the network model and the corresponding questions. And based on the network model and questions, we determine which kind of centrality we want to use, that is to say, we determine the **basic term** of our centrality.

There are four basic terms:

* Reachability. How can a vertex be reached? How many vertex can reach THIS vertex with some distance? How can a vertex can be reached by a random way? Reachability intents to answer this question concretely.  For example, the degree centrality is clearly a centrality of reachability. For more example, think about the closeness, the eccentricity and the random walk closeness.  But I want to criticize these basic term for that it is sure that every centrality index reflexes reachability and it causes confusion when you think about which or how much this basic term should be used in your centrality index. But it is a still a good basic term for us to think in and to find out what does it means to be important in our specific application.
* Amount of flow. Flow is an abstraction about the information or delivers or anything that can be divided and flow. In these basic term, we focus on the question the amount of flow that pass an element. Such as, current flow centrality, betweenness centrality, and random walk that can be viewed as random flowing.
* Vitality. Explained before. $$V=(G,x)$$, so the parameters of a Vitality do include the whole graph.
* Feedback. Explained before.

Personalization and normalization have been explained, so let's go to the **term operator**. 

As we can see from the figure, it is only meaningful to the first three basic terms.  It can be viewed as a function *f*, s.t. $$f: (S,V) \to R, ~where ~ S ~ is ~ a ~ set ~of ~function~s :(G,V)\to R.$$ That is to say, it is the a series of operations that we apply the some values that are defined and calculated from the graph to generate the centrality. For example, we average the first passage time the get the mean first passage time, and sum over the mean first passage times to get the random walk closeness centrality.

Last, mention the left dashed line. For feedback centrality, the Rootset personalization may not walk and the feedback centrality is usually calculated by translate its time-consuming calculation into the question of solving a system of linear equations.

#### Stability And Sensitivity

In my opinion, these two concepts reflex the same observation, but we name it a different name. We want to know how the degree of changes of a measure when the thing it measures changes. And a stable centrality index has a low sensitivity. We may want the result wouldn't change dramatically when a few links or nodes are added/removed to/from the network. 

Here I gives a few examples.

* If we focus on the centers, the set of vertexes that have the highest centrality, and only the addition of edge (u , v) where u is one of the center vertexes, then we would use the following three terms to describe the stability, H is the graph derived from G by adding (u,v).
  * unstable: $$S_c(H) \nsubseteq S_c(G)\cup \{v\} $$, which means the center drifts to the nodes that are outside of the expected center, new unexpected center forms.
  * quasi-stable:  $$S_c(H) \subseteq S_c(G)\cup{v}~and ~u\notin S_c(H) $$, which means the old center is no longer a center
  * stable: $$S_c(H) \nsubseteq S_c(G)\cup{v} ~and~ u\in S_c(G) $$, which means the old center is the old center, and the expected new center can be a new center.

  Let's think about our real work, is our society stable? Some kind of.

* If we focus on the numerical stability we get a common stability analysis. For example, studying the eigenvector that are used for the feedback computation, which can be an evidence for the sensitivity of a feedback algorithm.

* Another kind of stability is also discrete, one kind of this kind of stability is called rank stability. The idea is to measure the sensitivity of the ranking result.  Here I just pose a definition of rank-stable:

  ![rank-stable_def](W:\Typora\md\Network-Analysis-Foundations\img\rank-stable_def.png)

  In another word, the XOR of the ranking result goes to 0% when the changes in the graph are limited and the size of the graph goes to infinity.

And we should note that the first and the third stability are discrete and the second are normal sensitivity that are continuous.

 

### End: So, What Is Centrality? - the Axiomatization Efforts.

As mentioned before,  there is no generally acceptable definition of centrality. But researchers are making their best to give a definition by which we can study to get a founder understanding of centrality. Here I report a few axiomatizations for giving centrality (or one kind of centrality indexes) a definition. 

Reading these section about axiomatization, it would be confused because axiom in Chinese means “公理”,  which in my mind is something that cannot be proved and it is naturally correct. So, why can we have "公理" on the centrality indexes? on something abstract and unnatural?

Let me talk about the last time I met the term "axiom", which may help to understand the idea of axiomatization. It was the time when I learn Linear Algebra lecture taught by [Dr Oliver Roche Newton](http://oliverrochenewton.wixsite.com/homepage). the term "axiom" shown up, when we learned the Vector Space. And we just those axioms to determine whether a space is a Vector Space. 

I think the Chinese Linear Algebra textbook would not translate axiom into "公理", so as we cannot interpret  axiom here as "公理". In stead, it is more like the conditions a centrality should satisfy in order to be a legal centrality. Just like a space need to pass the examination of these axioms, then it is the Vector Space. 

#### Structural Index

![structural_index_def](.\img\structural_index_def.PNG)

The key idea behind this definition is INVARIANT. In this definition, the essence (the structure) is capture by the invariant under isomorphisms.

And if we define a structure index for a whole graph, the invariant under isomorphisms will also works. And if we want to come up with a definition of something, for example, the top two universities in China is the universities that is respected by every Chinese, invariant under the citizen of China. 

Yep, the no proposition of this idea is the idea behind the definition of vitality. 

#### What is vertex centrality - Answered by Sabidussi

![Sabidussi](C:/Users/lu%20zhao/Desktop/md/Network-Analysis-Foundations/img/Sabidussi.png)Here, moving an edge just like the mv command on Linux Shell, it is not rm. And it just move one end of the edge and fix other end point.

It is historically a enforced version of structural index.

#### What is vertex centrality - Answered by Kishi

![Kishi](C:/Users/lu%20zhao/Desktop/md/Network-Analysis-Foundations/img/Kishi.png)

Let H be the graph (V(G), E(G) $$\cup$$ {(u,v)}), and $$\Delta_{uv}(w) = C_H(w)-C_G(w) $$, for any w $$\in$$ V. 

#### What is vertex centrality - Answered van den Brink and Gilles

Readers may use the definition given above to verify the feedback centrality and find that feedback centrality is not a centrality. Well, the first two definitions are called distance-based centrality, that is to say they may only consider the first three terms. And it is worthy mentioning that although they have only consider the centrality indexes that are based on distance, they don't cover all the accepted distance-based centrality indexes. So, it still reminds a challenge to conquer all the centrality.

For feedback centrality indexes, there are also axiomatization effort. The first example is given by van den Brink and Gilles, it answers the question that what property should a measure of relational power has.. Unlike the first two definitions, instead of using one two three condition, they use the term "axiom" to verify the legality of a centrality.

![BG_axiom1](./img/BG_axiom1.PNG)

Axiom 1 says that the sum of the centrality index should be the number of the dominated vertexes (the vertexes having edge pointing to another vertex).![BG_axiom2](W:/Users/User/Desktop/img/BG_axiom2.PNG)

![BG_axiom3](./img/BG_axiom3.PNG)

![BG_axiom4](./img/BG_axiom4.PNG)

The independent partition of G is the partition of G that have all the vertexes and no the same edge would occur in two different partition and no edge is omitted.

That is all. Once again, it answers the question that what property should a measure of relational power has.

And if we change the first axiom into this:

![BG_axiom1b](./img/BG_axiom1b.PNG)

then the degree centrality meets all the axioms. That is why we call the axiomatization given by van den Brink and Gilles is a bridge between degree to feedback (viewed as relational power).

#### What is vertex centrality - Answered Palacios-Huerta and Volij

Pinski-Narin-centrality may be seen as the basic of PageRank, and the axioms proposed by Palacios-Huerta and Volij are under the enlightening of the centrality invented by Pinski and Narin.

And I think Pinski-Narin-centrality is the real PageRank, because it ranks the real paper pages in different journals.

Those axioms are described under the title "feedback centrality" in subsection 5.4.2 and the description is not complete enough so I just show the axioms here and give some comments. 

And more thing of interest show here is a set of ranking questions, there are:

- isoarticle problem.
- homogeneous problem.
- reduced ranking problem.
- ranking problem resulting from splitting j.

The detail won't be discussed here.

![PHV_axiom1](./img/PHV_axiom1.PNG)



![PHV_axiom2_title](./img/PHV_axiom2_title.PNG)

![PHV_axiom2_3_4](.\img\PHV_axiom2_3_4.PNG)



And the Pinski-Narin-centrality are the only centrality that satisfy this set of axioms.

#### What is vertex centrality - Answered Ruhnau’s vertex centrality axioms

Lastly, Freeman in 1979 put up with an intuitive understanding about centrality:

> “A person located in the center of a star is universally assumed to be structurally more central than any other person in any other position in any other network of similar size.”

And Ruhnau formalize it:

![Ruhnau_axiom](.\img\Ruhnau_axiom.PNG)

Due to the fact that the eigenvector centrality normalized by the Euclidean norm has the property that the maximal attainable value is t = 1/√2 (independent of n), and that it is attained exactly at the center of a star, it is also a vertex-centrality (multiplied by √2). 



