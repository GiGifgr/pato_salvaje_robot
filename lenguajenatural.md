# Lenguaje Natural
## 1) Componentes del juego (versión digital)

- **Tablero:** Es una cuadrícula de hexágonos (figuras de seis lados) donde cada celda tiene un tipo de terreno: **Bosque**, **Montaña**, **Agua** o **Llanura**.  
  El **borde gris** representa la zona donde inician los **Robots enemigos**.

- **Unidades (♥ = cantidad de vidas máximas):**
  - **Pato (1♥):** Es el objetivo a eliminar. En el agua puede moverse hasta **3 casillas hexagonales** por turno.  
  - **Zarigüeya (2♥):** Puede **esquivar el primer ataque** que reciba si la atacan primero (solo una vez en toda la partida).  
  - **Alce (2♥):** Puede moverse hasta **3 casillas** por turno.  
  - **Oso (2♥):** Sus ataques causan **2 puntos de daño**.  
  - **Zorro (2♥):** Si ataca por detrás, hace **2 puntos de daño**; en cualquier otro caso, **1 punto**.  
  - **Robot aliado (3♥):** Puede **bloquear un ataque** que vaya hacia sí mismo, hacia el Pato o hacia otro aliado.  
    Solo puede usar su habilidad o atacar una vez cada **3 turnos**.  
    Cuando usa una de esas acciones, debe esperar 3 turnos para volver a hacerlo. Mientras tanto puede moverse, pero no atacar ni activar el escudo.  
  - **Robot enemigo (3♥):** Se mueve **2 casillas** en terreno llano y **1 casilla** en montaña. En agua o bosque se mueve igual que en llanura.

- **Orientación (dirección):** Cada unidad tiene una dirección hacia donde mira.  
  Esta dirección cambia cuando se mueve (según la última casilla a la que avanzó).

- **Adyacencia:** Dos unidades son adyacentes si están en hexágonos que se tocan por un lado (no por las esquinas).

- **Ocupación:** Solo puede haber **una unidad por cada casilla**.


## 2) Preparación de la partida

1. Cargar el tablero con sus tipos de terreno.  
2. Colocar los **Robots enemigos** en el borde gris.  
3. Colocar el **Pato** en el centro, dentro del agua.  
4. Colocar los **Animales** en el lado opuesto al de los Robots.  
5. Asignar vidas, direcciones iniciales (mirando hacia el enemigo), y activar habilidades según corresponda.  
6. Definir el orden en que se moverán:
   - Animales: **Zorro → Robot aliado → Oso → Zarigüeya → Alce → Pato**
   - Robots enemigos: de izquierda a derecha.  
7. Iniciar el contador de turnos con ronda = 1.


## 3) Estructura del turno (por rondas)

Cada ronda tiene **dos fases**:

1. **Fase de los Animales:** cada Animal vivo actúa una vez en el orden indicado.  
2. **Fase de los Robots:** cada Robot enemigo vivo actúa una vez en su orden.

**Al final de la ronda:**
- Aumentar el número de ronda (ronda = ronda + 1).  
- Si el **Pato** sigue vivo y ya pasaron **25 rondas**, los **Animales ganan**.  
- Si el **Robot aliado** usó su escudo o atacó, reducir en 1 el número de turnos que le faltan para volver a usar su habilidad.


## 4) Secuencia del turno de una unidad

Cada unidad viva, cuando le toca su turno, sigue este orden:

### Movimiento
- **Animales:** pueden moverse hasta 2 casillas. El **Alce** puede moverse hasta 3.  
- **Robots enemigos:** pueden moverse 2 en llanura y 1 en montaña.  
- **Pato:** puede moverse 3 casillas si termina en el agua; en otro tipo de terreno, solo 2.  
- Ninguna unidad puede pasar por encima de otra ni salir del área del mapa.  
- Al finalizar su movimiento, la unidad actualiza su dirección según hacia dónde se movió.

### Habilidad especial
- **Robot aliado:**  
  - Si su habilidad está disponible, puede **elegir una acción:**
    1. Activar un **escudo de protección** sobre una unidad aliada cercana (a una casilla de distancia o sobre sí mismo).  
       El escudo bloquea un ataque y luego desaparece.  
    2. **Atacar** (ver reglas de ataque más abajo).  
  - Después de usar cualquiera de esas dos acciones, debe esperar **3 turnos** antes de volver a usarlas.  
- **Zarigüeya:** Su habilidad de esquivar se usa automáticamente la primera vez que la atacan.  
- **Zorro, Oso, Alce, Pato y Robots enemigos:** tienen habilidades pasivas (siempre activas).

### Ataque
Una unidad puede atacar si hay un enemigo **al frente y en una casilla adyacente**.

1. Comprobar si el objetivo está en la dirección hacia donde mira el atacante.  
2. Lanzar un dado de seis caras:  
   - Si sale **4, 5 o 6**, el ataque acierta.  
   - Si sale **1, 2 o 3**, el ataque falla.  
3. Si acierta, aplicar el daño según el tipo de unidad:  
   - Ataque normal: 1 punto de daño.  
   - **Oso:** hace 2 puntos.  
   - **Zorro:** hace 2 puntos si ataca por detrás del enemigo; en otro caso, 1 punto.  
4. Antes de aplicar el daño, comprobar defensas:
   - Si el objetivo tiene un **escudo del Robot aliado**, el ataque se bloquea y el escudo se elimina.  
   - Si el objetivo es la **Zarigüeya** y es su **primer ataque recibido**, lo esquiva completamente y no recibe daño.  
5. Aplicar el daño restante.  
   - Si la vida del objetivo llega a 0, se elimina del tablero.  
   - Si el eliminado es el **Pato**, los **Robots ganan automáticamente**.

> Todos los personajes pueden atacar en cada turno si cumplen las condiciones, excepto el Robot aliado, que debe esperar 3 turnos después de usar su habilidad o atacar.


## 5) Movimiento según el terreno

- **Montaña:** los Robots enemigos solo pueden moverse una casilla por turno.  
- **Agua:** el Pato puede moverse hasta tres casillas si termina en una celda de agua.  
- **Bosque y Llanura:** no tienen modificaciones.  
- No se puede mover en diagonal; el movimiento siempre es entre los seis lados de cada hexágono.


## 6) Ataque por detrás (Zorro)

- Tomar la dirección hacia donde mira el enemigo.  
- Identificar cuál es la casilla que está justo **detrás** de él.  
- Si el **Zorro** está en esa posición al atacar, se considera un **ataque por detrás** y hace **2 puntos de daño**.  
- Si está en otro lado, su ataque hace el daño normal (1 punto).


## 7) Orden correcto al resolver un combate

1. El atacante elige un enemigo adyacente al frente.  
2. Si es el Zorro, se comprueba si está atacando por detrás.  
3. Se lanza el dado (4–6 acierta, 1–3 falla).  
4. Si acierta, se revisa si el objetivo tiene un escudo.  
5. Si no hay escudo y el objetivo es la Zarigüeya, revisar si puede esquivar.  
6. Aplicar el daño correspondiente.  
7. Si el **Pato** muere, los Robots ganan automáticamente.


## 8) Condiciones de victoria

- **Robots:** ganan si logran eliminar al Pato antes de terminar la ronda número 25.  
- **Animales:** ganan si el Pato sobrevive hasta el final de la ronda número 25.  
- También se considera **Victoria de los Animales** si todos los Robots son eliminados antes de ese momento.


## 9) Reglas para evitar errores en la versión digital

- No permitir movimientos fuera del tablero o sobre otra unidad.  
- No permitir ataques si el enemigo no está adyacente o no está al frente.  
- No permitir que el Robot aliado use su escudo o ataque antes de que pasen los tres turnos de espera.  
- Si el Pato intenta moverse a una casilla inválida, el movimiento se cancela.  
- Después de cada ataque o eliminación, comprobar si se cumple alguna condición de victoria.


![Ejemplo](assests/ejemplotablero.png)