GitHub User Growth
======

We have [counts of GitHub users over time](https://api.scraperwiki.com/api/1.0/datastore/sqlite?format=csv&name=github_users_each_year&query=select+*+from+`swdata`&apikey=). Let's predict how this number changes.

Call new_users/(total_users - new_users) the interest_rate,
and skip the first 18 months as this growth rate doesn't fit them.
Here's interest rate over time:

	lm(formula = interest_rate ~ when_int, data = g[-1:-18, ])
        # when_int is the number of months after December 2007.
        # interest_rate is really last month's interest rate;
        # it's new_users/(total_users - new_users).

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
The plots (github-growth[1-4].pdf) make that relationship look very
strong and makes the assumptions approximate linearity, homoskedasticity
(unvarying variance), &c. seem valid.

I tried converting to a continuous interest formula, (That's what's in
my notebook.) but I didn't get very far. So I did monthly compounding;
Here's an estimate of the number of GitHub-users over time

    A[t+1] = A[t] * ( 1 + r[t] )

where

    A[t]   = Number of users in the current month (This must come from a month after June 2009.)
    A[t+1] = Number of users in the next month
    r[t]   = 14.67% - 0.15% * (Current number of months since December 2007)

And you can fix that so it chains months together and
averages them and whatnot.

So the growth looks exponential with a decreasing exponent;
think of it as compounded interest on number of users with
a linearly decreasing interest rate.
