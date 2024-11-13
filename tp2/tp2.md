![PNS](http://caillau.perso.math.cnrs.fr/logo-pns.png)
## MAM5-INUM - Commande optimale
# TP 2 - Tir simple

On considère le problème de contrôle optimal suivant :

$$ \frac{1}{2} \int_0^1 |u(t)|^2\ \mathrm{d}t \to \min $$

sous les contraintes

$$ \dot{x}(t) = -x(t)+u(t),\quad u(t) \in \mathbf{R},\quad t \in [0,1],\quad
   x(0)=-1,\quad x(1)=0. $$

Résoudre le problème numériquement à l'aide d'une méthode de tir.

```julia
using OptimalControl, OrdinaryDiffEq, MINPACK, Plots

t0 = 0.0
tf = 1.0
x0 = -1.0
xf_fixed = 0.0 # target

# Maximised Hamiltonian
h(x, p) = 0.0 # TO BE UPDATED

# Makes flow from Hamiltonian
f = Flow(Hamiltonian(h)) # (xf, pf) = f(t0, x0, p0, tf)

# Shooting function
function shoot!(s, p0)
    s[1] = 0.0 # TO BE UPDATED
end

# Solve
p0_guess = 1.0 # initial guess
sol = fsolve(shoot!, [ p0_guess ])
p0 = sol.x[1]

# Plots
guess = f((t0, tf), x0, p0_guess)
fig1 = plot(guess, xlabel="t", label=[ "x (guess)" "p (guess)" ], linestyle=:dash)
sol = f((t0, tf), x0, p0)
plot!(fig1, sol, label=[ "x (sol)" "p (sol)"], linestyle=:solid)
plot!(fig1, [ t0, tf ], [ xf_fixed[1], xf_fixed[1] ], label="target", colour=:black)
```
