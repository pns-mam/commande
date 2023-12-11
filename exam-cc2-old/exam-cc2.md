![PNS](http://caillau.perso.math.cnrs.fr/logo-pns.png)
## MAM5-INUM - Commande optimale
# Exam CC no. 2

**Durée 2H00. Tous les exercices sont indépendants.
Le barème prévisionnel est indiqué pour chaque exercice.**

## Exercice 1 (12 points)
On considère le problème de temps minimal pour

$$ \ddot{q}(t)=\dot{q}(t)-u(t),\quad |u(t)| \leq 1,\quad t \in [0,t_f] $$

où $q$ et $u$ sont à valeurs dans $\mathbf{R}$. Les conditions aux limites sont $q(0)=\dot{q}(0)=0$, et $q(t_f)=q_f$, $\dot{q}(t_f)=\dot{q}_f$ (avec $q_f$ et $\dot{q}_f$ fixés).

### 1.1
Mettre la dynamique sous la forme $\dot{x}(t)=f(x(t),u(t))$ en posant $x(t)=(q(t),\dot{q}(t))$, avec $f$ une fonction que l'on précisera.

$\rhd$ $f(x_1,x_2,u) = (x_2,x_2-u)$

### 1.2
Montrer que le problème n'admet pas de solution si $|\dot{q}_f| \geq 1$.

$\rhd$ FAUX : avec $|u| \leq 1$ et $\ddot{q}(t)=\dot{q}(t)-u(t)$ (et non $\ddot{q}(t)=-\dot{q}(t)-u(t)$), pas d'obstruction.

### 1.3
Écrire le hamiltonien du problème.

$\rhd$ $H(x,p,u) = p^0 + p_1 x_2 + p_2(x_2-u)$

### 1.4
Écrire le système différentiel vérifié par l'état adjoint $p=(p_1,p_2)$.

$\rhd$ $\dot{p}_1 = 0$, $\dot{p}_2 = -(p_1+p_2)$

### 1.5
En déduire l'expression de $p_2$.

$\rhd$ $p_2(t) = -p_1 + Be^{-t}$ où $p_1$ et $B$ sont des constantes à déterminer

### 1.6
Montrer que, si la fonction $p_2$ est constante, elle n'est pas nulle.

$\rhd$ Si $p_2 \equiv 0$, nécessairement $p_1=0$ ; comme (temps final libre), $0 = H = p_0$, on aurait alors $(p^0,p) = (0,0)$, ce qui est impossible.

### 1.7
En déduire que le contrôle vaut $+1$ ou $-1$, avec au plus une commutation.

$\rhd$ La fonction $p_2$ est donc soit constante et non nulle, soit strictement monotone : elle a donc au plus un zéro. La maximisation du hamiltonien indiquant $u=-\text{signe}(p_2)$ quand $p_2 \neq 0$, on en déduit le résultat.

### 1.8
Donner l'expression de $x_1$ et $x_2$ pour un arc le long duquel $u=1$ et qui part de $(0,0)$ en $t=0$.

$\rhd$ $x_1(t) = t+1-e^t$, $x_2(t) = 1-e^t$

### 1.9
En déduire l'équation en coordonnées $(x_1,x_2)$ de l'arc correspondant, noté $\Gamma_+$, situé dans le demi-plan $x_1 \leq 0$.

$\rhd$ $x_1 = x_2 + \ln(1-x_2)$, $x_2 \leq 0$ 

### 1.10
Donner l'allure de la synthèse dans le plan $(x_1,x_2)$. (Dessiner approximativement les trajectoires temps minimales partant de $(0,0)$ vers une cible arbitraire.)

$\rhd$ ![synthèse](fig1.jpg)

## Exercice 2 (8 points)

### 2.1
On considère un problème de commande optimale à temps final fixé dont les conditions terminales sont (état de dimension $2$)

$$ x_1(t_f)+x_2(t_f)=1. $$

Donner la condition de transversalité correspondante.

$\rhd$ L'espace tangent (en tout point) à l'ensemble cible est la droite $x_1+x_2 = 0$, donc $p_1(t_f)-p_2(t_f) = 0$.

### 2.2
En déduire comment compléter le code de tir ci-dessous :

```julia
function shoot(p0)
    xf, pf = f(t0, x0, p0, tf)
    s = [ xf[1] + xf[2] - 1, pf[1] - pf[2] ] # À COMPLÉTER
    return s
end
```

### 2.3
Dans le code MPC ci-dessous, quel est le rôle du calcul `w = drift(x1, y1)` ?

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

$\rhd$ Calculer la dérive (vraie) du système au point où la position est mesurée.

### 2.4
Dans ce même code, que représente le vecteur `ts` ?

$\rhd$ Les temps de commutation du contrôle (*cf.* structure à 3 arcs).

### 2.5
Dans le code Hexapawn ci-dessous, expliquer pourquoi il n'y a pas de renforcement après le coup no. 1 (`Move 1`).

```matlab
% Move 1: player2
u1 = play2(X1, lX1, lu1);
X2 = f2(X1, u1);
if dsp, disp('Player 2 move:'); disp(X2); end;

% Move 2: player1
u2 = play1(X2, inter);
X3 = f1(X2, u2);
if dsp, disp('Player 1 move:'); disp(X3); end;

if win1(X3) || isempty(play2(X3, lX3, lu3))
  winner = 1;
  lu1 = reinforce(X1, u1, lX1, lu1);

else...
```

$\rhd$ Aucun des deux joueurs ne peut gagner après le coup no. 1 (le second de la partie), rien à renforcer à ce niveau.

### 2.6
On considère une partie d'Hexapawn pendant laquelle la machine vient de jouer le coup ci-dessous :

```matlab
[ 2 2 0        [ 2 0 0 
  1 0 1    ->    1 2 1
  0 0 1 ]        0 0 1 ]
```

La liste de coups de la machine associée à l'état précédent (= avant son dernier coup) était

```matlab
{ [ 1 2   , [ 1 2   , [ 1 2
    2 1 ]     2 2 ]     2 3 ] }
```

Comment cette liste doit-elle être mise à jour par renforcement ?

$\rhd$ Le coup qu'elle a joué conduit la machine à perdre, il doit donc être supprimé par renforcement :

```matlab
{ [ 1 2   , [ 1 2
    2 1 ]     2 3 ] }
```

### 2.7
Dans le code de renforcement ci-dessous, une ligne, que l'on précisera, provient d'une équation de point fixe : énoncer cette équation.

```python
for step in range(max_steps):
        
        # Randomly Choose an Action
        action = env.action_space.sample()
        
        # Take the action -> observe new state and reward
        new_state, reward, done, info = env.step(action)
        
        # Update qtable values
        if done == True: # If last, do not count future accumulated reward
            break
        else: # Consider accumulated reward of best decision stream
            qtable[state, action] = qtable[state,action]
            +lr*(reward+gamma*np.max(qtable[new_state,:])-qtable[state,action])
            
        # moving states
        state = new_state
        
    episode += 1
```

$\rhd$ Il s'agit de la ligne `qtable[state, action] = ... -qtable[state,action])` qui provient de l'équation de point fixe vérifiée par la $Q$-fonction : 

$$ Q(x,u) = R(x,u) + \gamma E(\max_{u'} Q(f(x,u,e_0), u')). $$
