---
title: Directivas de solicitud de conexión
description: En este tema se proporciona información general sobre las directivas de solicitud de conexión del servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 4ec45e0c-6b37-4dfb-8158-5f40677b0157
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: a4fc6b2e7342f59f971ee42848f75e6598c1c263
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947801"
---
# <a name="connection-request-policies"></a>Directivas de solicitud de conexión

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para aprender a usar las directivas de solicitud de conexión NPS para configurar NPS como un servidor RADIUS, un proxy RADIUS o ambos.

>[!NOTE]
>Además de este tema, está disponible la siguiente documentación de la Directiva de solicitud de conexión.
> - [Configurar directivas de solicitud de conexión](nps-crp-configure.md)
> - [Configurar grupos de servidores RADIUS remotos](nps-crp-rrsg-configure.md)

Las directivas de solicitud de conexión son conjuntos de condiciones y configuraciones que permiten a los administradores de red designar qué servidores Servicio de autenticación remota telefónica de usuario (RADIUS) realizan la autenticación y autorización de las solicitudes de conexión que el servidor que ejecuta el servidor de directivas de redes (NPS) recibe de los clientes RADIUS. Las directivas de solicitud de conexión se pueden configurar para designar los servidores RADIUS que se usan para las cuentas RADIUS.

Puede crear directivas de solicitud de conexión para que algunos mensajes de solicitud RADIUS enviados desde clientes RADIUS se procesen localmente (NPS se usa como un servidor RADIUS) y otros tipos de mensajes se reenvíen a otro servidor RADIUS (NPS se usa como proxy RADIUS).

Con las directivas de solicitud de conexión, puede usar NPS como un servidor RADIUS o como proxy RADIUS, en función de factores como los siguientes:

- La hora del día y el día de la semana
- El nombre de dominio kerberos en la solicitud de conexión
- El tipo de conexión que se solicita
- La dirección IP del cliente RADIUS

NPS procesa o reenvía los mensajes de Access-Request RADIUS solo si la configuración del mensaje entrante coincide con al menos una de las directivas de solicitud de conexión configuradas en el NPS.

Si la configuración de la Directiva coincide y la Directiva requiere que el NPS procese el mensaje, NPS actúa como un servidor RADIUS, autenticando y autorizando la solicitud de conexión. Si la configuración de la Directiva coincide y la Directiva requiere que NPS reenvíe el mensaje, NPS actúa como proxy RADIUS y reenvía la solicitud de conexión a un servidor RADIUS remoto para su procesamiento.

Si la configuración de un mensaje entrante de solicitud de acceso RADIUS no coincide como mínimo con una de las directivas de solicitud de conexión, se envía un mensaje de rechazo de acceso al cliente RADIUS y se deniega el acceso al usuario o al equipo que intenta conectarse a la red.

## <a name="configuration-examples"></a>Ejemplos de configuración

Los siguientes ejemplos de configuración muestran cómo se pueden usar las directivas de solicitud de conexión.

### <a name="nps-as-a-radius-server"></a>NPS como servidor RADIUS

La directiva de solicitud de conexión predeterminada es la única directiva configurada. En este ejemplo, NPS se configura como un servidor RADIUS y el NPS local procesa todas las solicitudes de conexión. NPS puede autenticar y autorizar a los usuarios cuyas cuentas están en el dominio del dominio NPS y en los dominios de confianza.


### <a name="nps-as-a-radius-proxy"></a>NPS como proxy RADIUS

La directiva de solicitud de conexión predeterminada se elimina y se crean dos nuevas directivas de solicitud de conexión para reenviar solicitudes a dos dominios diferentes. En este ejemplo, NPS se configura como un proxy RADIUS. NPS no procesa ninguna solicitud de conexión en el servidor local. En su lugar, reenvía las solicitudes de conexión a un servidor NPS o a otros servidores RADIUS configurados como miembros de grupos de servidores RADIUS remotos.


### <a name="nps-as-both-radius-server-and-radius-proxy"></a>NPS como servidor RADIUS y proxy RADIUS

Además de la directiva de solicitud de conexión predeterminada, se crea una nueva directiva de solicitud de conexión que reenvía solicitudes de conexión a NPS u otro servidor RADIUS en un dominio que no es de confianza. En este ejemplo, la directiva de proxy aparece en primer lugar en la lista ordenada de directivas. Si la solicitud de conexión coincide con la directiva de proxy, la solicitud de conexión se reenvía al servidor RADIUS en el grupo de servidores remotos RADIUS. Si la solicitud de conexión no coincide con la directiva proxy pero sí coincide con la directiva de solicitud de conexión predeterminada, NPS procesa la solicitud de conexión en el servidor local. Si la solicitud de conexión no coincide con ninguna de las dos directivas, se descarta.

### <a name="nps-as-radius-server-with-remote-accounting-servers"></a>NPS como servidor RADIUS con servidores de cuentas remotas

En este ejemplo, el NPS local no está configurado para realizar cuentas y la Directiva de solicitud de conexión predeterminada se revisa para que los mensajes de cuentas RADIUS se reenvíen a un NPS u otro servidor RADIUS en un grupo de servidores RADIUS remotos. Aunque se reenvían los mensajes de cuentas, no se reenvían los mensajes de autenticación y autorización, y el NPS local realiza estas funciones para el dominio local y todos los dominios de confianza.


### <a name="nps-with-remote-radius-to-windows-user-mapping"></a>NPS con asignación de RADIUS remota a usuario de Windows

En este ejemplo, NPS actúa como servidor RADIUS y como proxy RADIUS para cada solicitud de conexión individual ya que reenvía la solicitud de autenticación a un servidor RADIUS remoto y, al mismo tiempo, usa la cuenta de usuario de Windows local para la autorización. Para implementar esta configuración, se debe establecer el atributo Asignación de RADIUS remota a usuario de Windows como una condición de la directiva de solicitud de conexión. (Además, se debe crear una cuenta de usuario local que tenga el mismo nombre que la cuenta de usuario remota que será la que usará el servidor RADIUS remoto para realizar la autenticación.)

## <a name="connection-request-policy-conditions"></a>Condiciones de la Directiva de solicitud de conexión

Las condiciones de la directiva de solicitud de conexión son uno o más atributos RADIUS que se comparan con los atributos del mensaje de solicitud de acceso RADIUS entrante. Si hay varias condiciones, entonces todas las condiciones del mensaje de solicitud de conexión y de la directiva de solicitud de conexión deben coincidir para que la directiva sea aplicada por NPS.


Los siguientes son los atributos de condición que se pueden configurar en las directivas de solicitud de conexión.

### <a name="connection-properties-attribute-group"></a>Propiedades de conexión (grupo de atributos)

El grupo de atributos Propiedades de la conexión contiene los siguientes atributos.

- **Protocolo de tramas**. Se usa para designar el tipo de entramado para los paquetes entrantes. Algunos ejemplos son Protocolo punto a punto (PPP), protocolo de Internet de línea de serie (SLIP), Frame Relay y X. 25.
- **Tipo de servicio**. Se usa para designar el tipo de servicio que se solicita. Entre los ejemplos se incluyen entramados (por ejemplo, conexiones PPP) y de inicio de sesión (por ejemplo, conexiones Telnet). Para obtener más información acerca de los tipos de servicio RADIUS, consulte la RFC 2865, que se refiere al Servicio de autenticación remota telefónica de usuario (RADIUS).
- **Tipo de túnel**. Se usa para designar el tipo de túnel que está creando el cliente que realiza la solicitud. Entre los tipos de túnel se incluyen el protocolo de túnel punto a punto (PPTP) y el protocolo de túnel de capa dos (L2TP).

### <a name="day-and-time-restrictions-attribute-group"></a>Grupo de atributos de restricciones de día y hora

El grupo de atributos Restricciones de día y hora contiene un atributo de Restricciones de día y hora. Con este atributo, puede designar el día de la semana y la hora del día del intento de conexión. El día y la hora son relativos al día y la hora del NPS.

### <a name="gateway-attribute-group"></a>Grupo de atributos de puerta de enlace

El grupo de atributos Puerta de enlace contiene los siguientes atributos.

- **Identificador de estación llamado**. Se usa para designar el número de teléfono del servidor de acceso a la red. Este atributo es una cadena de caracteres. Puede usar una sintaxis de coincidencia de patrón para especificar códigos de área.
- **Identificador de NAS**. Se usa para designar el nombre del servidor de acceso a la red. Este atributo es una cadena de caracteres. Puede usar una sintaxis de coincidencia de patrón para especificar identificadores de NAS.
- **Dirección IPv4 de NAS**. Se usa para designar la dirección del Protocolo de Internet versión 4 (IPv4) del servidor de acceso a la red (el cliente RADIUS). Este atributo es una cadena de caracteres. Puede usar una sintaxis de coincidencia de patrón para especificar redes IP.
- **Dirección IPv6 de NAS**. Se usa para designar la dirección del Protocolo de Internet versión 6 (IPv6) del servidor de acceso a la red (el cliente RADIUS). Este atributo es una cadena de caracteres. Puede usar una sintaxis de coincidencia de patrón para especificar redes IP.
- **Tipo de puerto de NAS**. Se usa para designar el tipo de medios usados por el cliente de acceso. Algunos ejemplos son las líneas telefónicas analógicas (conocidas como asincrónicas), la red digital de servicios integrados (RDSI), túneles o redes privadas virtuales (VPN), IEEE 802,11 inalámbrica y conmutadores Ethernet.

### <a name="machine-identity-attribute-group"></a>Grupo de atributos de identidad de equipo

El grupo de atributos ///Identidad del equipo contiene el atributo ///Identidad del equipo. Mediante este atributo, puede especificar el método con el que se identifican los clientes en la Directiva.

### <a name="radius-client-properties-attribute-group"></a>Grupo de atributos de propiedades de cliente RADIUS

El grupo de atributos Propiedades del cliente RADIUS contiene los siguientes atributos.

- **Identificador de estación que llama**. Se usa para designar el número de teléfono usado por quien llama (el cliente de acceso). Este atributo es una cadena de caracteres. Puede usar una sintaxis de coincidencia de patrón para especificar códigos de área.  En las autenticaciones de 802.1 x, la dirección MAC se rellena normalmente y se puede hacer coincidir desde el cliente.  Este campo se usa normalmente para escenarios de omisión de direcciones MAC cuando la Directiva de solicitud de conexión está configurada para "aceptar usuarios sin validar credenciales".
- **Nombre descriptivo del cliente**. Se usa para designar el nombre del equipo cliente RADIUS que solicita autenticación. Este atributo es una cadena de caracteres. Puede usar una sintaxis de coincidencia de patrón para especificar nombres de cliente.
- **Dirección IPv4 del cliente**. Se usa para designar la dirección IPv4 del servidor de acceso a la red (el cliente RADIUS). Este atributo es una cadena de caracteres. Puede usar una sintaxis de coincidencia de patrón para especificar redes IP.
- **Dirección IPv6 del cliente**. Se usa para designar la dirección IPv6 del servidor de acceso a la red (el cliente RADIUS). Este atributo es una cadena de caracteres. Puede usar una sintaxis de coincidencia de patrón para especificar redes IP.
- **Proveedor del cliente**. Se usa para designar el proveedor del servidor de acceso a la red que solicita autenticación. Un equipo que ejecuta el Servicio de enrutamiento y acceso remoto es el fabricante de Microsoft NAS. Puede usar este atributo para configurar directivas independientes para diferentes fabricantes de NAS. Este atributo es una cadena de caracteres. Puede usar una sintaxis de coincidencia de patrón.

### <a name="user-name-attribute-group"></a>Grupo de atributos de nombre de usuario

El grupo de atributos Nombre de usuario contiene el atributo Nombre de usuario. Con este atributo, puede designar el nombre de usuario, o una parte del nombre de usuario, que debe coincidir con el nombre de usuario proporcionado por el cliente de acceso en el mensaje RADIUS. Este atributo es una cadena de caracteres que normalmente contiene un nombre de dominio kerberos y un nombre de cuenta de usuario. Puede usar una sintaxis de coincidencia de patrón para especificar nombres de usuario.

## <a name="connection-request-policy-settings"></a>Configuración de la Directiva de solicitud de conexión

La configuración de la directiva de solicitud de conexión es un conjunto de propiedades que se aplican a un mensaje RADIUS entrante. La configuración consta de los siguientes grupos de propiedades.

- Authentication
- Control
- Manipulación de atributos
- Reenviando solicitud
- Avanzado

En las secciones siguientes se proporcionan detalles adicionales acerca de esta configuración.

### <a name="authentication"></a>Authentication

Al usar esta opción, puede invalidar la configuración de autenticación que se configura en todas las directivas de red y puede designar los métodos y tipos de autenticación que se necesitan para conectarse a la red.

>[!IMPORTANT]
>Si configura un método de autenticación en una directiva de solicitud de conexión que sea menos segura que el método de autenticación que configure en la Directiva de red, se invalidará el método de autenticación más seguro que configure en la Directiva de red. Por ejemplo, si tiene una directiva de red que requiere el uso de la autenticación extensible protegida Protocol-Microsoft protocolo de autenticación por desafío mutuo versión 2 \( PEAP-MS-CHAP V2 \) , que es un método de autenticación basado en contraseña para una red inalámbrica segura y también se configura una directiva de solicitud de conexión para permitir el acceso no autenticado, el resultado es que no es necesario que los clientes se autentiquen mediante PEAP-MS-CHAP v2. En este ejemplo, se concede a todos los clientes que se conectan a la red un acceso no autenticado.

### <a name="accounting"></a>Control

Mediante esta opción, puede configurar la Directiva de solicitud de conexión para reenviar información de cuentas a un NPS u otro servidor RADIUS en un grupo de servidores RADIUS remotos para que el grupo de servidores remotos RADIUS realice cuentas.

>[!NOTE]
>Si tiene varios servidores RADIUS y desea que la información de cuentas para todos los servidores se almacene en una base de datos de cuentas RADIUS central, puede usar la configuración de cuentas de la directiva de solicitud de conexión en una directiva en cada servidor RADIUS para reenviar datos de cuentas de todos los servidores a un servidor NPS o u otro servidor RADIUS que se designa como servidor de cuentas.

La configuración de cuentas de la Directiva de solicitud de conexión funciona independientemente de la configuración de cuentas del NPS local. En otras palabras, si configura el NPS local para registrar información de cuentas de RADIUS en un archivo local o en una base de datos de Microsoft SQL Server, lo hará independientemente de si configura una directiva de solicitud de conexión para reenviar mensajes de cuentas a un grupo de servidores RADIUS remotos.

Si desea que la información de cuentas se registre de forma remota pero no local, debe configurar el NPS local para que no realice cuentas, a la vez que configura también cuentas en una directiva de solicitud de conexión para reenviar datos de cuentas a un grupo de servidores RADIUS remotos.

### <a name="attribute-manipulation"></a>Manipulación de atributos

Puede configurar un conjunto de reglas de búsqueda y reemplazo que manipulen las cadenas de texto de uno de los siguientes atributos.

- Nombre de usuario
- Identificador de estación llamada
- Identificador de estación que llama

El procesamiento de la regla de buscar y reemplazar se produce para uno de los atributos precedentes antes de que el mensaje RADIUS se someta a la configuración de autenticación y administración de cuentas. Las reglas de manipulación de atributos se aplican a un solo atributo. No puede configurar reglas de manipulación de atributos para cada atributo. Además, la lista de atributos que se pueden manipular es una lista estática: no se pueden agregar atributos a la lista de atributos disponibles para su manipulación.

>[!NOTE]
>Si usa el protocolo de autenticación MS-CHAP v2, no puede manipular el atributo de Nombre de usuario si la directiva de solicitud de conexión se usa para reenviar el mensaje RADIUS. La única excepción se produce cuando se usa una barra diagonal inversa ( \) carácter y la manipulación solo afecta a la información que se encuentra a su izquierda). El carácter de barra diagonal inversa se usa normalmente para indicar un nombre de dominio (la información a la izquierda del carácter de barra diagonal inversa) y el nombre de una cuenta de usuario dentro del dominio (la información a la derecha del carácter de barra diagonal inversa). En este caso, sólo se permiten las reglas de manipulación de atributos que modifican o reemplazan el nombre de dominio.

Para ver ejemplos de cómo manipular el nombre de dominio Kerberos en el atributo de nombre de usuario, consulte la sección "ejemplos de manipulación del nombre de dominio Kerberos en el atributo de nombre de usuario" en el tema [uso de expresiones regulares en NPS](nps-crp-reg-expressions.md).

### <a name="forwarding-request"></a>Reenviando solicitud

Puede establecer las siguientes opciones de reenvío de solicitud que se usan para mensajes de solicitud de acceso de RADIUS:

- **Autenticar solicitudes en este servidor**. Con esta configuración, NPS usa un dominio de Windows NT 4,0, Active Directory o la base de datos de cuentas de usuario del administrador de cuentas de seguridad (SAM) local para autenticar la solicitud de conexión. Esta configuración también especifica que la directiva de red coincidente que se configura en NPS, junto con las propiedades de marcado de la cuenta de usuario, sean usadas por NPS para autorizar la solicitud de conexión. En este caso, el NPS está configurado para ejecutarse como un servidor RADIUS.

- **Reenviar solicitudes al siguiente grupo de servidores RADIUS remotos**. Al usar esta configuración, NPS reenvía las solicitudes de conexión al grupo de servidores RADIUS remoto que especifique. Si el NPS recibe un mensaje de Access-Accept válido que corresponde al mensaje de Access-Request, el intento de conexión se considera autenticado y autorizado. En este caso, el NPS actúa como un proxy RADIUS.

- **Aceptar usuarios sin validar credenciales**. Con esta configuración, NPS no comprueba la identidad del usuario que intenta conectarse a la red y NPS no intenta comprobar si el usuario o el equipo tiene derecho a conectarse a la red. Cuando NPS se configura para permitir el acceso no autenticado y recibe una solicitud de conexión, NPS envía de inmediato un mensaje de aceptación de acceso al cliente RADIUS y se concede al usuario o el equipo acceso a la red. Esta configuración se utiliza para algunos tipos de tunelización obligatoria en los que el cliente de acceso se tuneliza antes de que se autentiquen las credenciales de usuario.

>[!NOTE]
>Esta opción de autenticación no se puede usar cuando el protocolo de autenticación del cliente de acceso es MS-CHAP V2 o la autenticación extensible Protocol-Transport la seguridad de la capa (EAP-TLS), y ambas proporcionan autenticación mutua. En la autenticación mutua, el cliente de acceso demuestra que es un cliente de acceso válido al servidor de autenticación (NPS) y el servidor de autenticación demuestra que es un servidor de autenticación válido para el cliente de acceso. Cuando se usa esta opción de autenticación, se devuelve el mensaje de aceptación de acceso. Sin embargo, el servidor de autenticación no proporciona validación al cliente de acceso y se produce un error en la autenticación mutua.

Para obtener ejemplos de cómo usar expresiones regulares para crear reglas de enrutamiento que reenvían mensajes RADIUS con un nombre de dominio Kerberos especificado a un grupo de servidores RADIUS remotos, consulte la sección "ejemplo de reenvío de mensajes RADIUS por un servidor proxy" en el tema [uso de expresiones regulares en NPS](nps-crp-reg-expressions.md).

### <a name="advanced"></a>Avanzado

Puede configurar propiedades avanzadas para especificar la serie de atributos RADIUS que son:

- Se agrega al mensaje de respuesta de RADIUS cuando el NPS se usa como servidor de autenticación o cuentas RADIUS. Cuando hay atributos especificados en una directiva de red y la directiva de solicitud de conexión, los atributos que se envían al mensaje de respuesta RADIUS son la combinación de ambos conjuntos de atributos.
- Se agrega al mensaje RADIUS cuando el NPS se usa como proxy de autenticación o de cuentas RADIUS. Si el atributo ya existe en el mensaje que se reenvía, se reemplaza por el valor del atributo especificado en la directiva de solicitud de conexión.

Además, algunos atributos que están disponibles para la configuración en la pestaña **configuración** de la Directiva de solicitud de conexión en la categoría **avanzadas** proporcionan funcionalidad especializada. Por ejemplo, puede configurar el atributo **asignación de RADIUS remota a usuario de Windows** cuando desee dividir la autenticación y autorización de una solicitud de conexión entre dos bases de datos de cuentas de usuario.

El atributo **asignación de RADIUS remota a usuario de Windows** especifica que la autorización de Windows se produce para los usuarios autenticados por un servidor RADIUS remoto. En otras palabras, un servidor RADIUS remoto realiza la autenticación en una cuenta de usuario en una base de datos de cuentas de usuario remoto, pero el NPS local autoriza la solicitud de conexión en una cuenta de usuario en una base de datos de cuentas de usuario local. Esto resulta útil cuando se desea permitir el acceso a la red por parte de visitantes.

Por ejemplo, los visitantes de organizaciones asociadas pueden ser autenticados por su propio servidor RADIUS de la organización asociada y, a continuación, usar una cuenta de usuario de Windows en su organización para tener acceso a una red de área local (LAN) invitada en la red.

Otros atributos que proporcionan funciones especializadas son:

- **MS-Quarantine-IPFilter y MS-Quarantine-Session-Timeout**. Estos atributos se usan al implementar el Control de cuarentena de acceso a la red (NAQC) con la implementación de VPN de Enrutamiento y acceso remoto.
- **Passport-User-mapping-UPN-sufijo**. Este atributo permite autenticar solicitudes de conexión con credenciales de cuenta de usuario de identificador Windows Live™.
- **Tunnel-Tag**. Este atributo designa el número de identificador de VLAN al que la conexión debe ser asignada por el NAS cuando se implementan redes de área local virtuales (VLAN).

## <a name="default-connection-request-policy"></a>Directiva de solicitud de conexión predeterminada

Al instalar NPS se crea una directiva de solicitud de conexión predeterminada. Esta directiva tiene la configuración siguiente.

- Autenticación no se configura.
- Cuentas no se configura para reenviar información de cuentas a un grupo de servidores remotos RADIUS.
- Atributo no se configura con reglas de manipulación de atributos que reenvían solicitudes de conexión a grupos de servidores remotos RADIUS.
- La solicitud de reenvío está configurada para que las solicitudes de conexión se autentiquen y se autorice en el NPS local.
- Los atributos de Opciones avanzadas no se configuran.

La directiva de solicitud de conexión predeterminada usa NPS como servidor RADIUS. Para configurar un servidor que ejecuta NPS para funcionar como proxy RADIUS, también debe configurar un grupo de servidores remotos RADIUS. Puede crear un nuevo grupo de servidores RADIUS remotos mientras crea una nueva Directiva de solicitud de conexión mediante el Asistente para nueva Directiva de solicitud de conexión. Puede eliminar la Directiva de solicitud de conexión predeterminada o comprobar que la Directiva de solicitud de conexión predeterminada sea la última Directiva procesada por NPS colocándola en último lugar en la lista ordenada de directivas.

>[!NOTE]
>Si NPS y el servicio de acceso remoto están instalados en el mismo equipo y el servicio de acceso remoto está configurado para la autenticación y contabilidad de Windows, es posible que las solicitudes de cuentas y autenticación de acceso remoto se reenvíen a un servidor RADIUS. Esto puede ocurrir cuando las solicitudes de cuentas y autenticación de acceso remoto coinciden con una directiva de solicitud de conexión configurada para reenviarlos a un grupo de servidores RADIUS remotos.
