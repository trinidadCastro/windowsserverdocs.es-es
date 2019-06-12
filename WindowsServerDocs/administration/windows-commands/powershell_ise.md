---
title: PowerShell_ise
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 32c41b5b-a210-47d9-bd8c-91eb9830b4f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a5619396e29b446dbc6804ece7444f355dae4c0a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436303"
---
# <a name="powershellise"></a>PowerShell_ise



Windows PowerShell Integrated Scripting Environment (ISE) es una aplicación host gráfica que permite leer, escribir, ejecutar, depurar y probar scripts y módulos en un entorno asistido por gráfico. Características como IntelliSense, clave Show-Command, fragmentos de código, la finalización con tabulación, depuración visual y del color de sintaxis y proporcionan una rica experiencia de scripting de ayuda contextual.

El **PowerShell_ISE.exe** herramienta inicia una sesión de Windows PowerShell ISE. Cuando usas **PowerShell_ISE.exe**, puede usar los parámetros opcionales para abrir archivos en Windows PowerShell ISE o para iniciar una sesión de Windows PowerShell ISE con ningún perfil o con un apartamento multiproceso.

**PowerShell_ISE.exe** se introdujo en Windows PowerShell 2.0 y ampliado considerablemente en Windows PowerShell 3.0.

## <a name="using-powershelliseexe"></a>Uso de PowerShell_ISE.exe

Puede usar **PowerShell_ISE.exe** para iniciar y finalizar una sesión de Windows PowerShell como sigue:
- Para iniciar una sesión de Windows PowerShell ISE, en una ventana de símbolo del sistema, en Windows PowerShell, o en el menú Inicio, escriba:  
  ```
  PowerShell_Ise
  ```  
- Para abrir un script (. ps1), el módulo de script (. psm1), el manifiesto de módulo (. psd1), archivo XML o cualquier otro tipo de archivo admitido en Windows PowerShell ISE, use el siguiente formato de comando:  
  ```
  PowerShell_Ise <FilePath>
  ```  
  En Windows PowerShell 3.0, puede usar el elemento opcional **archivo** parámetro como sigue:  
  ```
  PowerShell_Ise -File <FilePath>
  ```  
- Para iniciar una sesión de Windows PowerShell ISE sin los perfiles de Windows PowerShell, use el **NoProfile** parámetro. (El **NoProfile** parámetro se incorporó en Windows PowerShell 3.0.)  
  ```
  PowerShell_Ise -NoProfile
  ```  
- Para ver el **PowerShell_ISE.exe** ayuda en una ventana del símbolo del sistema de archivos, use el siguiente formato de comando:  
  ```
  PowerShell_Ise -help, -?, /?
  ```  
  Para obtener una lista completa de la **PowerShell_ISE.exe** parámetros de línea de comandos, vea [about_PowerShell_Ise.exe](https://go.microsoft.com/fwlink/?LinkId=256512).

## <a name="start-windows-powershell-ise-in-other-ways"></a>Inicie Windows PowerShell ISE de otras maneras

Para obtener información sobre otras maneras de iniciar Windows PowerShell ISE, consulte [iniciar Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259).

## <a name="remarks"></a>Comentarios

Windows PowerShell se ejecuta en la opción de instalación Server Core de sistemas operativos Windows Server. Sin embargo, dado que Windows PowerShell ISE requiere una interfaz gráfica de usuario, no funciona en las instalaciones Server Core.

## <a name="additional-references"></a>Referencias adicionales

[about_PowerShell_Ise.exe](https://go.microsoft.com/fwlink/?LinkId=256512)
[about_PowerShell.exe](https://go.microsoft.com/fwlink/?LinkID=113439)
[Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116)
[Scripting con Windows PowerShell](https://technet.microsoft.com/scriptcenter/dd742419) Vea también