# Recommendation System in E-commerce & Retail

## <span style="color:red"> Introduction
As an artificial intelligence or AI algorithm, recommendation system is usually associated with Machine Learning, that uses Big Data to suggest or recommend additional products to consumers. These can be based on various criteria, including past purchases, search history, demographic information, and other factors. Recommendation systems are highly useful as they help users discover products and services they might otherwise have not found on their own.

Recommendation systems are trained to understand the preferences, previous decisions, and characteristics of people and products using data gathered about their interactions.  These include impressions, clicks, likes, and purchases. Because of their capability to predict consumer interests and desires on a highly personalized level, recommender systems are a favorite with content and product providers. They can drive consumers to just about any product or service that interests them, from books to videos to health classes to clothing.

## <span style="color:red"> Types of Recommendation Systems

### I) Popularity-Based Recommendation System
It is a type of recommendation system that works on the principle of popularity and/or anything which is in trend. It checks about the product or movie which are in trend or are most popular among the users and directly recommend those. For example, if a product is often purchased by most people then the system will get to know that that product is most popular so for every new user the system will recommend that product. It does not suffer from `cold start` problems.

### II) Collaborative Filtering
Collaborative algorithm recommends items based on preference information from many users. This approach uses similarity of user preference behavior, given previous interactions between users and items, recommender algorithms learn to predict future interaction. The idea is that if some people have made similar decisions and purchases in the past, like a movie choice, then there is a high probability that they will agree on additional future selections. For example, if a collaborative filtering recommender knows you and another user share similar tastes in movies, it might recommend a movie to you that it knows this other user already likes.

### III) Content Filtering
Content filtering uses the attributes of an item to recommend other items similar to the users' preferences. This approach is based on similarity of item and user features, given information about a user and items they have interacted with (e.g., the user's age, the category of a restaurant's cuisine, the average review for a movie), model the likelihood of a new interaction. For example, if a content filtering recommender sees you liked the movies "You've Got Mail and Sleepless in Seattle", it might recommend another movies to you with the same genres and/or cast such as "Joe Versus the Volcano".

### IV) Hybrid Filtering
Combines the advantages of collaborative filtering and content filtering.

### V) Context Filtering
Context filtering includes users' contextual information in the recommendation system process. 
This approach uses a sequence of contextual user actions, plus the current context, to predict the probability of the next action. For example, given one sequence for each user - the country, device, date, and time when they watched a movie - train a model to predict what to watch next.

## <span style="color:red"> Use Cases 

#### Landing Page
1. Popular products: recommend the products that many customers bought
2. Personalized recommendations: generate real-time product recommendations based on each customer's shopping behavior. In any case, as popular product recommendations are great in catering to the mainstream, personalized recommendations increase the sales of long-tail products. It's important to note that personalization in product recommendations requires a lot of behavioral data on users, which the system doesn't see in new visitors. This is known as the `cold start` problem for recommendation system. To avoid this problem, we will "fall back" to a more generalized logic (say, popular products) or category-filtered logic.

#### Product Page
1. Similar products
2. Customer who bought/viewed this ..
3. People like you buy...

#### Cart Page
1. Frequently bought together
2. Recommending accessories

#### Category Page Recommendations
1. Popular products
2. New arrivals
3. Buy together and save

## <span style="color:red"> Benefits of Recommendation Systems
Recommendation systems are a critical component driving personalized user experiences, deeper engagement with customers, and powerful decision support tools in retail, entertainment, healthcare, finance, and other industries. On some of the largest commercial platforms, recommendations account for as much as 30% of the revenue. A 1% improvement in the quality of recommendations can translate into billions of dollars in revenue.

Companies implement recommender systems for a variety of reasons, including:
- Improve retention: by continuously catering to the preferences of users and customers, businesses are more likely to retain them as loyal subscribers or shoppers. 
- Increase sales
- Help form customer habits and trends
- Speed up the pace of work
- Boost cart value

## <span style="color:red"> How Recommendation System Works?

### Matrix Factorization
MF techniques are the core of many popular algorithms, including word embedding and topic modeling, and have become a dominant methodology within `Collaborative Filtering` based algorithms. MF can be used to calculate the similarity in user's ratings or interactions to provide recommendations. 

<center><img align="center" width="240" height="200" src="images/img1.png"></center>

<br>

MF uses the `Alternating Least Squares (ALS)` algorithm to approximate the sparse user item rating matrix (U x I) as the product of two dense matrices, user and item factor matrices of size U x F and F x I (where U is the number of users, I the number of items and F the number of latent features). 

<center><img align="center" width="520
" height="200" src="images/img2.png"></center>

<br>

First matrix can be seen as the user matrix where rows 
represents users and columns are latent factors. The second matrix is the item matrix where rows are latent factors and columns represent items.
The model learns to factorize rating matrix into user and item representations, which allows model to predict better personalized item ratings for users. With MF, less-known items can have richer latent representations as much as popualr movies have, which improves recommender's ability to recommend less-known items.

Notice that the number of latent factors can be tuned via cross-validation. Latent factors are the features in the lower dimension latent space projected from user-item interaction matrix. The idea behind matrix factorization is to use latent factors to represent user preferences or item categories in a much lower dimension space. The number of latent factors determines the amount of abstract information that we want to store in a lower dimension space. A matrix factorization with one latent factor is equivalent to a most popular recommender. Increasing the number of latent factors will improve personalization, until the number of factors becomes too high, at which point the model starts to overfit.

### DL-based Models

#### I) FNN-based: Neural Collaborative Filtering

<center><img align="center" width="450" height="250" src="images/img3.png"></center>

<br>

NCF is a neural network that provides collaborative filtering based on user and item interactions. 

- The bottom input layer consists of two feature vectors $v_u$ and $v_i$ that describe user u and item i, respectively. 
- The embedding layer is a fully connected layer that projects the sparse representation to a dense vector. The obtained user/item embedding can be seen as the latent vector for user/item in the context of latent factor model.
- The user/item embedding are then fed into a multi-layer neural architecture, which we term as `neural collaborative filtering layers`, to map the latent vectors to prediction scores $y_{ui}$.

Paper Link: https://arxiv.org/abs/1708.05031

#### II) CNN-based: Variational Autoencoder for Collaborative Filtering

<center><img align="center" width="450" height="250" src="images/img4.png"></center>

<br>
The VAE-CF is a neural network that provides collaborative filtering based on user and item interactions. The training data for this model consists of pairs of user-item IDs for each interaction between a user and an item.

<br>
The model consists of two parts: the encoder and the decoder. The encoder is a fully connected neural network that transforms the input vector, containing the interactions for a specific user, into an n-dimensional variational distribution. This variational distribution is used to obtain a latent feature representation of a user (or embedding). This latent representation is then fed into the decoder, which is also a feedforward network with a similar structure to the encoder. The result is a vector of item interaction probabilities for a particular user.

Paper Link: https://arxiv.org/pdf/1802.05814.pdf

#### III) RNN-based: Contextual Sequence Learning
Session context-based recommendations apply the advances in sequence modeling from deep learning and NLP to recommendations. RNN models trained on the sequence of user events in a session (e.g., products viewed, data and time of interactions) learn to predict the next items in a session. User item interactions in a session are embedded similarly to words in a sentence. For example, movies viewed are translated into a set of numbers before being fed into RNN variants such as LSTM, GRU, or Transformer to understand the context.

#### IV) DLRM
DLRM is a DL-based model for recommendations. It's designed to make use of both categorical and numerical inputs that are usually present in recommender system training data. 

<center><img align="center" width="450" height="380" src="images/img5.png"></center>

<br>

- To handle the categorical data, embedding layers map each category to a dense representation before being fed into MLP. Numerical features can be fed directly into an MLP.
- At the next level, second-order interactions of different features are computed explicitly by taking the dot product between all pairs of embedding vectors and processed dense features. Those pairwise interactions are fed into a top-level MLP to compute the likelihood of interactions between a user and item pair.

Paper Link: https://arxiv.org/pdf/1906.00091.pdf