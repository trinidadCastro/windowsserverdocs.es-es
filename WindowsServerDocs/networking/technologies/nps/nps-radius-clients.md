---
title: Clientes RADIUS
description: En este tema se proporciona información general de los clientes RADIUS para el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d3a09ac9-75f8-4f57-aab4-b0fdfe110118
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fdca45237d971c1b2e08443112b63d3ce77e4a2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874316"
---
# <a name="radius-clients"></a>Clientes RADIUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Un servidor de acceso de red \(NAS\) es un dispositivo que proporciona cierto nivel de acceso a una red más grande. Un NAS que usa una infraestructura RADIUS también es un cliente RADIUS, enviar las solicitudes de conexión y mensajes de cuentas a un servidor RADIUS para autenticación, autorización y contabilidad.

>[!NOTE]
>Los equipos cliente, como equipos portátiles y otros equipos que ejecutan sistemas operativos cliente, no son clientes RADIUS. Los clientes RADIUS son servidores de acceso de red - como puntos de acceso inalámbrico, conmutadores de autenticación 802.1X, red privada virtual \(VPN\) servidores y servidores de acceso telefónico - porque usan el protocolo RADIUS para comunicarse con RADIUS los servidores como servidor de directivas de red \(NPS\) servidores.

Para implementar NPS como un servidor RADIUS o un proxy RADIUS, debe configurar a clientes RADIUS en NPS.

## <a name="radius-client-examples"></a>Ejemplos de clientes RADIUS

Ejemplos de servidores de acceso de red son:

- Servidores de acceso de red que proporcionan conectividad de acceso remoto a una red de la organización o a Internet. Un ejemplo es un equipo que ejecuta el sistema operativo Windows Server 2016 y el servicio de acceso remoto que proporciona tanto telefónico tradicional o servicios de acceso remoto de red privada virtual (VPN) a la intranet de una organización.
- Puntos de acceso inalámbrico que proporcionan acceso de nivel físico a la red de una organización mediante tecnologías inalámbricas de transmisión y recepción.
- Conmutadores que proporcionan acceso de nivel físico a la red de una organización mediante tecnologías tradicionales de LAN, como Ethernet.
- Servidores proxy RADIUS que reenvían las solicitudes de conexión a servidores RADIUS que son miembros de un grupo de servidores RADIUS remoto configurado en el proxy RADIUS.

## <a name="radius-access-request-messages"></a>Mensajes de solicitud de acceso RADIUS

Clientes RADIUS creación mensajes de solicitud de acceso RADIUS y reenvían a un proxy RADIUS o un servidor RADIUS, o reenvían los mensajes de solicitud de acceso a un servidor RADIUS que han recibido de otro cliente RADIUS pero no se han creado ellos mismos.

Los clientes RADIUS no procesan mensajes de solicitud de acceso al realizar la autenticación, autorización y contabilidad. Sólo los servidores RADIUS realizan dichas funciones.

NPS, sin embargo, puede configurarse como un proxy RADIUS y un servidor RADIUS simultáneamente, para que procese algunos mensajes de solicitud de acceso y reenvíe otros mensajes.

## <a name="nps-as-a-radius-client"></a>NPS como un cliente RADIUS

NPS actúa como un cliente RADIUS cuando se configura como un proxy RADIUS para reenviar los mensajes de solicitud de acceso a otros servidores RADIUS para su procesamiento. Cuando se usa NPS como proxy RADIUS, se requieren los siguientes pasos de configuración general:

1. Servidores de acceso de red, como puntos de acceso inalámbrico y los servidores VPN, se configuran con la dirección IP del proxy NPS como servidor RADIUS designado o el servidor de autenticación. Esto permite que los servidores de acceso de red, que crean mensajes de solicitud de acceso según la información que reciben de los clientes de acceso, reenviar mensajes al proxy NPS.

2. El proxy NPS se configura mediante la adición de cada servidor de acceso de red como un cliente RADIUS. Este paso de configuración permite que el proxy NPS recibir mensajes desde los servidores de acceso de red y comunicarse con ellos durante toda la autenticación. Además, las directivas de solicitud de conexión en el proxy NPS están configuradas para especificar qué mensajes de solicitud de acceso para reenviar a uno o más servidores RADIUS. Estas directivas también están configuradas con un grupo de servidores RADIUS remoto, que indica a NPS dónde debe enviar los mensajes que recibe de los servidores de acceso de red.

3. El NPS u otros servidores RADIUS que son miembros del grupo de servidores RADIUS remotos en el proxy NPS están configurados para recibir mensajes desde el proxy NPS. Esto se consigue configurando el proxy NPS como un cliente RADIUS.

## <a name="radius-client-properties"></a>Propiedades del cliente RADIUS

Cuando se agrega un cliente RADIUS para la configuración de NPS mediante la consola de NPS o mediante el uso de los comandos netsh para NPS o comandos de Windows PowerShell, está configurando el NPS para recibir mensajes de solicitud de acceso RADIUS de un servidor de acceso de red o un Proxy RADIUS.

Cuando se configura un cliente RADIUS en NPS, puede designar las siguientes propiedades:

### <a name="client-name"></a>Nombre del cliente

 Un nombre descriptivo para el cliente RADIUS, que resulta más fácil identificar cuando se usa el NPS complemento o los comandos netsh para NPS.

### <a name="ip-address"></a>Dirección IP

El protocolo de Internet versión 4 \(IPv4\) dirección o el sistema de nombres de dominio \(DNS\) nombre del cliente RADIUS.

### <a name="client-vendor"></a>Proveedor del cliente

El proveedor del cliente RADIUS. En caso contrario, puede usar el valor estándar de RADIUS para el proveedor del cliente.

### <a name="shared-secret"></a>Secreto compartido

Una cadena de texto que sirve como contraseña entre clientes RADIUS, servidores RADIUS y proxy RADIUS. Cuando se usa el atributo de autenticador de mensaje, el secreto compartido también se utiliza como clave para cifrar mensajes RADIUS. Esta cadena debe estar configurada en el cliente RADIUS y en el complemento NPS.

### <a name="message-authenticator-attribute"></a>Atributo de autenticador de mensaje

Se describe en RFC 2869, "Extensiones RADIUS", un Message Digest 5 \(MD5\) hash de todo el mensaje RADIUS. Si el atributo de autenticador de mensaje RADIUS está presente, se comprueba. Si se produce un error de comprobación, se descarta el mensaje RADIUS. Si la configuración de cliente requiere el atributo de autenticador de mensaje y no está presente, se descarta el mensaje RADIUS. Se recomienda usar el atributo de autenticador de mensaje.

>[!NOTE]
>El atributo de autenticador de mensaje es necesaria y habilitado de forma predeterminada cuando se usa el protocolo de autenticación Extensible \(EAP\) autenticación. 

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).

