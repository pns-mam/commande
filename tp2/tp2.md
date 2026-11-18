![PNS](http://caillau.perso.math.cnrs.fr/logo-pns.png)
## MAM5-INUM - Commande optimale
# **TP 2 - Tir simple**

On considère le problème de contrôle optimal suivant :

$$ \frac{1}{2} \int_0^1 |u(t)|^2\,\mathrm{d}t \to \min $$

sous les contraintes

$$ \dot{x}(t) = -x(t)+u(t),\quad u(t) \in \mathbf{R},\quad t \in [0,1], $$

$$ x(0)=-1,\quad x(1)=0. $$

Résoudre le problème numériquement à l'aide d'une méthode de tir.

## Questions

1. Écrire le pseudo-Hamiltonien $H(x,p,u)$ (cas normal, $p^0=-1$), puis maximiser en $u$ pour obtenir le Hamiltonien maximisé $h(x,p)$.
2. Compléter le Hamiltonien `h` dans le code ci-dessous (premier `TO BE UPDATED`).
3. Écrire la fonction de tir `shoot!` : l'inconnue est $p_0=p(0)$ et l'équation à résoudre est la condition finale sur $x(1)$ (second `TO BE UPDATED`).
4. Résoudre, puis vérifier le résultat : calculer à la main l'extrémale ($\dot{p}=p$) et comparer $p_0$ numérique et exact.

## Code

```julia
using OptimalControl, OrdinaryDiffEq, MINPACK, Plots

t0 = 0.0
tf = 1.0
x0 = -1.0
xf_fixed = 0.0 # target

# Maximised Hamiltonian
h(x, p) = 0.0 # TO BE UPDATED

# Makes flow from Hamiltonian
f = Flow(OptimalControl.Hamiltonian(h)) # (xf, pf) = f(t0, x0, p0, tf)

# Shooting function
function shoot!(s, p0)
    s[1] = 0.0 # TO BE UPDATED
end

# Solve
p0_guess = 1.0 # initial guess
sol_minpack = fsolve(shoot!, [p0_guess])
sol_minpack.converged || error("MINPACK did not converge")
p0 = sol_minpack.x[1]
println("p0 = ", p0)

# Plots
guess = f((t0, tf), x0, p0_guess)
fig1 = plot(guess, xlabel="t", label=["x (guess)" "p (guess)"], linestyle=:dash)
traj = f((t0, tf), x0, p0)
plot!(fig1, traj, label=["x (sol)" "p (sol)"], linestyle=:solid)
plot!(fig1, [t0, tf], [xf_fixed, xf_fixed], label="target", colour=:black)
```
