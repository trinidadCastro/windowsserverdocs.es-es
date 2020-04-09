---
title: Preparación de la imagen para su distribución
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 681c6cad-7fde-494f-86a5-f4c7c15d23f9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8ef4cb775f6d73f94a47d5d86ca235d459dd4b10
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819908"
---
# <a name="preparing-the-image-for-deployment"></a>Preparación de la imagen para su distribución

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Una herramienta típica para preparar una imagen es sysprep.exe. Ejecutando esta herramienta se generaliza la imagen y se apaga el servidor para que se ejecute la configuración inicial cuando se reinicie el servidor que contiene la imagen. Antes de ejecutar sysprep.exe, debe completar todas las modificaciones que desee realizar en la imagen.  
  
> [!NOTE]
>  Puede restablecer la Activación de productos de Windows un máximo de tres veces mediante sysprep.exe.  
  
#### <a name="to-prepare-the-image"></a>Para preparar la imagen  
  
1.  Elimine el archivo SkipIC.txt que ha añadido.  
  
2.  Abra una ventana del símbolo del sistema con permisos elevados. Haga clic en **Inicio**, haga clic con el botón secundario en **Símbolo del sistema** y, a continuación, seleccione **Ejecutar como administrador**.  
  
3.  Ejecute el comando siguiente para restablecer la clave de registro de forma que el usuario disfrute de un período de gracia completo antes de que el servidor sea incompatible.  
  
    ```  
    %systemroot%\system32\reg.exe add HKLM\Software\Microsoft\ServerInfrastructureLicensing /v Rearm /t REG_DWORD /d 1 /f  
    ```  
  
4.  Ejecute el comando siguiente para añadir la clave del Registro a la clave de visualización, página de idioma, página de configuración regional y página del CLUF. De forma predeterminada, estas páginas no se mostrarán durante la configuración inicial. Por tanto, si está lanzando una caja preinstalada, deberá agregar esta clave del Registro. No obstante, si publica un DVD no debe agregar esta clave, ya que estas páginas se mostrarán durante la ejecución de WinPE y la configuración inicial.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
5.  Deshabilite la página de clave de la configuración inicial si la caja tiene una clave previa. La página de clave solo se mostrará cuando ShowPreinstallPages = true y KeyPreInstalled != true.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v KeyPreInstalled /t REG_SZ /d true /f  
    ```  
  
6.  Ejecute el comando siguiente para agregar la clave del Registro si desea deshabilitar las comprobaciones de los requisitos de hardware. Esta es solo para la caja preinstalada que no cumple los requisitos de hardware. Si está lanzando un DVD, o la caja cumple los requisitos de hardware, se recomienda no agregar esta clave.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
    ```  
  
7.  (Opcional) Elimine los registros de **%programdata%\Microsoft\Windows Server\Logs**.  
  
8.  Prepare el archivo unattended xml para sysprep como se muestra en la siguiente plantilla.  
  
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
  
9. Ejecute el siguiente comando para sysprep.  
  
    ```  
    %systemroot%\system32\sysprep\sysprep.exe /generalize /OOBE /unattend:xxx.xml /Quit  
    ```  
  
    > [!IMPORTANT]
    >  También puede agregar el archivo unattend.xml bajo %systemdrive% en lugar de como un parámetro de sysprep. Si el archivo se encuentra en c:\ se cubrirá según la configuración del usuario, pero si se usa como parámetro de Sysprep, no se cubrirá según la configuración del usuario. El archivo unattend.xml bajo %systemdrive% se eliminará cada vez que se reinicie el servidor. Por lo tanto, asegúrese de que después de crear el archivo unattend.xml bajo %systemdrive%, el servidor no se reinicie.  
  
10. Ejecute el siguiente comando para agregar la clave de registro y así omitir la página clave OOBE de Windows.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedProductKey /t REG_DWORD /d 1 /f  
    ```  
  
11. Ejecute el siguiente comando para agregar la clave de registro y así omitir la página de selección de idiomas de Windows.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedLanguageSelection /t REG_DWORD /d 1 /f  
    ```  
  
    > [!IMPORTANT]
    >  Debe realizar los últimos 2 pasos, o en caso contrario se mostrará la página de OOBE de Windows de costumbre con la página de configuración inicial y se romperá el escenario de servidor administrado de forma remota.  
  
12. Cierre el cuadro después de sysprep; puede capturar una imagen o reiniciar el servidor para continuar la configuración inicial desde un equipo cliente.  
  
> [!IMPORTANT]
>  Los asociados que tengan previsto crear medios de recuperación del servidor deben capturar la imagen y crear los medios de recuperación antes de realizar el paso siguiente.