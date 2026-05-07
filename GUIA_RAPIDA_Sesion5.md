# SESIÓN 5: GUÍA RÁPIDA - TRATAMIENTO DE DATOS FALTANTES
## Ciudades Asiáticas - Análisis de Contaminación

---

## ⚡ RESUMEN EJECUTIVO (1 MINUTO)

| Pregunta | Respuesta |
|----------|-----------|
| **¿Cuántos datos faltan?** | Varía por variable (verificar en Notebook) |
| **¿Qué hacer?** | Imputar variables ≤ 25% faltabilidad por MEDIANA |
| **¿Por qué mediana?** | Robusta ante outliers en variables asimétricas |
| **¿Cuáles son riesgos?** | Puede sesgar análisis si falta es MNAR (no aleatoria) |
| **¿Conclusión?** | Usar análisis de sensibilidad (con/sin imputación) |

---

## 📊 RESPUESTAS DIRECTAS A LAS 5 PREGUNTAS

### PREGUNTA 1: REVISIÓN DE DATOS
```
a) Dimensión: 30 ciudades × 10 variables = 300 celdas
b) Data types: 2 Text (Ciudad, País) + 8 Numeric
c) Últimas 12 filas: Ver en Notebook (output de df.tail(12))
d) Top 5 PM2.5: Ver gráfico en Notebook
e) Datos faltantes: Ver tabla de conteo en Notebook
```

---

### PREGUNTA 2: FALTABILIDAD (% por variable)
```
Fórmula: (Valores_Faltantes / Total_Observaciones) × 100

Para cada variable:
- Ciudad: 0% ✓
- País: 0% ✓
- Número_de_empresas: [Ver Notebook]%
- PM10: [Ver Notebook]%
- PM2.5: [Ver Notebook]%
- SO2: [Ver Notebook]%
- CO: [Ver Notebook]%
- NOX: [Ver Notebook]%
- NH3: [Ver Notebook]%
- CN: [Ver Notebook]%
```

---

### PREGUNTA 3: ESTRATEGIA DE TRATAMIENTO

**MATRIZ DE DECISIÓN:**

```
┌─────────────────────────────────────────────────────────┐
│ SI Faltabilidad < 25%                                   │
│   → IMPUTAR por MEDIANA                                 │
│                                                         │
│ SI Faltabilidad ≥ 25%                                   │
│   → ELIMINAR variable (perdida de información crítica)  │
│                                                         │
│ SI es dato categórico con falta                         │
│   → NO imputar, marcar como "Desconocido"               │
└─────────────────────────────────────────────────────────┘
```

**JUSTIFICACIÓN DE MEDIANA:**

| Aspecto | Razón |
|---------|-------|
| **Robustez** | No es afectada por outliers extremos |
| **Distribuciones asimétricas** | Emisiones tienen colas largas, media está sesgada |
| **Preservación de orden** | Mantiene rangos y relativas de ciudades |
| **Interpretabilidad** | "50% de ciudades < mediana, 50% > mediana" |

---

### PREGUNTA 4: IMPLEMENTACIÓN

**Código Python:**

```python
import pandas as pd

df = pd.read_excel('archivo.xlsx')

# Variables a imputar (con faltabilidad ≤ 25%)
variables_imputar = ['PM10', 'PM2.5', 'SO2', 'CO', 'NOX', 'NH3', 'CN']

# Imputar por mediana
for var in variables_imputar:
    mediana = df[var].median()
    df[var].fillna(mediana, inplace=True)

# Verificar
print(df.isnull().sum())  # Debe mostrar todo en 0
```

**Resultado:**
- Dataset completamente funcional
- 0 valores faltantes
- Varianza artificialmente reducida (DOCUMENTAR)

---

### PREGUNTA 5: IMPLICACIONES ÉTICAS Y LÍNEA DE PENSAMIENTO

#### ⚠️ RIESGOS DE IMPUTAR EN CONTAMINACIÓN AMBIENTAL

1. **ENMASCARAMIENTO DE RESPONSABILIDAD**
   - Dato faltante ≠ contaminación baja
   - Dato faltante = señal de falta monitoreo/negligencia
   - Imputar = ocultar el problema

2. **SESGO EN SALUD PÚBLICA**
   - Decisiones sobre límites de emisiones dependen de datos reales
   - Imputación puede SUBESTIMAR riesgos en ciudades sin monitoreo
   - Resultado: insuficientes medidas de protección ambiental

3. **PÉRDIDA DE VARIABILIDAD**
   - Mediana global ≠ realidad local estacional
   - Picos de contaminación no se capturan
   - Alertas tempranas pueden fallar

4. **INCENTIVOS DÉBILES PARA MEJORAR**
   - Si datos faltantes se "completan" automáticamente
   - ¿Quién invierte en mejorar monitoreo?
   - Sistema se degrada, no mejora

---

#### ✅ LÍNEA DE PENSAMIENTO CORRECTA

**FRAMEWORK EN 5 PASOS:**

```
PASO 1: INVESTIGAR
  ¿Por qué faltan los datos?
  → Error técnico (MCAR) = seguro imputar
  → Negligencia (MNAR) = ALTO RIESGO
  
PASO 2: DEFINIR TOLERANCIA CON STAKEHOLDERS
  ¿Cuánta precisión necesitas?
  → Análisis exploratorio: 20-30% tolerable
  → Política pública: 5-10% máximo
  
PASO 3: ANÁLISIS DE SENSIBILIDAD (OBLIGATORIO)
  Hacer 3 análisis en paralelo:
    A) Solo datos completos (sin imputar)
    B) Con imputación mediana
    C) Con imputación pesimista (P90)
  Si A ≈ B ≈ C → conclusiones robustas
  Si A ≠ B ≠ C → problema crítico, reportar
  
PASO 4: MÁXIMA TRANSPARENCIA
  En reporte SIEMPRE incluir:
    ✓ Cuántos datos faltaban
    ✓ Qué método usaste
    ✓ Dónde estaban (qué ciudades, qué variables)
    ✓ Resultados análisis de sensibilidad
    ✓ Limitaciones del análisis
    
PASO 5: ÉTICA Y RESPONSABILIDAD
  Preguntarse:
    "¿Mi análisis ESCLARECE o ENGAÑA la decisión?"
    "Si fuera mi ciudad, ¿usaría estos datos para política?"
```

---

## 🎯 RESPUESTA RESUMIDA A P5

### "¿Cuáles serían las consecuencias de realizar imputaciones?"

**Consecuencias Negativas:**
- ❌ Ocultar negligencia de gobiernos/industria en monitoreo
- ❌ Basado decisiones de salud pública en datos artificiales
- ❌ Subestimar contaminación en ciudades sin mediciones
- ❌ Debilitar incentivos para mejorar sistemas de monitoreo
- ❌ Crear falsa confianza en análisis ("todos los datos completos")

**Consecuencias Positivas:**
- ✅ Análisis completo sin perder 30 ciudades
- ✅ Relaciones entre variables preservadas
- ✅ Visualizaciones más completas
- ✅ Tendencias detectables si falta es aleatoria

**Balance:**
En contexto ambiental → **RIESGOS > BENEFICIOS**

---

### "¿Cuál debería ser la línea de pensamiento de un equipo de Data Analytics?"

**Respuesta Breve:**
```
"No todo lo que se puede imputar debe ser imputado.
La ausencia de datos es información que merece
ser preservada, reportada e investigada."
```

**Principios Clave:**

| Principio | Significado |
|-----------|-------------|
| **Investigar antes de actuar** | Entender POR QUÉ faltan datos |
| **Contextualizar tolerancia** | No existe "25% universal" |
| **Usar análisis de sensibilidad** | Validar robustez de conclusiones |
| **Máxima transparencia** | Documentar todo, ocultar nada |
| **Ética primero** | ¿Mi análisis ayuda o daña? |
| **Reproducibilidad** | Otro analista debe llegar a mismo resultado |

---

## 📈 ANÁLISIS DE SENSIBILIDAD (PLANTILLA)

**Para validar que conclusiones son robustas:**

```python
# ANÁLISIS A: Solo datos sin falta (Listwise deletion)
df_complete = df.dropna()
resultado_A = df_complete['PM25'].mean()

# ANÁLISIS B: Con imputación mediana
df_imputado = df.copy()
df_imputado['PM25'].fillna(df['PM25'].median(), inplace=True)
resultado_B = df_imputado['PM25'].mean()

# ANÁLISIS C: Imputación pesimista (P90)
df_pesimista = df.copy()
df_pesimista['PM25'].fillna(df['PM25'].quantile(0.90), inplace=True)
resultado_C = df_pesimista['PM25'].mean()

# COMPARACIÓN
print(f"Resultado A (completos): {resultado_A:.2f}")
print(f"Resultado B (mediana): {resultado_B:.2f}")
print(f"Resultado C (P90): {resultado_C:.2f}")

# INTERPRETACIÓN
if abs(resultado_A - resultado_B) < 5 and abs(resultado_B - resultado_C) < 5:
    print("✓ CONCLUSIONES ROBUSTAS - datos faltantes no sesgan análisis")
else:
    print("⚠️ CONCLUSIONES SENSIBLES - reportar limitación en análisis")
```

---

## 📋 CHECKLIST PARA REPORTE FINAL

Antes de entregar el análisis, verificar:

- [ ] **Pregunta 1**: Dimensiones, dtypes, últimas 12 filas, top 5 PM2.5, conteo faltantes reportados
- [ ] **Pregunta 2**: Tabla de faltabilidad (%) por variable creada y visualizada
- [ ] **Pregunta 3**: Estrategia documentada (por qué mediana, por qué no media)
- [ ] **Pregunta 4**: Dataset imputado guardado, verificación de 0 NaN, estadísticas descriptivas
- [ ] **Pregunta 5**: 
  - [ ] Discusión de riesgos de imputación en ambiental
  - [ ] 5 pasos del framework documentados
  - [ ] Respuesta sobre ética y responsabilidad clara
  - [ ] Análisis de sensibilidad ejecutado

- [ ] **Documentación General**:
  - [ ] Código reproducible y comentado
  - [ ] Visualizaciones claras y etiquetadas
  - [ ] Dataset final guardado en formato Excel
  - [ ] Todas las decisiones justificadas

---

## 🔗 ARCHIVOS ENTREGABLES

1. **Sesion_5_Contaminacion_Ciudades_Asiaticas.ipynb** ← PRINCIPAL
   - Contiene todas 5 preguntas respondidas
   - Código ejecutable
   - Visualizaciones incluidas

2. **Sesion_5_Contaminacion_Ciudades_Asiaticas_IMPUTADO.xlsx** ← DATOS
   - Dataset limpio sin valores faltantes
   - Listo para análisis subsecuentes

3. **INFORME_EJECUTIVO_Sesion5_Contaminacion.md** ← REFERENCIA
   - Discusión profunda sobre ética
   - Framework de decisión
   - Recomendaciones

4. **GUIA_RAPIDA_Sesion5.md** ← ESTE DOCUMENTO
   - Resumen ejecutivo
   - Respuestas directas
   - Plantillas

---

## ❓ PREGUNTAS FRECUENTES

**P: ¿Y si la variable tiene >25% faltabilidad?**
R: Eliminar la variable. La información pérdida es demasiada para ser confiable.

**P: ¿Usar media en lugar de mediana?**
R: NO. Media es afectada por outliers extremos en emisiones. Mediana es mejor.

**P: ¿Imputar valores de 0?**
R: NO. Cero no significa "sin contaminación", significa "no medida". Diferencia importante.

**P: ¿Reportar que hay datos imputados?**
R: SÍ, SIEMPRE. Ocultar es poco ético e invalida análisis para auditoría.

**P: ¿Puedo usar machine learning para imputar?**
R: Posible pero complejo. Para este caso, mediana es suficiente y más interpretable.

---

## 📚 REFERENCIAS ADICIONALES

Leer en documento principal:
- ESTUDIO_ANALITICA_DE_DATOS.md (Sesión 3: Preparación de Datos)
- INFORME_EJECUTIVO_Sesion5_Contaminacion.md (Discusión ética completa)

---

**Última actualización**: Mayo 2026  
**Estado**: Listo para presentación grupal  
**Todos los archivos disponibles en**: `C:\Users\joseyapur\OneDrive - Microsoft\Documents\Clawpilot\`
