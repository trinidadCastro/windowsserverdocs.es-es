---
title: Personalizar carpetas compartidas
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 47bc4986-14eb-4a29-9930-83a25704a3a0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 861107035408fc39d0dc5e4d94a4d82d8dfba74e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818078"
---
# <a name="customize-shared-folders"></a>Personalizar carpetas compartidas

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

De forma predeterminada, las carpetas del servidor se crean en la última partición de datos del Disco 0. Los asociados pueden personalizar la ubicación y especificar carpetas de servidor adicionales utilizando los pasos siguientes:  
  
1. Utilice una configuración de partición personalizada para crear la imagen de fábrica y, a continuación, la nueva clave del Registro Storage antes de usar sysprep. Durante la configuración inicial, la tarea de configuración inicial del almacenamiento comprueba esta clave del Registro. Si existe, las carpetas de servidor predeterminadas se crearán en el directorio C:\ServerFolders.  
  
   #### <a name="to-create-a-new-storage-registry-key"></a>Para crear una nueva clave del Registro Storage  
  
   1.  En el servidor, mueva el ratón a la esquina superior derecha de la pantalla y haga clic en **Buscar**.  
  
   2.  En el cuadro de búsqueda, escriba **regedit**y después haga clic en la aplicación **Regedit** .  
  
   3.  En el panel de navegación, expanda **HKEY_LOCAL_MACHINE**, **SOFTWARE** y finalmente **Microsoft**.  
  
   4.  Haga clic con el botón secundario en **Windows Server**, en **Nuevo** y, a continuación, en **Clave**.  
  
   5.  Use **Storage** como el nombre de la clave.  
  
   6.  En el panel de navegación, haga clic con el botón secundario en la clave del Registro Storage, en **Nueva** y, a continuación, en **Valor de DWORD (32 bits)** .  
  
   7.  Use el nombre **CreateFoldersOnSystem** para la cadena.  
  
   8.  Haga clic con el botón secundario en **CreateFoldersOnSystem** y, a continuación, en **Modificar**. Se abrirá el cuadro de diálogo **Editar cadena**.  
  
   9. Escriba **1** como el valor de la nueva clave y, a continuación, haga clic en **Aceptar**.  
  
2. Utilice el script PostIC.cmd para mover las carpetas a otra ubicación o para crear carpetas adicionales. Vea el ejemplo siguiente: [Ejemplo 1: Crear una carpeta personalizada y mover las carpetas personalizadas a una nueva ubicación de PostIC.cmd mediante Windows PowerShell](Customize-Shared-Folders.md#BKMK_Example1).  
  
3. Utilice el SDK de Soluciones de Windows Server para mover las carpetas a otra ubicación o para crear carpetas adicionales. Vea el ejemplo siguiente: [Ejemplo 2: Crear una carpeta personalizada y mover una carpeta existente mediante el SDK de Soluciones de Windows Server](Customize-Shared-Folders.md#BKMK_Example2).  
  
   Los asociados tienen la opción de mantener las carpetas de datos en la unidad C. Esto permite que el usuario final o el revendedor determine el diseño de las carpetas de datos en las unidades de datos.  
  
###  <a name="example-1-create-a-custom-folder-and-move-the-default-folders-to-a-new-location-from-posticcmd-by-using-windows-powershell"></a><a name="BKMK_Example1"></a>Ejemplo 1: crear una carpeta personalizada y trasladar las carpetas predeterminadas a una nueva ubicación desde Postic. cmd mediante Windows PowerShell  
  
1.  Cree un archivo PostIC.cmd para ejecutar tareas de configuración inicial, tal y como se describe en la sección [Cómo crear el archivo PostIC.cmd para ejecutar tareas posteriores a la configuración inicial](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md).  
  
2.  Con el Bloc de notas, cree un archivo denominado **customizefolders. PS1** en la carpeta C:\Windows\Setup\Scripts y, a continuación, pegue los siguientes comandos de Windows PowerShell&reg; en el archivo (Desmarque las líneas correspondientes en función del comportamiento deseado).  
  
    ```  
    # Move the Documents folder to D:\ServerFolders  
    #Get-WssFolder -name Documents| Move-WssFolder - NewDrive D:\ -Force  
  
    # Check for last error. If last error is not null, exit with return code 1  
    #If ($error[0] -ne $null) { exit 1}   
  
    # Move all folders to D:\ServerFolders  
    #foreach( $folder in Get-WssFolder )  
    #{  
    #    Move-WssFolder $folder -NewDrive D:\ -Force  
    #}  
  
    # Check for last error. If last error is not null, exit with return code 1  
    #If ($error[0] -ne $null) { exit 1}   
  
    # Create a custom folder named Custom Folder  
    #Add-WssFolder -Name CustomFolder -Path D:\ServerFolders\CustomFolder -Description "Custom Folder"  
  
    # Check for last error. If last error is not null, exit with return code 1  
    #If ($error[0] -ne $null) { exit 1}   
  
    exit 0  
    ```  
  
3.  Agregue la siguiente línea al archivo PostlC.cmd para ejecutar este script.  
  
    ```  
    REM Lower the execution policy  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy RemoteSigned"  
  
    REM Execute the folder customization script  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" -NoProfile -Noninteractive -command ". %windir%\setup\scripts\customizefolders.ps1;exit $LASTEXITCODE"  
    Set error_level=%ERRORLEVEL%  
  
    REM Restore the execution policy to default  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy Restricted"  
    Set ERRORLEVEL=%error_level%  
    ```  
  
###  <a name="example-2-create-a-custom-folder-and-move-an-existing-folder-by-using-the-windows-server-solutions-sdk"></a><a name="BKMK_Example2"></a>Ejemplo 2: crear una carpeta personalizada y desplace una carpeta existente mediante el SDK de soluciones de Windows Server  
 El código que cree puede ser considerado como un ejecutable y posteriormente llamado desde el archivo PostIC.cmd o directamente desde un complemento instalado.  
  
```  
static void Main(string[] args)  
{  
 StorageManager storageManager = new StorageManager();  
 //Connect to the StorageManager  
 storageManager.Connect();  
  
 //Move the Documents folder to D:\ServerFolders  
 Folder targetFolder = storageManager.Folders.First(folder => folder.Name == "Documents");  
  
 MoveFolderRequest moveRequest = targetFolder.GetMoveRequest(@"D:\");  
 moveRequest.MoveFolder();  
  
 //Verify operation was successful, if so print result  
 if (moveRequest.Status == OperationStatus.Succeeded)  
 {  
  Console.WriteLine("Folder {0} now located at {1}", targetFolder.Name, targetFolder.Path);  
 }  
  
 string newFolderName = "New Custom Folder";  
 string newFolderLocation = @"C:\ServerFolders\New Custom Folder";  
  
 //Create add request based with specific name and location  
 CreateFolderRequest request = storageManager.GetCreateFolderRequest(newFolderName, newFolderLocation);  
  
 //Give Guest user read only permission to folder  
 request.PermissionsByName.Add(new NamePermission("Guest", Permission.ReadOnly));  
  
 //Create the new folders  
 request.CreateFolder();  
  
 //Verify operation was successful, if so print result  
 if( request.Status == OperationStatus.Succeeded)  
 {  
  Folder newFolder = storageManager.Folders.First(folder => folder.Path == newFolderLocation);  
  
  Console.WriteLine("Folder {0} created at {1}", newFolder.Name, newFolder.Path);  
 }  
}  
```  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](Testing-the-Customer-Experience.md)
