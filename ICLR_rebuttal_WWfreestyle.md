
# General Response:

We are grateful to all the reviewers for their positive feedback and insightful suggestions! We really appreciate your kind consideration.

* :(Reviewer1)
* :(Reviewer2)
* :(Reviewer3)
* :(Reviewer4)



# Reviewer1:


> **Point1**: "Each of the individual piece of EGCM (Multi-relation GNN, Transformer, Contrastive learning) is not new in itself ... in the related works."


## Response to the differences from previous work.

First of all, we appreciate reviewer for raising relevant doubts and please allow us to give an explanation:
* Our module does apply classical models and paradigms, which contribute to better experimental results. However, we are considering how to model long- and short-term multi-behavior paradigms in user-item interactions with rare validated classical milestone models <font size="1">(e.g., Diffusion Model, GNN, Contrastive and Transformer)</font>.

* Each module of our model is designed for scenario-specific data; they are not simple applications of the milestone model. We did many substitution experiments during the design process and used the version that made the best results in the end. In addition, we performed theoretical support
<font size="1" color=red>(e.g.,TODO: contrast learning for multi-view mutual information maximization and increased instance differentiation by increasing the gradient.)</font> for some modules and obtained experimental results that echoed the theory(<font size="1" color=red>(TODO:.)</font>).

* Allow us to present the advantages of our modules over the milestone model:
    * Transformer：
        * We do not use transformer in the model, but we introduce self-attention to model the relationship of multiple behaviors between time slot t and t-1. 
        * Probably because the writing confused the reviewer, we have rewritten this part of the article<font size="1" color=red>(TODO:.)</font>. Specifically, we simplified the formula and used easy-follow notation. 
        * To illustrate that our self-attention is used to model the relationship between each user's behavior rather than the tranfomer component, we have added the process of tensor dimension transformation table<font size="1" color=red>(TODO:.)</font> and expanded the description<font size="1" color=red>(TODO:.)</font> of the self-attention weight visualization experiment to the supplementary material.
    * Contrastive：
        * Our contrastive module models the commonality and diversity of user preference. And we provide theoretical support<font size="1" color=red>(TODO:.)</font> and experimental validation<font size="1" color=red>(TODO:.)</font> for "commonality" and "diversity" respectively. What's more, it is worth noting that, some experimental results with parameters not in the reasonable range are the extreme cases of the theoretical analysis.
        * Commonality: We believe that there are user common preferences among multiple behaviors. By performing mutual information maximization across behaviors, more accurate and robust user representations can be obtained. The theoretical analysis section also states that minimizing our multibehavioral cl loss is approximately a lower bound for maximizing multibehavioral mutual information. In addition, the theoretical analysis section provides us with a theoretical analysis for multi-view contrastive learning.
        * Diversity: The modeling of short-term user-item interactions is based on GNN, which helps to model higher-order connectivity[]. However, the multilayer GNN architecture introduces over-smoothing problems[]. The theoretical analysis part points out that the cl loss gives a larger gradient to the difficult negative samples, which means push away similar user representations, i.e., mitigating the over-soomthing problem. And, very interestingly, our experiments also corroborate with the theoretical analysis. That is, a small temperature coefficient does give a larger gradient, but too large may lead to a gradient explosion. We publish the code, data set, and parameters of the model (e.g., a Tmall temperature coefficient of 0.037 is optimal, but a loss of "NaN" occurs at 0.02 because of gradient explosion).
    * GNN: Our GNN has the following advantages as a short-term multi-behavior encoder.
        * The main point is that GNN is divided into time slots, and there is a part that passes information between time slots. And our self-attention is specifically used to model the multi-relationship between time slots.
        * GNN's initialization module 3.1.5 takes over the information from the previous time-slot.
        * Encodes heterogeneity for multi-behavior.
        * Encodes the information of short-term.

> **Point2** typos: "some typos... On page 2..."

Thanks again to reviewer for pointing out the typpos in our article. We directly fixed the syntax error and rewrote the part<font size="1" color=red>(TODO:.)</font> of the method that was more problematic as pointed out by the review.



# Reviewer2:

> **Point1**: The constituent... are known and there is limited ... novelty.

(For more information on this issue, please see the above "**Reply on the differences between the model module and previous work**.")


> **Point2**: Many of ... seem arbitrary ... not adequately justied. Over three pages ... loose intuitive motivation without actually justifying ... 

* Each of our modules is actually obtained through experimental attempts. Each module has been carefully designed and tested several times before being retained in the model framework. For example:
    * The modeling of the user and item representations in Equations 1 and 5 is itself in our ablation experiment w/o-MGB.
    * The normalization of the adjacency matrix in Equation 2 is different from the version mentioned in the GCN paper (the normalized Laplacian of GCN is symmetric) and is adapted to the user-item bipartite graph, which is a better choice for experimental results.
    * The Initialization of time information transfer in Equation 6 is also the result of several versions of the experiment. Alternative versions are concatenate, sum, mean.
    * Equations 7, 8, 9, and 10 all refer to the self-attention module in order to model multiple behavioral relationships across time-slots. The information of multiple time-slots itself can be replaced by RNN(eg.,GRU), and we likewise illustrate its effectiveness with ablation experiments.
    

> **Point3**: The ablation studies ... extremely limited, ... to justify the design decisions ... without ... test the model's ability to ... tautologically explain the observed performance improvements ... lack of rigorous experimental design to validate ... provides a solid foundation...


## Response to the ablation, weights and embedding visualization and parameter experiments to verify the validity of specific modules.

* Ablation experiments and EGCM modules：
    * Ablation experiments can demonstrate the effectiveness of the main modules of our model：
        * w/o-CL: The result of remove contrastive based on w/o-JBL. And it can be seen that especially on Tmall and IJCAI, contrastive learning contributes a lot to the results. The reason may be that these two datasets have more behavioral data and the overlap of individual behavioral timestamps is high.
        * r/w-GRU: The result that to remove the self-attention based memory module and model the sequential relationship between time-slots with GRU as a replacement.
        * w/o-MBG: The result of remove the short-term multi-behavior graph encoder based on w/o-JBL.
    * The main contributions to the superior results of our EGCM are the introducing of behavior-aware graph neural encoder(w/o-MBG), the use of multi-behavior data(w/o-JBL, we supplemented the multi-behavioral ablation experiment and added it to the <font size="1" color=red>(TODO:.)</font> of supplementary material) and contrastive learning(w/o-CL).
* Weights visualization and EGCM modules： 
    * We have added the details of this module in <font size="1" color=red>(TODO:.)</font> of the supplementary material. The following is a brief description of this experiment.
    * We visualized the learned self-attention weight matrices $\Phi$ in Eq.4. Each weight matrix $\phi_{b,b'} \in \mathbb{R}^{|\mathcal{B}| \times |\mathcal{B}|}$ in $\Phi$ represents the relationship between the behaviors of a user. For example, for the Tmall dataset there are four behaviors *PageView, Favorite, Cart, Purchase*, therefore, $\phi \in \mathbb{R}^{4 \times 4}$, And, from the visualization results, it can be seen that the higher the number of interactions, the higher the weight of the behavior.
    * The results of the experiment can show that our dynamic cross-relational memory network learns the semantic content of the data.
* Embedding visualization and EGCM modules：
    * We have added the details of this module in <font size="1" color=red>(TODO:.)</font> of the supplementary material. The following is a brief description of this experiment.
    * Visualizing behavior-specific embeddings in Fig.4 aims to show the influence of mutual information maximization on representation. Technically, we utilize t-SNE initialized with PCA[1]. And the experiment are conducted on datasets(Tmall, IJCAI) contain four types of behaviors(*page view/click, favorite, cart, purchase*). We can observe that the behaviors of EGCM are closer. In other words, for user $u$, embedding of other behaviors with the same index become closer, while users with different indexes $u\neq u'$ are pulled away.

[1]	Dunteman G H. Principal components analysis[M]. Sage, 1989.



> **Point4**: The paper cites [1], although it does not seem to take any lessons from the work ... applying non-linearities and transformation matrices to non-feature-based ... to apply to...

* LightGCN is a very famous work in the recommendation system research line. We have also done relevant experiments to hope to use the lightweight GNN architecture. However, the results of the experiment support us to use the current implementation method in the model framework.


> **Point5**: ... sampled ... despite previous work [2] ... misleading...chose this evaluation..

* LightGCN is a very well-known work in recommender systems. We have also done related experiments hoping to use a lightweight GNN architecture. However, the results of the experiments support us to use the current implementation in the model framework. <font size="1" color=red>(TODO:.)</font> 


> **Point6**: The "In-Depth Analysis of ECGM Model" ... only a paragraph, and the ... not adequately described... interesting, informative, or in depth.

## Response about "In-Depth Analysis of ECGM Model".

* This section shows the experimental results explained in some classes and partially corresponds to the results of the theoretical analysis in the supplementary material.

# Reviewer3:

> **Point1**: "The ... limited. ... simple applications/extensions of existing methods.."

## Response to the differences from previous work.

First of all, we appreciate reviewer for raising relevant doubts and please allow us to give an explanation:
* Our module does apply classical models and paradigms, which contribute to better experimental results. However, we are considering how to model long- and short-term multi-behavior paradigms in user-item interactions with rare validated classical milestone models <font size="1">(e.g., Diffusion Model, GNN, Contrastive and Transformer)</font>.

* Each module of our model is designed for scenario-specific data; they are not simple applications of the milestone model. We did many substitution experiments during the design process and used the version that made the best results in the end. In addition, we performed theoretical support
<font size="1" color=red>(e.g.,TODO: contrast learning for multi-view mutual information maximization and increased instance differentiation by increasing the gradient.)</font> for some modules and obtained experimental results that echoed the theory(<font size="1" color=red>(TODO:.)</font>).

* Allow us to present the advantages of our modules over the milestone model:
    * Transformer：
        * We do not use transformer in the model, but we introduce self-attention to model the relationship of multiple behaviors between time slot t and t-1. 
        * Probably because the writing confused the reviewer, we have rewritten this part of the article<font size="1" color=red>(TODO:.)</font>. Specifically, we simplified the formula and used easy-follow notation. 
        * To illustrate that our self-attention is used to model the relationship between each user's behavior rather than the tranfomer component, we have added the process of tensor dimension transformation table<font size="1" color=red>(TODO:.)</font> and expanded the description<font size="1" color=red>(TODO:.)</font> of the self-attention weight visualization experiment to the supplementary material.
    * Contrastive：
        * Our contrastive module models the commonality and diversity of user preference. And we provide theoretical support<font size="1" color=red>(TODO:.)</font> and experimental validation<font size="1" color=red>(TODO:.)</font> for "commonality" and "diversity" respectively. What's more, it is worth noting that, some experimental results with parameters not in the reasonable range are the extreme cases of the theoretical analysis.
        * Commonality: We believe that there are user common preferences among multiple behaviors. By performing mutual information maximization across behaviors, more accurate and robust user representations can be obtained. The theoretical analysis section also states that minimizing our multibehavioral cl loss is approximately a lower bound for maximizing multibehavioral mutual information. In addition, the theoretical analysis section provides us with a theoretical analysis for multi-view contrastive learning.
        * Diversity: The modeling of short-term user-item interactions is based on GNN, which helps to model higher-order connectivity[]. However, the multilayer GNN architecture introduces over-smoothing problems[]. The theoretical analysis part points out that the cl loss gives a larger gradient to the difficult negative samples, which means push away similar user representations, i.e., mitigating the over-soomthing problem. And, very interestingly, our experiments also corroborate with the theoretical analysis. That is, a small temperature coefficient does give a larger gradient, but too large may lead to a gradient explosion. We publish the code, data set, and parameters of the model (e.g., a Tmall temperature coefficient of 0.037 is optimal, but a loss of "NaN" occurs at 0.02 because of gradient explosion).
    * GNN: Our GNN has the following advantages as a short-term multi-behavior encoder.
        * The main point is that GNN is divided into time slots, and there is a part that passes information between time slots. And our self-attention is specifically used to model the multi-relationship between time slots.
        * GNN's initialization module 3.1.5 takes over the information from the previous time-slot.
        * Encodes heterogeneity for multi-behavior.
        * Encodes the information of short-term.


> **Point2**: "The proposed model is too complex...writing is not quite clear... difficult to follow."

* First of all, we would like to thank the reviewer for giving us valuable comments. We have carefully considered our shortcomings pointed out by the reviewer and thought that there must be some possible writing problems, so we have revised the method part. We also put some contents that affect the readers' reading in the supplementary materials<font size="1" color=red>(TODO:.)</font>.



# Reviewer4:

> pass






