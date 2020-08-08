---
title: Use como mínimo la versión 3,0 del protocolo SMB configurado para disponibilidad continua en recursos compartidos de archivos que almacenan archivos para máquinas virtuales.
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: a1fa5cf9-8a48-4f63-bb57-d81e63e77b30
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 7fd84ecf7876638d421f9a8f7042e81c131f2ab2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960278"
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
Lleve a cabo una de las siguientes acciones:

-   Mueva los archivos a un recurso compartido de archivos SMB 3,0 que esté configurado para disponibilidad continua.

-   Vuelva a configurar el recurso compartido de archivos actual para proporcionar disponibilidad continua.



