# CC219-TP-TF-2025-2-CC92

# Publicación de resultados y conclusiones

## Publicación de resultados
Durante el Hito 2 entrenamos dos familias de modelos: (1) clasificadores de texto para detectar reseñas falsas a partir de la columna `reviewContent_clean` con representaciones BoW, TF‑IDF y BERT; y (2) modelos estructurados para detectar cuentas con comportamiento automatizado ("bots") usando métricas del usuario como antigüedad, volumen de reseñas y tasas de utilidad. Las métricas se calcularon sobre un conjunto de prueba estratificado (20 % de los datos) y permiten comparar precisión, recall, F1 y ROC-AUC. La siguiente tabla resume los resultados más relevantes:

| Modelo | Representación | Exactitud | Precisión | Recall | F1 | ROC-AUC |
| --- | --- | --- | --- | --- | --- | --- |
| LightGBM | Estructurado | 0.888 | 0.704 | 0.888 | 0.785 | 0.956 |
| SVM (RBF) | Estructurado | 0.817 | 0.561 | 0.938 | 0.702 | 0.936 |
| LogReg + TF-IDF | Texto (TF-IDF) | 0.698 | 0.405 | 0.666 | 0.504 | 0.752 |
| LogReg + BoW | Texto (BoW) | 0.676 | 0.376 | 0.619 | 0.468 | 0.715 |
| LogReg + BERT | Texto (BERT) | 0.657 | 0.360 | 0.631 | 0.459 | 0.713 |
| RandomForest + BoW | Texto (BoW) | 0.773 | 0.529 | 0.119 | 0.195 | 0.737 |
| RandomForest + TF-IDF | Texto (TF-IDF) | 0.772 | 0.523 | 0.117 | 0.191 | 0.730 |
| RandomForest + BERT | Texto (BERT) | 0.771 | 0.692 | 0.007 | 0.014 | 0.709 |
| K-Means (exploratorio) | Estructurado | 0.770 | 0.000 | 0.000 | 0.000 | 0.500 |

**Hallazgos clave**
- Los modelos basados en atributos del usuario son los más confiables: LightGBM detecta cuentas sospechosas con un F1 de 0.79 y casi un 96 % de área bajo la curva ROC, lo que significa que distingue muy bien entre usuarios reales y bots.
- El SVM también es sólido: atrapa al 94 % de los casos problemáticos (recall) aunque sacrifica algo de precisión.
- En texto, la Regresión Logística con TF-IDF fue el mejor compromiso (F1≈0.50), lo que indica que todavía acierta 1 de cada 2 reseñas clasificadas como falsas; BoW y BERT ofrecen rendimientos parecidos.
- Los Random Forest sobre texto pierden rendimiento porque detectan pocos falsos (recall < 0.12) a pesar de ser precisos en los pocos casos que marcan.
- El clustering K-Means sólo sirvió como exploración: al no estar supervisado, no distingue correctamente entre reseñas genuinas y falsas.
- El análisis de importancia de variables mostró que la **antigüedad del usuario**, la **longitud de las reseñas** y la **tasa de reseñas útiles** son los indicadores más fuertes para detectar cuentas dudosas.

## Conclusiones
El proyecto aplicó una combinación de técnicas de Procesamiento de Lenguaje Natural (limpieza, BoW, TF-IDF y embeddings BERT) y de aprendizaje automático sobre datos estructurados (ingeniería de atributos, escalamiento, LightGBM, SVM y clustering). Los mejores resultados provinieron de LightGBM y SVM sobre atributos del usuario, alcanzando métricas propias de un sistema listo para despliegue (F1 ≥ 0.70 y ROC-AUC ≥ 0.93). Los modelos de texto todavía requieren mejoras (F1 ≤ 0.50), lo que sugiere explorar balanceo de clases, ajuste fino de BERT o modelos de lenguaje más grandes. Como trabajo futuro se recomienda: (1) combinar las salidas de texto y datos estructurados en un ensamblado único, (2) incorporar explicabilidad (SHAP) para apoyar decisiones operativas y (3) actualizar periódicamente los modelos con nuevas reseñas para reducir el drift en vocabulario y comportamiento de los usuarios.
