import pygame
import sys


pygame.init()


TAMANO_CASILLA = 100
ANCHO = 3 * TAMANO_CASILLA
ALTO = 3 * TAMANO_CASILLA
VENTANA = pygame.display.set_mode((ANCHO, ALTO))
pygame.display.set_caption("Tres en Raya")


BLANCO = (255, 255, 255)
LINEA_COLOR = (0, 0, 0)
LINEA_ANCHO = 15
CIRCULO_COLOR = (0, 0, 0)
CIRCULO_RADIO = 30
CIRCULO_ANCHO = 15
CROSS_COLOR = (255, 0, 0)
CROSS_ANCHO = 15


tablero = [['' for _ in range(3)] for _ in range(3)]


def dibujar_tablero():
    for fila in range(1, 3):
        pygame.draw.line(VENTANA, LINEA_COLOR, (0, fila * TAMANO_CASILLA), (ANCHO, fila * TAMANO_CASILLA), LINEA_ANCHO)
        pygame.draw.line(VENTANA, LINEA_COLOR, (fila * TAMANO_CASILLA, 0), (fila * TAMANO_CASILLA, ALTO), LINEA_ANCHO)

    for fila in range(3):
        for col in range(3):
            if tablero[fila][col] == 'X':
                pygame.draw.line(VENTANA, CROSS_COLOR, (col * TAMANO_CASILLA + 20, fila * TAMANO_CASILLA + 20),
                                 ((col + 1) * TAMANO_CASILLA - 20, (fila + 1) * TAMANO_CASILLA - 20), CROSS_ANCHO)
                pygame.draw.line(VENTANA, CROSS_COLOR, ((col + 1) * TAMANO_CASILLA - 20, fila * TAMANO_CASILLA + 20),
                                 (col * TAMANO_CASILLA + 20, (fila + 1) * TAMANO_CASILLA - 20), CROSS_ANCHO)
            elif tablero[fila][col] == 'O':
                pygame.draw.circle(VENTANA, CIRCULO_COLOR, (col * TAMANO_CASILLA + TAMANO_CASILLA // 2,
                                                            fila * TAMANO_CASILLA + TAMANO_CASILLA // 2),
                                   CIRCULO_RADIO, CIRCULO_ANCHO)

def verificar_estado_juego():
    
    for fila in tablero:
        if fila[0] == fila[1] == fila[2] and fila[0] != '':
            return fila[0]

    
    for col in range(3):
        if tablero[0][col] == tablero[1][col] == tablero[2][col] and tablero[0][col] != '':
            return tablero[0][col]

    
    if tablero[0][0] == tablero[1][1] == tablero[2][2] and tablero[0][0] != '':
        return tablero[0][0]
    if tablero[0][2] == tablero[1][1] == tablero[2][0] and tablero[0][2] != '':
        return tablero[0][2]

    
    empate = True
    for fila in tablero:
        for casilla in fila:
            if casilla == '':
                empate = False
                break
    if empate:
        return 'Empate'

    return None

def minimax(tablero, profundidad, es_maximizador):
    resultado = verificar_estado_juego()

    if resultado is not None:
        if resultado == 'X':
            return 1
        elif resultado == 'O':
            return -1
        else:
            return 0

    if es_maximizador:
        mejor_valor = -float('inf')
        for i in range(3):
            for j in range(3):
                if tablero[i][j] == '':
                    tablero[i][j] = 'X'
                    valor = minimax(tablero, profundidad + 1, False)
                    tablero[i][j] = ''
                    mejor_valor = max(mejor_valor, valor)
        return mejor_valor
    else:
        mejor_valor = float('inf')
        for i in range(3):
            for j in range(3):
                if tablero[i][j] == '':
                    tablero[i][j] = 'O'
                    valor = minimax(tablero, profundidad + 1, True)
                    tablero[i][j] = ''
                    mejor_valor = min(mejor_valor, valor)
        return mejor_valor

def encontrar_mejor_jugada():
    mejor_movimiento = None
    mejor_valor = -float('inf')

    for i in range(3):
        for j in range(3):
            if tablero[i][j] == '':
                tablero[i][j] = 'X'
                valor = minimax(tablero, 0, False)
                tablero[i][j] = ''
                if valor > mejor_valor:
                    mejor_valor = valor
                    mejor_movimiento = (i, j)

    return mejor_movimiento

turno = 'X'

while True:
    for evento in pygame.event.get():
        if evento.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

        if evento.type == pygame.MOUSEBUTTONDOWN and turno == 'O':
            x, y = pygame.mouse.get_pos()
            fila = y // TAMANO_CASILLA
            col = x // TAMANO_CASILLA
            if tablero[fila][col] == '':
                tablero[fila][col] = 'O'
                turno = 'X'

    if turno == 'X':
        mejor_movimiento = encontrar_mejor_jugada()
        if mejor_movimiento:
            fila, col = mejor_movimiento
            tablero[fila][col] = 'X'
            turno = 'O'
             # Agregar la impresión de la evaluación
            evaluacion = minimax(tablero, 0, False)
            print(f"El algoritmo ha optado por marcar el cuadrado en pos ({fila}, {col}) con una evaluación de: {evaluacion} ")

    VENTANA.fill(BLANCO)
    dibujar_tablero()
    pygame.display.update()

    resultado = verificar_estado_juego()
    if resultado:
        if resultado == 'Empate':
            print("¡Empate!")
        else:
            print(f"¡{resultado} gana!")
        pygame.time.wait(2000)
        pygame.quit()
        sys.exit()
