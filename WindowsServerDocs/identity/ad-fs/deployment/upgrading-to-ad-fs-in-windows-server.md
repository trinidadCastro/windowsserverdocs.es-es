---
ms.assetid: 7671e0c9-faf0-40de-808a-62f54645f891
title: Actualización a AD FS en Windows Server 2016
description: ''
author: billmath
manager: femila
ms.date: 04/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 39c735e9dde0fd60c7eb9ccfe0af890bdc5a5950
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838326"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-using-a-wid-database"></a>Actualización a AD FS en Windows Server 2016 mediante una base de datos WID

>Se aplica a: Windows Server 2019, Windows Server 2016


## <a name="upgrading-a-windows-server-2012-r2-or-2016-ad-fs-farm-to-windows-server-2019"></a>Actualizar un Windows Server 2012 R2 o granja 2016 AD FS a Windows Server 2019 
El siguiente documento describe cómo actualizar la granja de AD FS para AD FS en Windows Server 2019 cuando se usa una base de datos WID.  

### <a name="ad-fs-farm-behavior-levels-fbl"></a>Niveles de AD FS granja comportamiento (FBL)  
En AD FS para Windows Server 2016, se introdujo el nivel de comportamiento de la granja de servidores (FBL). Esta es toda la granja de servidores de configuración que determina que el conjunto de características de AD FS puede usar. 

En la tabla siguiente se enumera los valores FBL por versión de Windows Server:
| Versión de Windows Server  | FBL | Nombre de base de datos de configuración de AD FS |
| ------------- | ------------- | ------------- |
| 2012 R2  | 1  | AdfsConfiguration |
| 2016  | 3  | AdfsConfigurationV3 |
| 2019  | 4  | AdfsConfigurationV4 |

> [!NOTE]  
> Actualizar el FBL crea una nueva base de datos de configuración de AD FS.  Consulte la tabla anterior para los nombres de la base de datos de configuración para cada versión de Windows Server AD FS y el valor FBL

### <a name="new-vs-upgraded-farms"></a>Nueva vs actualizó granjas de servidores
De forma predeterminada, el FBL en una nueva granja de AD FS coincide con el valor de la versión de Windows Server del primer nodo de granja de servidores instalado.  

Un servidor de AD FS de una versión posterior puede unirse a una granja de servidores de AD FS 2012 R2 o 2016 y la granja de servidores que funcionarán en el mismo FBL como los nodos existentes. Cuando haya varias versiones de Windows Server funciona en la misma granja de servidores en el valor FBL de la versión más antigua, se dice que la granja de servidores "mixed". Sin embargo, no podrá aprovechar las ventajas de las características de las versiones posteriores hasta que se genere el FBL. Con una granja de servidores mixto:  

-   Los administradores pueden agregar nuevos servidores de federación de 2019 de Windows Server a un servidor existente de Windows Server 2012 R2 o 2016 granja. Como resultado, la granja de servidores está en "modo mixto" y funciona en el mismo nivel de comportamiento de la granja de servidores como granja original. Para garantizar un comportamiento coherente en la granja de servidores, se no se pueden configurar las características de las nuevas versiones de Windows Server AD FS o utilizadas.  

- Antes de que se provoque el FBL, los administradores deben quitar los nodos de AD FS de las versiones anteriores de Windows Server de la granja de servidores.  En el caso de una granja WID, tenga en cuenta que esto requiere uno de la nueva tp de servidores de federación 2019 de Windows Server se promueve a la función del nodo principal en la granja de servidores.

-   Una vez que todos los servidores de federación en la granja de servidores están en la misma versión de Windows Server, se puede generar el FBL.  Como resultado, a continuación, se pueden configurar y usar las nuevas características de servidor de Windows de AD FS de 2019.

Tenga en cuenta que en el modo mixto de la granja de servidores, la granja de servidores de AD FS no es capaz de características nuevas ni funcionalidad presentada en AD FS en Windows Server 2019. Esto significa que las organizaciones que desean probar las nuevas características no pueden hacerlo hasta que se genere el FBL. Por lo que si su organización está considerando para probar las nuevas características antes de rasing el FBL, deberá implementar una granja de servidores independiente para hacer esto.  

El resto de la es el documento proporciona los pasos para agregar un servidor de federación de Windows Server 2019 al entorno de 2012 R2 o Windows Server 2016 y, a continuación, generar el FBL a Windows Server 2019. Estos pasos se realizaron en un entorno de pruebas que se describen en el diagrama arquitectónico siguiente.  

> [!NOTE]  
> Antes de poder mover a AD FS en Windows Server 2019 FBL, debe quitar todas las Windows Server 2016 o nodos de 2012 R2. Simplemente no se puede actualizar Windows Server 2016 o sistema operativo de 2012 R2 a Windows Server 2019 y hacer que se convertirá en un nodo de 2019. Debe quitarlo y reemplazarlo con un nuevo nodo de 2019.



##### <a name="to-upgrade-your-ad-fs-farm-to-windows-server-2019-farm-behavior-level"></a>Para actualizar la granja de AD FS a nivel de comportamiento de granja de servidores de Windows Server de 2019  

1.  Con el administrador del servidor, instale el rol de servicios de federación de Active Directory en el servidor de Windows 2019 

2.  Mediante el Asistente para configuración de AD FS, unir el servidor de Windows Server 2019 nuevo a la granja de AD FS existente.  

    ![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_1.png)  

3.  En el servidor de federación de Windows Server 2019, abra Administración de AD FS. Tenga en cuenta que las capacidades de administración no están disponibles porque este servidor de federación no es el servidor principal.  

    ![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_3.png)  

4.  En el servidor de Windows Server 2019, abra una ventana de comandos de PowerShell con privilegios elevados y ejecute el siguiente cmdlet: `Set-AdfsSyncProperties -Role PrimaryComputer`

    ![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_4.png)  

5.  En el servidor de AD FS que configuró anteriormente como principal, abra una ventana de comandos de PowerShell con privilegios elevados y ejecute el siguiente cmdlet: `Set-AdfsSyncProperties -Role SecondaryComputer -PrimaryComputerName {FQDN} `

    ![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_5.png)  

6.  En cada Proxy de aplicación Web, volver a configurar el WAP ejecutando el siguiente comando de PowerShell en una ventana con privilegios elevados:  
```powershell
$trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred  
```

7.  Ahora en el servidor de federación de Windows Server 2016 abra Administración de AD FS. Tenga en cuenta que ahora todas las funcionalidades de administración aparecen porque el rol principal se ha transferido a este servidor.  

    ![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_6.png)  

8.  Si va a actualizar una granja de AD FS 2012 R2 a 2016 o de 2019, la actualización de la granja de servidores requiere el esquema de AD para que sea al menos el nivel 85.  Para actualizar el esquema, los medios de instalación con Windows Server 2016, abra un símbolo del sistema y desplácese al directorio support\adprep. Ejecute lo siguiente:  `adprep /forestprep`

    ![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_7.png)  

    Una vez que completa una ejecución `adprep/domainprep`
    >[!NOTE]
    >Antes de ejecutar el siguiente paso, asegúrese de que Windows Server es actual mediante la ejecución de Windows Update desde configuración. Continúa este proceso hasta que no se necesiten actualizaciones. 
    > 
    
    ![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_8.png)  

9. Ahora en el servidor de Windows Server 2016 abra PowerShell y ejecute el siguiente cmdlet:
    >[!NOTE]
    > Todos los servidores de R2 2012 deben quitarse de la granja de servidores antes de ejecutar el paso siguiente.
 
    `Invoke-AdfsFarmBehaviorLevelRaise`  

    ![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_9.png)  

10. Cuando se le solicite, escriba Y. Esto comenzará la elevación del nivel. Cuando se complete correctamente que han surgido la FBL.  

    ![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_10.png)  

11. Ahora, si va a administración de AD FS, verá que las nuevas funcionalidades se han agregado para la versión posterior de AD FS 

    ![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_12.png)  

13. Del mismo modo, puede usar el cmdlet de PowerShell: `Get-AdfsFarmInformation` para mostrar el FBL actual.  

    ![actualizar](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_13.png)  
    

