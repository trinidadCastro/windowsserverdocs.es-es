---
title: Crear archivos de conjunto de VHD de Hyper-V
description: Pasos para crear un archivo VHDset en Hyper-v 2016
author: jiwool
ms.author: jiwool
manager: senthilr
ms.date: 01/26/2017
ms.topic: article
ms.assetid: 444e1496-9e5a-41cf-bfbc-306e2ed8e00a
audience: IT Pros
ms.reviewer: kathydav
ms.openlocfilehash: a2c4b2ff3ca4dda2cb2989c629c5dac5f529cac0
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87991450"
---
# <a name="create-hyper-v-vhd-set-files"></a>Crear archivos de conjunto de VHD de Hyper-V
Los archivos de VHD Set son un nuevo modelo de discos virtuales compartidos para los clústeres invitados en Windows Server 2016. Los archivos de VHD Set admiten el cambio de tamaño en línea de los discos virtuales compartidos, admiten la réplica de Hyper-V y pueden incluirse en los puntos de comprobación coherentes con la aplicación.

Los archivos de VHD Set usan un nuevo tipo de archivo VHD,. VHD. Los archivos de conjunto de VHD almacenan información de puntos de control sobre el disco virtual de grupo que se usa en los clústeres invitados, en forma de metadatos.

Hyper-V controla todos los aspectos de la administración de las cadenas de punto de control y la combinación del conjunto de VHD compartido. El software de administración puede ejecutar operaciones de disco como el cambio de tamaño en línea en archivos VHD set de la misma manera que lo hace para. Archivos VHDX. Esto significa que el software de administración no necesita conocer el formato de archivo del conjunto de VHD.

> [!NOTE]
> Es importante evaluar el impacto de los archivos de VHD set antes de la implementación en producción. Asegúrese de que no haya degradación de rendimiento o funcional en su entorno, como la latencia de disco.

## <a name="create-a-vhd-set-file-from-hyper-v-manager"></a>Creación de un archivo de VHD Set desde el administrador de Hyper-V

1.  Abra el administrador de Hyper-V. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Administrador de Hyper-V**.
2.  En el panel Acción, haga clic en **Nuevo** y después en **Disco duro**.
3.  En la página **Elegir formato de disco** , seleccione **VHD establecido** como el formato del disco duro virtual.
4.  Continúe con las páginas del Asistente para personalizar el disco duro virtual. Puede hacer clic en **Siguiente** para recorrer cada página del asistente o bien en el nombre de una página del panel izquierdo para ir directamente a dicha página.
5.  Cuando termine de configurar el disco duro virtual, haga clic en **Finalizar**.

## <a name="create-a-vhd-set-file-from-windows-powershell"></a>Creación de un archivo de VHD Set desde Windows PowerShell

Use el cmdlet [New-VHD](/powershell/module/hyper-v/new-vhd?view=win10-ps) con el tipo de archivo. VHD en la ruta de acceso del archivo. En este ejemplo se crea un archivo de conjunto de VHD denominado base. vhd con un tamaño de 10 gigabytes.

``` PowerShell
PS c:\>New-VHD -Path c:\base.vhds -SizeBytes 10GB
```

## <a name="migrate-a-shared-vhdx-file-to-a-vhd-set-file"></a>Migrar un archivo VHDX compartido a un archivo de conjunto de VHD

La migración de un VHDX compartido existente a un VHD requiere desconectar la máquina virtual. Este es el proceso recomendado con Windows PowerShell:

1. Quite el VHDX de la máquina virtual. Por ejemplo, ejecute:
   ``` PowerShell
   PS c:\>Remove-VMHardDiskDrive existing.vhdx
   ```

2. Convierta VHDX en VHD. Por ejemplo, ejecute:
   ``` PowerShell
   PS c:\>Convert-VHD existing.vhdx new.vhds
   ```

3. Agregue los VHD a la máquina virtual. Por ejemplo, ejecute:
   ``` PowerShell
   PS c:\>Add-VMHardDiskDrive new.vhds
   ```