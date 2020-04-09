---
title: Exportar e importar máquinas virtuales
description: Muestra cómo exportar e importar máquinas virtuales mediante el administrador de Hyper-V o Windows PowerShell.
ms.prod: windows-server
author: kbdazure
ms.author: kathydav
manager: dongill
ms.technology: compute-hyper-v
ms.date: 12/13/2016
ms.topic: article
ms.assetid: 7fd996f5-1ea9-4b16-9776-85fb39a3aa34
ms.openlocfilehash: 1e9cd8710a53c1e5d9d97e464c32dbf7f17d29a7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860898"
---
>Se aplica a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

# <a name="export-and-import-virtual-machines"></a>Exportar e importar máquinas virtuales

En este artículo se muestra cómo exportar e importar una máquina virtual, que es una forma rápida de moverla o copiarla. En este artículo también se describen algunas de las opciones que se deben tomar al realizar una exportación o importación.

## <a name="export-a-virtual-machine"></a>Exportar una máquina virtual

Una exportación recopila todos los archivos necesarios en una unidad: archivos de disco duro virtual, archivos de configuración de máquina virtual y archivos de punto de comprobación. Puede hacerlo en una máquina virtual que se encuentra en estado iniciado o detenido.

### <a name="using-hyper-v-manager"></a>Uso del Administrador de Hyper-V

Para crear una exportación de máquina virtual:

1. En el administrador de Hyper-V, haga clic con el botón derecho en la máquina virtual y seleccione **exportar**.

2. Elija dónde desea almacenar los archivos exportados y haga clic en **exportar**.

Una vez finalizada la exportación, podrá ver todos los archivos exportados en la ubicación de exportación.

### <a name="using-powershell"></a>Uso de PowerShell

Abra una sesión como administrador y ejecute un comando similar al siguiente, después de reemplazar \<nombre de la máquina virtual\> y \<ruta de acceso\>:

```powershell
Export-VM -Name \<vm name\> -Path \<path\>
```

Para obtener más información, consulte [Export-VM](https://docs.microsoft.com/powershell/module/hyper-v/export-vm).

## <a name="import-a-virtual-machine"></a>Importar una máquina virtual. 

Al importar una máquina virtual, esta se registra con el host de Hyper-V. Puede volver a importar en el host o en el host nuevo. Si está importando al mismo host, no es necesario que exporte la máquina virtual en primer lugar, porque Hyper-V intenta volver a crear la máquina virtual a partir de los archivos disponibles. Al importar una máquina virtual, se registra para que se pueda usar en el host de Hyper-V.

El Asistente para importar máquinas virtuales también le ayuda a corregir las incompatibilidades que pueden existir al pasar de un host a otro. Esto suele ser una diferencia en el hardware físico, como la memoria, los conmutadores virtuales y los procesadores virtuales.

### <a name="import-using-hyper-v-manager"></a>Importar mediante el administrador de Hyper-V

Para importar una máquina virtual:

1. En el menú **acciones** del administrador de Hyper-V, haga clic en **importar máquina virtual**.

2. Haga clic en **Siguiente**.

3. Seleccione la carpeta que contiene los archivos exportados y haga clic en **siguiente**.

4. Seleccione la máquina virtual que se va a importar.

5. Elija el tipo de importación y haga clic en **siguiente**. (Para obtener descripciones, vea [tipos de importación](#import-types), a continuación).

6. Haga clic en **Finalizar**.

### <a name="import-using-powershell"></a>Importar mediante PowerShell

Use el cmdlet **Import-VM** , siguiendo el ejemplo para el tipo de importación que desee. Para obtener descripciones de los tipos, vea [tipos de importación](#import-types), más abajo. 

#### <a name="register-in-place"></a>Regístrese en su lugar

Este tipo de importación utiliza los archivos donde se almacenan en el momento de la importación y conserva el identificador de la máquina virtual. El comando siguiente muestra un ejemplo de un archivo de importación. Ejecute un comando similar con sus propios valores.

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' 
```

#### <a name="restore"></a>Restaurar

Para importar la máquina virtual especificando su propia ruta de acceso para los archivos de la máquina virtual, ejecute un comando similar al siguiente y reemplace los ejemplos por los valores siguientes:

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -VhdDestinationPath 'D:\Virtual Machines\WIN10DOC' -VirtualMachinePath 'D:\Virtual Machines\WIN10DOC'
```

#### <a name="import-as-a-copy"></a>Importar como copia

Para completar una importación de copia y trasladar los archivos de la máquina virtual a la ubicación predeterminada de Hyper-V, ejecute un comando similar al siguiente y reemplace los ejemplos por los valores siguientes:

``` PowerShell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -GenerateNewId
```

Para obtener más información, consulte [Import-VM](https://docs.microsoft.com/powershell/module/hyper-v/import-vm).

### <a name="import-types"></a>Importar tipos

Hyper-V ofrece tres tipos de importación:

- **Registrar en contexto** : este tipo presupone que los archivos de exportación se encuentran en la ubicación donde se almacenará y ejecutará la máquina virtual. La máquina virtual importada tiene el mismo identificador que tenía en el momento de la exportación. Por este motivo, si la máquina virtual ya está registrada con Hyper-V, debe eliminarse antes de que funcione la importación. Una vez completada la importación, los archivos de exportación se convierten en los archivos de estado en ejecución y no se pueden quitar.

- **Restaurar la máquina virtual** : restaure la máquina virtual en la ubicación que elija o use el valor predeterminado en Hyper-V. Este tipo de importación crea una copia de los archivos exportados y los mueve a la ubicación seleccionada. Cuando se importa, la máquina virtual tiene el mismo identificador que tenía en el momento de la exportación. Por este motivo, si la máquina virtual ya se está ejecutando en Hyper-V, debe eliminarse antes de que se pueda completar la importación. Una vez completada la importación, los archivos exportados permanecen intactos y se pueden quitar o volver a importar.

- **Copiar la máquina virtual** : es similar al tipo de restauración en el que se selecciona una ubicación para los archivos. La diferencia es que la máquina virtual importada tiene un nuevo identificador único, lo que significa que puede importar la máquina virtual en el mismo host varias veces.

