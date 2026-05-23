# Proyecto Final — MARTÍN

Este repositorio alberga el código fuente, las configuraciones de entorno y el protocolo de infraestructura correspondientes al **Proyecto Final de Sistemas Operativos II**. La solución implementa un ecosistema distribuido y desacoplado en la nube que actúa como el nodo receptor, validador y persistente de las métricas de telemetría y logs generados de forma asíncrona por el prototipo robótico de laboratorio (**MARTÍN**).

---

## 🌐 Información de Despliegue (Producción)

La infraestructura se encuentra completamente aprovisionada y operativa en producción. Con base en los requerimientos formales de la cátedra, el acceso se realiza de forma directa mediante direccionamiento IP nativo sin capas de enmascaramiento DNS:

* **Capa de Presentación (Dashboard Web):** `http://161.35.112.5`
* **Capa de Lógica de Negocio (REST API Entrypoint):** `http://161.35.112.5:3000`

---

## 📐 Diseño de la Arquitectura del Sistema

El sistema rompe con la rigidez de los esquemas monolíticos tradicionales al implementar una **Arquitectura Orientada a Microservicios (SOA)**. El flujo está segmentado en tres capas aisladas, independientes e indivisibles interconectadas mediante una topología de red virtualizada.

![Diagrama Técnico de Arquitectura de Contenedores y Flujo de Datos](image_1.png)

### Mecanismos de Aislamiento y Resiliencia en Redes
1. **Red Aislada de Tipo Bridge (`red_martin`):** Se implementó un conmutador virtual privado dentro del demonio de Docker. Este canal deniega de forma estricta cualquier exposición o mapeo público del puerto de la base de datos (`27017`) hacia Internet. La API (`lab_backend`) se comunica de forma exclusiva con el motor de persistencia mediante un **DNS interno por resolución de nombre de contenedor** (`mongodb://lab_mongodb:27017`), blindando el almacenamiento de datos contra vectores de ataque externos.
2. **Políticas de Autogestión de Procesos:** Todos los contenedores han sido inyectados con la directiva operativa `--restart unless-stopped`. Esto transfiere la responsabilidad de la orquestación de disponibilidad al motor nativo de Docker, obligando al host a levantar de forma automática el backend, el frontend y la base de datos inmediatamente tras un reinicio del sistema operativo, garantizando una alta tolerancia a fallos.

---

## 🛠️ Tecnologías y Ecosistema Tecnológico

### Infraestructura y Virtualización OS-Level
* **Cloud Hosting Provider:** DigitalOcean (Virtual Private Server - Droplet Aprovisionado).
* **Distribución Host Linux:** Ubuntu Server 24.04.4 LTS (Noble Numbat).
* **Núcleo del Sistema:** Linux Kernel 6.8.0-31-generic x86_64 architecture.
* **Motor de Contenedores:** Docker Engine Runtime Environment (Nativo).

### Stack de Componentes de Software
* **Frontend:** Nginx Server (Alpine-lightweight base image), encargado del renderizado estático de la interfaz del Dashboard mediante HTML5 semántico, CSS3 estructurado bajo patrones responsivos y JavaScript asíncrono puro (Fetch API).
* **Backend:** Python 3.10 + FastAPI Framework. Procesa la lógica transaccional, ejecuta la validación sintáctica de esquemas dinámicos JSON a través de modelos Pydantic y gestiona las políticas permisivas del middleware de Origen Cruzado (CORS).
* **Base de Datos:** MongoDB NoSQL Engine. Manejo flexible de colecciones orientadas a documentos BSON, vinculada a un volumen persistente mapeado directamente en el disco duro del Host (`proyecto_martin_mongo_data`).

---
## 🚀 Instrucciones de Uso y Flujo de Trabajo

Para la administración del sistema, auditorías de código o la demostración presencial del proyecto, siga el flujo operativo estándar detallado a continuación:

### 1. Conexión Remota al Servidor (SSH vía IP Pública)
Abra la terminal de su computadora local (PowerShell o CMD en Windows, o la Terminal en macOS/Linux) y ejecute el comando de transporte seguro apuntando a la IP pública del Droplet:
```bash
ssh martin@161.35.112.5
2. Autenticación e Ingreso de Contraseña (Seguridad de Linux)
Al dar Enter, el servidor solicitará las credenciales de acceso:

Bash
martin@161.35.112.5's password:
⚠️ Nota Crítica de Seguridad: Cuando escriba la contraseña en la terminal, no se va a reflejar ningún carácter en la pantalla (no aparecerán letras, asteriscos ni puntos). Esto es un mecanismo de seguridad nativo de Linux Ubuntu para evitar que alguien vea la longitud de su clave. Simplemente escríbala completa con el teclado y presione Enter.

3. Monitoreo y Verificación del Estado de los Contenedores
Una vez dentro del servidor, puede verificar en cualquier momento la salud, los sockets de escucha y el tiempo de actividad de los microservicios ejecutando:

Bash
sudo docker ps
Este comando devolverá una tabla en vivo. El proyecto está operando correctamente si visualiza los tres contenedores (lab_frontend, lab_backend y lab_mongodb) reportando el estado Up en la columna STATUS.

4. Inspección de Logs en Tiempo Real (Flujo del Robot)
Para comprobar que el robot MARTÍN (o los scripts de telemetría) están enviando los eventos con éxito y que el backend los está procesando de forma asíncrona, ejecute el comando de escucha activa:

Bash
sudo docker logs -f lab_backend
Presione Ctrl + C en su teclado cuando desee salir de la vista de logs y regresar a la consola ordinaria.



