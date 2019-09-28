---
title: Directivas de solicitud de conexión
description: En este tema se proporciona información general sobre las directivas de solicitud de conexión del servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 4ec45e0c-6b37-4dfb-8158-5f40677b0157
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0b373e496567f414657b380bad952baefcbe4b18
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396366"
---
# <a name="connection-request-policies"></a>Directivas de solicitud de conexión

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para aprender a usar las directivas de solicitud de conexión NPS para configurar NPS como un servidor RADIUS, un proxy RADIUS o ambos.

>[!NOTE]
>Además de este tema, está disponible la siguiente documentación de la Directiva de solicitud de conexión.
> - [Configurar directivas de solicitud de conexión](nps-crp-configure.md)
> - [Configurar grupos de servidores RADIUS remotos](nps-crp-rrsg-configure.md)

Las directivas de solicitud de conexión son conjuntos de condiciones y configuraciones que permiten a los administradores de red designar qué servidores Servicio de autenticación remota telefónica de usuario (RADIUS) realizan la autenticación y autorización de solicitudes de conexión que el el servidor que ejecuta el servidor de directivas de redes (NPS) recibe de los clientes RADIUS. Las directivas de solicitud de conexión se pueden configurar para designar qué servidores RADIUS se utilizan para las cuentas RADIUS.

Puede crear directivas de solicitud de conexión para que algunos mensajes de solicitud RADIUS enviados desde clientes RADIUS se procesen localmente (NPS se usa como un servidor RADIUS) y otros tipos de mensajes se reenvíen a otro servidor RADIUS (NPS se usa como proxy RADIUS).

Con las directivas de solicitud de conexión, puede usar NPS como un servidor RADIUS o como proxy RADIUS, en función de factores como los siguientes: 

- La hora del día y el día de la semana
- El nombre de dominio Kerberos en la solicitud de conexión
- El tipo de conexión que se solicita
- La dirección IP del cliente RADIUS

NPS procesa o reenvía los mensajes de solicitud de acceso RADIUS solo si la configuración del mensaje entrante coincide con al menos una de las directivas de solicitud de conexión configuradas en el NPS. 

Si la configuración de la Directiva coincide y la Directiva requiere que el NPS procese el mensaje, NPS actúa como un servidor RADIUS, autenticando y autorizando la solicitud de conexión. Si la configuración de la Directiva coincide y la Directiva requiere que NPS reenvíe el mensaje, NPS actúa como proxy RADIUS y reenvía la solicitud de conexión a un servidor RADIUS remoto para su procesamiento.

Si la configuración de un mensaje de solicitud de acceso RADIUS entrante no coincide con al menos una de las directivas de solicitud de conexión, se envía un mensaje de rechazo de acceso al cliente RADIUS y se deniega el acceso al usuario o equipo que intenta conectarse a la red.

## <a name="configuration-examples"></a>Ejemplos de configuración

Los siguientes ejemplos de configuración muestran cómo se pueden usar las directivas de solicitud de conexión.

### <a name="nps-as-a-radius-server"></a>NPS como servidor RADIUS

La Directiva de solicitud de conexión predeterminada es la única directiva configurada. En este ejemplo, NPS se configura como un servidor RADIUS y el NPS local procesa todas las solicitudes de conexión. NPS puede autenticar y autorizar a los usuarios cuyas cuentas están en el dominio del dominio NPS y en los dominios de confianza.


### <a name="nps-as-a-radius-proxy"></a>NPS como proxy RADIUS

Se elimina la Directiva de solicitud de conexión predeterminada y se crean dos nuevas directivas de solicitud de conexión para reenviar las solicitudes a dos dominios diferentes. En este ejemplo, NPS se configura como un proxy RADIUS. NPS no procesa ninguna solicitud de conexión en el servidor local. En su lugar, reenvía las solicitudes de conexión a NPS u otros servidores RADIUS que están configurados como miembros de grupos de servidores RADIUS remotos.


### <a name="nps-as-both-radius-server-and-radius-proxy"></a>NPS como servidor RADIUS y proxy RADIUS

Además de la Directiva de solicitud de conexión predeterminada, se crea una nueva Directiva de solicitud de conexión que reenvía las solicitudes de conexión a un NPS u otro servidor RADIUS en un dominio que no es de confianza. En este ejemplo, la Directiva de proxy aparece en primer lugar en la lista ordenada de directivas. Si la solicitud de conexión coincide con la Directiva de proxy, la solicitud de conexión se reenvía al servidor RADIUS en el grupo de servidores RADIUS remotos. Si la solicitud de conexión no coincide con la Directiva de proxy, pero coincide con la Directiva de solicitud de conexión predeterminada, NPS procesa la solicitud de conexión en el servidor local. Si la solicitud de conexión no coincide con ninguna de las dos directivas, se descarta.

### <a name="nps-as-radius-server-with-remote-accounting-servers"></a>NPS como servidor RADIUS con servidores de cuentas remotas

En este ejemplo, el NPS local no está configurado para realizar cuentas y la Directiva de solicitud de conexión predeterminada se revisa para que los mensajes de cuentas RADIUS se reenvíen a un NPS u otro servidor RADIUS en un grupo de servidores RADIUS remotos. Aunque se reenvían los mensajes de cuentas, no se reenvían los mensajes de autenticación y autorización, y el NPS local realiza estas funciones para el dominio local y todos los dominios de confianza.


### <a name="nps-with-remote-radius-to-windows-user-mapping"></a>NPS con asignación de usuarios de RADIUS remotos a Windows

En este ejemplo, NPS actúa como servidor RADIUS y como proxy RADIUS para cada solicitud de conexión individual mediante el reenvío de la solicitud de autenticación a un servidor RADIUS remoto mientras se usa una cuenta de usuario de Windows local para la autorización. Esta configuración se implementa mediante la configuración del atributo de asignación de RADIUS remota a usuario de Windows como una condición de la Directiva de solicitud de conexión. (Además, se debe crear una cuenta de usuario localmente que tenga el mismo nombre que la cuenta de usuario remota en la que el servidor RADIUS remoto realiza la autenticación).

## <a name="connection-request-policy-conditions"></a>Condiciones de la Directiva de solicitud de conexión

Las condiciones de la Directiva de solicitud de conexión son uno o más atributos RADIUS que se comparan con los atributos del mensaje de solicitud de acceso RADIUS entrante. Si hay varias condiciones, todas las condiciones del mensaje de solicitud de conexión y de la Directiva de solicitud de conexión deben coincidir para que NPS aplique la Directiva.


A continuación se muestran los atributos de condición disponibles que se pueden configurar en las directivas de solicitud de conexión.

### <a name="connection-properties-attribute-group"></a>Propiedades de conexión (grupo de atributos)

El grupo de atributos propiedades de conexión contiene los siguientes atributos.

- **Protocolo de tramas**. Se usa para designar el tipo de tramas para los paquetes entrantes. Algunos ejemplos son Protocolo punto a punto (PPP), protocolo de Internet de línea de serie (SLIP), Frame Relay y X. 25.
- **Tipo de servicio**. Se usa para designar el tipo de servicio que se solicita. Entre los ejemplos se incluyen tramas (por ejemplo, conexiones PPP) e inicio de sesión (por ejemplo, conexiones telnet). Para obtener más información acerca de los tipos de servicio RADIUS, consulte RFC 2865, "servicio de autenticación remota telefónica de usuario (RADIUS)".
- **Tipo de túnel**. Se usa para designar el tipo de túnel que va a crear el cliente que realiza la solicitud. Los tipos de túnel incluyen el protocolo de túnel punto a punto (PPTP) y el protocolo de túnel de capa dos (L2TP).

### <a name="day-and-time-restrictions-attribute-group"></a>Grupo de atributos de restricciones de día y hora 

El grupo de atributos de restricciones de día y hora contiene el atributo de restricciones de día y hora. Con este atributo, puede designar el día de la semana y la hora del día del intento de conexión. El día y la hora son relativos al día y la hora del NPS.

### <a name="gateway-attribute-group"></a>Grupo de atributos de puerta de enlace

El grupo de atributos de puerta de enlace contiene los siguientes atributos.

- **Identificador de estación llamado**. Se usa para designar el número de teléfono del servidor de acceso a la red. Este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrones para especificar códigos de área.
- **Identificador de NAS**. Se usa para designar el nombre del servidor de acceso a la red. Este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrones para especificar los identificadores de NAS.
- **Dirección IPv4 de NAS**. Se usa para designar la dirección del Protocolo de Internet versión 4 (IPv4) del servidor de acceso a la red (el cliente RADIUS). Este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrones para especificar redes IP.
- **Dirección IPv6 de NAS**. Se usa para designar la dirección del Protocolo de Internet versión 6 (IPv6) del servidor de acceso a la red (el cliente RADIUS). Este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrones para especificar redes IP.
- **Tipo de puerto de NAS**. Se usa para designar el tipo de medio usado por el cliente de acceso. Algunos ejemplos son las líneas telefónicas analógicas (conocidas como asincrónicas), la red digital de servicios integrados (RDSI), túneles o redes privadas virtuales (VPN), IEEE 802,11 inalámbrica y conmutadores Ethernet.

### <a name="machine-identity-attribute-group"></a>Grupo de atributos de identidad de equipo

El grupo de atributos de identidad de la máquina contiene el atributo de identidad de la máquina. Mediante este atributo, puede especificar el método con el que se identifican los clientes en la Directiva.

### <a name="radius-client-properties-attribute-group"></a>Grupo de atributos de propiedades de cliente RADIUS

El grupo de atributos propiedades del cliente RADIUS contiene los siguientes atributos.

- **Identificador de estación que llama**. Se usa para designar el número de teléfono usado por el autor de la llamada (el cliente de acceso). Este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrones para especificar códigos de área.  En las autenticaciones de 802.1 x, la dirección MAC se rellena normalmente y se puede hacer coincidir desde el cliente.  Este campo se usa normalmente para escenarios de omisión de direcciones MAC cuando la Directiva de solicitud de conexión está configurada para "aceptar usuarios sin validar credenciales".  
- **Nombre descriptivo del cliente**. Se usa para designar el nombre del equipo cliente RADIUS que solicita autenticación. Este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrones para especificar los nombres de cliente.
- **Dirección IPv4 del cliente**. Se usa para designar la dirección IPv4 del servidor de acceso a la red (el cliente RADIUS). Este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrones para especificar redes IP.
- **Dirección IPv6 del cliente**. Se usa para designar la dirección IPv6 del servidor de acceso a la red (el cliente RADIUS). Este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrones para especificar redes IP.
- **Proveedor del cliente**. Se usa para designar el proveedor del servidor de acceso a la red que solicita autenticación. Un equipo que ejecuta el servicio de enrutamiento y acceso remoto es el fabricante de Microsoft NAS. Puede usar este atributo para configurar directivas independientes para diferentes fabricantes de NAS. Este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrones.

### <a name="user-name-attribute-group"></a>Grupo de atributos de nombre de usuario

El grupo de atributos de nombre de usuario contiene el atributo de nombre de usuario. Con este atributo, puede designar el nombre de usuario, o una parte del nombre de usuario, que debe coincidir con el nombre de usuario proporcionado por el cliente de acceso en el mensaje RADIUS. Este atributo es una cadena de caracteres que normalmente contiene un nombre de dominio Kerberos y un nombre de cuenta de usuario. Puede usar la sintaxis de coincidencia de patrón para especificar nombres de usuario.

## <a name="connection-request-policy-settings"></a>Configuración de la Directiva de solicitud de conexión

La configuración de la Directiva de solicitud de conexión es un conjunto de propiedades que se aplican a un mensaje RADIUS entrante. La configuración consta de los siguientes grupos de propiedades.

- Autenticación
- Cuentas
- Manipulación de atributos
- Reenvío de solicitud
- Avanzado

En las secciones siguientes se proporcionan detalles adicionales acerca de esta configuración.

### <a name="authentication"></a>Autenticación

Al usar esta opción, puede invalidar la configuración de autenticación que se configura en todas las directivas de red y puede designar los métodos y tipos de autenticación que se necesitan para conectarse a la red.

>[!IMPORTANT]
>Si configura un método de autenticación en una directiva de solicitud de conexión que sea menos segura que el método de autenticación que configure en la Directiva de red, se invalidará el método de autenticación más seguro que configure en la Directiva de red. Por ejemplo, si tiene una directiva de red que requiere el uso del Protocolo de autenticación extensible protegido-protocolo de autenticación por desafío mutuo de Microsoft versión 2 \(PEAP-MS-CHAP V2 @ no__t-1, que es una autenticación basada en contraseña método para una red inalámbrica segura y también se configura una directiva de solicitud de conexión para permitir el acceso no autenticado, el resultado es que no se requiere que los clientes se autentiquen mediante PEAP-MS-CHAP v2. En este ejemplo, se concede acceso no autenticado a todos los clientes que se conectan a la red.

### <a name="accounting"></a>Cuentas

Mediante esta opción, puede configurar la Directiva de solicitud de conexión para reenviar información de cuentas a un NPS u otro servidor RADIUS en un grupo de servidores RADIUS remotos para que el grupo de servidores remotos RADIUS realice cuentas.

>[!NOTE]
>Si tiene varios servidores RADIUS y desea obtener información de cuentas para todos los servidores almacenados en una base de datos de cuentas RADIUS central, puede usar la configuración de cuentas de la Directiva de solicitud de conexión en una directiva en cada servidor RADIUS para reenviar datos de cuentas desde todos los servidores a un NPS u otro servidor RADIUS que se designa como servidor de cuentas.

La configuración de cuentas de la Directiva de solicitud de conexión funciona independientemente de la configuración de cuentas del NPS local. En otras palabras, si configura el NPS local para registrar información de cuentas de RADIUS en un archivo local o en una base de datos de Microsoft SQL Server, lo hará independientemente de si configura una directiva de solicitud de conexión para reenviar mensajes de cuentas a un RADIUS remoto. Grupo de servidores.

Si desea que la información de cuentas se registre de forma remota pero no local, debe configurar el NPS local para que no realice cuentas, a la vez que configura también cuentas en una directiva de solicitud de conexión para reenviar datos de cuentas a un grupo de servidores RADIUS remotos.

### <a name="attribute-manipulation"></a>Manipulación de atributos

Puede configurar un conjunto de reglas de búsqueda y reemplazo que manipulen las cadenas de texto de uno de los siguientes atributos.

- Nombre de usuario
- IDENTIFICADOR de estación llamado
- IDENTIFICADOR de estación que llama

El procesamiento de reglas de búsqueda y reemplazo se produce para uno de los atributos anteriores antes de que el mensaje RADIUS esté sujeto a la configuración de autenticación y cuentas. Las reglas de manipulación de atributos se aplican solo a un atributo único. No puede configurar reglas de manipulación de atributos para cada atributo. Además, la lista de atributos que se pueden manipular es una lista estática. no se puede Agregar a la lista de atributos disponibles para su manipulación.

>[!NOTE]
>Si usa el protocolo de autenticación MS-CHAP V2, no podrá manipular el atributo de nombre de usuario si se utiliza la Directiva de solicitud de conexión para reenviar el mensaje RADIUS. La única excepción se produce cuando se usa una barra diagonal inversa (\) y la manipulación solo afecta a la información que se encuentra a su izquierda. Un carácter de barra diagonal inversa se usa normalmente para indicar un nombre de dominio (la información a la izquierda del carácter de barra diagonal inversa) y un nombre de cuenta de usuario en el dominio (la información a la derecha del carácter de barra diagonal inversa). En este caso, solo se permiten las reglas de manipulación de atributos que modifican o reemplazan el nombre de dominio.

Para ver ejemplos de cómo manipular el nombre de dominio Kerberos en el atributo de nombre de usuario, consulte la sección "ejemplos de manipulación del nombre de dominio Kerberos en el atributo de nombre de usuario" en el tema [uso de expresiones regulares en NPS](nps-crp-reg-expressions.md).

### <a name="forwarding-request"></a>Reenvío de solicitud

Puede establecer las siguientes opciones de solicitud de reenvío que se usan para los mensajes de solicitud de acceso RADIUS:

- **Autenticar solicitudes en este servidor**. Con esta configuración, NPS usa un dominio de Windows NT 4,0, Active Directory o la base de datos de cuentas de usuario del administrador de cuentas de seguridad (SAM) local para autenticar la solicitud de conexión. Esta configuración también especifica que NPS usa la Directiva de red coincidente configurada en NPS, junto con las propiedades de acceso telefónico de la cuenta de usuario para autorizar la solicitud de conexión. En este caso, el NPS está configurado para ejecutarse como un servidor RADIUS.

- **Reenviar solicitudes al siguiente grupo de servidores RADIUS remotos**. Al usar esta configuración, NPS reenvía las solicitudes de conexión al grupo de servidores RADIUS remoto que especifique. Si el NPS recibe un mensaje de aceptación de acceso válido que corresponde al mensaje de solicitud de acceso, el intento de conexión se considera autenticado y autorizado. En este caso, el NPS actúa como un proxy RADIUS.

- **Aceptar usuarios sin validar credenciales**. Con esta configuración, NPS no comprueba la identidad del usuario que intenta conectarse a la red y NPS no intenta comprobar si el usuario o el equipo tiene derecho a conectarse a la red. Cuando NPS se configura para permitir el acceso no autenticado y recibe una solicitud de conexión, NPS envía inmediatamente un mensaje de aceptación de acceso al cliente RADIUS y se concede acceso a la red al usuario o equipo. Esta configuración se utiliza para algunos tipos de tunelización obligatoria en los que el cliente de acceso se tuneliza antes de que se autentiquen las credenciales de usuario.

>[!NOTE]
>Esta opción de autenticación no se puede usar cuando el protocolo de autenticación del cliente de acceso es MS-CHAP V2 o protocolo de autenticación extensible-seguridad de la capa de transporte (EAP-TLS), y ambos proporcionan autenticación mutua. En la autenticación mutua, el cliente de acceso demuestra que es un cliente de acceso válido al servidor de autenticación (NPS) y el servidor de autenticación demuestra que es un servidor de autenticación válido para el cliente de acceso. Cuando se usa esta opción de autenticación, se devuelve el mensaje de aceptación de acceso. Sin embargo, el servidor de autenticación no proporciona validación al cliente de acceso y se produce un error en la autenticación mutua.

Para obtener ejemplos de cómo usar expresiones regulares para crear reglas de enrutamiento que reenvían mensajes RADIUS con un nombre de dominio Kerberos especificado a un grupo de servidores RADIUS remotos, consulte la sección "ejemplo de reenvío de mensajes RADIUS mediante un servidor proxy" en el tema [usar normal Expresiones en NPS](nps-crp-reg-expressions.md).

### <a name="advanced"></a>Avanzado

Puede establecer propiedades avanzadas para especificar la serie de atributos RADIUS que son:

- Se agrega al mensaje de respuesta de RADIUS cuando el NPS se usa como servidor de autenticación o cuentas RADIUS. Cuando hay atributos especificados en una directiva de red y en la Directiva de solicitud de conexión, los atributos que se envían en el mensaje de respuesta RADIUS son la combinación de los dos conjuntos de atributos.
- Se agrega al mensaje RADIUS cuando el NPS se usa como proxy de autenticación o de cuentas RADIUS. Si el atributo ya existe en el mensaje que se reenvía, se reemplaza por el valor del atributo especificado en la Directiva de solicitud de conexión. 

Además, algunos atributos que están disponibles para la configuración en la pestaña **configuración** de la Directiva de solicitud de conexión en la categoría **avanzadas** proporcionan funcionalidad especializada. Por ejemplo, puede configurar el atributo **asignación de RADIUS remota a usuario de Windows** cuando desee dividir la autenticación y autorización de una solicitud de conexión entre dos bases de datos de cuentas de usuario.

El atributo **asignación de RADIUS remota a usuario de Windows** especifica que la autorización de Windows se produce para los usuarios autenticados por un servidor RADIUS remoto. En otras palabras, un servidor RADIUS remoto realiza la autenticación en una cuenta de usuario en una base de datos de cuentas de usuario remoto, pero el NPS local autoriza la solicitud de conexión en una cuenta de usuario en una base de datos de cuentas de usuario local. Esto resulta útil cuando desea permitir el acceso de los visitantes a la red.

Por ejemplo, los visitantes de organizaciones asociadas pueden ser autenticados por su propio servidor RADIUS de la organización asociada y, a continuación, usar una cuenta de usuario de Windows en su organización para tener acceso a una red de área local (LAN) invitada en la red.

Otros atributos que proporcionan funcionalidad especializada son:

- **MS-Quarantine-IPFilter y MS-Quarantine-Session-Timeout**. Estos atributos se usan al implementar el control de cuarentena de acceso a la red (NAQC) con la implementación de VPN de enrutamiento y acceso remoto.
- **Passport-User-mapping-UPN-sufijo**. Este atributo permite autenticar las solicitudes de conexión con las credenciales de la cuenta de usuario de Windows Live™ ID.
- **Tunnel-Tag**. Este atributo designa el número de ID. de VLAN al que el NAS debe asignar la conexión al implementar redes de área local virtuales (VLAN).

## <a name="default-connection-request-policy"></a>Directiva de solicitud de conexión predeterminada

Al instalar NPS, se crea una directiva de solicitud de conexión predeterminada. Esta directiva tiene la configuración siguiente.

- La autenticación no está configurada.
- Las cuentas no están configuradas para reenviar información de cuentas a un grupo de servidores RADIUS remotos.
- El atributo no está configurado con reglas de manipulación de atributos que reenvían solicitudes de conexión a grupos de servidores RADIUS remotos.
- La solicitud de reenvío está configurada para que las solicitudes de conexión se autentiquen y se autorice en el NPS local.
- Los atributos avanzados no están configurados.

La Directiva de solicitud de conexión predeterminada usa NPS como un servidor RADIUS. Para configurar un servidor que ejecuta NPS para que actúe como proxy RADIUS, también debe configurar un grupo de servidores RADIUS remotos. Puede crear un nuevo grupo de servidores RADIUS remotos mientras crea una nueva Directiva de solicitud de conexión mediante el Asistente para nueva Directiva de solicitud de conexión. Puede eliminar la Directiva de solicitud de conexión predeterminada o comprobar que la Directiva de solicitud de conexión predeterminada sea la última Directiva procesada por NPS colocándola en último lugar en la lista ordenada de directivas.

>[!NOTE]
>Si NPS y el servicio de acceso remoto están instalados en el mismo equipo y el servicio de acceso remoto está configurado para la autenticación y contabilidad de Windows, es posible que las solicitudes de cuentas y autenticación de acceso remoto se reenvíen a un servidor RADIUS. . Esto puede ocurrir cuando las solicitudes de cuentas y autenticación de acceso remoto coinciden con una directiva de solicitud de conexión configurada para reenviarlos a un grupo de servidores RADIUS remotos.
