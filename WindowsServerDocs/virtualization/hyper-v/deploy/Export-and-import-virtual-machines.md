---
title: Exportar e importar máquinas virtuales
description: Muestra cómo exportar e importar máquinas virtuales mediante el Administrador de Hyper-V o Windows PowerShell.
ms.prod: windows-server-threshold
author: KBDAzure
ms.author: kathydav
manager: dongill
ms.technology: compute-hyper-v
ms.date: 12/13/2016
ms.topic: article
ms.assetid: 7fd996f5-1ea9-4b16-9776-85fb39a3aa34
ms.openlocfilehash: b326fe8d7ff05ba73fc94225fa38921b42eb3fc6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844326"
---
>Se aplica a: Windows 10, Windows Server 2016 y Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

# <a name="export-and-import-virtual-machines"></a>Exportar e importar máquinas virtuales

Este artículo muestra cómo exportar e importar una máquina virtual, que es una manera rápida para copiarlos o moverlos. Este artículo describen algunas de las opciones para realizar cuando se realiza una exportación o importación.

## <a name="export-a-virtual-machine"></a>Exportar una máquina virtual

Una exportación reúne todos los archivos necesarios en una unidad: archivos de disco duro virtual, los archivos de configuración de máquina virtual y los archivos de punto de comprobación. Puede hacerlo en una máquina virtual que se encuentra en un estado iniciado o detenido.

### <a name="using-hyper-v-manager"></a>Uso del Administrador de Hyper-V

Para crear una exportación de máquina virtual:

1. En el Administrador de Hyper-V, haga clic en la máquina virtual y seleccione **exportar**.

2. Elija dónde quiere almacenar los archivos exportados y haga clic en **exportar**.

Cuando se realiza la exportación, puede ver todos los archivos exportados en la ubicación de exportación.

### <a name="using-powershell"></a>Con PowerShell

Abra una sesión como administrador y ejecute un comando similar al siguiente, después de reemplazar \<nombre de máquina virtual\> y \<ruta\>:

```powershell
Export-VM -Name \<vm name\> -Path \<path\>
```

Para obtener más información, consulte [Export-VM](https://docs.microsoft.com/powershell/module/hyper-v/export-vm).

## <a name="import-a-virtual-machine"></a>Importar una máquina virtual. 

Al importar una máquina virtual, esta se registra con el host de Hyper-V. Puede importar en el host o el nuevo host. Si va a importar en el mismo host, no es necesario exportar la máquina virtual en primer lugar, porque se trata de Hyper-V volver a crear la máquina virtual de archivos disponibles. Al importar una máquina virtual registra, por lo que puede usarse en el host de Hyper-V.

El Asistente para importar máquinas virtuales también le ayuda a corregir las incompatibilidades que pueden existir cuando se mueven desde un host a otro. Se trata normalmente de las diferencias en el hardware físico, como memoria, los conmutadores virtuales y los procesadores virtuales.

### <a name="import-using-hyper-v-manager"></a>Importación mediante el Administrador de Hyper-V

Para importar una máquina virtual:

1. Desde el **acciones** menú en el Administrador de Hyper-V, haga clic en **Importar máquina Virtual**.

2. Haz clic en **Siguiente**.

3. Seleccione la carpeta que contiene los archivos exportados y haga clic en **siguiente**.

4. Seleccione la máquina virtual para importar.

5. Elija el tipo de importación y haga clic en **siguiente**. (Para obtener descripciones, consulte [importar tipos](#import-types), más adelante.)

6. Haga clic en **Finalizar**.

### <a name="import-using-powershell"></a>Importación mediante PowerShell

Use la **Import-VM** cmdlet, a continuación el ejemplo para el tipo de importación que desee. Para obtener descripciones de los tipos, vea [importar tipos](#import-types), a continuación. 

#### <a name="register-in-place"></a>Registrar en su lugar

Este tipo de importación usa los archivos donde se almacenan en el momento de la importación y conserva el identificador de. la máquina virtual El comando siguiente muestra un ejemplo de un archivo de importación. Ejecutar un comando similar con sus propios valores.

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' 
```

#### <a name="restore"></a>Restaurar

Para importar la máquina virtual especificando su propia ruta de acceso para los archivos de máquina virtual, ejecute un comando similar al siguiente, reemplace los ejemplos por sus valores:

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -VhdDestinationPath 'D:\Virtual Machines\WIN10DOC' -VirtualMachinePath 'D:\Virtual Machines\WIN10DOC'
```

#### <a name="import-as-a-copy"></a>Importar como una copia

Para completar una importación de copiar y mover los archivos de máquina virtual a la ubicación predeterminada de Hyper-V, ejecute un comando similar al siguiente, reemplace los ejemplos por sus valores:

``` PowerShell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -GenerateNewId
```

Para obtener más información, consulte [Import-VM](https://docs.microsoft.com/powershell/module/hyper-v/import-vm).

### <a name="import-types"></a>Tipos de importación

Hyper-V ofrece tres tipos de importación:

- **En lugar de registrar** : este tipo se da por supuesto que los archivos de exportación se encuentran en la ubicación donde podrá almacenar y ejecutar la máquina virtual. La máquina virtual importada tiene el mismo identificador que tenía en el momento de la exportación. Por este motivo, si la máquina virtual ya está registrada con Hyper-V, debe eliminarse antes de que funciona la importación. Cuando haya finalizado la importación, los archivos de exportación se convierten en el que se ejecuta archivos de estado y no se puede quitar.

- **Restaurar la máquina virtual** : restaurar la máquina virtual en una ubicación que elija, o use el valor predeterminado para Hyper-V. Este tipo de importación crea una copia de los archivos exportados y los mueve a la ubicación seleccionada. Cuando se importa, la máquina virtual tiene el mismo identificador que tenía en el momento de la exportación. Por este motivo, si ya se está ejecutando la máquina virtual de Hyper-V, debe eliminarse antes de que se puede completar la importación. Cuando haya finalizado la importación, los archivos exportados se mantienen intactos y pueden quitar o volver a importar.

- **Copiar la máquina virtual** : Esto es similar al tipo de restauración que seleccione una ubicación para los archivos. La diferencia es que la máquina virtual importada tiene un nuevo identificador único, lo que significa que puede importar la máquina virtual al mismo host varias veces.

