---
ms.assetid: 4deff06a-d0ef-4e5a-9701-5911ba667201
title: "Herramienta de restauración de AD FS rápida"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/20/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cd1cc8dab07288ba73c507bc551f089bb79502bc
ms.sourcegitcommit: 36d7b1dfd7da8e9f303d007a628e76149de000f2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/21/2018
---
# <a name="ad-fs-rapid-restore-tool"></a>Herramienta de restauración de AD FS rápida

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

## <a name="overview"></a>Introducción
Hoy en día AD FS estará disponible muy configurando una granja de servidores de AD FS. Algunas organizaciones quieres como una forma de un único servidor de implementación de AD FS, eliminando la necesidad de varios servidores de AD FS y equilibrio infraestructura, y al mismo tiempo algunos de carga de red garantiza que el servicio puede restaurarse rápidamente si hay un problema.
La nueva herramienta Restaurar rápida de AD FS proporciona una forma de restaurar los datos de AD FS sin necesidad de una copia de seguridad completa y la restauración del sistema operativo o el estado del sistema. Puedes usar la nueva herramienta para exportar la configuración de AD FS a Azure o en una ubicación local.  A continuación, puedes aplicar los datos exportados a una instalación nueva de AD FS, volver a crear ni duplica el entorno de AD FS. 

## <a name="scenarios"></a>Escenarios
La herramienta Restaurar rápida de AD FS puede usarse en los siguientes escenarios:

1. Restaurar rápidamente la funcionalidad de AD FS después de un problema
    - Usar la herramienta para crear una instalación de modo de espera fría de AD FS que se puede implementar rápidamente en lugar de en el servidor de AD FS en línea
2. Implementar entornos de prueba y producción idénticos
    - Usar la herramienta para crear rápidamente una copia exacta de la producción de AD FS en un entorno de prueba, o para implementar rápidamente una configuración de prueba validado en producción

## <a name="what-is-backed-up"></a>¿Qué es una copia de seguridad
La herramienta realiza una copia de la siguiente configuración de AD FS
    
- AD FS configuración base de datos (SQL WID)
- Archivo de configuración (que se encuentra en la carpeta de AD FS)
- Genera automáticamente el token de firma y el descifrado de certificados y claves privadas (desde el contenedor DKM de Active Directory)
- Certificado SSL y cualquier inscritos externamente certificados (token de inicio de sesión, descifrado token y comunicación de servicio) y claves privadas correspondientes (Nota: las claves privadas deben puede exportables y el usuario que ejecuta el script debe tener permisos para acceder a ellos)
- Una lista de los proveedores de autenticación personalizada, el atributo tiendas y el proveedor de notificaciones locales confía en que se instalan.

## <a name="how-to-use-the-tool"></a>Cómo usar la herramienta
Primero, [descargar](https://go.microsoft.com/fwlink/?LinkId=825646) e instalar el archivo MSI en el servidor de AD FS.  

>[!NOTE]
>La herramienta de restaurar rápida de AD FS no es compatible con FIPS.

Ejecuta el siguiente comando desde un símbolo del sistema de PowerShell:

```powershell
import-module 'C:\Program Files (x86)\ADFS Rapid Recreation Tool\ADFSRapidRecreationTool.dll'
```

>[!NOTE] 
>Si estás usando Windows integrada Database (WID), entonces esta herramienta necesita ejecutarse en el servidor de AD FS principal.  Puedes usar la `Get-SyncProperties` cmdlet de PowerShell para determinar si el servidor se encuentra en el servidor principal.

### <a name="system-requirements"></a>Requisitos del sistema

- Esta herramienta funciona de AD FS en Windows Server 2012 R2 y versiones posteriores. 
- .NET framework necesaria es al menos 4.0. 
- La restauración debe realizarse en un servidor de AD FS de la misma versión que la copia de seguridad y usa la misma cuenta de Active Directory como la cuenta de servicio de AD FS.

## <a name="create-a-backup"></a>Crear una copia de seguridad
Para crear una copia de seguridad, usa el cmdlet ADFS de copia de seguridad. Este cmdlet copia de la configuración de AD FS, base de datos, certificados SSL, etcetera. 

El usuario debe haber al menos un administrador local para ejecutar este cmdlet. Para una copia de seguridad del contenedor DKM de Active Directory (necesario en la configuración de predeterminados de AD FS), el usuario tiene que ser administrador del dominio o debe superar las credenciales de cuenta de servicio de AD FS.

La copia de seguridad se denominará según el modelo "adfsBackup_ID_Date tiempo". Contendrá el número de versión, la fecha y la hora que se ha realizado la copia de seguridad.
El cmdlet acepta los siguientes parámetros:
    
Conjuntos de parámetros

![Herramienta de restauración de AD FS rápida](media/AD-FS-Rapid-Restore-Tool/parameter1.png)

### <a name="detailed-description"></a>Descripción detallada

- **BackupDKM** -realiza copias de seguridad del contenedor DKM de Active Directory que contiene las claves de AD FS en la configuración predeterminada (token generado automáticamente firma y el descifrado de certificados). Esto usa una herramienta de AD 'ldifde' para exportar el contenedor de anuncios y todos sus subárboles.

- -**StorageType &lt;cadena&gt; ** -el tipo de almacenamiento que el usuario quiera utilizar. "FileSystem" indica que el usuario quiere guardarla en una carpeta local, en la red "Azure" indica el usuario quiere almacenarlo en el contenedor de almacenamiento de Azure, cuando el usuario realiza la copia de seguridad, selecciona la ubicación de copia de seguridad, el sistema de archivos o en la nube. De Azure para usarse, debe pasar las credenciales de almacenamiento de Azure al cmdlet. Las credenciales de almacenamiento contiene el nombre de cuenta y la clave. Además, el nombre del contenedor también se pueden pasar en. Si no existe el contenedor, se crea durante la copia de seguridad. Para que el sistema de archivos que se usará, debe tener una ruta de acceso de almacenamiento. En el directorio, se creará un nuevo directorio para cada copia de seguridad. Cada directorio creado contendrá los archivos de copia de seguridad. 

- **ContraseñaDeCifrado &lt;cadena&gt; ** -la contraseña que se va a usar para cifrar todos los archivos de copia de seguridad antes de almacenarlos

- **AzureConnectionCredentials &lt;pscredential&gt; ** -el nombre de cuenta y la clave de la cuenta de almacenamiento de Azure

- **AzureStorageContainer &lt;cadena&gt; ** -el contenedor de almacenamiento donde se almacenará la copia de seguridad de Azure

- **StoragePath &lt;cadena&gt; ** -la ubicación se almacenará las copias de seguridad en

- **ServiceAccountCredential &lt;pscredential&gt; ** -especifica la cuenta de servicio que se usa para el servicio de AD FS ejecutan actualmente. Este parámetro sólo es necesario si el usuario desea hacer copias de seguridad de la DKM y no es administrador de dominio.

- **BackupComment &lt;string []&gt; ** -una cadena informativa sobre la copia de seguridad que se mostrará durante la restauración, similar al concepto de nomenclatura de Hyper-V punto de control. El valor predeterminado es una cadena vacía

 
## <a name="backup-examples"></a>Ejemplos de copia de seguridad
Los siguientes son ejemplos de copia de seguridad para usar la herramienta de restaurar rápida de AD FS.

### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-while-running-as-the-domain-admin"></a>Copia de seguridad de la configuración de AD FS, con DKM, al sistema de archivos, mientras se ejecuta como el Administrador de dominio

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM
```
 
### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-with-the-service-account-credential-running-as-local-admin"></a>Configuración de FS el anuncio de la copia de seguridad, con DKM, al sistema de archivos con la credencial de cuenta de servicio, ejecutar como administrador local

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM -ServiceAccountCredential $cred
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-azure-storage-container"></a>Copia de seguridad de la configuración de AD FS sin el DKM al contenedor de almacenamiento de Azure.

```powershell
Backup-ADFS -StorageType "Azure" -AzureConnectionCredentials $cred -AzureStorageContainer "adfsbackups"  -EncryptionPassword "password" -BackupComment "Clean Install of ADFS"
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-file-system"></a>La configuración de AD FS sin el DKM al sistema de archivos de copia de seguridad

```powershell   
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)"
```

## <a name="restore-from-backup"></a>Restaurar a partir de copia de seguridad
Para aplicar una configuración que se crean mediante ADFS de copia de seguridad a una nueva instalación de AD FS, usa el cmdlet de restauración ADFS.

Este cmdlet crea un nuevo conjunto de AD FS mediante el cmdlet `Install-AdfsFarm` y restaura la configuración de AD FS, base de datos, certificados, etcetera. Si el rol de AD FS no se ha instalado en el servidor, se instalará el cmdlet.  El cmdlet comprueba la ubicación de restauración de copias de seguridad existentes y pide al usuario que elija una copia de seguridad correcta en función de la fecha y la hora en que se tomó y los comentarios de copia de seguridad que el usuario podría asociados a la copia de seguridad. Si hay varias configuraciones de AD FS con los nombres de servicios de federación diferentes, se pide al usuario elegir primero la configuración de AD FS adecuada.
El usuario tiene que ser un administrador local y de dominio para ejecutar este cmdlet.


>[!NOTE] 
>Antes de usar la herramienta de recuperación de AD FS rápido, asegúrate de que el servidor está unido al dominio antes de restaurar la copia de seguridad. 

El cmdlet acepta los siguientes parámetros: 

![Herramienta de restauración de AD FS rápida](media/AD-FS-Rapid-Restore-Tool/parameter2.png)

### <a name="detailed-description"></a>Descripción detallada

- **StorageType &lt;cadena&gt; ** -el tipo de almacenamiento que el usuario quiera utilizar.
 "FileSystem" indica que el usuario quiere guardarla en una carpeta local o en la red "Azure" indica que el usuario desea almacenarlo en el contenedor de almacenamiento de Azure

- **DecryptionPassword &lt;cadena&gt; ** -la contraseña que se usó para cifrar todos los archivos de copia de seguridad 

- **AzureConnectionCredentials &lt;pscredential&gt; ** -el nombre de cuenta y la clave de la cuenta de almacenamiento de Azure

- **AzureStorageContainer &lt;cadena&gt; ** -el contenedor de almacenamiento donde se almacenará la copia de seguridad de Azure

- **StoragePath &lt;cadena&gt; ** -la ubicación se almacenará las copias de seguridad en

- **ADFSName &lt; cadena &gt; ** -el nombre de la federación que realizó la copia y se va a restaurar. Si esto no se proporciona y hay federación solo un nombre de servicio a continuación, se usará. Si hay servicios de federación de más de una copia de seguridad a la ubicación, se pide al usuario que elijas uno de los servicios de federación de copia.

- **ServiceAccountCredential &lt; pscredential &gt; ** -especifica la cuenta de servicio que se usará para el nuevo servicio de AD FS que se restaura 

- **GroupServiceAccountIdentifier &lt;cadena&gt; ** -GMSA el que el usuario quiere usar para el nuevo servicio de AD FS se restaure. De manera predeterminada, si ninguno de ellos se proporciona a continuación, la copia de seguridad de nombre de cuenta se usa si era GMSA, más se pide al usuario colocar en una cuenta de servicio

- **DBConnectionString &lt;cadena&gt; ** : si el usuario quieres usar como una base de datos diferente para la restauración, a continuación, se deben pasar la cadena de conexión SQL o el tipo en WID para WID.

- **Fuerza &lt;bool&gt; ** -omitir los mensajes que puede tener la herramienta cuando se elige la copia de seguridad

- **RestoreDKM &lt;bool&gt; ** : restaurar el contenedor DKM al anuncio, debe establecerse si va a un nuevo anuncio y la DKM realizó la copia inicialmente.

## <a name="restore-examples"></a>Restaurar ejemplos

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-azure-storage-container"></a>Restaurar la configuración de AD FS sin el DKM desde el contenedor de almacenamiento de Azure

```powershell
Restore-ADFS -StorageType "Azure" -AzureConnectionCredential $cred -DecryptionPassword "password" -AzureStorageContainer "adfsbackups"
```

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-file-system"></a>Restaurar la configuración de AD FS sin el DKM desde el sistema de archivos
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password"
```

### <a name="restore-the-ad-fs-configuration-with-the-dkm-to-the-file-system"></a>Restaurar la configuración de AD FS con la DKM al sistema de archivos 
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -RestoreDKM
```

### <a name="restore-the-ad-fs-configuration-to-wid"></a>Restaurar la configuración de AD FS a WID

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "WID"
``` 

### <a name="restore-the-ad-fs-configuration-to-sql"></a>Restaurar la configuración de AD FS to SQL

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "Data Source=TESTMACHINE\SQLEXPRESS; Integrated Security=True"
```

### <a name="restores-the-ad-fs-configuration-with-the-specified-gmsa"></a>Restaura la configuración de AD FS con el GMSA especificado

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -GroupServiceAccountIdentifier "mangupd1\adfsgmsa$"
```
### <a name="restore-the-ad-fs-configuration-with-the-specified-service-account-creds"></a>Restaurar la configuración de AD FS con las credenciales de cuenta de servicio especificado

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -ServiceAccountCredential $cred
```

## <a name="encryption-information"></a>Información de cifrado
Todos los datos de copia de seguridad se cifran antes de insertarlo en la nube o almacenarlo en el sistema de archivos.  

Cada documento que se creó como parte de la copia de seguridad se cifra con AES-256. La contraseña que se pasa a la herramienta se usa como una frase para generar una nueva contraseña utilizando la clase Rfc2898DeriveBytes. 

RngCryptoServiceProvider se usa para generar la sal usada AES y la clase Rfc2898DeriveBytes. 

## <a name="log-files"></a>Archivos de registro
Cada vez que se realiza una copia de seguridad o una restauración, se crea un archivo de registro. Estos pueden encontrarse en la siguiente ubicación:

- **%localappdata%\ADFSRapidRecreationTool**

>[!NOTE]
> Al realizar una restauración que puede crear un archivo PostRestore_Instructions que contiene una descripción general de los proveedores de autenticación adicional, el atributo almacena y confía en el proveedor de notificaciones locales para instalarse manualmente antes de iniciar el servicio de AD FS.
