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
ms.openlocfilehash: 327ac844bec0e4c89ee1443c193aa628de038dea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837408"
---
# <a name="powershell"></a>PowerShell

Windows PowerShell es un shell de línea de comandos basado en tareas y un lenguaje de scripting diseñado especialmente para la administración del sistema. Basado en .NET Framework, Windows PowerShell ayuda a los profesionales de TI y usuarios avanzados a controlar y automatizar la administración del sistema operativo Windows y las aplicaciones que se ejecutan en Windows.

La herramienta de línea de comandos de **PowerShell. exe** inicia una sesión de Windows PowerShell en una ventana del símbolo del sistema. Al usar **PowerShell. exe**, puede usar sus parámetros opcionales para personalizar la sesión. Por ejemplo, puede iniciar una sesión que use una directiva de ejecución determinada o una que excluya un perfil de Windows PowerShell. De lo contrario, la sesión es la misma que cualquier sesión iniciada en la consola de Windows PowerShell.

## <a name="using-powershellexe"></a>Usar PowerShell. exe

Puede usar la herramienta de línea de comandos de **PowerShell. exe** para iniciar una sesión de Windows PowerShell en una ventana del símbolo del sistema.

- Para iniciar una sesión de Windows PowerShell en una ventana del símbolo del sistema, escriba `PowerShell`. Se agrega un prefijo de **PS** al símbolo del sistema para indicar que se encuentra en una sesión de Windows PowerShell.

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

- Para finalizar una sesión de Windows PowerShell en una ventana del símbolo del sistema, escriba `exit`. Se devuelve el símbolo del sistema típico.

Para obtener una lista completa de los parámetros de línea de comandos de **PowerShell. exe** , consulte [about_PowerShell. exe](https://go.microsoft.com/fwlink/?LinkID=113439).

## <a name="other-ways-to-start-windows-powershell"></a>Otras maneras de iniciar Windows PowerShell

Para obtener información sobre otras maneras de iniciar Windows PowerShell, consulte [iniciar Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259).

## <a name="remarks"></a>Comentarios

Windows PowerShell se ejecuta en la opción de instalación Server Core de los sistemas operativos Windows Server. Sin embargo, las características que requieren una interfaz gráfica de usuario, como el [entorno de scripting integrado (ISE) de Windows PowerShell](https://technet.microsoft.com/library/hh849182)y los cmdlets [out-GridView](https://go.microsoft.com/fwlink/?LinkID=113364) y [Show-Command](https://go.microsoft.com/fwlink/?LinkID=217448) , no se ejecutan en instalaciones Server Core.

## <a name="additional-references"></a>Referencias adicionales

[about_PowerShell. exe](https://go.microsoft.com/fwlink/?LinkID=113439)
[about_PowerShell_Ise. exe](https://go.microsoft.com/fwlink/?LinkId=256512)
[scripting de
de](https://technet.microsoft.com/scriptcenter/dd742419) [Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116) con Windows PowerShell, vea también