---
title: Configurar servidores de contenido de Windows Server Update Services (WSUS)
description: En este tema forma parte de BranchCache Deployment Guide para Windows Server 2016, que demuestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN de sucursales.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e8576282be92f02daf716da82ea75eddc755ee5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873846"
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>Configurar servidores de contenido de Windows Server Update Services (WSUS)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de instalar la característica BranchCache e iniciar el servicio BranchCache, los servidores WSUS se deben configurar para almacenar los archivos de actualización en el equipo local. 

Al configurar los servidores WSUS para almacenar los archivos de actualización en el equipo local, tanto los metadatos de actualización como los archivos de actualización son descargados por el servidor WSUS y almacenados directamente en él. Esto garantiza que los equipos cliente de BranchCache reciban los archivos de actualización de productos de Microsoft del servidor WSUS en lugar de hacerlo directamente desde el sitio web de Microsoft Update.  
  
Para obtener más información acerca de la sincronización de WSUS, consulte [configurando las sincronizaciones de actualización](https://technet.microsoft.com/library/mt612311.aspx)  