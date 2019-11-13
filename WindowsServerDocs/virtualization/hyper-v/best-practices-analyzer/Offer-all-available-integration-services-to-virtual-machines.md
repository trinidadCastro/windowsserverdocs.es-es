---
title: Ofrecer todos los servicios de integración disponibles a las máquinas virtuales
description: Proporciona instrucciones para resolver el problema que informa esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2c4b2043-ad81-495e-aa7a-467f813bb3d2
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5ec2dc73cea8b8356d832bf9fdb960985df2df6c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393602"
---
# <a name="offer-all-available-integration-services-to-virtual-machines"></a>Ofrecer todos los servicios de integración disponibles a las máquinas virtuales

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
  
*Uno o varios servicios de integración disponibles no están habilitados en las máquinas virtuales.*  
  
## <a name="impact"></a>Impacto  
  
*Algunas funcionalidades no estarán disponibles para las siguientes máquinas virtuales:*  
  
\<lista de nombres de máquina virtual >  
  
## <a name="resolution"></a>Resolución  
  
*Si esto es intencionado, no es necesario realizar ninguna otra acción. En caso contrario, considere la posibilidad de ofrecer todos los servicios de integración en la configuración de estas máquinas virtuales.*  
  
La disponibilidad de algunos servicios de integración se puede administrar a través de la configuración de la máquina virtual.   
  
#### <a name="to-manage-the-availability-of-integration-services-to-a-virtual-machine"></a>Para administrar la disponibilidad de los servicios de integración en una máquina virtual  
  
1.  Abre el Administrador Hyper-V. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Administrador de Hyper-V**.  
  
2.  En el panel de resultados, en **virtual machines**, seleccione la máquina virtual que desea configurar.  
  
3.  En el panel **Acción**, en el nombre de la máquina virtual, haga clic en **Configuración**.  
  
4.  En **Administración**, haga clic en **Integration Services**.  
  
5.  En la lista de servicios de integración, active la casilla de cada servicio que desee ofrecer a la máquina virtual. Si una casilla no está disponible, el sistema operativo invitado que se ejecuta en la máquina virtual no admite ese servicio de integración determinado.  
  


