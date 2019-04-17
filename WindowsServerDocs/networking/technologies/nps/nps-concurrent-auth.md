---
title: Aumentar autenticaciones simultáneas procesados por NPS
description: Este tema proporciona instrucciones sobre cómo configurar autenticaciones simultáneas de servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: aa70c1a26e2c22d26545e1b46a6151d71a2b4095
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="increase-concurrent-authentications-processed-by-nps"></a>Aumentar autenticaciones simultáneas procesados por NPS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener instrucciones sobre cómo configurar autenticaciones simultáneas de servidor de directivas de red.

Si instalaste \(NPS\) el servidor de directivas de red en un equipo que no sea un controlador de dominio y el servidor NPS recibe un gran número de solicitudes de autenticación por segundo, puede mejorar el rendimiento de NPS aumentando el número de autenticaciones simultáneas que se permiten entre el servidor NPS y el controlador de dominio.

Para ello, debes editar la siguiente clave del registro: 

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters`

Agrega un nuevo valor denominado **MaxConcurrentApi** y asignar un valor comprendido entre 2 y 5. 

>[!CAUTION]
>Si se asigna un valor para **MaxConcurrentApi** que es demasiado alto, el servidor NPS puede colocar una carga excesiva en el controlador de dominio.

Para obtener más información acerca de cómo administrar NPS, consulta [administrar el servidor de directivas de red](nps-manage-top.md).

Para obtener más información acerca de NPS, consulta [servidor de directivas de redes (NPS)](nps-top.md).