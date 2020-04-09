---
title: Configurar controladores SCSI solo cuando sea compatible con el sistema operativo invitado
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 861f194f-467e-4b07-a1c5-55b35f6327c4
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: cf206d9568ef7634d724f3fce450985c34ebfac5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862168"
---
# <a name="configure-scsi-controllers-only-when-supported-by-the-guest-operating-system"></a>Configurar controladores SCSI solo cuando sea compatible con el sistema operativo invitado

>Se aplica a: Windows Server 2016


  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
  
*Una máquina virtual está configurada con una controladora SCSI que no se puede usar porque el sistema operativo invitado no es compatible con controladores SCSI.*  
  
## <a name="impact"></a>Impacto  
  
*Las máquinas virtuales no pueden usar el almacenamiento conectado a la controladora SCSI. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
  
*Apague la máquina virtual y use el administrador de Hyper-V para quitarla de la máquina virtual. A continuación, reinicie la máquina virtual.*  
  


