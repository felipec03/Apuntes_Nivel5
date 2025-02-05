### Fechas Evaluaciones
- PEP 1: 24 Octubre
- PEP 2: 28 Noviembre
- PEP 3: 9 Enero
- PDR: 16 Enero
### Laboratorio
- Programación en C
- Prestaciones del Sistema Operativo Linux
### Bibliografía
- UVirtual
***
## Introducción a Sistemas Operativos
*Cada sistema computacional necesita un software de administración de hardware.*

Hardware <-  Interrupciones -> Sistema Operativo <- System Call -> Procesos

- Interrupciones son señales eléctricas que llegan al procesador.
- Procesos son aplicaciones, administrador de tareas.
- System Call: Conjunto de instrucciones que determina el sistema operativo.

El usuario dificilmente interactua con el sistema operativo, el usuario interactua con una aplicación.

**Kernel == Sistema Operativo**

Kernel: Conjunto de servicios para comunicarse con el hardware. Puede poseer módulos de Kernel a medida que sean necesarios.
- Por ejemplo, un driver no es un módulo ya que interactua directamente con el hardware a través de instrucciones.
- Un módulo cómo ejemplo es el sistema de administrador de archivos. Desktop Environment.
Para ejecutar instrucciones del Kernel se necesita permisos de super usuario.

Máquina Virtual: Virtualización de componentes de hardware para emular S.O.