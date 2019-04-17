---
title: Cambiar el puerto de escucha de escritorio remoto
description: Obtén información sobre cómo cambiar el puerto de escucha para el cliente de escritorio remoto.
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
ms.openlocfilehash: b70479b644f4984c93136d6493483c372703244d
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297325"
---
# Cambiar el puerto de escucha de escritorio remoto en el equipo

>Se aplica a: Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2008 R2

Cuando se conecta a un equipo (un cliente de Windows o Windows Server) a través del cliente de escritorio remoto, la característica de escritorio remoto en el equipo "escucha" la solicitud de conexión a través de un puerto de escucha definido (3389 de manera predeterminada). Puedes cambiar ese puerto de escucha en equipos con Windows modificando el registro.

1. Iniciar el editor del registro. (Escriba regedit en el cuadro de búsqueda).
2. Ve a la siguiente subclave del registro: HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\PortNumber
3. Haz clic en **Editar > modificar**y, a continuación, haga clic en **Decimal**.
4. Escribe el número de puerto nuevo y, a continuación, haz clic en **Aceptar**. 
5. Cierra el editor del registro y reinicie el equipo.

La próxima vez que se conecta a este equipo mediante la conexión a Escritorio remoto, debes escribir el nuevo puerto. Si estás usando un firewall, asegúrate de que configurar el firewall para permitir conexiones al número de puerto nuevo.
