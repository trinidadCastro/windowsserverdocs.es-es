---
title: La extensión de conmutador virtual de WFP debe habilitarse si así lo requieren las extensiones de terceros
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8aa8a9a5-e3fa-4c9b-8331-ba5a3de22429
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5afe706c246276597b32400109370ba3129e5a24
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850626"
---
# <a name="the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions"></a>La extensión de conmutador virtual de WFP debe habilitarse si así lo requieren las extensiones de terceros

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>**Problema**  
*La extensión de conmutador virtual de Windows Filtering Platform (WFP) está deshabilitada.*  
  
## <a name="impact"></a>**Impact**  
*Algunas extensiones de conmutador virtual de terceros podrían no funcionar correctamente en los conmutadores virtuales siguientes:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Use el cmdlet de Windows PowerShell, Enable-VMSwitchExtension, para habilitar la plataforma de filtrado de Windows si así lo requieren las extensiones de terceros.*  
  
### <a name="enable-the-windows-filtering-platform-using-windows-powershell"></a>Habilitar la plataforma de filtrado de Windows mediante Windows PowerShell  
  
1.  Abra Windows PowerShell. (En el escritorio, haga clic en **iniciar** y comience a escribir **Windows PowerShell**.)  
  
2.  Haga clic en **Windows PowerShell** y haga clic en **ejecutar como administrador**.  
  
3.  Ejecute este comando después de reemplazar externo con el nombre del conmutador externo:  
  
```  
Enable-VMSwitchExtension -VMSwitchName External -Name "Microsoft Windows Filtering Platform"  
```  
  
## <a name="see-also"></a>Vea también  
[Enable-VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx)  
  


