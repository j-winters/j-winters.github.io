# TC

Transmission chain experiments have become increasingly popular over the last couple of decades. A useful explanation of transmission chains is to think of the childhood game of *Telephone*: 

A standard transmission chain might go something like this: 

Now, let's say we want to investigate the role of explanations in making 

In this blog post, I want to take a closer look at transmission chain experiments using a simple simulation model. 

Let's say you want to design an experiment that tests for the effects of understanding on optimizing some process. 

# A simple experimental task

## Task set up

Like Derex et al, I'm going to have two conditions: *configuration* and *configuration + theory*. But instead of modifying and predicting the speed of wheel, I'll keep things simple and have my simulated participants modify and predict the bias of a coin. A single trial looks something like this:

1. **Generate some data:** You flip a coin ten times and count the number of heads and tails. This is your data, $D$.
2. **Make an inference:** You then infer an underlying process that generated this data. Is the coin *biased* or *unbiased*? To compute this, [Bayes' theorem](https://en.wikipedia.org/wiki/Bayes%27_theorem) is used to generate a posterior probability $P(\theta|D)$ that incorporates our prior beliefs $\theta$ and the data $D$.
3. **Modify the bias of coin:** Armed with your posterior probabilities, you now have the option of changing the bias of the coin in the direction of heads or tails. The challenge, however, is that you do not have direct access to the underlying bias; you just have knowledge about what you think the bias is. Instead, you have to use your posterior probabilities to make an informed decision about changing the bias in one direction or another (at a fixed increment of 0.10).
4. **Make a prediction:** Now it's time to make a prediction. You do this by using your posterior probabilities, as well as your knowledge of the modification, to try and predict the frequency of heads and tails for the next ten coin flips.
5. **Generate new data:** Following the prediction, you are now in a position to generate some new data, which will be used to test the accuracy of our predictions. Again, this involved flipping the coin ten times and counting the number of heads and tails.
6. **Update prior distribution:** Finally, before you begin another trial, you need to update your prior beliefs using the posterior distribution.

\[Maybe just put the procedure above instead of a single trial. Rinse and repeat for five trials. Transmit to next generation.\]

Each generation involves performing five trials (as with Derex et al), before reaching the final step of transmission. Importantly, to see if understanding makes a difference, we can manipulate whether the posterior distribution of an individual is or is not transmitted to the next generation in the chain. Individuals in the *configuration* condition only receive the modified coin, whereas individuals in the *configuration + theory* condition receive both the modified coin and the posterior distribution. This is meant to approximate the transmission of theory in the experiment of Derex et al: individuals either start out naïve (with a prior that assumes an unbiased coin has the highest probability) or knowledgable (in that they update their prior with the posterior probabilities of the previous generation).

Two measures of performance are used here. The first is: How accurately does an individual infer the underlying bias of the coin flipping process (*Understanding*)? The second is: How accurately does an individual predict the frequency distribution of heads and tails (*Prediction*)? The optimal outcome is pretty well-defined in this case: if you want to accurately infer the underlying bias *and* maximize your predictive success, then you want the bias of a coin to be as close to 1.0 (always heads) or 0.0 (always tails) as possible. Or, to think of this in information theoretic terms, you want to push your process to producing low entropy states, where $H(D) = 0$.

## How to successfully predict coin flips using cultural transmission...

The first step is to see what happens with the participant set up of Derex et al where there are 14 chains of 5 generations per condition:

![run_bias_14c_5g.png][nextjournal#file#43595851-b9f4-4fce-82a6-ffb78de6d4f5]

![run_success_14c_5g.png][nextjournal#file#9f947335-181e-4673-858a-c8ab53c13cb2]

What we see for both *understanding* and *predictive performance* is similar to Derex et al: there is little difference between the two conditions. Unlike Derex et al, we do get an initial increase in understanding, which is not too surprising: a biased coin produces more predictable outcomes, making it easier to infer the underlying bias. The predictive performance, however, is strikingly parallel to Derex et al: we get incremental improvements in guessing the outcomes over successive generations. 

## More chains, please...

So far, our results align nicely with Derex et al. What happens if we increase the number of chains by a factor of 8 (112 chains per condition)? This should squash any concerns about non-independence. Well, it seems like the results are broadly comparable:

![run_bias_112c_5g.png][nextjournal#file#41d375b6-d530-4ff8-aa6a-539c45944d7e]

![run_success_112c_5g.png][nextjournal#file#c917091e-b9b0-40a3-8bbf-2c021f542170]

No surprises that the only real difference are smaller s.e.m bars (which is exactly what we should expect given more data). It is interesting that the level of understanding is now marginally better for groups without an explanation.

## It's my generation...

What happens if we go back to 14 chains per condition, but we instead increase the number of generations to, say, 80? Well, now things start to look a little bit more interesting:

![run_bias_14c_80g.png][nextjournal#file#913337f2-7511-4acb-8ec2-d500b4ef2fee]

![run_success_14c_80g.png][nextjournal#file#c221f34c-ec8b-40b8-beeb-c1b27dd04522]

We see that by \~20 generations the two conditions have substantially diverged. Transmitting a theory seems to make a big difference in the long run. Runs without a theory reach an asymptote where there are no further gains past a certain point, whereas runs with a theory are able to reach an optimal outcome. After \~50 generations, individuals in *configuration + theory* are making perfect predictions and have pretty much accurately inferred the underlying bias. 

A similar situation holds if we maximize both the chains and the number of generations:

![run_bias_112c_80g.png][nextjournal#file#3845c1c6-0b2c-47cc-b85b-74b36321905f]

![run_success_112c_80g.png][nextjournal#file#6830b23f-260c-4a82-8678-4c2cd7dc358c]

## What does all this mean?

It's always difficult to generalise beyond the confines of a simple model. Just to be clear: this isn't an exercise in saying that all transmission chains are wrong because they don't have a gazillion generations. My goal is to raise awareness about some of the challenges in designing and executing a transmission chain experiment. 

In particular, for all the advantages of transmission chains, they come with some distinct caveats, namely: How do we organise participants into chains and generations? Simulations can be a useful first step in designing such an experiment. For instance, if you wanted to run the experiment outlined here, then 14 chains of 5 generations is likely to be insufficient. 

\[Come up with a false negative: that there is no difference between the two conditions.\]

\[Map out the space of number of chains and number of generations. Show that we need a minimum of 14 chains of 20 generations per condition. That's n = 560! Of course, you could also...\]

## How to successfully predict coin flips using cultural transmission...

The task is structured as follows. Imagine you flipped a coin ten times and counted the number of heads and tails. Let's say you saw 3 heads and 7 tails: your first step is to try and infer the underlying process that generated this frequency distribution. Is it *biased* or *unbiased*? We can model this relatively simply because coin flips follow a[ binomial distribution](https://en.wikipedia.org/wiki/Binomial_distribution). To compute this, we can use [Bayes' theorem](https://en.wikipedia.org/wiki/Bayes%27_theorem):

$$
P(\theta | D)=\frac{P(D | \theta) P(\theta)}{P(D)}
$$

where $D$ is the observed sequence of 3 heads and 7 tails, $\theta$ is the space of possible hypotheses (e.g., $\theta = 0.5$ assumes the coin is unbiased, $\theta > 0.5$ is biased in favour of heads, and $\theta < 0.5$ is biased in favour of tails), and $P(\theta|D)$ is the conditional probability of the hypotheses given the data. The nice thing about Bayes' theorem is that it allows you to incorporate a prior, $P(\theta)$, which is used here to approximate the prior beliefs about the extent to which a coin is biased. 

For the model here, I think it's reasonable to assume that individuals will start out by believing the coin is unbiased. We can visualise this as a distribution over the hypothesis space: 

![prior.png][nextjournal#file#74205f17-856a-4fc8-8ab7-f99d0c038f7e]

What happens to $P(\theta)$  when incorporate our observations of 3 heads and 7 tails? Well, we can use information from our prior and our observations to generate a posterior distribution: 

![posterior.png][nextjournal#file#b12fad6f-bb55-4421-b8a1-4930a1b0f494]

As you can see, the posterior probability distribution has shifted closer to zero. 

Armed with your posterior probabilities, the next step in the task is to make a prediction about another series of flips. But there's a twist: before you generate this prediction, you have the option of changing the bias of the coin in the direction of heads or tails. The challenge, however, is that you do not have direct access to the underlying bias; you just have knowledge about what you think the bias is. So, instead, you have to use your posterior probabilities to make an informed decision about changing the bias in one direction or another. Finally, the modification of the bias is fixed at a given increment: either + or - 0.10.

# Bonus: Try out the model for yourself

[github-repository][nextjournal#github-repository#0f04f0f9-629b-400e-9c59-44d0327b5a04]

```python id=647955ff-c8c4-4cd7-934c-14eaa9852cc3
import sys
sys.path.insert(0, '/coins/')
```

```python id=3cbc7b09-bc77-4b9a-b482-d55f731139c4
from model import *
flip = coin_flips(flips=10,bias=0.5)
print(flip)
a,b,fig,ax = update_example(data=flip,alpha=2,beta=2)
fig
```

![result][nextjournal#output#3cbc7b09-bc77-4b9a-b482-d55f731139c4#result]

```python id=23d1da4d-e867-461b-87f3-b6673ea8c76b
a,b,fig1,ax1,c,d,fig2,ax2 = runs(chains=14,generations=100,prior=[5,5],flips=10,bias=0.5,explanation=[False,True],modification=True,strength=0.10,plotting=True,trials=5)
```

```python id=68fdb4ff-c89d-46cd-b48d-5b2934a6de8c
fig1
```

![result][nextjournal#output#68fdb4ff-c89d-46cd-b48d-5b2934a6de8c#result]

```python id=6d1ddd0d-1c65-43d2-9e1f-8809db561046
fig2
```

![result][nextjournal#output#6d1ddd0d-1c65-43d2-9e1f-8809db561046#result]

[nextjournal#file#43595851-b9f4-4fce-82a6-ffb78de6d4f5]:
<https://nextjournal.com/data/Qmeh3cS4B974WSR2AP2Ymc4en48v1VAMR9cFyChJ4FAYb9?filename=run_bias_14c_5g.png&content-type=image/png> (Level of understanding for runs with and without a theory. Understanding is operationalized as the distance between the actual bias of the coin and a point estimate sampled from the hypothesis space. Runs = 14; Generations = 5. Error bars show s.e.m \(as in Derex et al\).)

[nextjournal#file#9f947335-181e-4673-858a-c8ab53c13cb2]:
<https://nextjournal.com/data/QmQdQnqdsuNh2FmNhPpLPejqkNxTAd4YP3bfAnop1egtxg?filename=run_success_14c_5g.png&content-type=image/png> (Success rate of predictions for for runs with and without a theory. Runs = 14; Generations = 5. Error bars show s.e.m.)

[nextjournal#file#41d375b6-d530-4ff8-aa6a-539c45944d7e]:
<https://nextjournal.com/data/QmUr6BHph7TjKwpEaRKBGpc7V8gjXsVa19i6MnVCUP66G5?filename=run_bias_112c_5g.png&content-type=image/png> (Level of understanding for runs with and without a theory. Runs = 112; Generations = 5.)

[nextjournal#file#c917091e-b9b0-40a3-8bbf-2c021f542170]:
<https://nextjournal.com/data/QmcYdf4SNSFizaXZXBaxTTcijevvcjmdG4ZUuE2VbBaaqY?filename=run_success_112c_5g.png&content-type=image/png> (Success rate of predictions for for runs with and without a theory. Runs = 112; Generations = 5.)

[nextjournal#file#913337f2-7511-4acb-8ec2-d500b4ef2fee]:
<https://nextjournal.com/data/Qmc2LBuJAwQayLpkyqmh5NPfyyHG1MH9nmCZYia79xjVi6?filename=run_bias_14c_80g.png&content-type=image/png> (Level of understanding for runs with and without a theory. Runs = 14; Generations = 80.)

[nextjournal#file#c221f34c-ec8b-40b8-beeb-c1b27dd04522]:
<https://nextjournal.com/data/Qmf1DBxtrgAcZENiDyy7a8yWiMgcq5WoTSw8ber3MhY4dU?filename=run_success_14c_80g.png&content-type=image/png> (Success rate of predictions for for runs with and without a theory. Runs = 14; Generations = 80.)

[nextjournal#file#3845c1c6-0b2c-47cc-b85b-74b36321905f]:
<https://nextjournal.com/data/QmaY9TBbV4w5skWCvwQtZhqBaxVxSoNjrhTPXBNVPTyoGx?filename=run_bias_112c_80g.png&content-type=image/png> (Level of understanding for runs with and without a theory. Runs = 112; Generations = 80.)

[nextjournal#file#6830b23f-260c-4a82-8678-4c2cd7dc358c]:
<https://nextjournal.com/data/QmPQxrw7kfTgtfyfHeHAFN7qnyhFxX3a9D21tFM4xbfCF7?filename=run_success_112c_80g.png&content-type=image/png> (Success rate of predictions for for runs with and without a theory. Runs = 112; Generations = 80.)

[nextjournal#file#74205f17-856a-4fc8-8ab7-f99d0c038f7e]:
<https://nextjournal.com/data/QmRZ9aRwGkgmTrkW6dJ3HkFqTYtPiftscZZLiyB6Esf5J2?filename=prior.png&content-type=image/png>

[nextjournal#file#b12fad6f-bb55-4421-b8a1-4930a1b0f494]:
<https://nextjournal.com/data/QmbpQ8xNYBawb9267Tp45dEBS7f3jWDMJbo6yNSwudW5ug?filename=posterior.png&content-type=image/png>

[nextjournal#github-repository#0f04f0f9-629b-400e-9c59-44d0327b5a04]:
<https://github.com/j-winters/coins>

[nextjournal#output#3cbc7b09-bc77-4b9a-b482-d55f731139c4#result]:
<https://nextjournal.com/data/QmNpvaJzGdmRz8ab6dcwxBZ8TLQea447sr4FcHqhGvwgfA?content-type=image/svg%2Bxml>

[nextjournal#output#68fdb4ff-c89d-46cd-b48d-5b2934a6de8c#result]:
<https://nextjournal.com/data/QmRd5jQEYyG6j2qqhoaN3BrFvSyN3RYQd3fzAcS7GVPr75?content-type=image/svg%2Bxml>

[nextjournal#output#6d1ddd0d-1c65-43d2-9e1f-8809db561046#result]:
<https://nextjournal.com/data/QmQkmsuNNUtoCC93NadHjKgzJyoKbQjTeRVZMVrYJMzjn7?content-type=image/svg%2Bxml>

<details id="com.nextjournal.article">
<summary>This notebook was exported from <a href="https://nextjournal.com/a/MZFoUwt3oqkE5vQErnWzr?change-id=ChkUMbj78uZH8dpU9gZB1n">https://nextjournal.com/a/MZFoUwt3oqkE5vQErnWzr?change-id=ChkUMbj78uZH8dpU9gZB1n</a></summary>

```edn nextjournal-metadata
{:article
 {:nodes
  {"0f04f0f9-629b-400e-9c59-44d0327b5a04"
   {:id "0f04f0f9-629b-400e-9c59-44d0327b5a04",
    :kind "github-repository",
    :private? true,
    :ref "master"},
   "23d1da4d-e867-461b-87f3-b6673ea8c76b"
   {:compute-ref #uuid "ec57fe3f-1387-4006-bd72-1e9f656bf48d",
    :exec-duration 105536,
    :id "23d1da4d-e867-461b-87f3-b6673ea8c76b",
    :kind "code",
    :output-log-lines {:stdout 15},
    :runtime [:runtime "7380d768-741c-48b5-9a28-f8fb4dec70b6"]},
   "3845c1c6-0b2c-47cc-b85b-74b36321905f"
   {:id "3845c1c6-0b2c-47cc-b85b-74b36321905f", :kind "file"},
   "3cbc7b09-bc77-4b9a-b482-d55f731139c4"
   {:compute-ref #uuid "e4f14a65-93f2-4858-9630-7eb7b4ffe5f7",
    :exec-duration 701,
    :id "3cbc7b09-bc77-4b9a-b482-d55f731139c4",
    :kind "code",
    :output-log-lines {:stdout 3},
    :runtime [:runtime "7380d768-741c-48b5-9a28-f8fb4dec70b6"]},
   "41d375b6-d530-4ff8-aa6a-539c45944d7e"
   {:id "41d375b6-d530-4ff8-aa6a-539c45944d7e", :kind "file"},
   "43595851-b9f4-4fce-82a6-ffb78de6d4f5"
   {:id "43595851-b9f4-4fce-82a6-ffb78de6d4f5", :kind "file"},
   "647955ff-c8c4-4cd7-934c-14eaa9852cc3"
   {:compute-ref #uuid "7eb78d7f-93a9-4557-b7bf-70bc12bd6994",
    :exec-duration 139,
    :id "647955ff-c8c4-4cd7-934c-14eaa9852cc3",
    :kind "code",
    :output-log-lines {},
    :runtime [:runtime "7380d768-741c-48b5-9a28-f8fb4dec70b6"]},
   "6830b23f-260c-4a82-8678-4c2cd7dc358c"
   {:id "6830b23f-260c-4a82-8678-4c2cd7dc358c", :kind "file"},
   "68fdb4ff-c89d-46cd-b48d-5b2934a6de8c"
   {:compute-ref #uuid "ebe474fc-5f26-4bbd-b478-a37739d325ad",
    :exec-duration 666,
    :id "68fdb4ff-c89d-46cd-b48d-5b2934a6de8c",
    :kind "code",
    :output-log-lines {},
    :runtime [:runtime "7380d768-741c-48b5-9a28-f8fb4dec70b6"]},
   "6d1ddd0d-1c65-43d2-9e1f-8809db561046"
   {:compute-ref #uuid "689eaba6-4ad7-4f25-a03e-7f9ee2d0abdb",
    :exec-duration 661,
    :id "6d1ddd0d-1c65-43d2-9e1f-8809db561046",
    :kind "code",
    :output-log-lines {},
    :runtime [:runtime "7380d768-741c-48b5-9a28-f8fb4dec70b6"]},
   "7380d768-741c-48b5-9a28-f8fb4dec70b6"
   {:environment
    [:environment
     {:article/nextjournal.id
      #uuid "5b45e08b-5b96-413e-84ed-f03b5b65bd66",
      :change/nextjournal.id
      #uuid "5df5e18c-0be4-4d8d-b099-6ce55ca12cf4",
      :node/id "0149f12a-08de-4f3d-9fd3-4b7a665e8624"}],
    :id "7380d768-741c-48b5-9a28-f8fb4dec70b6",
    :kind "runtime",
    :language "python",
    :type :nextjournal,
    :runtime/mounts
    [{:src [:node "0f04f0f9-629b-400e-9c59-44d0327b5a04"],
      :dest "/coins"}]},
   "74205f17-856a-4fc8-8ab7-f99d0c038f7e"
   {:id "74205f17-856a-4fc8-8ab7-f99d0c038f7e", :kind "file"},
   "913337f2-7511-4acb-8ec2-d500b4ef2fee"
   {:id "913337f2-7511-4acb-8ec2-d500b4ef2fee", :kind "file"},
   "9f947335-181e-4673-858a-c8ab53c13cb2"
   {:id "9f947335-181e-4673-858a-c8ab53c13cb2", :kind "file"},
   "b12fad6f-bb55-4421-b8a1-4930a1b0f494"
   {:id "b12fad6f-bb55-4421-b8a1-4930a1b0f494", :kind "file"},
   "c221f34c-ec8b-40b8-beeb-c1b27dd04522"
   {:id "c221f34c-ec8b-40b8-beeb-c1b27dd04522", :kind "file"},
   "c917091e-b9b0-40a3-8bbf-2c021f542170"
   {:id "c917091e-b9b0-40a3-8bbf-2c021f542170", :kind "file"}},
  :nextjournal/id #uuid "02dec0dc-f06e-4dce-bf1f-501ed08cbaa7",
  :article/change
  {:nextjournal/id #uuid "5ec51100-42b4-4a50-a6af-5b9a0b158345"}}}

```
</details>
