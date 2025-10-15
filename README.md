# Sistema Inteligente de Optimización con Algoritmos Genéticos

Sistema full-stack para maximizar la rentabilidad de electrodomésticos dentro de un área de 50 m² utilizando Algoritmos Genéticos.

## Características

- **Backend Python (FastAPI)**: API REST que ejecuta el algoritmo genético
- **Frontend Next.js**: Interfaz interactiva para configuración y visualización
- **Visualización 2D**: Canvas interactivo mostrando distribución espacial
- **Gráfica de Convergencia**: Evolución del fitness a través de generaciones
- **Configuración Flexible**: Control total sobre parámetros del GA

## Estructura del Proyecto

\`\`\`
.
├── backend/
│   ├── main.py              # API FastAPI con algoritmo genético
│   ├── requirements.txt     # Dependencias Python
│   └── README.md           # Documentación del backend
├── app/
│   ├── page.tsx            # Página principal
│   ├── layout.tsx          # Layout de la aplicación
│   └── globals.css         # Estilos globales
├── components/
│   ├── article-selector.tsx       # Selector de artículos
│   ├── parameter-controls.tsx     # Controles de parámetros GA
│   ├── results-display.tsx        # Visualización de resultados
│   ├── spatial-distribution.tsx   # Canvas 2D
│   └── convergence-chart.tsx      # Gráfica de convergencia
├── lib/
│   └── api.ts              # Cliente API
└── README.md               # Este archivo
\`\`\`

## Requisitos Previos

- **Python 3.8+**
- **Node.js 18+**
- **npm o yarn**

## Instalación y Ejecución

### 1. Backend (Python/FastAPI)

\`\`\`bash
# Navegar a la carpeta backend
cd backend

# Crear entorno virtual
python -m venv venv

# Activar entorno virtual
# En Windows:
venv\Scripts\activate
# En macOS/Linux:
source venv/bin/activate

# Instalar dependencias
pip install -r requirements.txt

# Ejecutar servidor
uvicorn main:app --reload --port 8000
\`\`\`

El backend estará disponible en `http://localhost:8000`

**Documentación API**: `http://localhost:8000/docs`

### 2. Frontend (Next.js)

\`\`\`bash
# En la raíz del proyecto (carpeta principal)
npm install

# Ejecutar en modo desarrollo
npm run dev
\`\`\`

El frontend estará disponible en `http://localhost:3000`

## Uso del Sistema

### 1. Selección de Artículos

- Marca/desmarca artículos del catálogo usando los checkboxes
- Modifica área, beneficio y stock de cada artículo
- Solo los artículos seleccionados serán considerados en la optimización

### 2. Configuración de Parámetros

**Parámetros del Algoritmo Genético:**

- **Área Máxima**: Restricción de espacio total (default: 50 m²)
- **Tamaño de Población**: Número de individuos por generación (default: 200)
- **Número de Generaciones**: Iteraciones del algoritmo (default: 200)
- **Probabilidad de Cruce (Pc)**: Tasa de crossover (default: 0.6)
- **Probabilidad de Mutación (Pm)**: Tasa de mutación (default: 0.15)
- **Tipo de Selección**: Torneo o Ruleta
- **Tamaño del Torneo**: Solo para selección por torneo (default: 3)
- **Elitismo**: Número de mejores individuos preservados (default: 2)
- **Umbral de Eficiencia**: Mínimo beneficio/área aceptable (default: 100)

### 3. Ejecución y Resultados

1. Haz clic en "Ejecutar Optimización"
2. El sistema mostrará:
   - **Métricas principales**: Beneficio total, área usada, utilización
   - **Distribución 2D**: Visualización espacial de artículos
   - **Convergencia**: Gráfica de evolución del fitness
   - **Detalles**: Lista completa de artículos seleccionados

## API Endpoints

### `POST /optimize`

Ejecuta el algoritmo genético con los parámetros proporcionados.

**Request Body:**
\`\`\`json
{
  "articles": [
    {
      "id": 1,
      "nombre": "Mini nevera",
      "area": 0.25,
      "beneficio": 40,
      "stock": 5,
      "selected": true
    }
  ],
  "area_maxima": 50.0,
  "tam_poblacion": 200,
  "num_generaciones": 200,
  "prob_cruce": 0.6,
  "prob_mutacion": 0.15,
  "tipo_seleccion": "torneo",
  "tam_torneo": 3,
  "elitismo": 2,
  "umbral_eficiencia": 100.0
}
\`\`\`

**Response:**
\`\`\`json
{
  "mejor_solucion": { "1": 5, "2": 3 },
  "mejor_beneficio": 380.5,
  "area_usada": 48.2,
  "utilizacion_porcentaje": 96.4,
  "historial_fitness": [250.0, 280.5, 380.5],
  "distribucion_2d": [...],
  "articulos_seleccionados": [...]
}
\`\`\`

### `GET /health`

Verifica el estado del servidor.

## Algoritmo Genético

### Representación

Cada individuo es un vector de enteros donde cada posición representa la cantidad de un artículo específico.

### Función de Aptitud

\`\`\`python
fitness = beneficio_total - penalizacion_area - penalizacion_eficiencia
\`\`\`

**Penalizaciones:**
- Exceso de área: `1000 * (area - area_maxima)`
- Baja eficiencia: `(umbral - eficiencia) * cantidad * 0.3`

### Operadores Genéticos

1. **Selección**: Torneo o Ruleta
2. **Cruce**: Uniforme con probabilidad Pc
3. **Mutación**: Incremento/decremento/reinicio aleatorio con probabilidad Pm
4. **Elitismo**: Preserva los mejores individuos

## Tecnologías Utilizadas

### Backend
- **FastAPI**: Framework web moderno y rápido
- **Pydantic**: Validación de datos
- **Python**: Implementación del algoritmo genético

### Frontend
- **Next.js 15**: Framework React con App Router
- **TypeScript**: Tipado estático
- **Tailwind CSS v4**: Estilos utilitarios
- **shadcn/ui**: Componentes UI
- **Recharts**: Visualización de datos
- **Canvas API**: Distribución espacial 2D

## Solución de Problemas

### Backend no se conecta

1. Verifica que el backend esté corriendo: `http://localhost:8000/health`
2. Revisa que el puerto 8000 esté disponible
3. Confirma que las dependencias estén instaladas correctamente

### Error de CORS

El backend está configurado para aceptar peticiones desde cualquier origen. Si persiste el error, verifica la configuración de CORS en `backend/main.py`.

### Visualización no aparece

1. Asegúrate de que la optimización se completó exitosamente
2. Revisa la consola del navegador para errores
3. Verifica que los datos de respuesta sean válidos

## Desarrollo

### Agregar Nuevos Artículos

Modifica el array `DEFAULT_CATALOG` en `app/page.tsx`:

\`\`\`typescript
const DEFAULT_CATALOG = [
  { id: 13, nombre: "Nuevo Artículo", area: 0.5, beneficio: 100, stock: 5, selected: true },
  // ...
]
\`\`\`

### Modificar Parámetros por Defecto

Edita el objeto `parameters` en `app/page.tsx`:

\`\`\`typescript
const [parameters, setParameters] = useState({
  area_maxima: 50.0,
  // ... otros parámetros
})
\`\`\`

## Autor

Elaborado por John A para Machine Learning - Algoritmos Genéticos
Universidad de Cundinamarca
Ingeniería de Sistemas y Computación

## Licencia

Este proyecto es para uso académico.
