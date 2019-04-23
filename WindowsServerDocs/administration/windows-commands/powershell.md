---
title: PowerShell
description: Obtenga información sobre cómo abrir la consola de PowerShell desde un símbolo del sistema.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 694fc970-0b6c-4046-b1b5-7eb1a0d26609
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: c8070268fdf58fbbb71c159a7360b488222ef740
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852186"
---
# <a name="powershell"></a>PowerShell

Windows PowerShell es un shell de línea de comandos basado en tareas y lenguaje de scripting diseñado especialmente para la administración del sistema. Basado en .NET Framework, Windows PowerShell ayuda a los profesionales de TI y a los usuarios avanzados a controlar y automatizar la administración tanto del sistema operativo Windows como de las aplicaciones que se ejecutan en Windows.

El **PowerShell.exe** herramienta de línea de comandos inicia una sesión de Windows PowerShell en una ventana del símbolo del sistema. Cuando usas **PowerShell.exe**, puede usar los parámetros opcionales para personalizar la sesión. Por ejemplo, puede iniciar una sesión que usa una directiva de ejecución concreta o uno que excluya un perfil de Windows PowerShell. En caso contrario, la sesión es igual que cualquier sesión que se inicia en la consola de Windows PowerShell.

## <a name="using-powershellexe"></a>Uso de PowerShell.exe

Puede usar el **PowerShell.exe** herramienta de línea de comandos para iniciar una sesión de Windows PowerShell en una ventana del símbolo del sistema.

- Para iniciar una sesión de Windows PowerShell en una ventana del símbolo del sistema, escriba `PowerShell`. Un **PS** prefijo se agrega a la línea de comandos para indicar que se encuentra en una sesión de Windows PowerShell.
- Para iniciar una sesión con una directiva de ejecución determinada, use la **ExecutionPolicy** parámetro.  
    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```  
- Para iniciar una sesión de Windows PowerShell sin los perfiles de Windows PowerShell, use el **NoProfile** parámetro.  
    ```
    PowerShell.exe -NoProfile
    ```  
- Para iniciar una sesión, use la **ExecutionPolicy** parámetro.  
    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```  
- Para ver el PowerShell.exe archivo de ayuda, use el siguiente formato de comando.  
    ```
    PowerShell.exe -help, -?, /?
    ```  
- Para finalizar una sesión de Windows PowerShell en una ventana del símbolo del sistema, escriba `exit`. Devuelve el símbolo del sistema típico.

Para obtener una lista completa de la **PowerShell.exe** parámetros de línea de comandos, vea [about_PowerShell.Exe](https://go.microsoft.com/fwlink/?LinkID=113439).

## <a name="other-ways-to-start-windows-powershell"></a>Otras formas de iniciar Windows PowerShell

Para obtener información sobre otras maneras de iniciar Windows PowerShell, consulte [iniciar Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259).

## <a name="remarks"></a>Comentarios

Windows PowerShell se ejecuta en la opción de instalación Server Core de sistemas operativos Windows Server. Sin embargo, las características que requieren una gráfica de usuario de la interfaz, como el [Windows PowerShell Integrated Scripting Environment (ISE)](https://technet.microsoft.com/library/hh849182)y el [Out-GridView](https://go.microsoft.com/fwlink/?LinkID=113364) y [Show-Command](https://go.microsoft.com/fwlink/?LinkID=217448)cmdlets, no se ejecutan en instalaciones Server Core.

## <a name="additional-references"></a>Referencias adicionales

[about_PowerShell.exe](https://go.microsoft.com/fwlink/?LinkID=113439)
[about_PowerShell_Ise.exe](https://go.microsoft.com/fwlink/?LinkId=256512)
[Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116)
[Scripting con Windows PowerShell](https://technet.microsoft.com/scriptcenter/dd742419) Vea también