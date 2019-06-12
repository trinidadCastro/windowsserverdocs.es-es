---
title: Implementar espacios de almacenamiento directo
ms.prod: windows-server-threshold
manager: eldenc
ms.author: stevenek
ms.technology: storage-spaces
ms.topic: get-started-article
ms.assetid: 20fee213-8ba5-4cd3-87a6-e77359e82bc0
author: stevenek
ms.date: 06/07/2019
description: Instrucciones paso a paso para implementar almacenamiento definido por software con espacios de almacenamiento directo en Windows Server como infraestructuras hiperconvergidas o infraestructura convergente de (también conocido como desagregada).
ms.localizationpriority: medium
ms.openlocfilehash: a4159c85be23025ef57084b47dcc77d4f749888f
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812359"
---
# <a name="deploy-storage-spaces-direct"></a>Implementar espacios de almacenamiento directo

> Se aplica a: Windows Server 2019, Windows Server 2016

Este tema proporciona instrucciones paso a paso para implementar [espacios de almacenamiento directo](storage-spaces-direct-overview.md).

> [!Tip]
> ¿Desea para adquirir Hyper-Converged infraestructura? Microsoft recomienda adquirir una solución validada de hardware y software de nuestros asociados, que se incluyen los procedimientos y herramientas de implementación. Estas soluciones son diseñadas, ensamblar y validadas con respecto a nuestra arquitectura de referencia para garantizar la compatibilidad y confiabilidad, por lo que ponerse en marcha rápidamente. Para las soluciones de Windows Server 2019, visite la [sitio Web de soluciones de Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci). Para las soluciones de Windows Server 2016, obtenga más información en [definido por el Software de Windows Server](https://microsoft.com/wssd).

> [!Tip]
> Puede usar máquinas virtuales de Hyper-V, incluida en Microsoft Azure, a [evaluar espacios de almacenamiento directo sin hardware](storage-spaces-direct-in-vm.md). También puede revisar la práctica [scripts de implementación de Windows Server rápida laboratorio](https://aka.ms/wslab), que se utiliza para fines de aprendizaje.

## <a name="before-you-start"></a>Antes de empezar

Revise el [requisitos de hardware de espacios de almacenamiento directo](Storage-Spaces-Direct-Hardware-Requirements.md) y consulte Temas de este documento para familiarizarse con el enfoque general y notas importantes asociadas con algunos pasos.

Recopile la siguiente información:

- **Opción de implementación.** Admite espacios de almacenamiento directo [dos opciones de implementación: hiperconvergido y convergente](storage-spaces-direct-overview.md#deployment-options), también conocido como desagregada. Familiarícese con las ventajas de cada uno para decidir cuál es la adecuada para usted. Los pasos 1-3 siguientes se aplican a ambas opciones de implementación. Paso 4 solo es necesario para la implementación convergente.

- **Nombres de servidor.** Familiarizarse con las directivas de nomenclatura de equipos, archivos, rutas de acceso y otros recursos de su organización. Deberá aprovisionar varios servidores, cada uno con nombres únicos.

- **Nombre de dominio.** Familiarizarse con las directivas de su organización para los nombres de dominio y la unión de dominio.  Que puedas los servidores a su dominio, y deberá especificar el nombre de dominio. 

- **Red con RDMA.** Hay dos tipos de protocolos RDMA: iWarp y RoCE. Tenga en cuenta que los adaptadores de red usan, y si RoCE, tenga en cuenta también la versión (v1 o v2). Para RoCE, tenga en cuenta también el modelo del conmutador top of rack.

- **IDENTIFICADOR DE VLAN.** Tenga en cuenta el identificador de VLAN que se usará para los adaptadores de red de administración del sistema operativo en los servidores, si existe. Podrá obtenerlo del administrador de red.

## <a name="step-1-deploy-windows-server"></a>Paso 1: Implementar Windows Server

### <a name="step-11-install-the-operating-system"></a>Paso 1.1: Instalar el sistema operativo

El primer paso es instalar a Windows Server en todos los servidores que se incluirán en el clúster. Espacios de almacenamiento directo requiere Windows Server 2016 Datacenter Edition. Puede usar la opción de instalación Server Core o servidor con experiencia de escritorio.

Cuando se instala mediante el Asistente para instalación de Windows Server, puede elegir entre *Windows Server* (que hace referencia a Server Core) y *Windows Server (servidor con experiencia de escritorio)* , que es equivalente a de la *completa* opción de instalación disponible en Windows Server 2012 R2. Si no elige, obtendrá la opción de instalación Server Core. Para obtener más información, consulte [opciones de instalación para Windows Server 2016](../../get-started/Windows-Server-2016.md).

### <a name="step-12-connect-to-the-servers"></a>Paso 1.2: Conectarse a los servidores

Esta guía centra la opción de instalación Server Core y de implementar y administrar remotamente desde un sistema de administración independiente, que debe tener:

- Windows Server 2016 con las mismas actualizaciones que los servidores de administración
- Conectividad de red a los servidores que administra
- Unido al mismo dominio o un dominio de plena confianza
- Herramientas de administración remota del servidor (RSAT) y módulos de PowerShell para Hyper-V y clústeres de conmutación por error. Las herramientas RSAT y los módulos de PowerShell están disponibles en Windows Server y pueden instalarse sin necesidad de instalar otras características. También puede instalar el [herramientas de administración remota del servidor](https://www.microsoft.com/download/details.aspx?id=45520) en un equipo de administración de Windows 10.

En el sistema de administración, instale el clúster de conmutación por error y las herramientas de administración de Hyper-V. Esto puede hacerse a través del Administrador del servidor mediante el uso del Asistente para **agregar roles y características**. En la página **Características**, seleccione **Herramientas de administración remota del servidor** y, después, seleccione las herramientas que desea instalar.

Especifique la sesión de PS y use el nombre del servidor o la dirección IP del nodo al que desea conectarse. Le solicitará una contraseña después de ejecutar este comando, escriba la contraseña de administrador que especificó al configurar Windows.

   ```PowerShell
   Enter-PSSession -ComputerName <myComputerName> -Credential LocalHost\Administrator
   ```

   Este es un ejemplo de hacer lo mismo de forma que resulte más útil en scripts, en caso de deba hacer esto más de una vez:

   ```PowerShell
   $myServer1 = "myServer-1"
   $user = "$myServer1\Administrator"

   Enter-PSSession -ComputerName $myServer1 -Credential $user
   ```

> [!TIP]
> Si va a implementar remotamente desde un sistema de administración, podría obtener un error como *WinRM no puede procesar la solicitud.* Para solucionar este problema, use Windows PowerShell para agregar cada servidor a la lista de Hosts de confianza en el equipo de administración:  
>   
> `Set-Item WSMAN:\Localhost\Client\TrustedHosts -Value Server01 -Force`
>  
> Nota: la lista de hosts de confianza admite caracteres comodín, como `Server*`.
>
> Para ver la lista de Hosts de confianza, escriba `Get-Item WSMAN:\Localhost\Client\TrustedHosts`.  
>   
> Para vaciar la lista, escriba `Clear-Item WSMAN:\Localhost\Client\TrustedHost`.  

### <a name="step-13-join-the-domain-and-add-domain-accounts"></a>Paso 1.3: Unirse al dominio y agregar cuentas de dominio

Hasta ahora ha configurado los servidores individuales con la cuenta de administrador local, `<ComputerName>\Administrator`.

Para administrar espacios de almacenamiento directo, deberá unir los servidores a un dominio y usar una cuenta de dominio de Active Directory Domain Services que se encuentra en el grupo de administradores en todos los servidores.

En el sistema de administración, abra una consola de PowerShell con privilegios de administrador. Use `Enter-PSSession` para conectarse a cada servidor y ejecute el cmdlet siguiente, sustituyendo su propio nombre de equipo, nombre de dominio y las credenciales de dominio:

```PowerShell  
Add-Computer -NewName "Server01" -DomainName "contoso.com" -Credential "CONTOSO\User" -Restart -Force  
```

Si la cuenta de administrador de almacenamiento no es un miembro del grupo Admins. del dominio, agregue la cuenta de administrador de almacenamiento para el grupo de administradores local en cada nodo: o mejor aún, agregue el grupo que se usa para los administradores de almacenamiento. Puede usar el siguiente comando (o escribir una función de Windows PowerShell para hacerlo, consulte [usar PowerShell para agregar usuarios del dominio a un grupo Local](http://blogs.technet.com/b/heyscriptingguy/archive/2010/08/19/use-powershell-to-add-domain-users-to-a-local-group.aspx) para obtener más información):

```
Net localgroup Administrators <Domain\Account> /add
```

### <a name="step-14-install-roles-and-features"></a>Paso 1.4: Instalar roles y características

El siguiente paso es instalar roles de servidor en todos los servidores. Puede hacerlo mediante el uso de [Windows Admin Center](../../manage/windows-admin-center/use/manage-servers.md), [administrador del servidor](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)), o PowerShell. Estos son los roles para instalar:

- Clústeres de conmutación por error
- Hyper-V
- Servidor de archivos (si desea hospedar los recursos compartidos de archivos, por ejemplo, para una implementación convergida)
- Protocolo de puente del centro de datos (si usas RoCEv2 en lugar de adaptadores de red iWARP)
- RSAT-Clustering-PowerShell
- Hyper-V-PowerShell

Para instalar a través de PowerShell, use el [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature) cmdlet. Puede usarlo en un único servidor similar al siguiente:

```PowerShell
Install-WindowsFeature -Name "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"
```

Para ejecutar el comando en todos los servidores del clúster al mismo tiempo, use este pequeño script de modificación de la lista de variables al principio de la secuencia de comandos para ajustarse a su entorno.

```PowerShell
# Fill in these variables with your values
$ServerList = "Server01", "Server02", "Server03", "Server04"
$FeatureList = "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"

# This part runs the Install-WindowsFeature cmdlet on all servers in $ServerList, passing the list of features into the scriptblock with the "Using" scope modifier so you don't have to hard-code them here.
Invoke-Command ($ServerList) {
    Install-WindowsFeature -Name $Using:Featurelist
}
```

## <a name="step-2-configure-the-network"></a>Paso 2: Configurar la red

Si va a implementar espacios de almacenamiento directo dentro de máquinas virtuales, omita esta sección.

Espacios de almacenamiento directo requiere gran ancho de banda, baja latencia de red entre los servidores del clúster. Redes de al menos 10 GbE es necesario y se recomienda el acceso a memoria directa remota (RDMA). Puede usar ya sea iWARP o RoCE, siempre tiene el logotipo de Windows Server 2016, pero iWARP es normalmente más fácil de configurar.

> [!Important]
> Dependiendo de su equipo de red y especialmente con RoCE v2, puede requerirse una configuración del conmutador top of rack. Configuración del conmutador correcto es importante garantizar la confiabilidad y rendimiento de espacios de almacenamiento directo.

Windows Server 2016 presenta incrustado en el conmutador teaming (SET) en el conmutador virtual de Hyper-V. Esto permite a los mismos puertos NIC físicos que se usará para todo el tráfico de red al usar RDMA, lo que reduce el número de puertos NIC físicos necesarios. Para espacios de almacenamiento directo, se recomienda incrustado en el conmutador de formación de equipos.

Para que obtener instrucciones para configurar redes para espacios de almacenamiento directo, consulte [convergente NIC de Windows Server 2016 y Guía de implementación de invitado RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/S2D%20WS2016_ConvergedNIC_Configuration.docx).

## <a name="step-3-configure-storage-spaces-direct"></a>Paso 3: Configurar Espacios de almacenamiento directos

Los pasos siguientes se realizan en un sistema de administración que tenga la misma versión que los servidores que se están configurando. Los pasos siguientes deben no se puede ejecutar de forma remota mediante una sesión de PowerShell, pero en su lugar se ejecutan en una sesión de PowerShell local en el sistema de administración, con permisos administrativos.

### <a name="step-31-clean-drives"></a>Paso 3.1: Limpiar unidades

Antes de habilitar espacios de almacenamiento directo, asegúrese de que las unidades de disco están vacíos: no hay particiones antiguas u otros datos. Ejecute el script siguiente, sustituyendo los nombres de equipo, para quitar todas las particiones antiguas u otros datos.

> [!Warning]
> Este script quitará de forma permanente todos los datos de las unidades que no sean la unidad de arranque del sistema operativo.

```PowerShell
# Fill in these variables with your values
$ServerList = "Server01", "Server02", "Server03", "Server04"

Invoke-Command ($ServerList) {
    Update-StorageProviderCache
    Get-StoragePool | ? IsPrimordial -eq $false | Set-StoragePool -IsReadOnly:$false -ErrorAction SilentlyContinue
    Get-StoragePool | ? IsPrimordial -eq $false | Get-VirtualDisk | Remove-VirtualDisk -Confirm:$false -ErrorAction SilentlyContinue
    Get-StoragePool | ? IsPrimordial -eq $false | Remove-StoragePool -Confirm:$false -ErrorAction SilentlyContinue
    Get-PhysicalDisk | Reset-PhysicalDisk -ErrorAction SilentlyContinue
    Get-Disk | ? Number -ne $null | ? IsBoot -ne $true | ? IsSystem -ne $true | ? PartitionStyle -ne RAW | % {
        $_ | Set-Disk -isoffline:$false
        $_ | Set-Disk -isreadonly:$false
        $_ | Clear-Disk -RemoveData -RemoveOEM -Confirm:$false
        $_ | Set-Disk -isreadonly:$true
        $_ | Set-Disk -isoffline:$true
    }
    Get-Disk | Where Number -Ne $Null | Where IsBoot -Ne $True | Where IsSystem -Ne $True | Where PartitionStyle -Eq RAW | Group -NoElement -Property FriendlyName
} | Sort -Property PsComputerName, Count
```

El resultado será similar al siguiente, donde **recuento** es el número de unidades de cada modelo en cada servidor:

```
Count Name                          PSComputerName
----- ----                          --------------
4     ATA SSDSC2BA800G4n            Server01
10    ATA ST4000NM0033              Server01
4     ATA SSDSC2BA800G4n            Server02
10    ATA ST4000NM0033              Server02
4     ATA SSDSC2BA800G4n            Server03
10    ATA ST4000NM0033              Server03
4     ATA SSDSC2BA800G4n            Server04
10    ATA ST4000NM0033              Server04
```

### <a name="step-32-validate-the-cluster"></a>Paso 3.2: Validar el clúster

En este paso, ejecutará la herramienta de validación de clúster para asegurarse de que los nodos de servidor están configurados correctamente para crear un clúster con espacios de almacenamiento directo. Cuando la validación de clúster (`Test-Cluster`) es ejecutar antes de crear el clúster, ejecute las pruebas que comprueban que la configuración es adecuada para que funcione correctamente como un clúster de conmutación por error. El ejemplo siguiente usa el `-Include` se especifican el parámetro y, a continuación, las categorías de pruebas. Esto garantiza que se incluyen en la validación las pruebas específicas de Espacios de almacenamiento directo.

Utilice el siguiente comando de PowerShell para validar un conjunto de servidores para su uso como un clúster de Espacios de almacenamiento directo.

```PowerShell
Test-Cluster –Node <MachineName1, MachineName2, MachineName3, MachineName4> –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
```

### <a name="step-33-create-the-cluster"></a>Paso 3.3: Crear el clúster

En este paso, creará un clúster con los nodos que han validado para la creación del clúster en el paso anterior mediante el siguiente cmdlet de PowerShell.

Al crear el clúster, obtendrá una advertencia que indica: "se han producido problemas al crear el rol en clúster que puede evitar que se inicie. Para obtener más información, consulte el archivo de informe siguiente." Puede omitir con seguridad esta advertencia. Es debido a no hay ningún disco disponible para el cuórum de clúster. Se recomienda configurar un testigo del recurso compartido de archivos o un testigo en la nube después de crear el clúster.

> [!Note]
> Si los servidores están usando direcciones IP estáticas, modifique el siguiente comando para reflejar la dirección IP estática agregando el parámetro siguiente y especificando la dirección IP: -StaticAddress &lt;X.X.X.X&gt;.
> En el siguiente comando, el marcador de posición ClusterName debe reemplazarse por un nombre netbios que sea único y con 15 caracteres como máximo.
> ```PowerShell
> New-Cluster –Name <ClusterName> –Node <MachineName1,MachineName2,MachineName3,MachineName4> –NoStorage
> ```

Después de crear el clúster, puede tardar tiempo la replicación de la entrada DNS para el nombre del clúster. El tiempo depende del entorno y de la configuración de replicación de DNS. Si la resolución del clúster no se realiza correctamente, en la mayoría de los casos puede solucionarlo correctamente mediante el uso del nombre de la máquina de un nodo que sea un miembro activo del clúster que pueda utilizarse en lugar del nombre de clúster.

### <a name="step-34-configure-a-cluster-witness"></a>Paso 3.4: Configurar a un testigo de clúster

Se recomienda configurar a un testigo para el clúster, por lo que los clústeres con tres o más servidores pueden resistir dos servidores con errores o sin conexión. Una implementación de dos servidores requiere un testigo de clúster, en caso contrario, cualquier servidor que está quedándose sin conexión hace que el otro tampoco esté disponible. Con estos sistemas puede usar un recurso compartido de archivos como testigo o utilizar un testigo en la nube. 

Para obtener más información, consulta los temas siguientes:

- [Configurar y administrar el cuórum](../../failover-clustering/manage-cluster-quorum.md)
- [Implementación de un testigo en la nube para un clúster de conmutación por error](../../failover-clustering/deploy-cloud-witness.md)

### <a name="step-35-enable-storage-spaces-direct"></a>Paso 3.5: Habilitar Espacios de almacenamiento directos

Después de crear el clúster, use el `Enable-ClusterStorageSpacesDirect` cmdlet de PowerShell que poner el sistema de almacenamiento en el modo de espacios de almacenamiento directo y hacer lo siguiente automáticamente:

-   **Cree un grupo:** Crea un único grupo grande que tiene un nombre como "S2D on Cluster1".

-   **Configura las cachés de espacios de almacenamiento directo:** Si hay más de un medio de tipo (unidad) disponible para su uso de espacios de almacenamiento directo, habilita el más rápido como dispositivo de memoria caché (lectura y escritura en la mayoría de los casos)

-   **Niveles:** Crea dos niveles como niveles predeterminados. Uno se denomina "Capacidad" y otro se denomina "Rendimiento". El cmdlet analiza los dispositivos y configura cada nivel con la mezcla de tipos de dispositivos y resistencia.

Desde el sistema de administración, en una ventana de comandos de PowerShell abierta con privilegios de administrador, inicie el siguiente comando. El nombre del clúster es el nombre del clúster que creó en los pasos anteriores. Si este comando se ejecuta localmente en uno de los nodos, no es necesario el parámetro -CimSession.

```PowerShell
Enable-ClusterStorageSpacesDirect –CimSession <ClusterName>
```

Para habilitar Espacios de almacenamiento directo con el comando anterior, también puede utilizar el nombre de nodo en lugar del nombre de clúster. Con el nombre de nodo puede ser más confiable debido a los retrasos de replicación de DNS que puedan surgir con el nombre de clúster recién creado.

Cuando finalice este comando, que puede tardar varios minutos, el sistema estará listo para crear volúmenes.

### <a name="step-36-create-volumes"></a>Paso 3.6: Crear volúmenes

Se recomienda usar el `New-Volume` cmdlet tal como proporciona la experiencia más sencilla y rápida. Este cmdlet crea automáticamente por sí solo el disco virtual, lo particiona y formatea, crea el volumen con nombre coincidente y lo agrega a los volúmenes compartidos de clúster: todo en un paso sencillo.

Para obtener más información, echa un vistazo a [Crear volúmenes en Espacios de almacenamiento directo](create-volumes.md).

### <a name="step-37-optionally-enable-the-csv-cache"></a>Paso 3.7: Opcionalmente, habilitar la caché de CSV

Si lo desea, puede habilitar la caché de volumen (CSV) de clúster compartido utilizar la memoria del sistema (RAM) como una caché de nivel de bloque de escritura a través de las operaciones de lectura que ya no están almacenados en caché por el Administrador de caché de Windows. Esto puede mejorar el rendimiento de aplicaciones como Hyper-V. La caché de CSV puede mejorar el rendimiento de las solicitudes de lectura y también es útil para escenarios de servidor de archivos de escalabilidad horizontal.

Habilitación de la caché CSV reduce la cantidad de memoria disponible para ejecutar máquinas virtuales en un clúster hiperconvergido, por lo que tendrá que equilibrar el rendimiento de almacenamiento con la memoria disponible para los discos duros virtuales.

Para establecer el tamaño de la caché de CSV, abra una sesión de PowerShell en el sistema de administración con una cuenta que tenga permisos de administrador en el clúster de almacenamiento y, a continuación, use este script, cambiando el `$ClusterName` y `$CSVCacheSize` variables según corresponda (Esto en el ejemplo se establece una memoria caché CSV de 2 GB por servidor):

```PowerShell
$ClusterName = "StorageSpacesDirect1"
$CSVCacheSize = 2048 #Size in MB

Write-Output "Setting the CSV cache..."
(Get-Cluster $ClusterName).BlockCacheSize = $CSVCacheSize

$CSVCurrentCacheSize = (Get-Cluster $ClusterName).BlockCacheSize
Write-Output "$ClusterName CSV cache size: $CSVCurrentCacheSize MB"
```

Para obtener más información, consulte [utilizando los CSV en la memoria memoria caché de lectura](csv-cache.md).

### <a name="step-38-deploy-virtual-machines-for-hyper-converged-deployments"></a>Paso 3.8: Implementar máquinas virtuales para las implementaciones hiperconvergida

Si va a implementar un clúster hiperconvergido, el último paso es aprovisionar máquinas virtuales en el clúster de espacios de almacenamiento directo.

Se deben almacenar los archivos de la máquina virtual en el espacio de nombres de sistemas CSV (ejemplo: c:\\ClusterStorage\\Volume1) al igual que máquinas virtuales en clúster en clústeres de conmutación por error.

Puede usar el cuadro de herramientas u otras herramientas para administrar el almacenamiento y máquinas virtuales, como System Center Virtual Machine Manager.

## <a name="step-4-deploy-scale-out-file-server-for-converged-solutions"></a>Paso 4: Implementar Scale-Out File Server soluciones convergente

Si va a implementar una solución convergente, el siguiente paso es crear una instancia de servidor de archivos de escalabilidad horizontal y algunos recursos compartidos de archivos de instalación. Si va a implementar un clúster hiperconvergido: ya habrá terminado y no necesita esta sección.

### <a name="step-41-create-the-scale-out-file-server-role"></a>Paso 4.1: Crear el rol de servidor de archivos de escalabilidad horizontal

El siguiente paso en la configuración de los servicios de clúster para el servidor de archivos es crear el rol de servidor de archivos en clúster, que es cuando se crea la instancia de Scale-Out File Server en el que se hospedan los recursos compartidos de archivos disponibles continuamente.

#### <a name="to-create-a-scale-out-file-server-role-by-using-server-manager"></a>Para crear un rol de servidor de archivos de escalabilidad horizontal mediante el administrador del servidor

1. En el Administrador de clústeres de conmutación por error, seleccione el clúster, vaya a **Roles**y, a continuación, haga clic en **configurar rol...** .<br>Aparece el Asistente para alta disponibilidad.
2. En el **Seleccionar rol** página, haga clic en **servidor de archivos**.
3. En el **tipo de servidor de archivos** página, haga clic en **Scale-Out File Server datos de la aplicación**.
4. En el **punto de acceso cliente** página, escriba un nombre para el servidor de archivos de escalabilidad horizontal.
5. Compruebe que el rol se ha configurado correctamente, vaya a **Roles** y confirmar que el **estado** columna muestra **ejecutando** junto a la función de servidor de archivos en clúster que creó, como se muestra en la figura 1.

   ![Captura de pantalla del Administrador de clústeres de conmutación por error que muestra el servidor de archivos de escalabilidad horizontal](media/Hyper-converged-solution-using-Storage-Spaces-Direct-in-Windows-Server-2016/SOFS_in_FCM.png "Administrador de clústeres de conmutación por error que muestra el servidor de archivos de escalabilidad horizontal")

    **Figura 1** Administrador de clústeres de conmutación por error que muestra el servidor de archivos de escalabilidad horizontal con el estado de ejecución

> [!NOTE]
>  Después de crear el rol en clúster, podría haber alguna red demoras de propagación que podrían impedir la creación de recursos compartidos de archivos en él durante unos minutos, o puede ser más largo.  
  
#### <a name="to-create-a-scale-out-file-server-role-by-using-windows-powershell"></a>Para crear un rol de servidor de archivos de escalabilidad horizontal mediante Windows PowerShell

 En una sesión de Windows PowerShell que está conectada el clúster de servidor de archivos, escriba los siguientes comandos para crear el rol de servidor de archivos de escalabilidad horizontal, cambiar *FSCLUSTER* para que coincida con el nombre del clúster, y *SOFS* para que coincida con el nombre que desea asignar al rol de servidor de archivos de escalabilidad horizontal:

```PowerShell
Add-ClusterScaleOutFileServerRole -Name SOFS -Cluster FSCLUSTER
```

> [!NOTE]
>  Después de crear el rol en clúster, podría haber alguna red demoras de propagación que podrían impedir la creación de recursos compartidos de archivos en él durante unos minutos, o puede ser más largo. Si el rol de SOFS produce un error inmediatamente y no se inicia, es posible porque el objeto de equipo del clúster no tiene permiso para crear una cuenta de equipo para el rol de SOFS. Para obtener ayuda, consulte esta entrada de blog: [Escalabilidad horizontal archivo Server rol no se inicia con el Id. de evento 1205, 1069 y 1194](http://www.aidanfinn.com/?p=14142).

### <a name="step-42-create-file-shares"></a>Paso 4.2: Crear recursos compartidos de archivos

Después de que ha creado los discos virtuales y agregarlos a CSV, es hora de crear recursos compartidos de archivos en ellos: un recurso compartido por CSV por disco virtual. System Center Virtual Machine Manager (VMM) es probablemente la forma handiest hacer esto porque lo controla los permisos para usted, pero si no lo tiene en su entorno, puede usar Windows PowerShell para automatizar la implementación de parcialmente.

Use los scripts incluidos en el [configuración del recurso compartido SMB para cargas de trabajo de Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) secuencias de comandos que automatiza el proceso de creación de grupos y recursos compartidos de parcialmente. Se ha escrito para cargas de trabajo de Hyper-V, por lo que si va a implementar otras cargas de trabajo, es posible que deba modificar la configuración o realizar pasos adicionales después de crear los recursos compartidos. Por ejemplo, si usa Microsoft SQL Server, la cuenta de servicio de SQL Server debe tener control total sobre el recurso compartido y el sistema de archivos.

> [!NOTE]
>  Tendrá que actualizar la pertenencia al grupo al agregar nodos de clúster a menos que use System Center Virtual Machine Manager para crear los recursos compartidos.

Para crear recursos compartidos de archivos mediante el uso de scripts de PowerShell, realice lo siguiente:

1. Descargue los scripts incluidos en [configuración del recurso compartido SMB para cargas de trabajo de Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) a uno de los nodos del clúster del servidor de archivos.
2. Abra una sesión de Windows PowerShell con credenciales de administrador de dominio en el sistema de administración y, a continuación, use el siguiente script para crear un grupo de Active Directory para los objetos de equipo de Hyper-V, cambiando los valores de las variables más apropiados para su entorno:

    ```PowerShell
    # Replace the values of these variables
    $HyperVClusterName = "Compute01"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of script itself
    CD $ScriptFolder
    .\ADGroupSetup.ps1 -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName -HyperVClusterName $HyperVClusterName
    ```
3. Abra una sesión de Windows PowerShell con credenciales de administrador en uno de los nodos de almacenamiento y, a continuación, use el siguiente script para crear recursos compartidos para cada CSV y conceder permisos administrativos para los recursos compartidos para el grupo Admins. del dominio y el clúster de proceso.

    ```PowerShell
    # Replace the values of these variables
    $StorageClusterName = "StorageSpacesDirect1"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $SOFSName = "SOFS"
    $SharePrefix = "Share"
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of the script itself
    CD $ScriptFolder
    Get-ClusterSharedVolume -Cluster $StorageClusterName | ForEach-Object
    {
        $ShareName = $SharePrefix + $_.SharedVolumeInfo.friendlyvolumename.trimstart("C:\ClusterStorage\Volume")
        Write-host "Creating share $ShareName on "$_.name "on Volume: " $_.SharedVolumeInfo.friendlyvolumename
        .\FileShareSetup.ps1 -HyperVClusterName $StorageClusterName -CSVVolumeNumber $_.SharedVolumeInfo.friendlyvolumename.trimstart("C:\ClusterStorage\Volume") -ScaleOutFSName $SOFSName -ShareName $ShareName -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName
    }
    ```

### <a name="step-43-enable-kerberos-constrained-delegation"></a>Delegación restringida de Kerberos de habilitar paso 4.3

Para configurar la delegación restringida de Kerberos para administración remota del escenario y aumentar la seguridad de migración en vivo, uno de los nodos del clúster de almacenamiento, usan el script KCDSetup.ps1 incluido en [configuración del recurso compartido SMB para cargas de trabajo de Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a). Este es un pequeño contenedor para la secuencia de comandos:

```PowerShell
$HyperVClusterName = "Compute01"
$ScaleOutFSName = "SOFS"
$ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

CD $ScriptFolder
.\KCDSetup.ps1 -HyperVClusterName $HyperVClusterName -ScaleOutFSName $ScaleOutFSName -EnableLM
```

## <a name="next-steps"></a>Pasos siguientes

Después de implementar el servidor de archivos en clúster, se recomienda probar el rendimiento de la solución con cargas de trabajo sintéticas antes de poner las cargas de trabajo real. Esto le permite confirmar que la solución funcione correctamente y solucione cualquier problema persistente antes de agregar la complejidad de las cargas de trabajo. Para obtener más información, consulte [prueba almacenamiento espacios rendimiento utilizando cargas de trabajo sintéticas](https://technet.microsoft.com/library/dn894707.aspx).

## <a name="see-also"></a>Vea también

-   [Espacios de almacenamiento directo en Windows Server 2016](storage-spaces-direct-overview.md)
-   [Comprender la memoria caché en espacios de almacenamiento directo](understand-the-cache.md)
-   [Planificación de volúmenes en espacios de almacenamiento directo](plan-volumes.md)
-   [Tolerancia a errores de espacios de almacenamiento](storage-spaces-fault-tolerance.md)
-   [Espacios de almacenamiento directo de los requisitos de Hardware](Storage-Spaces-Direct-Hardware-Requirements.md)
-   [To RDMA, or not to RDMA – that is the question](https://blogs.technet.microsoft.com/filecab/2017/03/27/to-rdma-or-not-to-rdma-that-is-the-question/) (RDMA o no RDMA: esta es la cuestión) (blog de TechNet)
