README — Proyecto MARTÍN
DESCRIPCIÓN GENERAL

El presente documento describe la implementación técnica, arquitectura lógica, despliegue y validación funcional del Proyecto MARTÍN, desarrollado como una solución integral para el monitoreo y registro de eventos en sistemas robóticos.

La plataforma fue implementada sobre un servidor Linux en la nube pública utilizando contenedores Docker para garantizar modularidad, aislamiento de procesos y facilidad de mantenimiento.

El sistema integra:

Backend API para recepción de eventos.
Base de datos MongoDB para persistencia de información.
Frontend Dashboard Web para visualización de logs.

La arquitectura desacoplada permite tolerancia a fallos y continuidad operativa ante caídas parciales de servicios.

Todos los servicios operan dentro del mismo servidor cloud mediante contenedores Docker independientes interconectados sobre una red privada virtual.

OBJETIVOS DE MARTÍN
Objetivos Específicos
Aprovisionar y administrar un servidor Linux en DigitalOcean.
Implementar un backend capaz de recibir eventos del sistema robótico.
Integrar MongoDB para la persistencia estructurada de logs.
Diseñar un frontend web funcional para visualización de eventos.
Implementar los servicios mediante contenedores Docker.
Garantizar tolerancia a fallos mediante separación de microservicios.
Publicar el sistema mediante IP pública accesible desde internet.
ARQUITECTURA DEL SISTEMA

La arquitectura del Proyecto MARTÍN fue diseñada bajo el modelo de microservicios desacoplados, permitiendo separar las responsabilidades principales del sistema en distintos contenedores Docker independientes.

Esta estructura facilita:

Administración.
Mantenimiento.
Escalabilidad.
Reducción de puntos únicos de fallo.
Componentes Principales
Backend API

Encargado de la recepción y procesamiento de eventos.

MongoDB

Responsable del almacenamiento persistente de logs y telemetría.

Frontend Dashboard

Interfaz web para visualización de eventos mediante navegador.

Todos los servicios operan dentro de la red privada Docker:

red_martin
DIAGRAMA DE ARQUITECTURA
Robot/Cliente
      │
      ▼
Frontend Dashboard (Nginx + React)
      │
      ▼
Backend API (FastAPI)
      │
      ▼
MongoDB Database
BACKEND — SISTEMA DE LOGS

El backend del Proyecto MARTÍN funciona como el sistema central de recepción y administración de eventos generados por el entorno robótico.

Funciones Principales
Recibir eventos HTTP provenientes del robot.
Validar información recibida.
Registrar timestamps y datos asociados.
Persistir eventos en MongoDB.
Exponer endpoints REST.
Manejo controlado de errores.
Tecnologías Utilizadas
FastAPI
Docker
MongoDB Driver
REST API
ENDPOINT PRINCIPAL

El endpoint principal utilizado para la recepción de eventos es:

POST /logs

Este endpoint:

Recibe telemetría y eventos.
Valida información.
Registra datos en MongoDB.
Maneja errores controlados.
BASE DE DATOS MONGODB

MongoDB fue implementado como motor NoSQL para el almacenamiento flexible y escalable de eventos.

Ventajas Implementadas
Persistencia flexible.
Escalabilidad dinámica.
Manejo de estructuras JSON.
Integración directa con FastAPI.
Alta velocidad de consultas.
Funciones
Almacenamiento flexible de eventos.
Persistencia estructurada de logs.
Manejo eficiente de datos semiestructurados.
Escalabilidad futura del sistema.
FRONTEND DASHBOARD

El frontend corresponde a la interfaz web encargada de visualizar los eventos almacenados dentro del sistema.

Funciones
Visualización de eventos.
Consulta de logs.
Actualización dinámica.
Comunicación con API REST.
Monitoreo funcional en tiempo real.
Tecnologías
React
Nginx
Docker
Acceso Público
http://161.35.112.5
INFRAESTRUCTURA DEL SERVIDOR

La infraestructura fue desplegada sobre un Droplet de DigitalOcean utilizando Ubuntu Server 24.04 LTS.

Parámetro	Valor
Proveedor Cloud	DigitalOcean
Sistema Operativo	Ubuntu Server 24.04 LTS
Kernel Linux	6.8.0-31-generic
Arquitectura	x86_64
IP Pública	161.35.112.5
TECNOLOGÍAS UTILIZADAS
Infraestructura
Ubuntu Server 24.04 LTS
Docker
Docker Compose
DigitalOcean
Backend
FastAPI
REST API
Base de Datos
MongoDB
Frontend
React
Nginx
Control de Versiones
Git
GitHub
CONTENERIZACIÓN DEL SISTEMA

La contenerización fue implementada mediante Docker para aislar cada componente del sistema.

Contenedores Activos
Contenedor	Función	Puerto
lab_frontend	Dashboard Web	80
lab_backend	API FastAPI	3000
lab_mongodb	Base de Datos MongoDB	27017
Red Docker
red_martin
CONFIGURACIÓN DEL BACKEND

El backend fue desarrollado utilizando FastAPI debido a:

Alto rendimiento.
Baja latencia.
Facilidad de integración REST.
Compatibilidad con Docker.
Funciones Implementadas
Recepción de eventos.
Validación de datos.
Registro temporal.
Persistencia en MongoDB.
Manejo controlado de errores.
CONFIGURACIÓN DE MONGODB

MongoDB opera dentro de un contenedor Docker independiente y se comunica con el backend mediante una red privada Docker.

Ventajas
Persistencia flexible.
Escalabilidad.
Manejo dinámico de JSON.
Integración con FastAPI.
Consultas eficientes.
CONFIGURACIÓN DEL FRONTEND

El frontend fue desplegado utilizando Nginx como servidor web principal.

Funciones
Dashboard de monitoreo.
Consulta de eventos.
Actualización dinámica.
Comunicación con API REST.
PROTOCOLO DE VALIDACIÓN

El protocolo de validación fue diseñado para comprobar la independencia y estabilidad de los servicios.

Verificación del Sistema
Comandos
uname -a
lsb_release -a
Resultado Esperado
Ubuntu Server 24.04 LTS.
Infraestructura DigitalOcean operativa.
VERIFICACIÓN DE CONTENEDORES
Comando
sudo docker ps
Resultado Esperado

Visualización de:

lab_frontend
lab_backend
lab_mongodb

Todos ejecutándose simultáneamente.

PRUEBA DE CAÍDA DEL BACKEND
Comando
sudo docker stop lab_backend
Resultado Esperado
Frontend continúa accesible.
Dashboard muestra alerta de desconexión.
Scripts locales generan error de conexión.
Restauración
sudo docker start lab_backend
PRUEBA DE CAÍDA DE MONGODB
Comando
sudo docker stop lab_mongodb
Resultado Esperado
Backend sigue respondiendo.
API devuelve HTTP 500 controlado.
Eventos no son almacenados.
Restauración
sudo docker start lab_mongodb
PRUEBA DE CAÍDA DEL FRONTEND
Comando
sudo docker stop lab_frontend
Resultado Esperado
Navegador inaccesible.
Backend continúa funcionando.
Eventos siguen almacenándose correctamente.
Restauración
sudo docker start lab_frontend
DISEÑO DE ARQUITECTURA LÓGICA

La arquitectura lógica del Proyecto MARTÍN fue estructurada bajo el modelo de microservicios desacoplados.

Componentes
Frontend Dashboard.
Backend API.
MongoDB Database.
Beneficios
Tolerancia a fallos.
Independencia de servicios.
Continuidad operativa.
Escalabilidad.
Mantenimiento simplificado.
SEGURIDAD IMPLEMENTADA

La seguridad del sistema fue implementada mediante:

Acceso SSH remoto seguro.
Red privada Docker.
Separación de servicios.
Manejo controlado de excepciones.
Políticas CORS configuradas.
Aislamiento de procesos.
GITHUB Y CONTROL DE VERSIONES

GitHub fue utilizado como plataforma principal para control de versiones y administración colaborativa.

Implementaciones
Control de versiones distribuido.
Historial de commits.
Trabajo colaborativo.
Repositorio centralizado.
Evidencias Evaluadas
Commits individuales.
Participación de colaboradores.
Desarrollo incremental.
Versionamiento funcional.
CONCLUSIONES
La arquitectura desacoplada garantiza alta tolerancia a fallos.
Docker permitió una administración eficiente y modular.
MongoDB proporcionó flexibilidad y escalabilidad.
El sistema mantiene continuidad operativa ante fallos parciales.
RECOMENDACIONES A FUTURO
Implementar autenticación JWT.
Integrar monitoreo Prometheus/Grafana.
Configurar HTTPS mediante Nginx.
Implementar balanceo de carga.
Automatizar despliegues CI/CD.
Implementar backups automáticos de MongoDB.


