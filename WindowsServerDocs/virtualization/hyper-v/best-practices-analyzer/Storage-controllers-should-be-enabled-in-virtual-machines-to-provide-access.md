---
title: Controladores de almacenamiento deben estar habilitados en las máquinas virtuales para proporcionar acceso a almacenamiento conectado
description: Proporciona instrucciones para resolver el problema notificado por esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 532548a1-8ffe-4b5b-902e-ed2f0819012b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 42803a0eef84bf006e9f9e7ed6297ea21b4eb7b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849166"
---
# <a name="storage-controllers-should-be-enabled-in-virtual-machines-to-provide-access-to-attached-storage"></a>Controladores de almacenamiento deben estar habilitados en las máquinas virtuales para proporcionar acceso a almacenamiento conectado

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
  
*Uno o más controladores de almacenamiento se pueden deshabilitar en una máquina virtual.*  
  
## <a name="impact"></a>Impacto  
  
*Las máquinas virtuales no se puede usar el almacenamiento conectado a un controlador de almacenamiento deshabilitada. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de nombres de máquina virtual >  
  
## <a name="resolution"></a>Resolución  
  
*Use el Administrador de dispositivos en el sistema operativo invitado para habilitar todos los controladores de almacenamiento. Si no se requiere el controlador de almacenamiento, utilice el Administrador de Hyper-V para quitarlo de la máquina virtual.*  
  
Para obtener instrucciones sobre cómo usar el Administrador de dispositivos, consulte la Ayuda en el sistema operativo invitado. Para obtener instrucciones sobre cómo quitar el controlador de almacenamiento, vea el procedimiento siguiente.  
  
#### <a name="to-remove-a-scsi-storage-controller-from-the-virtual-machine"></a>Para quitar un controlador de almacenamiento SCSI de la máquina virtual  
  
1.  Abre el Administrador Hyper-V. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Administrador de Hyper-V**.  
  
2.  En el panel de resultados, bajo **máquinas virtuales**, seleccione la máquina virtual que desea configurar.  
  
3.  Si está ejecutando la máquina virtual, apague la máquina virtual. Haga clic en la máquina virtual y haga clic en **apagar**.  
  
4.  En el panel **Acción**, en el nombre de la máquina virtual, haga clic en **Configuración**.  
  
5.  En el panel izquierdo de la **configuración** cuadro de diálogo **Hardware**, haga clic en **controladora SCSI**.  
  
6.  En el panel derecho, haga clic en **quitar**.  
  


