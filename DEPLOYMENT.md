# Guía de Despliegue

## Despliegue Local

### Requisitos
- Python 3.8+
- Node.js 18+
- 2GB RAM mínimo

### Pasos

1. **Clonar repositorio**
\`\`\`bash
git clone <repository-url>
cd genetic-algorithm-optimizer
\`\`\`

2. **Configurar Backend**
\`\`\`bash
cd backend
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
uvicorn main:app --reload --port 8000
\`\`\`

3. **Configurar Frontend**
\`\`\`bash
# En otra terminal, desde la raíz del proyecto
npm install
npm run dev
\`\`\`

4. **Acceder a la aplicación**
- Frontend: http://localhost:3000
- Backend API: http://localhost:8000
- Documentación API: http://localhost:8000/docs

## Despliegue en Producción

### Backend (Python)

**Opción 1: Vercel (Serverless)**

1. Crear `vercel.json` en la raíz:
\`\`\`json
{
  "builds": [
    {
      "src": "backend/main.py",
      "use": "@vercel/python"
    }
  ],
  "routes": [
    {
      "src": "/api/(.*)",
      "dest": "backend/main.py"
    }
  ]
}
\`\`\`

2. Desplegar:
\`\`\`bash
vercel --prod
\`\`\`

**Opción 2: Railway/Render**

1. Crear `Procfile`:
\`\`\`
web: uvicorn backend.main:app --host 0.0.0.0 --port $PORT
\`\`\`

2. Conectar repositorio y desplegar

### Frontend (Next.js)

**Vercel (Recomendado)**

1. Conectar repositorio de GitHub
2. Configurar variables de entorno:
   - `NEXT_PUBLIC_API_URL`: URL del backend desplegado
3. Desplegar automáticamente

**Alternativa: Netlify**

\`\`\`bash
npm run build
netlify deploy --prod
\`\`\`

## Variables de Entorno en Producción

### Frontend
\`\`\`env
NEXT_PUBLIC_API_URL=https://tu-backend.vercel.app
\`\`\`

### Backend
No requiere variables de entorno adicionales para funcionalidad básica.

## Optimizaciones de Producción

### Backend
- Usar Gunicorn con workers: `gunicorn -w 4 -k uvicorn.workers.UvicornWorker backend.main:app`
- Configurar límites de rate limiting
- Habilitar compresión gzip

### Frontend
- Optimización automática de Next.js
- Imágenes optimizadas
- Code splitting automático

## Monitoreo

### Backend
- Logs: `uvicorn main:app --log-level info`
- Health check: `GET /health`

### Frontend
- Vercel Analytics incluido
- Monitoreo de errores en consola del navegador

## Seguridad

1. **CORS**: Configurar orígenes permitidos en producción
2. **Rate Limiting**: Implementar límites de peticiones
3. **Validación**: Pydantic valida todos los inputs
4. **HTTPS**: Obligatorio en producción

## Troubleshooting

### Error: Backend no responde
- Verificar que el backend esté corriendo
- Revisar logs del servidor
- Confirmar URL correcta en variables de entorno

### Error: CORS
- Actualizar `allow_origins` en `backend/main.py`
- Verificar que la URL del frontend esté permitida

### Error: Timeout en optimización
- Reducir número de generaciones
- Reducir tamaño de población
- Aumentar timeout del servidor
