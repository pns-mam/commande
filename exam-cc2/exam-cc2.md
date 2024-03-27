![PNS](http://caillau.perso.math.cnrs.fr/logo-pns.png)
## MAM5-INUM - Commande optimale
# Exam CC no. 2

**Durée 2H00. Documents autorisés. Tous les exercices sont indépendants. Le barème prévisionnel est indiqué pour chaque exercice.**

## Exercice 1 (12 points)
On considère le problème de Lagrange suivant, à temps final $t_f$ fixé,

$$ \dot{x}(t) = ax(t)u(t)-bx(t),\quad t \in [0,t_f], $$

$$ \int_0^{t_f} (u(t)-1)x(t)\ \mathrm{d}t \to \min, $$

où $a$ et $b$ sont des réels strictement positifs, avec $a > b$, et où $x(0)=x_0 > 0$ est fixé alors que $x(t_f)$ est laissé libre. On a la contrainte que, presque pour tout $t$,

$$ u(t) \in [0,1]. $$

### 1.1
Écrire le hamiltonien du problème.

### 1.2
Écrire le système différentiel vérifié par l'état adjoint.

### 1.3
Écrire les conditions de transversalité.

### 1.4
Montrer qu'on est dans le cas normal. (On prendra par conséquent $p^0=-1$ dans la suite.)

### 1.5
Appliquer la condition de maximisation du Hamiltonien. 

### 1.6
Montrer qu'un contrôle optimal est nécessairement nul sur un intervalle $]\tau,t_f]$, avec $\tau < t_f$.

### 1.7
Déterminer la valeur de l'état adjoint sur cet intervalle.

### 1.8
En déduire l'expression du temps $\tau$.

### 1.9
Montrer finalement que $u=1$ sur $[0,\tau[$.

## Exercice 2 (8 points)

### 2.1
On considère un problème de commande optimale à temps final fixé dont les conditions terminales sont (état de dimension $2$)

$$ x_1^2(t_f)+x_2(t_f) = 1. $$

Donner la condition de transversalité correspondante.

### 2.2
En déduire comment compléter le code de tir ci-dessous :

```julia
function shoot(p0)
    xf, pf = f(t0, x0, p0, tf)
    s = # À COMPLÉTER
    return s
end
```

### 2.3
Dans le code MPC ci-dessous, quel est le rôle de l'appel à la fonction `trajectory` ?

```julia
while true
    
    w = drift(x1, y1)
    us, τs = solve(x1, y1, θ1, xf, yf, θf, w, P, print_level=0)
    ts = [ t1+τs[1], t1+τs[1]+τs[2] ]
    tf = t1+τs[1]+τs[2]+τs[3]
    if (t1+Δt < tf)
        t2 = t1+Δt
    else
        t2 = tf
        println("t2=tf: ", t2)
    end
    sol = trajectory((t1, t2), x1, y1, θ1, us, ts, drift)
    ...
end
```

### 2.4
Dans ce même code, expliquer à quoi correspond la clause `else` dans laquelle `t2 = tf`.

### 2.5
Dans le code Hexapawn ci-dessous, expliquer le rôle de la boucle `while`.

```matlab
function u = play2(X, lX, lu)
% play2 -- Random control generation for player2

n = length(lX);

i = 1;
found = 0;

while (~found) & (i <= n)
  if (norm(lX{i}-X) == 0)
    found = 1;
    l = lu{i};
  else
    i = i+1;
  end;
end;
...
```

### 2.6
On considère une partie d'Hexapawn pendant laquelle la machine vient de jouer le coup ci-dessous :

```matlab
[ 2 2 0        [ 2 0 0 
  1 0 1    ->    1 0 2
  0 0 1 ]        0 0 1 ]
```

La liste de coups de la machine associée à l'état précédent (= avant son dernier coup) était

```matlab
{ [ 1 2   , [ 1 2   , [ 1 2
    2 1 ]     2 2 ]     2 3 ] }
```

Comment cette liste doit-elle être mise à jour par renforcement ?
