# Asignación de bodegas a clientes

Proyecto académico de **Fundamentos de la Programación**, orientado a lógica computacional, estructuras de control y algoritmos. Se priorizan construcciones básicas, con NumPy para el cálculo de distancias y Matplotlib para visualización.

## Descripción y alcance

El programa lee coordenadas de clientes y bodegas. Para cada cliente, elige la bodega más cercana que aún tenga capacidad; los límites distribuyen la cantidad de clientes de forma equilibrada entre bodegas.

Genera tres visualizaciones: ubicaciones iniciales, asignaciones y recorridos de reparto. Los recorridos usan la heurística del vecino más cercano y regresan a la bodega en el gráfico. Aunque el código los llama «óptimos», no garantizan el mínimo global.

## Estructura

```text
asignacion_bodegas/
├── asignacion_bodegas.py
├── informe_asignacion_bodegas.pdf
├── problema_propuesto.pdf
├── README.md
└── datos/
    ├── clientes_1.txt … clientes_4.txt
    └── bodegas_1.txt … bodegas_4.txt
```

## Requisitos y ejecución

Python 3, NumPy, Matplotlib y un entorno capaz de mostrar ventanas gráficas. Desde la raíz del repositorio:

```bash
cd Algoritmos_y_scripts/proyectos_academicos/asignacion_bodegas
python -m pip install numpy matplotlib
python asignacion_bodegas.py
```

Introduce el nombre **con extensión y sin ruta**, primero bodegas y luego clientes:

```text
Ingrese nombre del archivo de bodegas: bodegas_1.txt
Ingrese nombre del archivo de clientes: clientes_1.txt
```

Los archivos se buscan en `datos/`, relativo al script. Cada línea debe contener dos coordenadas enteras separadas por espacios. Cierra cada ventana gráfica para avanzar a la siguiente.

## Limitaciones

La asignación depende del orden de los clientes y de los cupos disponibles. El script no incorpora manejo de archivos inexistentes, validación general de datos ni optimización global de rutas. Usa los conjuntos de datos publicados para reproducir la práctica.

## Documentación

- [Informe metodológico](informe_asignacion_bodegas.pdf).
- [Problema propuesto](problema_propuesto.pdf).
- [Datos de entrada](datos/).
- [Código Python](asignacion_bodegas.py).

[Volver al portafolio](../../../README.md)
