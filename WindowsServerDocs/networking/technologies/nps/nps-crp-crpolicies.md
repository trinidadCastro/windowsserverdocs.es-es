---
title: Directivas de la solicitud de conexión
description: Este tema proporciona una visión general de las directivas de solicitud de conexión de servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4ec45e0c-6b37-4dfb-8158-5f40677b0157
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 496ba87597b0be6cad1109269d2f72f52de017f1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="connection-request-policies"></a>Directivas de la solicitud de conexión

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre cómo usar las directivas de solicitud de conexión de NPS para configurar el servidor NPS como un servidor RADIUS, un proxy RADIUS o ambos.

>[!NOTE]
>Además de este tema, la siguiente documentación de directiva de solicitud de conexión está disponible.
> - [Configurar las directivas de solicitud de conexión](nps-crp-configure.md)
> - [Configurar los grupos de servidores RADIUS remotos](nps-crp-rrsg-configure.md)

Directivas de solicitud de conexión son conjuntos de condiciones y opciones de configuración que permiten a los administradores de red designar qué servidores de servicio de autenticación remota telefónica de usuario (RADIUS) realizan la autenticación y autorización de solicitudes de conexión que el servidor que ejecuta el servidor de directivas de redes (NPS) recibe de los clientes RADIUS. Las directivas de la solicitud de conexión pueden configurarse para designar qué servidores RADIUS se usan para las cuentas RADIUS.

Puedes crear conexión solicitud directivas para que algunos mensajes de solicitud RADIUS enviados desde clientes RADIUS se procesen localmente (NPS se usa como un servidor RADIUS) y otros tipos de mensajes se reenvíen a otro servidor RADIUS (NPS se usa como un proxy de radio).

Con las directivas de la solicitud de conexión, puedes usar NPS como un servidor RADIUS o como un proxy de radio, en función de factores como la siguiente: 

- La hora del día y el día de la semana
- El nombre de dominio en la solicitud de conexión
- El tipo de conexión que se solicita
- La dirección IP del cliente RADIUS

Mensajes de solicitud de acceso RADIUS son procesa o reenvía NPS solo si la configuración del mensaje entrante coincide con al menos una de las directivas de la solicitud de conexión configuradas en el servidor NPS. 

Si coincide con la configuración de directiva y la directiva requiere que el servidor NPS procesar el mensaje, NPS actúa como un servidor RADIUS, autenticar y autorizar la solicitud de conexión. Si coincide con la configuración de directiva y la directiva requiere que el servidor NPS reenvía el mensaje, NPS actúa como proxy RADIUS y reenvía la solicitud de conexión a un servidor RADIUS remoto para su procesamiento.

Si la configuración de un mensaje de solicitud de acceso RADIUS entrante no coincide con al menos una de las directivas de la solicitud de conexión, se envía un mensaje de denegación de acceso al cliente RADIUS y el usuario o el equipo que intenta conectarse a la red se deniega el acceso.

## <a name="configuration-examples"></a>Ejemplos de configuración

Los ejemplos de configuración siguientes muestran cómo puedes usar directivas de solicitud de conexión.

### <a name="nps-as-a-radius-server"></a>NPS como un servidor de radio

La directiva de solicitud de conexión predeterminada es la única directiva configurada. En este ejemplo, NPS está configurado como un servidor RADIUS y todas las solicitudes de conexión se procesan por el servidor NPS local. El servidor NPS puede autenticar y autorizar usuarios cuyas cuentas se están en el dominio del dominio del servidor NPS y en dominios de confianza.


### <a name="nps-as-a-radius-proxy"></a>NPS como un proxy de radio

Se elimina la directiva de solicitud de conexión predeterminada y se crean dos nuevas directivas de solicitud de conexión para reenviar solicitudes a dos dominios distintos. En este ejemplo, NPS está configurado como un proxy RADIUS. NPS no procesa ninguna solicitud de conexión en el servidor local. En su lugar, reenvía solicitudes de conexión a NPS u otros servidores RADIUS que están configurados como miembros de grupos de servidores RADIUS remotos.


### <a name="nps-as-both-radius-server-and-radius-proxy"></a>NPS RADIUS server y proxy RADIUS

Además de la directiva de solicitud de conexión de forma predeterminada, se crea una nueva directiva de solicitud de conexión que reenvía solicitudes de conexión a NPS u otro servidor RADIUS en un dominio de confianza. En este ejemplo, la directiva de proxy aparece en primer lugar en la lista ordenada de directivas. Si la solicitud de conexión coincide con la directiva de proxy, la solicitud de conexión se reenvía al servidor de radio en el grupo de servidores RADIUS remotos. Si la solicitud de conexión no coincide con la directiva de proxy pero coincide con la directiva predeterminada de solicitud de conexión, NPS procesa la solicitud de conexión en el servidor local. Si la solicitud de conexión no coincide con cualquiera de ellas, se descarta.

### <a name="nps-as-radius-server-with-remote-accounting-servers"></a>NPS como servidor RADIUS con servidores remotos de contabilidad

En este ejemplo, el servidor NPS local no está configurado para administrar cuentas y la directiva predeterminada de solicitud de conexión se revisa para que se envían mensajes de cuentas RADIUS a NPS u otro servidor RADIUS en un grupo de servidores RADIUS remotos. Aunque se envían mensajes de cuentas, no se envían mensajes de autenticación y autorización y el servidor NPS local realiza las siguientes funciones para el dominio local y todos los dominios de confianza.


### <a name="nps-with-remote-radius-to-windows-user-mapping"></a>NPS con RADIUS remoto para la asignación de usuario de Windows

En este ejemplo, NPS actúa como servidor RADIUS, como un proxy de radio para cada solicitud de conexión en particular reenviando la solicitud de autenticación a un servidor remoto RADIUS mientras usando una cuenta de usuario local de Windows en la autorización. Esta configuración se implementa mediante la configuración de la radio remoto al atributo de asignación de usuarios de Windows como una condición de la directiva de solicitud de conexión. (Además, una cuenta de usuario se debe crear local que tenga el mismo nombre que la cuenta de usuario remoto con los que la autenticación se realiza mediante el servidor remoto RADIUS.)

## <a name="connection-request-policy-conditions"></a>Condiciones de directiva de solicitud de conexión

Condiciones de directiva de solicitud de conexión son uno o varios atributos RADIUS son en comparación con los atributos del mensaje de solicitud de acceso RADIUS entrante. Si hay varias condiciones, a continuación, todas las condiciones en la conexión de mensaje de solicitud y en la solicitud de conexión directiva debe coincidir en orden de la directiva se aplique NPS.

Siguiente es los atributos de la condición que se pueden configurar en las directivas de solicitud de conexión.

### <a name="connection-properties-attribute-group"></a>Grupo de atributos de propiedades de conexión

El grupo de atributos de propiedades de conexión contiene los siguientes atributos.

- **Enmarcada protocolo**. Se usa para designar el tipo de tramas de los paquetes de entrada. Algunos ejemplos son Protocolo punto a punto (PPP), el protocolo de Internet serie de línea (Resbalarse), relé de trama y X.25.
- **Tipo de servicio**. Se usa para designar el tipo de servicio que se solicita. Algunos ejemplos incluyen enmarcada (por ejemplo, las conexiones PPP) y el inicio de sesión (por ejemplo, las conexiones Telnet). Para obtener más información acerca de los tipos de servicio RADIUS, vea RFC 2865, "Servicio autenticación remota telefónica de usuario (RADIUS)."
- **Tipo de túnel**. Se usa para designar el tipo de túnel que se crea el cliente. Tipos de túnel incluyen el protocolo de túnel punto a punto (PPTP) y el protocolo de túnel de capa dos (L2TP).

### <a name="day-and-time-restrictions-attribute-group"></a>Grupo de atributos de restricciones de día y hora 

El grupo de atributos de restricciones de día y hora contiene el atributo de restricciones de día y hora. Con este atributo, puedes designar el día de la semana y la hora del día del intento de conexión. El día y hora es en relación con el día y hora del servidor NPS.

### <a name="gateway-attribute-group"></a>Grupo de atributos de puerta de enlace

El grupo de atributos de puerta de enlace contiene los siguientes atributos.

- **Identificador de estación llamada**. Se usa para designar el número de teléfono del servidor de acceso de red. Este atributo es una cadena de caracteres. Puedes usar la sintaxis de coincidencia para especificar los códigos de área.
- **Identificador de NAS**. Se usa para designar el nombre del servidor de acceso de red. Este atributo es una cadena de caracteres. Puedes usar la sintaxis de coincidencia para especificar identificadores NAS.
- **Dirección IPv4 NAS**. Se usa para designar el protocolo de Internet versión 4 (IPv4) la dirección del servidor de acceso de red (el cliente RADIUS). Este atributo es una cadena de caracteres. Puedes usar la sintaxis de coincidencia para especificar redes IP.
- **Dirección NAS IPv6**. Se usa para designar el protocolo de Internet versión 6 (IPv6) la dirección del servidor de acceso de red (el cliente RADIUS). Este atributo es una cadena de caracteres. Puedes usar la sintaxis de coincidencia para especificar redes IP.
- **Tipo de puerto NAS**. Se usa para designar el tipo de medio utilizado por el cliente de acceso. Algunos ejemplos son las líneas telefónicas analógicas (conocida como asincrónica), red (RDSI), túneles o redes privadas virtuales (VPN), IEEE 802.11 inalámbrico, y los conmutadores Ethernet.

### <a name="machine-identity-attribute-group"></a>Grupo de atributos de identidad de máquina

El grupo de atributos de identidad del equipo contiene el atributo de identidad del equipo. Con este atributo, puedes especificar el método con el que los clientes se identifican en la directiva.

### <a name="radius-client-properties-attribute-group"></a>Grupo de atributos de propiedades de cliente RADIUS

El grupo de atributos de propiedades del cliente RADIUS contiene los siguientes atributos.

- **Llamar a la ID. del**. Se usa para designar el número de teléfono usado por el llamador (el cliente de acceso). Este atributo es una cadena de caracteres. Puedes usar la sintaxis de coincidencia para especificar los códigos de área.
- **Nombre descriptivo del cliente**. Se usa para designar el nombre del equipo cliente RADIUS que solicita la autenticación. Este atributo es una cadena de caracteres. Puedes usar la sintaxis de coincidencia para especificar nombres de cliente.
- **Dirección IPv4 del cliente**. Se usa para designar la dirección IPv4 del servidor de acceso de red (el cliente RADIUS). Este atributo es una cadena de caracteres. Puedes usar la sintaxis de coincidencia para especificar redes IP.
- **Dirección IPv6 del cliente**. Se usa para designar la dirección IPv6 del servidor de acceso de red (el cliente RADIUS). Este atributo es una cadena de caracteres. Puedes usar la sintaxis de coincidencia para especificar redes IP.
- **Proveedor del cliente**. Se usa para designar el proveedor del servidor de acceso de red que solicita la autenticación. Un equipo que ejecuta el servicio de enrutamiento y acceso remoto es el fabricante de Microsoft NAS. Puedes usar este atributo para configurar directivas independientes para diferentes fabricantes NAS. Este atributo es una cadena de caracteres. Puedes usar la sintaxis de coincidencia.

### <a name="user-name-attribute-group"></a>Grupo de atributos de nombre de usuario

El grupo de atributos de nombre de usuario contiene el atributo de nombre de usuario. Con este atributo, puedes designar el nombre de usuario, o una parte del nombre de usuario, que debe coincidir con el nombre de usuario proporcionado por el cliente de acceso en el mensaje RADIUS. Este atributo es una cadena de caracteres que normalmente contiene un nombre de dominio Kerberos y un nombre de cuenta de usuario. Puedes usar la sintaxis de coincidencia para especificar nombres de usuario.

## <a name="connection-request-policy-settings"></a>Configuración de directiva de solicitud de conexión

Configuración de directiva de solicitud de conexión es un conjunto de propiedades que se aplican a un mensaje entrante de RADIUS. Configuración consta de los siguientes grupos de propiedades.

- Autenticación
- Administración de cuentas
- Manipulación de atributos
- Solicitud de desvío de llamadas
- Avanzada

Las secciones siguientes proporcionan información adicional acerca de estas opciones de configuración.

### <a name="authentication"></a>Autenticación

Con esta opción, puedes invalidar la configuración de autenticación que está configurada en todas las directivas de red y puede designar los métodos de autenticación y tipos que se necesitan para conectarse a la red.

>[!IMPORTANT]
>Si configuras un método de autenticación en la directiva de solicitud de conexión que es menos seguro que el método de autenticación que se configura en la directiva de red, se reemplaza el método de autenticación más seguro que se configura en la directiva de red. Por ejemplo, si tienes una directiva de red que requiere el uso de la versión 2 de protegido Extensible autenticación Microsoft de protocolo de protocolo de autenticación \ (v2\ PEAP-MS-CHAP), que es un método de autenticación basada en contraseña para inalámbrica segura, y también configurar una directiva de solicitud de conexión para permitir el acceso no autenticado, el resultado es que los clientes no están necesarios para autenticar mediante PEAP-MS-CHAPv2. En este ejemplo, se conceden a todos los clientes que se conectan a la red acceso no autenticado.

### <a name="accounting"></a>Administración de cuentas

Con esta opción, puedes configurar la directiva de solicitud de conexión para reenviar información de cuentas a NPS u otro servidor RADIUS en un grupo de servidores RADIUS remotos para que el grupo de servidores RADIUS remotos realiza el registro.

>[!NOTE]
>Si tienes varios servidores RADIUS y quieres que la información de cuentas para todos los servidores que se almacenan en un radio contabilidad base de datos central, puedes usar la configuración de cuentas de la directiva de solicitud de conexión en una directiva en todos los servidores RADIUS para reenviar datos de cuentas de todos los servidores a un servidor NPS u otro servidor RADIUS que está designada como un servidor de contabilidad.

Configuración de directiva de solicitud de conexión contabilidad función independiente de la configuración de cuentas del servidor NPS local. En otras palabras, si estableces el servidor NPS local para registrar información de cuentas de radio en un archivo local o en una base de datos de Microsoft SQL Server, lo hará independientemente de si configura una directiva de solicitud de conexión para reenviar mensajes de cuentas a un grupo de servidores RADIUS remotos.

Si quieres que la información de contabilidad registrada de forma remota, pero no local, debes configurar el servidor NPS local que no realice Contabilidad mientras también configurar cuentas en una directiva de solicitud de conexión para reenviar datos de cuentas para un grupo de servidores RADIUS remotos.

### <a name="attribute-manipulation"></a>Manipulación de atributos

Puedes configurar un conjunto de reglas de buscar y reemplazar que manipulan las cadenas de texto de uno de los siguientes atributos.

- Nombre de usuario
- Id. de la estación llamada
- Identificador de la estación de llamada

Procesamiento de la regla de buscar y reemplazar se produce uno de los atributos anteriores antes de que el mensaje RADIUS según la configuración de autenticación y cuentas. Reglas de manipulación de atributos que se aplican solo a un solo atributo. No puedes configurar las reglas de manipulación de atributo para cada atributo. Además, la lista de atributos que se pueden manipular es una lista estática; No puedes agregar a la lista de atributos disponibles para la manipulación.

>[!NOTE]
>Si estás usando el protocolo de autenticación de MS-CHAP v2, no puede manipular el atributo de nombre de usuario si se usa la directiva de solicitud de conexión para reenviar el mensaje RADIUS. La única excepción se produce cuando se usa un carácter de barra diagonal inversa (\) y la manipulación solo afecta a la información a la izquierda de la misma. Una barra diagonal inversa se usa normalmente para indicar un nombre de dominio (la información a la izquierda de la barra diagonal inversa) y un nombre de cuenta de usuario dentro del dominio (la información a la derecha de la barra diagonal inversa). En este caso, se permiten solo reglas de manipulación de atributos que se modifica o reemplazan el nombre de dominio.

Para ver ejemplos de cómo manipular el nombre de dominio en el atributo de nombre de usuario, consulta la sección "Ejemplos de manipulación del nombre de dominio en el atributo de nombre de usuario" en el tema [usa las expresiones regulares de NPS](nps-crp-reg-expressions.md).

### <a name="forwarding-request"></a>Solicitud de desvío de llamadas

Puede establecer las siguientes opciones de solicitud que se usan para mensajes de solicitud de acceso RADIUS de enrutamiento:

- **Autenticar las solicitudes de este servidor**. Con esta opción, NPS usa un dominio de Windows NT 4.0, Active Directory o la base de datos de cuentas de usuario de administrador de cuentas de seguridad (SAM) local para autenticar la solicitud de conexión. Esta configuración también especifica que la directiva de red coincidente configurada en NPS, junto con las propiedades de marcado de la cuenta de usuario, usa NPS para autorizar la solicitud de conexión. En este caso, el servidor NPS está configurado para actuar como servidor RADIUS.

- **Reenviar solicitudes a los siguientes grupos de servidores RADIUS remotos**. Con esta opción, NPS reenvía solicitudes de conexión al grupo de servidores RADIUS remotos que especifiques. Si el servidor NPS recibe un mensaje de aceptación de acceso válido que corresponda al mensaje de solicitud de acceso, el intento de conexión se considera autenticado y autorizado. En este caso, el servidor NPS actúa como proxy RADIUS.

- **Aceptar usuarios sin validar las credenciales**. Con esta opción, NPS no comprueba la identidad del usuario que intenta conectarse a la red y NPS no intenta comprobar que el usuario o equipo tenga el derecho a conectarse a la red. Cuando NPS esté configurado para permitir el acceso no autenticado y recibe una solicitud de conexión, NPS envía inmediatamente que un mensaje de aceptación de acceso para el radio del cliente y el usuario o el equipo se concede acceso a la red. Esta configuración se usa para algunos tipos de túnel obligatorio donde se aplica un túnel el cliente de acceso antes de que se autentican las credenciales de usuario.

>[!NOTE]
>Esta opción de autenticación no puede usarse cuando el protocolo de autenticación del cliente de acceso es MS-CHAPv2 o (EAP-TLS), de seguridad de capa de transporte de protocolo de autenticación Extensible que proporcionan la autenticación mutua. En la autenticación mutua, el cliente de acceso demuestra que es un cliente de acceso válido al servidor de autenticación (el servidor NPS), y el servidor de autenticación demuestra que es un servidor de autenticación válido para el cliente de acceso. Cuando se usa esta opción de autenticación, se devuelve el mensaje de aceptación de acceso. Sin embargo, el servidor de autenticación no proporcionan la validación en el cliente de acceso y se produce un error en la autenticación mutua.

Para ver ejemplos de cómo usar expresiones regulares para crear reglas de enrutamiento que reenvíen mensajes RADIUS con un nombre de dominio Kerberos especificado a un grupo de servidores RADIUS remotos, consulta la sección "Ejemplo de reenvío de mensajes RADIUS mediante un servidor proxy" en el tema [usa las expresiones regulares de NPS](nps-crp-reg-expressions.md).

### <a name="advanced"></a>Avanzada

Puedes establecer las propiedades avanzadas para especificar el conjunto de atributos de radio:

- Se agrega al mensaje de respuesta RADIUS cuando se usa como un servidor de contabilidad o autenticación RADIUS el servidor NPS. Cuando hay atributos especificados en una directiva de red y la directiva de solicitud de conexión, los atributos que se envían en el mensaje de respuesta RADIUS son la combinación de los dos conjuntos de atributos.
- Agregar al mensaje RADIUS cuando el servidor NPS se usa como una autenticación RADIUS proxy o cuentas. Si ya existe el atributo en el mensaje que se reenvía, se reemplaza con el valor del atributo especificado en la directiva de solicitud de conexión. 

Además, algunos atributos que están disponibles para la configuración en la conexión solicitan directiva **configuración** pestaña en la **avanzadas** categoría proporcionan funciones especializadas. Por ejemplo, puedes configurar el **RADIUS remoto para la asignación de usuarios de Windows** atributo cuando quieras dividir la autenticación y autorización de una solicitud de conexión entre dos usuarios cuentas bases de datos.

La **RADIUS remoto para la asignación de usuarios de Windows** atributo especifica que se produzca autorización de Windows para los usuarios autenticados por un servidor remoto de RADIUS. En otras palabras, un servidor remoto RADIUS realiza la autenticación con una cuenta de usuario en una base de datos de cuentas de usuario remoto, pero el servidor NPS local autoriza la solicitud de conexión con una cuenta de usuario en una base de datos de cuentas de usuario local. Esto es útil cuando quieres permitir que a los visitantes acceso a la red.

Por ejemplo, los visitantes de las organizaciones asociadas pueden autenticarse con su propio servidor RADIUS de la organización de partner y, a continuación, usan una cuenta de usuario de Windows en la organización para obtener acceso a invitado red de área local (LAN) de la red.

Otros atributos que proporcionan funciones especializadas son:

- **MS-cuarentena-IPFilter y MS-cuarentena-Session-Timeout**. Estos atributos se usan al implementar el Control de cuarentena de acceso de red (NAQC) con la implementación de enrutamiento y acceso remoto VPN.
- **Passport usuario asignación UPN sufijo**. Este atributo permite autenticar las solicitudes de conexión con credenciales de cuenta de usuario de Windows Live™ ID.
- **Etiqueta de túnel**. Este atributo designa el número de Id. de VLAN al que se debe asignar la conexión NAS cuando implementas virtuales redes de área local (VLAN).

## <a name="default-connection-request-policy"></a>Directiva predeterminada de la solicitud de conexión

Se crea una directiva de solicitud de conexión de forma predeterminada cuando se instala NPS. Esta directiva tiene la siguiente configuración.

- No está configurada la autenticación.
- Contabilidad no está configurado para reenviar información de cuentas para un grupo de servidores RADIUS remotos.
- Atributo no está configurado con las reglas de manipulación de atributo que reenvían solicitudes de conexión a grupos de servidores RADIUS remotos.
- Reenvío de solicitud se configura para que las solicitudes de conexión se autenticar y autorizar en el servidor NPS local.
- Atributos avanzados no están configurados.

La directiva de solicitud de conexión predeterminada usa NPS como un servidor RADIUS. Para configurar un servidor que ejecuta NPS para que actúe como un proxy RADIUS, también debes configurar un grupo de servidores RADIUS remotos. Puedes crear un nuevo grupo de servidores RADIUS remoto mientras vas a crear una nueva directiva de solicitud de conexión usando el Asistente para nueva directiva de solicitud de conexión. Puedes eliminar la directiva de solicitud de conexión predeterminada o comprobar que la directiva de solicitud de conexión predeterminada es la última directiva procesada por NPS colocando última en la lista ordenada de directivas.

>[!NOTE]
>Si se instalan NPS y el servicio de acceso remoto en el mismo equipo y el acceso remoto servicio está configurado para la autenticación de Windows y de cuentas, es posible para la autenticación de acceso remoto y solicitudes de cuentas se reenvíen a un servidor RADIUS. Esto puede ocurrir cuando acceso remoto solicitudes de autenticación y contabilidad coincide con una directiva de solicitud de conexión que está configurada para reenvíalo a un grupo de servidores RADIUS remotos.