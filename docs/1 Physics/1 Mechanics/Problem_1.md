# Lyapunov Exponent and Chaos in the Logistic Map

## Introduction

The **Lyapunov exponent** quantifies the sensitivity of a dynamical system to initial conditions. For the **logistic map**, given by:

$$ x_{n+1} = r x_n (1 - x_n) $$

where:
- \( x_n \) is the population at iteration \( n \)
- \( r \) is the growth rate parameter

The Lyapunov exponent \( \lambda \) is defined as:

$$ \lambda = \lim_{N \to \infty} \frac{1}{N} \sum_{n=1}^{N} \ln \left| f'(x_n) \right| $$

where \( f'(x) \) is the derivative of the logistic function:

$$ f'(x) = r(1 - 2x) $$

If \( \lambda > 0 \), small differences in initial conditions grow exponentially, indicating **chaotic behavior**.

---

## Python Implementation

The following Python code computes and visualizes the Lyapunov exponent for different values of \( r \):

```python
import numpy as np
import matplotlib.pyplot as plt

def logistic_map(x, r):
    return r * x * (1 - x)

def lyapunov_exponent(r, N=1000, x0=0.5):
    x = x0
    sum_log_deriv = 0
    
    for _ in range(N):
        x = logistic_map(x, r)
        deriv = abs(r * (1 - 2 * x))
        if deriv == 0:
            return -np.inf  # Avoid log(0) issue
        sum_log_deriv += np.log(deriv)
    
    return sum_log_deriv / N

r_values = np.linspace(2.5, 4, 500)
lyapunov_values = [lyapunov_exponent(r) for r in r_values]

plt.figure(figsize=(8, 5))
plt.plot(r_values, lyapunov_values, label='Lyapunov Exponent', color='b')
plt.axhline(0, color='k', linestyle='--')
plt.xlabel('r')
plt.ylabel('Lyapunov Exponent')
plt.title('Lyapunov Exponent of the Logistic Map')
plt.legend()
plt.show()
```

---

## Interpretation

- When **\( \lambda < 0 \)**, nearby points converge, leading to stable fixed points or cycles.
- When **\( \lambda > 0 \)**, the system is chaotic, meaning small perturbations grow exponentially.
- The transition point where \( \lambda = 0 \) marks the boundary between order and chaos.

The plot above shows how the Lyapunov exponent changes as \( r \) increases, revealing the onset of chaos.

---

## Conclusion

The Lyapunov exponent is a crucial tool in understanding **chaotic behavior**. For the logistic map:
- \( r < 3 \) leads to stability.
- \( r \geq 3.57 \) results in chaos.
- The exponent provides a **quantitative** way to distinguish between order and chaos in dynamical systems.

By analyzing this behavior, we gain insights into the unpredictability of nonlinear systems like population dynamics, weather patterns, and financial markets.

