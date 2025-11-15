# Laberinto con Backtracking y Energía
# Casilla verde (inicio) = 5
# Casilla roja (fin)     = 8
# Paredes = 99

ENERGIA_INICIAL = 18
N = 9

# MATRIZ EXACTA DE TU IMAGEN
laberinto = [
    [1, 1, 1, 1, 99, 1, 1, 1, 5],      # Fila 0
    [1, 99, 99, 1, 99, 1, 99, 1, 99],  # Fila 1
    [1, 1, 1, 1, 1, 99, 1, 99, 1],     # Fila 2
    [99, 1, 99, 1, 99, 1, 1, 1, 1],    # Fila 3
    [-2, 1, 99, -1, 1, 1, 1, 1, 3],    # Fila 4
    [1, 1, 99, -1, 99, 1, 1, 1, 1],    # Fila 5
    [1, 99, 1, 1, 1, 1, 99, 1, 99],    # Fila 6
    [1, 1, 1, 99, 2, 1, 1, 1, 1],      # Fila 7
    [8, 1, 3, 1, 1, 1, 99, 1, 1]       # Fila 8
]

# Buscar casilla verde y roja
inicio = None
fin = None
for i in range(N):
    for j in range(N):
        if laberinto[i][j] == 5:
            inicio = (i, j)
        if laberinto[i][j] == 8:
            fin = (i, j)

if inicio is None or fin is None:
    raise Exception("ERROR: falta la casilla verde (5) o la casilla roja (8).")

# Matriz donde marcamos el camino encontrado
camino = [[0]*N for _ in range(N)]

# Orden obligatorio: izquierda, abajo, arriba, derecha
movs = [(0, -1), (1, 0), (-1, 0), (0, 1)]


def backtrack(x, y, energia, visitados):
    # Fuera del tablero
    if x < 0 or x >= N or y < 0 or y >= N:
        return False

    # Pared
    if laberinto[x][y] == 99:
        return False

    # Si ya visitamos esta celda
    if (x, y) in visitados:
        return False

    # Si llegamos a la meta
    if (x, y) == fin:
        camino[x][y] = 1
        return True

    # Calcular energía nueva
    valor = laberinto[x][y]
    if valor not in (5, 8):  # inicio y fin no consumen energía
        energia -= valor  # si valor es negativo, energía sube

    if energia < 0:
        return False

    # Marcar visitado
    visitados.add((x, y))
    camino[x][y] = 1

    # Probar movimientos en el orden obligatorio
    for dx, dy in movs:
        if backtrack(x + dx, y + dy, energia, visitados):
            return True

    # Retroceder
    visitados.remove((x, y))
    camino[x][y] = 0
    return False


# -------------------------
# EJECUCIÓN DEL ALGORITMO
# -------------------------

print("Laberinto original:")
for fila in laberinto:
    print(fila)

print("\nBuscando camino...\n")

exito = backtrack(inicio[0], inicio[1], ENERGIA_INICIAL, set())

if exito:
    print("¡Camino encontrado con la energía disponible!")
else:
    print("NO existe un camino posible con 18 unidades de energía.")

print("\nMatriz del camino (1 = camino):")
for fila in camino:
    print(fila)

