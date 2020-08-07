---
title: Configurar servidores de contenido de Windows Server Update Services (WSUS)
description: Este tema forma parte de la guía de implementación de BranchCache para Windows Server 2016, que muestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso del ancho de banda WAN en las sucursales.
manager: brianlic
ms.topic: get-started-article
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2af2321a1f87eab1e29ecb6c483ee85c87b08ee7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971842"
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>Configurar servidores de contenido de Windows Server Update Services (WSUS)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de instalar la característica BranchCache e iniciar el servicio BranchCache, los servidores WSUS se deben configurar para almacenar los archivos de actualización en el equipo local.

Al configurar los servidores WSUS para almacenar los archivos de actualización en el equipo local, tanto los metadatos de actualización como los archivos de actualización son descargados por el servidor WSUS y almacenados directamente en él. Esto garantiza que los equipos cliente de BranchCache reciban los archivos de actualización de productos de Microsoft del servidor WSUS en lugar de hacerlo directamente desde el sitio web de Microsoft Update.

Para obtener más información acerca de la sincronización de WSUS, consulte [configuración de sincronizaciones de actualizaciones](https://technet.microsoft.com/library/mt612311.aspx) .