# Glosario esencial de conceptos — Certificación F5 Certified Administrator, BIG-IP

**Exámenes:** F5CAB1, F5CAB2, F5CAB3, F5CAB4 y F5CAB5  
**Propósito:** material de estudio para reconocer conceptos, interpretar configuraciones y resolver escenarios de examen.  
**Alcance:** incluye términos de los laboratorios y otros importantes para la certificación; no es un banco de preguntas ni una lista oficial de términos que aparecerán en el examen.

> **Cómo estudiar:** para cada término, explica (1) qué es, (2) qué problema resuelve y (3) en qué se diferencia de una opción parecida. Los nombres de menús y algunas capacidades pueden variar según la versión y la licencia de BIG-IP. Los exámenes F5CAB no se limitan a una versión concreta.

## Mapa de los cinco exámenes

| Examen | Dominio de estudio | Pregunta guía |
|---|---|---|
| **F5CAB1** | Instalación, configuración inicial y actualizaciones | ¿Cómo se instala, protege, licencia y actualiza BIG-IP? |
| **F5CAB2** | Conceptos del Data Plane | ¿Cómo entra, se selecciona y se encamina el tráfico? |
| **F5CAB3** | Configuración del Data Plane | ¿Cómo se publican aplicaciones con pools, perfiles y persistencia? |
| **F5CAB4** | Administración del Control Plane | ¿Cómo se administra, respalda, monitorea y recupera el equipo? |
| **F5CAB5** | Soporte y troubleshooting | ¿Cómo se distingue un síntoma de una causa raíz? |

---

## F5CAB1 — Instalación, configuración inicial y actualización

| Término | Definición indispensable | Clave de examen / distinción |
|---|---|---|
| **BIG-IP** | Plataforma F5 de entrega, disponibilidad y seguridad de aplicaciones, física o virtual. | Un BIG-IP puede ejecutar distintos módulos; BIG-IP no es sinónimo exclusivo de LTM. |
| **TMOS** | Sistema operativo y arquitectura de software de BIG-IP, que incluye herramientas de configuración y componentes de procesamiento. | Distinguir el sistema operativo de los módulos que ofrecen funciones. |
| **BIG-IP VE** | *Virtual Edition*: BIG-IP desplegado como máquina virtual. | Las características físicas (interfaces, LCD, hardware de aceleración) no necesariamente están presentes. |
| **LTM** | *Local Traffic Manager*: módulo que administra y distribuye tráfico de aplicaciones. | LTM es un módulo; TMM es el motor que procesa tráfico. |
| **Control Plane** | Plano de administración y control: configuración, servicios de gestión y procesos del sistema. | Acceder por SSH/TMUI no es lo mismo que enviar tráfico al VS. |
| **Data Plane** | Plano que procesa conexiones y paquetes de aplicaciones. | Un fallo de TMUI no implica necesariamente que el tráfico de aplicaciones se haya detenido. |
| **TMM** | *Traffic Management Microkernel*, componente de BIG-IP encargado de procesar tráfico de red. | Sus métricas de CPU y memoria deben distinguirse de las del host. |
| **TMUI / Configuration Utility** | Interfaz web de administración de BIG-IP. | Habitualmente se accede por la IP de Management; el acceso depende de ACL y configuración. |
| **TMSH** | *Traffic Management Shell*: CLI estructurada para consultar y modificar BIG-IP. | `list` muestra configuración y `show` presenta estado/estadísticas; `modify` cambia configuración. |
| **Management IP** | Dirección usada para administración fuera del flujo normal de aplicaciones. | No es la misma función que una Self IP o una VIP. |
| **Management Route** | Ruta utilizada por el subsistema de administración. | No debe confundirse con las rutas que utiliza TMM en el Data Plane. |
| **Self IP** | Dirección IP de BIG-IP dentro de una VLAN de datos; participa en comunicación y administración local de esa red. | No es, por definición, una Virtual Address, aunque en un laboratorio pueda coincidir la IP de un VS con otra dirección local si la configuración lo permite. |
| **Port Lockdown** | Política de servicios de acceso permitidos en una Self IP. | Restringe servicios de la Self IP; no equivale a cerrar un puerto de un Virtual Server. |
| **SSHD ACL** | Lista de direcciones origen autorizadas para acceder al servicio SSH de administración. | Diferenciar del Port Lockdown de una Self IP. |
| **HTTPD ACL** | Restricción de orígenes para acceder a la interfaz web de administración. | No regula el HTTP de aplicaciones publicado por LTM. |
| **Password Policy** | Parámetros de complejidad, caducidad, reutilización y bloqueo de contraseñas de usuarios locales. | Diferenciar una medida de autenticación de filtrado de red. |
| **System/Device Provisioning** | Asignación de recursos para habilitar módulos BIG-IP. | Un módulo puede estar licenciado pero no provisionado, o provisionado sin la capacidad/licencia requerida. |
| **License / Registration Key** | Derechos de uso y activación de funcionalidades del producto. | Identificar activación inicial, reactivación y relación con los módulos autorizados. |
| **Service Check Date (SCD)** | Fecha que se usa para determinar elegibilidad para instalar determinadas versiones de software según los derechos de soporte/actualización. | Una imagen descargada no implica automáticamente elegibilidad para instalarla. |
| **License Reactivation** | Actualización del estado o contenido de licencia con el sistema de licencias F5. | No equivale a aprovisionar un módulo o cambiar una versión de software. |
| **Software Image / ISO** | Imagen de instalación de BIG-IP. | Cargar una imagen no instala ni activa automáticamente la versión. |
| **Boot Volume / Boot Location** | Volumen desde el que BIG-IP puede iniciar una instalación de software. | Permite mantener un volumen alternativo y seleccionar a cuál arrancar. |
| **Active Boot Location** | Volumen actualmente arrancado. | Distinguir la versión *en ejecución* de una imagen almacenada en otro volumen. |
| **Upgrade** | Instalación de una versión más reciente, generalmente sobre un volumen de destino. | Revisar licencia/SCD, compatibilidad, backup, arranque y validación. |
| **Rollback** | Regreso a un estado o versión anterior tras una actualización fallida. | Volver de boot volume y restaurar configuración son acciones distintas. |
| **UCS** | *User Configuration Set*: archivo de respaldo de configuración y otros datos del dispositivo. | No sustituye automáticamente a una imagen ISO ni garantiza compatibilidad entre versiones. |
| **Attack Surface** | Conjunto de puntos de exposición potencialmente explotables de un sistema. | Reducir servicios de administración expuestos disminuye superficie de ataque. |
| **DoS / DDoS** | Ataque que intenta degradar disponibilidad; DDoS utiliza múltiples orígenes. | Un límite de conexiones es una posible protección, pero no equivale a un sistema completo de mitigación DoS. |

### Distinciones críticas CAB1

- **Management IP vs Self IP vs Virtual Address:** administración del equipo, presencia del equipo en una red de datos y destino publicado de una aplicación, respectivamente.
- **Licenciado vs provisionado vs operativo:** permiso de uso, recursos asignados y servicio funcional son condiciones diferentes.
- **ISO vs boot volume vs UCS:** software de instalación, instalación arrancable y respaldo de configuración no son intercambiables.

---

## F5CAB2 — Conceptos del Data Plane

| Término | Definición indispensable | Clave de examen / distinción |
|---|---|---|
| **ADC** | *Application Delivery Controller*: sistema que optimiza, protege y administra la entrega de aplicaciones. | El ADC toma decisiones de tráfico; no es únicamente un switch o router. |
| **Interface** | Puerto de red físico o virtual utilizado por BIG-IP. | Su estado *up* no demuestra por sí solo conectividad de aplicación. |
| **VLAN** | Dominio lógico de capa 2 que asocia interfaces y tráfico a una red. | VLAN y subred IP son conceptos de capas distintas. |
| **Tagged VLAN** | VLAN transportada con etiqueta IEEE 802.1Q. | Una interfaz trunk puede transportar varias VLAN etiquetadas. |
| **Untagged VLAN** | Tráfico de una VLAN que se intercambia sin etiqueta VLAN en el puerto correspondiente. | No es una VLAN inexistente: es una forma de transportar sus tramas. |
| **Trunk** | Agrupación lógica de interfaces físicas para redundancia y/o distribución de enlaces. | El trunk F5 es agrupación de puertos; no debe confundirse con el uso informal de “trunk” para VLAN tagged. |
| **LACP** | Protocolo de negociación de agregación de enlaces. | Relacionado con trunks, no con balanceo HTTP entre pool members. |
| **Route / Routing Table** | Regla que determina la salida hacia una red destino. | Una ruta no reemplaza la configuración de VLAN y Self IP necesaria para la conectividad. |
| **Default Route** | Ruta usada cuando no existe una coincidencia más específica. | En troubleshooting se debe identificar cuál plano utiliza la ruta. |
| **ARP** | Resolución de direcciones IPv4 a MAC en la red local. | Ausencia de ARP correcto puede impedir conectividad aunque las rutas sean válidas. |
| **Virtual Address** | Dirección IP a la que puede asociarse uno o más Virtual Servers. | El Virtual Server normalmente combina destino IP, puerto y protocolo. |
| **VIP (uso informal)** | Término habitual para la IP de servicio publicada por un Virtual Server. | No siempre identifica por sí solo el puerto/protocolo de la aplicación. |
| **Virtual Server (VS)** | Objeto BIG-IP que identifica tráfico entrante y define cómo se procesa. | La coincidencia de VS depende de destino, puerto, protocolo y otras condiciones. |
| **Standard VS** | VS basado en proxy que permite perfiles y funciones L4–L7. | Es diferente de Forwarding y de Performance L4. |
| **Forwarding (IP) VS** | VS destinado a reenviar tráfico IP sin balancear hacia un pool de aplicación. | Su función principal es encaminamiento, no distribución a servidores web. |
| **Forwarding (Layer 2) VS** | VS para reenvío a nivel Ethernet en escenarios compatibles. | No debe equipararse con un Standard VS HTTP. |
| **Stateless VS** | VS diseñado para procesar tráfico sin mantener el mismo estado de conexión que un proxy con estado. | Distinguir de un VS Standard con tabla completa de conexiones. |
| **Reject VS** | VS que rechaza explícitamente tráfico coincidente. | No es sinónimo de “deshabilitado”: la respuesta de red puede ser diferente. |
| **Performance (Layer 4) VS** | VS optimizado para procesamiento de tráfico L4 con camino de datos eficiente. | No ofrece necesariamente el mismo conjunto de funciones HTTP que Standard. |
| **Performance (HTTP) VS** | VS optimizado para aplicaciones HTTP mediante perfiles específicos. | Distinguir de Performance L4 y de Standard. |
| **Wildcard VS** | VS con dirección y/o servicio de coincidencia amplia, por ejemplo `0.0.0.0`. | Estudiar la precedencia de selección frente a VS más específicos. |
| **VS Precedence** | Reglas con las que BIG-IP determina qué VS coincide con un flujo cuando existen varias coincidencias posibles. | No asumir que “el último creado” o “el primero de la lista” recibe la conexión. |
| **Destination Address / Service Port** | IP y puerto/protocolo del servicio al que se conecta el cliente. | Puerto 443 por sí solo no significa que exista un perfil SSL. |
| **SNAT** | *Source Network Address Translation*: sustituye la IP origen cuando BIG-IP establece el flujo hacia el servidor. | Es distinto de cambiar la IP destino (DNAT). |
| **SNAT AutoMap** | SNAT que utiliza una dirección Self IP apropiada del BIG-IP para el lado servidor. | En nuestros labs el backend suele ver `10.1.2.200`, no `10.1.3.4`. |
| **SNAT Pool** | Conjunto de direcciones IP usado para traducción de origen. | Más control explícito que AutoMap sobre direcciones de traducción. |
| **DNAT / Destination Translation** | Transformación de la IP/puerto de destino hacia el servidor seleccionado. | No debe confundirse con SNAT ni con la IP pública de administración. |
| **Full Proxy** | Arquitectura en que BIG-IP termina una conexión cliente y establece otra conexión independiente hacia el servidor. | Dos conexiones TCP: *client-side* y *server-side*. |
| **Client-side Flow** | Conexión entre cliente y BIG-IP. | Conserva la IP cliente original hasta que BIG-IP procese el flujo; puede verse en capturas externas. |
| **Server-side Flow** | Conexión entre BIG-IP y servidor seleccionado. | Puede utilizar SNAT y perfiles diferentes del lado cliente. |
| **Traffic Group** | Agrupación lógica de objetos de tráfico que puede moverse entre dispositivos en HA. | No equivale a un pool o a un grupo de prioridades de pool members. |
| **Connection Mirroring** | Replicación selectiva del estado de conexión entre miembros HA para mejorar continuidad. | ConfigSync sincroniza configuración; mirroring se relaciona con estados/conexiones. |

### Distinciones críticas CAB2

- **Trunk vs VLAN:** el primero agrupa enlaces; la segunda separa dominios de tráfico.
- **Standard vs Performance L4 vs Reject:** proxy de aplicación, procesamiento L4 optimizado y rechazo explícito.
- **SNAT vs DNAT:** modificación del origen frente a modificación del destino.
- **Pool vs Traffic Group:** selección de servidores frente a movilidad de objetos entre BIG-IP en HA.

---

## F5CAB3 — Configuración del Data Plane

| Término | Definición indispensable | Clave de examen / distinción |
|---|---|---|
| **Pool** | Grupo de destinos de aplicación entre los que BIG-IP distribuye tráfico. | Un VS puede tener un pool predeterminado, y reglas/políticas pueden alterar la selección. |
| **Pool Member** | Combinación de nodo y puerto que ofrece un servicio concreto, p. ej. `10.1.2.4:80`. | Un servidor puede ofrecer varios miembros en puertos distintos. |
| **Node** | Representación del servidor por su dirección IP, independientemente del puerto del servicio. | Node `10.1.2.4` no equivale exactamente a member `10.1.2.4:80`. |
| **Load Balancing Method** | Algoritmo que decide qué pool member se selecciona para nuevas solicitudes/conexiones. | La persistencia puede prevalecer sobre la distribución ordinaria del algoritmo. |
| **Round Robin** | Selección rotatoria de miembros elegibles. | No garantiza 50/50 en todos los intervalos: influyen conexiones, persistencia y disponibilidad. |
| **Ratio** | Método que pondera la selección mediante valores configurados. | Distinguir peso relativo de prioridad de activación. |
| **Least Connections** | Favorece miembros con menos conexiones activas, según la variante configurada. | Diferente de Round Robin y de Fastest. |
| **Priority Group Activation** | Función que utiliza prioridades de miembros y un umbral para habilitar grupos de respaldo cuando los preferentes no cumplen la condición. | **Prioridad ≠ ratio**; validar cuántos miembros elegibles hay en el grupo prioritario. |
| **Priority Group** | Valor de prioridad asignado a miembros del pool para escalonar su uso. | Un miembro en prioridad inferior puede permanecer en reserva. |
| **Health Monitor** | Comprobación periódica de la salud/servicio de un destino. | El monitor mide el criterio configurado; no garantiza funcionalidad completa de la aplicación. |
| **HTTP Monitor** | Monitor que envía una petición HTTP y opcionalmente evalúa una cadena de respuesta. | Un `recv` incorrecto puede marcar DOWN un servicio que responde HTTP 200. |
| **TCP Monitor** | Monitor que comprueba establecimiento de conexión TCP al servicio. | Puerto abierto no significa HTTP funcional. |
| **Monitor Interval** | Frecuencia entre comprobaciones. | Diferente del timeout que determina cuándo se declara un fallo. |
| **Monitor Timeout** | Límite para considerar fallida la monitorización según su configuración. | No equivale al TCP idle-timeout del VS. |
| **Send String** | Solicitud que envía un monitor (por ejemplo una línea HTTP GET). | Debe ser compatible con la aplicación que recibe la prueba. |
| **Receive String** | Patrón buscado en la respuesta por un monitor. | Es posible producir falsos DOWN si la cadena no coincide. |
| **Availability State** | Estado calculado de disponibilidad según monitores y otros factores. | `Available`/`Offline` no es lo mismo que habilitado/deshabilitado administrativamente. |
| **Administrative State** | Estado configurado por el administrador: habilitado, deshabilitado o forzado fuera de servicio según objeto. | Un member puede estar administrativamente habilitado y estar DOWN por monitor. |
| **Disabled (Member)** | Estado administrativo que restringe la selección de nuevas conexiones según el comportamiento del producto y la persistencia. | Distinguir de Forced Offline, especialmente con conexiones persistentes. |
| **Forced Offline** | Estado que retira más estrictamente a un miembro de la selección para nuevos flujos. | No afirmar que mata automáticamente toda conexión existente. |
| **Connection Limit** | Máximo de conexiones simultáneas permitido en un objeto. | No es un límite de solicitudes por segundo ni significa que toda conexión adicional reciba HTTP 503. |
| **Persistence** | Mecanismo para dirigir solicitudes relacionadas al mismo destino conforme a una regla de afinidad. | Diferente del método de balanceo para solicitudes no persistentes. |
| **Source Address Affinity** | Persistencia que utiliza dirección IP de origen como clave. | Muchos clientes detrás de NAT pueden compartir una misma IP. |
| **Cookie Persistence** | Persistencia HTTP mediante una cookie que identifica la afinidad con un miembro. | Requiere procesamiento HTTP apropiado; no todas las cookies son de sesión de la aplicación. |
| **Cookie Insert** | Modo en que BIG-IP añade una cookie de persistencia a la respuesta HTTP. | Buscar `Set-Cookie` en la primera respuesta y `Cookie` en solicitudes posteriores. |
| **Cookie Rewrite** | BIG-IP modifica una cookie existente bajo ciertas condiciones para persistencia. | No equivale al modo Insert. |
| **Persistence Record** | Entrada de estado que conserva una asociación de persistencia según el perfil y método. | Algunos métodos no muestran la misma clase de registros; no asumir siempre una fila en `persist-records`. |
| **Fallback Persistence** | Método alterno de persistencia si el principal no puede aplicarse. | Diferente de usar un pool de respaldo. |
| **Profile** | Conjunto reutilizable de parámetros de procesamiento que se aplica a un VS. | Cambiar un perfil compartido puede afectar muchos VS. |
| **TCP Profile** | Parámetros de manejo de sesiones TCP, timers y funciones TCP. | Un timeout incorrecto puede cortar conexiones inactivas. |
| **TCP Idle Timeout** | Tiempo máximo de inactividad de conexión antes de su expiración. | No es un timeout de respuesta HTTP ni la duración total máxima de una descarga activa. |
| **HTTP Profile** | Habilita y controla la interpretación HTTP en el flujo. | Necesario para muchas funciones L7, incluida la persistencia por cookie. |
| **OneConnect** | Función para desacoplar/reutilizar conexiones server-side respecto de conexiones client-side, según configuración. | Evitar deducir una conexión de servidor por cada solicitud HTTP cuando está activo. |
| **Client SSL Profile** | Configura la terminación TLS entre cliente y BIG-IP. | **Client SSL no cifra automáticamente BIG-IP → backend.** |
| **Server SSL Profile** | Configura TLS entre BIG-IP y servidor de aplicación. | Requiere que el destino soporte TLS y parámetros compatibles. |
| **SSL Offloading** | BIG-IP termina TLS del cliente y puede enviar HTTP sin cifrar al backend. | Diferenciar de re-encryption (Client SSL + Server SSL). |
| **SSL Re-encryption** | BIG-IP termina una sesión TLS y establece otra hacia backend usando Server SSL. | Son dos sesiones TLS distintas, no un túnel TLS de extremo a extremo transparente. |
| **TLS Handshake** | Intercambio criptográfico para negociar parámetros, autenticar el servidor y establecer claves de sesión. | Un fallo TLS puede ocurrir con pool disponible y conexión TCP establecida. |
| **Certificate / Private Key** | Certificado identifica una clave pública/identidad; clave privada permite demostrar control criptográfico. | El certificado sin clave privada correspondiente no basta para terminar TLS. |
| **SNI** | *Server Name Indication*: extensión TLS con nombre del servidor solicitado. | Permite elegir certificados o contextos TLS según hostname. |
| **Cipher Suite** | Conjunto/selección de algoritmos utilizados durante una sesión TLS según versión y negociación. | La falta de compatibilidad cliente/servidor causa errores de negociación. |
| **iRule** | Lógica basada en eventos que puede inspeccionar o modificar decisiones de tráfico. | Se ejecuta en contextos/eventos específicos; no reemplaza una política de administración. |
| **LTM Policy** | Conjunto declarativo de reglas para decisiones L7, redirecciones, selección de pool y otras acciones compatibles. | Comparar política declarativa con lógica programada de iRule. |
| **iApp** | Plantilla/servicio de aplicación que crea y administra varios objetos asociados. | Los objetos pueden depender del servicio iApp y no conviene editarlos aisladamente. |
| **X-Forwarded-For (XFF)** | Cabecera HTTP que puede transportar la IP original del cliente a la aplicación. | Con SNAT el backend ve otra IP a nivel TCP; XFF no cambia el origen del paquete. |

### Distinciones críticas CAB3

- **Node vs Pool Member:** dirección del host frente a servicio IP:puerto.
- **Ratio vs Priority Group:** ponderar distribución frente a habilitar grupos por niveles.
- **Disabled vs Forced Offline vs Monitor DOWN:** decisión administrativa, exclusión administrativa estricta y condición monitorizada.
- **Cookie Insert vs Source Address:** persistencia basada en cookie HTTP frente a IP cliente.
- **Client SSL vs Server SSL:** cifrado cliente–BIG-IP frente a BIG-IP–servidor.
- **iRule vs LTM Policy vs iApp:** lógica basada en eventos, reglas declarativas y plantilla de despliegue de aplicación.

---

## F5CAB4 — Administración del Control Plane

| Término | Definición indispensable | Clave de examen / distinción |
|---|---|---|
| **Administrator / Operator Role** | Roles locales de BIG-IP con distintos permisos sobre configuración y estado. | Un usuario que puede consultar no necesariamente puede modificar. |
| **RBAC** | *Role-Based Access Control*: autorización basada en roles. | La identidad del usuario no implica permisos de administrador global. |
| **Authentication** | Proceso que verifica quién intenta acceder al sistema. | Distinguir de autorización (qué se le permite hacer). |
| **Authorization** | Determinación de acciones permitidas para una identidad autenticada. | Puede depender del rol o de atributos/grupos remotos. |
| **Local Authentication** | Cuentas y credenciales gestionadas directamente en BIG-IP. | Diferente de LDAP/RADIUS/TACACS+. |
| **LDAP** | Protocolo de acceso a directorios; puede utilizarse en autenticación remota según integración. | Directorio no es, por sí mismo, un método de autorización de tráfico LTM. |
| **RADIUS** | Protocolo AAA de uso frecuente para autenticación de acceso. | Distinguir de TACACS+ y de las contraseñas locales. |
| **TACACS+** | Protocolo AAA usado en administración de dispositivos, con capacidades de autorización y contabilidad. | No es sinónimo de RADIUS. |
| **AAA** | Autenticación, autorización y contabilidad (*accounting*). | Tres funciones relacionadas pero diferentes. |
| **NTP** | Sincronización horaria del sistema. | Hora correcta facilita certificados, correlación de logs y diagnóstico. |
| **DNS Resolver** | Servicio de resolución de nombres usado por componentes de BIG-IP. | Configurar DNS no significa que BIG-IP actúe como servidor DNS de aplicaciones. |
| **Syslog** | Protocolo/formato de envío de mensajes de registro. | Verificar destino, transporte y recepción; no confundir con SNMP. |
| **Remote Logging** | Exportación de eventos a un receptor externo. | Diferenciar eventos de administración, tráfico y seguridad. |
| **SNMP** | Protocolo de monitoreo y administración mediante objetos MIB/OID. | Una comunidad de lectura no garantiza que un monitor remoto pueda acceder. |
| **SNMP Community** | Cadena de acceso en SNMPv1/v2c. | SNMPv3 ofrece un modelo de seguridad más robusto que una comunidad compartida. |
| **SNMP Trap** | Notificación de evento que el agente envía a un receptor. | No equivale a consulta periódica (*polling*) de contadores. |
| **MIB / OID** | Estructura y identificador de variables consultables por SNMP. | MIB describe objetos; OID identifica uno concreto. |
| **/var/log/ltm** | Registro de eventos relacionados con tráfico local, monitores y otros componentes LTM. | El log correcto depende del síntoma; ausencia de un mensaje no confirma ausencia de falla. |
| **Audit Log** | Registro de operaciones y cambios administrativos. | Distinguir de tráfico HTTP y de fallas del sistema. |
| **Core Dump** | Archivo de diagnóstico generado cuando un proceso falla bajo determinadas circunstancias. | Puede requerir soporte técnico; no implica automáticamente corrupción de configuración. |
| **UCS Archive** | Archivo exportable para guardar la configuración de BIG-IP. | Crear, descargar y restaurar son operaciones distintas. |
| **SCF** | *Single Configuration File*: representación textual de configuración para determinadas tareas de respaldo/importación. | No contiene exactamente el mismo conjunto de datos que un UCS. |
| **ConfigSync** | Sincronización de configuración entre dispositivos integrantes de una asociación HA configurada. | No replica automáticamente el estado de conexiones existentes. |
| **Device Trust** | Relación de confianza criptográfica y administrativa entre dispositivos BIG-IP que cooperan. | Es un prerrequisito importante de escenarios HA/ConfigSync. |
| **Device Group** | Grupo de dispositivos para funciones de sincronización y/o conmutación. | Distinto de un pool de servidores de aplicación. |
| **Active/Standby** | Patrón HA en el que un dispositivo presta el tráfico de determinado grupo y otro está preparado para asumirlo. | El estado puede evaluarse por traffic group, no solo por “caja”. |
| **Failover** | Transferencia de responsabilidad de tráfico/objetos al equipo alterno. | Diferente de seleccionar Web02 dentro de un pool por Priority Group. |
| **ConfigSync Status** | Estado que indica si la configuración relevante de miembros de grupo está sincronizada. | *In sync* no garantiza disponibilidad de servidores ni ausencia de fallos TLS. |
| **Force Offline** | Acción de administración que fuerza determinado estado fuera de servicio de dispositivo u objeto, según contexto. | No confundir con *Forced Offline* de un pool member; cambia el alcance. |
| **Service Status** | Estado de procesos y componentes de control como `mcpd` o `httpd`. | Servicio de gestión inactivo no demuestra que TMM esté caído. |
| **MCPD** | Daemon responsable de gestión de configuración y coordinación de objetos en BIG-IP. | Fallos de `mcpd` pueden afectar cambios de configuración, sin equivaler a fallo del backend. |
| **Boot Integrity / Boot Location** | Elementos relacionados con ubicación de arranque, software instalado y recuperación. | Antes de actualizar, comprobar compatibilidad, espacio y opción de vuelta atrás. |

### Distinciones críticas CAB4

- **Authentication vs Authorization:** confirmar identidad frente a asignar privilegios.
- **ConfigSync vs Connection Mirroring:** sincronizar configuración frente a replicar estado de conexión.
- **UCS vs SCF:** respaldo más completo frente a configuración textual.
- **SNMP polling vs trap vs syslog:** consulta de métricas, notificación y transmisión de logs.
- **Failover HA vs Priority Group failover:** cambio de dispositivo frente a cambio de backend dentro de un pool.

---

## F5CAB5 — Soporte y troubleshooting

| Término | Definición indispensable | Clave de examen / distinción |
|---|---|---|
| **Troubleshooting Methodology** | Procedimiento disciplinado para identificar síntomas, aislar componentes, formular hipótesis, corregir y verificar. | No cambiar varios objetos sin evidencia: puede ocultar la causa raíz. |
| **Symptom vs Root Cause** | Síntoma: comportamiento observado; causa raíz: condición responsable que explica ese comportamiento. | `Pool offline` puede ser síntoma de un Receive String erróneo, no un servidor caído. |
| **Baseline** | Mediciones del sistema en condiciones conocidas para comparar cambios. | Una lectura aislada de CPU no prueba degradación. |
| **Availability vs Administrative State** | Disponibilidad calculada frente a estado configurado. | Un VS puede estar habilitado y no disponible por falta de miembros saludables. |
| **Network Map** | Representación gráfica de relaciones y estados de objetos LTM. | Útil para localizar el área afectada, no siempre para demostrar la causa. |
| **Statistics / Counters** | Valores acumulados o instantáneos de conexiones, bytes, paquetes, errores y eventos. | Distinguir `Current` de `Total`; los acumulados no representan una tasa. |
| **Current Connections** | Conexiones existentes en ese momento. | No equivale a solicitudes HTTP realizadas ni a conexiones totales históricas. |
| **Total Connections** | Número acumulado de conexiones durante el intervalo de conteo. | Puede seguir creciendo aunque `Current Connections` sea bajo. |
| **Throughput** | Volumen de información transmitida por unidad de tiempo. | No es el número de conexiones por segundo. |
| **Latency** | Tiempo de tránsito/respuesta de una operación medida. | No confundir con throughput ni con el idle timeout. |
| **Drops** | Paquetes descartados por alguna condición del sistema o interfaz. | El contador sin contexto no identifica la causa; revisar deltas y logs. |
| **Errors** | Errores de interfaz/procesamiento contabilizados. | Distinguir errores de enlace de errores de aplicación HTTP. |
| **Speed / Duplex** | Capacidad/negociación del enlace y modo de transmisión. | En VE no siempre se reproduce una discrepancia física de dúplex. |
| **CPU Utilization** | Porcentaje de tiempo de procesamiento utilizado. | El CPU global no debe confundirse con la utilización específica de TMM. |
| **TMM Memory** | Porción de memoria administrada para procesamiento TMM. | Un valor bajo no descarta presión de memoria de otros procesos. |
| **Other Memory** | Memoria usada por procesos ajenos a la reserva TMM, según la vista del sistema. | Uso elevado requiere investigar caché, procesos y swap antes de concluir falla. |
| **Swap** | Espacio de intercambio usado para páginas de memoria. | Uso de swap ≠ actividad actual intensa de intercambio. |
| **Disk Utilization** | Espacio efectivamente ocupado en los sistemas de archivos. | `show sys disk` puede mostrar tamaños de volúmenes; `df -h` muestra capacidad y uso del filesystem. |
| **Packet Capture** | Registro de paquetes para reconstruir secuencias de red y comparar ambos lados de BIG-IP. | Una captura filtrada solo por VIP puede omitir el flujo hacia backend si fue traducida la IP. |
| **tcpdump** | Herramienta para observar/capturar paquetes con filtros BPF. | En BIG-IP TMM se usa habitualmente interfaz `0.0` para observar tráfico de datos. |
| **PCAP** | Formato de archivo de captura que puede analizarse posteriormente. | Se debe registrar en qué interfaz y con qué filtro se generó. |
| **BPF Capture Filter** | Expresión aplicada durante la captura para seleccionar paquetes. | Diferente de filtros de visualización de Wireshark y del `grep` de texto. |
| **TCP Three-Way Handshake** | Establecimiento TCP mediante SYN, SYN-ACK y ACK. | Handshake TCP exitoso no confirma HTTP ni TLS exitosos. |
| **TCP RST** | Bandera de reset que finaliza o rechaza abruptamente una conexión. | Identificar cuál extremo envía RST y en qué momento del flujo. |
| **TCP FIN** | Bandera que inicia un cierre ordenado de dirección de datos TCP. | FIN es distinto de RST. |
| **TCP Retransmission** | Reenvío de segmento TCP ante pérdida o falta de confirmación. | No debe confundirse con una petición HTTP repetida por el usuario. |
| **HTTP Status Code** | Código de respuesta de aplicación (200, 404, 503, etc.). | Un 404 demuestra intercambio HTTP, pero no que se haya seleccionado el recurso correcto. |
| **Connection Refused / Timeout** | Rechazo explícito de una conexión frente a ausencia de respuesta dentro del tiempo de espera. | El síntoma distingue hipótesis de conectividad, VS y filtrado. |
| **TLS Alert / Handshake Failure** | Error del proceso de negociación TLS. | Puede existir aun cuando el TCP se establezca y el pool esté disponible. |
| **Action on Service Down** | Política sobre el tratamiento de conexiones existentes cuando un servicio de destino pasa a DOWN. | No se demuestra simplemente deshabilitando un member y probando conexiones nuevas; estudiar None, Reject/Reset y Reselect según versión/objeto. |
| **Reselect** | Intento de seleccionar otro miembro para tráfico afectado conforme a reglas compatibles. | No garantiza continuidad transparente de una sesión TCP ya establecida. |
| **Connection Table** | Tabla de flujos que administra TMM. | Permite correlacionar IP/puertos de client-side, VS, SNAT y servidor. |
| **Health Monitor False Negative** | Un destino funcional se considera DOWN debido a prueba mal diseñada o resultado no coincidente. | Confirmar petición del monitor y respuesta real del backend. |
| **Priority Group Misconfiguration** | Parámetros de prioridades/umbral que producen selección inesperada de miembros. | Distinguir problemas de elegibilidad de problemas de round-robin. |
| **Persistence Skew** | Distribución asimétrica causada por afinidad existente. | No asumir falla del balanceador solo porque Web01 recibe más solicitudes. |
| **QKView** | Paquete de diagnóstico de BIG-IP con estado y configuración para análisis/soporte. | Puede contener datos sensibles; revisar políticas antes de compartirlo. |
| **F5 iHealth** | Portal de análisis de archivos QKView y generación de diagnósticos/recomendaciones. | No sustituye la verificación directa del incidente ni garantiza detectar todas las fallas. |
| **SNMP Monitoring** | Consultas/traps para observabilidad de estado y contadores del dispositivo. | Habilitar comunidad sin receptor ni conectividad no demuestra monitoreo funcional. |
| **Root Cause Validation** | Confirmación de causa con evidencias independientes y prueba posterior a corrección. | La desaparición temporal del síntoma no basta si no se demuestra recuperación. |

### Distinciones críticas CAB5

- **SYN sin SYN-ACK vs TLS failure vs HTTP 500:** son fallas en etapas distintas del procesamiento de una solicitud.
- **RST vs timeout:** rechazo/cierre explícito frente a falta de respuesta observable.
- **Current Connections vs Total Connections:** estado instantáneo frente a contador acumulado.
- **Monitor DOWN vs servidor caído:** el problema puede estar en la comprobación del monitor.
- **Error de Client SSL vs Server SSL:** identificar en qué lado de BIG-IP se interrumpe TLS.
- **Captura client-side vs server-side:** observar ambas conexiones de Full Proxy y las traducciones SNAT/DNAT.

---

## Matriz rápida de síntomas: qué concepto recordar

| Síntoma | Conceptos que conviene revisar primero | Qué descartar antes de concluir |
|---|---|---|
| GUI inaccesible y aplicación responde | Management IP, HTTPD, ACL, Control Plane | No asumir caída de TMM. |
| VS habilitado pero offline | Pool, monitor, disponibilidad y estado administrativo | No confundir enabled con available. |
| Pool members DOWN, pero HTTP responde directamente | Receive String, Send String, timeout del monitor | No concluir servidor caído solo por color rojo. |
| Web01 recibe tráfico y Web02 casi ninguno | Persistence, Priority Group, límites, método LB | No asumir que Round Robin falla. |
| Cliente conecta por TCP pero falla HTTPS | TLS handshake, Client SSL, certificado/ciphers | No confundir un fallo TLS con indisponibilidad de pool. |
| HTTPS al VIP funciona, pero falla TLS al backend | Server SSL, compatibilidad TLS, puerto HTTPS de backend | No atribuir automáticamente el error al cliente. |
| IP cliente no aparece en servidor HTTP | SNAT, X-Forwarded-For, Full Proxy | IP de origen de TCP y cabecera HTTP son distintas. |
| Conexión termina tras período inactivo | TCP idle timeout y estado de conexión | No asumir límite de conexiones. |
| VIP rechaza, pero backends están UP | VS disabled, Reject VS, listener, selección de VS | Pool UP no demuestra VS operativo. |
| Alto Other Memory, TMM bajo | Memoria de host, caché, swap, tendencia | Un único porcentaje no determina incidente. |
| Captura no muestra backend | BPF del PCAP, traducción y segmentación client/server | Filtrar por VIP no incluye necesariamente IP del pool. |
| Configuración no se replica entre equipos | Device Trust, Device Group, ConfigSync | No confundir ConfigSync con failover. |

## Lista de autoevaluación indispensable

Marca un concepto como dominado solamente si puedes explicarlo sin consultar el glosario.

- [ ] Distinguir Management IP, Self IP y Virtual Server.
- [ ] Explicar TMM, Control Plane y Data Plane.
- [ ] Interpretar licencia, Service Check Date, ISO, boot volume y UCS.
- [ ] Distinguir trunk, tagged VLAN, untagged VLAN y ruta de salida.
- [ ] Comparar Standard, Performance L4, Forwarding, Stateless y Reject VS.
- [ ] Explicar precedencia de Virtual Servers y wildcard.
- [ ] Identificar los flujos client-side y server-side del Full Proxy.
- [ ] Distinguir SNAT AutoMap, SNAT Pool y traducción de destino.
- [ ] Diferenciar node, pool member, pool y virtual server.
- [ ] Explicar Round Robin, Least Connections, Ratio y Priority Group Activation.
- [ ] Diferenciar estado administrativo de disponibilidad monitorizada.
- [ ] Comparar persistencia por cookie y Source Address Affinity.
- [ ] Explicar Client SSL, Server SSL y el papel de SNI/certificados.
- [ ] Distinguir iRules, LTM Policies e iApps.
- [ ] Distinguir UCS, SCF, Device Trust, ConfigSync y failover.
- [ ] Reconocer diferencias entre autenticación local, LDAP, RADIUS y TACACS+.
- [ ] Identificar el log adecuado y el valor de SNMP, syslog y QKView.
- [ ] Interpretar SYN/SYN-ACK/ACK, RST, retransmisión y timeout.
- [ ] Analizar Action on Service Down sin confundirlo con deshabilitar un miembro.
- [ ] Explicar una causa raíz con al menos dos evidencias técnicas.

## Fuentes oficiales para ampliar

1. [F5 Education Services — F5 Certified Administrator BIG-IP](https://support.education.f5.com/hc/en-us/articles/37607208476059-What-is-the-F5-Certified-Administrator-BIG-IP-F5-CA-BIG-IP-certification)
2. [F5 Education — Ruta oficial F5-CA](https://education.f5.com/learning-path/view/9)
3. [F5 — Recursos oficiales de preparación](https://support.education.f5.com/hc/en-us/articles/4404740929819-Resources-for-Exam-Preparation)
4. [F5 — Índice de documentos y manuales BIG-IP](https://techdocs.f5.com/)
5. [F5 CloudDocs — Referencia TMSH](https://clouddocs.f5.com/cli/tmsh-reference/latest/)

> **Nota editorial:** este glosario es material didáctico original. Los términos y escenarios se organizaron de acuerdo con el alcance publicado para los cinco exámenes, pero F5 no garantiza preguntas de examen sobre cada término ni publica un diccionario cerrado de definiciones examinables. El contenido es conceptual y no reemplaza la documentación de la versión utilizada en producción.


# Glosario de comandos para la certificación F5 Certified Administrator, BIG-IP

**Exámenes:** F5CAB1 · F5CAB2 · F5CAB3 · F5CAB4 · F5CAB5  
**Propósito:** reconocer, interpretar y seleccionar comandos de BIG-IP para resolver situaciones de examen y problemas operativos. Complementa el glosario de comandos de los Laboratorios 1–9: incluye **comandos adicionales de estudio**, no necesariamente ejecutados en clase.  
**Alcance:** TMSH, utilidades Bash y comandos de diagnóstico. Incluye HA como referencia de examen, aunque no se haya configurado un segundo BIG-IP en el entorno.  
**Advertencia:** F5 no publica una lista oficial cerrada de comandos exigidos ni asegura que aparezcan comandos específicos en el examen; los exámenes evalúan objetivos y decisiones operativas, no memorización de sintaxis de una versión concreta.

## 0. Cómo usar este glosario

- **TMSH**: la sesión SSH del pod inicia en un prompt terminado en `(tmos)#`. Ejecuta `show ltm virtual`, **no** `tmsh show ltm virtual` desde ese prompt.
- **Bash del BIG-IP**: entra desde TMSH con `run util bash`; vuelve con `exit`. Desde Bash se usa `tmsh show ltm virtual` si se desea invocar TMSH.
- **Ubuntu**: equipo cliente `10.1.3.4`, desde el cual se prueba tráfico hacia VIPs `10.1.3.200`.
- **Tipo**: **L** = lectura; **DIA** = diagnóstico; **C** = cambia configuración; **RIESGO** = puede interrumpir tráfico, accesos, o reiniciar dispositivos.
- **Variables**: `<vs>`, `<pool>`, `<monitor>`, `<perfil>`, `<miembro>`, `<volumen>` son **marcadores**, no texto a ejecutar literalmente. Confirma nombres primero con `list`.
- **Pod de referencia:** Management `10.1.1.200`; External Self IP `10.1.3.200`; Internal Self IP/SNAT `10.1.2.200`; backends `10.1.2.4:80` y `10.1.2.5:80`. Los VS existentes utilizan puertos diferentes; nunca asumas que `:80` o `:443` están libres.
- **Comando de consulta versus salida:** `list` enseña **configuración**; `show` enseña **estado y estadísticas**. Un objeto configurado como *enabled* puede estar *unavailable* por el monitor.

## F5CAB1 — Instalación, configuración inicial y actualización

### Identidad, acceso, licencia y aprovisionamiento

| Comando (TMSH) | Tipo | Qué debe saber interpretar |
|---|---|---|
| `show sys version` | L | Versión/build activo; no asumir la versión indicada en un manual antiguo. |
| `show sys hardware` | L | Plataforma, hardware/VE e información de recursos. |
| `show sys license` | L | Licencia, características habilitadas y vigencia de soporte según campos disponibles. |
| `show sys license detail` | L | Información de licencia ampliada cuando la sintaxis está disponible. |
| `list sys provision` | L | Qué módulos tienen un nivel de aprovisionamiento configurado. |
| `show sys provision` | L | Estado/recursos de aprovisionamiento. **Licenciado no equivale a aprovisionado**. |
| `list sys global-settings` | L | Nombre del host y ajustes globales disponibles. |
| `list sys management-ip` | L | IP de administración. |
| `list sys management-route` | L | Gateway/rutas de administración; distintas de las rutas TMM. |
| `list sys httpd` | L | Configuración de la interfaz web de administración. |
| `list sys sshd` | L | Configuración del acceso SSH. |
| `list sys httpd allow` | L | Redes autorizadas para TMUI; un filtro erróneo puede bloquear al administrador. |
| `list sys sshd allow` | L | Redes autorizadas a SSH. |
| `list auth password-policy` | L | Política de contraseñas. |
| `list auth user` | L | Usuarios locales, roles y particiones. |
| `list auth source` | L | Fuente de autenticación configurada: local u otra, según plataforma. |
| `list net self` | L | Self IP y `allow-service` (Port Lockdown), diferente de la IP de Management. |

### Software, imágenes, boot volumes y respaldo

| Comando | Tipo | Interpretación / precaución |
|---|---|---|
| `show sys software status` | L | Estado de volúmenes instalados y software disponible. |
| `list sys software volume` | L | Versiones y volúmenes de arranque configurados. |
| `list sys software image` | L | Imágenes ISO conocidas por el sistema. |
| `list sys software hotfix` | L | Hotfix disponibles si el sistema utiliza esa modalidad. |
| `list sys provision ltm` | L | Aprovisionamiento particular de LTM. |
| `save sys config` | C | Persiste cambios hechos en configuración. No reemplaza un UCS. |
| `save sys ucs nombre_preupgrade.ucs` | C | Crea un respaldo UCS; puede contener secretos, certificados y configuración. |
| `list sys ucs` | L | Archivos UCS conocidos por TMSH. |
| `load sys ucs nombre_preupgrade.ucs` | **RIESGO** | Restauración; puede sobrescribir objetos y afectar tráfico. **Solo estudiar el procedimiento; no ejecutar en el pod compartido.** |
| `reboot` | **RIESGO** | Reinicia BIG-IP (requiere acceso/permisos y sintaxis apropiados para el contexto); no ejecutar en prácticas compartidas. |

**Situación de examen:** un volumen tiene la versión anterior y otro la versión nueva. Primero identifica **qué volumen está activo**, la integridad del respaldo, espacio, licencia y compatibilidad. No confundas *imagen almacenada*, *volumen instalado* y *volumen en ejecución*. La actualización de software no se demuestra simplemente importando una ISO.

## F5CAB2 — Conceptos del Data Plane y selección del tráfico

### Interfaces, VLAN, troncales y rutas

| Comando (TMSH) | Tipo | Qué responder en el examen |
|---|---|---|
| `list net interface` | L | Propiedades de interfaces físicas o virtuales. |
| `show net interface` | L | Estado, bytes, paquetes, errores y drops. |
| `show net interface 1.1` | L | Contadores de la interfaz External del pod. |
| `show net interface 1.2` | L | Contadores de Internal. |
| `list net vlan` | L | Interfaces miembro de VLAN; tagged frente a untagged. |
| `list net vlan External` | L | VLAN usada por los clientes. |
| `list net vlan Internal` | L | VLAN de servidores. |
| `list net trunk` | L | Trunks y miembros; distingue agregación LACP de una VLAN. |
| `show net trunk` | L | Estado/estadísticas de trunks existentes, si los hay. |
| `list net self` | L | Dirección, VLAN y port-lockdown de Self IPs. |
| `list net route` | L | Rutas de TMM configuradas. |
| `show net route` | L | Vista de rutas/estado (según versión). |
| `list net route-domain` | L | Route domains y sus identificadores. |
| `list net arp` | L | Vecinos ARP declarados/configurados. |
| `show net arp` | L | Entradas ARP aprendidas y su resolución. |

### Virtual Servers, coincidencia y SNAT

| Comando (TMSH) | Tipo | Qué significa |
|---|---|---|
| `list ltm virtual` | L | Todos los VS y sus destinos/puertos/tipos/profiles. |
| `list ltm virtual <vs>` | L | Configuración exacta: `destination`, `ip-protocol`, `pool`, `profiles`, `vlans`, `source-address-translation`. |
| `show ltm virtual <vs>` | L | Estado y contadores del VS. |
| `list ltm virtual-address` | L | Objetos Virtual Address, ARP y accesibilidad de VIPs. |
| `list ltm snat` | L | Objetos SNAT; no equivale a SNAT AutoMap ligado a un VS. |
| `list ltm snatpool` | L | Pools de direcciones de traducción. |
| `show sys connection cs-server-addr 10.1.3.200 cs-server-port 8083` | DIA | Conexiones del lado cliente a un VIP/puerto. |
| `show sys connection cs-client-addr 10.1.3.4` | DIA | Conexiones cuyo origen client-side es Ubuntu. |
| `show sys connection ss-server-addr 10.1.2.4` | DIA | Conexiones con backend Web01 en el lado servidor. |

**Interpretación esencial:** en un Standard VS con SNAT AutoMap, puede existir una conexión client-side `10.1.3.4 → 10.1.3.200:8083` y otra server-side `10.1.2.200 → 10.1.2.4:80`. **No son la misma conexión TCP**. La elección entre VS específico, wildcard y otras coincidencias requiere revisar dirección, puerto, origen, protocolo y las reglas de precedencia.

## F5CAB3 — Configuración del Data Plane

### Pools, Nodes, Health Monitors y prioridad

| Comando (TMSH) | Tipo | Para qué se usa |
|---|---|---|
| `list ltm pool` | L | Pools existentes, método de balanceo y monitor. |
| `list ltm pool <pool>` | L | Configuración del pool y sus miembros. |
| `show ltm pool <pool>` | L | Disponibilidad y estadísticas del pool. |
| `show ltm pool <pool> members` | L | Disponibilidad, conexiones y contadores por miembro. |
| `list ltm pool <pool> members` | L | Connection limits, prioridades y estado administrativo configurado. |
| `list ltm pool <pool> monitor` | L | Monitor asociado y combinación de monitores si aplica. |
| `list ltm pool <pool> min-active-members` | L | Umbral de Priority Group Activation (PGA). |
| `list ltm pool <pool> action-on-service-down` | L | Política para conexiones **existentes** cuando el servicio cae. |
| `list ltm node` | L | Nodos definidos; un node se distingue de un pool member con puerto. |
| `show ltm node 10.1.2.4` | L | Estado del nodo. |
| `list ltm monitor http` | L | Monitores HTTP y sus ajustes. |
| `list ltm monitor http <monitor>` | L | `send`, `recv`, `interval`, `timeout`. |
| `list ltm monitor tcp` | L | Monitores TCP existentes. |
| `modify ltm pool <pool> members modify { 10.1.2.4:80 { session user-disabled } }` | C | Deshabilita temporalmente nuevas selecciones del miembro; **no es lo mismo que un monitor fallido**. |
| `modify ltm pool <pool> members modify { 10.1.2.4:80 { session user-enabled } }` | C | Restaura el estado administrativo para el miembro. |
| `modify ltm pool <pool> members modify { 10.1.2.4:80 { connection-limit 1 } }` | C | Establece límite de concurrencia del miembro. Requiere prueba concurrente para apreciarlo. |
| `modify ltm pool <pool> members modify { 10.1.2.4:80 { connection-limit 0 } }` | C | Quita el límite (0 normalmente significa sin límite). |

**Situación de examen:** `State: enabled` + `Availability: offline` sugiere que el objeto está administrativamente habilitado, pero **no disponible** por monitor u otra dependencia. No cambies indiscriminadamente el monitor global; un monitor compartido puede afectar varios pools.

### Perfiles, iRules, iApps y persistencia

| Comando (TMSH) | Tipo | Interpretación |
|---|---|---|
| `list ltm virtual <vs> profiles` | L | Perfiles asociados: TCP, HTTP, Client SSL, Server SSL, etc. |
| `list ltm profile tcp` | L | Perfiles TCP disponibles. |
| `list ltm profile tcp <perfil>` | L | Valores como `idle-timeout`. |
| `list ltm profile http` | L | Perfiles HTTP disponibles; importantes para funciones de capa 7. |
| `list ltm profile client-ssl` | L | TLS del lado del **cliente** del BIG-IP. |
| `list ltm profile server-ssl` | L | TLS del lado del **backend**; por sí solo no habilita HTTPS en el servidor. |
| `list ltm persistence cookie` | L | Perfiles de persistencia de cookies. |
| `list ltm persistence source-addr` | L | Perfiles de persistencia por IP origen. |
| `list ltm virtual <vs> persist` | L | Persistencia aplicada al Virtual Server. |
| `show ltm persistence persist-records` | DIA | Tabla dinámica de persistencia, principalmente relevante para modos que mantienen registros; una cookie insertada en el cliente no garantiza una fila persist-records. |
| `list ltm rule` | L | iRules existentes. |
| `list ltm virtual <vs> rules` | L | iRules adjuntas al VS. |
| `list sys application service` | L | Servicios de aplicación, incluidos los generados desde plantillas/iApps cuando existen. |
| `list sys application template` | L | Plantillas de aplicaciones iApp disponibles, cuando existan. |

**Situaciones clave:**

- Si se elimina el perfil HTTP pero una función necesita interpretar cabeceras/cookies, identifica la dependencia de L7.
- El perfil **Client SSL** termina la sesión TLS del cliente; **Server SSL** crea TLS hacia el servidor. Verifica que Web01/Web02 puedan negociar TLS antes de asociar Server SSL a un pool con puerto 80.
- **Persistencia ≠ algoritmo de balanceo:** la primera selección puede obedecer a Round Robin y las siguientes dirigirse al mismo miembro por cookie.

## F5CAB4 — Administración del Control Plane

### Cuentas, servicios, registros y archivos UCS

| Comando | Contexto / tipo | Para qué sirve |
|---|---|---|
| `list auth user` | TMSH · L | Roles y particiones de usuarios locales. |
| `list auth source` | TMSH · L | Fuente de autenticación. |
| `list auth remote-role` | TMSH · L | Asignaciones remotas de roles/grupos, si están configuradas. |
| `list sys ntp` | TMSH · L | Servidores NTP. |
| `list sys dns` | TMSH · L | Resolutores DNS. |
| `list sys snmp` | TMSH · L | Configuración SNMP. |
| `list sys syslog` | TMSH · L | Parámetros de logging remoto. |
| `show sys service` | TMSH · L | Estado de servicios del dispositivo. |
| `show sys clock` | TMSH · L | Hora del BIG-IP, relevante para certificados y correlación de logs. |
| `run util bash -c "tail -n 60 /var/log/ltm"` | TMSH→Bash · DIA | Eventos LTM y monitores. |
| `run util bash -c "tail -n 60 /var/log/audit"` | TMSH→Bash · DIA | Auditoría de cambios, según registros habilitados. |
| `run util bash -c "tail -n 60 /var/log/secure"` | TMSH→Bash · DIA | Eventos de autenticación del sistema. |
| `run util bash -c "df -h"` | TMSH→Bash · L | **Uso real del disco**, no tamaño de volúmenes lógicos. |
| `save sys ucs respaldo.ucs` | TMSH · C | Genera respaldo. |
| `load sys ucs respaldo.ucs` | TMSH · RIESGO | Restaura configuración; no ejecutar en un pod compartido. |
| `save sys config` | TMSH · C | Guarda cambios en configuración activa. |

### HA y ConfigSync: reconocer comandos, no ejecutarlos en el pod standalone

| Comando (TMSH) | Tipo | Qué distingue el alumno |
|---|---|---|
| `show sys failover` | L | Rol local Active/Standby reportado, aunque sea standalone. |
| `show cm sync-status` | L | Estado ConfigSync/Standalone. |
| `list cm device` | L | Dispositivos conocidos por el sistema. |
| `list cm device-group` | L | Grupos de sincronización. |
| `list cm traffic-group` | L | Traffic groups y propietarios. |
| `run sys failover standby` | **RIESGO** | Conmutación administrativa; solo en una pareja HA real y con procedimiento. |

**Error de examen frecuente:** *Active* no prueba por sí solo que exista un segundo BIG-IP operativo. **Failover ≠ ConfigSync** y ambos tienen prerequisitos de red/configuración.

## F5CAB5 — Soporte, estadísticas y troubleshooting

### Recursos, TMM, tráfico y sesiones

| Comando | Contexto / tipo | Qué observar |
|---|---|---|
| `show sys cpu` | TMSH · L | Utilización global y por núcleo; observar periodos y `Idle`, `Iowait`, `Stolen`. |
| `show sys memory` | TMSH · L | `TMM Memory Used`, `Other Memory Used`, `Swap Used`; correlacionar, sin concluir saturación solo por un número. |
| `show sys performance all-stats` | TMSH · L | Métricas generales e históricas disponibles. |
| `show net interface 1.1` | TMSH · L | `Status`, `Pkts`, `Drops`, `Errs` de External. |
| `show net interface 1.2` | TMSH · L | Contadores de Internal. |
| `show ltm virtual <vs>` | TMSH · L | Disponibilidad, conexiones actuales y estadísticas. |
| `show ltm pool <pool> members` | TMSH · L | Cuál miembro recibe tráfico y si está up/down. |
| `show sys connection` | TMSH · DIA | Tabla de conexiones; muy extensa si no filtras. |
| `show sys connection cs-client-addr 10.1.3.4` | TMSH · DIA | Conexiones desde el cliente Ubuntu. |
| `show sys connection cs-server-addr 10.1.3.200 cs-server-port 8083` | TMSH · DIA | Conexiones hacia el Virtual Server específico. |
| `show sys connection ss-server-addr 10.1.2.5` | TMSH · DIA | Conexiones hacia Web02. |
| `run util bash -c "df -h"` | TMSH→Bash · DIA | Espacio libre y particiones cercanas a llenarse. |
| `run util bash -c "top -b -n 1 | head -n 25"` | TMSH→Bash · DIA | Procesos del host; **no** sustituye estadísticas TMM. |
| `run util bash -c "tail -n 80 /var/log/ltm"` | TMSH→Bash · DIA | Transiciones de monitores y eventos relevantes. |

### tcpdump, trazas y pruebas desde Ubuntu

| Comando | Dónde | Qué evidencia aporta |
|---|---|---|
| `tcpdump -nni 0.0 -s 0 'host 10.1.3.4 and tcp port 8083'` | BIG-IP Bash · DIA | Tráfico client-side del VS de ejemplo. |
| `tcpdump -nni 0.0 -s 0 'host 10.1.2.4 or host 10.1.2.5'` | BIG-IP Bash · DIA | Tráfico del lado servidor; puede incluir monitores. |
| `tcpdump -nni 0.0 -s 0 -w /var/tmp/examen_f5.pcap 'tcp port 8083'` | BIG-IP Bash · DIA | Captura en PCAP; parar con Ctrl+C; no dejar capturas grandes indefinidamente. |
| `tcpdump -nn -r /var/tmp/examen_f5.pcap` | BIG-IP Bash · DIA | Muestra direcciones y puertos de PCAP guardado. |
| `tcpdump -nn -A -r /var/tmp/examen_f5.pcap` | BIG-IP Bash · DIA | Añade contenido ASCII cuando no está cifrado. |
| `tcpdump -nn -A -r /var/tmp/examen_f5.pcap 2>/dev/null \| grep -B 8 -A 12 'lab08='` | BIG-IP Bash · DIA | Contexto antes/después de URL de prueba; `grep` puede recortar si la cabecera IP queda fuera de 8 líneas. |
| `curl --noproxy '*' -v 'http://10.1.3.200:8083/?prueba=1'` | Ubuntu Bash · DIA | Conectividad TCP, petición HTTP, cabeceras y respuesta. |
| `curl --noproxy '*' -s -o /dev/null -w '%{http_code}\n' 'http://10.1.3.200:8083/'` | Ubuntu Bash · DIA | Solo código HTTP; `000` indica ausencia de respuesta HTTP, no un código enviado por BIG-IP. |
| `curl --noproxy '*' -k -I 'https://10.1.3.200:8443/'` | Ubuntu Bash · DIA | Cabeceras HTTPS; `-k` omite validación del certificado, **solo en laboratorio**. |
| `curl --noproxy '*' -D - -o /dev/null 'http://10.1.3.200:8083/'` | Ubuntu Bash · DIA | Cabeceras HTTP, incluidas `Set-Cookie` si aplica. |
| `curl --noproxy '*' -c /tmp/cookies.txt -b /tmp/cookies.txt 'http://10.1.3.200:8083/'` | Ubuntu Bash · DIA | Conserva y reutiliza cookies; no se debe inferir persistencia desde peticiones que no reenvían cookie. |
| `openssl s_client -connect 10.1.3.200:8443 -servername ejemplo.local` | Ubuntu Bash · DIA | Muestra negociación TLS y certificado si la utilidad existe en el cliente. |

### QKView, logs, SNMP y toma de decisiones

| Comando | Contexto / tipo | Qué aprender |
|---|---|---|
| `run util bash -c "qkview -f /var/tmp/examen_f5.qkview"` | TMSH→Bash · DIA | Genera QKView; la sintaxis/ubicación puede depender de versión. Puede consumir tiempo/disco. |
| `run util bash -c "ls -lh /var/tmp/examen_f5.qkview"` | TMSH→Bash · L | Comprueba que el archivo existe y su tamaño; el tamaño no certifica integridad. |
| `list sys snmp` | TMSH · L | Comunidad, acceso y allowed addresses; **no supone que haya un NMS operativo**. |
| `show sys service` | TMSH · L | Servicios de plano de control. |
| `list ltm pool <pool> action-on-service-down` | TMSH · L | Distingue reacción a caída de un miembro de selección de nuevas conexiones. |
| `list ltm virtual <vs> profiles` | TMSH · L | Comprueba si perfil HTTP/TCP/SSL está presente. |
| `list ltm pool <pool> min-active-members` | TMSH · L | Umbral PGA. |
| `list ltm pool <pool> members` | TMSH · L | Prioridad y límite por miembro. |

## Situaciones de examen: elegir el primer comando correcto

| Escenario | Primer comando aconsejable | Segunda comprobación | Decisión que permite tomar |
|---|---|---|---|
| El VIP no responde a HTTP | `show ltm virtual <vs>` | `show ltm pool <pool> members` | Separar VS deshabilitado, pool sin miembros y problema de conectividad. |
| El backend contesta HTTP 200 pero BIG-IP lo declara down | `list ltm pool <pool> monitor` | `list ltm monitor http <monitor>` | Verificar `send`/`recv`, intervalo/timeout, sin culpar automáticamente al servidor. |
| Round Robin no distribuye como esperabas | `list ltm virtual <vs> persist` | `list ltm pool <pool> members` | Correlacionar persistencia, Priority Groups, limits y disponibilidad. |
| El cliente recibe reset tras una caída | `list ltm pool <pool> action-on-service-down` | `show sys connection ...` | Analizar conexiones existentes, política de caída y qué extremo resetea. |
| HTTPS desde cliente funciona, al backend no llega TLS | `list ltm virtual <vs> profiles` | `list ltm profile server-ssl` | Comprobar Server SSL y que el backend realmente hable TLS. |
| Un VS usa SNAT pero el backend ve una IP interna | `list ltm virtual <vs>` | `show sys connection cs-server-addr ...` | Comprobar AutoMap y diferencia client/server-side. |
| Interfaz UP pero se reporta pérdida de paquetes | `show net interface 1.1` | `tcpdump` filtrado | Identificar si crecen drops/errores o si el problema es de una capa distinta. |
| Falla el login a TMUI pero SSH funciona | `list sys httpd allow` | `list auth source` | Diferenciar restricción de red y autenticación. |
| El equipo tiene una ISO pero ejecuta versión anterior | `show sys software status` | `list sys software volume` | Distinguir imagen descargada, volumen instalado y volumen activo. |
| CPU host baja, usuarios reportan lentitud | `show sys cpu` | `show sys performance all-stats` y estadísticas por VS | Evitar atribuir todo a CPU; verificar TMM, errores y backend. |
| `Other Memory Used` aparece cerca del 100 % | `show sys memory` | `run util bash -c "free -m"` | No confundir memoria asignada/caché con presión actual; correlacionar swap y actividad. |
| Se necesita recuperar la configuración | `list sys ucs` | `show sys software status` | Evaluar compatibilidad, plan de restauración y riesgos antes de cargar UCS. |
| Dos BIG-IP aparecen Active (escenario de HA) | `show cm sync-status` | `list cm device-group` y `show sys failover` | Identificar estado de sincronización y failover; no ejecutar conmutaciones a ciegas. |
| Necesitas evidencia para soporte F5 | `run util bash -c "qkview -f /var/tmp/examen_f5.qkview"` | `run util bash -c "ls -lh /var/tmp/examen_f5.qkview"` | Generar archivo diagnóstico, protegerlo y analizarlo en iHealth si hay acceso. |

## Errores de interpretación que conviene evitar

1. **`show` no es `list`:** estado operativo no es lo mismo que configuración.
2. **Enabled no es Available:** un monitor puede dejar un member offline aunque esté habilitado.
3. **Pool member no es node:** el miembro incluye dirección y puerto.
4. **`connection-limit` no es `rate-limit`:** uno regula concurrencia, el otro nuevas conexiones por unidad de tiempo según configuración.
5. **`cs-...` no es `ss-...`:** las cuatro direcciones de un Full Proxy son un tema central para leer la tabla de conexiones.
6. **`tcpdump -A` no descifra HTTPS:** verás TLS del lado cliente, pero puedes observar HTTP claro del lado servidor si allí no hay Server SSL.
7. **Port Lockdown de Self IP no es firewall de la IP Management:** tienen roles y controles distintos.
8. **Una cookie de persistencia no tiene por qué aparecer en `persist-records`:** revisa el perfil y las cabeceras reales del navegador/cliente.
9. **No crear un monitor fallido en un objeto compartido:** podría dejar varios servicios down.
10. **No ejecutar `load sys ucs`, `reboot`, cambios de license ni comandos de failover en un pod compartido** sin ventana/plan de recuperación.
11. **`show sys disk` no es `df -h`:** consulta el uso real de espacio mediante `df -h` desde Bash.
12. **PGA no significa necesariamente que todos los miembros queden inactivos si el umbral no se cumple:** el comportamiento depende de miembros disponibles y grupos de prioridad; verifica estadísticas y documentación antes de concluir.

## Referencias oficiales

- Certificación F5-CA BIG-IP: https://support.education.f5.com/hc/en-us/articles/37607208476059-What-is-the-F5-Certified-Administrator-BIG-IP-F5-CA-BIG-IP-certification
- Recursos oficiales de preparación y blueprint: https://support.education.f5.com/hc/en-us/articles/4404740929819-Resources-for-Exam-Preparation
- Referencia TMSH: https://clouddocs.f5.com/cli/tmsh-reference/latest/
- Manual de conexiones `sys connection`: https://clouddocs.f5.com/cli/tmsh-reference/latest/modules/sys/sys_connection.html
- Documentación de software/volúmenes: https://clouddocs.f5.com/cli/tmsh-reference/latest/modules/sys/

> **Nota de precisión:** los ejemplos representan sintaxis TMSH de estudio. Algunas opciones, objetos y funciones varían por versión, licenciamiento y configuración. Contrasta siempre el comando con `help <módulo>`/referencia de la versión antes de modificar un sistema real.
