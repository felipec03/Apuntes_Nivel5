> Colección de reglas de prueba, permite inferir fórmulas. A esto le llamamos secuente.

$$\phi_1, \ \phi_2 \dotsc \phi_n \vdash \psi$$
**Ejemplo:**
$p:$ El oro es un metal
$q:$ La plata es un metal
¿Que podemos decir de $p, q \vdash p \land \neg q$?

> Eventualmente tendrá un problema ya que recae en el conocimiento de los hechos p y q.

***
- Introducción de la conjunción (IC)
	- Dados dos hechos, podemos conectarlos por medio de y lógico.
$$\phi \ \psi \iff \phi \land \psi$$
- Eliminación de la conjunción (EC)
	- Dada la conjunción lógica de un y lógico entre dos hechos, podemos eliminar alguno de los dos hechos
	- EC1:
	$$\phi \land \psi \iff \phi$$
	- EC2:
	$$\phi \land \psi \iff \psi$$
**Ejercicio:**
$$p \land q, \ r \vdash q\land r$$
1. p $\land$ q
2. r
3. q (EC2 en 1)
4. q $\land$ r (IC)
***

- Eliminación de la implicancia (EI)
	- asdasd
	$$\phi , \ \phi \rightarrow \psi \iff \psi$$
- Modus Tollens (MT)
	- "Sale vapor", "El agua hierve", "Si no sale vapor, el agua no hierve"
$$\phi \rightarrow \psi \ \neg \psi \iff \neg \phi$$
- Introducción de la implicancia (II)
	- Alcance: Serie de pasos que llevan a una fórmula, se resume cómo implicancia
$$\phi \dotsc \psi \iff \phi \rightarrow \psi$$
**Ejemplo:**
$$s \rightarrow \neg t, s, \neg t \rightarrow r \vdash r$$
1. $s \rightarrow \neg t$
2. s
3. $\neg t \rightarrow r$
4. $\neg t$ (EI: 2 con 1)
5. r (EI: 4 con 3)
**Otro ejemplo:**
$$p \rightarrow q \vdash \neg q \rightarrow \neg p$$
1. $p \rightarrow q$ (premisa)
2. [ ~q  ] (supuesto)
3. [ ~p ] (MT: 1 con 2)
4. $\neg q \rightarrow \neg p$ (II: 2 con 3)
***
- Introducción de la doble negación (IDN)
$$\phi \iff \neg \neg \phi$$
- Eliminación de la doble negación (EDN)
$$\neg \neg \phi \iff  \phi$$
**Ejemplo:**
$$p \rightarrow q, p \vdash \neg \neg q$$
1. p -> q
2. p
3. q (EI: 2 con 1)
4. ~~ q (IDN: 3)
***
- Introducción de la disyunción: (ID)
	- ID1:
$$\phi \iff \phi \lor \psi$$
	- ID2:
$$\psi \iff \phi \lor \psi$$
- Eliminación de la disyunción: (ED)
	- Para eliminar en una disyunción se debe generar por medio de supuestos
$$\phi \lor \psi, [\phi \dots X , \psi \dots X] \iff X$$
**Ejemplo:**
$$p \lor q \vdash q \lor p$$
1. $p \lor q$ (premisa)
2. [ p ...  (supuesto) 
3.  $q \lor p$] (ID2: 2)
4. [ q  (supuesto)
5. $q \lor p$] (ID1: 4)
6. $q \lor p$ (ED: 1, 2-3, 4-5).
***
- Introducción de la negación: (IN) 
	-  $\bot :$ contradicción
$$ [ \phi \dots \bot ] \iff \neg \phi$$
- Eliminación de la negación: (EN)
$$\phi, \neg \phi \iff \bot$$
**Ejemplo:**
$$p \rightarrow \neg p \vdash \neg p$$
1. p -> ~p
2. [ p (suposición)
3. ~p (EI: 2, 1)
4. $\bot$ ](EN: 2, 3)
5. ~p (IN: 2-4)
***
- Demostración por contradicción: (DC)
$$[\neg \phi \dots \bot] \iff \phi$$
- Eliminación de la contradicción: (EC)
$$\bot \iff \phi$$
***
# Sintaxis - Lógica Proposicional
> Se define un conjunto de símbolos (alfabeto)

- Conjunto $P$ , de variables proposicionales $p,q,r,s$
- Constantes: $V \ o \ F$
- Conectivos lógicos: $\neg, \lor, \land, \iff, \rightarrow$
- Símbolos de puntuación: $,$
### Lenguaje Proposicional
> A partir de un conjunto P de variables, podemos definir un lenguaje proposicional L(P), que contiene todas las fórmulas posibles a través de una definición inductiva.

**$L(P)$ está formado por fórmulas:**

- Backus Naur Form (BNF): Podemos compactar definición anterior cómo:
$$\phi :: = p | (\neg \phi) | (\phi \land \phi) | (\phi \lor \phi) | (\phi \rightarrow \phi)$$
**Ejemplo:**
> Si existe un árbol de análisis sintáctico
1. $(p \lor q) \land q$
- q: es una fórmula 
- p o q: p es fórmula y q también, por lo tanto es fórmula.
- $(p \lor q) \land q$
2. $(p\land \neg q \neg) \land V r)$
