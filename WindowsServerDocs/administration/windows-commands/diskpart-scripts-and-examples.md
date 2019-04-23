---
title: Ejemplos y Scripts de Diskpart
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 319c0795-11df-47c8-b203-eadb0577ee0d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a8bd0dfab98ca705cf64766d66285c69bd3d3129
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862796"
---
# <a name="diskpart-scripts-and-examples"></a>Ejemplos y Scripts de Diskpart

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Usar Diskpart `/s` para ejecutar los scripts que automatizan disco\-relacionados con tareas, como crear volúmenes o convertir discos en discos dinámicos. Estas tareas de secuencias de comandos son útil si implementa Windows mediante el programa de instalación desatendida o la herramienta Sysprep, que no admiten la creación de otros volúmenes que no sea el volumen de arranque.  
  
-   Para crear un script de Diskpart, cree un archivo de texto que contiene los comandos de Diskpart que se desean ejecutar, con un comando por línea y sin las líneas vacías. Puede iniciar una línea con `rem` para hacer un comentario de la línea.  
  
    Por ejemplo, s aquí un script que borra un disco y, a continuación, crea una partición de 300 MB para el entorno de recuperación de Windows:  
  
    ```  
    select disk 0  
    clean  
    convert gpt  
    create partition primary size=300  
    format quick fs=ntfs label="Windows RE tools"  
    assign letter="T"  
    ```  
  
-   Para ejecutar un script de DiskPart, en el símbolo del sistema, escriba el comando siguiente, donde *scriptname* es el nombre del archivo de texto que contiene el script.  
  
    ```  
    diskpart /s scriptname.txt  
    ```  
  
-   Para redirigir la salida de secuencias de comandos de DiskPart a un archivo, escriba el comando siguiente, donde *logfile* es el nombre del archivo de texto que DiskPart escribe su resultado.  
  
    ```  
    diskpart /s scriptname.txt > logfile.txt  
    ```  
  
> [!IMPORTANT]  
> Cuando se usa el **DiskPart** de comandos como parte de una secuencia de comandos, se recomienda completar todas las operaciones de DiskPart conjuntamente como parte de una sola secuencia de comandos de DiskPart. Puede ejecutar scripts de DiskPart consecutivos, pero debe esperar al menos 15 segundos entre cada script para que complete el cierre de la ejecución anterior antes de ejecutar el **DiskPart** comando nuevo en scripts sucesivos. En caso contrario, se pueden producir un error en los scripts sucesivos. Puede agregar una pausa entre los dos scripts de DiskPart agregando el `timeout /t 15` comando a su archivo por lotes junto con las secuencias de comandos de DiskPart.  
  
Cuando se inicia DiskPart, la presentación de nombre de versión y equipo de DiskPart en el símbolo del sistema. De forma predeterminada, si DiskPart detecta un error al intentar realizar una tarea con secuencias de comandos, DiskPart detiene el procesamiento de la secuencia de comandos y muestra un código de error \(a menos que se especificó el **noerr** parámetro\). Sin embargo, DiskPart siempre devuelve los errores cuando encuentra errores de sintaxis, independientemente de si utiliza el **noerr** parámetro. El **noerr** parámetro le permite realizar tareas útiles como el uso de una sola secuencia de comandos para eliminar todas las particiones de todos los discos independientemente del número total de discos.  
  
## <a name="see-also"></a>Vea también  
[Ejemplo: Configurar UEFI\/gpt\-basada disco duro las particiones de disco utilizar Windows PE y DiskPart](https://technet.microsoft.com/library/hh825686.aspx)  
[Ejemplo: Configurar BIOS\/MBR\-en función de las particiones de disco duro mediante el uso de Windows PE y DiskPart](https://technet.microsoft.com/library/hh825677.aspx)  
[Cmdlets de almacenamiento en Windows PowerShell](https://technet.microsoft.com/library/hh848705.aspx)  
  

