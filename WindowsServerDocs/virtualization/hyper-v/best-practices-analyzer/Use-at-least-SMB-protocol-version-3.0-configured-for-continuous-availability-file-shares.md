---
title: Use como mínimo la versión 3,0 del protocolo SMB configurado para disponibilidad continua en recursos compartidos de archivos que almacenan archivos para máquinas virtuales.
description: Obtenga información acerca de qué hacer cuando los archivos de máquina virtual o los archivos de disco duro virtual se almacenan en un recurso compartido de archivos de red que no está configurado con la característica de disponibilidad continua de la versión 3,0 del protocolo SMB.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: a1fa5cf9-8a48-4f63-bb57-d81e63e77b30
ms.date: 8/16/2016
ms.openlocfilehash: a71976a3ebe8eb152993be6e4760679664a6eeed
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846283"
---
# <a name="use-at-least-smb-protocol-version-30-configured-for-continuous-availability-on-file-shares-that-store-files-for-virtual-machines"></a>Use como mínimo la versión 3,0 del protocolo SMB configurado para disponibilidad continua en recursos compartidos de archivos que almacenan archivos para máquinas virtuales.

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
*Los archivos de máquina virtual o los archivos de disco duro virtual se almacenan en un recurso compartido de archivos de red que no está configurado con la característica de disponibilidad continua de la versión 3,0 del protocolo SMB.*

## <a name="impact"></a>**Impacto**
*Microsoft no recomienda esta configuración porque podría afectar a la disponibilidad de las máquinas virtuales con el servidor. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolución**
Realice una de las siguientes acciones:

-   Mueva los archivos a un recurso compartido de archivos SMB 3,0 que esté configurado para disponibilidad continua.

-   Vuelva a configurar el recurso compartido de archivos actual para proporcionar disponibilidad continua.



