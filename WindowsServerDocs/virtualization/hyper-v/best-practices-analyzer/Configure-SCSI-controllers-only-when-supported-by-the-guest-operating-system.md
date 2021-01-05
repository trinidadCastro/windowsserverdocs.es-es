---
title: Configurar controladores SCSI solo cuando sea compatible con el sistema operativo invitado
description: Obtenga información acerca de qué hacer cuando una máquina virtual está configurada con una controladora SCSI que no se puede usar porque el sistema operativo invitado no es compatible con controladores SCSI.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 861f194f-467e-4b07-a1c5-55b35f6327c4
ms.date: 8/16/2016
ms.openlocfilehash: 3661eb1010dd163d33cb518a16174ed4133b0d98
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846088"
---
# <a name="configure-scsi-controllers-only-when-supported-by-the-guest-operating-system"></a>Configurar controladores SCSI solo cuando sea compatible con el sistema operativo invitado

>Se aplica a: Windows Server 2016



|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema

*Una máquina virtual está configurada con una controladora SCSI que no se puede usar porque el sistema operativo invitado no es compatible con controladores SCSI.*

## <a name="impact"></a>Impacto

*Las máquinas virtuales no pueden usar el almacenamiento conectado a la controladora SCSI. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Solución

*Apague la máquina virtual y use el administrador de Hyper-V para quitarla de la máquina virtual. A continuación, reinicie la máquina virtual.*



