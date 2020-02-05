---
ms.assetid: 4deff06a-d0ef-4e5a-9701-5911ba667201
title: Herramienta de restauración rápida de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/02/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8b47cdc4770b1ed6478d1502ed5264164e99352b
ms.sourcegitcommit: a33404f92867089bb9b0defcd50960ff231eef3f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "77013050"
---
# <a name="ad-fs-rapid-restore-tool"></a>Herramienta de restauración rápida de AD FS

## <a name="overview"></a>Introducción
Hoy en día AD FS tiene una alta disponibilidad mediante la configuración de una granja de AD FS. Algunas organizaciones desean una manera de tener un solo servidor AD FS implementación, lo que elimina la necesidad de varios servidores de AD FS y de la infraestructura de equilibrio de carga de red, a la vez que se garantiza que el servicio se puede restaurar rápidamente en caso de que se produzca un problema.
La nueva herramienta de restauración rápida AD FS proporciona una manera de restaurar AD FS datos sin necesidad de una copia de seguridad y restauración completa del sistema operativo o del estado del sistema. Puede usar la nueva herramienta para exportar AD FS configuración a Azure o a una ubicación local.  A continuación, puede aplicar los datos exportados a una instalación nueva AD FS, volver a crear o duplicar el entorno de AD FS. 

## <a name="scenarios"></a>Escenarios
La herramienta de restauración rápida de AD FS se puede usar en los escenarios siguientes:

1. Restaurar rápidamente la funcionalidad de AD FS después de un problema
    - Use la herramienta para crear una instalación en espera pasiva de AD FS que se pueda implementar rápidamente en lugar del servidor de AD FS en línea.
2. Implementación de entornos de prueba y producción idénticos
    - Use la herramienta para crear rápidamente una copia precisa de los AD FS de producción en un entorno de prueba o para implementar rápidamente una configuración de prueba validada en producción.

>[!NOTE] 
>Si usa la replicación de mezcla de SQL o los grupos de disponibilidad AlwaysOn, no se admite la herramienta de restauración rápida. Se recomienda usar copias de seguridad basadas en SQL y una copia de seguridad del certificado SSL como alternativa.

## <a name="what-is-backed-up"></a>Contenido de copia de seguridad
La herramienta realiza una copia de seguridad de la siguiente configuración de AD FS
    
- AD FS base de datos de configuración (SQL o WID)
- Archivo de configuración (ubicado en AD FS carpeta)
- Certificados de firma y descifrado de tokens generados automáticamente y claves privadas (desde el Active Directory contenedor DKM)
- Certificado SSL y cualquier certificado inscrito externamente (firma de tokens, descifrado de tokens y comunicación de servicio) y claves privadas correspondientes (Nota: las claves privadas deben ser exportables y el usuario que ejecuta el script debe tener permisos de acceso)
- Una lista de los proveedores de autenticación personalizados, los almacenes de atributos y las confianzas de proveedor de notificaciones locales que están instaladas.

## <a name="how-to-use-the-tool"></a>Cómo usar la herramienta
En primer lugar, [Descargue](https://go.microsoft.com/fwlink/?LinkId=825646) e instale el archivo MSI en el servidor de AD FS.  

Ejecute el siguiente comando desde un símbolo del sistema de PowerShell:

```powershell
import-module 'C:\Program Files (x86)\ADFS Rapid Recreation Tool\ADFSRapidRecreationTool.dll'
```

>[!NOTE] 
>Si usa la base de datos integrada de Windows (WID), esta herramienta debe ejecutarse en el servidor de AD FS principal.  Puede usar el cmdlet de PowerShell de `Get-AdfsSyncProperties` para determinar si el servidor en el que se encuentra es el servidor principal.

### <a name="system-requirements"></a>Requisitos del sistema

- Esta herramienta funciona para AD FS en Windows Server 2012 R2 y versiones posteriores. 
- La versión de .NET Framework necesaria es al menos 4,0. 
- La restauración debe realizarse en un servidor de AD FS de la misma versión que la copia de seguridad y que use la misma cuenta Active Directory que la cuenta de servicio de AD FS.

## <a name="create-a-backup"></a>Crear una copia de seguridad
Para crear una copia de seguridad, use el cmdlet backup-ADFS. Este cmdlet hace una copia de seguridad de la configuración AD FS, la base de datos, los certificados SSL, etc. 

El usuario debe ser al menos un administrador local para ejecutar este cmdlet. Para realizar una copia de seguridad del contenedor DKM de Active Directory (necesario en la configuración de AD FS predeterminada), el usuario debe ser administrador del dominio, tener que pasar las credenciales de la cuenta de servicio de AD FS o tener acceso al contenedor DKM.  Si utiliza una cuenta de gMSA, el usuario debe ser administrador del dominio o tener permisos para el contenedor. no puede proporcionar las credenciales de gMSA. 

La copia de seguridad se denominará según el patrón "adfsBackup_ID_Date-Time". Contendrá el número de versión, la fecha y la hora en que se realizó la copia de seguridad.
El cmdlet toma los siguientes parámetros:
    
Conjuntos de parámetros

![Herramienta de restauración rápida de AD FS](media/AD-FS-Rapid-Restore-Tool/parameter1.png)

### <a name="detailed-description"></a>Descripción detallada

- **BackupDKM** : realiza una copia de seguridad del contenedor DKM Active Directory que contiene las claves de AD FS en la configuración predeterminada (los certificados de firma y descifrado de tokens generados automáticamente). Usa una herramienta de AD ' LDIFDE ' para exportar el contenedor de AD y todos sus subárboles.

- -**StorageType &lt;cadena&gt;** : el tipo de almacenamiento que el usuario desea usar. "Sistema de archivos" indica que el usuario desea almacenarlo en una carpeta localmente o en la red "Azure" indica que el usuario desea almacenarlo en el contenedor de Azure Storage cuando el usuario realiza la copia de seguridad, selecciona la ubicación de la copia de seguridad, ya sea el sistema de archivos o en el Administración. Para que se use Azure, se deben pasar Azure Storage credenciales al cmdlet. Las credenciales de almacenamiento contienen el nombre y la clave de la cuenta. Además de esto, también se debe pasar un nombre de contenedor. Si el contenedor no existe, se crea durante la copia de seguridad. Para el sistema de archivos que se va a usar, se debe proporcionar una ruta de acceso de almacenamiento. En ese directorio, se creará un nuevo directorio para cada copia de seguridad. Cada directorio creado contendrá los archivos de los que se ha realizado una copia de seguridad. 

- **EncryptionPassword &lt;cadena&gt;** : la contraseña que se va a usar para cifrar todos los archivos de los que se ha realizado una copia de seguridad antes de almacenarlos

- **AzureConnectionCredentials &lt;pscredential&gt;** : el nombre de cuenta y la clave de la cuenta de almacenamiento de Azure

- **AzureStorageContainer &lt;cadena&gt;** : el contenedor de almacenamiento donde se almacenará la copia de seguridad en Azure

- **StoragePath &lt;cadena&gt;** : la ubicación en la que se almacenarán las copias de seguridad

- **ServiceAccountCredential &lt;pscredential&gt;** : especifica la cuenta de servicio que se usa para el servicio de AD FS que se ejecuta actualmente. Este parámetro solo es necesario si el usuario desea realizar una copia de seguridad del DKM y no es un administrador de dominio o no tiene acceso al contenido del contenedor. 

- **BackupComment &lt;String []&gt;** -una cadena informativa sobre la copia de seguridad que se mostrará durante la restauración, similar al concepto de nomenclatura de los puntos de comprobación de Hyper-V. El valor predeterminado es una cadena vacía.

 
## <a name="backup-examples"></a>Ejemplos de copia de seguridad
A continuación se muestran ejemplos de copia de seguridad para usar la herramienta de restauración rápida AD FS.

### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-and-has-access-to-the-dkm-container-contents-either-domain-admin-or-delegated"></a>Realice una copia de seguridad de la configuración de AD FS, con el DKM, en el sistema de archivos y tenga acceso al contenido del contenedor DKM (Administrador del dominio o delegado).

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM
```
 
### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-with-the-service-account-credential-running-as-local-admin"></a>Realice una copia de seguridad de la configuración de AD FS, con el DKM, en el sistema de archivos con la credencial de cuenta de servicio, que se ejecuta como administrador local.

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM -ServiceAccountCredential $cred
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-azure-storage-container"></a>Realice una copia de seguridad de la configuración de AD FS sin el DKM en el contenedor de Azure Storage.

```powershell
Backup-ADFS -StorageType "Azure" -AzureConnectionCredentials $cred -AzureStorageContainer "adfsbackups"  -EncryptionPassword "password" -BackupComment "Clean Install of ADFS"
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-file-system"></a>Hacer una copia de seguridad de la configuración de AD FS sin el DKM en el sistema de archivos

```powershell   
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)"
```

## <a name="restore-from-backup"></a>Restaurar desde copia de seguridad
Para aplicar una configuración creada mediante backup-ADFS a una nueva AD FS instalación, use el cmdlet restore-ADFS.

Este cmdlet crea una nueva granja de AD FS mediante el cmdlet `Install-AdfsFarm` y restaura la configuración de AD FS, la base de datos, los certificados, etc.  Si el rol de AD FS no se ha instalado en el servidor, el cmdlet lo instalará.  El cmdlet comprueba la ubicación de restauración de las copias de seguridad existentes y solicita al usuario que elija una copia de seguridad adecuada en función de la fecha y la hora en que se realizó y cualquier comentario de copia de seguridad que el usuario haya adjuntado a la copia de seguridad. Si hay varias configuraciones de AD FS con distintos nombres de servicio de Federación, se solicitará al usuario que elija primero la configuración de AD FS adecuada.
El usuario debe ser administrador local y de dominio para ejecutar este cmdlet.


>[!NOTE] 
>Antes de usar la herramienta de recuperación rápida AD FS, asegúrese de que el servidor está unido al dominio antes de restaurar la copia de seguridad. 

El cmdlet toma los siguientes parámetros: 

![Herramienta de restauración rápida de AD FS](media/AD-FS-Rapid-Restore-Tool/parameter2.png)

### <a name="detailed-description"></a>Descripción detallada

- **StorageType &lt;cadena&gt;** : el tipo de almacenamiento que el usuario desea usar.
 "Sistema de archivos" indica que el usuario desea almacenarlo en una carpeta localmente o en la red "Azure" indica que el usuario desea almacenarlo en el contenedor de Azure Storage

- **DecryptionPassword &lt;cadena&gt;** -la contraseña que se usó para cifrar todos los archivos de los que se ha realizado una copia de seguridad 

- **AzureConnectionCredentials &lt;pscredential&gt;** : el nombre de cuenta y la clave de la cuenta de almacenamiento de Azure

- **AzureStorageContainer &lt;cadena&gt;** : el contenedor de almacenamiento donde se almacenará la copia de seguridad en Azure

- **StoragePath &lt;cadena&gt;** : la ubicación en la que se almacenarán las copias de seguridad

- **ADFSName &lt; cadena &gt;** : el nombre de la Federación de la que se realizó una copia de seguridad y que se va a restaurar. Si no se proporciona y solo hay un nombre de servicio de Federación, se usará. Si se realiza una copia de seguridad de más de un servicio de Federación en la ubicación, se solicitará al usuario que elija uno de los servicios de Federación de los que se ha realizado una copia de seguridad.

- **ServiceAccountCredential &lt; pscredential &gt;** : especifica la cuenta de servicio que se usará para el nuevo servicio de AD FS que se va a restaurar. 

- **GroupServiceAccountIdentifier &lt;cadena&gt;** : GMSA que el usuario desea usar para el nuevo servicio de AD FS que se está restaurando. De forma predeterminada, si no se proporciona ninguno, se usa el nombre de la cuenta de copia de seguridad si se GMSA, de lo contrario se solicita al usuario que se ponga en una cuenta de servicio.

- **DBConnectionString &lt;&gt;de cadena** : Si el usuario desea usar una base de BD diferente para la restauración, debe pasar la cadena de conexión de SQL o el tipo en WID para WID.

- **Forzar &lt;bool&gt;** : omitir los mensajes que podría tener la herramienta una vez elegida la copia de seguridad

- **RestoreDKM &lt;bool&gt;** : restaure el contenedor DKM en AD, debe establecerse si va a un nuevo anuncio y se ha realizado una copia de seguridad del DKM inicialmente.

## <a name="restore-examples"></a>Ejemplos de restore

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-azure-storage-container"></a>Restaure la configuración de AD FS sin el DKM del contenedor de Azure Storage

```powershell
Restore-ADFS -StorageType "Azure" -AzureConnectionCredential $cred -DecryptionPassword "password" -AzureStorageContainer "adfsbackups"
```

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-file-system"></a>Restaurar la configuración de AD FS sin el DKM del sistema de archivos
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password"
```

### <a name="restore-the-ad-fs-configuration-with-the-dkm-to-the-file-system"></a>Restaure la configuración de AD FS con el DKM en el sistema de archivos 
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -RestoreDKM
```

### <a name="restore-the-ad-fs-configuration-to-wid"></a>Restaurar la configuración de AD FS a WID

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "WID"
``` 

### <a name="restore-the-ad-fs-configuration-to-sql"></a>Restaurar la configuración de AD FS en SQL

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "Data Source=TESTMACHINE\SQLEXPRESS; Integrated Security=True"
```

### <a name="restores-the-ad-fs-configuration-with-the-specified-gmsa"></a>Restaura la configuración de AD FS con el GMSA especificado.

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -GroupServiceAccountIdentifier "mangupd1\adfsgmsa$"
```
### <a name="restore-the-ad-fs-configuration-with-the-specified-service-account-creds"></a>Restaure la configuración de AD FS con las credenciales de la cuenta de servicio especificada

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -ServiceAccountCredential $cred
```

## <a name="encryption-information"></a>Información de cifrado
Todos los datos de copia de seguridad se cifran antes de insertarlos en la nube o almacenarlos en el sistema de archivos.  

Cada documento que se crea como parte de la copia de seguridad se cifra mediante AES-256. La contraseña que se pasa a la herramienta se usa como frase de contraseña para generar una nueva contraseña mediante la clase Rfc2898DeriveBytes. 

RngCryptoServiceProvider se usa para generar el valor Salt que usa AES y la clase Rfc2898DeriveBytes. 

## <a name="log-files"></a>Archivos de registro de
Cada vez que se realiza una copia de seguridad o una restauración, se crea un archivo de registro. Se pueden encontrar en la siguiente ubicación:

- **%localappdata%\ADFSRapidRecreationTool**

>[!NOTE]
> Al realizar una restauración, es posible que se cree un archivo de PostRestore_Instructions que contenga información general de los proveedores de autenticación adicionales, los almacenes de atributos y las confianzas de proveedor de notificaciones locales que se instalarán manualmente antes de iniciar el servicio AD FS.

## <a name="version-release-history"></a>Historial de versiones

### <a name="version-10820"></a>Versión 1.0.82.0
Versión: 2019 de julio

**Problemas corregidos:**
- Corrección de errores para AD FS nombres de cuenta de servicio que contienen caracteres de escape LDAP


### <a name="version-10810"></a>Versión: 1.0.81.0
Versión: 2019 de abril

**Problemas corregidos:**


- Correcciones de errores de copia de seguridad y restauración de certificados
- Información de seguimiento adicional al archivo de registro


### <a name="version-10750"></a>Versión: 1.0.75.0
Versión: 2018 de agosto

**Problemas corregidos:**
* Actualice backup-ADFS al usar el modificador-BackupDKM.  La herramienta determinará si el contexto actual tiene acceso al contenedor DKM.  En caso afirmativo, no necesitará privilegios de administrador de dominio ni credenciales de cuenta de servicio.  Esto permite que las copias de seguridad automáticas se realicen sin proporcionar explícitamente credenciales ni ejecutar como cuenta de administrador de dominio.

### <a name="version-10730"></a>Versión: 1.0.73.0
Versión: 2018 de agosto

**Problemas corregidos:**
* Actualización de los algoritmos de cifrado para que la aplicación sea compatible con FIPS
    
    >[!NOTE]
    > Las copias de seguridad antiguas no funcionarán con la nueva versión debido a cambios en los algoritmos de cifrado según el cumplimiento de FIPS
    
* Agregar compatibilidad para clústeres de SQL que utilizan la replicación de mezcla

### <a name="version-10720"></a>Versión: 1.0.72.0
Versión: 2018 de julio

**Problemas corregidos:**

   - Corrección de errores: se corrigió el. Instalador de MSI para admitir actualizaciones en contexto 

### <a name="10180"></a>1.0.18.0
Versión: 2018 de julio

**Problemas corregidos:**

   - Corrección de errores: controlar contraseñas de cuentas de servicio que tienen caracteres especiales (es decir, ' & ')
   - Corrección de errores: se produce un error en la restauración porque otro proceso está usando Microsoft. IdentityServer. ServiceHost. exe. config


### <a name="1000"></a>1.0.0.0
Fecha de publicación: 2016 de octubre

Versión inicial de AD FS herramienta de restauración rápida
