[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/karinazulejo/prueba-tecnica-mlops-ia-cvs/blob/main/Pipeline_CV_NLP_LLM_Karen_Tech_Lead.ipynb)

#  Prueba Técnica – Pipeline de Análisis de Hojas de Vida (LLM + MLOps)

Este repositorio contiene un pipeline **modular, trazable y reproducible** para procesar y analizar hojas de vida mediante técnicas de procesamiento determinístico y modelos LLM, siguiendo buenas prácticas de arquitectura y MLOps.

---

##  Diagrama del Proceso

> Cuando subas tu imagen al repositorio con el nombre `diagrama_pipeline.png`, aparecerá aquí:

![Diagrama del Pipeline](./diagrama_pipeline.png)

---

##  Arquitectura General

El pipeline está organizado en **4 capas principales**, siguiendo un diseño claro, mantenible y extensible.

---

### **1. Capa de Ingesta**

- Carga del archivo `.zip` con CVs.  
- Conversión automática de **PDFs a texto** utilizando `pdfplumber`.  
- Validación y manejo básico de errores en archivos.  
- **Salida:** carpeta `CVs_txt/`.

---

### **2. Capa de Parsing Determinístico (Regex)**

Extracción explícita mediante reglas definidas para:

- Email  
- Teléfono  
- Años de experiencia profesional  

Estas reglas permiten:

- Reducir el número de consultas al LLM.  
- Aumentar precisión en señales estructuradas.  
- Mantener interpretabilidad y control del pipeline.

---

### **3. Capa Semántica (LLM – Groq, modelo llama-3.1-8b-instant)**

El LLM se utiliza exclusivamente para información que **no puede obtenerse mediante reglas determinísticas**.

Se extrae:

- Nombre del candidato  
- Formación en IA  
- Score del CV (0–1)  
- Nivel de confianza del modelo  

Incluye buenas prácticas:

- Prompt robusto  
- Respuesta en **JSON estricto**  
- Manejo de errores y fallback automático  
- **Temperatura baja** para reproducibilidad

---

### **4. Capa de Negocio**

Se genera un diccionario por CV con:

- Información extraída  
- Métricas de completitud  
- Penalización automática si faltan datos críticos  

Incluye metadata para trazabilidad:

- Modelo utilizado  
- Versión del pipeline  
- Fecha de ejecución  
- Nombre del archivo original

---

##  Salida

El pipeline genera el archivo:

📄 [Ver archivo de resultados](./resultados_cvs.json)


```
/content/resultados_cvs.json
```

Este archivo contiene un objeto por CV con:

- Datos normalizados  
- Puntuaciones  
- Señales determinísticas y semánticas  
- Trazabilidad completa del proceso  
