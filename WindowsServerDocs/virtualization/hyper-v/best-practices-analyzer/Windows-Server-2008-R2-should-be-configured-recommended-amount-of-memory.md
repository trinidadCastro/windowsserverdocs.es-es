---
title: Windows Server 2008 R2 debe configurarse con la cantidad de memoria recomendada
description: Proporciona instrucciones para resolver el problema que informa esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 26872519-ccf0-4757-827f-8df2a7a2b9f9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e5b103ed5f41b3d63320a0d9be85c7e52781041d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393177"
---
# <a name="windows-server-2008-r2-should-be-configured-with-the-recommended-amount-of-memory"></a>Windows Server 2008 R2 debe configurarse con la cantidad de memoria recomendada

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
  
*Una máquina virtual que ejecuta Windows Server 2008 R2 está configurada con menos de la cantidad de RAM recomendada, que es 2 GB.*  
  
## <a name="impact"></a>Impacto  
  
es posible que las aplicaciones y el sistema operativo invitado de @no__t 0The no funcionen correctamente. Puede que no haya suficiente memoria para ejecutar varias aplicaciones a la vez. Esto afecta a las siguientes máquinas virtuales: *  
  
@no__t: 0list de nombres de máquina virtual >  
  
## <a name="resolution"></a>Resolución  
  
*Use el administrador de Hyper-V para aumentar la memoria asignada a esta máquina virtual a 2 GB como mínimo.*  
  
### <a name="to-increase-the-memory-using-hyper-v-manager"></a>Para aumentar la memoria mediante el administrador de Hyper-V  
  
1.  Abre el Administrador Hyper-V. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Administrador de Hyper-V**.  
  
2.  En el panel de resultados, en **virtual machines**, seleccione la máquina virtual que desea configurar. El estado de la máquina virtual debe aparecer como **desactivado**. Si no es así, haga clic con el botón secundario en la máquina virtual y, a continuación, haga clic en **apagar**.  
  
3.  En el panel **Acción**, en el nombre de la máquina virtual, haga clic en **Configuración**.  
  
4.  En el panel de navegación, haga clic en **memoria**.  
  
5.  En la página **memoria** , establezca la **RAM de inicio** en al menos 2 GB y, a continuación, haga clic en **Aceptar**.  
  
### <a name="increase-the-memory-using-windows-powershell"></a>Aumentar la memoria mediante Windows PowerShell  
  
1.  Abra Windows PowerShell. (En el escritorio, haga clic en **Inicio** y comience a escribir **Windows PowerShell**).  
  
2.  Haga clic con el botón derecho en **Windows PowerShell** y haga clic en **Ejecutar como administrador**.  
  
3.  Ejecute este comando después de reemplazar \<MyVM > por el nombre de la máquina virtual:  
  
```  
Set-VMMemory <MyVM> -StartupBytes 2GB  
```  
  
## <a name="see-also"></a>Vea también  
[Set-VMMemory](https://technet.microsoft.com/library/hh848572.aspx)  
  


