---
title: Cambiar el puerto de escucha en el escritorio remoto
description: Obtenga información sobre cómo cambiar el puerto de escucha para el cliente de escritorio remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 07/19/2018
ms.localizationpriority: medium
ms.openlocfilehash: 5bf90010143e742f7a0c9b5c262be01e6ccf8c5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882726"
---
# <a name="change-the-listening-port-for-remote-desktop-on-your-computer"></a>Cambiar el puerto de escucha para escritorio remoto en el equipo

>Se aplica a: Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2, Windows Server 2008 R2

Cuando se conecta a un equipo (un cliente de Windows o Windows Server) a través del cliente de escritorio remoto, la característica de escritorio remoto en el equipo "escucha" la solicitud de conexión a través de un puerto de escucha definido (3389 de forma predeterminada). Puede cambiar ese puerto de escucha en los equipos de Windows modificando el registro.

1. Inicie el editor del registro. (En el cuadro de búsqueda, escriba regedit).
2. Navegue hasta la siguiente subclave del registro: HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\PortNumber
3. Haga clic en **Editar > modificar**y, a continuación, haga clic en **Decimal**.
4. Escriba el nuevo número de puerto y, a continuación, haga clic en **Aceptar**. 
5. Cierre el editor del registro y reinicie el equipo.

La próxima vez que se conecten a este equipo mediante el uso de la conexión a Escritorio remoto, debe escribir el nuevo puerto. Si usa un firewall, asegúrese de configurar el firewall para permitir las conexiones para el nuevo número de puerto.
