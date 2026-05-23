ACTUALIZACION DE README
DESCRIPCIÓN GENERAL
El presente documento describe la implementación técnica, arquitectura lógica, despliegue y validación funcional del Proyecto MARTÍN, desarrollado como una solución integral para el monitoreo y registro de eventos en sistemas robóticos. La plataforma fue implementada sobre un servidor Linux en la nube pública utilizando contenedores Docker para garantizar modularidad y aislamiento de procesos. El sistema integra un backend para recepción de eventos, una base de datos MongoDB para persistencia de información y un frontend web para visualización de logs. La arquitectura desacoplada permite tolerancia a fallos y continuidad operativa ante caídas parciales de servicios. Además, el proyecto demuestra el uso de tecnologías modernas de infraestructura cloud, microservicios y administración distribuida. La plataforma fue desplegada sobre un servidor Linux en la nube pública utilizando contenedores Docker para garantizar modularidad, aislamiento de procesos y facilidad de mantenimiento. El sistema está compuesto por tres microservicios principales:
	Backend API de recepción de eventos 
	Base de datos MongoDB 
	Frontend Dashboard Web 
Todos los servicios operan dentro del mismo servidor cloud mediante contenedores Docker independientes interconectados sobre una red privada virtual.

OBJETIVOS DE MARTIN
Objetivos Específicos
1.	Aprovisionar y administrar un servidor Linux en DigitalOcean. 
2.	Implementar un backend capaz de recibir eventos del sistema robótico. 
3.	Integrar MongoDB para la persistencia estructurada de logs. 
4.	Diseñar un frontend web funcional para visualización de eventos. 
5.	Implementar los servicios mediante contenedores Docker. 
6.	Garantizar tolerancia a fallos mediante separación de microservicios. 
7.	Publicar el sistema mediante IP pública accesible desde internet.
   
ARQUITECTURA DEL SISTEMA
La arquitectura del Proyecto MARTÍN fue diseñada bajo el modelo de microservicios desacoplados, permitiendo separar las responsabilidades principales del sistema en distintos contenedores Docker independientes. Esta estructura facilita la administración, mantenimiento y escalabilidad de la plataforma, reduciendo significativamente los puntos únicos de fallo dentro de la infraestructura cloud.
El sistema está compuesto por tres componentes esenciales: un backend desarrollado para la recepción y procesamiento de eventos, una base de datos MongoDB encargada del almacenamiento persistente de logs y un frontend web utilizado para la visualización de información mediante un dashboard accesible desde la red pública. Cada servicio opera dentro de su propio contenedor aislado, conectado mediante una red virtual privada Docker denominada “red_martin”.
La comunicación entre servicios se realiza de forma interna y segura dentro del servidor Linux implementado en DigitalOcean. Gracias a esta arquitectura desacoplada, si uno de los componentes presenta una falla temporal, los demás servicios continúan funcionando de manera independiente, garantizando estabilidad operativa y continuidad en la captura de eventos del sistema robótico.


Backend — Sistema de Logs
El backend del Proyecto MARTÍN funciona como el sistema central de recepción y administración de eventos generados por el entorno robótico. Este servicio fue desarrollado para procesar solicitudes HTTP, registrar información relevante y comunicarse directamente con MongoDB para almacenar los logs de manera persistente. Además, implementa manejo controlado de errores para garantizar estabilidad y continuidad operativa del sistema
1. Recibir eventos HTTP provenientes del robot. 
2. Validar información recibida. 
3. Registrar timestamps y datos asociados. 
4. Persistir eventos en MongoDB. 
5. Exponer endpoints REST.
   
Base de Datos MongoDB
MongoDB es un sistema de base de datos NoSQL orientado al almacenamiento de información en formato de documentos JSON. En el Proyecto MARTÍN, esta tecnología fue utilizada para almacenar de manera flexible y estructurada todos los eventos generados por el sistema robótico. Su implementación permite manejar grandes volúmenes de datos sin necesidad de esquemas relacionales rígidos, facilitando la escalabilidad del sistema. MongoDB opera dentro de un contenedor Docker independiente y se comunica directamente con el backend mediante una red privada virtual interna. Gracias a su arquitectura flexible, es posible agregar nuevos tipos de eventos o sensores sin modificar estructuras complejas en la base de datos. Además, proporciona rapidez en consultas, persistencia eficiente y estabilidad para el almacenamiento continuo de logs y telemetría.
Motor NoSQL utilizado para:
1.	Almacenamiento flexible de eventos. 
2.	Persistencia estructurada de logs. 
3.	Escalabilidad dinámica. 
4.	Manejo eficiente de datos semiestructurados.
   
Frontend Dashboard
El frontend dashboard del Proyecto MARTÍN corresponde a la interfaz web encargada de visualizar los eventos almacenados dentro del sistema. Este componente permite mostrar de forma clara y ordenada los logs generados por el entorno robótico mediante un navegador web accesible desde la IP pública del servidor. El dashboard consume la información directamente desde el backend utilizando solicitudes HTTP hacia la API REST implementada. Además, fue diseñado para brindar monitoreo funcional y consulta dinámica de eventos en tiempo real o bajo demanda. Su despliegue se realizó dentro de un contenedor Docker independiente utilizando Nginx como servidor web principal
Interfaz web encargada de:
1.	Mostrar logs almacenados. 
2.	Visualizar eventos en tiempo real. 
3.	Consumir información desde el backend. 
4.	Presentar estado operativo del sistema.




INFRAESTRUCTURA DEL SERVIDOR
La infraestructura del servidor del Proyecto MARTÍN fue implementada sobre un Droplet de DigitalOcean utilizando Ubuntu Server 24.04 LTS como sistema operativo principal. El servidor funciona como el entorno central encargado de alojar todos los servicios del sistema mediante contenedores Docker independientes. La administración remota se realiza a través de conexiones seguras SSH, permitiendo monitoreo y mantenimiento desde cualquier ubicación autorizada. Todos los componentes operan dentro del mismo servidor físico virtualizado, optimizando recursos y simplificando la comunicación interna entre servicios. La arquitectura implementada garantiza estabilidad, modularidad y facilidad de escalabilidad para futuras ampliaciones del sistema. Además, el uso de infraestructura cloud permite disponibilidad permanente y acceso público mediante dirección IP externa.
Parámetro	Valor
Proveedor Cloud	DigitalOcean
Sistema Operativo	Ubuntu Server 24.04 LTS
Kernel Linux	6.8.0-31-generic
Arquitectura	x86_64
IP Pública	161.35.112.5




TECNOLOGIAS UTILIZADAS
El Proyecto MARTÍN fue desarrollado utilizando un conjunto de tecnologías modernas orientadas a la administración de infraestructura cloud, desarrollo web y contenerización de servicios. Como sistema operativo principal se implementó Ubuntu Server 24.04 LTS debido a su estabilidad, seguridad y compatibilidad con entornos de producción. Para la contenerización y aislamiento de procesos se utilizó Docker, permitiendo ejecutar el backend, frontend y base de datos en contenedores independientes. Además, Docker Compose fue utilizado para simplificar la orquestación y comunicación interna entre los servicios desplegados dentro del servidor cloud.
El backend fue desarrollado utilizando FastAPI por su alto rendimiento y facilidad para crear APIs REST eficientes, mientras que MongoDB fue implementado como sistema de base de datos NoSQL para el almacenamiento flexible de eventos y logs. Para el frontend se utilizó React junto con Nginx como servidor web encargado de exponer el dashboard hacia internet mediante la IP pública del servidor. Finalmente, GitHub fue utilizado como plataforma de control de versiones y colaboración del proyecto, permitiendo gestionar commits, cambios y trabajo colaborativo entre los integrantes del equipo
Parámetro	Valor
Proveedor Cloud	DigitalOcean
Sistema Operativo	Ubuntu Server 24.04 LTS
Kernel Linux	6.8.0-31-generic
Arquitectura	x86_64
IP Pública	161.35.112.5

CONTENERIZACIÓN DEL SISTEMA
La contenerización del sistema fue implementada mediante Docker con el objetivo de aislar y administrar cada componente del Proyecto MARTÍN de forma independiente. El backend, la base de datos MongoDB y el frontend fueron desplegados dentro de contenedores separados, permitiendo una arquitectura modular y organizada. Todos los servicios se comunican mediante una red virtual privada Docker denominada “red_martin”, garantizando conectividad interna segura entre procesos. Esta metodología facilita el mantenimiento, actualización y reinicio individual de cada servicio sin afectar el funcionamiento global del sistema. Además, la contenerización mejora la portabilidad de la aplicación y simplifica su despliegue en diferentes entornos Linux. Gracias a Docker, el proyecto logra mayor estabilidad, tolerancia a fallos y administración eficiente de recursos del servidor cloud.
Contenedores Activos
Contenedor	Función	Puerto
lab_frontend	Dashboard Web	80
lab_backend	API FastAPI	3000
lab_mongodb	Base de Datos MongoDB	27017





RED DORCKER
Red_martin

Configuración del backend
La configuración del backend del Proyecto MARTÍN fue desarrollada utilizando FastAPI como framework principal para la creación de servicios REST eficientes y de alto rendimiento. Este componente fue diseñado para recibir eventos generados por el sistema robótico mediante solicitudes HTTP enviadas hacia la API. El backend procesa la información recibida, valida los datos y registra automáticamente información relevante como timestamps, tipos de eventos y datos asociados. Además, implementa comunicación directa con MongoDB para almacenar de forma persistente todos los registros generados dentro del sistema.
El servicio backend fue desplegado dentro de un contenedor Docker independiente, permitiendo aislamiento total respecto a los demás componentes de la infraestructura. La comunicación entre servicios se realiza mediante una red privada Docker, garantizando conectividad interna segura y estable. También se implementaron mecanismos de manejo controlado de errores para evitar interrupciones críticas en caso de fallos temporales de la base de datos. Gracias a esta configuración, el backend mantiene estabilidad operativa y permite el procesamiento continuo de telemetría y logs en tiempo real.
Backend fue desarrollado utilizando FastAPI debido a:
1. Alto rendimiento 
2. Baja latencia 
3. Facilidad de integración REST 
4. Compatibilidad con Docker 
5. Funciones Principales
6. Recepción de eventos 
7. Validación de datos 
8. Registro temporal 
9. Persistencia en MongoDB 
10 Manejo controlado de errores
   
ENDPOINT PRINCIPAL
El endpoint principal del Proyecto MARTÍN corresponde al punto de acceso utilizado para la recepción de eventos enviados por el sistema robótico hacia el servidor backend. Este endpoint fue implementado mediante una API REST utilizando FastAPI, permitiendo procesar solicitudes HTTP de forma rápida y eficiente. Su función principal consiste en recibir información relacionada con acciones, estados y telemetría generada por el robot durante su funcionamiento. Una vez recibidos los datos, el backend valida la información y procede a almacenarla en MongoDB para garantizar persistencia de los registros. El endpoint también incorpora manejo controlado de errores para responder adecuadamente ante fallos internos o problemas de conexión con la base de datos. Gracias a esta implementación, el sistema puede centralizar y administrar todos los eventos generados dentro de la infraestructura del proyecto. POST /logs.

CONFIGURACIÓN DE MONGODB
La configuración de MongoDB dentro del Proyecto MARTÍN fue implementada mediante un contenedor Docker independiente encargado del almacenamiento persistente de eventos y logs generados por el sistema robótico. Esta base de datos NoSQL permite manejar información de forma flexible utilizando documentos JSON, facilitando el almacenamiento dinámico de datos sin esquemas rígidos. MongoDB se comunica directamente con el backend mediante una red privada Docker segura y aislada del exterior. Además, su arquitectura permite almacenar grandes volúmenes de información manteniendo un rendimiento eficiente en consultas y registros continuos. La implementación dentro de Docker facilita tareas de mantenimiento, reinicio y administración independiente del resto de servicios. Gracias a esta configuración, el sistema garantiza persistencia confiable, escalabilidad y estabilidad operativa para el manejo de telemetría y eventos.
Ventajas Implementadas
•	Persistencia flexible 
•	Escalabilidad 
•	Manejo dinámico de estructuras JSON 
•	Integración directa con FastAPI
CONFIGURACIÓN DEL FRONTEND
La configuración del frontend del Proyecto MARTÍN fue desarrollada como una interfaz web encargada de visualizar los eventos almacenados dentro del sistema de monitoreo. Este componente consume información desde el backend mediante solicitudes HTTP hacia la API REST implementada en FastAPI. El frontend fue desplegado dentro de un contenedor Docker independiente utilizando Nginx como servidor web principal para exponer el dashboard hacia internet. La interfaz permite mostrar los logs de manera organizada, clara y accesible desde cualquier navegador mediante la dirección IP pública del servidor. Además, el sistema fue diseñado para mantener comunicación constante con el backend y reflejar dinámicamente el estado de los servicios. Gracias a esta configuración, el dashboard proporciona una experiencia funcional y eficiente para el monitoreo de telemetría y eventos en tiempo real.
Funciones

•	Visualización de eventos 
•	Consulta de logs 
•	Actualización dinámica 
•	Comunicación con API REST 
Acceso Público
http://161.35.112.5

PROTOCOLO DE VALIDACIÓN
El protocolo de validación del Proyecto MARTÍN fue diseñado para comprobar el correcto funcionamiento e independencia de cada uno de los servicios implementados dentro de la infraestructura cloud. Las pruebas realizadas consistieron en pausar individualmente el backend, la base de datos MongoDB y el frontend para analizar el comportamiento general del sistema ante fallos parciales. Durante la validación se verificó que los servicios restantes continuaran operando de forma estable gracias a la arquitectura desacoplada implementada mediante Docker. También se evaluó la persistencia de eventos, la disponibilidad del dashboard web y la respuesta de la API REST ante errores controlados. Cada prueba fue ejecutada directamente desde la terminal Linux del servidor utilizando comandos Docker y herramientas de monitoreo. Gracias a este protocolo, se demostró la estabilidad, tolerancia a fallos y correcta integración de todos los componentes del Proyecto MARTÍN.
Comandos
uname -a
lsb_release -a
Resultado Esperado
Demostrar que el sistema opera sobre Ubuntu Server 24.04 LTS en DigitalOcean.
Verificación de Contenedores
Comando
sudo docker ps
Resultado Esperado
Visualización de:
•	lab_frontend 
•	lab_backend 
•	lab_mongodb 
todos ejecutándose simultáneamente.

PRUEBA DE CAÍDA DEL BACKEND
La prueba de caída del backend consistió en detener únicamente el contenedor encargado de procesar las solicitudes y registrar los eventos del sistema robótico. Durante esta validación se comprobó que el frontend continuara accesible desde el navegador, aunque mostrando una alerta de error al no poder comunicarse con la API. Asimismo, los scripts locales utilizados para enviar telemetría devolvieron errores de conexión debido a que el servicio backend se encontraba fuera de línea. Esta prueba permitió demostrar el desacoplamiento entre servicios y la independencia funcional del frontend respecto al backend. Gracias a la arquitectura basada en contenedores Docker, el fallo del backend no comprometió la estabilidad general del servidor cloud.
Comando
sudo docker stop lab_backend
Resultado Esperado
•	Frontend continúa accesible. 
•	Dashboard muestra alerta de desconexión. 
•	Scripts locales generan error de conexión. 
Restauración
sudo docker start lab_backend

Prueba de Caída de MongoDB
Comando
sudo docker stop lab_mongodb
Resultado Esperado
•	Backend sigue respondiendo. 
•	API devuelve HTTP 500 controlado. 
•	Eventos no son almacenados. 
Restauración
sudo docker start lab_mongodb
________________________________________
10.5 Prueba de Caída del Frontend
Comando
sudo docker stop lab_frontend
Resultado Esperado
•	Navegador inaccesible. 
•	Backend continúa funcionando. 
•	Eventos siguen almacenándose correctamente. 
Restauración
sudo docker start lab_frontend
DISEÑO DE ARQUITECTURA LOGICA
El diseño de arquitectura lógica del Proyecto MARTÍN fue estructurado bajo el modelo de microservicios desacoplados para garantizar independencia y estabilidad entre componentes. La arquitectura está conformada por un frontend dashboard, un backend API y una base de datos MongoDB, todos ejecutándose dentro de contenedores Docker independientes. El frontend se encarga de visualizar los eventos del sistema mediante una interfaz web accesible desde la IP pública del servidor. El backend actúa como intermediario centralizado encargado de recibir, procesar y almacenar los eventos generados por el sistema robótico. MongoDB funciona como motor de persistencia para almacenar de forma estructurada toda la información registrada por la API. Gracias a esta arquitectura, el sistema mantiene tolerancia a fallos y continuidad operativa incluso ante caídas parciales de alguno de los servicios.

 
SEGURIDAD IMPLEMENTADA
La seguridad implementada en el Proyecto MARTÍN se basó en el aislamiento de servicios mediante contenedores Docker y en la administración remota segura a través de conexiones SSH. Cada componente del sistema opera dentro de una red privada Docker, evitando exposiciones innecesarias entre servicios internos. Además, se configuraron políticas de comunicación controlada mediante CORS para permitir únicamente conexiones autorizadas entre frontend y backend. El backend incorpora manejo controlado de errores para prevenir fallos críticos y proteger la estabilidad general de la plataforma. Gracias a estas medidas, el sistema mantiene un entorno más seguro, estable y confiable para el procesamiento de eventos y telemetría.
Medidas Aplicadas
•	Acceso SSH remoto seguro 
•	Red privada Docker 
•	Separación de servicios 
•	Manejo controlado de excepciones 
•	Políticas CORS configuradas 
•	Aislamiento de procesos
GitHub y Control de Versiones
GitHub fue utilizado como plataforma principal para el control de versiones y administración colaborativa del Proyecto MARTÍN. A través del repositorio centralizado, los integrantes del equipo pudieron gestionar cambios, actualizar código y mantener un historial organizado del desarrollo del sistema. Cada modificación realizada quedó registrada mediante commits individuales, permitiendo evidenciar la participación y contribución de cada integrante del proyecto. Además, GitHub facilitó el trabajo colaborativo y la sincronización del código entre los diferentes componentes del sistema. Gracias al uso de control de versiones distribuido, se logró mantener estabilidad, organización y seguimiento continuo durante todo el proceso de desarrollo.
El proyecto fue administrado mediante GitHub utilizando:
1.	Control de versiones distribuido 
2.	Historial de commits 
3.	Trabajo colaborativo 
4.	Repositorio centralizado 
Evidencias Evaluadas
1.	Commits individuales 
2.	Participación de colaboradores 
3.	Desarrollo incremental 
4.	Versionamiento funcional
CONCLUSIONES
1.	La arquitectura desacoplada implementada garantiza alta tolerancia a fallos. 
2.	Docker permitió una administración eficiente y modular de los servicios. 
3.	MongoDB proporcionó flexibilidad y escalabilidad para el almacenamiento de eventos. 
4.	El sistema mantiene continuidad operativa incluso ante fallos parciales.
RECOMENDACIONES A FUTURO
1.	Implementar autenticación JWT. 
2.	Integrar monitoreo Prometheus/Grafana. 
3.	Configurar HTTPS mediante Nginx. 
4.	Implementar balanceo de carga. 
5.	Automatizar despliegues mediante CI/CD. 
6.	Implementar backups automáticos de MongoDB. 


