---
title: Planear NPS como proxy RADIUS
description: En este tema se proporciona información acerca de la implementación de proxy de RADIUS de servidor de directivas de red planeación en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca77d64a-065b-4bf2-8252-3e75f71b7734
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 83fbe57ee62480439190dcc53428e02a4f8e6897
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829566"
---
# <a name="plan-nps-as-a-radius-proxy"></a>Planear NPS como proxy RADIUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Al implementar el servidor de directivas de redes (NPS) como un Remote Authentication Dial-in User Service \(RADIUS\) proxy, NPS recibe las solicitudes de conexión de clientes RADIUS, como servidores de acceso de red o de otros proxys RADIUS y, a continuación, reenvía estas solicitudes de conexión a servidores que ejecutan NPS u otros servidores RADIUS. Puede utilizar estas directrices de planeamiento para simplificar la implementación de RADIUS.

Estas instrucciones para la planeación no incluyen circunstancias en que desea implementar NPS como servidor RADIUS. Al implementar NPS como servidor RADIUS, NPS realiza la autenticación, autorización y contabilidad para las solicitudes de conexión para el dominio local y para dominios que confían en el dominio local.

Antes de implementar NPS como proxy RADIUS en la red, utilice las siguientes directrices para planear la implementación.

- Planear la configuración de NPS.

- Planear a los clientes RADIUS.

- Planear grupos de servidores RADIUS remotos.

- Planear las reglas de manipulación de atributos para el reenvío de mensajes.

- Planeación de las directivas de solicitud de conexión.

- Plan de contabilidad de NPS.

## <a name="plan-nps-configuration"></a>Planear la configuración de NPS

Cuando se usa NPS como proxy RADIUS, NPS reenvía las solicitudes de conexión a NPS u otros servidores RADIUS para su procesamiento. Por este motivo, la pertenencia al dominio del proxy NPS es irrelevante. El proxy no necesita estar registrado en Active Directory Domain Services \(AD DS\) porque no necesita acceso a las propiedades de acceso telefónico de cuentas de usuario. Además, no es necesario configurar las directivas de red en un proxy NPS porque el proxy no realiza la autorización para las solicitudes de conexión. El proxy NPS puede ser un miembro del dominio o puede ser un servidor independiente con ninguna pertenencia al dominio.

NPS debe configurarse para comunicarse con los clientes RADIUS, también denominados servidores de acceso de red, mediante el protocolo RADIUS. Además, puede configurar los tipos de eventos que registra NPS en el registro y se puede escribir una descripción para el servidor de eventos.

### <a name="key-steps"></a>Pasos clave

Durante el planeamiento de la configuración del proxy NPS, puede usar los pasos siguientes.

- Determinar los puertos RADIUS que utiliza el proxy NPS para recibir mensajes RADIUS de clientes RADIUS y enviar mensajes RADIUS a los miembros de grupos de servidores RADIUS remotos. Los puertos de protocolo de datagramas de usuario (UDP) de forma predeterminada son 1812 y 1645 para los mensajes de autenticación RADIUS y los puertos UDP 1813 y 1646 para los mensajes de cuentas RADIUS.

- Si se configura el proxy NPS con varios adaptadores de red, determinar los adaptadores a través de la que desea que se permite el tráfico RADIUS.

- Determinar los tipos de eventos que desea que NPS registre en el registro de eventos. Puede registrar las solicitudes de conexión rechazada, las solicitudes de conexión correcta o ambos.

- Determine si va a implementar más de un proxy NPS. Para proporcionar tolerancia a errores, use al menos dos proxies NPS. Se usa un proxy NPS como proxy RADIUS principal y la otra se usa como una copia de seguridad. Cada cliente RADIUS, a continuación, se configura en ambos proxies NPS. Si el proxy NPS principal deja de estar disponible, los clientes RADIUS, a continuación, envían mensajes de solicitud de acceso al proxy NPS alternativo.

- Planee la secuencia de comandos que se utilizan para copiar una configuración del proxy NPS en otros proxies NPS para guardar en la sobrecarga administrativa y evitar que la configuración incorrecta de un servidor. NPS proporciona los comandos Netsh que le permitan copiar todo o parte de una configuración del proxy NPS para importarlos a otro proxy NPS. Puede ejecutar los comandos manualmente en el símbolo del sistema de Netsh. Sin embargo, si guarda la secuencia de comandos como una secuencia de comandos, puede ejecutar el script en una fecha posterior si decide cambiar las configuraciones de proxy.

## <a name="plan-radius-clients"></a>Planear a los clientes RADIUS

Los clientes RADIUS son servidores de acceso de red, como puntos de acceso inalámbrico, red privada virtual \(VPN\) 802.1X, servidores de conmutadores compatibles con X y los servidores de acceso telefónico. Proxy RADIUS que reenvían los mensajes de solicitud a servidores RADIUS de conexión, también son clientes RADIUS. NPS es compatible con todos los servidores de acceso de red y proxy RADIUS que cumplen con el protocolo RADIUS, como se describe en RFC 2865, "Remote Authentication Dial-in User Service \(RADIUS\)," y RFC 2866, "Contabilidad de RADIUS".

Además, los puntos de acceso inalámbrico y conmutadores deben ser capaces de autenticación 802.1X. Si desea implementar el protocolo de autenticación Extensible (EAP) o protocolo de autenticación Extensible protegido (PEAP), conmutadores y puntos de acceso deben admitir el uso de EAP.

Para probar la interoperabilidad básica para las conexiones PPP para puntos de acceso inalámbrico, configure el punto de acceso y el cliente de acceso para usar el protocolo de autenticación de contraseña (PAP). Usar protocolos de autenticación adicionales basadas en PPP, como PEAP, hasta que haya probado los que se va a utilizar para el acceso a la red.

### <a name="key-steps"></a>Pasos clave

Durante el planeamiento de clientes RADIUS, puede usar los pasos siguientes.

- Documente los atributos específicos del proveedor (VSA), debe configurar en NPS. Si sus NAS requieren VSA, registrar la información de VSA para su uso posterior cuando configura las directivas de red en NPS.

- Documente las direcciones IP de clientes RADIUS y el proxy NPS para simplificar la configuración de todos los dispositivos. Al implementar los clientes RADIUS, debe configurar para que usen el protocolo RADIUS, con la dirección IP de proxy NPS especificada como el servidor de autenticación. Y cuando se configura NPS para comunicarse con los clientes RADIUS, debe especificar las direcciones IP del cliente RADIUS en el complemento NPS.

- Cree los secretos compartidos para la configuración de los clientes RADIUS y en el complemento NPS. Debe configurar a clientes RADIUS con un secreto compartido, o la contraseña, que también va a escribir en el complemento NPS al configurar clientes RADIUS en NPS.

## <a name="plan-remote-radius-server-groups"></a>Planear grupos de servidores RADIUS remotos

Al configurar un grupo de servidores RADIUS remotos en un proxy NPS, se está indicando al proxy NPS dónde debe enviar conexión algunos o todos los mensajes de solicitud que recibe de los servidores de acceso de red y servidores proxy NPS u otros proxys RADIUS.

Puede usar NPS como un proxy RADIUS para reenviar la conexión se solicita a uno o grupos de servidores RADIUS remotos y cada grupo pueden contener uno o más servidores RADIUS. Cuando quiera que el proxy NPS para reenviar mensajes a varios grupos, configurar una directiva de solicitud de conexión por grupo. La directiva de solicitud de conexión contiene información adicional, como las reglas de manipulación de atributos, que establece al proxy NPS qué mensajes para enviar al grupo de servidores RADIUS remoto especificado en la directiva.

Puede configurar grupos de servidores RADIUS remotos mediante los comandos Netsh para NPS, mediante la configuración de grupos directamente en el complemento NPS en grupos de servidores RADIUS remotos o bien ejecutando al Asistente para nueva directiva de solicitud de conexión.

### <a name="key-steps"></a>Pasos clave

Durante el planeamiento de grupos de servidores RADIUS remotos, puede usar los pasos siguientes.

- Determina los dominios que contienen los servidores RADIUS a la que desea que el proxy NPS para reenviar las solicitudes de conexión. Estos dominios contienen las cuentas de usuario para los usuarios que se conectan a la red a través de los clientes RADIUS que implementa.

- Determinar si necesita agregar nuevos servidores RADIUS en dominios que ya no está implementado RADIUS.

- Documente las direcciones IP de servidores RADIUS que desea agregar a grupos de servidores RADIUS remotos.

- Determine cuántos grupos de servidores RADIUS remotos, deberá crear. En algunos casos, es mejor crear un grupo de servidores RADIUS remoto por dominio y, a continuación, agregue los servidores RADIUS para el dominio al grupo. Sin embargo, puede haber casos en los que tienen una gran cantidad de recursos en un dominio, incluido un gran número de usuarios con cuentas de usuario en el dominio, un gran número de controladores de dominio y un gran número de servidores RADIUS. O el dominio puede cubrir una extensa área geográfica, provocando que tenga los servidores de acceso de red y servidores RADIUS en ubicaciones que están alejadas entre sí. En estos y, posiblemente, otros casos, puede crear varios grupos de servidores RADIUS remotos por dominio.

- Cree los secretos compartidos para la configuración en el proxy NPS y en los servidores RADIUS remotos.

## <a name="plan-attribute-manipulation-rules-for-message-forwarding"></a>Planear las reglas de manipulación de atributos para el reenvío de mensajes

Reglas de manipulación de atributos, que se configuran en las directivas de solicitud de conexión, podrá identificar los mensajes de solicitud de acceso que desea reenviar a un grupo de servidores RADIUS remoto específico.

Puede configurar NPS para que reenvíe todas las solicitudes de conexión a un grupo de servidores RADIUS remotos sin usar reglas de manipulación de atributos.

Si tiene más de una ubicación a la que desea reenviar las solicitudes de conexión, sin embargo, primero debe crear una directiva de solicitud de conexión para cada ubicación y luego configure la directiva con el grupo de servidores RADIUS remoto al que desea reenviar los mensajes, así como con las reglas de manipulación de atributos que indican qué mensajes reenviar a NPS.

Puede crear reglas para los siguientes atributos.

- Identificador de estación llamado El número de teléfono del servidor de acceso de red (NAS). El valor de este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrón para especificar códigos de área.

- Identificador de estación llamada El número de teléfono usado por el llamador. El valor de este atributo es una cadena de caracteres. Puede usar la sintaxis de coincidencia de patrón para especificar códigos de área.

- Nombre de usuario. El nombre de usuario proporcionado por el cliente de acceso y que se incluye en el mensaje de solicitud de acceso RADIUS NAS. El valor de este atributo es una cadena de caracteres que normalmente contiene un nombre de dominio Kerberos y un nombre de cuenta de usuario.

Para reemplazar o convertir los nombres de dominio Kerberos en nombre de usuario de una solicitud de conexión correctamente, debe configurar reglas de manipulación de atributos para el atributo de nombre de usuario en la directiva de solicitud de conexión correspondiente.

### <a name="key-steps"></a>Pasos clave

Durante la planeación de reglas de manipulación de atributos, puede usar los pasos siguientes.

- Planear el enrutamiento de mensajes desde el servidor NAS a través del proxy a los servidores RADIUS remotos para comprobar que tiene una ruta de acceso lógica con la que se va a reenviar los mensajes a los servidores RADIUS.

- Determinar uno o varios atributos que se va a usar para cada directiva de solicitud de conexión.

- Documente las reglas de manipulación de atributos que se va a usar para cada directiva de solicitud de conexión y coinciden con las reglas al grupo de servidores RADIUS remoto al que se reenvían los mensajes.

## <a name="plan-connection-request-policies"></a>Planeación de las directivas de solicitud de conexión

La directiva de solicitud de conexión predeterminada se configura para NPS cuando se usa como servidor RADIUS. Las directivas de solicitud de conexión adicionales pueden usarse para definir condiciones más específicas, cree las reglas de manipulación que indican qué mensajes reenviar a los grupos de servidores RADIUS remotos y para especificar atributos avanzados a NPS de atributo. Utilice al Asistente para nueva directiva de solicitud de conexión para crear directivas de solicitud de conexión comunes o personalizada.

### <a name="key-steps"></a>Pasos clave

Durante el planeamiento de directivas de solicitud de conexión, puede usar los pasos siguientes.

- Eliminar la directiva de solicitud de conexión predeterminada en cada servidor que ejecuta NPS que funciona exclusivamente como un proxy RADIUS.

- Planee las condiciones adicionales y la configuración que es necesarios para cada directiva, combinar esta información con el grupo de servidores RADIUS remotos y las reglas de manipulación de atributos planificadas para la directiva.

- Diseñar el plan para distribuir las directivas de solicitud de conexión comunes a todos los proxy NPS. Crear directivas comunes a varios servidores proxy NPS en un servidor NPS y, a continuación, usar los comandos Netsh para NPS para importar la configuración de directivas y el servidor de la solicitud de conexión en todos los demás servidores proxy.

## <a name="plan-nps-accounting"></a>Plan de contabilidad de NPS

Cuando se configura NPS como proxy RADIUS, puede configurarlo para realizar el registro de cuentas mediante el uso de los archivos de registro de formato NPS, los archivos de registro de formato compatible con la base de datos o NPS SQL Server RADIUS.

También puede reenviar los mensajes de cuentas a un grupo de servidores remotos RADIUS que realiza el registro mediante uno de estos formatos de registro.

### <a name="key-steps"></a>Pasos clave

Durante la planificación para la contabilidad de NPS, puede usar los pasos siguientes.

- Determine si desea que el proxy NPS para realizar servicios de cuentas o para reenviar los mensajes de cuentas a un grupo de servidores RADIUS remotos para las cuentas.

- Plan deshabilitar el proxy NPS local teniendo en cuenta si va a reenviar mensajes de cuentas a otros servidores.

- Planear pasos de configuración de directiva de solicitud de conexión si va a reenviar mensajes de cuentas a otros servidores. Si deshabilita las cuentas locales para el proxy NPS, cada directiva de solicitud de conexión que se configura en ese proxy debe tener el reenvío de mensajes de cuentas habilitadas y configuradas correctamente.

- Determinar el formato de registro que desea usar: Archivos de registro de formato IAS, los archivos de registro de formato compatible con la base de datos o registro NPS SQL Server.

Para configurar el equilibrio de carga para NPS como proxy RADIUS, consulte [equilibrio de carga de NPS Proxy Server](nps-manage-proxy-lb.md).

