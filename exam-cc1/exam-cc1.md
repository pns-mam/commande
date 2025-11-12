![PNS](https://raw.githubusercontent.com/pns-mam/mi1/master/logo-pns.png)

## MAM5 / M2 INUM

# Commande optimale

# 2025-26

# Exam cc no. 1

## Exercice 1 (8 points)

On considère le problème de minimisation du temps final pour la dynamique

$$ \ddot{q}(t) = u(t),\quad |u(t)| \leq 1, $$

où $q(t)$ et $u(t)$ sont de dimension un. Les conditions intiales $q_0$, $\dot{q}_0$ sont fixées.

### 1.1

Déterminer le contrôle optimal pour la condition finale $q(t_f) = 0$ (et $\dot{q}(t_f)$ libre).

### 1.2

Déterminer la fonction valeur correspondante, $t_f(q_0,\dot{q}_0)$ (temps min en fonction de la condition initiale).

### 1.3

Déterminer le contrôle optimal pour la condition finale $\dot{q}(t_f) = 0$ (et $q(t_f)$ libre).

### 1.4

Déterminer la fonction valeur correspondante, $t_f(q_0,\dot{q}_0)$ (temps min en fonction de la condition initiale).

## Exercice 2 (8 points)

On considère le problème de commande optimale général suivant, où $t_f>0$ est libre : 

$$ \int_0^{t_f} f^0(x(t),u(t))\,\mathrm{d}t \to \min $$

avec

$$ \dot{x}(t)=f(x(t),u(t)),\quad u(t) \in U \subset \mathbf{R}^m, $$

et des conditions aux limites sur $x(0)$ et $x(t_f)$ dans $\mathbf{R}^n$. On se ramène à temps final fixé sur $[0,1]$ par le changement de temps $t=t_f\cdot s$, et on  fait de $t_f$ un nouvelle variable d'état en posant ($'=\mathrm{d}/\mathrm{d}s$ représente la dérivée par rapport au nouveau temps $s$) : 

$$ t_f'(s) = 0. $$

### 2.1

On pose $\hat{x}(s):=(x(t_f(s)\cdot s),t_f(s))$ et $\hat{u}(s):=u(t_f(s)\cdot s)$. Montrer que $\hat{x}'(s) = \hat{f}(\hat{x}(s),\hat{u}(s))$ avec

$$ \hat{f}(\hat{x},\hat{u}) = (t_f\cdot f(x,u),0),\quad \hat{x}=(x,t_f) \in \mathbf{R}^{n+1},\quad \hat{u}=u \in \mathbf{R}^m. $$


### 2.2

Montrer que le coût de Lagrange s'écrit

$$ \int_0^{1} \hat{f}^0(\hat{x}(s),\hat{u}(s))\,\mathrm{d}s $$

avec $\hat{f}^0(\hat{x},\hat{u})=t_f \cdot f^0(x,u)$.

### 2.3

On note désormais $\hat{x}=(x,t_f) \in \mathbf{R}^{n+1}$, $\hat{p}=(p,p_{t_f}) \in \mathbf{R}^{n+1}$ (et $u$ pour le contrôle, $p^0$ pour le multiplicateur devant le terme de Lagrange). Écrire le hamiltonien du nouveau problème à temps final fixé.

### 2.4

Écrire l'équation différentielle vérifiée par $p_{t_f}$. 

### 2.5

Les valeurs $t_f(0)$ et $t_f(1)$ étant libres, en déduire les conditions de transversalité sur $p_{t_f}$.

### 2.6

En déduire la valeur de

$$ \int_0^1 t_f(s) \left[ p^0 f^0(x(s),u(s))+(p(s)|f(x(s),u(s))) \right] \,\mathrm{d}s. $$

### 2.7

En déduire que le hamiltonien du problème de départ à temps final libre est nul.

## Exercice 3 (4 points)

On considère la portion de code suivant, extrait de l'application d'une méthode directe au problème de navigation :

```julia
# Objective
@objective(sys, Min, tau[1] + tau[2] + tau[3])

# Constraints 
@constraints(sys, begin
    x[1, 1] == x0
    y[1, 1] == y0
    th[1, 1] == th0
    x[2, 1] == x[1, P]
    y[2, 1] == y[1, P]
    th[2, 1] == th[1, P]
    x[3, 1] == x[2, P]
    y[3, 1] == y[2, P]
    th[3, 1] == th[2, P]
    x[3, P] == xf
    y[3, P] == yf
    th[3, P] == thf
    end)
```

```julia
# Dynamics: Crank-Nicolson scheme
for j in 1 : P-1
    @NLconstraints(sys, begin
    # x' = w + cos(theta)
    x[1, j+1] == x[1, j] + 0.5 * tau[1]*Dt *
        (w + cos(th[1, j]) + w + cos(th[1, j+1]))
    x[2, j+1] == x[2, j] + 0.5 * tau[2]*Dt *
        (w + cos(th[2, j]) + w + cos(th[2, j+1]))
    x[3, j+1] == x[3, j] + 0.5 * tau[3]*Dt *
        (w + cos(th[3, j]) + w + cos(th[3, j+1]))
    # y' = sin(theta) 
    y[1, j+1] == y[1, j] + 0.5 * tau[1]*Dt *
        (sin(th[1, j]) + sin(th[1, j+1]))
    y[2, j+1] == y[2, j] + 0.5 * tau[2]*Dt *
        (sin(th[2, j]) + sin(th[2, j+1]))
    y[3, j+1] == y[3, j] + 0.5 * tau[3]*Dt *
        (sin(th[3, j]) + sin(th[3, j+1]))
    # theta' = u
    th[1, j+1] == th[1, j] + tau[1]*Dt * u[1]
    th[2, j+1] == th[2, j] + tau[2]*Dt * u[2]
    th[3, j+1] == th[3, j] + tau[3]*Dt * u[3]
    end)
end
```

### 3.1

Expliquer l'expression de la fonction coût minimisée.

### 3.2

Indiquer la portion de code traduisant les conditions de jonction entre le second et le troisième arc.

### 3.3

Quelle est le lien entre `Dt` et `P` ?

### 3.4

Comment modifier ce code si le courant est désormais un vecteur $(w_x,w_y)$ ?
