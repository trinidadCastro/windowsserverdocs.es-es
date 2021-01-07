---
title: Configurar servidores de contenido de Windows Server Update Services (WSUS)
description: Obtenga información acerca de cómo configurar los servidores de contenido de Windows Server Update Services (WSUS) para almacenar los archivos de actualización en el equipo local.
manager: brianlic
ms.topic: how-to
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: 9e55b470df3d1fc3eed8a69d89b228d3d7b7ed70
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950411"
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>Configurar servidores de contenido de Windows Server Update Services (WSUS)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de instalar la característica BranchCache e iniciar el servicio BranchCache, los servidores WSUS se deben configurar para almacenar los archivos de actualización en el equipo local.

Al configurar los servidores WSUS para almacenar los archivos de actualización en el equipo local, tanto los metadatos de actualización como los archivos de actualización son descargados por el servidor WSUS y almacenados directamente en él. Esto garantiza que los equipos cliente de BranchCache reciban los archivos de actualización de productos de Microsoft del servidor WSUS en lugar de hacerlo directamente desde el sitio web de Microsoft Update.

Para obtener más información acerca de la sincronización de WSUS, consulte [configuración de sincronizaciones de actualizaciones](../../../administration/windows-server-update-services/manage/setting-up-update-synchronizations.md) .