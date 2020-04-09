---
title: Reservar una o más redes virtuales externas para uso exclusivo de máquinas virtuales
description: Proporciona instrucciones para resolver el problema que informa esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f7732258-93f1-44e8-835b-5ad2d1c45cd9
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: c371743f20f8192b682ff68045c5d72e9e0f7e8d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861808"
---
# <a name="reserve-one-or-more-external-virtual-networks-for-exclusive-use-by-virtual-machines"></a>Reservar una o más redes virtuales externas para uso exclusivo de máquinas virtuales

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
  
*Todas las redes virtuales externas están configuradas para su uso por parte del sistema operativo de administración y de las máquinas virtuales.*  
  
## <a name="impact"></a>Impacto  
  
*El rendimiento de red puede verse afectado en el sistema operativo de administración.*  
  
## <a name="resolution"></a>Resolución  
  
*Use el administrador de conmutadores virtuales para dejar de compartir una red virtual externa con el sistema operativo de administración.*  
  
#### <a name="to-stop-sharing-the-external-virtual-network-with-the-management-operating-system"></a>Para dejar de compartir la red virtual externa con el sistema operativo de administración  
  
1.  Abre el Administrador Hyper-V. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Administrador de Hyper-V**.  
  
2.  En el menú **Acciones**, haz clic en **Administrador de conmutadores virtuales**.  
  
3.  En **conmutadores virtuales**, haga clic en el nombre del conmutador virtual externo.  
  
4.  En el área **tipo de conexión** , en el nombre del adaptador de red físico, desactive la casilla permitir que **el sistema operativo de administración comparta este adaptador de red** .  
  
5.  Haga clic en **Aceptar**.  
  


