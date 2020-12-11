---
layout: default
mathjax: true
---

# The Bitcoin Equation

In this article, we present the equation used  in Bitcoin to create the mechanism by which addresses are generated and transactions are signed in secure manner.

The equation used in Bitcoin is the following:
> $$ y^2 = x^3 + 7 $$

If you were to place all the possible values that satisfy this equation on a graph, it would look this: 

![ECDSA Curve used in Bitcoin](../../assets/images/ECDSA_Curve.png) 

Because this curve includes integers and fractions, this presents a challenge because computers are very inefficient with fractions relative to integers.  How does Bitcoin utilize the properties of this equation without having to deal with fractions and decimals?  The answer is in the [modular arithmetic](Modular_Arithmetic.html), presented in the previous section.

Bitcoin uses modular arithmetic to "map" all values of the above equation into values from 0 to a very large specially chosen number, *p*:

	p = 115792089237316195423570985008687907853269984665640564039457584007908834671663

Looking at the Bitcoin equation broken down and applying the modulus operation (%) with p, we have:

    (y*y)  % p = (x*x*x + 7) % p

Recall from the previous article that the order by which you apply modular arithmetic to numbers in an equation does not matter.  Also recall that it applies to any amount of addition and, hence, multiplication.  Applying mod p to all parts of the equation produces an equation that remains valid.  Amazingly, it retains all the important of properties of the original equation.

Because of the *modulus p*,  instead of a continuous line, we will have only points on the curve and they will be integers instead of fractions.

To illustrate lets see what the points look like with p = 37, a much smaller and more palatable number for humans.

![Points on ECC with p = 37](../../assets/images/ECDSA_Curve_37.png) 

This doesn't look exactly like the original curve, but since they are values of the equation they share the key properties.

Notice that the original curve can be 'cut in half'.  That is, it is symetric to the x axis.  In the case of the points, each point has a symetric point on the other side of the middle of the graph.  Instead of the middle being at the x-axis, it is actually half way in between *p*. This will be useful for the Bitcoin operations presented in the next article.

Applying the equation over modulus of 37 produces a specific number points.  The list of points are as follows:

(0,9) | (0,28) | (3,16) | (3,21) | (4,16) | (4,21) | (5,13) |
(5,24) | (6,1) | (6,36) | (8,1) | (8, 36) | (9,12) | (9,25) | 
(12,12) | (12,25) | (13,13) | (13, 24) | (16,12) | (16,25) | (17,6) |
 (17,31) | (18,17) | (18, 20) | (19,13) | (19,24) | (22,6) | (22,31) |
 (23,1) | (23,36) | (24, 17) | (24,20) |(30,16) | (30,21) | (32,17) | 
 (32, 20) | (35,6) | (35,31)

We can plug the values (x,y) of a point into the equation `y*y = x*x*x + 7` to verify that they are indeed valid points.  Let's try with point (35,6):

    (6*6) % 37 = (35*35*35 + 7) % 37
    36 % 37 = 42875 % 37
    36 = 36

Thus, we find that it is a valid point on the curve.

Now, imagine doing this but with the enormously large number used in Bitcoin.  Instead of the 38 points above, you will get a number of points somewhere near the *p* value used for Bitcoin. This is a number that is too unfathomably large for the human brain to comprehend.

As a sample, here is a point on the ECC curve in Bitcoin:

	x = 70895907010527395910842532980404498586816257565274307394361449397355223777196      
	y = 49249905081422558277826014068961381565912223159952377868086244327991468591684
	
Using Python, we can prove it is indeed a point on the curve.  The following code tests whether both sides of the equation are equal:

    >>> y * y % p == (x * x * x + 7) % p
    True

With this basis, we can now look at some properties of these points and demonstrate how Bitcoin uses them to create Bitcoin addresses, as well as enabling signing of transactions.
