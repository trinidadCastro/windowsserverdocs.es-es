---
title: Una máquina virtual que ejecuta Windows Vista y configurado con memoria dinámica debe usar los valores recomendados para la configuración de memoria
description: Proporciona instrucciones para resolver el problema notificado por esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: c35f08b2-e624-4811-a159-c1e5bb6d5281
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b099fb72590decdd59d847e98364e5f6d005dd6a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850586"
---
# <a name="a-virtual-machine-running-windows-vista-and-configured-with-dynamic-memory-should-use-recommended-values-for-memory-settings"></a>Una máquina virtual que ejecuta Windows Vista y configurado con memoria dinámica debe usar los valores recomendados para la configuración de memoria

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
*Una o más máquinas virtuales se configuran para utilizar memoria dinámica con menor que la cantidad de memoria recomendada para Windows Vista.*  
  
### <a name="impact"></a>Impacto  
*El sistema operativo invitado en las siguientes máquinas virtuales podrían no ejecutarse o podría ejecutarse no confiable:*  
  
\<lista de máquinas virtuales >  
      
## <a name="resolution"></a>Resolución  
*Use el Administrador de Hyper-V o Windows PowerShell para aumentar la memoria mínima al menos 256 MB, memoria de inicio al menos 512 MB y la memoria máxima en al menos 1 GB.*  
  
#### <a name="increase-memory-using-hyper-v-manager"></a>Aumente la memoria con el Administrador de Hyper-V  
  
1.  Abre el Administrador Hyper-V. (Desde el administrador del servidor, haga clic en **herramientas** > **Administrador de Hyper-V**.)  
  
2.  En la lista de máquinas virtuales, haga clic en lo que desee y después haga clic en **configuración**.  
  
3.  En el panel de navegación, haga clic en **memoria**.  
  
4.  Cambiar el **RAM** al menos 512 MB.  
  
5.  En **memoria dinámica**, cambie el **RAM mínima** al menos 256 MB y el **RAM máxima** a 1 GB.  
  
6.  Haga clic en **Aceptar**.  
  
### <a name="increase-memory-using-windows-powershell"></a>Aumente la memoria mediante Windows PowerShell  
  
1.  Abra Windows PowerShell. (En el escritorio, haga clic en **iniciar** y comience a escribir **Windows PowerShell**.)  
  
2.  Haga clic en **Windows PowerShell** y haga clic en **ejecutar como administrador**.  
  
3.  Ejecute un comando similar al siguiente, reemplazando MyVM con el nombre de la máquina virtual y la memoria de los valores con al menos los valores que se muestra a continuación.  
  
```  
Get-VM MyVM | Set-VMMemory -DynamicMemoryEnabled $True -MaximumBytes 1GB -MinimumBytes 256MB -StartupBytes 512MB  
```  
  


