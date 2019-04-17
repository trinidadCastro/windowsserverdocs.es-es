---
title: Clientes RADIUS
description: Este tema proporciona una visión general de los clientes RADIUS de servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d3a09ac9-75f8-4f57-aab4-b0fdfe110118
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a970dabcc6f3f4fc4d8ed5a0dedd01b5a7dee71a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="radius-clients"></a>Clientes RADIUS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Un servidor de acceso de red \(NAS\) es un dispositivo que proporciona cierto nivel de acceso a una red más grande. Un NAS con una infraestructura de RADIUS también es un cliente RADIUS, enviar solicitudes de conexión y mensajes de cuentas a un servidor RADIUS para la autenticación, autorización y cuentas.

>[!NOTE]
>Los equipos cliente, como equipos portátiles y otros equipos que ejecutan sistemas operativos cliente, no son clientes de RADIUS. Clientes de RADIUS son servidores de acceso de red - como puntos de acceso inalámbrico, conmutadores de autenticación 802.1X, servidores de red privada virtual \(VPN\) y servidores de acceso telefónico, ya que usan el protocolo RADIUS para comunicarse con los servidores RADIUS, como el servidor de directivas de red \(NPS\) servidores.

Para implementar NPS como un servidor RADIUS o proxy RADIUS, debes configurar a clientes RADIUS en NPS.

## <a name="radius-client-examples"></a>Ejemplos de clientes RADIUS

Algunos ejemplos de los servidores de acceso de red son:

- Servidores de acceso de red que proporcionan conectividad de acceso remoto a la red de una organización o a Internet. Un ejemplo es un equipo que ejecute el sistema operativo de Windows Server 2016 y el servicio de acceso remoto que proporciona bien telefónico tradicional o servicios de acceso remoto de red privada virtual (VPN) a la intranet de una organización.
- Puntos de acceso inalámbrico que proporcionan acceso de nivel físico a una red de la organización con tecnologías inalámbricas de transmisión y recepción.
- Conmutadores que proporcionan acceso de nivel físico a la red de una organización, con las tecnologías de LAN tradicionales, como Ethernet.
- Proxy RADIUS que reenvían solicitudes de conexión a servidores RADIUS que son miembros de un grupo de servidores RADIUS remoto configurado en el proxy RADIUS.

## <a name="radius-access-request-messages"></a>Mensajes de solicitud de acceso de radio

Los clientes RADIUS crear mensajes de solicitud de acceso de RADIUS y reenvíalo a un servidor de radio o proxy RADIUS o reenvían los mensajes de solicitud de acceso a un servidor de radio que han recibido de otro cliente RADIUS pero no has creado a sí mismos.

Los clientes RADIUS no procesa los mensajes de solicitud de acceso al realizar la autenticación, autorización y teniendo en cuenta. Solo los servidores RADIUS realizan estas funciones.

NPS, sin embargo, puede configurarse como un proxy de radio y un servidor RADIUS al mismo tiempo, para que se procesan algunos mensajes de solicitud de acceso y reenvía otros mensajes.

## <a name="nps-as-a-radius-client"></a>NPS como un cliente RADIUS

NPS actúa como un cliente RADIUS cuando se configura como un proxy de radio para enviar mensajes de solicitud de acceso a otros servidores RADIUS para su procesamiento. Cuando se usa NPS como un proxy RADIUS, los siguientes pasos de configuración general son obligatorios:

1. Los servidores de acceso de red, como puntos de acceso inalámbrico y servidores VPN, están configurados con la dirección IP del proxy NPS como el servidor RADIUS designado o el servidor de autenticación. Esto permite a los servidores de acceso de red, que crear mensajes de solicitud de acceso en función de información que reciben de los clientes de acceso, reenviar mensajes al proxy NPS.

2. El proxy NPS se configura mediante la adición de cada servidor de acceso de red como un cliente RADIUS. Este paso de configuración permite que el proxy NPS para recibir mensajes de los servidores de acceso de red y comunicarse con ellos durante toda la autenticación. Además, las directivas de la solicitud de conexión en el proxy NPS están configuradas para especificar los mensajes de solicitud de acceso para que reenvíen a uno o varios servidores RADIUS. Estas directivas también se configuran con un grupo de servidores RADIUS remoto, que indica dónde enviar los mensajes que recibe de los servidores de acceso de red NPS.

3. El NPS u otros servidores RADIUS que son miembros del grupo de servidores RADIUS remotos en el proxy NPS están configurados para recibir mensajes de proxy NPS. Esto se logra mediante la configuración de proxy NPS como un cliente RADIUS.

## <a name="radius-client-properties"></a>Propiedades del cliente RADIUS

Cuando agregas un cliente RADIUS a la configuración de NPS mediante la consola NPS o mediante el uso de los comandos netsh para NPS o los comandos de Windows PowerShell, se configuran NPS para recibir mensajes de solicitud de acceso de radio de un servidor de acceso de red o un proxy RADIUS.

Cuando se configura un cliente RADIUS en NPS, puedes designar las siguientes propiedades:

### <a name="client-name"></a>Nombre del cliente

 Un nombre descriptivo para el cliente RADIUS, lo cual facilita la identificación cuando se use el NPS complemento o los comandos netsh para NPS.

### <a name="ip-address"></a>Dirección IP

La dirección de protocolo de Internet versión 4 \(IPv4\) o el nombre del sistema de nombres de dominio \(DNS\) del cliente RADIUS.

### <a name="client-vendor"></a>Proveedor del cliente

El proveedor del cliente RADIUS. De lo contrario, puedes usar el valor de radio estándar para el proveedor del cliente.

### <a name="shared-secret"></a>Clave secreta compartida

Una cadena de texto que se usa como una contraseña entre clientes RADIUS, servidores RADIUS y servidores proxy RADIUS. Cuando se usa el atributo autenticador de mensaje, el secreto compartido también se usa como clave para cifrar mensajes RADIUS. Esta cadena debe estar configurada en el cliente RADIUS y en el complemento NPS.

### <a name="message-authenticator-attribute"></a>Atributo autenticador de mensaje

Se describe en la RFC 2869, "RADIUS extensiones", un hash de síntesis del mensaje 5 \(MD5\) de todo el mensaje RADIUS. Si el atributo autenticador de mensaje RADIUS está presente, se comprueba. Si se produce un error de comprobación, se descarta el mensaje RADIUS. Si la configuración de cliente requiere el atributo autenticador de mensaje y no está presente, se descarta el mensaje RADIUS. Se recomienda el uso del atributo autenticador de mensaje.

>[!NOTE]
>Este atributo es necesario y habilitado de manera predeterminada cuando se usa la autenticación del protocolo de autenticación Extensible \(EAP\). 

Para obtener más información acerca de NPS, consulta [servidor de directivas de redes (NPS)](nps-top.md).

