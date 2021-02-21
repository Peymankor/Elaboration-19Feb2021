## Objective:

A Diabetes doctor tries to Maximize the "Reduction of Sugar in Blood" of her patient - To do each month, she tries one of *potential medication* (here she has 5 options) and at the end of month, she take measuremnet from her patient and try to figure out which medication is best for next month.

## Alternatives 

At the edn of each month, she must make decision to perscribe *one* drug - And her options are the following 5 medication:

$\chi=[x_1,x_2,x_3,x_4,x_5]$

These drug names are (decision aletrnatives), where decision are made sequentially -Note that at each month you pick any of thses drusg, so your alternatives during these process are always *these Five types of drugs*:

## Source of Uncertainity:

You do not know how much each medication *Reduce Sugar Level* of your patient. Over the process you make decions and observe the measuremnet, you learn about that...

## Start the Processs of Sequential Decision Making:

Now at time , $n=0$, her initial knoweldge about the effectivenss of the drugs (in other word how much each drug reduce sugar level of patients is):
Note :( These are the measuremnt have been taken from millions of people with diabetes problem so give some estimate, say *initial belief*, however your *patient's* reaction to each medication is unique)

$$S^0 = [Norm(\overline{\mu}_{x=1}^{n=0}=30,\beta^{n=0}_{x=1}=0.1),\\ Norm(\overline{\mu}_{x=2}^{n=0}=25,\beta^{n=0}_{x=2}=0.1), \\Norm(\overline{\mu}_{x=3}^{n=0}=20,\beta^{n=0}_{x=3}=0.1),\\ Norm(\overline{\mu}_{x=4}^{n=0}=15,\beta^{n=0}_{x=4}=0.1),\\ Norm(\overline{\mu}_{x=5}^{n=0}=10,\beta^{n=0}_{x=5}=0.1)]$$ -

*Note*: Here I use term $\beta$ instead of $\sigma^2$ just to make Bayesian updating easier,($\beta_x^n= \frac{1}{(\sigma_x^n)^2}$)

Now, how she must decide to decide which of the drug is picked, given the objective is *feedback* she recives from patinet after one month and she measure patinet sugar level at $n=1$ . Now the way she use this policy,:


$$
X^{UCB}(S_n|\theta^{UCB}) = argmax(\overline{\mu}_x^n + 0.5\sqrt{\frac{log(n+1)}{N_x^n+1}}  \\
x \in \chi
$$

**1) Pure Exploitation Policy**

Now let's say her policy is *Pure Exploitatio*- It means the term $\theta^{UCB}=0$  <br/>
It means she slecet the drug that highest $\overline{\mu}^0$ so, her poicy is: <br/>

$$
X^{UCB}(S_n|\theta^{UCB}) = argmax(\overline{\mu}_x^n)  \\
x \in \chi
$$

*Note*: The reason this policy is *Pure Exploitation* is the doctor only relies on her best estimate, and do not try to learn about other drugs.

based on abovey policy, her choice will be $x_1$ - Now at time  $n=1$ she take test to see how much the patient's sugar level has decreased and she observe it is $W_{x=1}^{n=1}=25$.

$$
\overline{\mu}_x^{n+1} = \frac{\beta_x^n\overline{\mu}_x^n + \beta_x^WW_x^{n+1}}{\beta_x^n + \beta_x^W} \\ 
\\
\beta^{n+1}_x = \beta_x^n + \beta^W
$$

Now, she must update "her" belief about effectivenes of the drug she used, $x_1$ and she does that with bayes updating:

$$
\overline{\mu}_{x=1}^{1} = \frac{0.1 \times 30 + 25 \times 4}{0.1 + 4} =\frac{28}{1.1} = 25.12 \\
\beta_{x=1}^{1} = 0.1 + 4 = 4.1
$$

*Note*: I use $\beta_x^W$=4 as the *percision of measuremnt* generally should be high... 

Having updating our belief about drug 1 our state is:

$$S^1 = [Norm(\overline{\mu}_{x=1}^{n=1}=25.12,\beta^{n=1}_{x=1}=4.1),\\ Norm(\overline{\mu}_{x=2}^{n=1}=25,\beta^{n=1}_{x=2}=0.1), \\Norm(\overline{\mu}_{x=3}^{n=1}=20,\beta^{n=1}_{x=3}=0.1),\\ Norm(\overline{\mu}_{x=4}^{n=1}=15,\beta^{n=1}_{x=4}=0.1),\\ Norm(\overline{\mu}_{x=5}^{n=1}=10,\beta^{n=1}_{x=5}=0.1)]$$

Now, if we continue our policy, we again use the drug $x_1$ because it has highest $\mu$, and we do not try other drug, because if we do not test other drugs on our patients, we do do not learn about other drugs...
So, in both $n=0,n=1$ we use the drug $x_1$. Doctor's sequence of decision is: <br/>
$[x_1,x_1]$


This policy has weakness that, we only *exploiting* what we know at that time, not *eploring* other drugs. Now lets' change policy and have $\theta^{UCB} = 0.5$

**2) Pure Exploitation and Exploration Policy**

Now, with this $\theta$ our policy becomes:

$$
X^{UCB}(S_n|\theta^{UCB}) = argmax(\overline{\mu}_x^n + 0.5\sqrt{\frac{log(n+1)}{N_x^n+1}}  \\
x \in \chi
$$

For $n=0$, we can see that again, our choice will be drug number $x_1$, as trem inside the square root is zero at time $n=0$. After deciding on drug $x_1$, we observe  the decrease in patient sugar level is $W_{x=1}^{n=1}=25$.- and we update our state, exactly like the one did. Now, we are at time $n=1$ and make decision- I am calculating the pranthzes term for each drug:

$$
 \\
x=x_1; \ \ \overline{\mu}_{x_1}^{n=1} + 0.5\sqrt{\frac{log(n+1)}{N_x^n+1}} = 25.12 + 0.5 \times \sqrt{\frac{log(2)}{1+1}}= 25.41\\ 
x=x_2; \ \ \overline{\mu}_{x_2}^{n=1} + 0.5\sqrt{\frac{log(n+1)}{N_x^n+1}} = 25 + 0.5 \times \sqrt{\frac{log(2)}{0+1}}= 25.42 \\
x=x_3; \ \ \overline{\mu}_{x_3}^{n=1} + 0.5\sqrt{\frac{log(n+1)}{N_x^n+1}} = 20 + 0.5 \times \sqrt{\frac{log(2)}{0+1}}= 20.42 \\
x=x_4; \ \ \overline{\mu}_{x_4}^{n=1} + 0.5\sqrt{\frac{log(n+1)}{N_x^n+1}} = 15 + 0.5 \times \sqrt{\frac{log(2)}{0+1}}= 15.42 \\
x=x_5; \ \ \overline{\mu}_{x_5}^{n=1} + 0.5\sqrt{\frac{log(n+1)}{N_x^n+1}} = 10 + 0.5 \times \sqrt{\frac{log(2)}{0+1}}= 10.42 \\ 
$$

As we can see for time $n=1$, the drug $x_2$ is selected and now the course of action is:

$[x_1,x_2]$
