---
title: Cambio del puerto de escucha en Escritorio remoto
description: Aprende a cambiar el puerto de escucha para el cliente de Escritorio remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 07/19/2018
ms.localizationpriority: medium
ms.openlocfilehash: 818ae5217d0144b2a4ec6e45f3a7757455cfabf1
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80854668"
---
# <a name="change-the-listening-port-for-remote-desktop-on-your-computer"></a>Cambia el puerto de escucha para Escritorio remoto en el equipo

>Se aplica a: Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2008 R2

Cuando te conectas a un equipo (ya sea un cliente de Windows o Windows Server) mediante el cliente de Escritorio remoto, la característica de Escritorio remoto del equipo "escucha" la solicitud de conexión a través de un puerto de escucha definido (el puerto 3389 de forma predeterminada). Puedes cambiar ese puerto de escucha en los equipos de Windows modificando el registro.

1. Inicia el Editor del Registro. (Escribe regedit en el cuadro de búsqueda).
2. Ve hasta la siguiente subclave del Registro: HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\PortNumber
3. Haz clic en **Editar > Modificar**y, a continuación, haz clic en **Decimal**.
4. Escribe el nuevo número de puerto y, a continuación, haz clic en **Aceptar**. 
5. Cierra el Editor del Registro y reinicia el equipo.

La próxima vez que te conectes a este equipo mediante el uso de Conexión a Escritorio remoto, deberás escribir el nuevo puerto. Si usas un firewall, asegúrate de configurarlo para que permita las conexiones al nuevo número de puerto.
