---
title: Configurar controladores SCSI solo cuando sea compatible con el sistema operativo invitado
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 861f194f-467e-4b07-a1c5-55b35f6327c4
ms.date: 8/16/2016
ms.openlocfilehash: cadf4c0d0ce8cdd50d66a61f428b3e547dd457dc
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90747000"
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



