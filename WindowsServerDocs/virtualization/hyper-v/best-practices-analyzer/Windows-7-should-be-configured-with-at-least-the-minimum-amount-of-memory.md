---
title: Windows 7 debe estar configurado con al menos la cantidad mínima de memoria
description: Proporciona instrucciones para resolver el problema notificado por esta regla de Best Practices Analyzer".
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1b81ec0b-ceca-4fba-83ea-90d5f1d9bda8
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: de9b39731bb1e0376cbc54add33d4b91974d11c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840416"
---
# <a name="windows-7-should-be-configured-with-at-least-the-minimum-amount-of-memory"></a>Windows 7 debe estar configurado con al menos la cantidad mínima de memoria

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
  
*Una máquina virtual con Windows 7 se configura con menor que la cantidad mínima de RAM, que es de 512 MB.*  
  
## <a name="impact"></a>Impacto  
  
*El sistema operativo invitado en las siguientes máquinas virtuales podrían no ejecutarse o podría ejecutarse no confiable:*  
```  
<list of virtual machine names>  
```  
## <a name="resolution"></a>Resolución  
  
*Use el Administrador de Hyper-V para aumentar la memoria asignada a esta máquina virtual al menos 512 MB.*  
  
### <a name="to-increase-the-memory-using-hyper-v-manager"></a>Para aumentar la memoria con el Administrador de Hyper-V  
  
1.  Abre el Administrador Hyper-V. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Administrador de Hyper-V**.  
  
2.  En el panel de resultados, bajo **máquinas virtuales**, seleccione la máquina virtual que desea configurar. El estado de la máquina virtual debe aparecer como **desactivar**. Si no es así, haga clic en la máquina virtual y, a continuación, haga clic en **apagar**.  
  
3.  En el panel **Acción**, en el nombre de la máquina virtual, haga clic en **Configuración**.  
  
4.  En el panel de navegación, haga clic en **memoria**.  
  
5.  En el **memoria** , establezca el **RAM de inicio** para al menos 512 MB y, a continuación, haga clic en **Aceptar**.  
  
### <a name="increase-the-memory-using-windows-powershell"></a>Aumente la cantidad de memoria mediante Windows PowerShell  
  
1.  Abra Windows PowerShell. (En el escritorio, haga clic en **iniciar** y comience a escribir **Windows PowerShell**.)  
  
2.  Haga clic en **Windows PowerShell** y haga clic en **ejecutar como administrador**.  
  
3.  Ejecute este comando después de reemplazar \<MyVM > con el nombre de la máquina virtual:  
  
```  
Set-VMMemory <MyVM> -StartupBytes 512MB  
```  
  
## <a name="see-also"></a>Vea también  
[Set-VMMemory](https://technet.microsoft.com/library/hh848572.aspx)  
  


