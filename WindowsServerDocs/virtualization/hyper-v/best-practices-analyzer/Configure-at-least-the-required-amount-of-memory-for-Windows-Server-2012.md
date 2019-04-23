---
title: Configure al menos la cantidad necesaria de memoria para una máquina virtual que ejecuta Windows Server 2012 y habilitado para la memoria dinámica
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 46f9a5dc-355b-415b-863d-fb740609d6b6
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4d8df352a00809cba8ef57b3098d4fef19e1e89e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879156"
---
# <a name="configure-at-least-the-required-amount-of-memory-for-a-virtual-machine-running-windows-server-2012-and-enabled-for-dynamic-memory"></a>Configure al menos la cantidad necesaria de memoria para una máquina virtual que ejecuta Windows Server 2012 y habilitado para la memoria dinámica

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>**Problema**  
*Una o más máquinas virtuales se configuran para utilizar memoria dinámica con menor que la cantidad de memoria necesaria para Windows Server 2012.*  
  
## <a name="impact"></a>**Impact**  
*El sistema operativo invitado en las siguientes máquinas virtuales podrían no ejecutarse o podría ejecutarse no confiable:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Use el Administrador de Hyper-V para aumentar la memoria mínima al menos 256 MB y la memoria de inicio y la memoria máxima al menos 512 MB para esta máquina virtual.*  
  
### <a name="increase-memory-using-hyper-v-manager"></a>Aumente la memoria con el Administrador de Hyper-V  
  
1.  Abre el Administrador Hyper-V. (Desde el administrador del servidor, haga clic en **herramientas** > **Administrador de Hyper-V**.)  
  
2.  En la lista de máquinas virtuales, haga clic en lo que desee y después haga clic en **configuración**.  
  
3.  En el panel de navegación, haga clic en **memoria**.  
  
4.  Cambiar el **RAM** al menos 512 MB.  
  
5.  En **memoria dinámica**, cambie el **RAM mínima** al menos 256 MB y el **RAM máxima** a 512 MB.  
  
6.  Haga clic en **Aceptar**.  
  
### <a name="increase-memory-using-windows-powershell"></a>Aumente la memoria mediante Windows PowerShell  
  
1.  Abra Windows PowerShell. (En el escritorio, haga clic en **iniciar** y comience a escribir **Windows PowerShell**.)  
  
2.  Haga clic en **Windows PowerShell** y haga clic en **ejecutar como administrador**.  
  
3.  Ejecute un comando similar al siguiente, reemplazando MyVM con el nombre de la máquina virtual y la memoria de los valores con al menos los valores que se muestra a continuación.  
  
```  
Get-VM MyVM | Set-VMMemory -DynamicMemoryEnabled $True -MaximumBytes 512MB -MinimumBytes 256MB -StartupBytes 512MB  
```  
  


