# Análisis Experimental y de Superficie de Respuesta (RSM) en la Calidad Organoléptica y Estructural de Hot Cakes

## 1. Título y Resumen
Evaluación del impacto directo de las proporciones de ingredientes clave (Harina y Leche) sobre la calidad sensorial y la esponjosidad estructural de los hot cakes, utilizando un diseño experimental unifactorial y factorial con análisis de superficie de respuesta (RSM) en R.

## 2. Contexto y Pregunta de Investigación
- **Pregunta:** ¿Cuál es la proporción óptima entre harina y leche que maximiza la esponjosidad (grosor en mm post-cocción) de un hot cake sin deteriorar su aceptación organoléptica general?
- **Objetivo:** Aplicar principios de metrología, diseño de experimentos y optimización estadística aplicables a procesos de ingeniería de alimentos.

## 3. Datos y Fuente
- **Dataset:** `hot_cakes.csv` (almacenado en este repositorio).
- **Variables Independientes (Factores):**
  - $X_1$ (Leche): Evaluada en niveles codificados (0.5, 0.75 y 1.0 tazas).
  - $X_2$ (Harina): Evaluada en niveles codificados (0.5, 0.75 y 1.0 tazas).
- **Variables Dependientes (Respuestas):**
  - $Y_1$ (Esponjosidad): Medida en milímetros de grosor post-cocción.
  - $Y_2$ (Calificación Sensorial 1 y 2): Evaluación organoléptica en escala continua.

## 4. Metodología
1. **Limpieza y Codificación de Datos:** Carga del archivo `.csv` en R y transformación de las descripciones cualitativas de proporciones a valores numéricos continuos.
2. **Modelado Estadístico:** Ajuste del modelo de Superficie de Respuesta (RSM) de primer y segundo orden utilizando la librería `rsm`.
3. **Inferencia Estadística:** Análisis de Varianza (ANOVA) para determinar la significancia de los términos lineales, cuadráticos e interacciones.
4. **Visualización:** Generación de gráficos de tendencia lineal y de dispersión con `ggplot2`.

## 5. Resultados del Análisis Estadístico (ANOVA)

| Variable de Respuesta | Coeficiente de Determinación ($R^2$) | Significancia del Modelo (p-valor) | Efecto Principal Dominante |
| :--- | :--- | :--- | :--- |
| **Esponjosidad ($Y_1$)** | **0.9505** | **< 0.001** | Relación cuadrática Harina/Leche |
| **Calificación Sensorial 1 ($Y_2$)** | **0.7689** | **< 0.01** | Proporción de Leche ($X_1$) |

## 6. Código Fuente en R
```r
library(readxl)
library(rsm)
library(ggplot2)

# 1. Carga de datos y codificación numérica de variables
datos <- read.csv("hot_cakes.csv")
datos$Leche_num <- ifelse(datos$Leche == "media tasa", 0.5,
                   ifelse(datos$Leche == "tres cuartos", 0.75,
                   ifelse(datos$Leche == "una tasa", 1, NA)))

datos$Harina_num <- ifelse(datos$Harina == "media tasa", 0.5,
                    ifelse(datos$Harina == "tres cuartos", 0.75,
                    ifelse(datos$Harina == "una tasa", 1, NA)))

# 2. Ajuste de Modelo de Superficie de Respuesta (RSM) para Esponjosidad
modelo_esponjosidad <- rsm(Esponjocidad ~ FO(Leche_num, Harina_num) +
                           TWI(Leche_num, Harina_num) +
                           PQ(Leche_num, Harina_num), data = datos)
summary(modelo_esponjosidad)
anova(modelo_esponjosidad)

# 3. Visualización gráfica de tendencia
ggplot(datos, aes(x = Leche_num, y = Esponjocidad)) +
  geom_point(size = 3, color = "darkblue") +
  geom_smooth(method = "lm", se = FALSE, color = "red") +
  labs(title = "Esponjosidad vs Proporción de Leche",
       x = "Leche (Tazas)",

## 📄 Documentación Completa
Puedes consultar el reporte completo en formato PDF aquí: [Ver Reporte en PDF](./Análisis Experimental de Calidad de Hot Cakes - Reporte Final.pdf)
       y = "Esponjosidad (mm)") +
  theme_minimal()
