## Gramática Regular Lineal por la Derecha
- Creación de GRLD
- Producciones
- Normalizar GRLD
## Expresiones Regulares
- Creación de ER
- Simplificación de ER
- Construcción de Thompson
## AFD (Autómata Finito Determinista)
- Construcción de AFD
- Representaciones de AFD
	- Tabla de transiciones
	- Conjunto 
	- Diagrama de transiciones
- Minimización de AFD por medio de equivalencia
	- Método 1: Listas
	- Método 2: Subgrupos
- Operaciones con AFD
- Transformar AFD -> Expresión regular
	- Método 1: Lema de Arden $X = AX + B = A^{\ast}B$
	- Método 2 (único a AFD): $r_{ij}$
## AFN
- Construcción de AFN, utilización de conjunto vacío
- Transformar AFN -> AFD
	- Método 1: Probar todas las combinaciones de estados con conjunto potencia de estados en AFN.
	- Método 2: En base a estado inicial, ir viendo transiciones y si se generan nuevos estados para aplicar transición.
- Traza de AFN
- Minimización
- Transformar AFN -> Expresión Regular
	- Método 1: Lema de Arden