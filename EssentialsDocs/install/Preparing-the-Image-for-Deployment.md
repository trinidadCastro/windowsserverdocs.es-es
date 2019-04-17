---
title: "Preparación de la imagen para la implementación"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 681c6cad-7fde-494f-86a5-f4c7c15d23f9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 16411ab073e9417c52592aa9a6b13707dd461537
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="preparing-the-image-for-deployment"></a>Preparación de la imagen para la implementación

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Una herramienta para preparar una imagen típica es sysprep.exe. Ejecutar esta herramienta generaliza la imagen y cierra el servidor para que se ejecutará la configuración inicial cuando se reinicie el servidor que contiene la imagen. Todas las modificaciones a la imagen deben completas antes de ejecutar sysprep.exe.  
  
> [!NOTE]
>  Puedes restablecer la activación de producto de Windows un máximo de tres veces usando sysprep.exe.  
  
#### <a name="to-prepare-the-image"></a>Para preparar la imagen  
  
1.  ¿Eliminar SkipIC.txt que has agregado?  
  
2.  Abre una ventana de símbolo del sistema con privilegios elevados. Haz clic en **inicio**, haga clic **símbolo**y, a continuación, selecciona **ejecutar como administrador**.  
  
3.  Ejecuta el siguiente comando para restablecer la clave del registro para que el usuario tendrá el período de gracia completo antes de que el servidor se vuelva no compatible.  
  
    ```  
    %systemroot%\system32\reg.exe add HKLM\Software\Microsoft\ServerInfrastructureLicensing /v Rearm /t REG_DWORD /d 1 /f  
    ```  
  
4.  Ejecuta el siguiente comando para agregar la clave del registro para mostrar la clave, la página de idioma, la página de configuración regional y la página de CLUF. De manera predeterminada, estas páginas no se mostrarán durante la configuración inicial. Por lo tanto, si estás lanzando un cuadro preinstalado, deberás agregar esta clave del registro. Sin embargo, si estás lanzando un DVD, no debes agregar esta clave, como estas páginas mostrarán durante WinPE y la configuración inicial.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
5.  Deshabilitar la página de la clave de la configuración inicial, si el cuadro es previamente con clave. ¡Solo mostrará la página de la clave cuando ShowPreinstallPages = true y KeyPreInstalled! = true.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v KeyPreInstalled /t REG_SZ /d true /f  
    ```  
  
6.  Ejecuta el siguiente comando para agregar la clave del registro si quieres deshabilitar las comprobaciones de requisitos de hardware. Esto es solo para el cuadro preinstalado que no cumpla los requisitos de hardware. Si estás lanzando un DVD, o bien, el cuadro cumple los requisitos de hardware, se recomienda no agregues esta clave.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
    ```  
  
7.  (Opcional) Quitar los registros en **%programdata%\Microsoft\Windows Server\Logs**.  
  
8.  Preparar el archivo xml de instalación desatendida para sysprep tal como se muestra en la siguiente plantilla.  
  
    ```  
    <unattend xmlns="urn:schemas-microsoft-com:unattend" xmlns:ms="urn:schemas-microsoft-com:asm.v3">  
  
      <settings pass="oobeSystem">  
        <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="https://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  
          <!-- Must have -->  
          <OOBE>  
             <HideEULAPage>true</HideEULAPage>  
          </OOBE>  
          <!-- Must have -->  
          <AutoLogon>   
            <Enabled>true</Enabled>   
            <Username>Administrator</Username>   
            <Domain>.</Domain>   
            <Password>   
              <!--You can set any password you like, but keep it consistent with password settings -->       
              <Value>Admin@123</Value>   
              <PlainText>true</PlainText>   
            </Password>   
          </AutoLogon>   
          <UserAccounts>   
           <AdministratorPassword>   
             <!--You can set any password you like, but keep it consistent with auto logon settings -->       
             <Value>Admin@123</Value>   
             <PlainText>true</PlainText>   
           </AdministratorPassword>   
          </UserAccounts>  
  
          <!-- Optional -->  
          <OEMInformation>  
             <HelpCustomized>true</HelpCustomized>  
             <Manufacturer>OEM name</Manufacturer>  
             <Model>model name</Model>  
             <SupportHours>hours</SupportHours>  
             <SupportPhone>123-456-7890</SupportPhone>  
             <SupportURL>http://www.contoso.com</SupportURL>  
          </OEMInformation>  
  
        </component>  
  
      </settings>  
  
      <settings pass="specialize">  
        <component name="Microsoft-Windows-Shell-Setup" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" processorArchitecture="amd64">  
          <!-- Must have -->  
          <ComputerName>Server</ComputerName>          
          <!-- Optional -->  
          <ProductKey>XXXXX-XXXXX-XXXXX-XXXXX-XXXXX</ProductKey>  
        </component>  
      </settings>  
    </unattend>  
    ```  
  
9. Ejecuta el siguiente comando para sysprep.  
  
    ```  
    %systemroot%\system32\sysprep\sysprep.exe /generalize /OOBE /unattend:xxx.xml /Quit  
    ```  
  
    > [!IMPORTANT]
    >  También puedes agregar unattend.xml en % systemdrive % en lugar de como un parámetro de sysprep. Si se encuentra el archivo en c:\ que lo hará cubiertos por la configuración de s de usuario, pero si se usa como un parámetro de sysprep, no se pueden describir por la configuración de s de usuario. Cada vez que se reinicie el servidor se eliminarán unattend.xml en % systemdrive %. Por lo tanto, asegúrate de que después de crear unattend.xml en % systemdrive %, no se reinicie el servidor.  
  
10. Ejecuta el siguiente comando para agregar la clave del registro para omitir la página de la clave de Windows OOBE.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedProductKey /t REG_DWORD /d 1 /f  
    ```  
  
11. Ejecuta el siguiente comando para agregar la clave del registro para omitir la página de selección de idioma de Windows.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedLanguageSelection /t REG_DWORD /d 1 /f  
    ```  
  
    > [!IMPORTANT]
    >  Debes realizar los 2 últimos pasos o, de lo contrario, la página de configuración rápida de Windows se logras que vence con el escenario de servidor de administración remota de página y salto de la configuración inicial.  
  
12. Cierra el cuadro después de sysprep, puedes capturar una imagen o reiniciar el servidor para continuar con la configuración inicial desde un equipo cliente.  
  
> [!IMPORTANT]
>  Los asociados que vas a crear medios de recuperación del servidor deben capturar la imagen y crear los medios de recuperación antes de continuar con el siguiente paso.