---
title: Los adaptadores de pantalla deben estar habilitados en las máquinas virtuales para proporcionar funcionalidades de vídeo
description: Obtenga información acerca de qué hacer cuando el dispositivo de vídeo de bus de máquina virtual de Microsoft puede estar deshabilitado en una máquina virtual.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: ac5992e6-3c0b-46c2-a48e-6ef37b679228
ms.date: 8/16/2016
ms.openlocfilehash: 415d8c59b789ebdd77bfd16840926c55007749b9
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846382"
---
# <a name="display-adapters-should-be-enabled-in-virtual-machines-to-provide-video-capabilities"></a>Los adaptadores de pantalla deben estar habilitados en las máquinas virtuales para proporcionar funcionalidades de vídeo

>Se aplica a: Windows Server 2016



*Para obtener más información sobre los análisis y los procedimientos recomendados, consulte* [analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema

*El dispositivo de vídeo de bus de máquina virtual de Microsoft puede estar deshabilitado en una máquina virtual.*

El dispositivo de vídeo de bus de máquina virtual de Microsoft es un adaptador de vídeo virtual optimizado para su uso con máquinas virtuales de Hyper-V. Cuando una máquina virtual no está configurada para usar el dispositivo de vídeo de bus de máquina virtual de Microsoft, se usa un adaptador de vídeo heredado. El dispositivo de vídeo de bus de máquina virtual de Microsoft funciona mejor que un adaptador de vídeo heredado.

## <a name="impact"></a>Impacto

*Se degradará el rendimiento de vídeo para las siguientes máquinas virtuales:*

\<list of virtual machine names>

## <a name="resolution"></a>Solución

*Use Device Manager del sistema operativo invitado para habilitar el dispositivo de vídeo de bus de máquina virtual de Microsoft.*

Los pasos necesarios para usar Device Manager varían en función del sistema operativo. Para obtener instrucciones, consulte la ayuda del sistema operativo invitado.



