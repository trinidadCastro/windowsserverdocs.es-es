---
title: Crear archivos de conjunto de disco duro virtual de Hyper-V
description: Pasos para crear un archivo VHDset de Hyper-v de 2016
author: jiwool
ms.author: jiwool
manager: senthilr
ms.date: 01/26/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: compute-hyper-v
ms.assetid: 444e1496-9e5a-41cf-bfbc-306e2ed8e00a
audience: IT Pros
ms.reviewer: kathydav
ms.openlocfilehash: 61f2450857cbeaffd7f75f7b259e9f9de06ba5c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870406"
---
# <a name="create-hyper-v-vhd-set-files"></a>Crear archivos de conjunto de disco duro virtual de Hyper-V
Disco duro virtual archivos son un nuevo modelo de disco Virtual compartido para clústeres de invitados de Windows Server 2016. Archivos VHD establecer admiten el cambio de tamaño en línea de los discos virtuales compartidos, compatibilidad con réplica de Hyper-V y pueden incluirse en los puntos de control coherentes con la aplicación. 

Archivos VHD establecer usan un nuevo tipo de archivo de disco duro virtual. DISCOS DUROS VIRTUALES. Establezca el disco duro virtual archivos almacenan información de punto de control sobre el disco virtual de grupo usado en clústeres de invitados, en forma de metadatos.

Hyper-V controla todos los aspectos de la administración de las cadenas de punto de comprobación y combinar el disco duro virtual compartido conjunto. Software de administración puede ejecutar como en línea el cambio de tamaño en VHD archivos de la misma manera que lo hace para las operaciones de disco. Archivos VHDX. Esto significa que el software de administración no necesita saber acerca del formato de archivo de VHD establecido.

## <a name="create-a-vhd-set-file-from-hyper-v-manager"></a>Cree un archivo VHD establecer desde el Administrador de Hyper-V

1.  Abre el Administrador Hyper-V. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Administrador de Hyper-V**.
2.  En el panel Acción, haga clic en **Nuevo** y después en **Disco duro**.
3.  En el **Elegir formato de disco** página, seleccione **VHD establecer** como el formato del disco duro virtual.
4.  Continúe con las páginas del Asistente para personalizar el disco duro virtual. Puede hacer clic en **Siguiente** para recorrer cada página del asistente o bien en el nombre de una página del panel izquierdo para ir directamente a dicha página.
5.  Cuando termine de configurar el disco duro virtual, haga clic en **Finalizar**.

## <a name="create-a-vhd-set-file-from-windows-powershell"></a>Cree un archivo VHD establecido desde Windows PowerShell

Use la [nuevo VHD](https://technet.microsoft.com/library/hh848503.aspx) cmdlet con el tipo de archivo. Discos duros virtuales en la ruta de acceso de archivo. Este ejemplo crea un archivo de conjunto de disco duro virtual denominado base.vhds de 10 Gigabytes de tamaño.

``` PowerShell
PS c:\>New-VHD -Path c:\base.vhds -SizeBytes 10GB
```

## <a name="migrate-a-shared-vhdx-file-to-a-vhd-set-file"></a>Migrar un archivo VHDX compartido a un archivo VHD establecido

Migrar un VHDX compartido existente a un VHD requiere desconectar la máquina virtual. Este es el proceso recomendado mediante Windows PowerShell:

1.  Quite el VHDX de la máquina virtual. Por ejemplo, ejecute: 
  ``` PowerShell
  PS c:\>Remove-VMHardDiskDrive existing.vhdx
  ```
  
2.  Convertir el VHDX en un VHD. Por ejemplo, ejecute:
  ``` PowerShell
  PS c:\>Convert-VHD existing.vhdx new.vhds
  ```
  
3.  Agregue los discos duros virtuales a la máquina virtual. Por ejemplo, ejecute:
  ``` PowerShell
  PS c:\>Add-VMHardDiskDrive new.vhds
  ```
  



