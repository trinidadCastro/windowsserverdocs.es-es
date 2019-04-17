---
title: Crear una unidad Flash USB de arranque
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fe8e35c-69f9-40b3-a270-22e2402510d8
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9d587329e1141040b2511e1574649f1844dcec90
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-bootable-usb-flash-drive"></a>Crear una unidad Flash USB de arranque

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puedes crear una unidad flash USB de arranque para usar para implementar Windows Server Essentials. El primer paso es preparar la unidad flash USB mediante DiskPart, que es una utilidad de línea de comandos. Para obtener información acerca de DiskPart, consulte [opciones de línea de comandos de DiskPart ](https://go.microsoft.com/fwlink/?LinkId=207073).  
  
 Para escenarios adicionales en el que desea crear o usar un USB de arranque de una unidad flash, consulte los siguientes temas:  
  
-   [Restaurar un sistema completo de una copia de seguridad del equipo cliente existente](https://technet.microsoft.com/library/jj713539.aspx#BKMK_CreateBootable)  
  
-   [Restaurar o reparar el servidor que ejecuta Windows Server Essentials](https://technet.microsoft.com/library/jj593197.aspx#BKMK_Restore_2)  
  
### <a name="to-create-a-bootable-usb-flash-drive"></a>Para crear una unidad flash USB de arranque  
  
1.  Inserta una unidad flash USB en un equipo en ejecución.  
  
2.  Abre una ventana de símbolo del sistema como administrador.  
  
3.  Tipo `diskpart`.  
  
4.  En la nueva ventana de la línea de comandos que se abre, para determinar la flash USB número de unidad o letra de unidad, en el símbolo del sistema, escriba `list disk`y, a continuación, haz clic en ENTRAR. La `list disk`comando muestra todos los discos en el equipo. Ten en cuenta el número de unidad o una letra de unidad de la unidad flash USB.  
  
5.  En el símbolo del sistema, escribe `select disk <X>`, donde X es el número de disco o unidad flash letra de unidad USB y, a continuación, haz clic en ENTRAR.  
  
6.  Tipo `clean`, haga clic en ENTRAR. Este comando elimina todos los datos de la unidad flash USB.  
  
7.  Para crear una nueva partición principal en la unidad flash USB, escriba `create part pri`y, a continuación, haz clic en ENTRAR.  
  
8.  Para seleccionar la partición que acabas de crear, escribir `select part 1`y, a continuación, haz clic en ENTRAR.  
  
9. Para formatear la partición, escriba `format fs=ntfs quick`y, a continuación, haz clic en ENTRAR.  
  
    > [!IMPORTANT]
    >  Si la plataforma de servidor admite Unified Extensible Firmware Interface (UEFI), debes formatear la unidad flash USB como FAT32 en lugar de NTFS. Para formatear la partición como FAT32, escriba `format fs=fat32 quick`y, a continuación, haz clic en ENTRAR.  
  
10. Tipo `active`y, a continuación, haz clic en ENTRAR.  
  
11. Tipo `exit`y, a continuación, haz clic en ENTRAR.  
  
12. Cuando termines de preparar la imagen personalizada, guardarlo en la raíz de la unidad flash USB.  
  
## <a name="see-also"></a>Consulta también  

 [Tareas iniciales con el ADK de Windows Server Essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)   

 [Tareas iniciales con el ADK de Windows Server Essentials](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Crear y personalizar la imagen](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](../install/Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](../install/Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](../install/Testing-the-Customer-Experience.md)   

 [¿Cómo podemos ayudarte?](https://windows.microsoft.com/windows/support)
