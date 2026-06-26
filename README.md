# Algoritmo_luciernagas_P4_ManejoDeRestricciones

## Algoritmo de las luciérnagas con manejo de restricciones (Práctica 4)

Este repositorio contiene la implementación del **Algoritmo de las Luciérnagas (Firefly Algorithm - FA)** acoplado con la técnica de **Jerarquías Estocásticas (Stochastic Ranking - SR)** y mecanismos de autoadaptación de parámetros en línea. El objetivo principal es resolver problemas de optimización de ingeniería de alta complejidad sujetos a restricciones de desigualdad físicas y geométricas.

## Información Institucional

* **Institución:** Instituto Politécnico Nacional (IPN)
* **Curso:** Algoritmos Bioinspirados
* **Docente:** Dra. Miriam Pescador Rojas
* **Integrantes:**
  * Arcia Portillo Héctor
  * Cortés Reyes Karla Fátima
  * Castro Luna Diego Francisco

---

## Estructura de los Archivos del Proyecto

* **`Luciérnagas_P4_ManejoDeRestricciones.ipynb`**
  * Cuaderno principal de Jupyter que integra las definiciones de los problemas, la lógica del algoritmo FA modificado, los experimentos de sensibilidad de parámetros, las pruebas estadísticas y la visualización de resultados en tiempo real.
* **`README.md`**
  * Guía técnica explicativa del proyecto.
* **`requeriments.txt`**
  * Lista de bibliotecas necesarias para reproducir las simulaciones de forma local.

---

## Metodología de Optimización con Restricciones

La optimización de problemas con restricciones de desigualdad ($g_i(x) \leq 0$) se aborda mediante dos componentes críticos desarrollados sobre el algoritmo FA:

### 1. Jerarquías Estocásticas (Stochastic Ranking)
Basado en el enfoque de Runarsson & Yao, este operador equilibra de manera estocástica la importancia de la función objetivo ($f(x)$) y la suma del cuadrado de las violaciones de restricciones ($\phi(x)$). 
El parámetro de control clave es **$P_f$** (probabilidad de clasificación por objetivo):
* **$P_f = 0.20$ (Conservador):** Prioriza la factibilidad ($\phi(x)$). Útil para problemas con restricciones extremadamente difíciles, aunque con riesgo de estancamiento.
* **$P_f = 0.45$ (Balanceado):** Proporciona un equilibrio entre la exploración y el respeto a la física del problema.
* **$P_f = 0.80$ (Permisivo/Agresivo):** Prioriza la minimización del objetivo ($f(x)$), lo que acelera la velocidad pero incrementa la probabilidad de caer en regiones infactibles.

### 2. Autoadaptación Dinámica de Parámetros
El algoritmo evalúa iteración tras iteración la tasa de éxito ($SR$), definida como la proporción de luciérnagas que mejoraron su posición o factibilidad. 
* Si $SR > 0.3$, se incrementa la exploración aleatoria ($\alpha$) y se facilita la visión a larga distancia (disminuye $\gamma$).
* Si $SR < 0.1$ (indica estancamiento), se reduce drásticamente el tamaño del paso ($\alpha$) para realizar una micro-búsqueda local y se incrementa $\gamma$ para concentrarse en la vecindad inmediata.

---

## Problemas de Ingeniería Evaluados

Se resuelven dos problemas clásicos de diseño mecánico estructural con restricciones complejas:

### Problema 1: Resorte de Compresión (Tension/Compression Spring)
* **Objetivo:** Minimizar el peso de un resorte de compresión.
* **Variables de Diseño:** Diámetro del alambre ($d$), diámetro medio de la espira ($D$) y número de espiras activas ($N$).
* **Restricciones:** 4 restricciones de desigualdad (esfuerzo cortante, deflexión, frecuencia de oscilación libre y límite de diámetro exterior).

### Problema 2: Viga Soldada (Welded Beam)
* **Objetivo:** Minimizar el costo de fabricación de una viga soldada.
* **Variables de Diseño:** Altura del cordón de soldadura ($h$), longitud de la soldadura ($l$), altura de la viga ($t$) y espesor de la viga ($b$).
* **Restricciones:** 7 restricciones físicas y estructurales (esfuerzo cortante en la soldadura, esfuerzo normal en la viga, carga crítica de pandeo, deflexión del extremo y límites geométricos).

---

## Experimentos y Resultados de Sensibilidad

Se realizan pruebas sistemáticas ejecutando **30 corridas independientes** de 200 generaciones por cada nivel de $P_f$ para evaluar la estabilidad del algoritmo y su porcentaje de factibilidad final:

| Problema | $P_f$ | Óptimo Promedio | Desv. Estándar | % Factible |
| :--- | :---: | :---: | :---: | :---: |
| **Resorte** | 0.20 | 0.013246 | 0.000140 | 100.0% |
| **Resorte** | 0.45 | 0.014361 | 0.003260 | 100.0% |
| **Resorte** | 0.80 | 0.015889 | 0.003482 | 100.0% |
| **Viga** | 0.20 | 2.479309 | 0.247862 | 100.0% |
| **Viga** | 0.45 | 2.528818 | 0.218076 | 100.0% |
| **Viga** | 0.80 | 2.649840 | 0.296108 | 100.0% |

*Los datos de los experimentos confirman que el enfoque adaptativo con un parámetro de Stochastic Ranking conservador ($P_f = 0.20$) favorece la estabilidad estructural, permitiendo mantener tasas de factibilidad del 100% en ambos problemas críticos de ingeniería.*

---

## Ejecución del Proyecto

Para reproducir localmente las simulaciones y visualizar el trazado interactivo de las curvas de convergencia, ejecute:

```bash
pip install -r requeriments.txt
