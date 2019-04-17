---
title: Escritorio remoto - permitir el acceso a su PC desde fuera de la red
description: Obtenga información sobre las opciones de acceso remoto a su PC desde fuera de la red de PC
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: haley-rowland
manager: dongill
ms.author: elizapo
ms.date: 04/04/2018
ms.localizationpriority: medium
ms.openlocfilehash: d77c362d9d06b70ad0747002ed8853a39e05b7ff
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "1708663"
---
# <a name="remote-desktop---allow-access-to-your-pc-from-outside-your-pcs-network"></a>Escritorio remoto - permitir el acceso a su PC desde fuera de la red de su PC

>Se aplica a: Windows 10, Windows Server 2016

Cuando se conecta a su PC mediante el uso de un cliente de escritorio remoto, está creando una conexión de punto a punto. Esto significa que necesita acceso directo a su PC (a veces denominado "host"). Si necesita para conectarse a su PC desde fuera de la red que se ejecuta en su PC, debe habilitar el acceso de ese. Tiene dos opciones: utilizar el reenvío de puerto o configurar una red privada virtual.

## <a name="enable-port-forwarding-on-your-router"></a>Habilitar el reenvío de puerto en el enrutador

Reenvío de puerto simplemente asigna el puerto en la dirección IP de su enrutador (su IP pública) para el puerto y dirección IP del equipo al que desea tener acceso. 

Los pasos específicos para habilitar el reenvío de puerto dependen el enrutador que está utilizando, por lo que necesitará para buscar en línea para obtener instrucciones de su enrutador. Para obtener una descripción general de los pasos, desproteger [wikiHow para establecer seguridad de enrutamiento de puerto en un enrutador](https://www.wikihow.com/Set-Up-Port-Forwarding-on-a-Router).

Antes de asignar el puerto necesita lo siguiente:

- Dirección IP interna de PC: buscar en **Configuración > red e Internet > estado > Ver propiedades de red**. Busque la configuración de red con un estado "Operativa" y, a continuación, obtener la **dirección IPv4**.

   ![Configuración de red operativa](../media/rdclient-operational-network.png)

- La dirección IP pública (del enrutador IP). Hay muchas formas de encontrar esta: puede buscar en "Mi IP" (de Bing o Google) o ver las [Propiedades de red Wi-Fi](https://binged.it/2Gwob34) (para Windows 10).
- Número de puerto que se asigna. En la mayoría de los casos es 3389 - que es el puerto predeterminado utilizado por las conexiones de escritorio remoto.
- Acceso de administrador a su enrutador.  

   >[!WARNING]
   > Se va a abrir su PC hasta internet: asegúrese de que establecer una contraseña segura para su PC.

Después de asignar el puerto, podrá conectarse a su PC host desde fuera de la red local conectándose a la dirección IP pública de su enrutador (la segunda viñeta anterior).

Puede cambiar la dirección del enrutador IP: proveedor de servicios internet (ISP) puede asignar una nueva dirección IP en cualquier momento. Para evitar este problema, considere el uso de DNS dinámico: Esto permite conectar al PC mediante una fácil de recordar el nombre de dominio, en lugar de la dirección IP. El enrutador actualiza automáticamente el servicio DDNS con la nueva dirección IP, debe cambiar.

Con la mayoría de los enrutadores puede definir qué dirección IP de origen o de la red de origen puede utilizar la asignación de puerto. Por lo que, si sabe que va a conectar desde el trabajo, puede agregar la dirección IP de su red de trabajo, que le permite evitar que se abra el puerto a internet pública todo. Si el host que está utilizando para conectarse utiliza la dirección IP dinámica, establezca la restricción de origen para permitir el acceso de toda la gama de ese ISP determinado.

También puede resultar conveniente configurar una [dirección IP estática](/windows-hardware/customize/mobile/mcsf/enable-static-ip) en su PC para que no cambie la dirección IP interna. Si lo hace que, a continuación, el enrutador reenvío de puerto siempre señalará a la dirección IP correcta.


## <a name="use-a-vpn"></a>Usar una red privada virtual

Si se conecta a la red de área local mediante el uso de una red privada virtual (VPN), no tiene que abrir su PC a internet pública. En su lugar, cuando se conecta a la VPN, el cliente de escritorio remoto actúa como y forma parte de la misma red y tener acceso a su PC. Hay un número de servicios VPN: puede buscar y utilizar el que mejor adapte a.