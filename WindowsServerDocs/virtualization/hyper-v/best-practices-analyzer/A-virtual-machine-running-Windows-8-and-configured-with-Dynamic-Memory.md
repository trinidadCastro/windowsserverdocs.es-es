---
title: Una máquina virtual que ejecute Windows 8 y que esté configurada con Memoria dinámica debe usar los valores recomendados para la configuración de memoria
description: Proporciona instrucciones para resolver el problema que informa esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a17d774e-62bb-40a7-9ddb-80d07596d51c
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: a551be3a0f1d4bdd0ca8e9ccbb98a583e55ae461
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857918"
---
# <a name="a-virtual-machine-running-windows-8-and-configured-with-dynamic-memory-should-use-recommended-values-for-memory-settings"></a>Una máquina virtual que ejecute Windows 8 y que esté configurada con Memoria dinámica debe usar los valores recomendados para la configuración de memoria

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
*Una o varias máquinas virtuales están configuradas para usar Memoria dinámica con menos de la cantidad de memoria recomendada para Windows 8.*  
  
## <a name="impact"></a>**Impacto**  
*Es posible que el sistema operativo invitado de las siguientes máquinas virtuales no se ejecute o se ejecute de forma no confiable:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Use el administrador de Hyper-V para aumentar la cantidad mínima de memoria hasta 256 MB, la memoria de inicio en al menos 512 MB y la memoria máxima de al menos 1 GB para esta máquina virtual.*  
  
#### <a name="increase-memory-using-hyper-v-manager"></a>Aumentar la memoria con el administrador de Hyper-V  
  
1.  Abre el Administrador Hyper-V. (En Administrador del servidor, haga clic en **herramientas** > **Administrador de Hyper-V**).  
  
2.  En la lista de máquinas virtuales, haga clic con el botón secundario en la que desee y, a continuación, haga clic en **configuración**.  
  
3.  En el panel de navegación, haga clic en **memoria**.  
  
4.  Cambie la **RAM** a un mínimo de 512 MB.  
  
5.  En **memoria dinámica**, cambie la **ram mínima** a 256 MB como mínimo y la **RAM máxima** a 1 GB.  
  
6.  Haga clic en **Aceptar**.  
  
### <a name="increase-memory-using-windows-powershell"></a>Aumentar la memoria mediante Windows PowerShell  
  
1.  Abra Windows PowerShell. (En el escritorio, haga clic en **Inicio** y comience a escribir **Windows PowerShell**).  
  
2.  Haga clic con el botón derecho en **Windows PowerShell** y haga clic en **Ejecutar como administrador**.  
  
3.  Ejecute un comando similar al siguiente, reemplazando MyVM por el nombre de la máquina virtual y los valores de memoria por al menos los valores que se muestran a continuación.  
  
```  
Get-VM MyVM | Set-VMMemory -DynamicMemoryEnabled $True -MaximumBytes 1GB -MinimumBytes 256MB -StartupBytes 512MB  
```  
  


