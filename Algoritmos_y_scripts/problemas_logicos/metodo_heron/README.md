# Método de Herón

Este programa implementa el **método de Herón de Alejandría** para aproximar la raíz cuadrada de un número positivo.

---

## Descripción del algoritmo

El método consiste en:

1. **Inicialización:** Se comienza con un valor inicial positivo arbitrario (`candidato`), que será la estimación inicial de la raíz cuadrada.
2. **Promedio:** Se calcula el promedio entre el valor actual de `candidato` y el cociente `S / candidato`.
3. **Repetición:** El proceso se repite hasta que la diferencia absoluta entre `S` y `candidato²` sea menor o igual a la tolerancia establecida.

Este método es iterativo y converge rápidamente a la raíz cuadrada, incluso con un candidato inicial poco preciso.
Si el candidato inicial está muy alejado del valor real, el algoritmo puede necesitar más iteraciones. La precisión alcanzable depende de la representación numérica.

---

## Ejemplo de ejecución

Entrada:

```text
Ingrese el número positivo S: 25
Ingrese el valor inicial del candidato: 5
Ingrese la tolerancia: 0.0001
```

Salida:

```text
La raíz cuadrada aproximada de 25.0 es: 5.0
```

---

## Requisitos y ejecución

Python 3, sin dependencias externas. Desde la raíz del repositorio:

```bash
cd Algoritmos_y_scripts/problemas_logicos/metodo_heron
python heron.py
```

Ingresa:

1. **S:** Número positivo del que se desea calcular la raíz cuadrada.
2. **Candidato:** Estimación inicial.
3. **Tolerancia:** Precisión deseada para la aproximación.

---

## Alcance y limitaciones

La aproximación iterativa está implementada. Usa `S`, candidato y tolerancia positivos; un candidato cero provoca división por cero. La tolerancia se aplica a `abs(S - candidato²)`, no al error absoluto de la raíz. No hay validación de entradas ni límite de iteraciones, por lo que no se garantiza la terminación con tolerancias incompatibles con la precisión numérica.

[Volver al portafolio](../../../README.md)
