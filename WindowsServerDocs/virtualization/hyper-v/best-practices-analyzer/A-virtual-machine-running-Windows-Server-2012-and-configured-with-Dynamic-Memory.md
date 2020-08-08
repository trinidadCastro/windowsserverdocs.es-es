---
title: Una máquina virtual que ejecuta Windows Server 2012 y está configurada con Memoria dinámica debe usar los valores recomendados para la configuración de memoria
description: Proporciona instrucciones para resolver el problema que informa esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 0aa35e36-8e3b-498b-b71d-003a0a0947be
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 7c9ffcd0422b0512c431a903acb3e65e70f5c453
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948658"
---
# <a name="a-virtual-machine-running-windows-server-2012-and-configured-with-dynamic-memory-should-use-recommended-values-for-memory-settings"></a>Una máquina virtual que ejecuta Windows Server 2012 y está configurada con Memoria dinámica debe usar los valores recomendados para la configuración de memoria

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>**Problema**
*Una o varias máquinas virtuales están configuradas para usar Memoria dinámica con menos de la cantidad de memoria recomendada para Windows Server 2012.*

## <a name="impact"></a>**Impacto**
*Es posible que el sistema operativo invitado de las siguientes máquinas virtuales no se ejecute o se ejecute de forma no confiable:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolución**
*Use el administrador de Hyper-V para aumentar al menos 256 MB, la memoria de inicio en al menos 512 MB y la memoria máxima de al menos 2 GB para esta máquina virtual.*

#### <a name="increase-memory-using-hyper-v-manager"></a>Aumentar la memoria con el administrador de Hyper-V

1.  Abra el administrador de Hyper-V. (En Administrador del servidor, haga clic en **herramientas**  >  **Administrador de Hyper-V**).

2.  En la lista de máquinas virtuales, haga clic con el botón secundario en la que desee y, a continuación, haga clic en **configuración**.

3.  En el panel de navegación, haga clic en **memoria**.

4.  Cambie la **RAM** a un mínimo de 512 MB.

5.  En **memoria dinámica**, cambie la **ram mínima** a 256 MB como mínimo y la **RAM máxima** a 2 GB.

6.  Haga clic en **Aceptar**.

### <a name="increase-memory-using-windows-powershell"></a>Aumentar la memoria mediante Windows PowerShell

1.  Abra Windows PowerShell. (En el escritorio, haga clic en **Inicio** y comience a escribir **Windows PowerShell**).

2.  Haga clic con el botón derecho en **Windows PowerShell** y haga clic en **Ejecutar como administrador**.

3.  Ejecute un comando similar al siguiente, reemplazando MyVM por el nombre de la máquina virtual y los valores de memoria por al menos los valores que se muestran a continuación.

```
Get-VM MyVM | Set-VMMemory -DynamicMemoryEnabled $True -MaximumBytes 2GB -MinimumBytes 256MB -StartupBytes 512MB
```



