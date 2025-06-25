# 🚀 **DESPLIEGUE COMPLETO DE MAYBE EN RAILWAY**

## 📋 **RESUMEN EJECUTIVO**

✅ **Fork completado**: `https://github.com/S-FX-com/maybe`  
✅ **Archivos de configuración creados**: `railway.json`, `railway.toml`  
✅ **Guías paso a paso preparadas**

---

## 🎯 **PASOS INMEDIATOS A SEGUIR**

### **1. Generar SECRET_KEY_BASE**
Usa uno de estos métodos:

**Opción A - Generador Online:**
- Ve a: https://randomkeygen.com/
- Copia una "CodeIgniter Encryption Key" (256-bit)
- O usa: https://generate.plus/en/hex/secret-key

**Opción B - Comando Ruby:**
```bash
ruby -e "require 'securerandom'; puts SecureRandom.hex(64)"
```

**Ejemplo de SECRET_KEY_BASE (genera la tuya propia):**
```
a1b2c3d4e5f6789012345678901234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef12
```

### **2. Configurar Railway (15 minutos)**

#### **A. Crear Proyecto**
1. Ve a [railway.app](https://railway.app)
2. Login con GitHub
3. "New Project" → "Deploy from GitHub repo"
4. Selecciona: `S-FX-com/maybe`

#### **B. Agregar Servicios**
```
1. PostgreSQL: New Service → Database → PostgreSQL
2. Redis: New Service → Database → Redis  
3. Worker: New Service → GitHub Repo → S-FX-com/maybe
```

#### **C. Variables de Entorno (Web Service)**
```bash
SECRET_KEY_BASE=tu_clave_generada_128_caracteres
SELF_HOSTED=true
RAILS_ENV=production
RAILS_FORCE_SSL=true
RAILS_ASSUME_SSL=true
PORT=3000
```

#### **D. Configurar Worker (Sidekiq)**
```bash
# Start Command:
bundle exec sidekiq

# Build Command:  
bundle install

# Variables: Copiar TODAS del web service
```

#### **E. Build Settings (Web Service)**
```bash
# Build Command:
bundle install && bundle exec rails assets:precompile

# Start Command:
bundle exec rails server -p $PORT -e production
```

### **3. Primera Ejecución**
```bash
# En Railway terminal (después del primer deploy):
bundle exec rails db:create db:migrate db:seed
```

### **4. Verificar Funcionamiento**
- [ ] Web service: ✅ Running
- [ ] Worker service: ✅ Running  
- [ ] Database: ✅ Connected
- [ ] Redis: ✅ Connected
- [ ] App URL: ✅ Accessible
- [ ] Registration: ✅ Working

---

## 🔧 **CONFIGURACIÓN AVANZADA (OPCIONAL)**

### **Variables Adicionales**
```bash
# Datos de mercado (opcional)
SYNTH_API_KEY=obtener_de_synthfinance.com

# Email (opcional)
SMTP_ADDRESS=smtp.resend.com
SMTP_PORT=465
SMTP_USERNAME=resend
SMTP_PASSWORD=tu_clave_resend
SMTP_TLS_ENABLED=true
EMAIL_SENDER=noreply@tu-dominio.com

# Dominio personalizado
APP_DOMAIN=tu-dominio-personalizado.com
```

### **Configurar Dominio Personalizado**
1. Railway → Web Service → Settings → Domains
2. "Custom Domain" → Ingresar dominio
3. Configurar DNS según instrucciones
4. Actualizar `APP_DOMAIN` en variables

---

## 🛠️ **SOLUCIÓN DE PROBLEMAS**

### **Error: Database Connection**
```bash
bundle exec rails db:create
bundle exec rails db:migrate
```

### **Error: SECRET_KEY_BASE missing**
- Verificar que la variable esté configurada
- Debe tener exactamente 128 caracteres hex

### **Error: SSL/HTTPS**
```bash
RAILS_FORCE_SSL=true
RAILS_ASSUME_SSL=true
```

### **Worker no funciona**
- Verificar que Sidekiq esté corriendo
- Verificar REDIS_URL en worker

---

## 📊 **ARQUITECTURA FINAL**

```
┌─────────────────┐    ┌─────────────────┐
│   Web Service   │    │  Worker Service │
│   (Rails App)   │    │    (Sidekiq)    │
└─────────┬───────┘    └─────────┬───────┘
          │                      │
          └──────────┬───────────┘
                     │
         ┌───────────▼───────────┐
         │     PostgreSQL        │
         │      Database         │
         └───────────────────────┘
                     │
         ┌───────────▼───────────┐
         │        Redis          │
         │       Cache           │
         └───────────────────────┘
```

---

## 🎉 **RESULTADO ESPERADO**

Una vez completado, tendrás:

✅ **ObieBooks** completamente funcional  
✅ **Hospedado en Railway** con escalabilidad automática  
✅ **Base de datos PostgreSQL** configurada  
✅ **Redis** para jobs en background  
✅ **Worker Sidekiq** procesando tareas  
✅ **SSL/HTTPS** habilitado  
✅ **Dominio personalizado** (opcional)

**URL de acceso**: `https://tu-proyecto.railway.app`

---

## 📞 **PRÓXIMOS PASOS**

1. **Completar despliegue** siguiendo esta guía
2. **Registrar primera cuenta** en la aplicación
3. **Configurar datos de mercado** (Synth API)
4. **Personalizar dominio** si es necesario
5. **Planificar extensiones** futuras

**¿Listo para comenzar?** 🚀

Sigue los pasos del **PASO 2** y tendrás ObieBooks funcionando en Railway en menos de 20 minutos. 