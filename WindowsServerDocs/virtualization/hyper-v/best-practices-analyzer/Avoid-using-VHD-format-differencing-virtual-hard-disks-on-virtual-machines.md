---
title: Evite el uso de discos duros virtuales de diferenciación con formato VHD en máquinas virtuales que ejecutan cargas de trabajo de servidor en un entorno de producción.
description: Obtenga información acerca de qué hacer cuando una o varias máquinas virtuales usan discos duros virtuales de diferenciación de formato VHD.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 272de33d-2708-4679-8564-ee28848a2839
ms.date: 8/16/2016
ms.openlocfilehash: 4a5d1a2b1c94845b26fd90df4e62932f28c99d0d
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834610"
---
# <a name="avoid-using-vhd-format-differencing-virtual-hard-disks-on-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>Evite el uso de discos duros virtuales de diferenciación con formato VHD en máquinas virtuales que ejecutan cargas de trabajo de servidor en un entorno de producción.

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>**Problema**
*Una o varias máquinas virtuales usan discos duros virtuales de diferenciación con formato VHD.*

## <a name="impact"></a>**Impacto**
*Los discos duros virtuales de diferenciación con formato VHD podrían experimentar problemas de coherencia si se produce un error de alimentación. Pueden producirse problemas de coherencia si el disco físico realiza una actualización incompleta o incorrecta en un sector de un archivo. vhd que se está modificando cuando se produce un error de alimentación. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolución**
*Apague la máquina virtual y convierta la cadena de discos duros virtuales de diferenciación con formato VHD al formato VHDX o mezcle la cadena en un disco duro virtual fijo. (El formato VHDX tiene mecanismos de confiabilidad que ayudan a proteger el disco de daños debidos a errores de alimentación). Sin embargo, no convierta el disco duro virtual si es probable que se adjunte a una versión anterior de Windows en algún momento. Las versiones de Windows anteriores a Windows Server 2012 no admiten el formato VHDX.*



