---
title: Configuración de sistemas operativos invitados para las copias de seguridad basadas en VSS para habilitar instantáneas coherentes con la aplicación para la réplica de Hyper-V
description: Obtenga información sobre qué hacer cuando las instantáneas coherentes con la aplicación requieren que los servicios de instantáneas de volumen (VSS) estén habilitados y configurados en los sistemas operativos invitados de las máquinas virtuales que participan en la replicación.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 7638e996-d42d-47b8-a670-1e09e7183850
ms.date: 8/16/2016
ms.openlocfilehash: ca6b2656554f0f22a956159c770f24ad824462c6
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846086"
---
# <a name="configure-guest-operating-systems-for-vss-based-backups-to-enable-application-consistent-snapshots-for-hyper-v-replica"></a>Configuración de sistemas operativos invitados para las copias de seguridad basadas en VSS para habilitar instantáneas coherentes con la aplicación para la réplica de Hyper-V

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Error|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema
*Las instantáneas coherentes con la aplicación requieren que los servicios de instantáneas de volumen (VSS) estén habilitados y configurados en los sistemas operativos invitados de las máquinas virtuales que participan en la replicación.*

## <a name="impact"></a>Impacto
*Incluso si las instantáneas coherentes con la aplicación se especifican en la configuración de replicación, Hyper-V no las usará a menos que se configure VSS. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Solución
*Use conexión a máquina virtual para instalar los servicios de integración en la máquina virtual.*



