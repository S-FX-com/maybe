# 🚀 Guía Completa: Despliegue de ObieBooks en Railway

## ✅ **PASO 1: Preparación Inicial**

### 1.1 Verificar el Repositorio
- ✅ Fork completado: `https://github.com/S-FX-com/maybe`
- ✅ Repositorio listo para desplegar

### 1.2 Generar SECRET_KEY_BASE
**Opción A - Usando sitio web:**
1. Ve a: https://allorigins.hexcollab.workers.dev/
2. Usa el generador de claves de Rails online
3. Copia la clave generada (128 caracteres)

**Opción B - Manualmente:**
```bash
# En tu terminal local con Ruby instalado:
ruby -e "require 'securerandom'; puts SecureRandom.hex(64)"
```

**Clave de ejemplo (DEBES generar tu propia clave):**
```
a1b2c3d4e5f6789012345678901234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef12
```

## ✅ **PASO 2: Configuración en Railway**

### 2.1 Crear Proyecto en Railway
1. Ve a [railway.app](https://railway.app)
2. Haz clic en "Login" y conecta con GitHub
3. Haz clic en "New Project"
4. Selecciona "Deploy from GitHub repo"
5. Busca y selecciona tu repositorio: `S-FX-com/maybe`

### 2.2 Configurar Servicios Necesarios
Railway creará automáticamente la aplicación web. Necesitamos agregar:

**A. Agregar PostgreSQL:**
1. En tu proyecto, haz clic en "New Service"
2. Selecciona "Database" 
3. Elige "PostgreSQL"
4. Railway generará automáticamente las variables de conexión

**B. Agregar Redis:**
1. Haz clic en "New Service" otra vez
2. Selecciona "Database"
3. Elige "Redis"
4. Railway generará automáticamente REDIS_URL

## ✅ **PASO 3: Configurar Variables de Entorno**

### 3.1 Variables Obligatorias para el Servicio Web
Ve a tu servicio web en Railway → Settings → Environment Variables y agrega:

```bash
# === VARIABLES OBLIGATORIAS ===
SECRET_KEY_BASE=TU_CLAVE_GENERADA_DE_128_CARACTERES
SELF_HOSTED=true
RAILS_ENV=production

# === CONFIGURACIÓN DE DOMINIO ===
APP_DOMAIN=tu-dominio.railway.app
RAILS_FORCE_SSL=true
RAILS_ASSUME_SSL=true

# === CONFIGURACIÓN DE PUERTO ===
PORT=3000
```

### 3.2 Variables de Base de Datos (Automáticas)
Railway generará automáticamente estas variables cuando agregues PostgreSQL:
- `DATABASE_URL`
- `POSTGRES_PASSWORD`
- `POSTGRES_USER`
- `POSTGRES_DB`
- `POSTGRES_HOST`
- `POSTGRES_PORT`

### 3.3 Variables de Redis (Automáticas)
Railway generará automáticamente:
- `REDIS_URL`

### 3.4 Variables Opcionales
```bash
# === DATOS DE MERCADO (OPCIONAL) ===
SYNTH_API_KEY=obtener_de_synthfinance.com

# === EMAIL (OPCIONAL) ===
SMTP_ADDRESS=smtp.resend.com
SMTP_PORT=465
SMTP_USERNAME=resend
SMTP_PASSWORD=tu_clave_resend
SMTP_TLS_ENABLED=true
EMAIL_SENDER=noreply@tu-dominio.com
```

## ✅ **PASO 4: Configuración del Worker (Sidekiq)**

### 4.1 Crear Servicio Worker
1. En Railway, haz clic en "New Service"
2. Selecciona "GitHub Repo"
3. Selecciona el mismo repositorio: `S-FX-com/maybe`
4. En Settings del worker:
   - **Start Command:** `bundle exec sidekiq`
   - **Build Command:** `bundle install`

### 4.2 Variables de Entorno del Worker
Copia TODAS las mismas variables del servicio web al worker.

## ✅ **PASO 5: Configurar Build Settings**

### 5.1 Servicio Web - Configuración de Build
En tu servicio web → Settings:

**Build Command:**
```bash
bundle install && bundle exec rails assets:precompile
```

**Start Command:**
```bash
bundle exec rails server -p $PORT -e production
```

**Root Directory:** `.` (raíz del proyecto)

### 5.2 Verificar Dockerfile
Railway usará el Dockerfile existente. Verifica que está presente en la raíz.

## ✅ **PASO 6: Desplegar y Verificar**

### 6.1 Inicial Deployment
1. Railway comenzará el despliegue automáticamente
2. Monitorea los logs en tiempo real
3. Espera a que aparezca "Listening on port 3000"

### 6.2 Ejecutar Migraciones (Primera vez)
Una vez que la app esté desplegada, ejecuta las migraciones:

1. Ve a tu servicio web en Railway
2. Haz clic en la pestaña "Deployments"
3. Busca el deployment activo y haz clic en "View Logs"
4. Usa el terminal integrado o corre:

```bash
bundle exec rails db:create db:migrate db:seed
```

### 6.3 Verificar Funcionamiento
1. Ve a la URL de tu aplicación (Railway te la proporcionará)
2. Deberías ver la página de login de ObieBooks
3. Registra tu primera cuenta

## ✅ **PASO 7: Configurar Dominio Personalizado**

### 7.1 Configurar DNS
1. En Railway, ve a tu servicio web → Settings → Domains
2. Haz clic en "Custom Domain"
3. Ingresa tu dominio personalizado
4. Configura los registros DNS según las instrucciones de Railway

### 7.2 Actualizar Variables de Entorno
Una vez configurado el dominio personalizado:
```bash
APP_DOMAIN=tu-dominio-personalizado.com
```

## ✅ **PASO 8: Monitoreo y Mantenimiento**

### 8.1 Monitoreo
- Usa Railway Dashboard para monitorear logs
- Configura alertas si es necesario
- Revisa el uso de recursos regularmente

### 8.2 Actualizaciones
Para actualizar ObieBooks:
1. Haz push a tu repositorio GitHub
2. Railway desplegará automáticamente
3. Las migraciones se ejecutarán automáticamente

## 🛠️ **Solución de Problemas Comunes**

### Error: "ActiveRecord::DatabaseConnectionError"
```bash
# En el terminal de Railway, ejecuta:
bundle exec rails db:create
bundle exec rails db:migrate
```

### Error: "SECRET_KEY_BASE is missing"
- Verifica que la variable SECRET_KEY_BASE esté configurada
- Debe tener 128 caracteres hex

### Error de SSL/HTTPS
```bash
# Asegúrate de tener configurado:
RAILS_FORCE_SSL=true
RAILS_ASSUME_SSL=true
```

### Worker no procesa jobs
- Verifica que el servicio Sidekiq esté corriendo
- Verifica que REDIS_URL esté disponible en el worker

## 📋 **Checklist Final**

- [ ] Fork del repositorio completado
- [ ] Proyecto creado en Railway
- [ ] PostgreSQL agregado y configurado  
- [ ] Redis agregado y configurado
- [ ] Variables de entorno configuradas
- [ ] Servicio web desplegado
- [ ] Worker (Sidekiq) desplegado
- [ ] Migraciones ejecutadas
- [ ] Aplicación accesible
- [ ] Primera cuenta registrada
- [ ] Dominio personalizado configurado (opcional)

¡Listo! Tu aplicación ObieBooks debería estar completamente funcional en Railway. 🎉 