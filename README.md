# Sistema Doble de Procesamiento de Imágenes usando Concurrencia

Este proyecto implementa un sistema concurrente para el procesamiento de imágenes modelado mediante una Red de Petri (RdP). Fue desarrollado como **Trabajo Práctico Final** de la materia **Programación Concurrente** (FCEFyN, UNC - 2023).

## 👥 Integrantes

- Federica Mayorga  
- Giuliano Matías Palombarini  
- Marcos Raimondi  
- Gastón Marcelo Segura  

## 📌 Descripción General

El sistema simula un flujo completo de procesamiento de imágenes en múltiples etapas: carga, mejora de calidad, recorte y exportación. Cada etapa se modela utilizando una Red de Petri (RdP) y se implementa de forma concurrente en Java con **monitores personalizados**, **hilos** y **semáforos**.

Se analizaron distintos niveles de paralelismo (secuencial, semi-secuencial y concurrente), explorando su impacto sobre el tiempo total de ejecución, uso de recursos y validación de políticas.

## 🧠 Objetivos

- Modelar el sistema usando RdP para representar el flujo de imágenes.
- Implementar el sistema en Java utilizando programación concurrente.
- Analizar propiedades de la RdP: vivacidad, ausencia de deadlocks, seguridad, limitación.
- Validar dos políticas de resolución de conflictos (round-robin y probabilística 80/20).
- Evaluar rendimiento según distintas cantidades de hilos y segmentaciones.

## 🧱 Estructura del Sistema

- **Modelo RdP**: Simulado y analizado con PIPE (Platform Independent Petri Net Editor).
- **Implementación**: Java + Threads + Semáforos + Monitores.
- **Sincronización**: Controlada mediante estructuras como `CyclicBarrier`, semáforos y estructuras tipo monitor personalizadas.
- **Políticas de disparo**:
  - *Política 1*: Round-Robin entre transiciones en conflicto.
  - *Política 2*: Priorización probabilística 80%-20% para ciertas ramas.

## 🔄 Flujo de Procesamiento

1. **Carga**: Las imágenes se cargan desde un buffer de entrada.
2. **Ajuste de calidad**: Mejora de resolución u otros parámetros.
3. **Recorte**: Ajuste de dimensiones finales.
4. **Exportación**: Salida de las imágenes del sistema.

Cada transición del flujo está modelada y controlada por hilos responsables, determinados por el análisis de invariantes de transición y plazas.

## 🔧 Arquitectura del Código

- `Main.java`: Inicializa la red, el monitor y los hilos de procesamiento.
- `RdP.java`: Representa la Red de Petri, con métodos para disparo de transiciones y verificación de invariantes.
- `Monitor.java`: Controla el acceso concurrente a la RdP.
- `Proceso.java`: Clase `Runnable` asociada a cada hilo de ejecución.
- `Politica.java`: Superclase para distintas estrategias de resolución de conflictos.
- `Logger.java`: Log de transiciones y verificación mediante expresiones regulares.
- `Cola.java` / `Colas.java`: Representan las colas de espera para cada transición.
- `VectorSensibilizadas.java`: Controla el estado de las transiciones sensibilizadas.
- `Constants.java`: Configuraciones del sistema (secuencias, tiempos, políticas, etc).

## 📊 Resultados y Análisis de Rendimiento

Se realizaron pruebas variando:

- Cantidad de hilos (desde 6 hasta 14).
- Nivel de paralelismo (secuencial, semi y totalmente concurrente).
- Política de resolución de conflictos.

### Tiempos observados (procesamiento de 200 imágenes):

| Modalidad            | Tiempo estimado | Handling por transición |
|----------------------|------------------|--------------------------|
| Secuencial completa  | 25 s             | ~0.035 s                 |
| Semi-secuencial      | 19 s             | ~0.025 s                 |
| Concurrente ideal    | 6.3 s            | ~0.0115 s                |

### Validación:

- El sistema logró cumplir la ejecución de 200 invariantes.
- Se validó que las transiciones se disparan conforme a lo especificado por cada política.
- El uso de expresiones regulares sobre los logs permitió verificar la secuencia de transiciones.

---

**Materia:** Programación Concurrente  
**Universidad Nacional de Córdoba** – Facultad de Ciencias Exactas, Físicas y Naturales  
Año: 2023
