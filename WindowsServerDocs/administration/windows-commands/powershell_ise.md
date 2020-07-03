---
title: PowerShell_ise
description: Artículo de referencia para el comando PowerShell_ise, que inicia una sesión de entorno de scripting integrado (ISE) de Windows PowerShell.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 32c41b5b-a210-47d9-bd8c-91eb9830b4f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3f983ea5b8464748d86264108a2ee8660ca0e3f2
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926054"
---
# <a name="powershell_ise"></a>PowerShell_ise

El entorno de scripting integrado (ISE) de Windows PowerShell es una aplicación de host gráfico que permite leer, escribir, ejecutar, depurar y probar scripts y módulos en un entorno asistido por gráficos. Características clave como IntelliSense, show-Command, Snippets, finalización con tabulación, color de sintaxis, depuración visual y ayuda contextual proporcionan una experiencia de scripting enriquecida.

## <a name="using-powershellexe"></a>Usar PowerShell.exe

La herramienta **PowerShell_ISE.exe** inicia una sesión Windows PowerShell ISE. Al usar **PowerShell_ISE.exe**, puede usar sus parámetros opcionales para abrir archivos en Windows PowerShell ISE o para iniciar una sesión de Windows PowerShell ISE sin ningún perfil o con un apartamento multiproceso.

- Para iniciar una sesión de Windows PowerShell ISE en una ventana del símbolo del sistema, en Windows PowerShell, o en el menú **Inicio** , escriba:

  ```powershell
  PowerShell_Ise.exe
  ```

- Para abrir un script (. PS1), un módulo de script (. psm1), un manifiesto de módulo (. psd1), un archivo XML o cualquier otro archivo compatible en Windows PowerShell ISE, escriba:

  ```powershell
  PowerShell_Ise.exe <filepath>
  ```

  En Windows PowerShell 3,0, puede usar el parámetro **archivo** opcional de la siguiente manera:

  ```powershell
  PowerShell_Ise.exe -file <filepath>
  ```

- Para iniciar una sesión de Windows PowerShell ISE sin sus perfiles de Windows PowerShell, use el parámetro **NOPROFILE** . (El parámetro **NOPROFILE** se incluye en Windows PowerShell 3,0). Escriba:

  ```powershell
  PowerShell_Ise.exe -NoProfile
  ```

- Para ver el archivo de ayuda PowerShell_ISE.exe, escriba:

    ```powershell
    PowerShell_Ise.exe -help
    PowerShell_Ise.exe -?
    PowerShell_Ise.exe /?
    ```

### <a name="remarks"></a>Comentarios

- Para obtener una lista completa de los parámetros de línea de comandos de **PowerShell_ISE.exe** , vea [about_PowerShell_Ise.Exe](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_powershell_ise_exe).

- Para obtener información sobre otras maneras de iniciar Windows PowerShell, consulte [iniciar Windows PowerShell](https://docs.microsoft.com/powershell/scripting/windows-powershell/starting-windows-powershell).

- Windows PowerShell se ejecuta en la opción de instalación Server Core de los sistemas operativos Windows Server. Sin embargo, dado que Windows PowerShell ISE requiere una interfaz gráfica de usuario, no se ejecuta en instalaciones Server Core.

## <a name="additional-references"></a>Referencias adicionales

- [about_PowerShell_Ise.exe] (https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_powershell_ise_exe

- [about_PowerShell.Exe](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_powershell_exe)

- [Windows PowerShell](https://docs.microsoft.com/powershell/)
