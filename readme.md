GitHub User Growth
======

It looks exponential with a decreasing exponent;
think of it as compounded interest on number of users
with a linearly decreasing interest rate.

Call new_users/(total_users - new_users) the interest_rate,
and skip the first 18 months as this growth rate doesn't fit them.
Here's interest rate over time:

	lm(formula = interest_rate ~ when_int, data = g[-1:-18, ])

	Residuals:
	      Min        1Q    Median        3Q       Max 
	-0.043111 -0.004783  0.000957  0.006122  0.027432 

	Coefficients:
		      Estimate Std. Error t value Pr(>|t|)    
	(Intercept)  0.1467628  0.0080344  18.267  < 2e-16 ***
	when_int    -0.0015762  0.0002181  -7.226  3.3e-08 ***
	---
	Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1 

	Residual standard error: 0.01248 on 32 degrees of freedom
	Multiple R-squared:  0.62,Adjusted R-squared: 0.6081 
	F-statistic: 52.21 on 1 and 32 DF,  p-value: 3.303e-08 

Note the that the interest rate decreases by about 0.15% every month.
The plots makes that relationship look very strong and also like assumptions
approximate linearity, homoskedasticity (unvarying variance), &c. are valid.

I tried converting to a continuous interest formula, (That's what's in
my notebook.) but I didn't get very far. So I did monthly compounding;
Here's an estimate of the number of GitHub-users over time

    A = P*(1+r)^t

where

    P = Number of users in the starting month (This must come from a month after June 2009.)
    A = Number of users in the end month
    t = Number of months between the starting and end month (This is 1 for February to March.)
    r = 14.67% - 0.15% * (Number of months since December 2007)
