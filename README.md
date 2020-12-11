# IR Swaption Implied Volatility

Overview
	An implied volatility is the volatility implied by the market price of an option based on the Black-Scholes option pricing model. An interest rate swaption volatility surface is a four-dimensional plot of the implied volatility of a swaption as a function of strike and expiry and tenor. 
	The term structures of implied volatilities which provide indications of the market’s near- and long-term uncertainty about future short- and long-term swap rates. A crucial property of the implied volatility surface is the absence of arbitrage.

Summary
	Swaption Volatility Surface Introduction
	The Summary of Volatility Surface Construction Approaches
	Arbitrage Free Conditions
	The SABR Model
	Swaption Volatility Surface Construction via The SABR Model
	Swaption Volatility

Swaption Volatility Surface Introduction
	An implied volatility is the volatility implied by the market price of an option based on the Black-Scholes option pricing model. 
	An swaption volatility surface is a four-dimensional plot of the implied volatility of a swaption as a function of strike and expiry and tenor. 
	The term structures of implied volatilities provide indications of the market’s near- and long-term uncertainty about future short- and long-term swap rates.
	Vol skew or smile pattern is directly related to the conditional non-nomality of the underlying return risk-neutral distribution. 
	In particular, a smile reflects fat tails in the return distribution whereas a skew indicates return distribution asymmetry.
	A crucial property of the implied volatility surface is the absence of arbitrage.

The Summary of Volatility Surface 
Construction Approaches
	To construct a reliable volatility surface, it is necessarily to apply robust interpolation methods to a set of discrete volatility data. Arbitrage free conditions may be implicitly or explicitly embedded in the procedure. Typical approaches are
	Local Volatility Model: a generalisation of the Blaack-Scholes model.
	Stochastic Volatility Models: such as SABR, Heston, Levy
	Parametric or Semi-Parametric Models: such as SVI, Omega
	Market Volatility Model: directly modeling the implied volatility dynamics
	Interpolation/Extrapolation Model: interpolating or extrapolating volatility data using specific function forms

Arbitrage Free Conditions
	Any volatility models must meet arbitrage free conditions.
	Typical arbitrage free conditions
	Static arbitrage free condition: Static arbitrage free condition makes it impossible to invest nothing today and receive positive return tomorrow. 
	Calendar arbitrage free condition: The cost of a calendar spread should be positive.
	Vertical (spread) arbitrage free condition: The cost of a vertical spread should be positive.
	Horizontal (butterfly) arbitrage free condition: The cost of a butterfly spread should be positive.
	Vertical arbitrage free and horizontal arbitrage free conditions for swaption volatility surfaces depend on different strikes.
	There is no calendar arbitrage in swaption volatility surfaces as swaptions with different expiries and tenors have different underlying swaps and are associated with different indices. In other words, they can be treated independently.
	The absence of triangular arbitrage condition is sufficient to exclude static arbitrages in swaption surfaces.
	Let Sw(t,T_s,T_e,K)  be the present value of a swaption at time t, where  𝑇﷮𝑠﷯ is the start date of the underlying swap;  𝑇﷮𝑒﷯ is the end date of the underlying swap; K is the fixed rate of the underlying swap.
	The triangular arbitrage free conditions are

	Sw(t_1,T_s,T_e,K)≤Sw(t_2,T_s,T_e,K) 		
where t_1≤t_2

Sw(T_1,T_1,T_3,K)≤Sw(T_1,T_1,T_2,K)+Sw(T_2,T_2,T_3,K) 	
where T_1≤T_2≤T_3
At FinPricing, we use the SABR model to construct swaption volatility surfaces following the best market practice.

The SABR Model
	SABR stands for “stochastic alpha, beta, rho” referring to the parameters of the model.
	The SABR model is a stochastic volatility model for the evolution of the forward price of an asset, which attempts to capture the volatility smile/skew in derivative markets.
	There is a closed-form approximation of the implied volatility of the SABR model.
	In the swaption volatility case, the underlying asset is the forward swap rate.
	The dynamics of the SABR model

dF ̂=α ̂F ̂^β dW_1
dα ̂=vα ̂dW_2
dW_1 dW_2=ρdt
α ̂(0)=α
where
	F ̂	the forward swap rate
	α ̂	the forward volatility
W_1,W_2	the standard Brownian motions
ρ	the instantaneous correlation between W_1  and W_2
	The four model parameters (a , b , r, n ) above have the following intuitive meaning:
	𝛼		the volatility that is tied closely to the at-the-money volatility value.
	𝛽 		the exponent that is related to the backbone that the ATM volatility traces when the ATM forward rate varies.
	𝜌 		the correlation that describes the “skew” or average slope of the volatility curve across strike.
	𝑣 		the volatility of volatility that describes the “smile” or curvature/convexity of the volatility curve across the strike for a given term and tenor.

Constructing Swaption Volatility Surface 
via The SABR Model
	For each term (expiry) and tenor of the swaption, conduct the following calibration procedure.
	The 𝛽 parameter is estimated first and typically chosen a priri according to how the market prices are to be observed.
	Alternatively 𝛽  can be estimated by a linear regression on a time series of ATM volatilities and of forward rates.
	After 𝛽 is set, we can obtain 𝛼 by using  𝜎﷮𝐴𝑇𝑀﷯ to solve the following equition
	σ_ATM=α/f^(1-β)  {1+[(α^2 〖(1-β)〗^2)/(24f^(2(1-β)) )+ρβvα/(4f^(1-β) )+((2-3ρ^2)v^2)/24]T}
The Viete method is used to solve this equation
	Given the 𝛼 and 𝛽  solved above,  we can find  the optimized  value  of (𝜌,𝑣) by minimizing the distance between the SABR model output volatilities and market volatilities across all strikes for each term and tenor.
	min∑_(i=1)^n▒[σ_i^SABR (α,β,ρ,v)-σ_i^Market ]^2 
The Levenberg-Marquardt least-squares optimization routine is used for optimization.
	After 𝛼,𝛽,𝜌,𝑣 calibrated, one can generate SABR volatility (swaption volatility) for any moneyness.
	Repeat the above process for each term and tenor.


You can find more details at
https://finpricing.com/lib/FxVolIntroduction.html

