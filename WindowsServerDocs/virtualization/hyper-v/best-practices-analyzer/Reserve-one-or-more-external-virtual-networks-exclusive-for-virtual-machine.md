---
title: Reserve uno o más redes virtuales externas para uso exclusivo por máquinas virtuales
description: Proporciona instrucciones para resolver el problema notificado por esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f7732258-93f1-44e8-835b-5ad2d1c45cd9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c8c90a74352bae0b348608db0fc05107e4d09010
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884746"
---
# <a name="reserve-one-or-more-external-virtual-networks-for-exclusive-use-by-virtual-machines"></a>Reserve uno o más redes virtuales externas para uso exclusivo por máquinas virtuales

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
  
*Todas las redes virtuales externas se configuran para su uso por el sistema operativo de administración y las máquinas virtuales.*  
  
## <a name="impact"></a>Impacto  
  
*Rendimiento de redes puede verse degradado en el sistema operativo de administración.*  
  
## <a name="resolution"></a>Resolución  
  
*Use el Administrador de conmutadores virtuales para dejar de compartir una red virtual externa con el sistema operativo de administración.*  
  
#### <a name="to-stop-sharing-the-external-virtual-network-with-the-management-operating-system"></a>Para dejar de compartir la red virtual externa con el sistema operativo de administración  
  
1.  Abre el Administrador Hyper-V. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Administrador de Hyper-V**.  
  
2.  En el menú **Acciones**, haz clic en **Administrador de conmutadores virtuales**.  
  
3.  En **conmutadores virtuales**, haga clic en el nombre del conmutador virtual externo.  
  
4.  En el **tipo de conexión** área bajo el nombre del adaptador de red físico, desactive la **permitir que el sistema operativo de administración comparta este adaptador de red** casilla de verificación.  
  
5.  Haga clic en **Aceptar**.  
  


