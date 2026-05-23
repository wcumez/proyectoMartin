Markdown# Sistema de Monitoreo y Log de Eventos - Proyecto MARTÍN

Este repositorio contiene el código fuente y la arquitectura de despliegue para el **Proyecto Final de Sistemas Operativos II** de la **Universidad Mariano Gálvez de Guatemala (Centro Universitario de Chimaltenango)**. El sistema es un entorno distribuido de microservicios contenerizados en la nube que actúa como receptor centralizado de telemetría y logs para el proyecto de robótica (Asistente de Laboratorio MARTÍN).

---

## 🌐 Dirección IP Pública del Servidor

Los servicios se encuentran totalmente desplegados y disponibles en producción en la siguiente dirección:
* **Dashboard Web (Frontend):** `http://161.35.112.5`
* **API Endpoint (Backend):** `http://161.35.112.5:3000`

*Nota: Con base en los requerimientos, el acceso se realiza directamente a través de la dirección IP pública del servidor sin intermediación de dominios personalizados.*

---

## 📐 Diseño de la Arquitectura del Sistema

El sistema implementa una arquitectura desacoplada de tres capas de software autónomas e indivisibles, ejecutándose dentro del mismo servidor Linux mediante contenedores aislados.

### Diagrama Lógico de Interconexión
[ Robot Martín / Scripts de Prueba ]│▼ (Petición HTTP POST / Puerto 3000)┌──────────────────┐│  lab_backend     │ ◄───► [ Red Interna Virtual: red_martin ]│    (FastAPI)     │                   │└──────────────────┘                   ▼ (Puerto Interno 27017)▲                     ┌──────────────┐│ (Fetch asíncrono)   │ lab_mongodb  ││                     │  (MongoDB)   │┌──────────────────┐            └──────────────┘│  lab_frontend    ││    (Nginx)       │└──────────────────┘▲│ (Acceso HTTP / Puerto 80)[ Navegador Web ]
### Componentes de la Red e Infraestructura
1. **Red Aislada (`red_martin`):** Un conmutador virtual privado (Bridge) en el motor de Docker que interconecta los contenedores. Aísla completamente la base de datos MongoDB del tráfico de Internet, permitiendo la comunicación exclusiva con el Backend a través del DNS interno por nombre de contenedor (`mongodb://lab_mongodb:27017`).
2. **Políticas de Resiliencia:** Todos los contenedores cuentan con la directiva operativa `--restart unless-stopped`. Esto delega en el motor nativo de Docker la auto-orquestación y arranque inmediato de los microservicios si el servidor en la nube sufre un reinicio eléctrico o de mantenimiento, garantizando la tolerancia a fallos.

---

## 🛠️ Tecnologías Utilizadas

### Infraestructura Cloud y Sistemas Operativos
* **Proveedor Cloud:** DigitalOcean (Virtual Private Server - Droplet)
* **Sistema Operativo Host:** Ubuntu Server 24.04.4 LTS (Noble Numbat)
* **Núcleo del Sistema:** Linux Kernel 6.8.0-31-generic x86_64
* **Motor de Virtualización:** Docker Engine (Nativo) v26+

### Stack de Software del Ecosistema
* **Frontend (Capa de Presentación):** Nginx Alpine Image (Servidor HTTP ligero de alta concurrencia) con HTML5 semántico, CSS3 y JavaScript asíncrono (Fetch API).
* **Backend (Capa de Lógica/API):** Python 3.10 + FastAPI + Pydantic (Validación de modelos de datos elásticos y control de middleware de CORS).
* **Base de Datos (Capa de Persistencia):** MongoDB (Motor NoSQL orientado a documentos JSON/BSON con volumen persistente en disco del Host).

---

## 🚀 Instrucciones de Uso y Runbook Operativo

### 1. Acceso Remoto al Servidor (SSH)
Para tareas de mantenimiento y auditoría por parte de los integrantes del grupo desde PowerShell o CMD, conectarse mediante:
```bash
ssh martin@161.35.112.5
2. Inicialización Manual del EcosistemaSi requiere levantar la arquitectura completa desde cero (Red, Volúmenes y Contenedores), ejecute en orden jerárquico dentro del servidor:Bash# Crear red lógica aislada
sudo docker network create red_martin 2>/dev/null

# Levantar Capa de Persistencia (Base de Datos)
sudo docker run -d --name lab_mongodb --network red_martin -v proyecto_martin_mongo_data:/data/db mongo:latest

# Levantar Capa de Lógica (Backend API)
sudo docker run -d --name lab_backend --network red_martin -p 3000:3000 -e MONGO_URI=mongodb://lab_mongodb:27017/logs_robotica martin_backend

# Levantar Capa de Presentación (Frontend Web)
sudo docker run -d --name lab_frontend --network red_martin -p 80:80 martin_frontend
3. Comandos de Monitoreo y Diagnóstico en ProducciónInstrucciones críticas para validar el estado de la entrega frente al docente evaluador:Bash# Revisar el estado actual y puertos de los contenedores
sudo docker ps

# Monitorear consumo de hardware (CPU, RAM y Red) en tiempo real
sudo docker stats

# Seguir el flujo de logs de peticiones entrantes del robot al backend
sudo docker logs -f lab_backend
4. Protocolo de Gestión de Ciclo de Vida (Comandos Rápidos)Bash# Detener temporalmente toda la infraestructura
sudo docker stop lab_frontend lab_backend lab_mongodb

# Volver a encender los contenedores existentes en el servidor
sudo docker start lab_mongodb lab_backend lab_frontend

# Reiniciar la API en caliente
sudo docker restart lab_backend
📊 Matriz de Simulación de Escenarios de Tolerancia a FallosDurante la demostración presencial, se validará la robustez de la arquitectura mediante las siguientes instrucciones operativas:Escenario de Prueba (Rúbrica)Comando de ConsolaComportamiento Esperado del SistemaPausar únicamente el Backendsudo docker stop lab_backendEl frontend web (puerto 80) carga de forma exitosa, pero la interfaz despliega automáticamente un banner de alerta rojo: "Error al conectar con el Backend". No se registran nuevos eventos.Pausar únicamente la Base de Datossudo docker stop lab_mongodbEl backend (puerto 3000) recibe los JSON de telemetría del robot, pero al no poder persistirlos en disco, la API captura el error mediante bloques try/except y responde un código de Estado HTTP 500.Pausar el Frontend Websudo docker stop lab_frontendLa URL web deja de estar disponible en el navegador (Timeout). Sin embargo, el canal de la API (puerto 3000) y MongoDB continúan activos en segundo plano procesando y almacenando eventos del robot con normalidad (Estado HTTP 200).Normalización del Ecosistemasudo docker start [contenedor]El servicio afectado recobra instantáneamente la sincronía operativa y el Dashboard refresca los logs cronológicos de forma automática.
