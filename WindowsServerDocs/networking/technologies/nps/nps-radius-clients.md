---
title: Clientes RADIUS
description: En este tema se proporciona información general de los clientes RADIUS para el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d3a09ac9-75f8-4f57-aab4-b0fdfe110118
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1dfc1bb71d2800a8a9587e54147950dfd7fb371f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396001"
---
# <a name="radius-clients"></a>Clientes RADIUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Un servidor de acceso a la red \(NAS @ no__t-1 es un dispositivo que proporciona cierto nivel de acceso a una red de mayor tamaño. Un NAS que usa una infraestructura de RADIUS también es un cliente RADIUS, que envía solicitudes de conexión y mensajes de cuentas a un servidor RADIUS para la autenticación, la autorización y las cuentas.

>[!NOTE]
>Los equipos cliente, como los equipos portátiles y otros equipos que ejecutan sistemas operativos cliente, no son clientes RADIUS. Los clientes RADIUS son servidores de acceso a la red, como puntos de acceso inalámbricos, conmutadores de autenticación 802.1 X, red privada virtual \(VPN @ no__t-1 y servidores de acceso telefónico, porque usan el protocolo RADIUS para comunicarse con servidores RADIUS, como como servidor de directivas de redes \(NPS @ no__t-3 servidores.

Para implementar NPS como un servidor RADIUS o un proxy RADIUS, debe configurar los clientes RADIUS en NPS.

## <a name="radius-client-examples"></a>Ejemplos de clientes RADIUS

Algunos ejemplos de servidores de acceso a la red son:

- Servidores de acceso a la red que proporcionan conectividad de acceso remoto a la red de una organización o a Internet. Un ejemplo es un equipo que ejecuta el sistema operativo Windows Server 2016 y el servicio de acceso remoto, que proporciona servicios de acceso remoto de acceso telefónico o de red privada virtual (VPN) tradicionales a una intranet de la organización.
- Puntos de acceso inalámbrico que proporcionan acceso de nivel físico a la red de una organización mediante tecnologías de recepción y transmisión basadas en redes inalámbricas.
- Conmutadores que proporcionan acceso de nivel físico a la red de una organización mediante tecnologías de LAN tradicionales, como Ethernet.
- Servidores proxy RADIUS que reenvían las solicitudes de conexión a servidores RADIUS que son miembros de un grupo de servidores RADIUS remotos que está configurado en el proxy RADIUS.

## <a name="radius-access-request-messages"></a>Mensajes de solicitud de acceso RADIUS

Los clientes RADIUS crean mensajes de solicitud de acceso RADIUS y los reenvían a un proxy RADIUS o un servidor RADIUS, o bien reenvían los mensajes de solicitud de acceso a un servidor RADIUS que han recibido de otro cliente RADIUS pero que no se han creado.

Los clientes RADIUS no procesan los mensajes de solicitud de acceso mediante la autenticación, autorización y cuentas. Solo los servidores RADIUS realizan estas funciones.

NPS, sin embargo, se puede configurar como un proxy RADIUS y un servidor RADIUS simultáneamente, de modo que procese algunos mensajes de solicitud de acceso y reenvíe otros mensajes.

## <a name="nps-as-a-radius-client"></a>NPS como cliente RADIUS

NPS actúa como un cliente RADIUS cuando se configura como un proxy RADIUS para reenviar mensajes de solicitud de acceso a otros servidores RADIUS para su procesamiento. Cuando se usa NPS como un proxy RADIUS, son necesarios los siguientes pasos de configuración general:

1. Los servidores de acceso a la red, como los puntos de acceso inalámbrico y los servidores VPN, se configuran con la dirección IP del proxy NPS como el servidor RADIUS designado o el servidor de autenticación. Esto permite que los servidores de acceso a la red, que crean mensajes de solicitud de acceso en función de la información que reciben de los clientes de acceso, reenvíen mensajes al proxy NPS.

2. El proxy NPS se configura agregando cada servidor de acceso a la red como un cliente RADIUS. Este paso de configuración permite que el proxy NPS reciba mensajes de los servidores de acceso a la red y se comunique con ellos a través de la autenticación. Además, las directivas de solicitud de conexión en el proxy NPS están configuradas para especificar los mensajes de solicitud de acceso que se van a reenviar a uno o varios servidores RADIUS. Estas directivas también se configuran con un grupo de servidores RADIUS remotos, que indica a NPS dónde debe enviar los mensajes que recibe de los servidores de acceso a la red.

3. El NPS u otros servidores RADIUS que son miembros del grupo de servidores RADIUS remotos en el proxy NPS están configurados para recibir mensajes del proxy NPS. Esto se logra configurando el proxy NPS como un cliente RADIUS.

## <a name="radius-client-properties"></a>Propiedades del cliente RADIUS

Al agregar un cliente RADIUS a la configuración de NPS a través de la consola de NPS o mediante el uso de los comandos Netsh para NPS o comandos de Windows PowerShell, está configurando NPS para recibir mensajes de solicitud de acceso RADIUS de un servidor de acceso a la red o un Proxy RADIUS.

Al configurar un cliente RADIUS en NPS, puede designar las siguientes propiedades:

### <a name="client-name"></a>Nombre de cliente

 Un nombre descriptivo para el cliente RADIUS, que facilita la identificación al usar el complemento NPS o los comandos Netsh para NPS.

### <a name="ip-address"></a>Dirección IP

La dirección del Protocolo de Internet versión 4 \(IPv4 @ no__t-1 o el sistema de nombres de dominio \(DNS @ no__t-3 nombre del cliente RADIUS.

### <a name="client-vendor"></a>Cliente-proveedor

Proveedor del cliente RADIUS. De lo contrario, puede usar el valor estándar de RADIUS para cliente-proveedor.

### <a name="shared-secret"></a>Secreto compartido

Cadena de texto que se usa como contraseña entre clientes RADIUS, servidores RADIUS y proxies RADIUS. Cuando se usa el atributo de autenticador de mensaje, el secreto compartido también se usa como clave para cifrar los mensajes RADIUS. Esta cadena debe estar configurada en el cliente RADIUS y en el complemento NPS.

### <a name="message-authenticator-attribute"></a>Atributo de autenticador de mensaje

Se describe en RFC 2869, "RADIUS Extensions", un hash Message Digest 5 \(MD5 @ no__t-1 de todo el mensaje RADIUS. Si el atributo de autenticador de mensaje RADIUS está presente, se comprueba. Si se produce un error en la comprobación, se descarta el mensaje RADIUS. Si la configuración de cliente requiere el atributo de autenticador de mensaje y no está presente, se descarta el mensaje RADIUS. Se recomienda el uso del atributo de autenticador de mensaje.

>[!NOTE]
>El atributo de autenticador de mensaje es obligatorio y está habilitado de forma predeterminada cuando se usa el protocolo de autenticación extensible \(EAP @ no__t-1. 

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).

