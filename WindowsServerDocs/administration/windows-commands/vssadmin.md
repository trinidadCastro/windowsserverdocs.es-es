---
title: Vssadmin
description: Información general de los comandos vssadmin.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: f3618841eb2f511323873d2ea962838f9ab777d0
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86954697"
---
# <a name="vssadmin"></a>Vssadmin

> Se aplica a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Muestra las copias de seguridad de instantáneas de volumen actuales y todos los escritores y proveedores de instantáneas instalados. Seleccione un nombre de comando en la tabla siguiente ver la sintaxis del comando.

|Comando|Descripción|Disponibilidad
|---|---|---
|[Vssadmin Add shadowstorage](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788051(v%3dws.11))|Agrega una asociación de almacenamiento de instantáneas de volumen.| Solo servidor
|[Vssadmin crear sombra](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788055(v%3dws.11))|Crea una nueva instantánea de volumen.| Solo servidor
|[Vssadmin eliminar sombras](vssadmin-delete-shadows.md)|Elimina las instantáneas de volumen.| Cliente y servidor
|[Vssadmin Delete shadowstorage](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc785461(v%3dws.11))|Elimina las asociaciones de almacenamiento de instantáneas de volumen.| Solo servidor
|[Vssadmin list providers](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788108(v%3dws.11))|Enumera los proveedores de instantáneas de volumen registrados.| Cliente y servidor
|[Vssadmin list shadows](vssadmin-list-shadows.md)|Muestra las instantáneas de volumen existentes.| Cliente y servidor
|[Vssadmin List shadowstorage](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788045(v%3dws.11))|Muestra todas las asociaciones de almacenamiento de instantáneas en el sistema.| Cliente y servidor
|[Vssadmin List Volumes](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788064(v%3dws.11))|Muestra los volúmenes que son válidos para las instantáneas.| Cliente y servidor
|[Vssadmin list writers](vssadmin-list-writers.md)|Enumera todos los escritores de instantáneas de volumen suscritos en el sistema.| Cliente y servidor
|[Vssadmin cambiar el tamaño de shadowstorage](vssadmin-resize-shadowstorage.md)|Cambia el tamaño máximo de una asociación de almacenamiento de instantáneas.| Cliente y servidor
