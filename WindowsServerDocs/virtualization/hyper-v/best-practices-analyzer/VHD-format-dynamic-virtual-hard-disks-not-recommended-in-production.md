---
title: No se recomienda usar discos duros virtuales dinámicos con formato VHD para las máquinas virtuales que ejecutan cargas de trabajo de servidor en un entorno de producción.
description: Obtenga información acerca de qué hacer cuando una o varias máquinas virtuales usan discos duros virtuales de expansión dinámica en formato VHD.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 324a60a0-1d15-4ef2-9f17-23cbd2eb42ce
ms.date: 8/16/2016
ms.openlocfilehash: 53e32d215311712130a48009359706ad1aea4a14
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845759"
---
# <a name="vhd-format-dynamic-virtual-hard-disks-are-not-recommended-for-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>No se recomienda usar discos duros virtuales dinámicos con formato VHD para las máquinas virtuales que ejecutan cargas de trabajo de servidor en un entorno de producción.

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
*Una o varias máquinas virtuales usan discos duros virtuales de expansión dinámica de formato VHD.*

## <a name="impact"></a>**Impacto**
*Los discos duros virtuales dinámicos con formato VHD podrían experimentar problemas de coherencia si se produce un error de alimentación. Pueden producirse problemas de coherencia si el disco físico realiza una actualización incompleta o incorrecta en un sector de un archivo. vhd que se está modificando cuando se produce un error de alimentación. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolución**
*Apague la máquina virtual y convierta el disco duro virtual dinámico con formato VHD en un disco duro virtual con formato VHDX o en un disco duro virtual fijo. (El formato VHDX tiene mecanismos de confiabilidad que ayudan a proteger el disco de daños causados por errores de alimentación del sistema). Sin embargo, no convierta el disco duro virtual si es probable que se adjunte a una versión anterior de Windows en algún momento. Las versiones de Windows anteriores a Windows Server 2012 no admiten el formato VHDX.*



