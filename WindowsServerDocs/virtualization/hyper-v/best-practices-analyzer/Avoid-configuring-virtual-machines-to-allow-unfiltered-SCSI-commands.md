---
title: Evite configurar máquinas virtuales para permitir comandos SCSI sin filtrar
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: dd4a3d78-a77f-451e-a383-d5cf45ea17cf
ms.date: 8/16/2016
ms.openlocfilehash: 95d7fa223371aa3dbc3b66efcfefed217eed5216
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90747090"
---
# <a name="avoid-configuring-virtual-machines-to-allow-unfiltered-scsi-commands"></a>Evite configurar máquinas virtuales para permitir comandos SCSI sin filtrar

>Se aplica a: Windows Server 2016



*Para obtener más información sobre los análisis y los procedimientos recomendados, consulte* [analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Operations|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema

*Una máquina virtual está configurada para permitir comandos SCSI sin filtrar.*

## <a name="impact"></a>Impacto

*Omitir el filtrado de comandos SCSI supone un riesgo para la seguridad. Esta configuración solo debe habilitarse si es necesaria para la compatibilidad con las aplicaciones de almacenamiento que se ejecutan en el sistema operativo invitado. Las siguientes máquinas virtuales están configuradas para permitir comandos SCSI sin filtrar:*

\<list of virtual machine names>

## <a name="resolution"></a>Solución

*Póngase en contacto con el proveedor de almacenamiento para determinar si se requiere esta configuración. Además, si el sistema operativo de administración u otros sistemas operativos invitados están en peligro o presentan un comportamiento inusual, vuelva a configurar la máquina virtual para que bloquee los comandos.*



