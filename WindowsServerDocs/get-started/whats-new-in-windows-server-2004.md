---
title: Novedades de Windows Server, versión 2004
description: Nuevas características de Windows Server, versión 2004
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: Heidilohr
ms.author: helohr
ms.date: 05/27/2020
ms.localizationpriority: high
ms.openlocfilehash: e0136dad7180e41f15ae6226008aa7580ec53283
ms.sourcegitcommit: c63672805c93d5bf2a9eb71b3e2de2df00194529
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2020
ms.locfileid: "84124903"
---
# <a name="whats-new-in-windows-server-version-2004"></a>Novedades de Windows Server, versión 2004

>Se aplica a: Windows Server (Canal semianual)

Para obtener información sobre las características más recientes de Windows, consulta [Novedades de Windows Server](whats-new-in-windows-server.md). En este tema se describen algunas de las nuevas características de Windows Server, versión 2004.

## <a name="server-core-container-improvements"></a>Mejoras de contenedor de Server Core

Hemos reducido el tamaño total de las imágenes de contenedor de Server Core para mejorar el rendimiento y las velocidades de descarga. Hemos incluido las siguientes mejoras:

- Se quitaron la mayoría de las imágenes NGEN de la imagen de contenedor de Server Core para reducir el tamaño de las imágenes.
- Las imágenes del entorno de ejecución .NET Framework basadas en imágenes de contenedor de Server Core ahora están optimizadas para aplicaciones de ASP.NET y rendimiento de scripts de Windows PowerShell.
- El equipo de .NET también se ha asegurado que solo haya una copia de cada imagen NGEN, lo que genera un tamaño más pequeño de las imágenes de .NET Framework.

Para darte una idea más clara del tamaño de estos contenedores, en la tabla siguiente se compara la versión actual del contenedor a la fecha de la [actualización de seguridad mensual de mayo de 2020](https://support.microsoft.com/help/4561769/windows-server-containers-for-may-2020) (también conocida como actualización "5B") con las versiones anteriores.

| Versión de contenedor | Tamaño de descarga | Tamaño en disco |
|---|---|---|
| Windows Server, versión 1903 | 2,311 GB | 5,1 GB |
| Windows Server, versión 1909 | 2,257 GB | 4,97 GB |
| Windows Server, versión 2004 | 1,830 GB | 3,98 GB |

Para más información sobre la actualización de Windows Server, versión 2004, consulta [nuestra entrada de blog](https://techcommunity.microsoft.com/t5/containers/windows-server-version-2004-now-available/ba-p/1419194). Para más información sobre las actualizaciones de contenedores de Windows en general, consulta [Actualización de contenedores de Windows Server](/virtualization/windowscontainers/deploy-containers/update-containers/).
