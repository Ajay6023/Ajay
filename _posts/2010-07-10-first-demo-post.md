---
layout: post
title: Sample post
date: 2020-07-10 13:32:20 +0300
description: You'll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
image: # Add image post (optional)
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
    Arrays and Their Multiple Facets &middot; John Gamboa
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

\[\begin{align*}
P(X = sunny)  &amp;= \frac{e^{10}}{e^{10} + e^{-9} + e^{-1}} = \frac{22026.46}{224497026.83} = 0.99998 \\ \\
P(X = cloudy) &amp;= \frac{e^{-9}}{e^{10} + e^{-9} + e^{-1}}  = \frac{0.0000647101234}{278482026.83} = 0.000000005678454874784547 \\ \\
P(X = rainy)  &amp;= \frac{e^{-1}}{e^{10} + e^{-9} + e^{-1}}  = \frac{0.3679}{22026.83} = 0.000016744795416 \\ \\
\end{align*}\]

<p>Let’s recapitulate what we learned in the previous blog post. In the
example, I had a signal $f$ that looked like the following:</p>



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

