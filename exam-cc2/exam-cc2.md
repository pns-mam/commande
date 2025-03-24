![PNS](http://caillau.perso.math.cnrs.fr/logo-pns.png)
## MAM5-INUM - Commande optimale
# Exam CC no. 2

**Durée 2H00. Documents autorisés. Tous les exercices sont indépendants. Le barème prévisionnel est indiqué pour chaque exercice.**

## Exercice 1 (15 points)

On considère une barque se déplaçant à vitesse constante dans un canal rectiligne, de largeur constante. On peut supposer que la norme de la vitesse vaut un, et normaliser de même la largeur du canal à un. Il y a dans ce canal un courant $c(y)$ dirigé selon $(Ox)$ que l'on suppose *fort* (avec $c$ une fonction $\mathscr{C}^\infty$), c'est à dire tel que $c(y) > 1$, quel que soit $y$. On contrôle directement le cap du bateau, noté $u(t)$, de sorte que le système s'écrit

$$ \dot{x}(t) = \cos u(t) + c(y(t)),\quad \dot{y}(t) = \sin u(t), $$

avec $(x,y) \in \mathbf{R}^2$ les coordonnées dans le canal (voir figure ci-après) et $u \in \mathbf{R}$. Dans tout l'exercice on prend une condition initiale à l'origine, $(x(0),y(0)) = (0,0)$, le temps final $t_f$ est supposé *libre*, et on admettra que tous les problèmes considérés possèdent une solution.

![IMG_3915](fig1.jpg)

**Partie A.** On s'intéresse pour commencer à minimiser le déport $x(t_f)$ quand on vise la berge opposée, $y(t_f) = 1$.

### 1.1

Mettre le problème sous forme de Lagrange avec un intégrande $f^0$ que l'on précisera.

### 1.2

Écrire le Hamiltonien du problème.

### 1.3

Écrire le système adjoint.

### 1.4

Écrire les conditions de transversalité.

### 1.5

Montrer qu'on est nécessairement dans le cas normal. (On posera donc $p^0 = -1$.)

### 1.6

Appliquer la condition de maximisation.

### 1.7

En utilisant le fait que temps final est libre, montrer que

$$ \cos u(t) = -1/c(y(t)). $$

Que dire du signe de $\sin u(t)$ ?

### 1.8

Donner l'expression du temps final sous la forme d'une intégrale (dépendant de la fonction $c$).

**Partie B.** On s'intéresse désormais au problème du temps minimal pour atteindre l'autre berge (conditions finales inchangées : $x(t_f)$ libre et $y(t_f) = 1$).

### 1.9

Écrire le Hamiltonien du problème.

### 1.10

Montrer qu'on est nécessairement dans le cas normal. (On posera donc $p^0 = -1$.)

### 1.11

Montrer que $p_y$ ne s'annule pas.

### 1.12

Appliquer la condition de maximisation et en déduire le contrôle optimal.

### 1.13

Déterminer le temps minimal.

**Partie C.** On s'intéresse finalement au problème du temps minimal pour atteindre un point fixé sur l'autre berge : $x(t_f) = x_f > 0$ et $y(t_f) = 1$.

### 1.14

Dans le cas normal, donner l'expression de $u$ en fonction des adjoints.

### 1.15

Dans le cas anormal, donner l'expression de $u$ : comment interpréter cette solution ?

## Exercice 2 (5 points)

### 2.1
On considère un problème de commande optimale à temps final fixé dont les conditions terminales sont (état de dimension $2$)

$$ x_1(t_f) x_2(t_f) = 1. $$

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
Dans le code MPC ci-dessous, à quoi correspond le vecteur `ts` ?

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

On considère une partie d'Hexapawn pendant laquelle la machine vient de jouer le coup ci-dessous :

```matlab
[ 2 2 0        [ 2 0 0 
  1 0 1    ->    2 0 1
  0 0 1 ]        0 0 1 ]
```

La liste de coups de la machine associée à l'état précédent (= avant son dernier coup) était

```matlab
{ [ 1 2   , [ 1 2   , [ 1 2
    2 1 ]     2 2 ]     2 3 ] }
```

Comment cette liste doit-elle être mise à jour par renforcement ?
