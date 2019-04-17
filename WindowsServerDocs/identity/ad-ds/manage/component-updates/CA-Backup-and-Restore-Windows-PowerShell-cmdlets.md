---
ms.assetid: 7e195f5b-b194-40f3-a26d-5cf4ade5fc4d
title: Cmdlets de copia de seguridad de CA y restaurar Windows PowerShell
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: aba86ed080cc0b4043805531f0a2138b1b1d3cf8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ca-backup-and-restore-windows-powershell-cmdlets"></a>Cmdlets de copia de seguridad de CA y restaurar Windows PowerShell

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
>
**Autor**: Justin Turner, Senior ingeniero de soporte técnico con el grupo de Windows  
  
> [!NOTE]  
> Este contenido está escrito por un ingeniero de soporte técnico al cliente de Microsoft y está destinada a arquitectos de sistemas que buscan explicaciones técnicas más profundos de características y soluciones de Windows Server 2012 R2 que normalmente que proporcionan los temas en TechNet y administradores experimentados. Sin embargo, no ha sufrido las mismas fases de edición, forma parte del lenguaje que parezca que menos refinada que lo que normalmente se encuentra en TechNet.  
  
## <a name="overview"></a>Introducción  
El módulo de PowerShell de Windows ADCSAdministration se introdujo en la ventana Server 2012.  Se han agregado dos nuevos cmdlets para este módulo en la ventana Server 2012 R2 para admitir la copia de seguridad y restauración de una entidad de certificación.  
  
-   Copia de seguridad CARoleService  
  
-   Restaurar CARoleService  
  
## <a name="backup-caroleservice"></a>Copia de seguridad CARoleService  
**Tabla de tabla SEQ \\\ * árabe 17: una copia de seguridad y restaurar los Cmdlets de Windows PowerShell**  
  
**ADCSAdministration Cmdlet: Copia de seguridad-CARoleService**  
  
|Argumentos: **negrita** argumentos son necesarios|Descripción|  
|------------------------------------------------|---------------|  
|**-Path**|-Cadena - ubicación para guardar la copia de seguridad<br />-Este es el único parámetro de nombre<br />-parámetro posicional<br /><br />**Ejemplo:**<br /><br />Copia de seguridad-CARoleService.-c:\adcsbackup1 ruta de acceso<br /><br />Copia de seguridad CARoleService c:\adcsbackup2|  
|-KeyOnly|-El certificado de CA sin la base de datos de copia de seguridad<br /><br />**Ejemplo:**<br /><br />Copia de seguridad CARoleService c:\adcsbackup3 - KeyOnly|  
|: Contraseña|: Especifica la contraseña para proteger los certificados de entidad emisora de certificados y claves privadas<br />-Debe ser una cadena segura<br />-El parámetro - DatabaseOnly no es válido<br /><br />Ejemplo:<br /><br />Copia de seguridad CARoleService c:\adcsbackup4-contraseña (Read-Host - símbolo del sistema "contraseña:" - AsSecureString)<br /><br />Copia de seguridad CARoleService c:\adcsbackup5-contraseña (ConvertTo-SecureString "Pa55w0rd!" Forzar - AsPlainText)|  
|-DatabaseOnly|-Copia de seguridad de la base de datos sin el certificado de CA<br /><br />Copia de seguridad CARoleService c:\adcsbackup6 - DatabaseOnly|  
|-Force|1. permite que se sobrescriba la copia de seguridad que ya exista en la ubicación especificada en el parámetro - Path<br /><br />Copia de seguridad CARoleService c:\adcsbackup1-fuerza|  
|-Incremental|-Realizar una copia de seguridad incremental<br /><br />Copia de seguridad CARoleService c:\adcsbackup7-Incremental|  
|-GuardarRegistro|1. indica el comando para mantener los archivos de registro. Si no se especifica el conmutador, archivos de registro se truncan de manera predeterminada, excepto en el escenario Incremental<br /><br />Copia de seguridad CARoleService c:\adcsbackup7 - GuardarRegistro|  
  
### <a name="-password-secure-string"></a>: Contraseña<Secure String>  
Si la - contraseña se usa el parámetro, la contraseña debe ser una cadena segura.  Usar el **Read-Host** cmdlet para iniciar un símbolo del sistema interactiva de entrada de contraseña segura o usar la **ConvertTo SecureString** cmdlet para especificar la contraseña en línea.  
  
Revisa los siguientes ejemplos  
  
**Especifica una cadena segura para el parámetro de contraseña con el Host de lectura**  
  
```powershell  
Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)  
```  
  
**Especifica una cadena segura para el parámetro de contraseña con ConvertTo SecureString**  
  
```powershell  
Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)  
```  
  
## <a name="restore-caroleservice"></a>Restaurar CARoleService  
**ADCSAdministration Cmdlet: Restauración-CARoleService**  
  
|Argumentos: **negrita** argumentos son necesarios|Descripción|  
|------------------------------------------------|---------------|  
|**-Path**|-Cadena - ubicación para restaurar la copia de seguridad de<br />-Este es el único parámetro de nombre<br />-parámetro posicional<br /><br />**Ejemplo:**<br /><br />Restaurar-CARoleService.-ruta de acceso c:\adcsbackup1-fuerza<br /><br />Restaurar CARoleService c:\adcsbackup2-fuerza|  
|-KeyOnly|-Restaurar el certificado de CA sin la base de datos<br />-Se debe especificar si se realizó la copia de seguridad con la opción - KeyOnly<br /><br />**Ejemplo:**<br /><br />Restaurar CARoleService c:\adcsbackup3 - KeyOnly-fuerza|  
|: Contraseña|: Especifica la contraseña de los certificados de entidad emisora de certificados y claves privadas<br />-Debe ser una cadena segura<br /><br />**Ejemplo:**<br /><br />Restaurar CARoleService c:\adcsbackup4-contraseña (host de lectura: símbolo del sistema "contraseña:" - AsSecureString)-fuerza<br /><br />Restaurar CARoleService c:\adcsbackup5-contraseña (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText - Force)-fuerza|  
|-DatabaseOnly|-Restablecer la base de datos sin el certificado de CA<br /><br />Restaurar CARoleService c:\adcsbackup6 - DatabaseOnly|  
|-Force|: Permite sobrescribir las claves existentes<br />-Es un parámetro opcional, pero cuando se restaura en el lugar, es probable que necesario<br /><br />Restaurar CARoleService c:\adcsbackup1-fuerza|  
  
### <a name="issues"></a>Problemas  
Se toma una copia de seguridad protegida no contraseña si se produce un error en la función ConvertTo SecureString mientras usas el CARoleService de copia de seguridad con el - parámetro de contraseña.  
  
![Copia de seguridad de CA y restauración](media/CA-Backup-and-Restore-Windows-PowerShell-cmdlets/GTR_ADDS_BackupCARole.gif)  
  
**Tabla de tabla SEQ \\\ * árabe 18: errores comunes**  
  
|Acción|Error|Comentario|  
|----------|---------|-----------|  
|**Restaurar CARoleService C:\ADCSBackup**|CARoleService de restauración: El proceso no puede acceder al archivo porque está siendo utilizado por otro proceso. (Excepción de HRESULT:<br /><br />0 x 80070020)|Detener el servicio de servicios de certificados de Active Directory antes de ejecutar el cmdlet de restauración CARoleService|  
|**Restaurar CARoleService C:\ADCSBackup**|CARoleService de restauración: El directorio no está vacío. (Excepción de HRESULT: 0x80070091)|Usa el parámetro - Force para sobrescribir las claves existentes|  
|**Copia de seguridad CARoleService C:\ADCSBackup-contraseña (Read-Host - pedir "contraseña:" - AsSecureString) - DatabaseOnly**|CARoleService de copia de seguridad: Conjunto de parámetros no se puede resolver con parámetros con nombre.|-Contraseña parámetro es solo se usa para la contraseña proteger las claves privadas y, por tanto, es válido cuando se están no una copia de seguridad|  
|**Restaurar CARoleService C:\ADCSBack15-contraseña (Read-Host - pedir "contraseña:" - AsSecureString) - DatabaseOnly**|CARoleService de restauración: Conjunto de parámetros no se puede resolver con parámetros con nombre.|-Contraseña parámetro es solo se usa para contraseña proteger las claves privadas y, por tanto, es válido cuando no se está restaurando|  
|**Restaurar CARoleService C:\ADCSBack14-contraseña (Read-Host - pedir "contraseña:" - AsSecureString)**|CARoleService de restauración: El sistema no puede encontrar el archivo especificado. (Excepción de HRESULT: 0 x 80070002)|La ruta especificada no contiene una copia de seguridad de la base de datos válido.  Quizás la ruta de acceso es válido o se realizó la copia de seguridad con la opción - KeysOnly?|  
  
## <a name="additional-resources"></a>Recursos adicionales  
[Guía de migración de servicios de certificados de Active Directory](https://technet.microsoft.com/library/ee126170(v=ws.10).aspx)  
  
[Hacer copias de seguridad de una base de datos y la clave privada](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_BackUpDB)  
  
[Restauración de la base de datos y la configuración del servidor de destino](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_RestoreCA)  
  
## <a name="try-this-backup-the-ca-in-your-lab-using-windows-powershell"></a>Lo prueba siguiente: Copia de seguridad de la entidad de certificación en el laboratorio mediante Windows PowerShell  
  
1.  Usar los comandos en esta lección a la base de datos y la clave privada protegido con una contraseña de la copia de seguridad.  
  
2.  Conservar la restauración de la CA en este momento.  
  


