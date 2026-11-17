# Métodos de búsqueda

Este directorio contiene dos implementaciones clásicas de algoritmos de búsqueda en Python:

- **Búsqueda lineal** (`lineal.py`)
- **Búsqueda binaria** (`binaria.py`)

---

## Descripción de los algoritmos

### Búsqueda lineal

- **Funcionamiento:** Recorre la lista elemento por elemento hasta encontrar el valor buscado o llegar al final.
- **Ventajas:** Funciona en listas ordenadas o desordenadas.
- **Desventajas:** Poco eficiente en listas grandes, ya que su complejidad es **O(n)**.

### Búsqueda binaria

- **Funcionamiento:** Requiere que la lista esté **ordenada**. Compara el elemento central y descarta la mitad de la lista en cada paso, repitiendo hasta encontrar el elemento o agotar las opciones.
- **Ventajas:** Muy eficiente para listas grandes, con complejidad **O(log n)**.
- **Desventajas:** Solo se puede aplicar en listas ordenadas.

---

## Resultados registrados en el laboratorio

Las entradas A, B y C no están publicadas en esta carpeta; se conserva la tabla como referencia histórica, no como benchmark reproducible. El contador binario cuenta iteraciones de descarte, mientras que el lineal cuenta elementos examinados; no son métricas idénticas.

### Tabla de resultados

| Algoritmo | Comparaciones en Lista A | Comparaciones en Lista B | Comparaciones en Lista C |
|-----------|--------------------------|--------------------------|--------------------------|
| Lineal    | 3                        | 7                        | 15                       |
| Binaria   | 2                        | 3                        | 4                        |

**Conclusión:** La **búsqueda binaria** realiza muchas menos comparaciones que la lineal, especialmente en listas grandes. Esto se debe a que divide el rango de búsqueda a la mitad en cada paso. Sin embargo, requiere que la lista esté previamente ordenada.

## Requisitos y ejecución

Python 3, sin dependencias externas. Desde la raíz del repositorio:

```bash
cd Algoritmos_y_scripts/metodos_de_busqueda
python lineal.py
python binaria.py
```

Cada script solicita el número buscado y una lista, por ejemplo `[4, 7, 23]`. Muestra si se encontró el valor y su contador. Para la búsqueda binaria, usa una lista no vacía y ordenada de menor a mayor.

## Alcance y limitaciones

Ambos scripts están implementados como ejercicios interactivos. Usan `eval` para leer entradas: introduce únicamente números y listas de confianza. La versión binaria no maneja listas vacías. No hay validación general de entradas.

[Volver al portafolio](../../README.md)
