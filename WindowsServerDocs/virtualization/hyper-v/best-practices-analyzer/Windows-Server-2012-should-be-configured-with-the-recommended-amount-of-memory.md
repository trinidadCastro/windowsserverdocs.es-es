---
title: Windows Server 2012 debe configurarse con la cantidad de memoria recomendada
description: Proporciona instrucciones para resolver el problema que informa esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 12d0b473-cf6a-4746-b03d-2ceeb701c5d0
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c81af38c3ff3d1a48fc733d7614ded1709a71799
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364422"
---
# <a name="windows-server-2012-should-be-configured-with-the-recommended-amount-of-memory"></a>Windows Server 2012 debe configurarse con la cantidad de memoria recomendada

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|   
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>**Problema**  
*Una máquina virtual que ejecuta Windows Server 2012 se configura con menos de la cantidad de RAM recomendada, que es 2 GB.*  
  
## <a name="impact"></a>**Impacto**  
es posible que las aplicaciones y el sistema operativo invitado de @no__t 0The no funcionen correctamente. Puede que no haya suficiente memoria para ejecutar varias aplicaciones a la vez. Esto afecta a las siguientes máquinas virtuales: *  
  
\<list de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Use el administrador de Hyper-V para aumentar la memoria asignada a esta máquina virtual a 2 GB como mínimo.*  
  
### <a name="increase-the-memory-using-hyper-v-manager"></a>Aumentar la memoria mediante el administrador de Hyper-V  
  
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
  


