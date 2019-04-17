---
title: Planear NPS como un proxy de radio
description: Este tema proporciona información sobre la implementación de proxy de radio de servidor de directivas de red planificación en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca77d64a-065b-4bf2-8252-3e75f71b7734
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 966e555ebcac6c7daf4a26b322f0d29f023f8539
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="plan-nps-as-a-radius-proxy"></a>Planear NPS como un proxy de radio

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Al implementar el servidor de directivas de redes (NPS) como proxy \(RADIUS\) servicio autenticación remota telefónica de usuario, NPS recibe solicitudes de conexión de los clientes RADIUS, como los servidores de acceso de red o a otros servidores proxy de RADIUS y, a continuación, reenvía estas solicitudes de conexión a servidores que ejecuten NPS u otros servidores RADIUS. Puedes usar estas directrices de diseño para simplificar la implementación de RADIUS.

Estas directrices de diseño no incluyen circunstancias en que quieres implementar NPS como un servidor RADIUS. Cuando implementas NPS como un servidor RADIUS, NPS realiza autenticación, autorización y contabilidad para las solicitudes de conexión para el dominio local y en los dominios que confían en el dominio local.

Antes de implementar NPS como un proxy RADIUS de la red, usa las directrices siguientes para planear la implementación.

- Planear la configuración del servidor NPS.

- Planea a los clientes RADIUS.

- Planea los grupos de servidores RADIUS remotos.

- Planear las reglas de manipulación de atributo para reenvío de mensajes.

- Planear las directivas de la solicitud de conexión.

- Planear la administración de cuentas de NPS.

## <a name="plan-nps-server-configuration"></a>Planear la configuración del servidor NPS

Cuando se usa NPS como un proxy RADIUS, NPS reenvía solicitudes de conexión a un servidor NPS u otros servidores RADIUS para su procesamiento. Por este motivo, la pertenencia a dominio del servidor proxy NPS es irrelevante. El proxy no tiene que estar registrados en los servicios de dominio de Active Directory \(AD DS\) porque no tiene acceso a las propiedades de marcado de cuentas de usuario. Además, no es necesario configurar las directivas de red en un proxy NPS porque el proxy no realiza la autorización de solicitudes de conexión. El proxy NPS puede ser un miembro de dominio o puede ser un servidor independiente con ninguna pertenencia a un dominio.

NPS debe estar configurada para comunicarse con los clientes RADIUS, también denominados servidores de acceso de red mediante el protocolo RADIUS. Además, puedes configurar los tipos de eventos que grabe NPS en el caso de registro y escribir una descripción para el servidor.

### <a name="key-steps"></a>Pasos clave

Durante la planificación de la configuración del proxy NPS, puedes usar los siguientes pasos.

- Determinar los puertos de radio que utiliza el proxy NPS para recibir mensajes RADIUS de clientes RADIUS y enviar mensajes RADIUS a los miembros de grupos de servidores RADIUS remotos. Los puertos de protocolo de datagramas de usuario (UDP) predeterminados son 1812 y 1645 para mensajes de autenticación RADIUS y puertos UDP 1813 y 1646 para los mensajes RADIUS.

- Si el proxy NPS está configurado con varios adaptadores de red, determina los adaptadores sobre la que quieres que el tráfico de radio para poder.

- Determinar los tipos de eventos que desee NPS para grabar en el registro de eventos. Puede iniciar las solicitudes de conexión rechazadas, las solicitudes de conexión correcta o ambos.

- Determinar si vas a implementar más de un proxy NPS. Para proporcionar tolerancia a errores, usa a al menos dos servidores proxy NPS. Se usa un proxy NPS como proxy RADIUS principal y el otro se usa como una copia de seguridad. Cada cliente RADIUS, a continuación, se configura en tanto proxies NPS. Si el servidor NPS principal no está disponible, los clientes RADIUS envían mensajes de solicitud de acceso a lo proxy NPS alternativo.

- Planear el script que se usa para copiar una configuración de proxy NPS a otros servidores proxy de NPS para guardar en la sobrecarga administrativa y para evitar que la configuración incorrecta de un servidor. NPS proporciona los comandos Netsh que podrá copiar todo o parte de una configuración de proxy NPS para importarlos en otro servidor proxy NPS. Puedes ejecutar los comandos manualmente en el símbolo del sistema de Netsh. Sin embargo, si guardas la secuencia de comandos como un script, puedes ejecutar el script en una fecha posterior si decides cambiar la configuración de proxy.

## <a name="plan-radius-clients"></a>Clientes RADIUS de plan

Clientes de RADIUS son servidores de acceso de red, como puntos de acceso inalámbrico, servidores de red privada virtual \(VPN\), 802.1X conmutadores compatible con X y los servidores de acceso telefónico. Servidores proxy de radio, que reenviar mensajes de solicitud a servidores RADIUS de conexión, también son clientes de RADIUS. NPS es compatible con todos los servidores de acceso de red y proxy RADIUS que cumplen con el protocolo RADIUS, como se describe en la RFC 2865, "autenticación remota telefónica de usuario de servicio \(RADIUS\)," y RFC 2866, "Cuentas RADIUS".

Además, los puntos de acceso inalámbrico y cambia debe ser capaz de autenticación 802.1X. Si quieres implementar el protocolo de autenticación Extensible (EAP) o protocolo de autenticación Extensible protegido (PEAP), puntos de acceso y modificadores deben admitir el uso de EAP.

Para probar la interoperabilidad básica para las conexiones de PPP para puntos de acceso inalámbrico, configurar el punto de acceso y el cliente de acceso para usar el protocolo de autenticación de contraseña (PAP). Usa protocolos de autenticación adicional basado en PPP, como PEAP, hasta que haya probado las que vayas a usar para el acceso a la red.

### <a name="key-steps"></a>Pasos clave

Durante la planeación para clientes RADIUS, puedes usar los siguientes pasos.

- Atributos de documento específico del proveedor (VSA) debe configurar en NPS. Si tu NAS requieren VSA, registrar la información de VSA para su uso posterior al configurar las directivas de red en NPS.

- Documentar las direcciones IP de los clientes RADIUS y el proxy NPS para simplificar la configuración de todos los dispositivos. Cuando implementas los clientes RADIUS, debes configurar para usar el protocolo RADIUS, con la dirección IP del proxy NPS escrita como el servidor de autenticación. Y cuando se configura NPS para comunicarse con los clientes RADIUS, tienes que escribir las direcciones IP de cliente RADIUS en el complemento de NPS.

- Crear secretos compartidos para la configuración de los clientes RADIUS y en el complemento NPS. Debes configurar a clientes RADIUS con un secreto compartido o la contraseña, que también se especifica en el complemento NPS durante la configuración de clientes de RADIUS en NPS.

## <a name="plan-remote-radius-server-groups"></a>Planear los grupos de servidores RADIUS remotos

Al configurar un grupo de servidores RADIUS remotos en un servidor proxy NPS, se indica al proxy NPS dónde deben enviarte conexión algunos o todos los mensajes de solicitud que recibe de los servidores de acceso de red y servidores proxy NPS u otros servidores proxy de RADIUS.

Puedes usar NPS como un proxy de radio para reenviar conexión solicita a uno o más remotos grupos de servidores RADIUS y cada grupo pueden contener uno o varios servidores RADIUS. Cuando quieras que el proxy NPS reenviar mensajes a varios grupos, configurar una directiva de solicitud de conexión por grupo. La directiva de solicitud de conexión contiene información adicional, como reglas de manipulación de atributo, que establece al proxy NPS qué mensajes para enviar a los grupos de servidores RADIUS remotos especificado en la directiva.

Puedes configurar los grupos de servidores RADIUS remotos mediante los comandos Netsh para NPS, configurando grupos directamente en el complemento NPS en grupos de servidores RADIUS remotos o ejecutar al Asistente para la nueva directiva de solicitud de conexión.

### <a name="key-steps"></a>Pasos clave

Durante la planeación para grupos de servidores RADIUS remotos, puedes usar los siguientes pasos.

- Determinar los dominios que contienen los servidores RADIUS al que quieres que el proxy NPS para reenviar las solicitudes de conexión. Estos dominios contienen las cuentas de usuario para los usuarios que se conectan a la red a través de los clientes RADIUS que implementas.

- Determinar si necesitas agregar nuevos servidores RADIUS en dominios donde RADIUS ya no se ha implementado.

- Documentar las direcciones IP de servidores RADIUS que quieras agregar a grupos de servidores RADIUS remotos.

- Determinar cuántos grupos de servidores RADIUS remotos que necesitas para crear. En algunos casos, es mejor crear un grupo de servidores RADIUS remoto por dominio y, a continuación, agrega los servidores RADIUS para el dominio al grupo. Sin embargo, es posible que haya casos en los que tengan una gran cantidad de recursos en un dominio, incluido un gran número de usuarios con cuentas de usuario en el dominio, un gran número de controladores de dominio y un gran número de servidores RADIUS. O es posible que tu dominio abarque un área geográfico grande, causando que los servidores de acceso de red y los servidores RADIUS en ubicaciones que están alejadas entre sí. En estos y, posiblemente, otros casos, puedes crear varios grupos de servidores RADIUS remotos por dominio.

- Crear secretos compartidos para la configuración en el proxy NPS y en los servidores RADIUS remotos.

## <a name="plan-attribute-manipulation-rules-for-message-forwarding"></a>Planear las reglas de manipulación de atributo para reenvío de mensajes

Reglas de manipulación de atributo, que se configuran en las directivas de solicitud de conexión, te permiten identificar los mensajes de solicitud de acceso que quieras reenviar a un grupo de servidores RADIUS remoto específico.

Puedes configurar NPS para reenviar todas las solicitudes de conexión a un grupo de servidores RADIUS remotos sin usar reglas de manipulación de atributo.

Si tienes más de una ubicación a la que quieres reenviar solicitudes de conexión, sin embargo, debe crear una directiva de solicitud de conexión para cada ubicación, configurar la directiva con el grupo de servidores RADIUS remoto a las que quieras reenviar mensajes así como con las reglas de manipulación de atributos que te proporcionarán NPS qué mensajes reenviar.

Puedes crear reglas para los siguientes atributos.

- Identificador de estación llama El número de teléfono del servidor de acceso de red (NAS). El valor de este atributo es una cadena de caracteres. Puedes usar la sintaxis de coincidencia para especificar los códigos de área.

- Identificador de estación de llamada El número de teléfono usado por el llamador. El valor de este atributo es una cadena de caracteres. Puedes usar la sintaxis de coincidencia para especificar los códigos de área.

- Nombre de usuario. El nombre de usuario que es proporcionada por el cliente de acceso y que se incluye en el mensaje de solicitud de acceso RADIUS NAS. El valor de este atributo es una cadena de caracteres que normalmente contiene un nombre de dominio Kerberos y un nombre de cuenta de usuario.

Para reemplazar o convertir los nombres de dominio en el nombre de usuario de una solicitud de conexión correctamente, debes configurar las reglas de manipulación de atributo para el atributo de nombre de usuario en la directiva de solicitud de conexión apropiados.

### <a name="key-steps"></a>Pasos clave

Durante la planificación de las reglas de manipulación de atributo, puedes usar los siguientes pasos.

- Planear el enrutamiento de mensajes desde el NAS a través del proxy para los servidores RADIUS remotos para comprobar que tienes una ruta de acceso lógica con la que se va a enviar mensajes a los servidores RADIUS.

- Determinar uno o varios atributos que quieras usar para cada directiva de solicitud de conexión.

- Documentar las reglas de manipulación de atributo que vas a usar para cada directiva de solicitud de conexión y coinciden con las reglas para el grupo de servidores RADIUS remoto a la que se envían mensajes.

## <a name="plan-connection-request-policies"></a>Planear las directivas de la solicitud de conexión

La directiva de solicitud de conexión predeterminada está configurada para NPS cuando se usa como un servidor RADIUS. Las directivas de la solicitud de conexión adicional pueden usarse para definir condiciones más específicas, crear reglas de manipulación que te proporcionarán NPS que mensajes para reenviar a grupos de servidores RADIUS remotos y para especificar los atributos avanzados de atributo. Usar al Asistente para nueva directiva de solicitud de conexión para crear directivas de solicitud de conexión comunes o personalizadas.

### <a name="key-steps"></a>Pasos clave

Durante el diseño de directivas de la solicitud de conexión, puedes usar los siguientes pasos.

- Elimina la directiva predeterminada de solicitud de conexión en cada servidor NPS que funciona únicamente como un proxy RADIUS.

- Planea condiciones adicionales y opciones de configuración que son necesarios para cada directiva, la combinación de esta información con el grupo de servidores RADIUS remotos y las reglas de manipulación de atributo planificadas para la directiva.

- Diseña el plan para distribuir directivas de solicitud de conexión comunes a todos los servidores proxy NPS. Crear directivas comunes a varios servidores proxy NPS en un servidor NPS y, a continuación, usar los comandos Netsh para NPS para importar la configuración de directivas y el servidor de la solicitud de conexión en todos los otros servidores proxy.

## <a name="plan-nps-accounting"></a>Planear la administración de cuentas de NPS

Cuando se configura NPS como un proxy RADIUS, puede configurar para que realice el radio de registro de cuentas usando NPS SQL Server, archivos de registro de formato compatible con la base de datos o archivos de registro de formato NPS.

También puedes reenviar mensajes de cuentas para un grupo de servidores RADIUS remoto que realiza el registro mediante uno de estos formatos de registro.

### <a name="key-steps"></a>Pasos clave

Durante la planeación para las cuentas de NPS, puedes usar los siguientes pasos.

- Determinar si quieres que el proxy NPS para prestar servicios de contabilidad o reenviar mensajes de contabilidad a un grupo de servidores RADIUS remoto de contabilidad.

- Planear deshabilitar el proxy NPS local teniendo en cuenta si vas a reenviar contabilidad mensajes a otros servidores.

- Planea los pasos de configuración de directiva de solicitud de conexión si vas a reenviar contabilidad mensajes a otros servidores. Si deshabilitas contabilidad local para el proxy NPS, cada directiva de solicitud de conexión que configuran en esa proxy debe tener el reenvío de mensajes de contabilidad habilitado y configurado correctamente.

- Determinar el formato de registro que quieres usar: registro NPS SQL Server, archivos de registro de formato compatible con la base de datos o archivos de registro de formato IAS.

Para configurar el equilibrio de carga para NPS como proxy RADIUS, consulta [NPS Proxy Server equilibrio de carga de](nps-manage-proxy-lb.md).

