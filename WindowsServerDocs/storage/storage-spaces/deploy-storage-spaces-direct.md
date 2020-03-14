---
title: Implementar espacios de almacenamiento directo
ms.prod: windows-server
manager: eldenc
ms.author: stevenek
ms.technology: storage-spaces
ms.topic: get-started-article
ms.assetid: 20fee213-8ba5-4cd3-87a6-e77359e82bc0
author: stevenek
ms.date: 06/07/2019
description: Instrucciones paso a paso para implementar el almacenamiento definido por software con Espacios de almacenamiento directo en Windows Server como una infraestructura hiperconvergida o una infraestructura convergente (también conocida como desagregada).
ms.localizationpriority: medium
ms.openlocfilehash: 60b29cbebb19cd8f1ce364d1eb7e920759375285
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79323297"
---
# <a name="deploy-storage-spaces-direct"></a>Implementar espacios de almacenamiento directo

> Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se proporcionan instrucciones paso a paso para implementar [espacios de almacenamiento directo](storage-spaces-direct-overview.md).

> [!Tip]
> ¿Desea adquirir una infraestructura hiperconvergida? Microsoft recomienda adquirir una solución de hardware/software validada de nuestros asociados, que incluye procedimientos y herramientas de implementación. Estas soluciones se diseñan, ensamblan y validan con nuestra arquitectura de referencia para garantizar la compatibilidad y la confiabilidad, de modo que pueda ponerse en marcha rápidamente. Para obtener soluciones de Windows Server 2019, visite el [sitio web de soluciones de hcl Azure Stack](https://azure.microsoft.com/overview/azure-stack/hci). En el caso de las soluciones de Windows Server 2016, obtenga más información en [Windows Server Software-Defined](https://microsoft.com/wssd).

> [!Tip]
> Puede usar máquinas virtuales de Hyper-V, incluido en Microsoft Azure, para [evaluar espacios de almacenamiento directo sin hardware](storage-spaces-direct-in-vm.md). También puede revisar los útiles [scripts de implementación rápida de Windows Server](https://aka.ms/wslab), que se usan con fines de entrenamiento.

## <a name="before-you-start"></a>Antes de empezar

Revise los [requisitos de hardware de espacios de almacenamiento directo](Storage-Spaces-Direct-Hardware-Requirements.md) y consulte este documento para familiarizarse con el enfoque general y con las notas importantes asociadas a algunos pasos.

Recopile la información siguiente:

- **Opción de implementación.** Espacios de almacenamiento directo admite [dos opciones de implementación: hiperconvergido y convergente](storage-spaces-direct-overview.md#deployment-options), también conocido como desagregado. Familiarícese con las ventajas de cada una de ellas para decidir cuál es la adecuada para usted. Los pasos 1-3 siguientes se aplican a ambas opciones de implementación. El paso 4 solo es necesario para la implementación convergente.

- **Nombres de servidor.** Familiarícese con las directivas de nomenclatura de su organización para equipos, archivos, rutas de acceso y otros recursos. Deberá aprovisionar varios servidores, cada uno con nombres únicos.

- **Nombre de dominio.** Familiarícese con las directivas de su organización para la combinación de nombres de dominio y dominios.  Unirá los servidores a su dominio y tendrá que especificar el nombre de dominio. 

- **Redes RDMA.** Hay dos tipos de protocolos RDMA: iWarp y RoCE. Observe cuál de los adaptadores de red usa y, si RoCE, tenga en cuenta también la versión (V1 o V2). En el caso de RoCE, tenga en cuenta también el modelo del conmutador para parte superior del rack.

- **IDENTIFICADOR DE VLAN.** Tenga en cuenta el identificador de VLAN que se usará para los adaptadores de red del sistema operativo de administración en los servidores, si hay alguno. Podrá obtenerlo del administrador de red.

## <a name="step-1-deploy-windows-server"></a>Paso 1: Implementar Windows Server

### <a name="step-11-install-the-operating-system"></a>Paso 1,1: instalar el sistema operativo

El primer paso es instalar Windows Server en todos los servidores que estarán en el clúster. Espacios de almacenamiento directo requiere Windows Server 2016 Datacenter Edition. Puede usar la opción de instalación Server Core o servidor con experiencia de escritorio.

Al instalar Windows Server mediante el Asistente para la instalación, puede elegir entre *Windows Server* (que hace referencia a Server Core) y *Windows Server (servidor con experiencia de escritorio)* , que es el equivalente de la opción de instalación *completa* disponible en Windows Server 2012 R2. Si no elige, obtendrá la opción de instalación Server Core. Para obtener más información, consulte [Opciones de instalación para Windows Server 2016](../../get-started/Windows-Server-2016.md).

### <a name="step-12-connect-to-the-servers"></a>Paso 1,2: conexión a los servidores

Esta guía se centra en la opción de instalación Server Core y en la implementación y administración de forma remota desde un sistema de administración independiente, que debe tener:

- Windows Server 2016 con las mismas actualizaciones que los servidores que está administrando
- Conectividad de red a los servidores que administra
- Unido al mismo dominio o a un dominio de plena confianza
- Herramientas de administración remota del servidor (RSAT) y módulos de PowerShell para Hyper-V y clústeres de conmutación por error. Las herramientas de RSAT y los módulos de PowerShell están disponibles en Windows Server y se pueden instalar sin necesidad de instalar otras características. También puede instalar el [herramientas de administración remota del servidor](https://www.microsoft.com/download/details.aspx?id=45520) en un equipo de administración de Windows 10.

En el sistema de administración, instale el clúster de conmutación por error y las herramientas de administración de Hyper-V. Esto puede hacerse a través del Administrador del servidor mediante el uso del Asistente para **agregar roles y características**. En la página **Características**, seleccione **Herramientas de administración remota del servidor** y, después, seleccione las herramientas que desea instalar.

Especifique la sesión de PS y use el nombre del servidor o la dirección IP del nodo al que desea conectarse. Se le solicitará una contraseña después de ejecutar este comando, escriba la contraseña de administrador que especificó al configurar Windows.

   ```PowerShell
   Enter-PSSession -ComputerName <myComputerName> -Credential LocalHost\Administrator
   ```

   Este es un ejemplo de cómo hacer lo mismo de una manera más útil en scripts, por si necesita hacer esto más de una vez:

   ```PowerShell
   $myServer1 = "myServer-1"
   $user = "$myServer1\Administrator"

   Enter-PSSession -ComputerName $myServer1 -Credential $user
   ```

> [!TIP]
> Si va a realizar la implementación remota desde un sistema de administración, es posible que reciba un error como *WinRM no puede procesar la solicitud.* Para solucionar este fin, use Windows PowerShell para agregar cada servidor a la lista de hosts de confianza del equipo de administración:  
>   
> `Set-Item WSMAN:\Localhost\Client\TrustedHosts -Value Server01 -Force`
>  
> Nota: la lista de hosts de confianza admite caracteres comodín, como `Server*`.
>
> Para ver la lista de hosts de confianza, escriba `Get-Item WSMAN:\Localhost\Client\TrustedHosts`.  
>   
> Para vaciar la lista, escriba `Clear-Item WSMAN:\Localhost\Client\TrustedHost`.  

### <a name="step-13-join-the-domain-and-add-domain-accounts"></a>Paso 1,3: unir el dominio y agregar cuentas de dominio

Hasta ahora ha configurado los servidores individuales con la cuenta de administrador local, `<ComputerName>\Administrator`.

Para administrar Espacios de almacenamiento directo, deberá unir los servidores a un dominio y usar una cuenta de dominio Active Directory Domain Services que esté en el grupo de administradores en cada servidor.

En el sistema de administración, abra una consola de PowerShell con privilegios de administrador. Use `Enter-PSSession` para conectarse a cada servidor y ejecute el siguiente cmdlet, sustituyendo el nombre del equipo, el nombre de dominio y las credenciales del dominio:

```PowerShell  
Add-Computer -NewName "Server01" -DomainName "contoso.com" -Credential "CONTOSO\User" -Restart -Force  
```

Si la cuenta de administrador de almacenamiento no es miembro del grupo Admins. del dominio, agregue su cuenta de administrador de almacenamiento al grupo de administradores locales en cada nodo, o bien, agregue el grupo que usa para los administradores de almacenamiento. Puede usar el siguiente comando (o escribir una función de Windows PowerShell para hacerlo; consulte [uso de PowerShell para agregar usuarios de dominio a un grupo local](https://blogs.technet.com/b/heyscriptingguy/archive/2010/08/19/use-powershell-to-add-domain-users-to-a-local-group.aspx) para obtener más información):

```
Net localgroup Administrators <Domain\Account> /add
```

### <a name="step-14-install-roles-and-features"></a>Paso 1,4: instalación de roles y características

El siguiente paso consiste en instalar roles de servidor en cada servidor. Para ello, puede usar el [centro de administración de Windows](../../manage/windows-admin-center/use/manage-servers.md), [Administrador del servidor](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)) o PowerShell. Estos son los roles que se van a instalar:

- Clústeres de conmutación por error
- Hyper-V
- Servidor de archivos (si desea hospedar recursos compartidos de archivos, como en el caso de una implementación convergente)
- Protocolo de puente del centro de datos (si usas RoCEv2 en lugar de adaptadores de red iWARP)
- RSAT-Clustering-PowerShell
- Hyper-V-PowerShell

Para instalar a través de PowerShell, use el cmdlet [install-WindowsFeature](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature) . Puede utilizarlo en un solo servidor como este:

```PowerShell
Install-WindowsFeature -Name "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"
```

Para ejecutar el comando en todos los servidores del clúster al mismo tiempo, use este poco de script y modifique la lista de variables al principio del script para que se ajuste a su entorno.

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

Si va a implementar Espacios de almacenamiento directo en máquinas virtuales, omita esta sección.

Espacios de almacenamiento directo requiere redes de baja latencia y alto ancho de banda entre los servidores del clúster. Se requiere al menos 10 GbE redes y se recomienda el acceso directo a memoria remota (RDMA). Puede usar iWARP o RoCE, siempre y cuando tenga el logotipo de Windows Server 2016, pero normalmente es más fácil de configurar.

> [!Important]
> Según el equipo de red, y especialmente con RoCE V2, es posible que se necesite cierta configuración del conmutador para parte superior del rack. La configuración del conmutador correcta es importante para garantizar la confiabilidad y el rendimiento de Espacios de almacenamiento directo.

Windows Server 2016 presenta la formación de equipos incrustados en el conmutador (SET) en el conmutador virtual de Hyper-V. Esto permite que se usen los mismos puertos de NIC físicos para todo el tráfico de red mientras se usa RDMA, lo que reduce el número de puertos de NIC físicos necesarios. Switch: se recomienda la formación de equipos incrustados para Espacios de almacenamiento directo.

Interconexiones de nodo conmutado o sin conmutador
- Cambiado: los conmutadores de red deben estar configurados correctamente para administrar el ancho de banda y el tipo de red. Si usa RDMA que implementa el protocolo RoCE, la configuración del dispositivo y del conmutador de red es aún más importante.
- Sin conmutador: los nodos se pueden interconectar mediante conexiones directas, evitando el uso de un modificador. Es necesario que todos los nodos tengan una conexión directa con todos los demás nodos del clúster.

Para obtener instrucciones sobre cómo configurar las funciones de red para Espacios de almacenamiento directo, consulte la [Guía de implementación convergente de NIC y de Windows Server 2016](https://github.com/Microsoft/SDN/blob/master/Diagnostics/S2D%20WS2016_ConvergedNIC_Configuration.docx).

## <a name="step-3-configure-storage-spaces-direct"></a>Paso 3: Configurar Espacios de almacenamiento directo

Los pasos siguientes se realizan en un sistema de administración que tenga la misma versión que los servidores que se están configurando. Los pasos siguientes no deben ejecutarse de forma remota mediante una sesión de PowerShell, sino que se ejecutan en una sesión local de PowerShell en el sistema de administración, con permisos administrativos.

### <a name="step-31-clean-drives"></a>Paso 3,1: limpiar unidades

Antes de habilitar Espacios de almacenamiento directo, asegúrese de que las unidades están vacías: no hay particiones antiguas u otros datos. Ejecute el siguiente script, sustituyendo los nombres de equipo, para quitar todas las particiones antiguas u otros datos.

> [!Warning]
> Este script quitará permanentemente todos los datos de las unidades que no sean la unidad de arranque del sistema operativo.

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

La salida tendrá un aspecto similar al siguiente, donde **recuento** es el número de unidades de cada modelo de cada servidor:

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

### <a name="step-32-validate-the-cluster"></a>Paso 3,2: validar el clúster

En este paso, ejecutará la herramienta de validación de clústeres para asegurarse de que los nodos del servidor están configurados correctamente para crear un clúster mediante Espacios de almacenamiento directo. Cuando se ejecuta la validación de clúster (`Test-Cluster`) antes de crear el clúster, se ejecutan las pruebas que comprueban que la configuración parece adecuada para funcionar correctamente como un clúster de conmutación por error. En el ejemplo siguiente se usa el parámetro `-Include` y, a continuación, se especifican las categorías de pruebas específicas. Esto garantiza que se incluyen en la validación las pruebas específicas de Espacios de almacenamiento directo.

Utilice el siguiente comando de PowerShell para validar un conjunto de servidores para su uso como un clúster de Espacios de almacenamiento directo.

```PowerShell
Test-Cluster –Node <MachineName1, MachineName2, MachineName3, MachineName4> –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
```

### <a name="step-33-create-the-cluster"></a>Paso 3,3: creación del clúster

En este paso, creará un clúster con los nodos que ha validado para la creación del clúster en el paso anterior mediante el siguiente cmdlet de PowerShell.

Al crear el clúster, obtendrá una advertencia que indica: "hubo problemas al crear el rol en clúster que puede impedir que se inicie. Para obtener más información, consulte el archivo de informe siguiente." Puede omitir esta advertencia de forma segura. Es debido a no hay ningún disco disponible para el cuórum de clúster. Se recomienda configurar un testigo del recurso compartido de archivos o un testigo en la nube después de crear el clúster.

> [!Note]
> Si los servidores están usando direcciones IP estáticas, modifique el siguiente comando para reflejar la dirección IP estática agregando el parámetro siguiente y especificando la dirección IP: -StaticAddress &lt;X.X.X.X&gt;.
> En el siguiente comando, el marcador de posición ClusterName debe reemplazarse por un nombre netbios que sea único y con 15 caracteres como máximo.
> ```PowerShell
> New-Cluster –Name <ClusterName> –Node <MachineName1,MachineName2,MachineName3,MachineName4> –NoStorage
> ```

Después de crear el clúster, puede tardar tiempo la replicación de la entrada DNS para el nombre del clúster. El tiempo depende del entorno y de la configuración de replicación de DNS. Si la resolución del clúster no se realiza correctamente, en la mayoría de los casos puede solucionarlo correctamente mediante el uso del nombre de la máquina de un nodo que sea un miembro activo del clúster que pueda utilizarse en lugar del nombre de clúster.

### <a name="step-34-configure-a-cluster-witness"></a>Paso 3,4: configurar un testigo de clúster

Se recomienda configurar un testigo para el clúster, por lo que los clústeres con tres o más servidores pueden resistir dos servidores con errores o sin conexión. Una implementación en dos servidores requiere un testigo de clúster; de lo contrario, si el servidor se queda sin conexión, el otro no estará disponible también. Con estos sistemas puede usar un recurso compartido de archivos como testigo o utilizar un testigo en la nube. 

Para obtener más información, consulta los temas siguientes:

- [Configurar y administrar el cuórum](../../failover-clustering/manage-cluster-quorum.md)
- [Implementación de un testigo en la nube para un clúster de conmutación por error](../../failover-clustering/deploy-cloud-witness.md)

### <a name="step-35-enable-storage-spaces-direct"></a>Paso 3.5: Habilitar Espacios de almacenamiento directo

Después de crear el clúster, use el cmdlet de PowerShell de `Enable-ClusterStorageSpacesDirect`, que pondrá el sistema de almacenamiento en el modo de Espacios de almacenamiento directo y realizará lo siguiente automáticamente:

-   **Crear un grupo:** Crea un único grupo grande que tiene un nombre como "S2D on Cluster1".

-   **Configurar las cachés de Espacios de almacenamiento directo:** si hay más de un tipo de soporte físico (unidad) disponible para usarlo en Espacios de almacenamiento directo, habilita el más rápido como dispositivo de memoria caché (lectura y escritura en la mayoría de los casos)

-   **Niveles:** Crea dos niveles como niveles predeterminados. Uno se denomina "Capacidad" y otro se denomina "Rendimiento". El cmdlet analiza los dispositivos y configura cada nivel con la mezcla de tipos de dispositivos y resistencia.

Desde el sistema de administración, en una ventana de comandos de PowerShell abierta con privilegios de administrador, inicie el siguiente comando. El nombre del clúster es el nombre del clúster que creó en los pasos anteriores. Si este comando se ejecuta localmente en uno de los nodos, no es necesario el parámetro -CimSession.

```PowerShell
Enable-ClusterStorageSpacesDirect –CimSession <ClusterName>
```

Para habilitar Espacios de almacenamiento directo con el comando anterior, también puede utilizar el nombre de nodo en lugar del nombre de clúster. Con el nombre de nodo puede ser más confiable debido a los retrasos de replicación de DNS que puedan surgir con el nombre de clúster recién creado.

Cuando finalice este comando, que puede tardar varios minutos, el sistema estará listo para crear volúmenes.

### <a name="step-36-create-volumes"></a>Paso 3.6: Crear volúmenes

Se recomienda usar el cmdlet `New-Volume`, ya que proporciona la experiencia más rápida y sencilla. Este cmdlet crea automáticamente por sí solo el disco virtual, lo particiona y formatea, crea el volumen con nombre coincidente y lo agrega a los volúmenes compartidos de clúster: todo en un paso sencillo.

Para obtener más información, echa un vistazo a [Crear volúmenes en Espacios de almacenamiento directo](create-volumes.md).

### <a name="step-37-optionally-enable-the-csv-cache"></a>Paso 3,7: habilitar opcionalmente la memoria caché de CSV

Opcionalmente, puede habilitar la caché de volumen compartido de clúster (CSV) para usar la memoria del sistema (RAM) como una caché de nivel de bloque de escritura de las operaciones de lectura que el administrador de caché de Windows no ha almacenado en caché. Esto puede mejorar el rendimiento de aplicaciones como Hyper-V. La memoria caché de CSV puede aumentar el rendimiento de las solicitudes de lectura y también es útil para escenarios de Servidor de archivos de escalabilidad horizontal.

Al habilitar la memoria caché de CSV se reduce la cantidad de memoria disponible para ejecutar máquinas virtuales en un clúster hiperconvergido, por lo que tendrá que equilibrar el rendimiento de almacenamiento con la memoria disponible para los VHD.

Para establecer el tamaño de la memoria caché de CSV, abra una sesión de PowerShell en el sistema de administración con una cuenta que tenga permisos de administrador en el clúster de almacenamiento y, a continuación, use este script, cambiando las variables `$ClusterName` y `$CSVCacheSize` según corresponda (en este ejemplo se establece una caché CSV de 2 GB por servidor):

```PowerShell
$ClusterName = "StorageSpacesDirect1"
$CSVCacheSize = 2048 #Size in MB

Write-Output "Setting the CSV cache..."
(Get-Cluster $ClusterName).BlockCacheSize = $CSVCacheSize

$CSVCurrentCacheSize = (Get-Cluster $ClusterName).BlockCacheSize
Write-Output "$ClusterName CSV cache size: $CSVCurrentCacheSize MB"
```

Para obtener más información, consulte [uso de la caché de lectura en memoria de CSV](csv-cache.md).

### <a name="step-38-deploy-virtual-machines-for-hyper-converged-deployments"></a>Paso 3,8: implementación de máquinas virtuales para implementaciones hiperconvergidas

Si va a implementar un clúster hiperconvergido, el último paso es aprovisionar máquinas virtuales en el clúster de Espacios de almacenamiento directo.

Los archivos de la máquina virtual deben almacenarse en el espacio de nombres CSV del sistema (ejemplo: c:\\ClusterStorage\\volume1), al igual que las máquinas virtuales en clúster en clústeres de conmutación por error.

Puede usar herramientas integradas u otras herramientas para administrar el almacenamiento y las máquinas virtuales, como System Center Virtual Machine Manager.

## <a name="step-4-deploy-scale-out-file-server-for-converged-solutions"></a>Paso 4: implementar Servidor de archivos de escalabilidad horizontal para las soluciones convergentes

Si va a implementar una solución convergente, el siguiente paso es crear una instancia de Servidor de archivos de escalabilidad horizontal y configurar algunos recursos compartidos de archivos. Si va a implementar un clúster hiperconvergido, ya ha terminado y no necesita esta sección.

### <a name="step-41-create-the-scale-out-file-server-role"></a>Paso 4,1: crear el rol de Servidor de archivos de escalabilidad horizontal

El siguiente paso para configurar los servicios de clúster para el servidor de archivos es crear el rol de servidor de archivos en clúster, que es cuando se crea la instancia de Servidor de archivos de escalabilidad horizontal en la que se hospedan los recursos compartidos de archivos disponibles continuamente.

#### <a name="to-create-a-scale-out-file-server-role-by-using-server-manager"></a>Para crear un rol de Servidor de archivos de escalabilidad horizontal mediante Administrador del servidor

1. En Administrador de clústeres de conmutación por error, seleccione el clúster, vaya a **roles**y, a continuación, haga clic en **configurar rol.** .<br>Aparece el Asistente para alta disponibilidad.
2. En la página **Seleccionar rol** , haga clic en **servidor de archivos**.
3. En la página **tipo de servidor de archivos** , haga clic en **servidor de archivos de escalabilidad horizontal para datos de aplicación**.
4. En la página **punto de acceso de cliente** , escriba un nombre para el servidor de archivos de escalabilidad horizontal.
5. Para comprobar que el rol se configuró correctamente, vaya a **roles** y confirme que la columna **Estado** muestra en **ejecución** junto al rol de servidor de archivos en clúster que ha creado, tal como se muestra en la figura 1.

   ![Captura de pantalla de Administrador de clústeres de conmutación por error que muestra el Servidor de archivos de escalabilidad horizontal](media/Hyper-converged-solution-using-Storage-Spaces-Direct-in-Windows-Server-2016/SOFS_in_FCM.png "Administrador de clústeres de conmutación por error que muestra el Servidor de archivos de escalabilidad horizontal")

    **Figura 1** Administrador de clústeres de conmutación por error mostrar el Servidor de archivos de escalabilidad horizontal con el estado en ejecución

> [!NOTE]
>  Después de crear el rol en clúster, es posible que se produzcan algunos retrasos en la propagación de red que podrían impedir la creación de recursos compartidos de archivos en él durante unos minutos, o incluso más.  
  
#### <a name="to-create-a-scale-out-file-server-role-by-using-windows-powershell"></a>Para crear un rol de Servidor de archivos de escalabilidad horizontal mediante Windows PowerShell

 En una sesión de Windows PowerShell que esté conectada al clúster de servidores de archivos, escriba los siguientes comandos para crear el rol de Servidor de archivos de escalabilidad horizontal, cambiando *FSCLUSTER* para que coincida con el nombre del clúster y *sofs* para que coincida con el nombre que desea asignar al rol de servidor de archivos de escalabilidad horizontal:

```PowerShell
Add-ClusterScaleOutFileServerRole -Name SOFS -Cluster FSCLUSTER
```

> [!NOTE]
>  Después de crear el rol en clúster, es posible que se produzcan algunos retrasos en la propagación de red que podrían impedir la creación de recursos compartidos de archivos en él durante unos minutos, o incluso más. Si el rol SOFS produce un error inmediatamente y no se inicia, puede deberse a que el objeto de equipo del clúster no tiene permiso para crear una cuenta de equipo para el rol SOFS. Para obtener ayuda con esto, consulte esta entrada de blog: [servidor de archivos de escalabilidad horizontal rol no se puede iniciar con los identificadores de evento 1205, 1069 y 1194](http://www.aidanfinn.com/?p=14142).

### <a name="step-42-create-file-shares"></a>Paso 4,2: crear recursos compartidos de archivos

Después de crear los discos virtuales y agregarlos a CSV, es el momento de crear recursos compartidos de archivos en ellos: un recurso compartido de archivos por CSV por cada disco virtual. System Center Virtual Machine Manager (VMM) es probablemente la forma handiest de hacerlo porque controla los permisos, pero si no lo tiene en su entorno, puede usar Windows PowerShell para automatizar parcialmente la implementación.

Use los scripts incluidos en el script [configuración de recurso compartido SMB para cargas de trabajo de Hyper-V](https://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) , que automatiza parcialmente el proceso de creación de grupos y recursos compartidos. Está diseñado para cargas de trabajo de Hyper-V, por lo que si va a implementar otras cargas de trabajo, es posible que tenga que modificar la configuración o realizar pasos adicionales después de crear los recursos compartidos. Por ejemplo, si utiliza Microsoft SQL Server, se debe conceder control total a la cuenta de servicio de SQL Server en el recurso compartido y el sistema de archivos.

> [!NOTE]
>  Tendrá que actualizar la pertenencia a grupos al agregar nodos de clúster, a menos que use System Center Virtual Machine Manager para crear sus recursos compartidos.

Para crear recursos compartidos de archivos mediante scripts de PowerShell, haga lo siguiente:

1. Descargue los scripts incluidos en la [configuración de recursos compartidos SMB para cargas de trabajo de Hyper-V](https://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) en uno de los nodos del clúster de servidores de archivos.
2. Abra una sesión de Windows PowerShell con credenciales de administrador de dominio en el sistema de administración y, a continuación, use el siguiente script para crear un grupo de Active Directory para los objetos de equipo de Hyper-V, y cambie los valores de las variables según corresponda para su entorno

    ```PowerShell
    # Replace the values of these variables
    $HyperVClusterName = "Compute01"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of script itself
    CD $ScriptFolder
    .\ADGroupSetup.ps1 -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName -HyperVClusterName $HyperVClusterName
    ```
3. Abra una sesión de Windows PowerShell con credenciales de administrador en uno de los nodos de almacenamiento y, a continuación, use el siguiente script para crear recursos compartidos para cada CSV y conceda permisos administrativos para los recursos compartidos al grupo Admins. del dominio y al clúster de proceso.

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

### <a name="step-43-enable-kerberos-constrained-delegation"></a>Paso 4,3 habilitación de la delegación restringida de Kerberos

Para configurar la delegación restringida de Kerberos para la administración remota de escenarios y aumentar Migración en vivo seguridad, desde uno de los nodos de clúster de almacenamiento, use el script KCDSetup. PS1 incluido en la [configuración de recursos compartidos de SMB para cargas de trabajo de Hyper-V](https://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a). Este es un pequeño contenedor del script:

```PowerShell
$HyperVClusterName = "Compute01"
$ScaleOutFSName = "SOFS"
$ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

CD $ScriptFolder
.\KCDSetup.ps1 -HyperVClusterName $HyperVClusterName -ScaleOutFSName $ScaleOutFSName -EnableLM
```

## <a name="next-steps"></a>Pasos siguientes

Después de implementar el servidor de archivos en clúster, se recomienda probar el rendimiento de la solución mediante cargas de trabajo sintéticas antes de que se muestren las cargas de trabajo reales. Esto le permite confirmar que la solución funciona correctamente y solucionar cualquier problema persistente antes de agregar la complejidad de las cargas de trabajo. Para obtener más información, vea rendimiento de los [espacios de almacenamiento de prueba con cargas de trabajo sintéticas](https://technet.microsoft.com/library/dn894707.aspx).

## <a name="see-also"></a>Vea también

-   [Espacios de almacenamiento directo en Windows Server 2016](storage-spaces-direct-overview.md)
-   [Descripción de la memoria caché en Espacios de almacenamiento directo](understand-the-cache.md)
-   [Planeación de volúmenes en Espacios de almacenamiento directo](plan-volumes.md)
-   [Tolerancia a errores de espacios de almacenamiento](storage-spaces-fault-tolerance.md)
-   [Requisitos de hardware Espacios de almacenamiento directo](Storage-Spaces-Direct-Hardware-Requirements.md)
-   [To RDMA, or not to RDMA – that is the question](https://blogs.technet.microsoft.com/filecab/2017/03/27/to-rdma-or-not-to-rdma-that-is-the-question/) (RDMA o no RDMA: esta es la cuestión) (blog de TechNet)
