---
title: Todos los servicios de integración disponibles a las máquinas virtuales de la oferta
description: Proporciona instrucciones para resolver el problema notificado por esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2c4b2043-ad81-495e-aa7a-467f813bb3d2
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c2b5137594f78980f87f6520ae4b4af8203aef32
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883786"
---
# <a name="offer-all-available-integration-services-to-virtual-machines"></a>Todos los servicios de integración disponibles a las máquinas virtuales de la oferta

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
  
*Uno o varios servicios de integración disponibles no están habilitados en las máquinas virtuales.*  
  
## <a name="impact"></a>Impacto  
  
*Algunas funciones no estarán disponibles para las siguientes máquinas virtuales:*  
  
\<lista de nombres de máquina virtual >  
  
## <a name="resolution"></a>Resolución  
  
*Si esto es intencionado, se requiere ninguna acción adicional. De lo contrario, considere la posibilidad de ofrecer todos los servicios de integración en la configuración de estas máquinas virtuales.*  
  
La disponibilidad de algunos servicios de integración puede administrarse a través de la configuración de máquina virtual.   
  
#### <a name="to-manage-the-availability-of-integration-services-to-a-virtual-machine"></a>Para administrar la disponibilidad de servicios de integración a una máquina virtual  
  
1.  Abre el Administrador Hyper-V. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Administrador de Hyper-V**.  
  
2.  En el panel de resultados, bajo **máquinas virtuales**, seleccione la máquina virtual que desea configurar.  
  
3.  En el panel **Acción**, en el nombre de la máquina virtual, haga clic en **Configuración**.  
  
4.  En **administración**, haga clic en **Integration Services**.  
  
5.  En la lista de servicios de integración, seleccione la casilla de verificación para cada servicio que desea ofrecer a la máquina virtual. Si una casilla de verificación no está disponible, que no se admite el servicio de integración determinado por el sistema operativo invitado que se ejecuta en la máquina virtual.  
  


