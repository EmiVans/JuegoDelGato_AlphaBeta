# JuegoDelGato_AlphaBeta
# Práctica 2: Algoritmo α−β aplicado al Juego del Gato

**Autores:**
Brandon Imanol Regalado Urbina,  
Vazquez Amador Daniel Emiliano,  
Arroyo Lozano Santiago,  
Valencia Cruz Jonathan Josué,  
Luis Gerardo Estrada García

**Fecha:** \today

## Introducción
En esta práctica, se implementó el algoritmo **α−β pruning** aplicado al problema del juego del gato. El objetivo principal es optimizar la búsqueda de decisiones en este juego de dos jugadores. El jugador MAX utiliza las tiradas "X" y el jugador MIN utiliza las tiradas "O". La función de utilidad está dada por:

$$
f(x) =
\begin{cases}
    +1 & \text{si MAX gana}\\
    0 & \text{si hay empate}\\
    -1 & \text{si MIN gana}
\end{cases}
$$

## Desarrollo
Para implementar este algoritmo, primero se desarrollaron las funciones que permiten manejar el juego del gato, incluyendo la creación del tablero, las reglas del juego, y la evaluación de los estados.

### Funciones Implementadas

```python
# Crea el tablero donde se jugara el juego
def Gato():
    return [[' ' for _ in range(3)] for _ in range(3)]

# Funcion que imprime el tablero
def tablero(tablero):
    for row in tablero:
        print('|'.join(row))
        print()

# Funcion para mover una pieza en el tablero
def resultado(tablero, jugada, player):
    if not is_final(tablero):
        if tablero[jugada[0]][jugada[1]] == " ":
            if player == 1:
                tablero[jugada[0]][jugada[1]] = "X"
                return tablero
            elif player == 2:
                tablero[jugada[0]][jugada[1]] = "O"
                return tablero
        else:
            return tablero

# Regresa la utilidad del tablero
def utility(tablero):
    # Verificar filas
    for fila in tablero:
        if fila[0] == fila[1] == fila[2] and fila[0] != ' ':
            if fila[0] == "X":
                return 1
            else:
                return -1
    # Verificar columnas
    for i in range(3):
        if tablero[0][i] == tablero[1][i] == tablero[2][i] and tablero[0][i] != ' ':
            if tablero[0][i] == "X":
                return 1
            else:
                return -1
    # Verificar diagonales
    if tablero[0][0] == tablero[1][1] == tablero[2][2] and tablero[0][0] != ' ':
        if tablero[0][0] == "X":
            return 1
        else:
            return -1
```
## Algoritmo de α−β pruning
La implementación del algoritmo de alpha-beta pruning permite optimizar la búsqueda de decisiones al podar ramas que no afectan el resultado final, acelerando el proceso de decisión del juego.

```python
def alpha_beta(tablero, alpha, beta, player):
    if is_final(tablero):
        return utility(tablero)

    if player == 1:  # MAX
        max_eval = -float('inf')
        for move in valid_moves(tablero):
            new_tablero = resultado(tablero, move, player)
            eval = alpha_beta(new_tablero, alpha, beta, 2)
            max_eval = max(max_eval, eval)
            alpha = max(alpha, eval)
            if beta <= alpha:
                break
        return max_eval

    else:  # MIN
        min_eval = float('inf')
        for move in valid_moves(tablero):
            new_tablero = resultado(tablero, move, player)
            eval = alpha_beta(new_tablero, alpha, beta, 1)
            min_eval = min(min_eval, eval)
            beta = min(beta, eval)
            if beta <= alpha:
                break
        return min_eval
```
## Resultados
El algoritmo implementado fue capaz de optimizar la búsqueda de soluciones en el juego del gato. El uso de la poda alpha-beta permitió reducir el número de nodos evaluados, mejorando la eficiencia sin alterar el resultado final.

## Conclusión
La implementación del algoritmo α−β pruning aplicado al juego del gato muestra la eficacia de este enfoque en la toma de decisiones óptima para juegos de dos jugadores. Este tipo de algoritmos es de gran utilidad para la inteligencia artificial en juegos de estrategia.
