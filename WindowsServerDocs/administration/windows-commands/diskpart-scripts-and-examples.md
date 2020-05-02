---
title: Ejemplos y scripts de Diskpart
description: Tema de referencia sobre los scripts de Diskpart y ejemplos sobre cómo automatizar tareas relacionadas con disco, como crear volúmenes o convertir discos en discos dinámicos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 319c0795-11df-47c8-b203-eadb0577ee0d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 74cefe87fbbeaa1f2bacccbe4addeff952b2c432
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719474"
---
# <a name="diskpart-scripts-and-examples"></a>Ejemplos y scripts de Diskpart

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Use DiskPart `/s` para ejecutar scripts que automaticen tareas relacionadas con el disco, como la creación de volúmenes o la conversión de discos en discos dinámicos. La creación de scripts para realizar estas tareas es útil si implementa Windows mediante instalación desatendida o la herramienta Sysprep, que no son compatibles con la creación de otros volúmenes que no sean el volumen de arranque.  
  
-   Para crear un script de DiskPart, cree un archivo de texto que contenga los comandos de Diskpart que desea ejecutar, con un comando por línea y sin líneas vacías. Puede iniciar una línea con `rem` para convertir la línea en un comentario.  
  
    por ejemplo, aquí es un script que borra un disco y, a continuación, crea una partición de 300 MB para el entorno de recuperación de Windows:  
  
    ```  
    select disk 0  
    clean  
    convert gpt  
    create partition primary size=300  
    format quick fs=ntfs label=Windows RE tools  
    assign letter=T  
    ```  
  
-   Para ejecutar un script de DiskPart, en el símbolo del sistema, escriba el siguiente comando, donde *scriptName* es el nombre del archivo de texto que contiene el script.  
  
    ```  
    diskpart /s scriptname.txt  
    ```  
  
-   Para redirigir la salida de scripts de Diskpart a un archivo, escriba el siguiente comando, donde *logfile* es el nombre del archivo de texto donde DiskPart escribe su salida.  
  
    ```  
    diskpart /s scriptname.txt > logfile.txt  
    ```  
  
> [!IMPORTANT]  
> Cuando se usa el comando **DiskPart** como parte de un script, se recomienda completar todas las operaciones de Diskpart juntas como parte de un único script de Diskpart. Puede ejecutar scripts de Diskpart consecutivos, pero debe permitir al menos 15 segundos entre cada script para un cierre completo de la ejecución anterior antes de volver a ejecutar el comando **DiskPart** en scripts sucesivos. De lo contrario, puede que los scripts sucesivos no funcionen. Puede Agregar una pausa entre scripts de Diskpart consecutivos agregando el `timeout /t 15` comando al archivo por lotes junto con los scripts de Diskpart.  
  
Cuando se inicia DiskPart, la versión de DiskPart y el nombre del equipo se muestran en el símbolo del sistema. De forma predeterminada, si DiskPart detecta un error al intentar realizar una tarea con scripts, DiskPart detiene el procesamiento del script y muestra un código \(de error a menos que se especifique el parámetro **noerr** \)Noerr. Sin embargo, DiskPart siempre devuelve errores cuando encuentra errores de sintaxis, independientemente de si ha usado el parámetro **Noerr** . El parámetro **Noerr** permite realizar tareas útiles como el uso de un solo script para eliminar todas las particiones de todos los discos, independientemente del número total de discos.  
  
## <a name="additional-references"></a>Referencias adicionales
  
- [Ejemplo: configurar particiones de disco duro basadas en UEFI\/GPT\-con Windows PE y DiskPart](https://technet.microsoft.com/library/hh825686.aspx)  
- [Ejemplo: configurar particiones\-de disco duro basadas en MBR de BIOS\/mediante Windows PE y DiskPart](https://technet.microsoft.com/library/hh825677.aspx)  
- [Cmdlets de almacenamiento en Windows PowerShell](https://technet.microsoft.com/library/hh848705.aspx)  
  

