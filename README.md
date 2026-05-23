# 🤖 Proyecto Final — MARTÍN



### Sistema Distribuido de Monitoreo, Persistencia y Telemetría para el Robot MARTÍN

</div>

---

# 📖 Descripción General

Este repositorio contiene el código fuente, configuraciones de infraestructura y documentación técnica correspondientes al **Proyecto Final de Sistemas Operativos II**.

La solución implementa un ecosistema distribuido y desacoplado desplegado completamente en la nube, encargado de:

- Recepción de métricas de telemetría.
- Validación de eventos JSON.
- Persistencia de logs en tiempo real.
- Visualización web de eventos del robot.
- Tolerancia a fallos mediante contenedores Docker.

El sistema actúa como nodo receptor y procesador de información generada de forma asíncrona por el prototipo robótico de laboratorio:

# 🤖 MARTÍN

---

# 🌐 Infraestructura de Producción

La plataforma se encuentra completamente desplegada y operativa en producción utilizando infraestructura cloud sobre DigitalOcean.

## 🔗 Endpoints Públicos

| Servicio | Endpoint |
|---|---|
| 🌍 Dashboard Web | `http://161.35.112.5` |
| ⚡ REST API Backend | `http://161.35.112.5:3000` |

---

# 🏗️ Arquitectura General del Sistema

La solución rompe con el modelo monolítico tradicional mediante una arquitectura desacoplada basada en microservicios.

## 📌 Componentes Principales

```text
                    ┌────────────────────┐
                    │    Robot MARTÍN    │
                    │  Generación Logs   │
                    └─────────┬──────────┘
                              │ JSON
                              ▼
                  ┌────────────────────────┐
                  │     FastAPI Backend    │
                  │   Validación + API     │
                  │      Puerto 3000       │
                  └─────────┬──────────────┘
                            │
                            ▼
                  ┌────────────────────────┐
                  │       MongoDB          │
                  │ Persistencia BSON/JSON │
                  └─────────┬──────────────┘
                            │
                            ▼
                  ┌────────────────────────┐
                  │     Frontend Nginx     │
                  │ Dashboard Web HTML/CSS │
                  │       Puerto 80        │
                  └────────────────────────┘
```

---

# 🔐 Mecanismos de Seguridad y Resiliencia

## 🌉 Red Virtual Privada Docker (`red_martin`)

El sistema utiliza una red aislada tipo `bridge` creada dentro del motor Docker.

### Características:

- El puerto de MongoDB (`27017`) NO está expuesto públicamente.
- La comunicación interna ocurre únicamente mediante DNS interno Docker.
- El Backend se comunica con MongoDB utilizando:

```text
mongodb://lab_mongodb:27017
```

### Beneficios:

- Aislamiento de servicios.
- Reducción de superficie de ataque.
- Protección contra accesos externos no autorizados.

---

## ♻️ Recuperación Automática de Servicios

Todos los contenedores utilizan la política:

```bash
--restart unless-stopped
```

Esto permite que:

- Los servicios se reinicien automáticamente.
- El sistema recupere disponibilidad tras reinicios del host.
- La infraestructura mantenga alta tolerancia a fallos.

---

# 🛠️ Ecosistema Tecnológico

# ☁️ Infraestructura Cloud

| Tecnología | Descripción |
|---|---|
| DigitalOcean | VPS Cloud Provider |
| Ubuntu Server 24.04.4 LTS | Sistema Operativo Host |
| Linux Kernel 6.8 | Núcleo del Sistema |
| Docker Engine | Virtualización OS-Level |

---

# 💻 Stack de Software

## 🌐 Frontend

- Nginx Server
- HTML5
- CSS3 Responsivo
- JavaScript Vanilla
- Fetch API

### Función:

- Visualización de eventos.
- Dashboard operativo.
- Comunicación asíncrona con Backend.

---

## ⚡ Backend

- Python 3.10
- FastAPI Framework
- Pydantic Validation
- Middleware CORS

### Función:

- Recepción de telemetría.
- Validación JSON.
- Gestión de solicitudes HTTP.
- Persistencia de eventos.

---

## 🍃 Base de Datos

- MongoDB NoSQL
- Documentos BSON
- Volumen persistente Docker

### Volumen Persistente:

```text
proyecto_martin_mongo_data
```

---

# 🚀 Protocolo de Uso y Demostración

---

# 🌐 1. Acceso Remoto al Servidor Linux

## 🔑 Conexión SSH

Desde la terminal local:

```bash
ssh martin@161.35.112.5
```

---

# 🔐 2. Autenticación

El sistema solicitará la contraseña:

```bash
martin@161.35.112.5's password:
```

> ⚠️ **Importante:**  
> Linux NO mostrará caracteres mientras escribe la contraseña.  
> Esto es completamente normal y forma parte del mecanismo de seguridad del sistema operativo.

---

# 🐳 3. Verificación de Contenedores Activos

Ejecute:

```bash
sudo docker ps
```

## ✅ Resultado Esperado

| CONTAINER ID | IMAGE | STATUS | NAMES |
|---|---|---|---|
| xxx | frontend | Up | lab_frontend |
| xxx | backend | Up | lab_backend |
| xxx | mongodb | Up | lab_mongodb |

---

# 🧪 Simulación de Tolerancia a Fallos

---

# 🟢 Escenario A — Detener Backend

## 🔻 Comando

```bash
sudo docker stop lab_backend
```

---

## ⚙️ Resultado Esperado

- El Frontend sigue operativo.
- El Dashboard permanece accesible.
- Se despliega el mensaje:

```text
⚠️ Error al conectar con el Backend de Logs
```

---

## 🔄 Restaurar Servicio

```bash
sudo docker start lab_backend
```

---

# 🔵 Escenario B — Detener MongoDB

## 🔻 Comando

```bash
sudo docker stop lab_mongodb
```

---

## ⚙️ Resultado Esperado

- FastAPI continúa activo.
- El Backend recibe eventos JSON.
- La persistencia falla temporalmente.

### Código HTTP Esperado

```http
HTTP 500 - Internal Server Error
```

---

## 🔄 Restaurar Servicio

```bash
sudo docker start lab_mongodb
```

---

# 🔴 Escenario C — Detener Frontend

## 🔻 Comando

```bash
sudo docker stop lab_frontend
```

---

## ⚙️ Resultado Esperado

La URL:

```text
http://161.35.112.5
```

dejará de responder.

Sin embargo:

- Backend continúa operativo.
- MongoDB continúa activo.
- El robot sigue enviando eventos.

---

## 📜 Monitor de Logs en Tiempo Real

```bash
sudo docker logs -f lab_backend
```

### Resultado Esperado

```text
HTTP 200 OK
```

> 📌 Presione `CTRL + C` para salir.

---

## 🔄 Restaurar Servicio

```bash
sudo docker start lab_frontend
```

---

# 📂 Estructura General del Proyecto

```text
Proyecto-Martin/
│
├── backend/
│   ├── app/
│   ├── requirements.txt
│   └── Dockerfile
│
├── frontend/
│   ├── index.html
│   ├── style.css
│   ├── script.js
│   └── Dockerfile
│
├── mongodb/
│
├── docker-compose.yml
│
└── README.md
```
---
Desarrollado como solución académica para la implementación de una arquitectura distribuida tolerante a fallos aplicada al robot de laboratorio:

# 🤖 MARTÍN

