# Sudoku_con_Heuristica

Código de solución de sudoku con una heurística 
El código en general se encuentra documentado, pero para un mejor entendimiento del código se 
Agregará este apartado.
-----------------
## Importar librerías

  import time

Se importa la librería time para poder medir cuánto tiempo toma resolver el Sudoku. Esto es útil para evaluar el rendimiento del algoritmo.
----------------------
## Definir el tamaño del Sudoku

  N = 9

Aquí se define el tamaño del Sudoku, que es un tablero de 9x9 (9 filas y 9 columnas). Esto es estándar para los Sudokus.
----------------------------------
## Función para imprimir el Sudoku

  def printing(arr):
    """Imprime el tablero de Sudoku"""
    for i in range(N):
        for j in range(N):
            print(arr[i][j], end=" ")
        print()

Esta función printing toma como entrada un arreglo arr que representa el tablero del Sudoku.
Recorre el tablero y muestra cada valor de la fila, separando las celdas por espacios.
Cada fila se imprime en una nueva línea.

## Verificar si es seguro colocar un número en una celda

  def isSafe(grid, row, col, num):
    """Verifica si es seguro asignar un número en una celda específica"""
    if num in grid[row]:  # Revisar la fila
        return False
    if num in [grid[i][col] for i in range(N)]:  # Revisar la columna
        return False
    startRow, startCol = row - row % 3, col - col % 3  # Subcuadrante 3x3
    for i in range(3):
        for j in range(3):
            if grid[i + startRow][j + startCol] == num:
                return False
    return True

La función isSafe revisa si es seguro colocar un número num en la posición (row, col) del tablero:
- Revisar la fila: Comprueba si el número num ya está presente en la fila correspondiente. Si es así, no es seguro colocar el número.
- Revisar la columna: Verifica si el número num está en la columna. Si está, no se puede colocar.
- Revisar el subcuadrante 3x3: Cada tablero de Sudoku se divide en 9 subcuadrantes de 3x3. Esta parte calcula el subcuadrante al que pertenece la celda y revisa si el número ya está en este área.
- 
Si ninguna de estas condiciones se cumple, entonces es seguro colocar el número y la función devuelve True.

## Heurística MRV (Minimum Remaining Values)

  def findEmptyCell(grid):
    """Encuentra la celda vacía con menos opciones (heurística MRV)"""
    min_options = 10  # Más que el máximo número de opciones posibles
    best_row, best_col = -1, -1
    
    for row in range(N):
        for col in range(N):
            if grid[row][col] == 0:  # Encontrar una celda vacía
                options = sum(1 for num in range(1, N+1) if isSafe(grid, row, col, num))
                if options < min_options:
                    min_options = options
                    best_row, best_col = row, col
    
    return best_row, best_col

La heurística MRV se usa para mejorar la eficiencia del algoritmo de backtracking.
En vez de elegir celdas vacías al azar, findEmptyCell busca la celda vacía con menos opciones posibles (es decir, el número más bajo de valores válidos que se pueden colocar allí).
Recorre el tablero buscando celdas vacías (aquellas donde el valor es 0) y cuenta cuántas opciones válidas tiene cada celda usando la función isSafe.
Devuelve la celda (row, col) con menos opciones válidas.

## Resolver el Sudoku utilizando Backtracking con MRV

  def solveSudoku(grid):
    """Resuelve el Sudoku utilizando backtracking y MRV"""
    row, col = findEmptyCell(grid)  # Usar MRV para elegir la siguiente celda
    
    if row == -1 and col == -1:  # Si no hay más celdas vacías, está resuelto
        return True
    
    for num in range(1, N + 1):
        if isSafe(grid, row, col, num):
            grid[row][col] = num  # Asignar el número
            if solveSudoku(grid):
                return True
            grid[row][col] = 0  # Revertir si no funciona
    
    return False

solveSudoku es la función principal que resuelve el Sudoku utilizando backtracking (una técnica que prueba soluciones y retrocede si encuentra un error) y la heurística MRV.
Primero, encuentra una celda vacía usando MRV (función findEmptyCell). Si no encuentra ninguna (lo que significa que el Sudoku está completo), devuelve True, indicando que ha sido resuelto.
Si encuentra una celda vacía, prueba colocar cada número del 1 al 9 en esa celda. Cada vez que coloca un número, verifica si es seguro usar ese número (isSafe).
Si una asignación es válida, llama recursivamente a solveSudoku para intentar resolver el tablero con esa nueva asignación.
Si una asignación no lleva a una solución válida, se revierte el número (se vuelve a poner en 0) y se prueba el siguiente número.
Si ningún número es válido, devuelve False, lo que hace que retroceda al paso anterior del algoritmo.

## Grid de ejemplo (Tablero de Sudoku)

  grid = [[ 6, 0, 2, 0, 0, 0, 0, 0, 0],
        [ 3, 0, 0, 0, 0, 2, 0, 5, 0],
        [ 0, 4, 0, 3, 0, 0, 0, 7, 0],
        [ 0, 8, 0, 4, 0, 0, 0, 9, 0],
        [ 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [ 0, 3, 0, 0, 1, 0, 0, 0, 5],
        [ 0, 0, 0, 2, 0, 0, 0, 0, 6],
        [ 0, 6, 8, 0, 0, 4, 0, 0, 1],
        [ 0, 5, 0, 9, 0, 0, 0, 0, 8]]

Este es el tablero de Sudoku con algunas celdas ya completadas. Los 0 representan las celdas vacías que necesitan ser llenadas.

## Medir el tiempo de ejecución

  start_time = time.time()


Antes de empezar a resolver el Sudoku, usamos time.time() para guardar el tiempo de inicio. Esto nos ayudará a medir cuánto tiempo toma resolverlo.

## Resolver el Sudoku y medir el tiempo

  if solveSudoku(grid):
    printing(grid)
  else:
    print("No tiene solucion")

  end_time = time.time()
  execution_time = end_time - start_time
  print(f"\nTiempo de ejecución: {execution_time} segundos")

Llamamos a solveSudoku con el tablero inicial.
Si lo resuelve, muestra el tablero completo usando la función printing.
Si no puede encontrar una solución, imprime "No tiene solución".
Finalmente, calculamos el tiempo total de ejecución restando el tiempo de inicio del tiempo final, y lo imprimimos.
