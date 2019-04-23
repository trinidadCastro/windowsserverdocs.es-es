---
title: Directivas de solicitud de conexión
description: En este tema se proporciona información general de directivas de solicitud de conexión de servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4ec45e0c-6b37-4dfb-8158-5f40677b0157
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 21330b3838064c7b31e78ef70bdc04f0db0f50db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872946"
---
# <a name="connection-request-policies"></a>Directivas de solicitud de conexión

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para obtener información sobre cómo usar las directivas de solicitud de conexión de NPS para configurar NPS como un servidor RADIUS, proxy RADIUS o ambos.

>[!NOTE]
>Además de este tema, está disponible la siguiente documentación de directiva de solicitud de conexión.
> - [Configurar las directivas de solicitud de conexión](nps-crp-configure.md)
> - [Configurar grupos de servidores RADIUS remotos](nps-crp-rrsg-configure.md)

Las directivas de solicitud de conexión son conjuntos de condiciones y configuraciones que permitan a los administradores de red designar qué servidores de servicio de autenticación remota telefónica de usuario (RADIUS) realizan la autenticación y autorización de conexión que solicita el servidor que ejecuta el servidor de directivas de redes (NPS) recibe de los clientes RADIUS. Las directivas de solicitud de conexión pueden configurarse para designar qué servidores RADIUS se usan para las cuentas RADIUS.

Se puede crear conexión de las directivas de solicitud para que algunos mensajes de solicitud RADIUS enviados desde clientes RADIUS se procesen localmente (NPS se usa como servidor RADIUS) y otros tipos de mensajes se reenvían a otro servidor RADIUS (NPS se usa como proxy RADIUS).

Con las directivas de solicitud de conexión, puede usar NPS como un servidor RADIUS o como un proxy RADIUS, basándose en factores como la siguiente: 

- La hora del día y el día de la semana
- El nombre de dominio Kerberos en la solicitud de conexión
- El tipo de conexión que se solicita
- La dirección IP del cliente RADIUS

Mensajes de solicitud de acceso RADIUS son procesados o reenviados por NPS sólo si la configuración del mensaje entrante coincide con al menos una de las directivas de solicitud de conexión configuradas en el NPS. 

Si la configuración de directiva coincide y la directiva requiere que el NPS procesa el mensaje, NPS actúa como un servidor RADIUS, autenticar y autorizar la solicitud de conexión. Si coincide con la configuración de directiva y la directiva requiere que el NPS reenvía el mensaje, NPS actúa como un proxy RADIUS y reenvía la solicitud de conexión a un servidor RADIUS remoto para su procesamiento.

Si la configuración de un mensaje de solicitud de acceso RADIUS entrante no coincide con al menos una de las directivas de solicitud de conexión, se envía un mensaje de rechazo de acceso al cliente RADIUS y el usuario o equipo intenta conectarse a la red se les deniega el acceso.

## <a name="configuration-examples"></a>Ejemplos de configuración

Los ejemplos de configuración siguientes muestran cómo puede usar las directivas de solicitud de conexión.

### <a name="nps-as-a-radius-server"></a>NPS como servidor RADIUS

La directiva de solicitud de conexión predeterminada es la única directiva configurada. En este ejemplo, NPS se configura como servidor RADIUS y se procesan todas las solicitudes de conexión al NPS local. NPS puede autenticar y autorizar a los usuarios cuyas cuentas están en el dominio del dominio NPS y en dominios de confianza.


### <a name="nps-as-a-radius-proxy"></a>NPS como proxy RADIUS

La directiva de solicitud de conexión predeterminada se elimina y se crean dos nuevas directivas de solicitud de conexión para reenviar solicitudes a dos dominios diferentes. En este ejemplo, se configura NPS como proxy RADIUS. NPS no procesa las solicitudes de conexión en el servidor local. En su lugar, reenvía las solicitudes de conexión a NPS u otros servidores RADIUS configurados como miembros de grupos de servidores RADIUS remotos.


### <a name="nps-as-both-radius-server-and-radius-proxy"></a>NPS como servidor RADIUS y proxy RADIUS

Además de la directiva de solicitud de conexión de forma predeterminada, se crea una nueva directiva de solicitud de conexión que reenvía las solicitudes de conexión a NPS u otro servidor RADIUS en un dominio de confianza. En este ejemplo, la directiva de proxy aparece en primer lugar en la lista ordenada de directivas. Si la solicitud de conexión coincide con la directiva de proxy, la solicitud de conexión se reenvía al servidor RADIUS en el grupo de servidores RADIUS remotos. Si la solicitud de conexión no coincide con la directiva proxy pero coincide con la directiva de solicitud de conexión de forma predeterminada, NPS procesa la solicitud de conexión en el servidor local. Si la solicitud de conexión no coincide con cualquiera de ellas, se descarta.

### <a name="nps-as-radius-server-with-remote-accounting-servers"></a>NPS como servidor RADIUS con servidores de cuentas remotas

En este ejemplo, el NPS local no está configurado para administrar cuentas y la directiva de solicitud de conexión predeterminada se revisó para que se reenvían los mensajes de cuentas RADIUS a NPS u otro servidor RADIUS en un grupo de servidores RADIUS remotos. Aunque se reenvían los mensajes de cuentas, no se reenvían los mensajes de autenticación y autorización y el NPS local realiza estas funciones para el dominio local y todos los dominios de confianza.


### <a name="nps-with-remote-radius-to-windows-user-mapping"></a>NPS con RADIUS remota a la asignación de usuario de Windows

En este ejemplo, NPS actúa como servidor RADIUS y como proxy RADIUS para cada solicitud de conexión individuales mediante el reenvío de la solicitud de autenticación a un servidor RADIUS remoto mientras se usa una cuenta de usuario de Windows local para la autorización. Esta configuración se implementa mediante la configuración de la de RADIUS remota a atributos de asignación de usuario de Windows como una condición de la directiva de solicitud de conexión. (Además, una cuenta de usuario debe crearse localmente que tiene el mismo nombre que la cuenta de usuario remoto en el que se realiza autenticación por el servidor RADIUS remoto.)

## <a name="connection-request-policy-conditions"></a>Condiciones de directiva de solicitud de conexión

Condiciones de directiva de solicitud de conexión son uno o más atributos RADIUS que se comparan con los atributos del mensaje de solicitud de acceso RADIUS entrante. Si hay varias condiciones, todas las condiciones en la conexión de mensaje de solicitud y en la solicitud de conexión directiva debe coincidir para que la directiva sea aplicada por NPS.


A continuación es los atributos de condición que se pueden configurar en las directivas de solicitud de conexión.

### <a name="connection-properties-attribute-group"></a>Grupo de atributos de las propiedades de conexión

El grupo de atributos de las propiedades de conexión contiene los siguientes atributos.

- **Protocolo entramado**. Se usa para designar el tipo de entramado para los paquetes entrantes. Algunos ejemplos son el protocolo punto a punto (PPP), protocolo de Internet de línea serie (SLIP), Frame Relay y X.25.
- **Tipo de servicio**. Se usa para designar el tipo de servicio que se solicita. Algunos ejemplos incluyen entramados (por ejemplo, las conexiones PPP) y el inicio de sesión (por ejemplo, conexiones Telnet). Para obtener más información acerca de los tipos de servicio RADIUS, consulte RFC 2865, "Servicio autenticación remota telefónica de usuario (RADIUS)."
- **Tipo de túnel**. Se usa para designar el tipo de túnel que se va a crear el cliente solicitante. Tipos de túnel incluyen el protocolo de túnel punto a punto (PPTP) y el protocolo de túnel de capa dos (L2TP).

### <a name="day-and-time-restrictions-attribute-group"></a>Grupo de atributos de restricciones de día y hora 

El grupo de atributos de restricciones de día y hora contiene el atributo de restricciones de día y hora. Con este atributo, puede designar el día de la semana y la hora del día del intento de conexión. El día y hora es relativa a la fecha y hora de NPS.

### <a name="gateway-attribute-group"></a>Grupo de atributos de la puerta de enlace

El grupo de atributos de la puerta de enlace contiene los siguientes atributos.

- **Identificador de estación llamada**. Se usa para designar el número de teléfono del servidor de acceso de red. Este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrón para especificar códigos de área.
- **Identificador de NAS**. Se usa para designar el nombre del servidor de acceso de red. Este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrón para especificar identificadores de NAS.
- **Dirección NAS IPv4**. Se usa para designar el protocolo de Internet versión 4 (IPv4) dirección del servidor de acceso de red (el cliente RADIUS). Este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrón para especificar redes IP.
- **Dirección NAS IPv6**. Se usa para designar el protocolo de Internet versión 6 (IPv6) dirección del servidor de acceso de red (el cliente RADIUS). Este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrón para especificar redes IP.
- **Tipo de puerto de NAS**. Se usa para designar el tipo de medio usado por el cliente de acceso. Algunos ejemplos son líneas telefónicas analógicas (conocidas como asincrónicas), red Digital de servicios integrados (RDSI), túneles o redes privadas virtuales (VPN), conexiones inalámbricas IEEE 802.11 y conmutadores Ethernet.

### <a name="machine-identity-attribute-group"></a>Grupo de atributos de identidad de máquina

El grupo de atributos de identidad de máquina contiene el atributo de identidad de la máquina. Mediante el uso de este atributo, puede especificar el método con el que los clientes se identifican en la directiva.

### <a name="radius-client-properties-attribute-group"></a>Grupo de atributos de propiedades del cliente RADIUS

El grupo de atributos de propiedades del cliente RADIUS contiene los siguientes atributos.

- **Identificador de estación que llama**. Se usa para designar el número de teléfono usado por el llamador (el cliente de acceso). Este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrón para especificar códigos de área.  En las 802.1X autenticaciones la dirección MAC normalmente se rellena y pueden coincidir desde el cliente.  Este campo se utiliza normalmente para escenarios de omisión de dirección Mac cuando se configura la directiva de solicitud de conexión para "Aceptar usuarios sin validar credenciales".  
- **Nombre descriptivo del cliente**. Se usa para designar el nombre del equipo cliente RADIUS que solicita autenticación. Este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrón para especificar nombres de cliente.
- **Dirección IPv4 del cliente**. Se usa para designar la dirección IPv4 del servidor de acceso de red (el cliente RADIUS). Este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrón para especificar redes IP.
- **Dirección IPv6 del cliente**. Se usa para designar la dirección IPv6 del servidor de acceso de red (el cliente RADIUS). Este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrón para especificar redes IP.
- **Proveedor del cliente**. Se usa para designar el proveedor del servidor de acceso de red que solicita autenticación. Un equipo que ejecuta el servicio de enrutamiento y acceso remoto es el fabricante de Microsoft NAS. Puede usar este atributo para configurar directivas independientes para diferentes fabricantes de NAS. Este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrones.

### <a name="user-name-attribute-group"></a>Grupo de atributos de nombre de usuario

El grupo de atributos de nombre de usuario contiene el atributo de nombre de usuario. Mediante el uso de este atributo, puede designar el nombre de usuario o una parte del nombre de usuario, que debe coincidir con el nombre de usuario proporcionado por el cliente de acceso en el mensaje RADIUS. Este atributo es una cadena de caracteres que normalmente contiene un nombre de dominio Kerberos y un nombre de cuenta de usuario. Puede usar la sintaxis de coincidencia de patrón para especificar los nombres de usuario.

## <a name="connection-request-policy-settings"></a>Configuración de directiva de solicitud de conexión

Configuración de directiva de solicitud de conexión es un conjunto de propiedades que se aplican a un mensaje RADIUS entrante. Opciones de configuración constan de los siguientes grupos de propiedades.

- Autenticación
- Cuentas
- Manipulación de atributos
- Solicitud de reenvío
- Avanzado

Las secciones siguientes proporcionan detalles adicionales sobre esta configuración.

### <a name="authentication"></a>Autenticación

Con esta configuración, puede invalidar la configuración de autenticación que se configura en todas las directivas de red y puede designar los métodos de autenticación y tipos que son necesarias para conectarse a la red.

>[!IMPORTANT]
>Si configura un método de autenticación en la directiva de solicitud de conexión que es menos seguro que el método de autenticación que se configura en la directiva de red, se invalida el método de autenticación más seguro que configure en la directiva de red. Por ejemplo, si tiene una directiva de red que requiere el uso de la versión 2 de Protected Extensible Authentication Microsoft del protocolo de protocolo de autenticación \(PEAP-MS-CHAP v2\), que está basada en contraseña método de autenticación para conexiones inalámbricas seguras y también configura una directiva de solicitud de conexión para permitir el acceso no autenticado, el resultado es que ningún cliente es necesario para autenticarse mediante PEAP-MS-CHAP v2. En este ejemplo, se conceden todos los clientes que se conectan a la red de acceso no autenticado.

### <a name="accounting"></a>Cuentas

Con esta configuración, puede configurar la directiva de solicitud de conexión para reenviar información de cuentas a NPS u otro servidor RADIUS en un grupo de servidores RADIUS remotos para que el grupo de servidores RADIUS remotos realiza el registro.

>[!NOTE]
>Si tiene varios servidores RADIUS y desea que la información de cuentas para todos los servidores almacenada en una base de cuentas RADIUS central, puede usar la configuración de cuentas de directiva de solicitud de conexión en una directiva en cada servidor RADIUS para reenviar datos de cuentas de todos los servidores a un servidor NPS u otro servidor RADIUS que se designa como servidor de cuentas.

Solicitud de conexión de configuración de cuentas de la directiva funciona independientemente de la configuración de cuentas del NPS local. En otras palabras, si configura el NPS local para registrar información de cuentas RADIUS en un archivo local o en una base de datos de Microsoft SQL Server, lo hará independientemente de si configura una directiva de solicitud de conexión para reenviar los mensajes de cuentas a un radio remoto grupo de servidores.

Si desea información de cuentas que se registran de forma remota, pero no de forma local, debe configurar el NPS local para no realizar la administración de cuentas, al tiempo que configura cuentas en una directiva de solicitud de conexión para reenviar datos de cuentas a un grupo de servidores RADIUS remotos.

### <a name="attribute-manipulation"></a>Manipulación de atributos

Puede configurar un conjunto de reglas de buscar y reemplazar que manipulen las cadenas de texto de uno de los siguientes atributos.

- Nombre de usuario
- Identificador de estación llamada
- Identificador de estación que llama

Procesamiento de la regla de buscar y reemplazar se produce para uno de los atributos precedentes antes de que el mensaje RADIUS está sujeto a la configuración de autenticación y cuentas. Reglas de manipulación de atributos se aplican solo a un solo atributo. No se puede configurar reglas de manipulación de atributos para cada atributo. Además, la lista de atributos que se pueden manipular es una lista estática; no se puede agregar a la lista de atributos disponibles para la manipulación.

>[!NOTE]
>Si utiliza el protocolo de autenticación MS-CHAP v2, no puede manipular el atributo de nombre de usuario si se usa la directiva de solicitud de conexión para reenviar el mensaje RADIUS. La única excepción se produce cuando una barra diagonal inversa (\) carácter se usa y la manipulación sólo afecta a la información a la izquierda de la misma. Un carácter de barra diagonal inversa se usa normalmente para indicar un nombre de dominio (la información a la izquierda del carácter de barra diagonal inversa) y un nombre de cuenta de usuario dentro del dominio (la información a la derecha del carácter de barra diagonal inversa). En este caso, se permiten solo reglas de manipulación de atributos que modifican o reemplazan el nombre de dominio.

Para obtener ejemplos de cómo manipular el nombre de territorio en el atributo de nombre de usuario, vea la sección "Ejemplos de tratamiento del nombre de dominio Kerberos en el atributo de nombre de usuario" en el tema [usar expresiones regulares en NPS](nps-crp-reg-expressions.md).

### <a name="forwarding-request"></a>Solicitud de reenvío

Puede establecer las siguientes opciones de solicitud que se usan para los mensajes de solicitud de acceso RADIUS de reenvío:

- **Autenticar solicitudes en este servidor**. Con esta configuración, NPS usa un dominio de Windows NT 4.0, Active Directory o la base de datos de cuentas de usuario de administrador de cuentas de seguridad (SAM) local para autenticar la solicitud de conexión. Esta configuración también especifica que la directiva de red coincidente que se configura en NPS, junto con las propiedades de acceso telefónico de la cuenta de usuario, se usa NPS para autorizar la solicitud de conexión. En este caso, el NPS se configura para llevar a cabo como un servidor RADIUS.

- **Reenviar solicitudes a los siguientes grupos de servidores RADIUS remotos**. Con esta configuración, NPS reenvía las solicitudes de conexión para el grupo de servidores remotos RADIUS que especifique. Si NPS recibe un mensaje de aceptación de acceso válido que se corresponde con el mensaje de solicitud de acceso, el intento de conexión se considera autenticado y autorizado. En este caso, el NPS actúa como un proxy RADIUS.

- **Aceptar usuarios sin validar credenciales**. Con esta configuración, NPS no comprueba la identidad del usuario que intenta conectarse a la red y NPS no intenta comprobar que el usuario o equipo tiene derecho a conectarse a la red. Cuando NPS se configura para permitir el acceso no autenticado y recibe una solicitud de conexión, NPS envía inmediatamente que un mensaje de aceptación de acceso para el radio del cliente y el usuario o equipo se concede acceso a la red. Esta configuración se utiliza para algunos tipos de túnel obligatorio donde se abren el cliente de acceso antes de que se autentican las credenciales de usuario.

>[!NOTE]
>No se puede usar esta opción de autenticación cuando el protocolo de autenticación del cliente de acceso es MS-CHAP v2 o Extensible Authentication Protocol-Transport Layer Security (EAP-TLS), que ofrecen la autenticación mutua. En la autenticación mutua, el cliente de acceso demuestra que es un cliente de acceso válido para el servidor de autenticación (NPS) y el servidor de autenticación demuestra que es un servidor de autenticación válido para el cliente de acceso. Cuando se usa esta opción de autenticación, se devuelve el mensaje de aceptación de acceso. Sin embargo, el servidor de autenticación no proporciona validación al cliente de acceso y se produce un error en la autenticación mutua.

Para obtener ejemplos de cómo usar expresiones regulares para crear reglas de enrutamiento que reenvíen mensajes RADIUS con un nombre de dominio Kerberos especificado a un grupo de servidores RADIUS remotos, vea la sección "Ejemplo de reenvío de mensajes RADIUS por un servidor proxy" en el tema [usar Expresiones regulares en NPS](nps-crp-reg-expressions.md).

### <a name="advanced"></a>Avanzado

Puede establecer propiedades avanzadas para especificar la serie de atributos RADIUS que son:

- Agrega al mensaje de respuesta RADIUS cuando se usa el NPS como un servidor de administración de cuentas o autenticación RADIUS. Cuando hay atributos especificados en una directiva de red y la directiva de solicitud de conexión, los atributos que se envían en el mensaje de respuesta RADIUS son la combinación de los dos conjuntos de atributos.
- Se agrega al mensaje RADIUS cuando NPS se utiliza como autenticación de RADIUS o cuentas de proxy. Si el atributo ya existe en el mensaje que se reenvía, se reemplaza con el valor del atributo especificado en la directiva de solicitud de conexión. 

Además, algunos atributos que están disponibles para la configuración en la conexión de directiva de solicitud **configuración** pestaña en el **avanzadas** categoría proporcionan funciones especializadas. Por ejemplo, puede configurar el **de RADIUS remota a la asignación de usuario de Windows** atributo al que desea dividir la autenticación y autorización de una solicitud de conexión entre el usuario de dos cuentas de bases de datos.

El **de RADIUS remota a la asignación de usuario de Windows** atributo especifica que la autorización de Windows se produce para los usuarios autenticados por un servidor RADIUS remoto. En otras palabras, un servidor RADIUS remoto realiza la autenticación con una cuenta de usuario en una base de datos de cuentas de usuario remoto, pero el NPS local autoriza la solicitud de conexión con una cuenta de usuario en una base de datos de cuentas de usuario local. Esto es útil cuando desee permitir el acceso a la red a los visitantes.

Por ejemplo, los visitantes de organizaciones asociadas pueden ser autenticados por su propio servidor RADIUS de organización de asociado y, a continuación, usa una cuenta de usuario de Windows en la organización para tener acceso a una red de área de local (LAN) de invitado en la red.

Otros atributos que proporcionan funciones especializadas son:

- **MS-Quarantine-IPFilter y MS-Quarantine-Session-Timeout**. Estos atributos se usan al implementar el Control de cuarentena de acceso de red (NAQC) con la implementación de enrutamiento y acceso remoto VPN.
- **Passport-User-Mapping-UPN-Suffix**. Este atributo permite autenticar solicitudes de conexión con las credenciales de cuenta de usuario de Windows Live™ ID.
- **Etiqueta de túnel**. Este atributo designa el número de Id. de VLAN al que la conexión debe asignarse por el NAS cuando se implementan redes de área local virtuales (VLAN).

## <a name="default-connection-request-policy"></a>Directiva de solicitud de conexión predeterminada

Al instalar NPS, se crea una directiva de solicitud de conexión predeterminada. Esta directiva tiene la siguiente configuración.

- Autenticación no está configurada.
- Administración de cuentas no está configurado para reenviar información de cuentas a un grupo de servidores RADIUS remotos.
- Atributo no está configurado con reglas de manipulación de atributos que reenvían las solicitudes de conexión a grupos de servidores RADIUS remotos.
- Reenviar solicitud está configurado para que las solicitudes de conexión se autentiquen y autoricen en el NPS local.
- No se configuran los atributos avanzados.

La directiva de solicitud de conexión predeterminada usa NPS como servidor RADIUS. Para configurar un servidor que ejecuta NPS para que actúe como proxy RADIUS, también debe configurar un grupo de servidores RADIUS remotos. Puede crear un nuevo grupo de servidores remotos RADIUS mientras crea una nueva directiva de solicitud de conexión utilizando el Asistente para nueva directiva de solicitud de conexión. Puede eliminar la directiva de solicitud de conexión predeterminada o comprobar que la directiva de solicitud de conexión predeterminada sea la última directiva procesada por NPS colocando por última vez en la lista ordenada de directivas.

>[!NOTE]
>Si NPS y el servicio de acceso remoto están instalados en el mismo equipo y el acceso remoto, el servicio está configurado para la autenticación de Windows y cuentas, es posible para la autenticación de acceso remoto y las solicitudes de cuentas se reenvíen a un servidor RADIUS . Esto puede ocurrir cuando las solicitudes de cuentas y de autenticación de acceso remoto coincide con una directiva de solicitud de conexión que está configurada para reenviarlas a un grupo de servidores RADIUS remotos.
