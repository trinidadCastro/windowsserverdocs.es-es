---
title: PowerShell
description: Artículo de referencia para el comando de PowerShell, que abre la consola de PowerShell desde un símbolo del sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 694fc970-0b6c-4046-b1b5-7eb1a0d26609
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 8a252efe57cec1e77bd4d814ced75decb1f2ceb7
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931374"
---
# <a name="powershell"></a>PowerShell

Windows PowerShell es un shell de línea de comandos basado en tareas y un lenguaje de scripting diseñado especialmente para la administración del sistema. Basado en .NET Framework, Windows PowerShell ayuda a los profesionales de TI y a los usuarios avanzados a controlar y automatizar la administración tanto del sistema operativo Windows como de las aplicaciones que se ejecutan en Windows.

## <a name="using-powershellexe"></a>Usar PowerShell.exe

La herramienta de línea de comandos **PowerShell.exe** inicia una sesión de Windows PowerShell en una ventana del símbolo del sistema. Al usar **PowerShell.exe**, puede usar sus parámetros opcionales para personalizar la sesión. Por ejemplo, puede iniciar una sesión que use una directiva de ejecución determinada o una que excluya un perfil de Windows PowerShell. De lo contrario, la sesión es la misma que cualquier sesión iniciada en la consola de Windows PowerShell.

- Para iniciar una sesión de Windows PowerShell en una ventana del símbolo del sistema, escriba `PowerShell` . Se agrega un prefijo de **PS** al símbolo del sistema para indicar que se encuentra en una sesión de Windows PowerShell.

- Para iniciar una sesión con una directiva de ejecución determinada, use el parámetro **ExecutionPolicy** y escriba:

    ```powershell
    PowerShell.exe -ExecutionPolicy Restricted
    ```

- Para iniciar una sesión de Windows PowerShell sin sus perfiles de Windows PowerShell, use el parámetro **NOPROFILE** y escriba:

    ```powershell
    PowerShell.exe -NoProfile
    ```

- Para iniciar una sesión, use el parámetro **ExecutionPolicy** y escriba:

    ```powershell
    PowerShell.exe -ExecutionPolicy Restricted
    ```

- Para ver el archivo de ayuda PowerShell.exe, escriba:

    ```powershell
    PowerShell.exe -help
    PowerShell.exe -?
    PowerShell.exe /?
    ```

- Para finalizar una sesión de Windows PowerShell en una ventana del símbolo del sistema, escriba `exit` . Se devuelve el símbolo del sistema típico.

### <a name="remarks"></a>Comentarios

- Para obtener una lista completa de los parámetros de línea de comandos de **PowerShell.exe** , vea [about_PowerShell.Exe](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_powershell_exe).

- Para obtener información sobre otras maneras de iniciar Windows PowerShell, consulte [iniciar Windows PowerShell](https://docs.microsoft.com/powershell/scripting/windows-powershell/starting-windows-powershell).

- Windows PowerShell se ejecuta en la opción de instalación Server Core de los sistemas operativos Windows Server. Sin embargo, las características que requieren una interfaz gráfica de usuario, como el [entorno de scripting integrado (ISE) de Windows PowerShell](https://docs.microsoft.com/previous-versions//hh849182(v=technet.10))y los cmdlets [out-GridView](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/out-gridview) y [Show-Command](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Utility/Show-Command) , no se ejecutan en instalaciones Server Core.

## <a name="additional-references"></a>Referencias adicionales

- [about_PowerShell.Exe](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_powershell_exe)

- [about_PowerShell_Ise.exe](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_powershell_ise_exe)

- [Windows PowerShell](https://docs.microsoft.com/powershell/)
