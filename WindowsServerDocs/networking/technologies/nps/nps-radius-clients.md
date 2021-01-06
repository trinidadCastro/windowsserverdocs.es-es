---
title: Clientes RADIUS
description: En este tema se proporciona información general de los clientes RADIUS para el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: d3a09ac9-75f8-4f57-aab4-b0fdfe110118
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 2f664ad42781846ea44cb6b382b0078cbf403a4f
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947461"
---
# <a name="radius-clients"></a>Clientes RADIUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Un servidor de acceso a la red \( NAS \) es un dispositivo que proporciona cierto nivel de acceso a una red de mayor tamaño. Un NAS que usa una infraestructura RADIUS es también un cliente RADIUS, que envía solicitudes de conexión y mensajes de cuentas a un servidor RADIUS para su autenticación, autorización y administración de cuentas.

>[!NOTE]
>Los equipos cliente, como los equipos portátiles y otros equipos que ejecutan sistemas operativos cliente, no son clientes RADIUS. Los clientes RADIUS son servidores de acceso a la red, como puntos de acceso inalámbricos, conmutadores de autenticación de 802.1 X, servidores VPN de red privada virtual \( \) y servidores de acceso telefónico, porque usan el protocolo RADIUS para comunicarse con servidores RADIUS como servidores NPS de servidor de directivas de redes \( \) .

Para implementar NPS como un servidor RADIUS o un proxy RADIUS, debe configurar los clientes RADIUS en NPS.

## <a name="radius-client-examples"></a>Ejemplos de clientes RADIUS

Algunos ejemplos de servidores de acceso a la red son:

- Servidores de acceso a la red que proporcionan conectividad de acceso remoto a la red de una organización o a Internet. Un ejemplo es un equipo que ejecuta el sistema operativo Windows Server 2016 y el servicio de acceso remoto, que proporciona servicios de acceso remoto de acceso telefónico o de red privada virtual (VPN) tradicionales a una intranet de la organización.
- Puntos de acceso inalámbrico que proporcionan acceso de nivel físico a la red de una organización mediante tecnologías inalámbricas de recepción y transmisión.
- Conmutadores que proporcionan acceso de nivel físico a la red de una organización mediante tecnologías tradicionales de LAN, como Ethernet.
- Servidores proxy RADIUS que reenvían las solicitudes de conexión a servidores RADIUS que son miembros de un grupo de servidores RADIUS remotos configurado en el proxy RADIUS.

## <a name="radius-access-request-messages"></a>Mensajes de solicitud de acceso de RADIUS

Los clientes RADIUS pueden crear mensajes de solicitud de acceso de RADIUS y reenviarlos a un proxy RADIUS o un servidor RADIUS, o bien, pueden reenviar mensajes de solicitud de acceso a un servidor RADIUS que han recibido de otro cliente RADIUS pero que no han creado ellos mismos.

Los clientes RADIUS no procesan mensajes de solicitud de acceso mediante la autenticación, autorización y administración de cuentas. Sólo los servidores RADIUS realizan dichas funciones.

No obstante, se puede configurar el NPS como un proxy RADIUS y un servidor RADIUS simultáneamente, de forma que procese algunos mensajes de solicitud de acceso y reenvíe otros mensajes.

## <a name="nps-as-a-radius-client"></a>NPS como cliente RADIUS

NPS actúa como un cliente RADIUS cuando se configura como un proxy RADIUS para reenviar mensajes de solicitud de acceso a otros servidores RADIUS para su procesamiento. Cuando se usa un NPS como un proxy RADIUS, es necesario realizar los siguientes pasos de configuración general:

1. Los servidores de acceso a la red, como los puntos de acceso inalámbrico y los servidores VPN, están configurados con la dirección IP del proxy NPS como el servidor RADIUS designado o el servidor de autenticación. Esto permite a los servidores de acceso a la red, que crean mensajes de solicitud de acceso basados en la información que reciben de los clientes de acceso, reenviar mensajes al proxy NPS.

2. Para configurar el proxy NPS, se deben agregar todos los servidores de acceso a la red como clientes RADIUS. Este paso de configuración permite al proxy NPS recibir mensajes de los servidores de acceso a la red y comunicarse con ellos durante toda la autenticación. Además, las directivas de solicitud de conexión establecidas en el proxy NPS están configuradas para especificar qué mensajes de solicitud de acceso se deben reenviar a uno o varios servidores RADIUS. Estas directivas también están configuradas con un grupo de servidores RADIUS remotos, que indica a NPS dónde debe enviar los mensajes que recibe de los servidores de acceso a la red.

3. El servidor NPS u otros servidores RADIUS que son miembros del grupo de servidores RADIUS remotos en el proxy NPS están configurados para recibir mensajes del proxy NPS. Esto se consigue configurando el proxy NPS como un cliente RADIUS.

## <a name="radius-client-properties"></a>Propiedades del cliente RADIUS

Al agregar un cliente RADIUS a la configuración de NPS a través de la consola de NPS o mediante el uso de los comandos Netsh para NPS o comandos de Windows PowerShell, está configurando NPS para recibir mensajes de Access-Request RADIUS de un servidor de acceso a la red o un proxy RADIUS.

Cuando se configura un cliente RADIUS en NPS, se pueden designar las siguientes propiedades:

### <a name="client-name"></a>Nombre del cliente

 Nombre descriptivo para el cliente RADIUS, que facilite su identificación cuando se use el complemento NPS o los comandos netsh para NPS.

### <a name="ip-address"></a>Dirección IP

La dirección IPv4 del Protocolo de Internet versión 4 \( \) o el \( nombre DNS del sistema de nombres de dominio \) del cliente RADIUS.

### <a name="client-vendor"></a>Client-Vendor

Proveedor del cliente RADIUS. De lo contrario, se puede usar el valor estándar de RADIUS para el proveedor del cliente.

### <a name="shared-secret"></a>Secreto compartido

Cadena de texto que se usa como contraseña entre clientes RADIUS, servidores RADIUS y servidores proxy RADIUS. Cuando se usa el atributo autenticador de mensaje, el secreto compartido también se usa como clave para cifrar mensajes RADIUS. Esta cadena debe estar configurada en el cliente RADIUS y en el complemento NPS.

### <a name="message-authenticator-attribute"></a>Atributo de autenticador de mensaje

Se describe en RFC 2869, "extensiones de RADIUS", un \( hash MD5 \) de síntesis del mensaje 5 del mensaje RADIUS completo. Si está presente el atributo de autenticador de mensaje de RADIUS, se comprueba. Si la comprobación genera un error, se descartará el mensaje RADIUS. Si la configuración del cliente requiere la presencia del atributo de autenticador de mensaje y éste no está presente, se descartará el mensaje RADIUS. Se recomienda usar el atributo de autenticador de mensaje.

>[!NOTE]
>El atributo de autenticador de mensaje es obligatorio y está habilitado de forma predeterminada cuando se usa la autenticación EAP del Protocolo de autenticación extensible \( \) .

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).

