---
ms.assetid: 7671e0c9-faf0-40de-808a-62f54645f891
title: Actualización a AD FS en Windows Server 2016
author: billmath
manager: femila
ms.date: 04/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4c13a3ecbcc6ade1455c10dde5f6a89e0303e161
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857638"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-using-a-wid-database"></a>Actualización a AD FS en Windows Server 2016 mediante una base de datos WID


> [!NOTE]
> Solo se inicia una actualización con un período de tiempo definitivo planeado para su finalización. No se recomienda mantener AD FS en un estado de modo mixto durante un largo período de tiempo, ya que si se deja AD FS en un estado de modo mixto, pueden producirse problemas con la granja.

## <a name="upgrading-a-windows-server-2012-r2-or-2016-ad-fs-farm-to-windows-server-2019"></a>Actualización de una granja de servidores de Windows Server 2012 R2 o 2016 AD FS a Windows Server 2019
En el documento siguiente se describe cómo actualizar la granja de AD FS a AD FS en Windows Server 2019 cuando se utiliza una base de datos WID.

### <a name="ad-fs-farm-behavior-levels-fbl"></a>Niveles de comportamiento de granja de AD FS (FBL)
En AD FS para Windows Server 2016, se presentó el nivel de comportamiento de la granja de servidores (FBL). Se trata de una configuración que abarca todo el conjunto de servidores que determina las características que puede usar la granja de AD FS.

En la tabla siguiente se enumeran los valores de FBL por versión de Windows Server:

| Versión de Windows Server  | FBL | Nombre de la base de datos de configuración AD FS |
| ------------- | ------------- | ------------- |
| 2012 R2  | 1  | AdfsConfiguration |
| 2016  | 3  | AdfsConfigurationV3 |
| 2019  | 4  | AdfsConfigurationV4 |

> [!NOTE]
> Al actualizar FBL, se crea una nueva base de datos de configuración de AD FS.  Vea la tabla anterior para ver los nombres de la base de datos de configuración para cada versión de Windows Server AD FS y el valor FBL

### <a name="new-vs-upgraded-farms"></a>Nuevas granjas de servidores y actualizadas
De forma predeterminada, FBL en una nueva granja de AD FS coincide con el valor de la versión de Windows Server del primer nodo de la granja instalado.

Un servidor de AD FS de una versión posterior puede estar unido a una granja de AD FS 2012 R2 o 2016, y la granja funcionará en el mismo FBL que los nodos existentes. Cuando hay varias versiones de Windows Server que funcionan en la misma granja en el valor FBL de la versión más antigua, se dice que la granja está "mixta". Sin embargo, no podrá beneficiarse de las características de las versiones posteriores hasta que se genere el FBL. Con una granja mixta:

- Los administradores pueden agregar nuevos servidores de Federación de Windows Server 2019 a una granja existente de Windows Server 2012 R2 o 2016. Como resultado, la granja está en "modo mixto" y funciona en el mismo nivel de comportamiento de la granja que la granja original. Para garantizar un comportamiento coherente en toda la granja, no se pueden configurar o usar las características de las versiones más recientes de Windows Server AD FS.

- Antes de que se pueda generar el FBL, los administradores deben quitar el AD FS nodos de versiones anteriores de Windows Server de la granja.  En el caso de una granja WID, tenga en cuenta que esto requiere que uno de los nuevos servidores de Federación de Windows Server 2019 se promueva al rol del nodo principal de la granja.

- Una vez que todos los servidores de Federación de la granja están en la misma versión de Windows Server, se puede generar FBL.  Como resultado, se pueden configurar y usar las nuevas características AD FS Windows Server 2019.

Tenga en cuenta que, en el modo de granja mixta, la granja de AD FS no es capaz de ofrecer nuevas características o funciones introducidas en AD FS en Windows Server 2019. Esto significa que las organizaciones que deseen probar nuevas características no podrán hacerlo hasta que se genere el FBL. Por lo tanto, si su organización desea probar las nuevas características antes de generar FBL, deberá implementar una granja independiente para hacerlo.

El resto del documento is proporciona los pasos para agregar un servidor de Federación de Windows Server 2019 a un entorno de Windows Server 2016 o 2012 R2 y, a continuación, elevar FBL a Windows Server 2019. Estos pasos se realizaron en un entorno de prueba que se describe en el diagrama arquitectónico siguiente.

> [!NOTE]
> Antes de poder pasar a AD FS en Windows Server 2019 FBL, debe quitar todos los nodos de Windows Server 2016 o 2012 R2. No se puede actualizar un sistema operativo Windows Server 2016 o 2012 R2 a Windows Server 2019 y hacer que se convierta en un nodo 2019. Tendrá que quitarlo y reemplazarlo por un nuevo nodo 2019.

##### <a name="to-upgrade-your-ad-fs-farm-to-windows-server-2019-farm-behavior-level"></a>Para actualizar el nivel de comportamiento de la granja de servidores de AD FS a Windows Server 2019

1. Con Administrador del servidor, instale el rol de Servicios de federación de Active Directory (AD FS) en Windows Server 2019

2. Mediante el Asistente para configuración de AD FS, una el nuevo servidor de Windows Server 2019 a la granja de AD FS existente.

![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_1.png)

3. En el servidor de Federación de Windows Server 2019, abra Administración de AD FS. Tenga en cuenta que las capacidades de administración no están disponibles porque este servidor de Federación no es el servidor principal.

![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_3.png)

4. En el servidor de Windows Server 2019, abra una ventana de comandos de PowerShell con privilegios elevados y ejecute el siguiente cmdlet:

```PowerShell
Set-AdfsSyncProperties -Role PrimaryComputer
```

![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_4.png)

5. En el servidor de AD FS que se configuró previamente como principal, abra una ventana de comandos de PowerShell con privilegios elevados y ejecute el siguiente cmdlet:

```PowerShell
Set-AdfsSyncProperties -Role SecondaryComputer -PrimaryComputerName {FQDN}
```

![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_5.png)

6. Ahora, en el servidor de Federación de Windows Server 2016, abra Administración de AD FS. Tenga en cuenta que ahora todas las capacidades de administración aparecen porque el rol principal se ha transferido a este servidor.

![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_6.png)

7. Si va a actualizar una granja AD FS 2012 R2 a 2016 o 2019, la actualización de la granja de servidores requiere que el esquema de AD tenga como mínimo el nivel 85.  Para actualizar el esquema, con el disco de instalación de Windows Server 2016, abra un símbolo del sistema y navegue hasta el directorio support\adprep Ejecute lo siguiente: `adprep /forestprep`

![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_7.png)

Una vez que se complete la ejecución `adprep/domainprep`

> [!NOTE]
> Antes de ejecutar el paso siguiente, asegúrese de que Windows Server esté actualizado ejecutando Windows Update desde la configuración. Continúa este proceso hasta que no se necesiten actualizaciones.

![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_8.png)

8. Ahora, en el servidor de Windows Server 2016, abra PowerShell y ejecute el siguiente cmdlet:


> [!NOTE]
> Todos los servidores 2012 R2 deben quitarse de la granja antes de ejecutar el siguiente paso.

```PowerShell
Invoke-AdfsFarmBehaviorLevelRaise
```

![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_9.png)

9. Cuando se le solicite, escriba Y. Esto comenzará a elevar el nivel. Una vez completado esto, habrá generado correctamente el FBL.

![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_10.png)

10. Ahora, si va a la administración de AD FS, verá que se han agregado nuevas funcionalidades para la versión más reciente AD FS

![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_12.png)

11. Del mismo modo, puede usar el cmdlet de PowerShell: `Get-AdfsFarmInformation` para mostrar el FBL actual.

![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_13.png)

12. Para actualizar los servidores WAP al nivel más reciente, en cada proxy de aplicación Web, vuelva a configurar el WAP ejecutando el siguiente cmdlet de PowerShell en una ventana con privilegios elevados:

```PowerShell
$trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred
```

Quite los servidores antiguos del clúster y mantenga solo los servidores WAP que ejecutan la versión más reciente del servidor, que se reconfiguró anteriormente, mediante la ejecución del siguiente cmdlet de PowerShell.

```PowerShell
Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
```

Compruebe la configuración de WAP ejecutando el cmdlet Get-WebApplicationProxyConfiguration. El ConnectedServersName reflejará el servidor que se ejecuta desde el comando anterior.

```PowerShell
Get-WebApplicationProxyConfiguration
```
Para actualizar el ConfigurationVersion de los servidores WAP, ejecute el siguiente comando de PowerShell.

```PowerShell
Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
```

Esto completará la actualización de los servidores WAP.


> [!NOTE] 
> Existe un problema de PRT conocido en AD FS 2019 si se realiza Windows Hello para empresas con una confianza de certificado híbrido. Puede encontrar este error en los registros de eventos de administración de ADFS: solicitud de OAuth no válida recibida. El cliente "NOMBRE" no tiene permiso para acceder al recurso con el ámbito "ugs". Para corregir este error: 
> 1. Inicia la consola de administración de AD FS. Ve a "Servicios > Descripciones de ámbito"
> 2. Haz clic con el botón derecho en "Descripciones de ámbito" y selecciona "Agregar descripción de ámbito"
> 3. Para el nombre, escribe "ugs" y haz clic en Aplicar > Aceptar
> 4. Inicia PowerShell como administrador.
> 5. Ejecuta el comando "Get-AdfsApplicationPermission". Busca el elemento ScopeNames: {openid, aza} que contenga ClientRoleIdentifier. Toma nota del elemento ObjectIdentifier.
> 6. Ejecuta el comando "Set-AdfsApplicationPermission -TargetIdentifier <ObjectIdentifier del paso 5> -AddScope "ugs"
> 7. Reinicia el servicio ADFS.
> 8. En el cliente: reinicie el cliente. Se pedirá al usuario que aprovisione WHPB.
> 9. Si la ventana de aprovisionamiento no aparece, tienes que recopilar los registros de seguimiento de NGC y solucionar el problema.
