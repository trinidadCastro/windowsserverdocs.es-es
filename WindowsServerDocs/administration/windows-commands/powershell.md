---
title: PowerShell
description: Obtenga información sobre cómo abrir la consola de PowerShell desde un símbolo del sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 694fc970-0b6c-4046-b1b5-7eb1a0d26609
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: b733a187017293e8a33ff307b485380ef8f9b472
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436570"
---
# <a name="powershell"></a>PowerShell

Windows PowerShell es un shell de línea de comandos basado en tareas y un lenguaje de scripting diseñado especialmente para la administración del sistema. Basado en .NET Framework, Windows PowerShell ayuda a los profesionales de TI y a los usuarios avanzados a controlar y automatizar la administración tanto del sistema operativo Windows como de las aplicaciones que se ejecutan en Windows.

La herramienta de línea de comandos de **PowerShell. exe** inicia una sesión de Windows PowerShell en una ventana del símbolo del sistema. Al usar **PowerShell. exe**, puede usar sus parámetros opcionales para personalizar la sesión. Por ejemplo, puede iniciar una sesión que use una directiva de ejecución determinada o una que excluya un perfil de Windows PowerShell. De lo contrario, la sesión es la misma que cualquier sesión iniciada en la consola de Windows PowerShell.

## <a name="using-powershellexe"></a>Usar PowerShell. exe

Puede usar la herramienta de línea de comandos de **PowerShell. exe** para iniciar una sesión de Windows PowerShell en una ventana del símbolo del sistema.

- Para iniciar una sesión de Windows PowerShell en una ventana del símbolo del sistema, escriba `PowerShell` . Se agrega un prefijo de **PS** al símbolo del sistema para indicar que se encuentra en una sesión de Windows PowerShell.

- Para iniciar una sesión con una directiva de ejecución determinada, use el parámetro **ExecutionPolicy** .

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```

- Para iniciar una sesión de Windows PowerShell sin sus perfiles de Windows PowerShell, use el parámetro **NOPROFILE** .

    ```
    PowerShell.exe -NoProfile
    ```

- Para iniciar una sesión, use el parámetro **ExecutionPolicy** .

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```

- Para ver el archivo de ayuda de PowerShell. exe, use el siguiente formato de comando.

    ```
    PowerShell.exe -help, -?, /?
    ```

- Para finalizar una sesión de Windows PowerShell en una ventana del símbolo del sistema, escriba `exit` . Se devuelve el símbolo del sistema típico.

Para obtener una lista completa de los parámetros de línea de comandos de **PowerShell. exe** , consulte [about_PowerShell. exe](https://go.microsoft.com/fwlink/?LinkID=113439).

## <a name="other-ways-to-start-windows-powershell"></a>Otras maneras de iniciar Windows PowerShell

Para obtener información sobre otras maneras de iniciar Windows PowerShell, consulte [iniciar Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259).

## <a name="remarks"></a>Observaciones

Windows PowerShell se ejecuta en la opción de instalación Server Core de los sistemas operativos Windows Server. Sin embargo, las características que requieren una interfaz gráfica de usuario, como el [entorno de scripting integrado (ISE) de Windows PowerShell](https://technet.microsoft.com/library/hh849182)y los cmdlets [out-GridView](https://go.microsoft.com/fwlink/?LinkID=113364) y [Show-Command](https://go.microsoft.com/fwlink/?LinkID=217448) , no se ejecutan en instalaciones Server Core.

## <a name="additional-references"></a>Referencias adicionales

[about_PowerShell. exe](https://go.microsoft.com/fwlink/?LinkID=113439) 
 [about_PowerShell_Ise. exe](https://go.microsoft.com/fwlink/?LinkId=256512) 
 [Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116) 
 [Scripting con Windows PowerShell](https://technet.microsoft.com/scriptcenter/dd742419) Vea también