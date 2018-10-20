# intro

this series is the notes I made while I am learning the fundation of Network Analysis as a beginner written in my bad English. but I will use Chinese when life if hard.

in these series of articles, I will briefly describe the idea and concepts of this field, as well as the R codes(which is another stuff I have never met before).

this article is just an introduction.

## Contents
* why we study network
* real world examples
* tools and references

## Why study network
1. network is naturally everywhere
2. **random network** is the network of chaos and unreashablilities (but we can still study it), but in the real world, networks are more well of structure. as we already noticed that:

    * the Pareto principle
    * the recursive pattern of network
    * cumulative influence of nodes and edge
    
    Thus, usaully the network is not random and has some insterets of researching
3. we have re-invented the wheels again and again in many different fields, while the commom problems may share the same essences. So it is meanningful to study them seperately.
4. I just think it is fun to extract something from a large complex system that is modled by graph (or let' say network)


## real world examples

### Technological Network

##### The Internet

Interenet can be modled as a massive graph, and network analysis is well applied on them. For example, th routing algorthm at the network level, which makes the packets that travles among the routers going on and going on and finally reach their destinations, which happens autonomously among Heterogeneous subnetworks.

and of course the Internet has structure, at least with which we can control it

##### The telephone network

tradinationally the teletphone network is circuit switch and has a tree-tiered structure.

#### Power grid

casading effects: a failure of one node may recusively provoke the failures of the connected nodes. just image that the whole campus is out of eletric power suddently and simultaneously.

#### Transportation and distribution network

for efficiency, many of such networks usually should not contains loops. so the majority of them is a tree structure.

for example, the rivers, the blood vessles and ... the trees(plants).


## Tools and references
* R and RStudio on windows
* *Network Analysis and Visualization with R and igraph*, by Katherine Ognyanova, available at http://kateto.net/netscix2016
* *[book 2005] Network Analysis - Methodological Foudation*, edied by Ulrik Brandes and Thomas Erlebach
* *[book 2010] Managing and Mining Graph Data*, by Charu C. Aggarwal and Haixun Wang
* https://www.sci.unich.it/~francesc/teaching/network/


