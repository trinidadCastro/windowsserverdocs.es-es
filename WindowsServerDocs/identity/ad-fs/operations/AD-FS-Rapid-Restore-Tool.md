---
ms.assetid: 4deff06a-d0ef-4e5a-9701-5911ba667201
title: Herramienta de restauración rápida de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/02/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 525ba403473a9de522d9ab30662adc868b17b88d
ms.sourcegitcommit: c02756b7f5c92bf5018e17192f6fffb4754b0f06
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67533505"
---
# <a name="ad-fs-rapid-restore-tool"></a>Herramienta de restauración rápida de AD FS

## <a name="overview"></a>Información general
Hoy en día se realiza AD FS alta disponibilidad mediante la configuración de una granja de AD FS. Algunas organizaciones desearían una manera de tener un único servidor de implementación de AD FS, lo que elimina la necesidad de varios servidores de AD FS y carga de red, equilibrio de la infraestructura, mientras sigue teniendo algunos garantía de que el servicio se puede restaurar rápidamente si hay un problema.
La nueva herramienta de restauración rápida de AD FS proporciona una manera de restaurar los datos de AD FS sin necesidad de realizar una copia de seguridad completa y restaure el sistema operativo o el estado del sistema. Puede usar la nueva herramienta para exportar la configuración de AD FS en Azure o en una ubicación local.  A continuación, puede aplicar los datos exportados a una nueva instalación de AD FS, volver a crear o duplicar el entorno de AD FS. 

## <a name="scenarios"></a>Escenarios
La herramienta de restauración rápida de AD FS se puede usar en los escenarios siguientes:

1. Restaurar rápidamente la funcionalidad de AD FS tras un problema
    - Use la herramienta para crear una instalación en modo de espera en frío de AD FS que se pueda implementar rápidamente en lugar del servidor de AD FS en línea
2. Implementar entornos de prueba y producción idénticos
    - Utilice la herramienta para crear rápidamente una copia exacta de la producción de AD FS en un entorno de prueba, o para implementar rápidamente una configuración de pruebas validado en producción

## <a name="what-is-backed-up"></a>¿Qué es una copia de seguridad
La herramienta realiza una copia de seguridad de la siguiente configuración de AD FS
    
- Base de datos de AD FS configuration (WID o SQL)
- Archivo de configuración (ubicada en la carpeta de AD FS)
- Genera automáticamente el token de firma y descifrado de certificados y claves privadas (desde el contenedor DKM de Active Directory)
- Certificado SSL y los inscriben externamente (firma de tokens, descifrado de tokens y comunicación del servicio) de certificados y claves privadas correspondientes (Nota: las claves privadas deben ser exportables y el usuario que ejecuta el script debe tener permisos para tener acceso a ellos)
- Una lista de los proveedores de autenticación personalizados, los almacenes de atributos y proveedor de notificaciones local confía en que se instalan.

## <a name="how-to-use-the-tool"></a>Cómo usar la herramienta
Primero, [descargar](https://go.microsoft.com/fwlink/?LinkId=825646) e instale el MSI a su servidor de AD FS.  

Ejecute el siguiente comando desde un símbolo del sistema de PowerShell:

```powershell
import-module 'C:\Program Files (x86)\ADFS Rapid Recreation Tool\ADFSRapidRecreationTool.dll'
```

>[!NOTE] 
>Si usa la base de datos integrada de Windows (WID), esta herramienta debe ejecutarse en el servidor de AD FS principal.  Puede usar el `Get-AdfsSyncProperties` cmdlet de PowerShell para determinar si el servidor se encuentra en el servidor principal.

### <a name="system-requirements"></a>Requisitos del sistema

- Esta herramienta funciona para AD FS en Windows Server 2012 R2 y versiones posteriores. 
- Requiere .NET framework es al menos 4.0. 
- La restauración debe realizarse en un servidor de AD FS de la misma versión que la copia de seguridad y que usa la misma cuenta de Active Directory como cuenta de servicio de AD FS.

## <a name="create-a-backup"></a>Crear una copia de seguridad
Para crear una copia de seguridad, use el cmdlet de copia de seguridad de AD FS. Este cmdlet realiza una copia de seguridad de la configuración de AD FS, base de datos, los certificados SSL, etcetera. 

El usuario debe ser al menos un administrador local para ejecutar este cmdlet. Para el contenedor DKM de Active Directory (obligatorio en la configuración de valor predeterminado de AD FS) de copia de seguridad, el usuario debe ser administrador de dominio, tiene que pasar las credenciales de cuenta de servicio de AD FS o tiene acceso al contenedor DKM.  Si usa una cuenta de gMSA, el usuario debe ser administrador de dominio o tener permisos para el contenedor; no puede proporcionar las credenciales de la gMSA. 

La copia de seguridad se denominará según el patrón "adfsBackup_ID_Date-Time". Contendrá el número de versión, fecha y hora en que se realizó la copia de seguridad.
El cmdlet toma los parámetros siguientes:
    
Conjuntos de parámetros

![Herramienta de restauración rápida de AD FS](media/AD-FS-Rapid-Restore-Tool/parameter1.png)

### <a name="detailed-description"></a>Descripción detallada

- **BackupDKM** -realiza copias de seguridad del contenedor DKM de Active Directory que contiene las claves de AD FS en la configuración predeterminada (generado automáticamente token de firma y descifrado de certificados). Esto usa una herramienta AD 'ldifde' para exportar el contenedor y todos sus subárboles.

- -**StorageType &lt;cadena&gt;**  -el tipo de almacenamiento que el usuario desea usar. "FileSystem" indica que el usuario desea almacenarlos en una carpeta local o en la red "Azure" indica el usuario desea almacenarla en el contenedor de almacenamiento de Azure cuando el usuario realiza la copia de seguridad, seleccionen la ubicación de copia de seguridad, el sistema de archivos o en el en la nube. Para que Azure se utilice, se deben pasar las credenciales de Azure Storage al cmdlet. Las credenciales de almacenamiento contiene el nombre de cuenta y la clave. Además, también se debe pasar un nombre de contenedor en. Si el contenedor no existe, se crea durante la copia de seguridad. Para que el sistema de archivos que se utilizará, debe proporcionarse una ruta de acceso de almacenamiento. En ese directorio, se creará un nuevo directorio para cada copia de seguridad. Cada directorio creado contendrá los archivos de copiados de seguridad. 

- **ContraseñaDeCifrado &lt;cadena&gt;**  -la contraseña que se va a usar para cifrar todos los archivos de copiados de seguridad antes de almacenarlos

- **AzureConnectionCredentials &lt;pscredential&gt;**  -el nombre de cuenta y la clave para la cuenta de almacenamiento de Azure

- **AzureStorageContainer &lt;cadena&gt;**  -el contenedor de almacenamiento donde se almacenará la copia de seguridad en Azure

- **StoragePath &lt;cadena&gt;**  -la ubicación de las copias de seguridad se almacenarán en

- **ServiceAccountCredential &lt;pscredential&gt;**  -especifica la cuenta de servicio que se va a usar para el servicio AD FS ejecutando actualmente. Este parámetro solo es necesario si el usuario desea la DKM de copia de seguridad y no es administrador de dominio o no tiene acceso al contenido del contenedor. 

- **BackupComment &lt;string []&gt;**  -una cadena informativa sobre la copia de seguridad que se mostrará durante la restauración, similar al concepto de asignación de nombres de punto de comprobación de Hyper-V. El valor predeterminado es una cadena vacía

 
## <a name="backup-examples"></a>Ejemplos de copia de seguridad
Los siguientes son ejemplos de copia de seguridad para usar la herramienta de restauración rápida de AD FS.

### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-and-has-access-to-the-dkm-container-contents-either-domain-admin-or-delegated"></a>La configuración de AD FS con DKM, para el sistema de archivos de copia de seguridad y tiene acceso para el contenido del contenedor DKM (ya sea administrador de dominio o delegado)

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM
```
 
### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-with-the-service-account-credential-running-as-local-admin"></a>Configuración de FS AD de la copia de seguridad, con DKM, en el sistema de archivos con la credencial de cuenta de servicio, que se ejecuta como administrador local

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM -ServiceAccountCredential $cred
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-azure-storage-container"></a>Copia de seguridad de la configuración de AD FS sin la DKM para el contenedor de almacenamiento de Azure.

```powershell
Backup-ADFS -StorageType "Azure" -AzureConnectionCredentials $cred -AzureStorageContainer "adfsbackups"  -EncryptionPassword "password" -BackupComment "Clean Install of ADFS"
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-file-system"></a>La configuración de AD FS sin el DKM al sistema de archivos de copia de seguridad

```powershell   
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)"
```

## <a name="restore-from-backup"></a>Restaurar desde copia de seguridad
Para aplicar una configuración creada mediante copia de seguridad de AD FS a una nueva instalación de AD FS, use el cmdlet Restore-ADFS.

Este cmdlet crea una nueva granja de AD FS mediante el cmdlet `Install-AdfsFarm` y restaura la configuración de AD FS, base de datos, certificados, etcetera.  Si el rol de AD FS no se ha instalado en el servidor, el cmdlet lo instalará.  El cmdlet comprueba la ubicación de restauración de copias de seguridad existentes y solicita al usuario que elija una copia de seguridad adecuado en función de la fecha y hora que se tomó y cualquier comentario de copia de seguridad que el usuario es posible que asociados a la copia de seguridad. Si hay varias configuraciones de AD FS con los nombres de servicio de federación diferentes, se solicita al usuario elegir primero la configuración de AD FS adecuada.
El usuario tiene que ser administrador local y de dominio para ejecutar este cmdlet.


>[!NOTE] 
>Antes de usar la herramienta de recuperación rápida de AD FS, asegúrese de que el servidor está unido al dominio antes de restaurar la copia de seguridad. 

El cmdlet toma los parámetros siguientes: 

![Herramienta de restauración rápida de AD FS](media/AD-FS-Rapid-Restore-Tool/parameter2.png)

### <a name="detailed-description"></a>Descripción detallada

- **StorageType &lt;cadena&gt;**  -el tipo de almacenamiento que el usuario desea usar.
 "FileSystem" indica que el usuario desea almacenarla en una carpeta local o en la red "Azure" indica que el usuario desea almacenarla en el contenedor de almacenamiento de Azure

- **DecryptionPassword &lt;cadena&gt;**  -la contraseña que se usó para cifrar todos los archivos de copiados de seguridad 

- **AzureConnectionCredentials &lt;pscredential&gt;**  -el nombre de cuenta y la clave para la cuenta de almacenamiento de Azure

- **AzureStorageContainer &lt;cadena&gt;**  -el contenedor de almacenamiento donde se almacenará la copia de seguridad en Azure

- **StoragePath &lt;cadena&gt;**  -la ubicación de las copias de seguridad se almacenarán en

- **ADFSName &lt; cadena &gt;**  -el nombre de la federación que realizó la copia y se va a restaurar. Si no se proporciona y hay solo un nombre, a continuación, se usará. Si hay más de un servicio de federación una copia de seguridad a la ubicación, a continuación, se solicita al usuario elegir una de las copias de seguridad de servicios de federación.

- **ServiceAccountCredential &lt; pscredential &gt;**  -especifica la cuenta de servicio que se usará para el nuevo servicio de AD FS que se va a restaurar 

- **GroupServiceAccountIdentifier &lt;cadena&gt;**  -la GMSA que el usuario desea usar para el nuevo servicio de AD FS que se está restaurando. De forma predeterminada, si se proporciona ninguno, a continuación, la copia de seguridad de nombre de la cuenta se usa si fuese GMSA, else se solicita al usuario a colocar en una cuenta de servicio

- **DBConnectionString &lt;cadena&gt;**  : si el usuario desea usar una base de datos diferente para la restauración, a continuación, debe pasar la cadena de conexión de SQL o el tipo WID para WID.

- **Forzar &lt;bool&gt;**  -omitir las indicaciones que podría tener la herramienta una vez que se elige la copia de seguridad

- **RestoreDKM &lt;bool&gt;**  : restaure el contenedor DKM con AD, debe establecerse si se va a un nuevo anuncio y el DKM realizó una copia inicialmente.

## <a name="restore-examples"></a>Ejemplos de restore

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-azure-storage-container"></a>Restaurar la configuración de AD FS sin el DKM desde el contenedor de almacenamiento de Azure

```powershell
Restore-ADFS -StorageType "Azure" -AzureConnectionCredential $cred -DecryptionPassword "password" -AzureStorageContainer "adfsbackups"
```

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-file-system"></a>Restaurar la configuración de AD FS sin el DKM desde el sistema de archivos
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password"
```

### <a name="restore-the-ad-fs-configuration-with-the-dkm-to-the-file-system"></a>Restaurar la configuración de AD FS con el DKM al sistema de archivos 
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -RestoreDKM
```

### <a name="restore-the-ad-fs-configuration-to-wid"></a>Restaurar la configuración de AD FS a WID

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "WID"
``` 

### <a name="restore-the-ad-fs-configuration-to-sql"></a>Restaurar la configuración de AD FS a SQL

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "Data Source=TESTMACHINE\SQLEXPRESS; Integrated Security=True"
```

### <a name="restores-the-ad-fs-configuration-with-the-specified-gmsa"></a>Restaura la configuración de AD FS con la GMSA especificada

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -GroupServiceAccountIdentifier "mangupd1\adfsgmsa$"
```
### <a name="restore-the-ad-fs-configuration-with-the-specified-service-account-creds"></a>Restaurar la configuración de AD FS con las credenciales de cuenta de servicio especificado

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -ServiceAccountCredential $cred
```

## <a name="encryption-information"></a>Información de cifrado
Todos los datos de copia de seguridad se cifran antes de insertarla en la nube o almacenarlo en el sistema de archivos.  

Cada documento que se crea como parte de la copia de seguridad se cifra mediante AES-256. La contraseña que se pasan a la herramienta se utiliza como una frase para generar una nueva contraseña mediante la clase Rfc2898DeriveBytes. 

RngCryptoServiceProvider se usa para generar el valor "salt" que usa AES y la clase Rfc2898DeriveBytes. 

## <a name="log-files"></a>Archivos de registro
Cada vez que se realiza una copia de seguridad o restauración se crea un archivo de registro. Estos pueden encontrarse en la siguiente ubicación:

- **%localappdata%\ADFSRapidRecreationTool**

>[!NOTE]
> Al realizar una restauración que puede crearse un archivo de PostRestore_Instructions que contiene una descripción general de los proveedores de autenticación adicional, atributo almacena y confianzas de proveedor de notificaciones local instalarse manualmente antes de iniciar el servicio AD FS.

## <a name="version-release-history"></a>Historial de versiones

### <a name="version-10820"></a>Versión 1.0.82.0
Versión: Julio de 2019

**Problemas corregidos:**
- Los nombres de cuenta que contienen caracteres de escape LDAP de servicio de corrección de errores de AD FS


### <a name="version-10810"></a>Versión: 1.0.81.0
Versión: Abril de 2019

**Problemas corregidos:**


- Correcciones de errores de copia de seguridad del certificado y la restauración
- Información de seguimiento adicional en el archivo de registro


### <a name="version-10750"></a>Versión: 1.0.75.0
Versión: Agosto de 2018

**Problemas corregidos:**
* Actualizar copia de seguridad de AD FS cuando se usa el modificador - BackupDKM.  La herramienta determinará si el contexto actual tiene acceso al contenedor DKM.  Si es así, no se requieren privilegios de administrador de dominio o las credenciales de cuenta de servicio.  Esto permite que las copias de seguridad automatizadas a ocurrir sin tener que proporcionar las credenciales de forma explícita o que se ejecuta como una cuenta de administrador de dominio.

### <a name="version-10730"></a>Versión: 1.0.73.0
Versión: Agosto de 2018

**Problemas corregidos:**
* Actualizar los algoritmos de cifrado para que la aplicación es compatible con FIPS
    
    >[!NOTE]
    > Copias de seguridad antiguas no funcionará con la nueva versión debido a cambios en los algoritmos de cifrado según la compatibilidad con FIPS
    
* Agregar compatibilidad para clústeres SQL que usan la replicación de mezcla

### <a name="version-10720"></a>Versión: 1.0.72.0
Versión: Julio de 2018

**Problemas corregidos:**

   - Corrección de errores: Se ha corregido la. Instalador MSI para admitir las actualizaciones en contexto 

### <a name="10180"></a>1.0.18.0
Versión: Julio de 2018

**Problemas corregidos:**

   - Corrección de errores: administrar las contraseñas de cuentas de servicio que tienen caracteres especiales en ellos (es decir, '&')
   - Corrección de errores: se produce un error en la restauración porque otro proceso está usando la Microsoft.IdentityServer.Servicehost.exe.config


### <a name="1000"></a>1.0.0.0
Fecha de publicación: Octubre de 2016

Versión inicial de la herramienta de restauración rápida de AD FS
