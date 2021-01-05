---
title: Evite configurar máquinas virtuales para permitir comandos SCSI sin filtrar
description: Obtenga información acerca de qué hacer cuando una máquina virtual está configurada para permitir comandos SCSI sin filtrar.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: dd4a3d78-a77f-451e-a383-d5cf45ea17cf
ms.date: 8/16/2016
ms.openlocfilehash: 70282ef6367c711e04bf5d29ee10303841019b2c
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834700"
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



