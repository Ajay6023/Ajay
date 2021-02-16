---
layout: post
title: Second Demo Post
date: 2020-07-11 13:32:20 +0300
description: You'll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
image: /assets/images/posts/1.jpg
fig-caption: # Add figcaption (optional)
tags: [Holidays, Hawaii]
---


<html lang="en-us">

  <head>
  <link href="http://gmpg.org/xfn/11" rel="profile">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta http-equiv="content-type" content="text/html; charset=utf-8">

  <!-- Enable responsiveness on mobile devices-->
  <!-- <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=5"> -->

  <title>
    Arrays and Their Multiple Facets &middot; (Author Name) 
  </title>
    <!-- Useless code that was already here>
    
      Arrays and Their Multiple Facets &middot; John Gamboa
    
    <end of the useless code... -->
  <!-- CSS -->
  <link rel="stylesheet" href="/Ajay/assets/css/poole.css">
  <link rel="stylesheet" href="/Ajay/assets/css/syntax.css">
  <link rel="stylesheet" href="/Ajay/assets/css/hyde.css">

  <script type="text/x-mathjax-config"> MathJax.Hub.Config({ TeX: { equationNumbers: { autoNumber: "all" } } }); </script>
  <script type="text/x-mathjax-config">
         MathJax.Hub.Config({
           tex2jax: {
             inlineMath: [ ['$','$'], ["\\(","\\)"] ],
             processEscapes: true
           }
         });
  </script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML" type="text/javascript"></script>

  <!--link rel="stylesheet" href="public/css/poole.css" -->
  <!--link rel="stylesheet" href="public/css/syntax.css" -->
  <!--link rel="stylesheet" href="public/css/hyde.css" -->
  <link rel="stylesheet" href="//fonts.googleapis.com/css?family=PT+Sans:400,400italic,700|Abril+Fatface">

  <!-- Icons -->
  <link rel="apple-touch-icon-precomposed" sizes="144x144" href="public/apple-touch-icon-144-precomposed.png">
                                 <link rel="shortcut icon" href="public/favicon.ico">

  <!-- RSS -->
  <link rel="alternate" type="application/rss+xml" title="RSS" href="/atom.xml">
</head>


  <body class="layout-reverse">


    <div class="content container">
      <div class="post">
  <h1 class="post-title">Arrays and Their Multiple Facets</h1>
  <span class="post-date">31 Jan 2018</span>
  <p>In
<a href="https://vaulttech.github.io/2017/08/12/what-are-convolutions/">my first blog post on Convolutions</a>
(no need to go read there: this blog post is supposed to be
“self-contained”)
I discusssed a little about how it would be a good idea to reinterpret
the discretized version of the 1D function $f$ as a vector with an
infinite number of dimensions. Basically, the only difference between
the two ways of viewing this “list of numbers” was that the vector
lacked a “reference point”, <em>i.e.</em>, the $t$ we had there. Because $f$
was a
very nice type of function that was non-zero only for a certain range
of $t$’s, we found a way to get this reference point back by dropping
the rest of $f$ where $f$ was always zero.</p>

<p>In this blog post, I want to talk about yet another way in which we
can look at a vector (and, consequently, at a function $f$). In the
next few sections, I will recapitulate the ideas presented in
<a href="https://vaulttech.github.io/2017/08/12/what-are-convolutions/">the blog post on Convolutions</a>,
explain the other interpretation of vectors, and show how it may be
useful when training a classifier.</p>

<h2 id="arrays-can-be-reinterpreted-as-discrete-functions">Arrays Can Be Reinterpreted As Discrete Functions</h2>

<p>Let’s recapitulate what we learned in the previous blog post. In the
example, I had a signal $f$ that looked like the following:</p>

<p><img src="/public/fx_continuous.png" alt="The original f function" /></p>

<p>Because we wanted to avoid calculating an integral (the calculation
of the convolution, which was the problem we wanted to solve,
required the solution of an integral), and because we
were not dramatically concerned with numeric precision, we concluded
it would be a good approximation to just use a discrete version of
this signal. We therefore sampled only certain evenly spaced points
from this function, and we called this process “discretization”:</p>

<p><img src="/public/fx_discretized.png" alt="The discretized f" /></p>

<p><em>(In our original setting, $f$ was a function that turned out to be
composed by non-zero values only in a small part of its domain. The
rest was only zeros, extending vastly to the right and to the left
of that region. This was convenient for our convolutions, and will
be convenient too for our discussion below, although most of the
ideas presented below are going to still work if we drop this
assumption.)</em></p>

<p>I would like to introduce some names here, so that I can refer to
things in a more unambiguous way. Let $f_{discretized}$ be the newly
created function, that came into existence after we sampled several
points from $f$, all of which are evenly spaced. Additionally, let
us call $s$ the space between each sample. For the purposes of
this blog post, we will consider we have any arbitrary $s$. It does
not really matter how big or small $s$ is, as long as you (as a
human being) feel that the new discrete function you are defining
resembles well enough (based on your own notion of “enough”) the
original $f$. If you choose an $s$ that is too large, you might
end up missing all non-zero points of $f$ (or taking only
one non-zero point, depending on where you start). If your
$s \to 0$, then you have back the continuous function, and your
discretization had basically no effect.</p>

<p>Your new function $f_{discretized}$ now could be seen as a vector
composed of mostly zeros, except for a small region:</p>

\[f_{discretized} = [\dots 0, 0, 1, 1, 1, 1, 0, 0, \dots]\]

<p>Because this is an infinite array, it is hard to know exactly where
it “starts” (or where it “ends”). In the introduction to this post I
said this was a “problem”, and we had solved it by dropping
the two regions composed exclusively by zeroes:</p>

\[f_{discretized} = [1, 1, 1, 1]\]

<p>Of course, we could have retained some of the zeros, if it was for
any reason convenient to us. It doesn’t matter much. The main idea
here is that we now have a convenient way to represent functions
compactly through vectors. This also means that anything that works
for vectors (dot products, angles, norms) also should have some
interpretation for discrete functions. Think about it!</p>

<h3 id="disclaiming-interlude">Disclaiming Interlude</h3>

<p>To say the truth, I don’t think that the lack of a “reference point”,
as I said before, is a problem at all. From a
“maths” perspective, we could solve this by adopting literally any
element as our “start”, and from there we can index all other
elements. We could even conveniently choose the element that
corresponds to our $t = 0$, and it is almost as if we had $f$ back.
Mathematicians are quite used to deal with “infinity”, and
these seem quite reasonable ideas.</p>

<p>Other human beings, however, would probably not have the same ease,
and our machines have unfortunately a limited amount of memory. We
would like to keep in our memory only the things we actually care
about… and we don’t care a lot about zeros: they kill any number
they multiply with, and work as an identity after the sum.</p>

<h2 id="arrays-can-be-reinterpreted-as-distributions">Arrays Can Be Reinterpreted As Distributions</h2>

<p>It is very likely that, just by reading the heading of this section,
you already got everything you need to know. There is no magic
insight in here: I just intend to go through the ideas slowly and
make it clear why (and, in some ways, how) the heading is true.
If you already got it, I would invite you to skip to the next section,
that tries to show examples when the multiple facets of vectors are
useful. If you stick to me, however, I hope this section may be
beneficial.</p>

<h3 id="what-is-a-distribution">What is a Distribution?</h3>

<p>When I had a course on Statistics in my Bachelor, it was really bad.
At the time of the exam, it seemed I should be much more concerned
with how to round the decimal numbers after the
<a href="https://en.wikipedia.org/wiki/Decimal_mark">comma</a>, than with the
actual concepts I was supposed to have learnt.
As a consequence, I didn’t understand much of statistics when I
started with Machine Learning and it took me a great deal of
self-studying to realize some of the things in this blog post.</p>

<p>One of these things was the meaning of the word <em>distribution</em>. This
is for me a tricky word, and to be fair I might still miss some of its
theoretical details (I just went to Wikipedia, and
<a href="https://en.wikipedia.org/wiki/Probability_distribution">the article on the topic</a>
seems so much more complicated than I’d like it to be). For our
purposes here, I will consider a <em>distribution</em> any function that
satisfies the following two criteria:</p>

<ol>
  <li>It is composed exclusively of positive numbers</li>
  <li>The area below the curve sums up to 1</li>
</ol>

<p><em>(For the avid reader: I am avoiding the word “integral”
because I don’t want to bump into “the integral of a point”, that is
tricky and unnecessary here)</em></p>

<p>There is one more important element to be discussed about
distributions: any distribution is a function of one of more
<em>random variables</em>. These variables represent the thing we are trying
to find the probability of. For example, they might be the <em>height</em>
of the people in a population, the <em>time</em> people take to read a
sentence, or the <em>age</em> of people when they lose their first tooth.</p>

<h3 id="on-discrete-distributions">On Discrete Distributions</h3>

<p>(I actually spent a lot of time writing about how continuous
distributions could be reinterpreted as vectors, but I have the
feeling it was becoming overcomplicated, so I thought I better
dedicate one new blog post to my views on continuous distributions)</p>

<p>I believe you should think of Discrete Distributions as the
collection of the
probabilities that a given random variable assumes any of the values
it can assume. For example, let’s say that my random variable $X$
represents the current weather, and that it can be one of the
following three possibilities: (1) sunny, (2) cloudy, (3) rainy.
Let’s put these three values in a set $\mathcal{X}$, i.e.,
$\mathcal{X} = \{sunny, cloudy, rainy \}$. Then
a probability distribution would tell me all of $P(sunny)$,
$P(cloudy)$ and $P(rainy)$. Let’s say that we know the values for
these three probabilities:</p>

\[\begin{align*}
P(X = sunny)  &amp;= 0.7  \\
P(X = cloudy) &amp;= 0.2  \\
P(X = rainy)  &amp;= 0.1  \\
\end{align*}\]

<p>In that case, it should be easy to conclude that we could represent
this probability distribution with the vector $[0.7, 0.2, 0.1]$.
Yes! It is this simple! Each one of the outcomes becomes one of the
elements of the vector. The ordering is arbitrary. We could have just
as well chosen to create a vector $[0.2, 0.7, 0.1]$ from those three
values.</p>

<h3 id="but-what-if-my-vector-does-not-sum-up-to-1">But What If My Vector Does Not Sum Up To 1</h3>

<p>It may be too easy to transform a distribution into a vector; but
what if I have a vector and would like to transform it into a
probability distribution? For example, let’s say that I have some
computer program that receives all sorts of data (such as the
humidity of the air in several sensors, the temperature, the speed
of the wind, etc.) and just outputs scores for how sunny, cloudy or
rainy it may be. Imagine that one possible vector of scores is
$[101, 379, 44]$. Let’s call it $A$. To facilitate the notation, I
would like to be able to call the three elements of $A$ by the value
of $X$ they represent. So $A_{sunny} = 101$, $A_{cloudy} = 379$, and
$A_{rainy} = 44$.
If I wanted to transform $A$ into a distribution, then how should I
proceed?</p>

<p>There are actually two common ways of doing this. I’ll start by the
naïve way, which is not very common, but could be useful if your
values are really <em>almost</em> summing up to 1. (Really… they just need
some rounding, and you’d like to make this rounding.) In this case,
do it the easy way: just divide each number by the sum of all values
in $A$:</p>

\[P(X = x) = \frac{A_x}{\sum_{i \in \mathcal{X}}{~A_i}}\]

<p>This solution would actually work well for our scores. Let’s see how
it works in practice:</p>

\[\begin{align*}
P(X = sunny)  &amp;= \frac{101}{101 + 379 + 44} = 0.19 \\ \\
P(X = cloudy) &amp;= \frac{379}{101 + 379 + 44} = 0.72 \\ \\
P(X = rainy)  &amp;= \frac{44} {101 + 379 + 44} = 0.08 \\ \\
P(X) &amp;= [0.19, 0.72, 0.08]
\end{align*}\]

<p>While this might seem like an intuitive way of doing things, this is
normally not the way people transform vectors into probabilities.
Why? Notice that this worked well because all our scores were
positive. Take a look at what would have happened if our scores were
$B = [10, -9, -1]$:</p>

\[\begin{align*}
P(X = sunny)  &amp;= \frac{10}{10 - 9 - 1}  = \frac{10}{0} \\ \\
P(X = cloudy) &amp;= \frac{-9}{10 - 9 - 1}  = \frac{-9}{0} \\ \\
P(X = rainy)  &amp;= \frac{-1}{10 - 9 - 1}  = \frac{-1}{0} \\ \\
\end{align*}\]

<p><a href="http://i0.kym-cdn.com/photos/images/facebook/000/008/720/Divide_by_Zero_by_milkman_your_dad.jpg"><em>(Ahem)</em></a></p>

<p>You could argue that I should, then, instead, just take the absolute
values of the scores. This would still not work: the probability
$P(X=cloudy)$ would be almost the same as $P(X=sunny)$,
even though $-9$ seems much “worse” than $10$ (or even worse than
$-1$). Take a look:</p>

\[\begin{align*}
P(X = sunny)  &amp;= \frac{10}{10 + 9 + 1}  = \frac{10}{20} \\ \\
P(X = cloudy) &amp;= \frac{ 9}{10 + 9 + 1}  = \frac{9}{20}  \\ \\
P(X = rainy)  &amp;= \frac{ 1}{10 + 9 + 1}  = \frac{1}{20}  \\ \\
\end{align*}\]

<p>So what is the right way? To make things always work, we want to only
have positive values in our fractions. What kind of function receives
any real number and transforms it into some positive number? You bet
well: the exponential! So what we want to do is to pass each
element of $B$ (or $A$) through an exponential function. To make things
concrete:</p>

\[\begin{align*}
P(X = sunny)  &amp;= \frac{e^{10}}{e^{10} + e^{-9} + e^{-1}} = \frac{22026.46}{22026.83} = 0.99998 \\ \\
P(X = cloudy) &amp;= \frac{e^{-9}}{e^{10} + e^{-9} + e^{-1}}  = \frac{0.0001234}{22026.83} = 0.0000000056 \\ \\
P(X = rainy)  &amp;= \frac{e^{-1}}{e^{10} + e^{-9} + e^{-1}}  = \frac{0.3679}{22026.83} = 0.0000167 \\ \\
\end{align*}\]

<p>The exponential function does amplify a lot the discrepancy between
the values (now $sunny$ has probability almost 1), but it is the
common way of transforming real numbers into a probability
distribution:</p>

\[P(X = x) = \frac{\exp({A_x})}{\sum_{i \in \mathcal{X}}{~\exp({A_i})}}\]

<p>This formula goes by the name of <em>softmax</em> and you should totally get
super used to it: it appears everywhere in Machine Learning!</p>

<h2 id="ok-but-so-what-how-is-this-even-useful">Ok… but… so what? How is this even useful?</h2>

<p>More or less at the same time I was writing this blog post, I was
preparing some class related to Deep Learning that I was
supposed to present at the University of Fribourg (in November/2017). I thought
it would be a good idea to introduce the exact same discussion above to the
people there. When I reached this part of the lecture, it became actually quite
hard to find good reasons why knowing all of the above was useful.</p>

<p>One reason, however, came to my mind, that I liked. If you
know that the vector you have is a distribution (<em>i.e.</em>, if you
are able to interpret it this way), then all of the results you know from
Information Theory should automatically apply. Most importantly, the discussion
above should be able to justify why you would like to use the Cross-Entropy as a
loss function to train your neural network. To make things clearer, let’s say
that you were given many images of digits written by hand (like those I referred
to in <a href="https://jcbgamboa.github.io/2017/09/09/representation-learning-101/">my previous blog post</a>):</p>

<p><img src="https://www.tensorflow.org/images/mnist_digits.png" alt="MNIST digits" /></p>

<p>Now let’s say that you wanted to train a neural network that, given any of these
images, would output the “class” that it belongs to. For example, in the image
above, the first image is of the “class” 5, the second image is of the the
class 0, and so on. If you are used to
<a href="https://en.wikipedia.org/wiki/Backpropagation">backpropagation</a>
then you would (probably thoughtlessly) write your code using something like
the <code class="language-plaintext highlighter-rouge">categorical_crossentropy</code> of
<a href="http://tflearn.org/objectives/#categorical-crossentropy">tflearn</a> (or anything
equivalent). This function receives the output of the network (the values
“predicted” by the network) and the expected output. This expected output is
normally a one-hot encoded vector,
<em>i.e.</em>, a vector with zeros in all positions, except for the position
corresponding to the class of the input, where it should have a 1. In our
example, if the first position corresponds to the class 0, then every time we
gave a picture of a 0 to the network we would also use, in the call to our loss
function, a one-hot encoded vector with a 1 in the first position. If the second
position corresponded to the class 1, then every time we gave a picture of a 1
to the network we would also give a one-hot encoded vector with a 1 in the
second position to our loss function.</p>

<p>If you look at these two vectors, you will realize that both of them can be
interpreted as probability distributions: the “predicted” vector (the vector
output by the network) is the output of a softmax layer; and the “one-hot”
encoded vector always sums up to 1 (because it has zeros in all positions
except one of them). Since both of them are distributions, then we can
calculate the cross-entropy $H(expected, predicted)$ as</p>

\[H(expected, predicted) = - \sum_i{expected_i \log(predicted_i)}\]

<p>and this value will be large when the predicted values are very different from
the expected ones, which sounds like exactly what we would like to have as a
loss function.</p>

<h2 id="conclusion">Conclusion</h2>

<p>Everything discussed in this blog post was extremely basic. I would have been
very thankful, however, if anyone had told me these things before. I hope this
will be helpful to people who are starting with Machine Learning.</p>


</div>

<div class="related">
  <h2>Related Posts</h2>
  <ul class="related-posts">
    
      <li>
        <h3>
          <a href="/2020/04/26/On-inequality-and-the-Coronavirus/">
            On Inequality and the Coronavirus
            <small>26 Apr 2020</small>
          </a>
        </h3>
      </li>
    
      <li>
        <h3>
          <a href="/2018/07/22/On-Linear-Regressions/">
            On Linear Regressions
            <small>22 Jul 2018</small>
          </a>
        </h3>
      </li>
    
      <li>
        <h3>
          <a href="/2017/09/09/representation-learning-101/">
            A (Very Simple) Introduction to Representation Learning
            <small>09 Sep 2017</small>
          </a>
        </h3>
      </li>
    
  </ul>
</div>

    </div>

  </body>
</html>

