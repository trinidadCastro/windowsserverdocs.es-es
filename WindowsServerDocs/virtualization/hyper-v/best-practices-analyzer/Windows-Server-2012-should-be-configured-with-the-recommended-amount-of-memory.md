---
title: Windows Server 2012 debe configurarse con la cantidad de memoria recomendada
description: Obtenga información acerca de qué hacer cuando una máquina virtual que ejecuta Windows Server 2012 se configura con menos de la cantidad de RAM recomendada, que es 2 GB.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 12d0b473-cf6a-4746-b03d-2ceeb701c5d0
ms.date: 8/16/2016
ms.openlocfilehash: 42a290188469cbca8552c22c2c70c531e29c409e
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97833640"
---
# <a name="windows-server-2012-should-be-configured-with-the-recommended-amount-of-memory"></a>Windows Server 2012 debe configurarse con la cantidad de memoria recomendada

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
*Una máquina virtual que ejecuta Windows Server 2012 se configura con menos de la cantidad de RAM recomendada, que es 2 GB.*

## <a name="impact"></a>**Impacto**
*Es posible que el sistema operativo invitado y las aplicaciones no funcionen correctamente. Puede que no haya suficiente memoria para ejecutar varias aplicaciones a la vez. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolución**
*Use el administrador de Hyper-V para aumentar la memoria asignada a esta máquina virtual a 2 GB como mínimo.*

### <a name="increase-the-memory-using-hyper-v-manager"></a>Aumentar la memoria mediante el administrador de Hyper-V

1.  Abra el administrador de Hyper-V. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Administrador de Hyper-V**.

2.  En el panel de resultados, en **virtual machines**, seleccione la máquina virtual que desea configurar. El estado de la máquina virtual debe aparecer como **desactivado**. Si no es así, haga clic con el botón secundario en la máquina virtual y, a continuación, haga clic en **apagar**.

3.  En el panel **Acción**, en el nombre de la máquina virtual, haga clic en **Configuración**.

4.  En el panel de navegación, haga clic en **memoria**.

5.  En la página **memoria** , establezca la **RAM de inicio** en al menos 2 GB y, a continuación, haga clic en **Aceptar**.

### <a name="increase-the-memory-using-windows-powershell"></a>Aumentar la memoria mediante Windows PowerShell

1.  Abra Windows PowerShell. (En el escritorio, haga clic en **Inicio** y comience a escribir **Windows PowerShell**).

2.  Haga clic con el botón derecho en **Windows PowerShell** y haga clic en **Ejecutar como administrador**.

3.  Ejecute este comando después \<MyVM> de reemplazar por el nombre de la máquina virtual:

```
Set-VMMemory <MyVM> -StartupBytes 2GB
```

## <a name="see-also"></a>Consulte también
[Set-VMMemory](/powershell/module/hyper-v/set-vmmemory)
