---
ms.assetid: 7e195f5b-b194-40f3-a26d-5cf4ade5fc4d
title: Cmdlets de Windows PowerShell para copia de seguridad y restauración de CA
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: b008ec74adfc3a4b6c63ec29f719c45483412b1d
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939565"
---
# <a name="ca-backup-and-restore-windows-powershell-cmdlets"></a>Cmdlets de Windows PowerShell para copia de seguridad y restauración de CA

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
>
> **Autor**: Diego Turner, Ingeniero de soporte técnico de nivel superior con el grupo de Windows
>
> [!NOTE]
> Este contenido está escrito por un ingeniero de asistencia al cliente de Microsoft y está destinado a los arquitectos de sistemas y administradores con experiencia que están buscando explicaciones técnicas más detalladas de características y soluciones de Windows Server 2012 R2 que los temas que se suelen proporcionar en TechNet. Sin embargo, no ha experimentado los mismos pasos de edición, por lo que parte del lenguaje puede parecer menos perfeccionado de lo que se encuentra normalmente en TechNet.

## <a name="overview"></a>Información general
El módulo ADCSAdministration de Windows PowerShell se presentó en Windows Server 2012.  Se han agregado dos nuevos cmdlets a este módulo en Windows Server 2012 R2 para admitir la copia de seguridad y restauración de una CA.

-   Backup-CARoleService

-   Restore-CARoleService

## <a name="backup-caroleservice"></a>Backup-CARoleService
**Tabla SEQ \\ \* . árabe 17: copia de seguridad y restauración de cmdlets de Windows PowerShell**

**Cmdlet ADCSAdministration: backup-CARoleService**

|Arguments: se requieren argumentos en **negrita**|Descripción|
|------------------------------------------------|---------------|
|**-Path**|-String-Location para guardar la copia de seguridad<br />-Este es el único parámetro sin nombre<br />-parámetro posicional<p>**Ejemplo**:<p>Backup-CARoleService.-Path c:\adcsbackup1<p>Backup-CARoleService c:\adcsbackup2|
|-KeyOnly|-Hacer una copia de seguridad del certificado de CA sin la base de datos<p>**Ejemplo**:<p>Backup-CARoleService c:\adcsbackup3-KeyOnly|
|-Password|: Especifica la contraseña para proteger los certificados y las claves privadas de la entidad de certificación.<br />-Debe ser una cadena segura<br />-No es válido con el parámetro-DatabaseOnly<p>Ejemplo:<p>Backup-CARoleService c:\adcsbackup4-Password (Read-host-prompt "Password:"-AsSecureString)<p>Backup-CARoleService c:\adcsbackup5-Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText-Force)|
|-DatabaseOnly|-Hacer una copia de seguridad de la base de datos sin el certificado de CA<p>Backup-CARoleService c:\adcsbackup6-DatabaseOnly|
|-Force|1. permite sobrescribir la copia de seguridad que existe en la ubicación especificada en el parámetro-Path.<p>Backup-CARoleService c:\adcsbackup1-Force|
|-Incremental|-Realizar una copia de seguridad incremental<p>Backup-CARoleService c:\adcsbackup7-incremental|
|-KeepLog|1. indica al comando que mantenga los archivos de registro. Si no se especifica el modificador, los archivos de registro se truncan de forma predeterminada, excepto en el escenario incremental.<p>Backup-CARoleService c:\adcsbackup7-KeepLog|

### <a name="-password-secure-string"></a>-Password <Secure String>
Si se usa el parámetro-password, la contraseña proporcionada debe ser una cadena segura.  Use el cmdlet **Read-host** para iniciar un símbolo del sistema interactivo para la entrada de contraseña segura o use el cmdlet **ConvertTo-SecureString** para especificar la contraseña en línea.

Revisar los ejemplos siguientes

**Especificación de una cadena segura para el parámetro password mediante Read-host**

```powershell
Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)
```

**Especificar una cadena segura para el parámetro de contraseña mediante ConvertTo-SecureString**

```powershell
Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)
```

## <a name="restore-caroleservice"></a>Restore-CARoleService
**Cmdlet ADCSAdministration: restore-CARoleService**

|Arguments: se requieren argumentos en **negrita**|Descripción|
|------------------------------------------------|---------------|
|**-Path**|-Cadena: ubicación de la que se va a restaurar la copia de seguridad<br />-Este es el único parámetro sin nombre<br />-parámetro posicional<p>**Ejemplo**:<p>Restore-CARoleService.-Path c:\adcsbackup1-Force<p>Restore-CARoleService c:\adcsbackup2-Force|
|-KeyOnly|-Restaurar el certificado de la entidad de certificación sin la base de datos<br />-Se debe especificar si la copia de seguridad se realizó con la opción-KeyOnly<p>**Ejemplo**:<p>Restore-CARoleService c:\adcsbackup3-KeyOnly-Force|
|-Password|: Especifica la contraseña de los certificados y las claves privadas de la entidad de certificación.<br />-Debe ser una cadena segura<p>**Ejemplo**:<p>Restore-CARoleService c:\adcsbackup4-Password (Read-host-prompt "contraseña:"-AsSecureString)-Force<p>Restore-CARoleService c:\adcsbackup5-Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText-Force)-Force|
|-DatabaseOnly|-Restaurar la base de datos sin el certificado de CA<p>Restore-CARoleService c:\adcsbackup6-DatabaseOnly|
|-Force|: Permite sobrescribir las claves existentes<br />-Es un parámetro opcional, pero al restaurar en contexto, es probable que sea necesario.<p>Restore-CARoleService c:\adcsbackup1-Force|

### <a name="issues"></a>Incidencias
Se realiza una copia de seguridad sin protección por contraseña si se produce un error en la función ConvertTo-SecureString mientras se usa el parámetro backup-CARoleService con el parámetro-password.

![Copia de seguridad y restauración de CA](media/CA-Backup-and-Restore-Windows-PowerShell-cmdlets/GTR_ADDS_BackupCARole.gif)

**Tabla SEQ \\ \* árabe 18: errores comunes**

|Acción|Error|Comentario|
|----------|---------|-----------|
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService: el proceso no puede obtener acceso al archivo porque otro proceso lo está usando. (Excepción de HRESULT:<p>0x80070020|Detener el servicio de servicios de Certificate Server de Active Directory antes de ejecutar el cmdlet restore-CARoleService|
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService: el directorio no está vacío. (Excepción de HRESULT: 0x80070091)|Usar el parámetro-Force para sobrescribir claves existentes|
|**Backup-CARoleService C:\ADCSBackup-Password (Read-host-prompt "Password:"-AsSecureString)-DatabaseOnly**|Backup-CARoleService: el conjunto de parámetros no se puede resolver mediante los parámetros con nombre especificados.|El parámetro-password solo se usa para proteger las claves privadas de la contraseña y, por lo tanto, no es válida cuando no se realiza una copia de seguridad.|
|**Restore-CARoleService C:\ADCSBack15-Password (Read-host-prompt "Password:"-AsSecureString)-DatabaseOnly**|Restore-CARoleService: el conjunto de parámetros no se puede resolver mediante los parámetros con nombre especificados.|El parámetro-password solo se usa para proteger las claves privadas de contraseña y, por lo tanto, no es válida cuando no se restauran.|
|**Restore-CARoleService C:\ADCSBack14-Password (Read-host-prompt "Password:"-AsSecureString)**|Restore-CARoleService: el sistema no encuentra el archivo especificado. (Excepción de HRESULT: 0x80070002)|La ruta de acceso especificada no contiene una copia de seguridad de base de datos válida.  Quizás la ruta de acceso no es válida o la copia de seguridad se realizó con la opción-KeysOnly?|

## <a name="additional-resources"></a>Recursos adicionales
[Guía de migración de los Servicios de certificados de Active Directory](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee126170(v=ws.10))

[Hacer una copia de seguridad de una base de datos y clave privada de CA](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee126140(v=ws.10)#BKMK_BackUpDB)

[Restaurar la base de datos y configuración de CA en el servidor de destino](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee126140(v=ws.10)#BKMK_RestoreCA)

## <a name="try-this-backup-the-ca-in-your-lab-using-windows-powershell"></a>Pruebe esto: realizar una copia de seguridad de la CA en el laboratorio con Windows PowerShell

1.  Use los comandos de esta lección para hacer una copia de seguridad de la base de datos de CA y la clave privada protegidas con una contraseña.

2.  Mantenga presionada la restauración de la CA en este momento.

