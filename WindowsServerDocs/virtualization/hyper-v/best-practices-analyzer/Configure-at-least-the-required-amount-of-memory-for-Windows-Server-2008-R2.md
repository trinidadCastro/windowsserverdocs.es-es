---
title: Configure al menos la cantidad de memoria necesaria para una máquina virtual que ejecute Windows Server 2008 R2 y que esté habilitada para Memoria dinámica
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: db0bff4c-39fe-49c1-903a-1f2a0b0db891
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 7745a0399314726ac4d8fa1f67865b43c730464d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365051"
---
# <a name="configure-at-least-the-required-amount-of-memory-for-a-virtual-machine-running-windows-server-2008-r2-and-enabled-for-dynamic-memory"></a>Configure al menos la cantidad de memoria necesaria para una máquina virtual que ejecute Windows Server 2008 R2 y que esté habilitada para Memoria dinámica

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
*Una o varias máquinas virtuales están configuradas para usar Memoria dinámica con menos de la cantidad de memoria necesaria para Windows Server 2008 R2.*  
  
## <a name="impact"></a>Impacto  
*Es posible que el sistema operativo invitado de las siguientes máquinas virtuales no se ejecute o se ejecute de forma no confiable:*  
  
\<list de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Use el administrador de Hyper-V o Windows PowerShell para aumentar la cantidad mínima de memoria hasta 256 MB y la memoria de inicio y la memoria máxima al menos 512 MB.*  
  
### <a name="increase-memory-using-hyper-v-manager"></a>Aumentar la memoria con el administrador de Hyper-V  
  
1.  Abre el Administrador Hyper-V. (En Administrador del servidor, haga clic en **herramientas** > **Administrador de Hyper-V**).  
  
2.  En la lista de máquinas virtuales, haga clic con el botón secundario en la que desee y, a continuación, haga clic en **configuración**.  
  
3.  En el panel de navegación, haga clic en **memoria**.  
  
4.  Cambie la **RAM** a un mínimo de 512 MB.  
  
5.  En **memoria dinámica**, cambie la **ram mínima** a 256 MB como mínimo y la **RAM máxima** a 512 MB.  
  
6.  Haga clic en **Aceptar**.  
  
### <a name="increase-memory-using-windows-powershell"></a>Aumentar la memoria mediante Windows PowerShell  
  
1.  Abra Windows PowerShell. (En el escritorio, haga clic en **Inicio** y comience a escribir **Windows PowerShell**).  
  
2.  Haga clic con el botón derecho en **Windows PowerShell** y haga clic en **Ejecutar como administrador**.  
  
3.  Ejecute un comando similar al siguiente, reemplazando MyVM por el nombre de la máquina virtual y los valores de memoria por al menos los valores que se muestran a continuación.  
  
```  
Get-VM MyVM | Set-VMMemory -DynamicMemoryEnabled $True -MaximumBytes 512MB -MinimumBytes 256MB -StartupBytes 512MB  
```  
  


