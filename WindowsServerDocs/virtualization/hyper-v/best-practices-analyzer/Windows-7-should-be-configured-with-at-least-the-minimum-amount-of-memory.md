---
title: Windows 7 debe configurarse con al menos la cantidad mínima de memoria
description: Obtenga información acerca de qué hacer cuando una máquina virtual que ejecuta Windows 7 está configurada con menos de la cantidad mínima de RAM, que es 512 MB.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 1b81ec0b-ceca-4fba-83ea-90d5f1d9bda8
ms.date: 8/16/2016
ms.openlocfilehash: e8f24fd0ebb0bcd40efcc11b4ba003339055077b
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834040"
---
# <a name="windows-7-should-be-configured-with-at-least-the-minimum-amount-of-memory"></a>Windows 7 debe configurarse con al menos la cantidad mínima de memoria

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Error|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema

*Una máquina virtual que ejecuta Windows 7 se configura con menos de la cantidad mínima de RAM, que es 512 MB.*

## <a name="impact"></a>Impacto

*Es posible que el sistema operativo invitado de las siguientes máquinas virtuales no se ejecute o se ejecute de forma no confiable:*
```
<list of virtual machine names>
```
## <a name="resolution"></a>Solución

*Use el administrador de Hyper-V para aumentar la memoria asignada a esta máquina virtual al menos 512 MB.*

### <a name="to-increase-the-memory-using-hyper-v-manager"></a>Para aumentar la memoria mediante el administrador de Hyper-V

1.  Abra el administrador de Hyper-V. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Administrador de Hyper-V**.

2.  En el panel de resultados, en **virtual machines**, seleccione la máquina virtual que desea configurar. El estado de la máquina virtual debe aparecer como **desactivado**. Si no es así, haga clic con el botón secundario en la máquina virtual y, a continuación, haga clic en **apagar**.

3.  En el panel **Acción**, en el nombre de la máquina virtual, haga clic en **Configuración**.

4.  En el panel de navegación, haga clic en **memoria**.

5.  En la página **memoria** , establezca la **RAM de inicio** en al menos 512 MB y, a continuación, haga clic en **Aceptar**.

### <a name="increase-the-memory-using-windows-powershell"></a>Aumentar la memoria mediante Windows PowerShell

1.  Abra Windows PowerShell. (En el escritorio, haga clic en **Inicio** y comience a escribir **Windows PowerShell**).

2.  Haga clic con el botón derecho en **Windows PowerShell** y haga clic en **Ejecutar como administrador**.

3.  Ejecute este comando después \<MyVM> de reemplazar por el nombre de la máquina virtual:

```
Set-VMMemory <MyVM> -StartupBytes 512MB
```

## <a name="see-also"></a>Consulte también
[Set-VMMemory](/powershell/module/hyper-v/set-vmmemory)
