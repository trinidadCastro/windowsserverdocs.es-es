---
title: La extensión de conmutador virtual de WFP debe habilitarse si así lo requieren las extensiones de terceros
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8aa8a9a5-e3fa-4c9b-8331-ba5a3de22429
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: d4cc23ce638f7b5ee95f80de067b4ad5b360d118
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859308"
---
# <a name="the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions"></a>La extensión de conmutador virtual de WFP debe habilitarse si así lo requieren las extensiones de terceros

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>**Problema**  
*La extensión de conmutador virtual de la plataforma de filtrado de Windows (WFP) está deshabilitada.*  
  
## <a name="impact"></a>**Impacto**  
*Es posible que algunas extensiones de conmutador virtual de terceros no funcionen correctamente en los siguientes conmutadores virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Use el cmdlet de Windows PowerShell, enable-VMSwitchExtension, para habilitar la plataforma de filtrado de Windows si es necesaria para las extensiones de terceros.*  
  
### <a name="enable-the-windows-filtering-platform-using-windows-powershell"></a>Habilitar la plataforma de filtrado de Windows mediante Windows PowerShell  
  
1.  Abra Windows PowerShell. (En el escritorio, haga clic en **Inicio** y comience a escribir **Windows PowerShell**).  
  
2.  Haga clic con el botón derecho en **Windows PowerShell** y haga clic en **Ejecutar como administrador**.  
  
3.  Ejecute este comando después de reemplazar external por el nombre del conmutador externo:  
  
```  
Enable-VMSwitchExtension -VMSwitchName External -Name Microsoft Windows Filtering Platform  
```  
  
## <a name="see-also"></a>Consulta también  
[Enable-VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx)  
  


