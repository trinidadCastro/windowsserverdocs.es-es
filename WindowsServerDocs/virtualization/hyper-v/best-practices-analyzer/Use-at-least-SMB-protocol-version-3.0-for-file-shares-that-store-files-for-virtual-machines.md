---
title: Use como mínimo la versión 3,0 del protocolo SMB para los recursos compartidos de archivos que almacenan archivos para máquinas virtuales.
description: Obtenga información acerca de qué hacer cuando los archivos de máquina virtual o los archivos de disco duro virtual se almacenan en un recurso compartido de archivos que no admite como mínimo la versión 3,0 del protocolo SMB.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 4bb832b8-f1aa-4c1f-a0f2-324dd53553ea
ms.date: 8/16/2016
ms.openlocfilehash: 783ba7b2e8b5dc83eb575ee395d85e58701c0b89
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846221"
---
# <a name="use-at-least-smb-protocol-version-30-for-file-shares-that-store-files-for-virtual-machines"></a>Use como mínimo la versión 3,0 del protocolo SMB para los recursos compartidos de archivos que almacenan archivos para máquinas virtuales.

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Error|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>**Problema**
*Los archivos de máquina virtual o los archivos de disco duro virtual se almacenan en un recurso compartido de archivos que no admite como mínimo la versión 3,0 del protocolo SMB.*

## <a name="impact"></a>**Impacto**
*Microsoft no admite esta configuración. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolución**
*Mueva los archivos a un recurso compartido de archivos que use como mínimo la versión 3,0 del protocolo SMB.*



