## Precision and Recall @ K (Evaluation of Recommender Systems)

### Problem Statement
A common problem in evaluation of recommender systems is to compute 
the precision@k and recall@k. Here, I discuss the procedure I'm using 
to compute the precision/recall @k in a topK recommender system.

Lets say we have N userIds in the ecosystem, and we have a 32 
floating-point embedding for each userId.

We would like to compute the precision and recall for a topK 
recommender system, assuming k = 300.

Here, the system is recommending users who are similar to other users, 
so user embeddings whose cosine distance with other user embeddings is highest.

Using brute force, compute the top-300 userIds for each userId
Using hnsw algorithm, compute the top-300 userIds for each userId
Now, we assume that brute force gives us the optimal set of 300 
recommendations for each userId.

Note that its possible to get fewer than 300 recommendations. 
For example, if we limit the cosine distance to be >= 0.9, we might 
end up with fewer than 300 users who are similar to the query userId.


---

### Solution

We will compute the precision and recall for each userId in the system. 
Lets set K = 300, and a query userId M, lets say we have the 
following different cases, then the precision and recall will be as follows:

| Brute Force | Recommender Algorithm | Common | Precision | Recall  |
|-------------|-----------------------|--------|-----------|---------|
|relevant items | recommended items | recommended items that are relevant | common/recommended | common/relevant  |
| 300         | 300                   | 200    | 200/300   | 200/300 |
| 250         | 300                   | 200    | 200/300   | 200/250 |
| 300         | 180                   | 160    | 160/180   | 160/300 |



Once we compute the precision per userId, we can then just take the 
average precision and average recall and use that as an evaluation 
measure of the recommender system.



#### Corner Cases
Incase the number of "relevant" items is 0, a division by zero is 
possible. In that case, we will use recall as 1.0.
Incase the number of "recommended" items is 0, a division by 
zero is possible. In that case, we will use precision as 1.0.

## Glossary

| Term | Definition                                                                                  |
|-------------|---------------------------------------------------------------------------------------------|
|cosine similarity | cosine of the angle between two vectors (or 32-dimensional vectors/embeddings in this case) |
|cosine distance | 1 - cosine similarity                                                                       |
|precision @k | Precision at k is the proportion of recommended items in the topK set that are relevant     |
|recall @k | Recall at k is the proportion of relevant items found in the topK recommendations           |

## References

 - https://medium.com/@m_n_malaeb/recall-and-precision-at-k-for-recommender-systems-618483226c54
 - https://stackoverflow.com/questions/33697625/recall-recall-ratek-and-precision-in-top-k-recommendation
 - https://ieeexplore.ieee.org/document/6100061
 - https://youtu.be/mpv8iMe24-Q
 - https://www.youtube.com/watch?v=BD9TkvEsKwM