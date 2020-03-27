---
title: Planear NPS como proxy RADIUS
description: En este tema se proporciona información acerca del planeamiento de la implementación del proxy RADIUS del servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ca77d64a-065b-4bf2-8252-3e75f71b7734
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d64fceaf7242b7fe44912f105229c132ef9ee3b3
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315753"
---
# <a name="plan-nps-as-a-radius-proxy"></a>Planear NPS como proxy RADIUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Cuando se implementa el servidor de directivas de redes (NPS) como Servicio de autenticación remota telefónica de usuario \(RADIUS\) proxy, NPS recibe solicitudes de conexión de clientes RADIUS, como servidores de acceso a la red u otros proxies RADIUS y, a continuación, reenvía estas solicitudes de conexión a servidores que ejecutan NPS u otros servidores RADIUS. Puede utilizar estas directrices de planeación para simplificar la implementación de RADIUS.

Estas directrices de planeación no incluyen las circunstancias en las que desea implementar NPS como un servidor RADIUS. Cuando se implementa NPS como un servidor RADIUS, NPS realiza la autenticación, la autorización y la administración de cuentas para las solicitudes de conexión para el dominio local y para los dominios que confían en el dominio local.

Antes de implementar NPS como un proxy RADIUS en la red, use las siguientes directrices para planear la implementación.

- Planear la configuración de NPS.

- Planear clientes RADIUS.

- Planear grupos de servidores RADIUS remotos.

- Planear reglas de manipulación de atributos para el reenvío de mensajes.

- Planear directivas de solicitud de conexión.

- Planear cuentas NPS.

## <a name="plan-nps-configuration"></a>Planear la configuración de NPS

Cuando se usa NPS como un proxy RADIUS, NPS reenvía las solicitudes de conexión a un NPS u otros servidores RADIUS para su procesamiento. Por este motivo, la pertenencia al dominio del proxy NPS es irrelevante. No es necesario registrar el proxy en Active Directory Domain Services \(AD DS\) porque no necesita acceso a las propiedades de acceso telefónico de las cuentas de usuario. Además, no es necesario configurar directivas de red en un proxy NPS porque el proxy no realiza la autorización para las solicitudes de conexión. El proxy NPS puede ser un miembro de dominio o puede ser un servidor independiente sin pertenencia a dominio.

NPS debe estar configurado para comunicarse con clientes RADIUS, también denominados servidores de acceso a la red, mediante el protocolo RADIUS. Además, puede configurar los tipos de eventos que NPS registra en el registro de eventos y puede escribir una descripción para el servidor.

### <a name="key-steps"></a>Pasos clave

Durante la planeación de la configuración del proxy NPS, puede seguir estos pasos.

- Determine los puertos RADIUS que el proxy NPS usa para recibir mensajes RADIUS de clientes RADIUS y para enviar mensajes RADIUS a miembros de grupos de servidores RADIUS remotos. Los puertos del Protocolo de datagramas de usuario (UDP) predeterminados son 1812 y 1645 para los mensajes de autenticación RADIUS y los puertos UDP 1813 y 1646 para los mensajes de cuentas RADIUS.

- Si el proxy NPS está configurado con varios adaptadores de red, determine los adaptadores en los que desea que se permita el tráfico RADIUS.

- Determine los tipos de eventos que desea que NPS registre en el registro de eventos. Puede registrar solicitudes de conexión rechazadas, solicitudes de conexión correctas o ambas.

- Determine si va a implementar más de un proxy NPS. Para proporcionar tolerancia a errores, utilice al menos dos servidores proxy NPS. Se usa un proxy NPS como proxy RADIUS principal y el otro como copia de seguridad. Después, cada cliente RADIUS se configura en ambos servidores proxy NPS. Si el proxy NPS principal deja de estar disponible, los clientes RADIUS enviarán mensajes de solicitud de acceso al proxy NPS alternativo.

- Planee el script usado para copiar una configuración de proxy NPS en otros proxies NPS para ahorrar en la sobrecarga administrativa y para evitar la configuración incorrecta de un servidor. NPS proporciona los comandos Netsh que permiten copiar la configuración de un proxy NPS o parte de ella para su importación en otro proxy NPS. Puede ejecutar los comandos manualmente en el símbolo del sistema de Netsh. Sin embargo, si guarda la secuencia de comandos como un script, puede ejecutar el script en una fecha posterior si decide cambiar las configuraciones de proxy.

## <a name="plan-radius-clients"></a>Planeación de clientes RADIUS

Los clientes RADIUS son servidores de acceso a la red, como puntos de acceso inalámbricos, redes privadas virtuales \(servidores VPN\), conmutadores compatibles con 802.1 X y servidores de acceso telefónico. Los proxies RADIUS, que reenvían los mensajes de solicitud de conexión a los servidores RADIUS, también son clientes RADIUS. NPS es compatible con todos los servidores de acceso a la red y los proxies RADIUS que cumplen con el protocolo RADIUS, como se describe en RFC 2865, "servicio de usuario de acceso telefónico de autenticación remota \(RADIUS\)," y RFC 2866, "cuentas RADIUS".

Además, los puntos de acceso inalámbrico y los conmutadores deben ser capaces de la autenticación de 802.1 X. Si desea implementar el protocolo de autenticación extensible (EAP) o el protocolo de autenticación extensible protegido (PEAP), los puntos de acceso y los conmutadores deben admitir el uso de EAP.

Para probar la interoperabilidad básica de las conexiones PPP para puntos de acceso inalámbricos, configure el punto de acceso y el cliente de acceso para usar el protocolo de autenticación de contraseña (PAP). Use protocolos de autenticación basados en PPP adicionales, como PEAP, hasta que haya probado los que piensa usar para el acceso a la red.

### <a name="key-steps"></a>Pasos clave

Durante la planeación de clientes RADIUS, puede seguir estos pasos.

- Documente los atributos específicos del proveedor (VSA) que debe configurar en NPS. Si los NAS requieren VSA, registre la información de VSA para su uso posterior al configurar las directivas de red en NPS.

- Documente las direcciones IP de los clientes RADIUS y el proxy NPS para simplificar la configuración de todos los dispositivos. Al implementar los clientes RADIUS, debe configurarlos para que usen el protocolo RADIUS, con la dirección IP del proxy NPS especificada como servidor de autenticación. Y al configurar NPS para que se comunique con los clientes RADIUS, debe especificar las direcciones IP del cliente RADIUS en el complemento NPS.

- Cree secretos compartidos para la configuración en los clientes RADIUS y en el complemento NPS. Debe configurar los clientes RADIUS con un secreto compartido, o una contraseña, que también escribirá en el complemento NPS mientras configura los clientes RADIUS en NPS.

## <a name="plan-remote-radius-server-groups"></a>Planear grupos de servidores RADIUS remotos

Al configurar un grupo de servidores RADIUS remotos en un proxy NPS, está indicando al proxy NPS dónde debe enviar algunos o todos los mensajes de solicitud de conexión que recibe de los servidores de acceso a la red y los proxies NPS u otros servidores proxy RADIUS.

Puede usar NPS como proxy RADIUS para reenviar las solicitudes de conexión a uno o varios grupos de servidores RADIUS remotos, y cada grupo puede contener uno o más servidores RADIUS. Si desea que el proxy NPS reenvíe mensajes a varios grupos, configure una directiva de solicitud de conexión por grupo. La Directiva de solicitud de conexión contiene información adicional, como reglas de manipulación de atributos, que indican al proxy NPS qué mensajes se envían al grupo de servidores remotos RADIUS especificado en la Directiva.

Puede configurar grupos de servidores RADIUS remotos mediante los comandos Netsh para NPS, mediante la configuración de grupos directamente en el complemento NPS en grupos de servidores RADIUS remotos o mediante la ejecución del Asistente para nueva Directiva de solicitud de conexión.

### <a name="key-steps"></a>Pasos clave

Durante la planeación de los grupos de servidores RADIUS remotos, puede usar los pasos siguientes.

- Determine los dominios que contienen los servidores RADIUS a los que desea que el proxy NPS reenvíe las solicitudes de conexión. Estos dominios contienen las cuentas de usuario de los usuarios que se conectan a la red a través de los clientes RADIUS que se implementan.

- Determine si necesita agregar nuevos servidores RADIUS en los dominios en los que no se ha implementado RADIUS.

- Documente las direcciones IP de los servidores RADIUS que desea agregar a los grupos de servidores RADIUS remotos.

- Determine el número de grupos de servidores RADIUS remotos que debe crear. En algunos casos, es mejor crear un grupo de servidores RADIUS remotos por dominio y, a continuación, agregar los servidores RADIUS del dominio al grupo. Sin embargo, puede haber casos en los que tenga una gran cantidad de recursos en un dominio, incluido un gran número de usuarios con cuentas de usuario en el dominio, un gran número de controladores de dominio y un gran número de servidores RADIUS. O bien, el dominio puede abarcar un área geográfica grande, lo que hace que tenga servidores de acceso a la red y servidores RADIUS en ubicaciones que están lejanas entre sí. En estos y, posiblemente, en otros casos, puede crear varios grupos de servidores RADIUS remotos por dominio.

- Cree secretos compartidos para la configuración en el proxy NPS y en los servidores RADIUS remotos.

## <a name="plan-attribute-manipulation-rules-for-message-forwarding"></a>Planear reglas de manipulación de atributos para el reenvío de mensajes

Las reglas de manipulación de atributos, que están configuradas en directivas de solicitud de conexión, le permiten identificar los mensajes de solicitud de acceso que desea reenviar a un grupo de servidores RADIUS remotos específico.

Puede configurar NPS para que reenvíe todas las solicitudes de conexión a un grupo de servidores remotos RADIUS sin usar reglas de manipulación de atributos.

Sin embargo, si tiene más de una ubicación a la que desea reenviar las solicitudes de conexión, debe crear una directiva de solicitud de conexión para cada ubicación y, a continuación, configurar la Directiva con el grupo de servidores remotos RADIUS al que desea reenviar los mensajes, así como con las reglas de manipulación de atributos que indican a NPS qué mensajes se deben reenviar.

Puede crear reglas para los siguientes atributos.

- IDENTIFICADOR de estación llamada. Número de teléfono del servidor de acceso a la red (NAS). El valor de este atributo es una cadena de caracteres. Puede usar una sintaxis de coincidencia de patrón para especificar códigos de área.

- IDENTIFICADOR de la estación de llamada. Número de teléfono usado por el autor de la llamada. El valor de este atributo es una cadena de caracteres. Puede usar una sintaxis de coincidencia de patrón para especificar códigos de área.

- Nombre del usuario. El nombre de usuario proporcionado por el cliente de acceso y que el NAS incluye en el mensaje de solicitud de acceso de RADIUS. El valor de este atributo es una cadena de caracteres que normalmente contiene un nombre de dominio Kerberos y un nombre de cuenta de usuario.

Para reemplazar o convertir correctamente nombres de dominio Kerberos en el nombre de usuario de una solicitud de conexión, debe configurar reglas de manipulación de atributos para el atributo de nombre de usuario en la Directiva de solicitud de conexión adecuada.

### <a name="key-steps"></a>Pasos clave

Durante la planeación de las reglas de manipulación de atributos, puede usar los pasos siguientes.

- Planee el enrutamiento de mensajes desde el servidor NAS a través del proxy a los servidores RADIUS remotos para comprobar que tiene una ruta de acceso lógica con la que reenviar los mensajes a los servidores RADIUS.

- Determine uno o más atributos que desee usar para cada directiva de solicitud de conexión.

- Documente las reglas de manipulación de atributos que va a usar para cada directiva de solicitud de conexión y haga coincidir las reglas con el grupo de servidores RADIUS remotos al que se reenvían los mensajes.

## <a name="plan-connection-request-policies"></a>Planear directivas de solicitud de conexión

La Directiva de solicitud de conexión predeterminada está configurada para NPS cuando se usa como un servidor RADIUS. Las directivas de solicitud de conexión adicionales se pueden usar para definir condiciones más específicas, crear reglas de manipulación de atributos que indican a NPS qué mensajes se deben reenviar a grupos de servidores RADIUS remotos y especificar atributos avanzados. Use el Asistente para nueva Directiva de solicitud de conexión para crear directivas de solicitud de conexión comunes o personalizadas.

### <a name="key-steps"></a>Pasos clave

Durante la planeación de las directivas de solicitud de conexión, puede usar los pasos siguientes.

- Elimine la Directiva de solicitud de conexión predeterminada en cada servidor que ejecute NPS que funcione exclusivamente como proxy RADIUS.

- Planee las condiciones y la configuración adicionales que se requieren para cada Directiva, combinando esta información con el grupo de servidores RADIUS remoto y las reglas de manipulación de atributos planeadas para la Directiva.

- Diseñe el plan para distribuir directivas de solicitud de conexión comunes a todos los servidores proxy NPS. Cree directivas comunes a varios servidores proxy NPS en un NPS y, a continuación, use los comandos Netsh para NPS para importar las directivas de solicitud de conexión y la configuración del servidor en todos los demás servidores proxy.

## <a name="plan-nps-accounting"></a>Planear cuentas NPS

Al configurar NPS como un proxy RADIUS, puede configurarlo para realizar cuentas RADIUS mediante el uso de archivos de registro de formato NPS, archivos de registro de formato compatible con la base de datos o el registro de SQL Server de NPS.

También puede reenviar mensajes de cuentas a un grupo de servidores RADIUS remotos que realice cuentas mediante uno de estos formatos de registro.

### <a name="key-steps"></a>Pasos clave

Durante el planeamiento de cuentas NPS, puede seguir estos pasos.

- Determine si desea que el proxy NPS realice servicios de contabilidad o reenvíe mensajes de cuentas a un grupo de servidores RADIUS remotos para cuentas.

- Planee la deshabilitación de cuentas de proxy NPS locales si planea reenviar mensajes de cuentas a otros servidores.

- Planee los pasos de configuración de la Directiva de solicitud de conexión si planea reenviar mensajes de cuentas a otros servidores. Si deshabilita la contabilidad local para el proxy NPS, cada directiva de solicitud de conexión que configure en ese proxy debe tener habilitado el reenvío de mensajes de contabilidad y configurado correctamente.

- Determine el formato de registro que desea usar: los archivos de registro de formato de IAS, los archivos de registro de formato compatible con la base de datos o el registro de SQL Server de NPS.

Para configurar el equilibrio de carga para NPS como proxy RADIUS, consulte [equilibrio de carga del servidor proxy NPS](nps-manage-proxy-lb.md).

