---
layout: post
title: "sqrt newton way"
date: 2013-05-12 00:24
comments: true
categories: [scheme, lisp]
---
I was working through a way to get a square-root function to work in scheme.

{% highlight scheme %}
(define (sqrt2 x)
  (sqrt-iter 1 x))

(define (sqrt-iter guess x)
  (if (good-enough guess x)
      guess
      (sqrt-iter (make-guess guess x) x)))

(define (make-guess guess x)
  (/ (+ (/ x guess) guess) 2))

(define (good-enough guess x)
  (< (abs2 (-(* guess guess) x)) 0.001))

(define (abs2 x)
  (if (< x 0)
      (- 0 x)
      x))
{% endhighlight %}

Running: 

{% highlight scheme %}
(sqrt2 100)
{% endhighlight %}

I got the number 10 followed by the precision: 139008452377144732764939786789661303114218850808529137991604824430036072629766435941001769154109609521811665540548899435521 / 993650612510151824074958925538589300439185013461061345483147770187307778405188218146967823138235109552370030745199319835640898137728

A precision I won't forget. COOL!!!
