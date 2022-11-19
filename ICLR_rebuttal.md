# General Response:

We are grateful to all the reviewers for their positive feedback and insightful suggestions! We really appreciate your kind consideration.

* :(Reviewer1)
* :(Reviewer2)
* :(Reviewer3)
* :(Reviewer4)



# Reviewer1:

## Point1: Statement of the Innovation and Motivation of EGCM

**Response**: First of all, we appreciate the reviewer for raising this question and please allow us to give explanation with the following details. In response to your comments, we have made modification in our revised paper to further clarify the difference and advantage of our newly proposed method over existing techniques.

Each module of our EGCM framework is specially designed to tackle the unique challenge of the multi-behavior sequential recommendation task. While some existing techniques (e.g., graph neural networks, contrastive learning, transformer) are adopted as the backbone or learning paradigm in our method, how to explicitly capture the behavior heterogeneity in a dynamic environment with evolving user preference, requires non-trivial tailored modelling. The difference between our EGCM and existing techniques are elaborated from the following three perspectives:
* Transformer. Different from leveraging Transformer as the single sequence encoder in most existing Transformer methods, our multi-behavior sequential recommender system involve heterogenous behavior sequences and their implicit cross-behavior dependencies, which cannot be easily handled by existing Transformer solutions. To capture the time-evolving dependencies across different types of behaviors, we design a Transformer-like dynamic memory network(*Sec.3.2*) to take the type-aware behavior embeddings from temporally adjacent time slots as input, and then calculate their correlation weights based on self-attentive scheme. With our new tailored Transformer-based memory network, the dynamic behavior heterogeneity can be well preserved in our encoded time-aware behavior representations.

* Contrastive Learning. Following the self-supervised learning (SSL) line, contrastive learning can be considered as a general learning paradigm which generates auxiliary self-supervision signals through instance contrasting. Inspired by the contrastive learning, we design new contrastive learning framework(*Sec.3.3*) to fit the dynamic heterogenous interaction modelling, with the aim of preserving both the multi-behavior commonality and user-specific preference diversity. 

    * Commonality(*Theoretical Analysis: Appendix.A.1.1*): We believe that there are user common preferences among multiple behaviors. By performing mutual information maximization across behaviors, more accurate and robust user representations can be obtained. The theoretical analysis section also states that minimizing our multi-behavioral cl loss is approximately a lower bound for maximizing multi-behavioral mutual information(*A.1.1.Eq.14*). In addition, the theoretical analysis section provides us with a theoretical analysis for multi-view contrastive learning(*A.1.1.Eq.15*). 
    * Diversity(*Theoretical Analysis: Appendix.A.1.2*): The modeling of short-term user-item interactions is based on GNN, which helps to model higher-order connectivity[1]. However, the multilayer GNN architecture introduces over-smoothing problems. The theoretical analysis part points out that the cl loss gives a larger gradient to the difficult negative samples(*A.1.2.Eq.21*), which means push away similar user representations, i.e., mitigating the over-smoothing problem. our experiments(*Sec.4.4 and A.9.3*) also corroborate with the theoretical analysis. That is, a small temperature coefficient does give a larger gradient, but too large may lead to a gradient explosion.

* Graph Neural Network. Our proposed multi-behavior graph neural network has the following advantages as a short-term multi-behavior encoder.
    * The main point is that GNN is divided into time slots(*Sec.3.1.1*), and there is a part that passes information between time slots. And our self-attention is specifically used to model the multi-relationship between time slots(*Sec.3.2*).
    * GNN's initialization module(*Sec.3.1.3*) takes over the information from the previous time-slot.
    * Encodes heterogeneity for multi-behavior.
    * Encodes the information of short-term.

	
> [1]Wang X, He X, Wang M, et al. Neural graph collaborative filtering[C]//Proceedings of the 42nd international ACM SIGIR conference on Research and development in Information Retrieval. 2019: 165-174.

## Point2: Typo Fixing

**Response**: Thanks to reviewer for pointing out the typos in our paper. We have fixed the syntax error in our revision(*Sec.3.4*). We also proofread our paper to eliminate identified typos.










# Reviewer2:


We want to start by expressing our gratitude to the reviewers for spending the time to read the paper thoroughly and point out the problems. We provide the explanations below in answer to several points.

## Point1: Statement of the Innovation and Motivation of EGCM

**Response**: (We have included the key responses to the EGCM in reviewer1's response "Statement of the Innovation and Motivation of EGCM". We would be grateful if you would take the time to review it, and we would be pleased if we could relieve some of your worries regarding EGCM.)




## Point2: Model Design and Detailing Decisions

**Response**: Experimental efforts determine each module of EGCM. Before being incorporated into the model architecture, every module has undergone meticulous design and testing. For example:
* The representation learning for user and item in Eq.1, 2 can be verified using ablation experiments w/o-MBG to verify the effect.
* The normalization of the adjacency matrix in Equation 2 is different from the version mentioned in the GCN paper[1] (the normalized Laplacian of GCN is symmetric) and is adapted to the user-item bipartite graph, which is a better choice for experimental results(*Details are placed in Appendix.A.5*).
* The initialization of time information transfer in Eq.3 is also the result of several versions of the experiment. Alternative versions are concatenate, sum, mean or without the combine.
* Eq.4,5 refer to the dynamic cross-relational memory network(Sec.3.2) to model multiple behavioral relationships across time-slots. The messaging across time slots can be replaced by GRU, i.e. ablation experiments r/w-GRU(Sec.4.3). The results prove the effectiveness of the module we designed.
* The attention in Eq.5 is still the result after trying sum, mean, concatenate+transformation.
* InfoNCE[2] in Eq.7 is still the result after multiple attempts. In the denominator, the second term $exp({s( \textbf{e}_{u}^{b}, \textbf{e}_{u'}^{b})/\tau})$ in $\sum$ is because of the better results after adding the negative samples which are in the same view of the anchor node.
* LightGCN[3] is a very well-known work in recommender systems. We have also done related experiments hoping to use a lightweight GNN architecture. However, the results of the experiments support us to use the current implementation in the model framework. We think that the relevant reason may be the complex relationship of multi-behavior data, and the use of linear layers and activation functions helps to fit.

> [1]Kipf T N, Welling M. Semi-supervised classification with graph convolutional networks[J]. arXiv preprint arXiv:1609.02907, 2016.
> [2]Oord A, Li Y, Vinyals O. Representation learning with contrastive predictive coding[J]. arXiv preprint arXiv:1807.03748, 2018.
> [3]He, Xiangnan, et al. "Lightgcn: Simplifying and powering graph convolution network for recommendation." Proceedings of the 43rd International ACM SIGIR conference on research and development in Information Retrieval. 2020.



## Point3: Details and Description of Sec.4.4 and Experiments.

**Response**: For the modules designed in the EGCM framework, in addition to the main experiments(*Tab.2*) that can illustrate the overall results, the ablation experiments(*Tab.3*), parameter experiment(*Fig.2(e)*), the visualization experiments of weights(*Fig.2(f)*) and embedding(*Fig.2(a)-(d)*) can verify the validity of the corresponding modules. Moreover, we add experiments on ablation behavior(Appendix.A.8) to illustrate the contribution of behavioral information to the results. In the following, we describe each module in detail. (We released the codes and the reviewer may not have noticed it before.)


* Ablation experiments and EGCM modules：
    * Ablation experiments can demonstrate the effectiveness of the main modules of our model：
        * w/o-CL: The result of remove contrastive based on w/o-JBL. And it can be seen that especially on Tmall and IJCAI, contrastive learning contributes a lot to the results. The reason may be that these two datasets have more behavioral data and larger overlap of timestamps.
        * r/w-GRU: The result that to remove the self-attention based memory module and model the sequential relationship between time-slots with GRU as a replacement.
        * w/o-MBG: The result of remove the short-term multi-behavior graph encoder based on w/o-JBL.
    * The main contributions to the superior results of our EGCM are the introducing of behavior-aware graph neural encoder(w/o-MBG), the use of multi-behavior data(w/o-JBL, the supplemented the multi-behavioral ablation experiment(*Appendix.A.8*) and contrastive learning(w/o-CL).
* Weights visualization and EGCM modules： 
    * We have added the details of this module(*Appendix.A.9.2*). The following is a brief description of this experiment.
    * We visualized the learned self-attention weight matrices $\Phi$ in Eq.4. Each weight matrix $\phi_{b,b'} \in \mathbb{R}^{|\mathcal{B}| \times |\mathcal{B}|}$ in $\Phi$ represents the relationship between the behaviors of a user. For example, for the Tmall dataset there are four behaviors *PageView, Favorite, Cart, Purchase*, therefore, $\phi \in \mathbb{R}^{4 \times 4}$, And, from the visualization results, it can be seen that the higher the number of interactions, the higher the weight of the behavior.
    * The results of the experiment can show that our dynamic cross-relational memory network learns the semantic content of the data.
* Embedding visualization and EGCM modules：
    * We have added the details of this module(*Appendix.A.9.1*) of the supplementary material. The following is a brief description of this experiment.
    * Visualizing behavior-specific embeddings in Fig.4 aims to show the influence of mutual information maximization on representation. Technically, we utilize t-SNE initialized with PCA[1]. And the experiment are conducted on datasets(Tmall, IJCAI) contain four types of behaviors(*page view/click, favorite, cart, purchase*). We can observe that the behaviors of EGCM are closer. In other words, for user $u$, embedding of other behaviors with the same index become closer, while users with different indexes $u\neq u'$ are pulled away.

* Ablation Experiments of User Behavior: 


|         |        Tmall       |                    |                    |                  |   |        IJCAI       |                    |                    |                  |   |  E-commerce  |              |                  |
|:-------:|:------------------:|:------------------:|:------------------:|:----------------:|---|:------------------:|:------------------:|:------------------:|:----------------:|---|:------------:|:------------:|:----------------:|
|         | $w/o$-View | $w/o$-Fav. | $w/o$-Cart | Purchase |   | $w/o$-View | $w/o$-Fav. | $w/o$-Cart | Purchase |   | $w/o$-Review | $w/o$-Browse | Purchase |
|  H@10  |       0.4625       |       0.5469       |       0.5338       |      0.3696      |   |       0.3546       |       0.4171       |       0.4634       |      0.3046      |   |    0.7323    |    0.7109    |      0.6768      |
| N@10 |       0.2641       |       0.3265       |       0.3186       |      0.2295      |   |       0.1973       |       0.2341       |       0.2693       |      0.1773      |   |    0.4456    |    0.4412    |      0.4108      |

* We appreciate the reviewers' thorough analysis. We have taken the insightful criticism and have rewritten(*Sec.4.4*) this module after reading the reviewers' remarks on the "in-depth examination of EGCM model" part. This, in our opinion, is a fascinating section of the article. It shows some visualization(self-attention weights, behavior-specific embedding, influence of $\tau$) results and combines the results with the theoretical analysis in the supplementary material(*Appendix.A.1*).


* We appreciate the review's insightful remarks and provided references, which are necessary for our test procedure. In order to address potential issues with the test, we quickly implemented a version of the full-item test (without training data), chose baselines with superior results for comparison, and the experimental outcomes are as follows:

|      |   Tmall  |          |   |   IJCAI  |          |   | E-commerce |          |
|:----:|:--------:|:--------:|:-:|:--------:|:--------:|:-:|:----------:|:--------:|
|      |   HR@10  |  NDCG@10 |   |   HR@10  |  NDCG@10 |   |    HR@10   |  NDCG@10 |
| EHCF | 0.011751 | 0.005270 |   | 0.024629 | 0.012748 |   |  0.135780  | 0.065764 |
|  CML | 0.013989 | 0.006279 |   | 0.029593 | 0.014911 |   |  0.139536  | 0.067804 |
| EGCM | 0.015703 | 0.006932 |   | 0.035461 | 0.018640 |   |  0.151299  | 0.075318 |


> [1]	Dunteman G H. Principal components analysis[M]. Sage, 1989.












# Reviewer3:

It should be highlighted that we are incredibly appreciative to the reviewers for thoroughly reading our article, offering helpful suggestions, and identifying specific sections that needed to be revised. After accepting the comments from the reviewers and thinking carefully about our shortcomings, we revised the article. For some of the required responses, part of them have been stated in the responses of the previous reviewers, and the other part will be explained as follows:

## Point: Modifications Made to the Article's "method" Section

**Response**: We spent a lot of effort organizing and updating the "methods" part in response to the second and third comments highlighted by the reviewer in the "Weaknesses" section. We believed that these issues were related to the poor writing in that section.









# Reviewer4:

> pass






