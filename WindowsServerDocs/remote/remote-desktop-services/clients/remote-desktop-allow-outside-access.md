---
title: 'Escritorio remoto: permitir el acceso a su equipo desde fuera de la red'
description: Obtenga información acerca de las opciones de acceso remoto a su equipo desde fuera de la red del equipo
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888156"
---
# <a name="remote-desktop---allow-access-to-your-pc-from-outside-your-pcs-network"></a>Escritorio remoto: permitir el acceso a su equipo desde fuera de la red de su PC

>Se aplica a: Windows 10, Windows Server 2016

Cuando se conecta a su equipo mediante un cliente de escritorio remoto, se crea una conexión punto a punto. Esto significa que necesita acceso directo al equipo (a veces se denomina "host"). Si necesita conectarse a su equipo desde fuera de la red en en que el equipo se ejecuta, deberá habilitar el acceso. Tiene dos opciones: utilizar el reenvío de puerto o configurar una VPN.

## <a name="enable-port-forwarding-on-your-router"></a>Habilitar el reenvío de puerto en el enrutador

Reenvío de puerto simplemente asigna el puerto en la dirección IP de su enrutador (la dirección IP pública) para el puerto y dirección IP del equipo que desea tener acceso. 

Los pasos específicos para habilitar el reenvío de puerto dependen el enrutador que usa, por lo que necesitará buscar en línea para obtener instrucciones de su enrutador. Para obtener una explicación general de los pasos, eche un vistazo [wikiHow para establecer seguridad de enrutamiento de puerto en un enrutador](https://www.wikihow.com/Set-Up-Port-Forwarding-on-a-Router).

Antes de asignar el puerto necesita lo siguiente:

- Dirección IP interna de PC: Buscar en **configuración > red e Internet > estado > ver las propiedades de red**. Obtenga la configuración de red cuyo estado sea "Operational" y, a continuación, el **dirección IPv4**.

   ![Configuración de red operativa](../media/rdclient-operational-network.png)

- La dirección IP pública (IP del enrutador). Hay muchas maneras para encontrarlo, puede buscar "Mi IP" (en Bing o Google) o ver el [propiedades de la red Wi-Fi](https://binged.it/2Gwob34) (para Windows 10).
- Número de puerto que se está asignando. En la mayoría de los casos es 3389 - que es el puerto predeterminado usado por las conexiones de escritorio remoto.
- Acceso de administrador a su enrutador.  

   >[!WARNING]
   > Se abre su equipo hasta internet; Asegúrese de que establecer una contraseña segura para su PC.

Después de asignar el puerto, podrá conectarse a su equipo host desde fuera de la red local mediante la conexión a la dirección IP pública del enrutador (la segunda viñeta anterior).

Puede cambiar la dirección IP del enrutador: proveedor de servicios internet (ISP) puede asignar una dirección IP nueva en cualquier momento. Para evitar este problema, considere el uso de DNS dinámico: Esto le permite conectarse al equipo mediante una fácil de recordar el nombre de dominio, en lugar de la dirección IP. El enrutador actualiza automáticamente el servicio DDNS con la nueva dirección IP, debe cambiar.

Con la mayoría de los enrutadores puede definir qué dirección IP de origen o la red de origen puede usar la asignación de puertos. Por lo tanto, si sabe que va a conectar desde el trabajo, puede agregar la dirección IP para la red del trabajo - que le permite evitar la apertura del puerto en todo internet pública. Si el host que usa para conectarse usa la dirección IP dinámica, Establece la restricción de origen para permitir el acceso desde el intervalo completo de ese ISP determinado.

También podría considerar la configuración de un [dirección IP estática](/windows-hardware/customize/mobile/mcsf/enable-static-ip) en su PC, por lo que no cambia la dirección IP interna. Si lo hace que, a continuación, el enrutador reenvío de puerto siempre señalará a la dirección IP correcta.


## <a name="use-a-vpn"></a>Usar una VPN

Si se conecta a la red de área local mediante el uso de una red privada virtual (VPN), no tendrá que abrir su PC a la internet pública. En su lugar, cuando se conecta a la VPN, el cliente de escritorio remoto actúa como forma parte de la misma red y tener acceso a su PC. Hay una serie de servicios VPN, puede encontrar y usar lo que mejor le convenga.