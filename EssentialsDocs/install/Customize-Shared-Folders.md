---
title: Personalizar carpetas compartidas
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47bc4986-14eb-4a29-9930-83a25704a3a0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: bcdd43183512bb225dd4afa916f2782c6eb79d7e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="customize-shared-folders"></a>Personalizar carpetas compartidas

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

De manera predeterminada, las carpetas del servidor se crean en la partición de datos más grande en el disco 0. Partners pueden personalizar la ubicación y especificar carpetas de servidor adicionales mediante los siguientes pasos:  
  
1.  Con una configuración de particiones personalizado, crea la imagen de fábrica y, a continuación, crear una nueva clave del registro de almacenamiento antes de usar sysprep. La tarea de IC de almacenamiento durante la configuración inicial (CI), busca esta clave del registro. Si existe, se crean las carpetas del servidor de forma predeterminada en el directorio C:\ServerFolders.  
  
    #### <a name="to-create-a-new-storage-registry-key"></a>Para crear una nueva clave del registro de almacenamiento  
  
    1.  En el servidor, mueve el mouse a la esquina superior derecha de la pantalla y haz clic en **búsqueda**.  
  
    2.  En el cuadro de búsqueda, escriba **regedit**y, a continuación, haz clic en el **Regedit** aplicación.  
  
    3.  En el panel de navegación, expande **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**y después expande **Microsoft**.  
  
    4.  Haz clic en **Windows Server**, haz clic en **nueva**y, a continuación, haz clic en **clave**.  
  
    5.  Nombre de la clave **almacenamiento**.  
  
    6.  En el panel de navegación, haz clic en el nuevo almacenamiento del registro clave, haz clic en **nueva**y, a continuación, haz clic en **valor DWORD (32 bits)**.  
  
    7.  La cadena el nombre **CreateFoldersOnSystem**.  
  
    8.  Haz clic en **CreateFoldersOnSystem**y, a continuación, haz clic en **modificar**. La **Editar cadena** aparece el cuadro de diálogo.  
  
    9. Establece el valor de esta nueva clave en **1**y, a continuación, haz clic en **Aceptar**.  
  
2.  Usa el script PostIC.cmd para mover las carpetas a otra ubicación o para crear más carpetas. Consulta el siguiente ejemplo: [ejemplo 1: crear una carpeta personalizada y mover las carpetas predeterminadas a una nueva ubicación desde PostIC.cmd mediante Windows PowerShell](Customize-Shared-Folders.md#BKMK_Example1).  
  
3.  Usar el SDK de soluciones de Windows Server para mover las carpetas a otra ubicación o para crear más carpetas. Consulta el siguiente ejemplo: [ejemplo 2: crear una carpeta personalizada y mover una carpeta existente mediante el SDK de soluciones de Windows Server](Customize-Shared-Folders.md#BKMK_Example2).  
  
 Opcionalmente, puedes asociados pueden dejar las carpetas de datos en la unidad C. Esto permite al usuario final o al revendedor determinar el diseño de las carpetas de datos en las unidades de datos.  
  
###  <a name="BKMK_Example1"></a>Ejemplo 1: Crear una carpeta personalizada y mover las carpetas predeterminadas a una nueva ubicación desde PostIC.cmd mediante Windows PowerShell  
  
1.  Crear un archivo PostIC.cmd para ejecutar tareas de configuración inicial de posteriores como se detalla en el [crear el archivo PostIC.cmd para ejecutar registrar otro tareas de configuración inicial](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md) sección.  
  
2.  El Bloc de notas, crea un archivo llamado **customizefolders.ps1** en la carpeta C:\Windows\Setup\Scripts y, a continuación, copia el siguiente Windows PowerShell® comandos en el archivo (desmarca las líneas apropiadas según el comportamiento deseado).  
  
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
  
3.  Agrega la siguiente línea al archivo PostIC.cmd para ejecutar este script.  
  
    ```  
    REM Lower the execution policy  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy RemoteSigned"  
  
    REM Execute the folder customization script  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" -NoProfile -Noninteractive -command ". %windir%\setup\scripts\customizefolders.ps1;exit $LASTEXITCODE"  
    Set error_level=%ERRORLEVEL%  
  
    REM Restore the execution policy to deafult  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy Restricted"  
    Set ERRORLEVEL=%error_level%  
    ```  
  
###  <a name="BKMK_Example2"></a>Ejemplo 2: Crear una carpeta personalizada y mover una carpeta existente mediante el SDK de soluciones de Windows Server  
 El código que creas que puede compilarse como ejecutable y, a continuación, llamado desde el archivo PostIC.cmd o directamente desde un complemento instalado.  
  
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
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)