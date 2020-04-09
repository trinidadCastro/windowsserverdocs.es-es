---
title: Windows 8 debe configurarse con al menos la cantidad mínima de memoria
description: Proporciona instrucciones para resolver el problema que informa esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 519d1091-fa4d-44d7-83ca-83f6aa71fb7d
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 1c979299fbd66ef23ae637488fe3c4f3a3ccbbc6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857658"
---
# <a name="windows-8-should-be-configured-with-at-least-the-minimum-amount-of-memory"></a>Windows 8 debe configurarse con al menos la cantidad mínima de memoria

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes se proporcionan detalles sobre el problema específico. Cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para el problema específico.  
  
## <a name="issue"></a>**Problema**  
*Una máquina virtual que ejecuta Windows 8 se configura con menos de la cantidad mínima de RAM, que es 512 MB.*  
  
## <a name="impact"></a>**Impacto**  
*Es posible que el sistema operativo invitado de las siguientes máquinas virtuales no se ejecute o se ejecute de forma no confiable:*  
```  
<list of virtual machines>  
```  
## <a name="resolution"></a>**Resolución**  
*Use el administrador de Hyper-V para aumentar la memoria asignada a esta máquina virtual al menos 512 MB.*  
  
#### <a name="increase-the-memory-using-hyper-v-manager"></a>Aumentar la memoria mediante el administrador de Hyper-V  
  
1.  Abre el Administrador Hyper-V. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Administrador de Hyper-V**.  
  
2.  En el panel de resultados, en **virtual machines**, seleccione la máquina virtual que desea configurar. El estado de la máquina virtual debe aparecer como **desactivado**. Si no es así, haga clic con el botón secundario en la máquina virtual y, a continuación, haga clic en **apagar**.  
  
3.  En el panel **Acción**, en el nombre de la máquina virtual, haga clic en **Configuración**.  
  
4.  En el panel de navegación, haga clic en **memoria**.  
  
5.  En la página **memoria** , establezca la **RAM de inicio** en al menos 512 MB y, a continuación, haga clic en **Aceptar**.  
  
### <a name="increase-the-memory-using-windows-powershell"></a>Aumentar la memoria mediante Windows PowerShell  
  
1.  Abra Windows PowerShell. (En el escritorio, haga clic en **Inicio** y comience a escribir **Windows PowerShell**).  
  
2.  Haga clic con el botón derecho en **Windows PowerShell** y haga clic en **Ejecutar como administrador**.  
  
3.  Ejecute este comando después de reemplazar \<MyVM > por el nombre de la máquina virtual:  
  
```  
Set-VMMemory <MyVM> -StartupBytes 512MB  
```  
  
## <a name="see-also"></a>Consulta también  
[Set-VMMemory](https://technet.microsoft.com/library/hh848572.aspx)  
  


