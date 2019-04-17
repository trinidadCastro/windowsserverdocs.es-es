---
title: Configurar los servidores de contenido de Windows Server Update Services (WSUS)
description: Este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8200a0905f7bc5c403288a22faece5f84eac8af9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>Configurar los servidores de contenido de Windows Server Update Services (WSUS)

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Después de instalar la característica BranchCache e iniciar el servicio de BranchCache, servidores WSUS deben configurarse para almacenar archivos de actualización en el equipo local. 

Configurar servidores WSUS para almacenar archivos de actualización en el equipo local, los metadatos de actualización y los archivos de actualización se descarga y almacena directamente en el servidor WSUS. Esto garantiza que los equipos cliente BranchCache reciban los archivos de actualización de producto de Microsoft desde el servidor WSUS, en lugar de directamente desde el Website Update Microsoft.  
  
Para obtener más información sobre la sincronización de WSUS, consulta [configurar sincronizaciones de actualización](https://technet.microsoft.com/en-us/library/mt612311.aspx)  