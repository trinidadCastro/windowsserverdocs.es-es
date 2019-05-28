---
title: Crear una unidad flash USB de arranque
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 05/04/2018
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fe8e35c-69f9-40b3-a270-22e2402510d8
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cb62a460c09fdb2874bcc051176a05e88cee19e7
ms.sourcegitcommit: 7cb939320fa2613b7582163a19727d7b77debe4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2019
ms.locfileid: "65621283"
---
# <a name="create-a-bootable-usb-flash-drive"></a>Crear una unidad flash USB de arranque

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puede crear una unidad flash USB de arranque a usar para implementar Windows Server Essentials. El primer paso es preparar la unidad flash USB con DiskPart, una utilidad de línea de comandos. Para obtener información sobre DiskPart, consulte [Opciones de línea de comandos de DiskPart](https://go.microsoft.com/fwlink/?LinkId=207073).  


> [!TIP]
> Para crear una unidad flash USB de arranque para su uso en la recuperación o volver a instalar Windows en un equipo en lugar de un servidor, consulte [crear una unidad de recuperación](https://support.microsoft.com/help/4026852/windows-create-a-recovery-drive).
  
 Para otros escenarios en los que quiera crear o usar una unidad flash USB de arranque, vea los temas siguientes:  
  
-   [Restauración completa del sistema desde una copia de seguridad existente del equipo cliente](../manage/restore-a-full-system-from-an-existing-client-computer-backup.md)  
  
-   [Restaurar o reparar el servidor que ejecuta Windows Server Essentials](../manage/restore-or-repair-your-server-running-windows-server-essentials.md)  

  
### <a name="to-create-a-bootable-usb-flash-drive"></a>Para crear una unidad flash USB de arranque  
  
1.  Inserte una unidad flash USB en un equipo en ejecución.  
  
2.  Abra una ventana del símbolo del sistema como administrador.  
  
3.  Escriba `diskpart`.  
  
4.  Para determinar el número de la unidad flash USB o la letra de la unidad, escriba `list disk` en el símbolo del sistema de la nueva ventana de línea de comandos que se abre y, a continuación, haga clic en ENTRAR. El comando `list disk` muestra todos los discos del equipo. Tome nota del número o la letra de la unidad flash USB.  
  
5.  En el símbolo del sistema, escriba `select disk <X>` (donde X es el número o la letra de la unidad flash USB) y haga clic en ENTRAR.  
  
6.  Escriba `clean`y haga clic en ENTRAR. Este comando elimina todos los datos de la unidad flash USB.  
  
7.  Para crear una nueva partición principal en la unidad flash USB, escriba `create partition primary` y haga clic en ENTRAR.  
  
8.  Para seleccionar la partición que acaba de crear, escriba `select partition 1`y haga clic en ENTRAR.  
  
9. Para dar formato a la partición, escriba `format fs=ntfs quick`y haga clic en ENTRAR.  
  
    > [!IMPORTANT]
    >  Si su plataforma de servidor es compatible con Unified Extensible Firmware Interface (UEFI), debe dar formato a la unidad flash USB como FAT32, en lugar de NTFS. Para dar formato a la partición como FAT32, escriba `format fs=fat32 quick`y haga clic en ENTRAR.  
  
10. Escriba `active`y haga clic en ENTRAR.  
  
11. Escriba `exit`y haga clic en ENTRAR.  
  
12. Cuando haya terminado de preparar la imagen personalizada, guárdela en la raíz de la unidad flash USB.  
  
## <a name="see-also"></a>Vea también  

 [Introducción al ADK de Windows Server Essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparar la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](Testing-the-Customer-Experience.md)   

 [Introducción al ADK de Windows Server Essentials](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Crear y personalizar la imagen](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](../install/Additional-Customizations.md)   
 [Preparar la imagen para la implementación](../install/Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](../install/Testing-the-Customer-Experience.md)   

 [¿Cómo podemos ayudarle?](https://windows.microsoft.com/windows/support)
