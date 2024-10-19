# Sudoku_con_Heuristica

Código de solución de sudoku con una heurística 
El código en general se encuentra documentado, pero para un mejor entendimiento del código se 
Agregará este apartado.

La heurística que se aplicó al código fue: MRV


Integrantes de la solucion:

- Francisco Calisto
- Francisca Gutierrez

--------------------------

Explicación del Código

 ### 1. Importaciones

    import time

Solo se utiliza la biblioteca time para medir el tiempo de ejecución del algoritmo.

### 2. Definición de Constantes
   
       N = 9  # Tamaño del tablero de Sudoku (9x9)

El tamaño del tablero de Sudoku es fijo y tiene dimensiones 9x9, lo cual es estándar para este tipo de puzzles.

### 3. Función de Impresión del Tablero

       def printing(arr):
        print("┌───────┬───────┬───────┐")
        for i in range(N):
            for j in range(N):
                if j % 3 == 0:
                    print("│", end=" ")
                print(arr[i][j] if arr[i][j] != 0 else ".", end=" ")
            print("│")
            if (i + 1) % 3 == 0 and i != N - 1:
                print("├───────┼───────┼───────┤")
        print("└───────┴───────┴───────┘")

Propósito: Imprime el tablero de Sudoku en un formato claro y visualmente organizado, con un recuadro que delimita los subcuadrantes 3x3.

Detalles: Las celdas vacías se representan con un punto (.) para facilitar la visualización.

### 4. Verificación de Celdas Seguras (isSafe)
   
        def isSafe(grid, row, col, num):
            if num in grid[row]:
                return False
            if num in [grid[i][col] for i in range(N)]:
                return False
            startRow, startCol = row - row % 3, col - col % 3
            for i in range(3):
                for j in range(3):
                    if grid[i + startRow][j + startCol] == num:
                        return False
            return True

Propósito: Determina si un número puede ser colocado en una celda específica, verificando:
- La fila.
- La columna.
- El subcuadrante 3x3.

### 5. Heurística de Valores Mínimos Restantes (MRV)
        
        def findEmptyCell(grid):
            min_options = 10
            best_row, best_col = -1, -1
            for row in range(N):
                for col in range(N):
                    if grid[row][col] == 0:
                        options = sum(1 for num in range(1, N+1) if isSafe(grid, row, col, num))
                        if options < min_options:
                            min_options = options
                            best_row, best_col = row, col
            return best_row, best_col

Propósito: Implementa la heurística MRV, que selecciona la celda vacía con la menor cantidad de opciones posibles. Esto reduce el espacio de búsqueda y acelera la solución.

Detalles: Para cada celda vacía, calcula cuántos números del 1 al 9 podrían colocarse de manera segura. Elige la celda con menos posibilidades.

### 6. Algoritmo de Búsqueda con Backtracking

        def solveSudoku(grid):
            row, col = findEmptyCell(grid)
            if row == -1 and col == -1:
                return True
            for num in range(1, N + 1):
                if isSafe(grid, row, col, num):
                    grid[row][col] = num
                    if solveSudoku(grid):
                        return True
                    grid[row][col] = 0
            return False

Propósito: Utiliza backtracking para probar diferentes números en las celdas vacías. El algoritmo:
- Encuentra la celda vacía más prometedora (usando MRV).
- Intenta colocar un número válido.
- Si encuentra un error, revierte el cambio (backtracking) y prueba otro número.

### 7. Medición del Tiempo de Ejecución

        start_time = time.time()
        
        if solveSudoku(grid):
            printing(grid)
        else:
            print("No tiene solución")
        
        end_time = time.time()
        execution_time = end_time - start_time
        print(f"\nTiempo de ejecución: {execution_time} segundos")

Propósito: Mide y muestra el tiempo de ejecución del algoritmo. Esto es importante para evaluar la eficiencia del agente inteligente.

Detalles: El tiempo se mide en segundos y muestra cuánto demora el algoritmo en converger (resolver el Sudoku).

### 8. Tablero de Sudoku de Entrada

        grid = [[ 6, 0, 2, 0, 0, 0, 0, 0, 0],
                [ 3, 0, 0, 0, 0, 2, 0, 5, 0],
                [ 0, 4, 0, 3, 0, 0, 0, 7, 0],
                [ 0, 8, 0, 4, 0, 0, 0, 9, 0],
                [ 0, 0, 0, 0, 0, 0, 0, 0, 0],
                [ 0, 3, 0, 0, 1, 0, 0, 0, 5],
                [ 0, 0, 0, 2, 0, 0, 0, 0, 6],
                [ 0, 6, 8, 0, 0, 4, 0, 0, 1],
                [ 0, 5, 0, 9, 0, 0, 0, 0, 8]]


----------------------------

## Codigos

Cabe agregar que el código presentado de las clases no contaba con una heurística.
La forma en la que relizaba la solución era por medio de backtracking.

A esa versión se le agregó la heurística: Minimum Remaining Values (MRV).
Que consiste en elegir la celda vacía con el menor número de opciones disponibles.
Agregar esta heuritica al algotimo ayudo a que los tiempos de de solucion del agoritmos
Se redujerán significativamente 

La versión anterior del código con el sudoku que se encuentra agregado 
En el archivo ipynb tuvo un tiempo de solución de 3.97 segundos.

Y con el código con la heurística, paso a solucionarlo en 1.39 segundos 

Hemos notado que las dos formas con bactraking y MRV sacan tiempos parecidos en pruebas con otros sudokus de mayo dificultad.
