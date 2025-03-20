# European Options via Monte Carlo Simulation

This notebook implements European option pricing using a Monte Carlo simulation approach. European options can only be exercised at maturity, which simplifies the simulation process.

## Key Features
- **Stock Price Simulation:** Uses geometric Brownian motion to generate many random stock price paths.
- **Payoff Calculation:** Computes the option payoff at maturity (e.g., for a call: `max(S_T - K, 0)`).
- **Discounting:** Averages the payoffs and discounts them back to the present using the risk-free rate.
- **Variance Reduction (Optional):** Implements antithetic variates to improve simulation efficiency.

## How It Works
1. **Simulate Stock Paths:**  
   Generate random paths for the underlying asset using its drift and volatility.
2. **Compute Terminal Payoffs:**  
   For each path, calculate the payoff at maturity.
3. **Discount and Average:**  
   Discount the average payoff at the risk-free rate to obtain the present option price.



---




# American Options via Binomial Trees

This notebook implements American option pricing using a binomial tree method. American options allow early exercise, and the binomial tree approach captures this by making optimal exercise decisions at each node.

## Key Features
- **Tree Construction:**  
  Builds a binomial tree of possible stock prices over time.
- **Backward Induction:**  
  At each node, the option value is computed by comparing the immediate exercise value with the discounted continuation value.
- **Greeks Calculation:**  
  Estimates option sensitivities (Greeks) using finite difference approximations by perturbing input parameters.
- **Visualizations:**  
  Generates plots of:
  - Option price versus time to maturity.
  - Option value versus spot price at time 0.

## How It Works
1. **Construct the Tree:**  
   Starting from the current stock price \(S_0\), compute the possible up and down moves over a set number of time steps.
2. **Terminal Payoffs:**  
   At maturity, set the option value equal to its payoff (e.g., for a put: `max(K - S, 0)`).
3. **Backward Induction:**  
   Work backward from maturity. At each node, choose the maximum of the immediate exercise value and the discounted expected future value.
4. **Interpolation:**  
   Interpolate the computed values to obtain the option price at the exact initial stock price.
5. **Compute Greeks:**  
   Use finite difference approximations by perturbing parameters (like \(S_0\)) to estimate sensitivities such as Delta and Gamma.



---




# American Options via Finite Difference Method

This notebook implements American option pricing by solving the Black–Scholes PDE using a finite difference method. An iterative PSOR (Projected Successive Over-Relaxation) technique is employed to enforce the early exercise constraint.

## Key Features
- **Grid Setup:**  
  Discretizes the stock price range (from 0 to a maximum value) and the time to maturity into a grid.
- **PDE Discretization:**  
  Approximates the Black–Scholes PDE using finite differences for spatial and time derivatives.
- **PSOR Iteration:**  
  Iteratively solves the resulting linear system while ensuring that the option value at each grid point is not less than the intrinsic payoff.
- **Visualizations:**  
  Provides plots of:
  - Option price versus time to maturity.
  - Option value versus spot price at time 0.

## How It Works
1. **Set Up the Grid:**  
   - Create a spatial grid for stock prices using `np.linspace(0, Smax, M+1)`.
   - Divide the time to maturity into \(N\) time steps (\(\Delta t = T/N\)).
2. **Terminal Condition:**  
   At maturity, the option value is set equal to the intrinsic payoff (e.g., for a put: `max(K - S, 0)`).
3. **Finite Difference Approximation:**  
   Replace the continuous derivatives in the Black–Scholes PDE with finite difference approximations, forming a tridiagonal system.
4. **PSOR Iterative Solution:**  
   Use the PSOR method to solve the system at each time step, repeatedly updating and enforcing the constraint \(V \>= payoff \).
5. **Interpolation:**  
   After stepping back to time 0, interpolate the solution to obtain the option price at the initial stock price.

