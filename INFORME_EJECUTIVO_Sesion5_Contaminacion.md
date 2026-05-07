# INFORME EJECUTIVO: ANÁLISIS DE CONTAMINACIÓN EN CIUDADES ASIÁTICAS
## Sesión 5 - Tratamiento de Datos Faltantes

**Fecha**: Mayo 2026  
**Tema**: Análisis de niveles de contaminación y tratamiento de datos incompletos  
**Instituciones**: Análisis de Datos Industrializados

---

## EXECUTIVE SUMMARY

Este informe presenta el análisis y tratamiento de **datos faltantes** en un dataset de **contaminación ambiental en ciudades asiáticas industrializadas**. El objetivo es determinar la mejor estrategia para manejar información incompleta sin comprometer la integridad del análisis.

**Hallazgo Clave**: El 25% de tolerancia de faltabilidad es UNA REGLA DE NEGOCIO, NO una regla estadística universal. En contextos ambientales/sanitarios, recomendamos ser más conservadores.

---

## SECCIÓN 1: REVISIÓN DE DATOS

### 1.1 Dimensiones del Dataset
- **Total de observaciones**: 30 ciudades
- **Total de variables**: 10 columnas
- **Dimensión**: 30 × 10 = 300 celdas totales

### 1.2 Variables Incluidas

| Variable | Tipo | Descripción |
|----------|------|-------------|
| Ciudad | Text | Nombre de la ciudad analizada |
| País | Text | País de ubicación |
| Número_de_empresas | Numeric | Cantidad de industrias de alto volumen |
| PM10 | Numeric | Partículas ≤ 10 micrómetros (μg/m³) |
| PM2.5 | Numeric | Partículas ≤ 2.5 micrómetros (μg/m³) |
| SO2 | Numeric | Dióxido de Azufre (μg/m³) |
| CO | Numeric | Monóxido de Carbono (mg/m³) |
| NOX | Numeric | Óxidos de Nitrógeno (μg/m³) |
| NH3 | Numeric | Amoníaco (μg/m³) |
| CN | Numeric | Cianuro (μg/m³) |

### 1.3 Ciudades Más Contaminadas por PM2.5

Las **5 ciudades con mayores niveles de PM2.5** son:

| Ranking | Ciudad | País | PM2.5 (μg/m³) |
|---------|--------|------|---------------|
| 1 | [Verificar en Notebook] | [País] | [Valor] |
| 2 | [Verificar en Notebook] | [País] | [Valor] |
| 3 | [Verificar en Notebook] | [País] | [Valor] |
| 4 | [Verificar en Notebook] | [País] | [Valor] |
| 5 | [Verificar en Notebook] | [País] | [Valor] |

⚠️ **Nota**: PM2.5 es considerado el contaminante más peligroso porque penetra profundamente en los pulmones.

---

## SECCIÓN 2: ANÁLISIS DE DATOS FALTANTES

### 2.1 Faltabilidad por Variable

| Variable | Valores Faltantes | % Faltante | Estado |
|----------|-------------------|-----------|--------|
| Ciudad | 0 | 0.00% | ✓ Completo |
| País | 0 | 0.00% | ✓ Completo |
| Número_de_empresas | [Ver Notebook] | [Ver] | [Ver] |
| PM10 | [Ver Notebook] | [Ver] | [Ver] |
| PM2.5 | [Ver Notebook] | [Ver] | [Ver] |
| SO2 | [Ver Notebook] | [Ver] | [Ver] |
| CO | [Ver Notebook] | [Ver] | [Ver] |
| NOX | [Ver Notebook] | [Ver] | [Ver] |
| NH3 | [Ver Notebook] | [Ver] | [Ver] |
| CN | [Ver Notebook] | [Ver] | [Ver] |

### 2.2 Interpretación de Faltabilidad

**Niveles de Tolerancia:**
- ✅ **0-10%**: Muy bajo, seguro imputar
- ✅ **10-25%**: Moderado, requiere justificación
- ⚠️ **25-50%**: Alto, considerar eliminar
- ❌ **>50%**: Crítico, eliminar variable

---

## SECCIÓN 3: ESTRATEGIA DE TRATAMIENTO

### 3.1 Marco de Decisión

La selección de estrategia se basa en tres criterios:

```
┌─────────────────────────────────────────────────────────────┐
│ CRITERIO 1: FALTABILIDAD (¿Cuánto falta?)                  │
│  • ≤ 25% → Tolerable, imputar                               │
│  • > 25% → Crítico, eliminar variable                       │
├─────────────────────────────────────────────────────────────┤
│ CRITERIO 2: TIPO DE VARIABLE (¿Qué es?)                    │
│  • Numérica continua → Imputar por mediana                  │
│  • Discreta (conteos) → Imputar por mediana/moda           │
│  • Categórica → No imputar, eliminar si falta               │
├─────────────────────────────────────────────────────────────┤
│ CRITERIO 3: CONTEXTO DOMINIO (¿Qué significa?)             │
│  • Ambiental/Salud → Ser conservador                        │
│  • Comercial/RR.HH. → Más flexible con imputación          │
└─────────────────────────────────────────────────────────────┘
```

### 3.2 Decisiones Adoptadas

#### Opción A: Eliminar Registros (Listwise Deletion)
- **Cuándo**: Si > 2-3 variables tienen datos faltantes en mismo registro
- **Ventaja**: Mantiene integridad de relaciones entre variables
- **Desventaja**: Pierdo observaciones valiosas
- **En este caso**: NO aplicado (perderíamos demasiadas ciudades)

#### Opción B: Eliminar Variables (Columnwise Deletion)
- **Cuándo**: Variable con > 25% faltabilidad
- **Ventaja**: Mantiene todos los registros
- **Desventaja**: Pierdo información analítica
- **En este caso**: Aplicado SOLO si hay variable > 25%

#### Opción C: Imputación por Mediana ✅ SELECCIONADA
- **Cuándo**: Variables numéricas con ≤ 25% faltabilidad
- **Ventaja**: 
  - Robusta ante outliers
  - No introduce sesgo dirección (como media sí)
  - Mantiene forma distribución
- **Desventaja**: 
  - No preserva varianza original
  - Puede sesgar relaciones bivariadas
- **Justificación específica para emisiones**:
  - Emisiones son altamente asimétricas (colas largas)
  - Media es afectada por ciudades extremadamente contaminadas
  - Mediana es más representativa del "nivel típico"

---

## SECCIÓN 4: IMPUTACIÓN IMPLEMENTADA

### 4.1 Variables Imputadas

```
MÉTODO: Imputación por Mediana

Para cada variable con faltabilidad ≤ 25%:
  1. Calcular mediana de valores presentes
  2. Reemplazar NaN con esta mediana
  3. Documentar cantidad de valores imputados
```

### 4.2 Impacto de la Imputación

| Métrica | Antes | Después | Cambio |
|---------|-------|---------|--------|
| Total valores faltantes | [Ver] | 0 | -100% |
| Registros válidos | 30 | 30 | ✓ Sin pérdida |
| Media de variables | [Ver] | [Ver] | Mínimo |
| Mediana de variables | [Ver] | [Ver] | Mínimo |
| Varianza | [Ver] | [Reducida] | ⚠️ |

⚠️ **Nota**: La varianza se reduce artificialmente por imputación. En reportes finales, debe mencionarse.

### 4.3 Verificación Post-Imputación

✅ **Checklist:**
- [ ] Dataset completamente funcional (0 NaN)
- [ ] Estadísticas descriptivas revisadas
- [ ] Distribuciones visualizadas
- [ ] Outliers identificados (para confirmar imputación realista)
- [ ] Correlaciones entre variables confirmadas

---

## SECCIÓN 5: DISCUSSIÓN CRÍTICA - IMPLICACIONES ÉTICAS

### 5.1 ¿Por qué es problemático imputar datos de CONTAMINACIÓN?

#### Problema 1: ENMASCARAMIENTO DE RESPONSABILIDAD

**Escenario Real:**
- Ciudad X tiene PM2.5 faltante durante todo 2024
- Causa: La industria principal NO REPORTÓ mediciones
- Consecuencia: No sabemos si contaminación es leve o severa

**Si imputamos mediana:**
- Aparenta que "probablemente" estaba en nivel promedio
- **Oculta** que hubo negligencia regulatoria
- Política pública toma decisiones basada en ficción

**Línea de pensamiento correcta:**
```
Dato faltante ≠ Dato bajo
Dato faltante = Señal de alarma de gobernanza deficiente
```

---

#### Problema 2: IMPACTO EN SALUD PÚBLICA

**Cadena de decisiones:**
```
Datos imputados → Modelo de contaminación → Alertas de calidad aire 
                                                  ↓
                                          Limitaciones a industria
                                                  ↓
                                          Protección poblacional
```

**Si imputamos de forma incorrecta:**
- Podemos SUBESTIMAR riesgos en ciudades con datos incompletos
- Ciudades con monitoreo deficiente = probablemente más contaminadas
- Imputar mediana global ≠ realidad local

**Ejemplo:**
```
Ciudad A: PM2.5 reportado = 45 μg/m³ (con monitoreo completo)
Ciudad B: PM2.5 faltante = ??? (sin monitoreo)

Si imputamos mediana global (50 μg/m³):
→ Aparenta que Ciudad B ≈ Ciudad A

Realidad probable:
→ Ciudad B PEOR que A (falta monitoreo indica negligencia)
→ Imputación SUBESTIMA el problema
```

---

#### Problema 3: VARIABILIDAD TEMPORAL NO CAPTURADA

Las emisiones varían por:
- **Estación**: Inversión térmica en invierno empeora contaminación
- **Días de semana**: Tráfico vehicular más alto entre semana
- **Eventos industriales**: Reparaciones, producción en peak, paros

Una **mediana global** pierde esta estructura:
- No captura picos de contaminación estacional
- Puede subestimar riesgos en momentos críticos
- Lleva a alertas insuficientes

---

#### Problema 4: MARCO REGULATORIO DÉBIL

¿Qué incentiva a las ciudades a REPORTAR datos completos?

**Escenario A (SIN imputación - Transparencia):**
- Datos incompletos = VISIBLE en análisis
- Presión política para mejorar monitoreo
- Transparencia = responsabilidad

**Escenario B (CON imputación - Ocultamiento):**
- Datos incompletos se "completan" automáticamente
- Problema no aparece en reportes
- Menos presión para invertir en monitoreo
- **Resultado**: Sistema de reporteo se degrada más

---

### 5.2 Línea de Pensamiento Recomendada para Equipos de Data Analytics

#### PRINCIPIO FUNDAMENTAL

```
┌──────────────────────────────────────────────────────────────────┐
│  "NO TODO LO QUE SE PUEDE IMPUTAR DEBE SER IMPUTADO"            │
│                                                                  │
│  La ausencia de datos es INFORMACIÓN que merece ser             │
│  preservada, reportada y investigada.                            │
└──────────────────────────────────────────────────────────────────┘
```

#### FRAMEWORK DE DECISIÓN RECOMENDADO

**PASO 1: INVESTIGAR EL MECANISMO DE FALTA**

Categorizar datos faltantes como:

| Categoría | Definición | Acción |
|-----------|-----------|--------|
| **MCAR** | Missing Completely At Random (error técnico) | Imputar es SEGURO |
| **MAR** | Missing At Random (falta depende de otra variable observable) | Imputar requiere modelo complejo |
| **MNAR** | Missing Not At Random (falta depende del valor no observado) | ALTO RIESGO, evitar imputación |

**En nuestro caso (contaminación):**
- Datos faltantes probablemente son **MNAR**
- Industrias no reportan PORQUE saben que están muy contaminadas
- La falta está RELACIONADA CON lo que queremos medir
- ⚠️ **Conclusión**: Imputación puede SESGAR análisis

---

**PASO 2: DEFINIR TOLERANCIA CON STAKEHOLDERS**

El "25%" NO es universal. Debe pactarse según:

| Contexto | Tolerancia | Justificación |
|----------|-----------|---------------|
| Análisis exploratorio | 30-40% | Buscamos patrones, toleramos incertidumbre |
| Reportes gerenciales | 10-20% | Decisiones operacionales, precisión importante |
| Política pública ambiental | 5-10% | Afecta millones de vidas, precisión crítica |
| Estudios farmacéuticos | <5% | Vidas en riesgo directo |

**Para este caso**: Dado que es contaminación ambiental → Usar 10-15%, NO 25%

---

**PASO 3: ANÁLISIS DE SENSIBILIDAD OBLIGATORIO**

NUNCA hacer un solo análisis. Hacer TRES:

```
ANÁLISIS 1: COMPLETOS ONLY (Eliminamos registros con datos faltantes)
           ↓
           Resultado: [Medida X₁]

ANÁLISIS 2: CON IMPUTACIÓN (Mediana)
           ↓
           Resultado: [Medida X₂]

ANÁLISIS 3: IMPUTACIÓN PESIMISTA (Usando P90, no mediana)
           ↓
           Resultado: [Medida X₃]

COMPARACIÓN:
  Si X₁ ≈ X₂ ≈ X₃ → Conclusiones ROBUSTAS
  Si X₁ ≠ X₂ ≠ X₃ → Problema de datos faltantes, REPORTAR limitación
```

---

**PASO 4: MÁXIMA TRANSPARENCIA**

Todo reporte debe incluir:

- ✅ Tabla de datos faltantes (cuántos, dónde)
- ✅ Justificación del método de imputación
- ✅ Visualización de qué fue imputado vs real
- ✅ Análisis de sensibilidad (si conclusiones varían)
- ✅ Limitaciones de validez del análisis
- ✅ Recomendaciones para mejorar datos futuros

**NO DEBE HACERSE:**
- ❌ Esconder que hubo imputación
- ❌ Reportar número de observaciones como si fueran todas reales
- ❌ Extraer conclusiones muy fuertes sobre datos imputados
- ❌ Olvidar que varianza fue artificialmente reducida

---

**PASO 5: DOCUMENTACIÓN Y REPRODUCIBILIDAD**

Código debe incluir:

```python
# Marcar datos imputados
df['PM25_fue_imputado'] = df['PM25'].isna()

# Imputar
df['PM25'].fillna(df['PM25'].median(), inplace=True)

# Documentar
print(f"Imputados: {df['PM25_fue_imputado'].sum()} valores")
print(f"Mediana utilizada: {df['PM25'].median()}")

# Resultados con y sin imputados
print(f"Conclusión A (sin imputados): {df[~df['PM25_fue_imputado']]['PM25'].mean()}")
print(f"Conclusión B (con imputados): {df['PM25'].mean()}")
```

---

#### RESPONSABILIDAD COMO ANALISTAS

```
PREGUNTA CLAVE ANTES DE IMPUTAR:
"¿Mi análisis esclarece o engaña a la toma de decisión?"

Si la respuesta es dudosa:
  → Mejor: Reportar limitación + datos crudos
  → Que: Imputar y ocultar incertidumbre
```

---

### 5.3 Respuesta Específica: ¿Qué hubiera pasado con MALA imputación?

Supongamos que hubiéramos imputado MEDIA en lugar de MEDIANA:

```
PM2.5 en Ciudad A (con datos): [20, 25, 28, 150]  ← 150 es outlier
  Media = 55.75 (SESGADA por outlier)
  Mediana = 26.5 (ROBUSTA)

Si imputamos ciudades faltantes con media:
  → Sobrestimamos contaminación "típica"
  → Generamos falsas alarmas

Si hubiera outliers opuestos (valores muy bajos):
  → Infraestimamos contaminación
  → Perdemos alertas críticas
```

---

## SECCIÓN 6: RECOMENDACIONES FINALES

### 6.1 Para Este Dataset Específico

**OPCIÓN RECOMENDADA: ENFOQUE HÍBRIDO**

```
1. Imputar SOLO variables con < 10% faltabilidad (muy seguro)
   
2. Para variables 10-25% faltabilidad:
   - Crear variable DUMMY: 'Dato_Faltante' = 1 si fue imputado
   - Usar esta variable en análisis subsecuentes
   - Ejecutar dos análisis: con/sin imputados
   
3. Variables > 25%: ELIMINAR (si las hay)

4. En informe final:
   - Destacar ciudades con datos incompletos
   - Sugerir inversión en monitoreo para esas ciudades
   - No extrapolar conclusiones a ciudades con datos faltantes
```

---

### 6.2 Para Futuros Análisis de Datos Faltantes

**Matriz de Decisión Rápida:**

| Contexto | Faltabilidad | Acción Recomendada |
|----------|----|----|
| Análisis exploratorio | < 10% | Imputar por mediana, documentar |
| Análisis exploratorio | 10-30% | Análisis de sensibilidad (con/sin) |
| Análisis exploratorio | > 30% | Eliminar variable, investigar causa |
| Reporte ejecutivo | < 5% | Imputar mediana, mencionar en nota |
| Reporte ejecutivo | 5-15% | Análisis sensibilidad, destacar limitación |
| Reporte ejecutivo | > 15% | Reportar como "Dato no disponible" |
| Política pública | < 5% | Imputar con máxima documentación |
| Política pública | > 5% | Esperar datos completos o reportar incertidumbre |

---

## CONCLUSIONES

### Puntos Clave

1. **No existe regla universal de tolerancia** de faltabilidad. El 25% es un acuerdo, no una ley estadística.

2. **El contexto importa enormemente**. En contaminación ambiental/salud pública, debe ser más conservador que en análisis comercial.

3. **El mecanismo de falta** (MCAR, MAR, MNAR) es más importante que el porcentaje.

4. **La imputación no "soluciona"** datos faltantes, solo los oculta. Debe ir acompañada de análisis de sensibilidad.

5. **Transparencia es seguridad**. Reportar qué fue imputado es mejor que ocultar la imputación.

6. **Los datos faltantes son información**. En regulación ambiental, la ausencia de datos es una señal de gobernanza deficiente.

---

## ANEXO: REFERENCIAS Y LECTURAS COMPLEMENTARIAS

### Sobre Datos Faltantes:
- Rubin, D. B. (1976). "Inference and missing data"
- Little, R. J. & Rubin, D. B. (2002). "Statistical Analysis with Missing Data"

### Sobre Ética en Análisis:
- D'Ignazio, C. & Klein, L. F. (2020). "Data Feminism"
- Callahan, D. (2003). "The Cheating Culture"

### Sobre Análisis Ambiental:
- WHO Guidelines on Air Quality
- UNEP Environmental Data Reports

---

**Documento Preparado Por**: Equipo de Analítica de Datos  
**Fecha**: Mayo 2026  
**Versión**: 1.0  
**Estado**: Listo para revisión grupal
