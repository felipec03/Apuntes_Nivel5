### Intento Solución PEP 2 - 2/2023
![[REDACTED-pep2_sistope-1-2023.pdf#page=1&rect=24,661,557,811|REDACTED-pep2_sistope-1-2023, p.1]]
1. En un sistema concurrente con memoria compartida el orden pasa a ser secundario, puesto que la ejecución múltiple genera las inconsistencias en el orden de ejecución dependiendo de la velocidad de las hebras, **es una característica**.
2. **La condición de carrera puede ocurrir en sistemas monoprocesadores, NO es una características.**
3. Una de las consecuencias de la concurrencias son las Condiciones de Carrera.
4. Es posible que las velocidades de las hebras sea igual o correspondiente al orden de instrucciones en un programa.
![[REDACTED-pep2_sistope-1-2023.pdf#page=1&rect=23,484,573,661|REDACTED-pep2_sistope-1-2023, p.1]]
> Una sección crítica es donde se encuentran recursos compartidos por dos o más hebras, la particularidad de esta es que las hebras pueden entrar en competencia (condición de carrera) y generar inconsistencias en el resultado final del proceso multihebreado.

c) Código ejecutado por dos o más hebras donde por lo menos una realiza operaciones de escritura sobre memoria global o compartida.
![[REDACTED-pep2_sistope-1-2023.pdf#page=1&rect=28,303,540,480|REDACTED-pep2_sistope-1-2023, p.1]]
> Dos hebras distintas y dos secciones críticas distintas.

1. Si no hay implementación de exclusión mutua, entonces es posible que una hebra distinta esté en la misma sección crítica. Hay EM.
2. Si la hebra 1 entra a sección crítica A y revisa si hay otra hebra en la misma sección, entonces, se cumple EM.
3. Se garantiza que si hebra 1 entra a sección crítica A, eventualmente entrará a la sección crítica, esto es indicativo de exclusión mutua, ya que se espera una señal de otra hebra para dar el paso.
4. Cuando una hebra1 está en una SC2, y hebra2 esta en SC2, si las hebras comparten memoria, entonces es posible que dependiendo de las instrucciones en las secciones, es POSIBLE que ocurra una condición de carrera, ESTA ES.
![[REDACTED-pep2_sistope-1-2023.pdf#page=1&rect=24,171,547,301|REDACTED-pep2_sistope-1-2023, p.1]]
1. Falso, instrucciones de alto nivel son agrupaciones de ensamblador.
2. ???, no estoy seguro
3. Falso, inanición no implica deadlock, ya que puede haber inanición entre hebras, pero para considerarse Deadlock, todas las hebras deben de estar en inanición.
4. **Verdadero**, si una hebra fuera a tener dos recursos asociados a otras dos hebras, existirían 2 secciones críticas dentro de la misma hebra.

>[!INFO]
>Para la pregunta 5 y 6

![[REDACTED-pep2_sistope-1-2023.pdf#page=2&rect=18,625,571,775&color=yellow|REDACTED-pep2_sistope-1-2023, p.2]]
![[REDACTED-pep2_sistope-1-2023.pdf#page=2&rect=22,388,575,618&color=yellow|REDACTED-pep2_sistope-1-2023, p.2]]

![[REDACTED-pep2_sistope-1-2023.pdf#page=2&rect=17,279,513,386&color=important|REDACTED-pep2_sistope-1-2023, p.2]]

```c
// a -> No hay deadlock, funciona hasta completación
boleto = 1;
turno = 0;

while(1){
	miturno = fetchadd(1) = 2
	while(0 != 2); // Spinlock
	// CAMBIO DE CONTEXTO
	:D
	fetchadd(0) = turno = 1
}

// b
boleto = 0;
turno = 1;
while(1){
	miturno = fetchadd(0) = 1
	while(1 != 1) // Sigue sin spinlock
}
```
> Al no haber spinlock efectivo en la opción b, se considera deadlock.

![[REDACTED-pep2_sistope-1-2023.pdf#page=2&rect=17,166,304,276&color=important|REDACTED-pep2_sistope-1-2023, p.2]]

```c
boleto = 0
turno = 0

while(1){
	miturno = fetchadd(0) = 1
	while(1 != 0); // Spinlock
	
}
```

****
![[REDACTED-pep2_sistope-1-2023.pdf#page=3&rect=51,606,567,743&color=important|REDACTED-pep2_sistope-1-2023, p.3]]
![[REDACTED-pep2_sistope-1-2023.pdf#page=3&rect=53,122,533,598&color=important|REDACTED-pep2_sistope-1-2023, p.3]]
![[REDACTED-pep2_sistope-1-2023.pdf#page=4&rect=38,614,532,698&color=important|REDACTED-pep2_sistope-1-2023, p.4]]
![[REDACTED-pep2_sistope-1-2023.pdf#page=4&rect=57,494,544,608&color=important|REDACTED-pep2_sistope-1-2023, p.4]]
1. asdasd
2. asd
3. asd
4. asd

![[REDACTED-pep2_sistope-1-2023.pdf#page=4&rect=52,401,524,497|REDACTED-pep2_sistope-1-2023, p.4]]
1. 


***
# Ayudantía Antes de la PEP
```c
r = 0
s = 1
P1(){
	wait(r); // r = -1 -> sl
	wait(s);
	A();
	signal(s)
	B();
	signal(r);
}

P2(){
	C(); // Se ejecuta C
	signal(r);
	D();
	wait(s);
	E();
	signal(s);
}
```

1. A() no puede ejecutarse hasta que C() termine: TRUE
2. A() y D() no pueden ejecutarse concurrentemente: FALSE 
3. A() y E() no pueden ejecutarse concurrentemente: FALSO
4. B() y E() no pueden ejecutarse concurrentemente: FALSO

### Algoritmo del banquero
- Matriz de necesidad: C (claim)
- Matriz de asignación: A
- Vector de recursos: R
- Vector de disponibles: D
- Matriz resultante: C - A.
Procesos: Columna -> Filas: Recursos

Si vector recursos = vector disponibles, se asegura que no hay Deadlock,