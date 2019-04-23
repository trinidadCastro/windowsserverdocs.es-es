---
ms.assetid: 7e195f5b-b194-40f3-a26d-5cf4ade5fc4d
title: Cmdlets de copia de seguridad de entidad de certificación y restauración de Windows PowerShell
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a4bbeeedfb40e789a799103f9a29a848a2b32324
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877356"
---
# <a name="ca-backup-and-restore-windows-powershell-cmdlets"></a>Cmdlets de copia de seguridad de entidad de certificación y restauración de Windows PowerShell

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
>
**Autor**: Justin Turner, jefe ingeniero de soporte técnico con el grupo de Windows  
  
> [!NOTE]  
> Este contenido está escrito por un ingeniero de asistencia al cliente de Microsoft y está destinado a los arquitectos de sistemas y administradores con experiencia que están buscando explicaciones técnicas más detalladas de características y soluciones de Windows Server 2012 R2 que los temas que se suelen proporcionar en TechNet. Sin embargo, no ha experimentado los mismos pasos de edición, por lo que parte del lenguaje puede parecer menos perfeccionado de lo que se encuentra normalmente en TechNet.  
  
## <a name="overview"></a>Información general  
El módulo ADCSAdministration Windows PowerShell se introdujo en Windows Server 2012.  Se han agregado dos nuevos cmdlets para este módulo en Window Server 2012 R2 para admitir la copia de seguridad y restauración de una entidad de certificación.  
  
-   Backup-CARoleService  
  
-   Restore-CARoleService  
  
## <a name="backup-caroleservice"></a>Backup-CARoleService  
**Tabla SEQ tabla \\ \* árabe 17: Copia de seguridad y restauración de Windows PowerShell Cmdlets**  
  
**ADCSAdministration Cmdlet: Backup-CARoleService**  
  
|Argumentos - **negrita** se requieren argumentos|Descripción|  
|------------------------------------------------|---------------|  
|**-Path**|-String - ubicación para guardar la copia de seguridad<br />-Éste es el único parámetro sin nombre<br />-parámetro posicional<br /><br />**Ejemplo:**<br /><br />Backup-CARoleService.-Path c:\adcsbackup1<br /><br />Backup-CARoleService c:\adcsbackup2|  
|-KeyOnly|-El certificado de CA sin la base de datos de copia de seguridad<br /><br />**Ejemplo:**<br /><br />Backup-CARoleService c:\adcsbackup3 -KeyOnly|  
|-Password|-Especifica la contraseña para proteger los certificados de entidad emisora de certificados y claves privadas<br />-Debe ser una cadena segura<br />-No es válido con el parámetro - DatabaseOnly<br /><br />Por ejemplo:<br /><br />Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)<br /><br />Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)|  
|-DatabaseOnly|-Copia de seguridad de la base de datos sin el certificado de CA<br /><br />Backup-CARoleService c:\adcsbackup6 -DatabaseOnly|  
|-Force|1.  Permite sobrescribir la copia de seguridad que ya exista en la ubicación especificada en el parámetro - Path<br /><br />Backup-CARoleService c:\adcsbackup1 -Force|  
|-Incremental|-Realizar una copia de seguridad incremental<br /><br />Backup-CARoleService c:\adcsbackup7 -Incremental|  
|-KeepLog|1.  Indica al comando para mantener los archivos de registro. Si el conmutador no se especifica, se truncan los archivos de registro de forma predeterminada, excepto en el escenario Incremental<br /><br />Backup-CARoleService c:\adcsbackup7 -KeepLog|  
  
### <a name="-password-secure-string"></a>-Password <Secure String>  
Si-contraseña se usa el parámetro, la contraseña proporcionada debe ser una cadena segura.  Utilice la **Read-Host** cmdlet para iniciar un aviso interactivo para la entrada de contraseña segura, o usar el **ConvertTo-SecureString** cmdlet para especificar la contraseña en línea.  
  
Revise los siguientes ejemplos  
  
**Especifica una cadena segura para el parámetro de contraseña mediante Read-Host**  
  
```powershell  
Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)  
```  
  
**Especifica una cadena segura para el parámetro de contraseña mediante ConvertTo-SecureString**  
  
```powershell  
Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)  
```  
  
## <a name="restore-caroleservice"></a>Restore-CARoleService  
**ADCSAdministration Cmdlet: Restore-CARoleService**  
  
|Argumentos - **negrita** se requieren argumentos|Descripción|  
|------------------------------------------------|---------------|  
|**-Path**|-String - ubicación para restaurar la copia de seguridad de<br />-Éste es el único parámetro sin nombre<br />-parámetro posicional<br /><br />**Ejemplo:**<br /><br />Restore-CARoleService.-Path c:\adcsbackup1 -Force<br /><br />Restore-CARoleService c:\adcsbackup2 -Force|  
|-KeyOnly|-Restaurar el certificado de CA sin la base de datos<br />-Debe especificarse si se realizó la copia de seguridad con la opción - KeyOnly<br /><br />**Ejemplo:**<br /><br />Restore-CARoleService c:\adcsbackup3 -KeyOnly -Force|  
|-Password|-Especifica la contraseña de los certificados de entidad emisora de certificados y claves privadas<br />-Debe ser una cadena segura<br /><br />**Ejemplo:**<br /><br />Restore-CARoleService c:\adcsbackup4 -Password (read-host -prompt "Password:" -AsSecureString) -Force<br /><br />Restore-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force) -Force|  
|-DatabaseOnly|-Restauración de la base de datos sin el certificado de CA<br /><br />Restore-CARoleService c:\adcsbackup6 -DatabaseOnly|  
|-Force|-Permite sobrescribir las claves existentes<br />-Es un parámetro opcional pero durante la restauración en contexto, es probable que necesario<br /><br />Restore-CARoleService c:\adcsbackup1 -Force|  
  
### <a name="issues"></a>Problemas  
No se realiza una copia de seguridad protegido sin contraseña si se produce un error en la función de ConvertTo-SecureString mientras usa la Backup-CARoleService con el parámetro - contraseña.  
  
![Restauración y copia de seguridad de CA](media/CA-Backup-and-Restore-Windows-PowerShell-cmdlets/GTR_ADDS_BackupCARole.gif)  
  
**Tabla SEQ tabla \\ \* árabe 18: Errores comunes**  
  
|Acción|Error|Comentario|  
|----------|---------|-----------|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService: El proceso no puede acceder al archivo porque otro proceso lo está usando. (Excepción de HRESULT:<br /><br />0x80070020)|Detener el servicio de Active Directory Certificate Services antes de ejecutar el cmdlet Restore-CARoleService|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService: El directorio no está vacío. (Excepción de HRESULT: 0x80070091)|Use el parámetro - Force para sobrescribir las claves existentes|  
|**Backup-CARoleService C:\ADCSBackup-contraseña (Read-Host - Prompt "contraseña:" - AsSecureString) - DatabaseOnly**|Backup-CARoleService: Conjunto de parámetros no se puede resolver mediante especificado parámetros con nombre.|-Es el parámetro de contraseña solo se usa para la contraseña de proteger las claves privadas y, por tanto, es válido y es no una copia de seguridad|  
|**Restore-CARoleService C:\ADCSBack15-contraseña (Read-Host - Prompt "contraseña:" - AsSecureString) - DatabaseOnly**|Restore-CARoleService: Conjunto de parámetros no se puede resolver mediante especificado parámetros con nombre.|-Es el parámetro de contraseña solo se usa para la contraseña de proteger las claves privadas y, por tanto, es válido y no va a restaurar|  
|**Restore-CARoleService C:\ADCSBack14-contraseña (Read-Host - Prompt "contraseña:" - AsSecureString)**|Restore-CARoleService: El sistema no puede encontrar el archivo especificado. (Excepción de HRESULT: 0x80070002)|La ruta de acceso especificada no contiene una copia de seguridad de base de datos válido.  Quizás la ruta de acceso no es válido o se ha realizado la copia de seguridad con la opción - KeysOnly?|  
  
## <a name="additional-resources"></a>Recursos adicionales  
[Guía de migración de servicios de certificados de Active Directory](https://technet.microsoft.com/library/ee126170(v=ws.10).aspx)  
  
[Hacer una copia de seguridad de una base de datos y clave privada de CA](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_BackUpDB)  
  
[Restaurar la base de datos y configuración de CA en el servidor de destino](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_RestoreCA)  
  
## <a name="try-this-backup-the-ca-in-your-lab-using-windows-powershell"></a>Pruebe lo siguiente: Copia de seguridad de la entidad de certificación en su laboratorio por medio de Windows PowerShell  
  
1.  Use los comandos en esta lección para copia de seguridad de la base de datos y la clave privada protegida con una contraseña.  
  
2.  Aplace la restauración de la entidad de certificación en este momento.  
  


