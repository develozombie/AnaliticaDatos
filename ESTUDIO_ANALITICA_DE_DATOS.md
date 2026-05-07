# ESTUDIO CONSOLIDADO: ANALÍTICA DE DATOS
**Preparado para el examen - Todas las sesiones**

---

## SESIÓN 1: FUNDAMENTOS DE ANALÍTICA

### 1.1 Conceptos Clave en Analítica
- **Analítica de Datos**: Proceso de examinar datos brutos para encontrar patrones, llegar a conclusiones y tomar decisiones informadas
- **Objetivo Principal**: Transformar datos en información accionable
- **Diferencia clave**: 
  - **Análisis descriptivo**: QUÉ pasó (reportes históricos)
  - **Análisis predictivo**: QUÉ va a pasar (predicciones)
  - **Análisis prescriptivo**: QUÉ hacer (recomendaciones)

### 1.2 Caso de Estudio: MONEYBALL
**Contexto**: Oakland Athletics (Béisbol) - 2002

**Problema**: 
- Equipo pobre con presupuesto mínimo ($41M vs $126M de equipos ricos)
- Necesitaba competir con equipos con 3x más presupuesto

**Solución Analítica**:
- **Billy Beane** (Manager) + **Paul DePodesta** (Analista)
- Rechazar métricas tradicionales (HR, RBIs, AVG)
- Usar **sabermetrics**: OBP (On-Base Percentage) y SLG (Slugging)
- **Insight clave**: Jugadores subestimados en el mercado que podían llegar a base

**Resultados**:
- Ganaron 103 juegos con presupuesto bajo
- Demostraron que análisis de datos > scouting tradicional
- Revolucionó cómo se reclutan jugadores en deportes

**Lección para Analítica**:
1. Cuestionar el status quo
2. Usar datos, no intuición
3. Encontrar ineficiencias en el mercado/datos
4. Métricas correctas = ventaja competitiva

### 1.3 Caso de Estudio: KHAN ACADEMY
**Contexto**: Educación en línea

**Problema**:
- Estudiantes con diferentes ritmos de aprendizaje
- Profesor no puede personalizar enseñanza
- Datos faltaban sobre qué estudiantes iban rezagados

**Solución Analítica**:
- Plataforma recolecta datos de cada interacción
- Monitoreo en tiempo real de progreso
- Identifica estudiantes en riesgo
- Recomendaciones personalizadas de videos

**Datos Clave Recolectados**:
- Tiempo en cada lección
- Tasa de error
- Patrones de revisión
- Progresión por tema

**Impacto**:
- Educación escalable y personalizada
- Datos guían intervención docente
- Mejora resultados académicos

### 1.4 La Ciencia de Datos en Contexto
**Ciclo de la Ciencia de Datos**:
1. **Pregunta/Problema** → Definir qué se quiere saber
2. **Recolección de Datos** → Obtener datos relevantes
3. **Limpieza de Datos** → Preparar datos
4. **Análisis Exploratorio** → Entender patrones
5. **Modelamiento** → Crear modelos predictivos
6. **Evaluación** → Validar resultados
7. **Comunicación** → Compartir insights
8. **Acción** → Tomar decisiones basadas en datos

---

## SESIÓN 2: HERRAMIENTAS Y PREPARACIÓN DE DATOS

### 2.1 Introducción a Python para Analítica
**Por qué Python**:
- Lenguaje interpretado (fácil de aprender)
- Librerías poderosas: pandas, numpy, matplotlib, scikit-learn
- Comunidad activa
- Estándar en industria de datos

**Librerías Principales**:
```python
import pandas as pd      # Manipulación de datos (DataFrames)
import numpy as np       # Cálculos numéricos
import matplotlib.pyplot as plt  # Visualización
import seaborn as sns    # Gráficos estadísticos
```

### 2.2 Caso Práctico: AUTOS USADOS

#### Problema Original:
Dataset de autos usados con:
- Variables: precio, año, millas, cilindros, hp (caballos de fuerza), etc.
- **Problema**: Muchos datos faltantes (missing values)

#### Tipos de Datos Faltantes:
1. **MCAR** (Missing Completely At Random): Faltan al azar, sin patrón
2. **MAR** (Missing At Random): Falta depende de otra variable observada
3. **MNAR** (Missing Not At Random): Falta depende del valor mismo

#### Estrategias para Manejar Datos Faltantes:

**Opción 1: Eliminar**
```python
df.dropna()  # Elimina filas con NaN
```
- **Ventaja**: Simple, no introduce sesgo
- **Desventaja**: Perdemos información valiosa

**Opción 2: Imputar (Reemplazar)**
- **Media**: Usar promedio (para variables continuas)
- **Mediana**: Menos sensible a outliers
- **Moda**: Para variables categóricas
- **Forward Fill/Backward Fill**: Para series temporales

**Opción 3: Predicción**
- Usar regresión para predecir valores faltantes
- Mantiene relaciones entre variables

### 2.3 Exploración de Datos (EDA)
**Preguntas Clave**:
- ¿Cuántas observaciones tenemos?
- ¿Cuáles son los tipos de variables?
- ¿Cuál es la distribución?
- ¿Hay outliers?
- ¿Hay correlaciones?

**Comandos Python**:
```python
df.head()           # Primeras filas
df.info()           # Tipos de datos
df.describe()       # Estadísticas descriptivas
df.isnull().sum()   # Contar valores faltantes
df.corr()           # Matriz de correlaciones
```

---

## SESIÓN 3: ESTADÍSTICA Y PREPROCESSING

### 3.1 Conceptos Estadísticos Fundamentales

#### Medidas de Tendencia Central:
- **Media (μ)**: Promedio aritmético
- **Mediana**: Valor central (menos sensible a outliers)
- **Moda**: Valor más frecuente

#### Medidas de Dispersión:
- **Rango**: Máx - Mín
- **Varianza (σ²)**: Promedio de desviaciones cuadradas
- **Desviación Estándar (σ)**: Raíz cuadrada de varianza
- **Coeficiente de Variación**: σ/μ (comparar dispersión entre variables)

#### Distribuciones Importantes:
1. **Distribución Normal**:
   - Forma de campana simétrica
   - Media = Mediana = Moda
   - 68% de datos ±1σ, 95% ±2σ, 99.7% ±3σ

2. **Distribución Binomial**:
   - Para eventos con dos resultados (éxito/fracaso)
   - Parámetros: n (intentos), p (probabilidad)

3. **Distribución Uniforme**:
   - Todos los valores igualmente probables

### 3.2 Probabilidad y Conceptos Clave

**Teorema del Límite Central**:
- Cuando n es grande, la distribución de medias muestrales es aproximadamente normal
- Permite usar normal como aproximación

**Intervalo de Confianza**:
- Rango de valores donde esperamos que esté el parámetro poblacional
- Ej: IC 95% significa confianza del 95%
- Fórmula: μ ± (z × σ/√n)

**Pruebas de Hipótesis**:
1. **H₀** (Hipótesis nula): Afirmación que asumimos cierta
2. **H₁** (Hipótesis alternativa): Lo opuesto a H₀
3. **p-value**: Probabilidad de obtener resultados si H₀ es cierta
4. **Nivel de significancia (α)**: Generalmente 0.05 (5%)

**Tipos de Errores**:
- **Error Tipo I (α)**: Rechazar H₀ siendo verdadera (falso positivo)
- **Error Tipo II (β)**: No rechazar H₀ siendo falsa (falso negativo)

### 3.3 Preparación de Datos (Data Preprocessing)

#### Limpieza de Datos:
1. **Detectar outliers**:
   - Método: IQR = Q3 - Q1
   - Outliers: valores < Q1 - 1.5×IQR o > Q3 + 1.5×IQR

2. **Normalización**:
   - Escalar datos a rango similar [0,1]
   - Fórmula: (x - min) / (max - min)
   - Importante para algoritmos sensibles a escala

3. **Estandarización (Z-score)**:
   - Fórmula: (x - μ) / σ
   - Media 0, desv. estándar 1

4. **Codificación de Categorías**:
   - **Label Encoding**: 0, 1, 2... (orden importa)
   - **One-Hot Encoding**: Variables binarias (sin orden)

#### Transformaciones:
- **Log Transform**: Para datos sesgados
- **Box-Cox**: Transformación automática para normalidad
- **Feature Scaling**: Llevar variables a escala comparable

### 3.4 Análisis de Distribuciones

**Prueba de Normalidad**:
- **Shapiro-Wilk Test**: ¿Es normal esta distribución?
- **Q-Q Plot**: Gráfico para visualizar normalidad

**Simetría y Curtosis**:
- **Skewness** (Asimetría): 
  - Skew = 0 → Simétrica
  - Skew > 0 → Sesgada a derecha
  - Skew < 0 → Sesgada a izquierda
- **Kurtosis**: Qué tan "picuda" es la distribución

---

## SESIÓN 4: VISUALIZACIÓN Y DETECCIÓN DE ANOMALÍAS

### 4.1 Visualización Estadística

#### Histograma:
- **Definición**: Gráfico que muestra frecuencia de valores en rangos (bins)
- **Uso**: Entender distribución de variable continua
- **Interpretación**:
  - Forma simétrica → normal
  - Picos múltiples → múltiples poblaciones
  - Cola larga → sesgada

#### Boxplot (Diagrama de Cajas):
```
      max_outlier
         |
    ┌────┴────┐
    │          │    Q3 (75%)
    │    ▄▄▄▄  │
    │   │────│     Mediana (50%)
    │    ▀▀▀▀  │
    │          │    Q1 (25%)
    └────┬────┘
         |
      min_outlier
```

**Componentes**:
- **Caja**: Rango intercuartílico (IQR) = Q3 - Q1, contiene 50% central
- **Línea interior**: Mediana
- **Bigotes**: Hasta 1.5×IQR desde cuartiles
- **Puntos**: Outliers

**Ventajas**:
- Identifica outliers fácilmente
- Compara múltiples distribuciones lado a lado
- Resumen de 5 números: min, Q1, mediana, Q3, max

### 4.2 Detección de Outliers

#### Métodos:
1. **IQR Method** (Rango Intercuartílico):
   - Outliers: x < Q1 - 1.5×IQR o x > Q3 + 1.5×IQR
   - Robusto, sin asumir distribución

2. **Z-score**:
   - Z = (x - μ) / σ
   - Outlier si |Z| > 3 (normal) o |Z| > 2.5 (estricto)
   - Asume distribución normal

3. **Modified Z-score**:
   - Usa mediana en lugar de media
   - Más robusto que Z-score

#### Tratamiento de Outliers:
- **Remover**: Si son errores de entrada
- **Mantener**: Si son valores verdaderos importantes
- **Transformar**: Log, sqrt, Box-Cox
- **Separar análisis**: Analizar con y sin outliers

### 4.3 Caso: Autos Usados - Análisis de HP (Caballos de Fuerza)

**Descubrimientos Típicos**:
1. Distribución sesgada a derecha (cola hacia arriba)
2. Algunos autos con HP muy alto (outliers)
3. Relación clara con precio (más HP → más caro)
4. Posible relación con año (autos más nuevos = más HP)

**Pasos de Análisis**:
1. Crear histograma de HP → observar forma
2. Crear boxplot → identificar outliers
3. Calcular IQR y límites de outliers
4. Decidir: ¿mantener o remover?
5. Analizar relación HP vs Precio

### 4.4 Correlación y Relaciones

**Coeficiente de Correlación Pearson**:
- Rango: -1 a +1
- +1 = correlación positiva perfecta
- 0 = sin correlación
- -1 = correlación negativa perfecta

**Interpretación**:
- |r| < 0.3 → Débil
- 0.3 ≤ |r| < 0.7 → Moderada
- |r| ≥ 0.7 → Fuerte

**Heatmap de Correlación**:
```python
import matplotlib.pyplot as plt
plt.figure(figsize=(10,8))
sns.heatmap(df.corr(), annot=True, cmap='coolwarm', center=0)
plt.show()
```

---

## CONCEPTOS TRANSVERSALES

### 1. Ciclo de Vida de un Proyecto de Analítica

```
1. DEFINICIÓN        → Entender el problema negocio
   ↓
2. EXPLORACIÓN       → EDA, entender datos
   ↓
3. PREPARACIÓN       → Limpiar, transformar
   ↓
4. MODELAMIENTO      → Crear modelos
   ↓
5. EVALUACIÓN        → Validar resultados
   ↓
6. COMUNICACIÓN      → Presentar insights
   ↓
7. IMPLEMENTACIÓN    → Tomar acciones
   ↓
8. MONITOREO         → Vigilar rendimiento
```

### 2. Principios Clave en Analítica

#### Calidad de Datos:
- "Garbage in, garbage out" (GIGO)
- Datos limpios > modelos complejos
- Validación en cada etapa

#### Reproducibilidad:
- Documentar todo el código
- Usar control de versiones
- Scripts vs punto-y-clica

#### Comunicación:
- Los datos son para humanos
- Visualizaciones simples y claras
- Historias con datos

### 3. Herramientas Comunes en Industria

**Excel/Google Sheets**:
- Análisis rápido
- Visualizaciones básicas
- Limitado para datos grandes

**Python/R**:
- Análisis complejo
- Reproducible
- Estándar en ciencia de datos

**SQL**:
- Extraer datos de bases de datos
- Joins, agregaciones, filtros

**Tableau/Power BI**:
- Dashboards interactivos
- Visualización profesional

**Spark/Hadoop**:
- Datos masivos (big data)
- Procesamiento distribuido

---

## PREPARACIÓN PARA EL EXAMEN

### Temas Críticos a Dominar:

✅ **Conceptos Fundamentales**:
- Definición de analítica de datos
- Diferencias entre análisis descriptivo, predictivo, prescriptivo
- Ciclo de vida de proyecto de datos

✅ **Casos de Estudio**:
- Moneyball: ¿Cómo revolucionó con datos? ¿Qué métricas usó?
- Khan Academy: ¿Qué datos recolectan? ¿Cómo los usan?
- Autos Usados: ¿Cómo manejar datos faltantes? ¿Cómo detectar outliers?

✅ **Herramientas y Técnicas**:
- Python: pandas, numpy, matplotlib
- Manejo de datos faltantes (métodos)
- Detección de outliers (IQR, Z-score)
- Visualización: histograma, boxplot

✅ **Estadística**:
- Medidas de tendencia central y dispersión
- Distribuciones normales
- Intervalos de confianza
- Pruebas de hipótesis
- Correlación

✅ **Preparación de Datos**:
- Limpieza
- Normalización y estandarización
- Codificación de categorías
- Tratamiento de outliers

### Ejercicios Tipo Examen:

**Pregunta 1**: "¿Cuál es la diferencia entre análisis descriptivo y predictivo?"
- Respuesta: Descriptivo cuenta QUÉ pasó, predictivo predice QUÉ va a pasar

**Pregunta 2**: "En Moneyball, ¿qué permitió al Oakland Athletics competir con presupuesto bajo?"
- Respuesta: Usar datos/sabermetrics (OBP, SLG) para encontrar jugadores subestimados

**Pregunta 3**: "¿Cómo detectarías outliers en caballos de fuerza (HP) de autos usados?"
- Respuesta: Método IQR: Q1 - 1.5×IQR y Q3 + 1.5×IQR, o Z-score > 3

**Pregunta 4**: "¿Qué es la normalización y por qué es importante?"
- Respuesta: Llevar variables a escala [0,1]: (x-min)/(max-min). Importante para algoritmos sensibles a escala

**Pregunta 5**: "¿Cuáles son las tres formas de manejar datos faltantes?"
- Respuesta: Eliminar filas, imputar (media/mediana/moda), predecir con regresión

**Pregunta 6 (Sesión 5)**: "¿Por qué NO se debe imputar mediana en datos de contaminación ambiental sin análisis de sensibilidad?"
- Respuesta: Datos faltantes podrían indicar negligencia regulatoria. Imputar sin análisis sensibilidad oculta el problema y puede sesgar decisiones críticas de salud pública.

---

## SESIÓN 5: CASO GRUPAL - CIUDADES ASIÁTICAS (CONTAMINACIÓN)

### 5.1 Contexto del Caso

**Problema**: Estudiar contaminación en 30 ciudades asiáticas industrializadas con **datos incompletos**.

**Variables Analizadas**:
- PM10, PM2.5 (partículas inhalables)
- SO2, CO, NOX, NH3, CN (sustancias químicas)
- Número de empresas de alto volumen

### 5.2 Faltabilidad de Datos - Estrategia de Decisión

**Tolerancia máxima de negocio**: 25%

**Framework de decisión**:
1. **Faltabilidad ≤ 25%**: Imputar por MEDIANA
2. **Faltabilidad > 25%**: Eliminar variable (información insuficiente)
3. **Variables categóricas**: No imputar, marcas como "Desconocido"

### 5.3 ¿Por Qué MEDIANA y No MEDIA?

| Aspecto | Mediana | Media |
|---------|---------|-------|
| Robustez outliers | ✅ Inmune | ❌ Afectada |
| Distribuciones asimétricas | ✅ Ideal | ❌ Sesgada |
| Interpretabilidad | ✅ 50-50 split | ⚠️ Punto balance |
| Preserva varianza | ❌ Reduce | ⚠️ Mantiene |
| Para emisiones químicas | ✅ RECOMENDADA | ❌ No |

**Justificación**: Emisiones tienen distribuciones asimétricas (colas largas). Una ciudad extremadamente contaminada sesga la media. Mediana es más representativa del "nivel típico".

### 5.4 Imputación Implementada

```python
# Proceso paso a paso:
import pandas as pd

# 1. Identificar variables con faltabilidad ≤ 25%
variables_imputar = ['PM10', 'PM2.5', 'SO2', 'CO', 'NOX', 'NH3', 'CN']

# 2. Calcular medianas
for var in variables_imputar:
    mediana = df[var].median()
    print(f"{var} - Mediana: {mediana:.4f}")

# 3. Imputar
for var in variables_imputar:
    df[var].fillna(df[var].median(), inplace=True)

# 4. Verificar
print(df.isnull().sum())  # Debe mostrar todo en 0
```

**Resultado**: Dataset completamente funcional (0 valores faltantes)

### 5.5 Riesgos Críticos de Imputación en Datos Ambientales

⚠️ **IMPORTANTE**: En salud pública/ambiental, la imputación tiene riesgos específicos:

#### Riesgo 1: Enmascaramiento de Responsabilidad
```
REALIDAD:
  Ciudad A: PM2.5 reportado = 45 μg/m³
  Ciudad B: PM2.5 NO reportado (falta de monitoreo)

SIN IMPUTACIÓN:
  ✓ Visible que Ciudad B está incompleta
  ✓ Señal de alarma: negligencia regulatoria
  ✓ Presión política para mejorar monitoreo

CON IMPUTACIÓN (mediana=50):
  ✗ Aparenta que Ciudad B ≈ 50 μg/m³
  ✗ Oculta que hay falta de monitoreo
  ✗ Se debilitan incentivos para mejorar sistema
```

#### Riesgo 2: Sesgo en Decisiones Críticas
```
Cadena de decisión:
  Análisis datos → Límites regulatorios → Salud de población

Si imputamos incorrectamente:
  → Podemos SUBESTIMAR riesgos en ciudades sin datos
  → Ciudades con monitoreo deficiente = probablemente MÁS contaminadas
  → Imputación de mediana global ≠ realidad local
  → RESULTADO: Protección ambiental insuficiente
```

#### Riesgo 3: Pérdida de Variabilidad Temporal
```
Las emisiones varían por:
  • Estación (inversión térmica en invierno)
  • Día de semana (tráfico más alto entre semana)
  • Eventos (reparaciones, picos de producción)

Una mediana global oculta estos patrones:
  → No captura picos críticos
  → Alerta tardía o ausente
  → Riesgo de salud no detectado
```

#### Riesgo 4: Debilitamiento de Marco Regulatorio
```
¿Qué incentiva a ciudades a REPORTAR completo?

ESCENARIO A (Sin imputación - Transparencia):
  → Datos incompletos = VISIBLE
  → Presión política
  → Inversión en monitoreo

ESCENARIO B (Con imputación - Ocultamiento):
  → Datos incompletos se "completan"
  → Problema no visible
  → Menos inversión
  → SISTEMA SE DEGRADA
```

### 5.6 Framework Recomendado: 5 Pasos

#### PASO 1: Investigar el Mecanismo de Falta

Categorizar según:

| Categoría | Definición | Acción |
|-----------|-----------|--------|
| **MCAR** | Missing Completely At Random (error técnico) | ✅ Imputar es seguro |
| **MAR** | Missing At Random (falta depende de variable observable) | ⚠️ Complejo, análisis sensibilidad |
| **MNAR** | Missing Not At Random (falta depende del valor no observado) | ❌ ALTO RIESGO, evitar |

**En contaminación ambiental**:
- Datos faltantes probablemente sean **MNAR**
- Industrias no reportan PORQUE están muy contaminadas
- La falta está CORRELACIONADA con lo que medimos
- ⚠️ Conclusión: Imputación puede SESGAR gravemente

#### PASO 2: Contextualizar Tolerancia con Stakeholders

**NO existe "25% universal"** - debe pactarse según contexto:

| Contexto | Tolerancia | Justificación |
|----------|-----------|---------------|
| Análisis exploratorio | 30-40% | Buscamos patrones, toleramos incertidumbre |
| Reportes gerenciales | 10-20% | Decisiones operacionales importantes |
| Política pública ambiental | 5-10% | Afecta millones de vidas, precisión CRÍTICA |
| Estudios farmacéuticos | <5% | Vidas en riesgo directo |

**Para este caso**: Contaminación ambiental → Usar **10-15%**, NO 25%

#### PASO 3: Análisis de Sensibilidad OBLIGATORIO

**NUNCA hacer UN solo análisis. Hacer TRES:**

```python
# ANÁLISIS A: Solo datos sin falta (Listwise deletion)
df_complete = df.dropna()
resultado_A = df_complete['PM25'].mean()

# ANÁLISIS B: Con imputación mediana (estándar)
df_imputado = df.copy()
df_imputado['PM25'].fillna(df['PM25'].median(), inplace=True)
resultado_B = df_imputado['PM25'].mean()

# ANÁLISIS C: Imputación pesimista (P90 en lugar de P50)
df_pesimista = df.copy()
df_pesimista['PM25'].fillna(df['PM25'].quantile(0.90), inplace=True)
resultado_C = df_pesimista['PM25'].mean()

# COMPARACIÓN E INTERPRETACIÓN
print(f"A (Completos): {resultado_A:.2f}")
print(f"B (Mediana): {resultado_B:.2f}")
print(f"C (P90): {resultado_C:.2f}")

if abs(resultado_A - resultado_B) < 5 and abs(resultado_B - resultado_C) < 5:
    print("✅ CONCLUSIONES ROBUSTAS - datos faltantes NO sesgan análisis")
else:
    print("⚠️ CONCLUSIONES SENSIBLES - REPORTAR limitación en análisis")
```

**Interpretación**:
- Si A ≈ B ≈ C → Conclusiones robustas, confiables
- Si A ≠ B ≠ C → Problema de datos faltantes, NO confiar en conclusiones

#### PASO 4: Máxima Transparencia en Reporte

**TODO reporte DEBE incluir:**

```
✓ Tabla de datos faltantes (cantidad y %)
✓ Qué método de imputación se usó y POR QUÉ
✓ DÓNDE estaban los faltantes (qué ciudades, variables)
✓ Visualización de datos faltantes vs imputados
✓ Resultados de análisis de sensibilidad
✓ LIMITACIONES de conclusiones debido a datos faltantes
✓ Recomendaciones para mejorar datos en futuro
```

**NO DEBE HACERSE:**
```
✗ Esconder que hubo imputación
✗ Reportar N como si todos fueran reales (incluir nota al pie)
✗ Extraer conclusiones muy fuertes sobre datos imputados
✗ Olvidar que varianza fue artificialmente reducida
```

#### PASO 5: Ética y Responsabilidad

**Pregunta clave antes de publicar:**

> "Si fuera mi ciudad y mis datos fueran los faltantes,  
> ¿usaría yo estas conclusiones para tomar decisiones sobre salud ambiental?"

Si la respuesta es "No" → Revisar y ajustar análisis.

---

### 5.7 Respuesta a Pregunta 5: Línea de Pensamiento Recomendada

#### Parte A: Consecuencias de Imputar en Variables de Emisiones

**Consecuencias Negativas** (RIESGOS):
1. Enmascaramiento de negligencia regulatoria
2. Sesgo en decisiones críticas de salud pública
3. Pérdida de variabilidad temporal/estacional
4. Debilitamiento de incentivos para mejorar monitoreo
5. Falsa confianza en análisis ("datos completos")

**Consecuencias Potencialmente Positivas** (BENEFICIOS):
1. Dataset completo sin perder ciudades
2. Análisis más robusto estadísticamente (si falta es aleatoria)
3. Relaciones entre variables preservadas
4. Visualizaciones más completas
5. Detección de tendencias globales

**Balance**:
En contexto de CONTAMINACIÓN AMBIENTAL → **RIESGOS >> BENEFICIOS**

#### Parte B: Línea de Pensamiento Correcta para Equipos de Data Analytics

**Principio Fundamental**:

```
╔════════════════════════════════════════════════════════════════╗
║  "NO TODO LO QUE SE PUEDE IMPUTAR DEBE SER IMPUTADO"         ║
║                                                                ║
║  La ausencia de datos es INFORMACIÓN que merece ser:          ║
║    • Preservada (no ocultada)                                 ║
║    • Reportada (documentada)                                  ║
║    • Investigada (entender POR QUÉ)                           ║
╚════════════════════════════════════════════════════════════════╝
```

**Principios Rectores**:

| Principio | Significado | Implementación |
|-----------|-------------|-----------------|
| **Investigar antes de actuar** | Entender mecanismo de falta | ¿MCAR? ¿MAR? ¿MNAR? |
| **Contextualizar tolerancia** | No existe regla universal | Pactar con stakeholders |
| **Análisis de sensibilidad** | Validar robustez | 3 análisis (con/sin/pesimista) |
| **Máxima transparencia** | Documentar todo | Archivo de metadata |
| **Ética primero** | ¿Mi análisis ayuda o daña? | Pensar en consecuencias reales |
| **Reproducibilidad** | Otro analista = mismo resultado | Código limpio y comentado |

---

### 5.8 Archivos de Sesión 5 Generados

1. **Sesion_5_Contaminacion_Ciudades_Asiaticas.ipynb** ⭐ PRINCIPAL
   - Notebook ejecutable con todas 5 preguntas respondidas
   - Código Python reproducible
   - Visualizaciones incluidas
   - Análisis paso a paso

2. **Sesion_5_Contaminacion_Ciudades_Asiaticas_IMPUTADO.xlsx**
   - Dataset limpio (sin valores faltantes)
   - Listo para análisis subsecuentes
   - Traceable (se puede auditar qué fue imputado)

3. **INFORME_EJECUTIVO_Sesion5_Contaminacion.md** 📋 REFERENCIA
   - Discusión profunda sobre ética en imputación
   - Framework completo de 5 pasos
   - Recomendaciones basadas en contexto
   - Referencias académicas

4. **GUIA_RAPIDA_Sesion5.md** 🚀 RESUMEN
   - Respuestas directas a 5 preguntas
   - Matrices de decisión
   - Plantillas de código
   - Checklist de entrega



### Estadística:
- **Media**: μ = Σx / n
- **Varianza**: σ² = Σ(x - μ)² / n
- **Desv. Estándar**: σ = √σ²
- **Z-score**: Z = (x - μ) / σ
- **IQR**: Q3 - Q1
- **Correlación**: r = Σ[(x-μx)(y-μy)] / (n×σx×σy)

### Normalización:
- **Min-Max**: (x - min) / (max - min)
- **Z-score**: (x - μ) / σ

### Confianza:
- **Intervalo de Confianza**: μ ± (z × σ/√n)

---

## NOTAS FINALES

**Recuerda antes del examen**:
1. Lee cuidadosamente cada pregunta
2. Identifica si pide concepto, fórmula o aplicación
3. Usa ejemplos de los casos de estudio
4. Explica el "por qué", no solo el "qué"
5. Si no sabes, usa lógica y datos que sí conozcas

**Buena suerte en tu examen de Analítica de Datos** 📊✨
