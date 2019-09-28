---
title: Configurar máquinas virtuales para usar SR-IOV solo cuando sea compatible con el sistema operativo invitado
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 33cf5b68-e43e-47ef-adbc-6b266c1d4dce
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8c43e06806f66ce0faae255f0f34d80a653fbe10
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366258"
---
# <a name="configure-virtual-machines-to-use-sr-iov-only-when-supported-by-the-guest-operating-system"></a>Configurar máquinas virtuales para usar SR-IOV solo cuando sea compatible con el sistema operativo invitado

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
*Una o varias máquinas virtuales están configuradas para usar la virtualización de e/s de raíz única (SR-IOV), pero el sistema operativo invitado no es compatible con SR-IOV.*  
  
## <a name="impact"></a>Impacto  
*Las funciones virtuales de SR-IOV no se asignarán a las siguientes máquinas virtuales:*  
  
\<list de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Deshabilite SR-IOV en todas las máquinas virtuales que ejecutan sistemas operativos invitados que no son compatibles con SR-IOV.*  
  
SR-IOV solo se admite en algunos invitados de Windows de 64 bits. Para obtener más información, consulte compatibilidad de las [características de Hyper-V por generación e invitado](../Hyper-V-feature-compatibility-by-generation-and-guest.md).  
  


