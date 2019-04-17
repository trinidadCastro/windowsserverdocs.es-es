---
title: Implementar espacios de almacenamiento directo
ms.prod: windows-server-threshold
manager: eldenc
ms.author: stevenek
ms.technology: storage-spaces
ms.topic: get-started-article
ms.assetid: 20fee213-8ba5-4cd3-87a6-e77359e82bc0
author: stevenek
ms.date: 8/16/2018
description: Instrucciones paso a paso para implementar almacenamiento definido por software con espacios de almacenamiento directo en Windows Server como infraestructuras hiperconvergidas o convergida infraestructura (también conocida como desagregada separa).
ms.localizationpriority: medium
ms.openlocfilehash: 55cfa0e066506d7174f9e5b1e61cc0aa290706d7
ms.sourcegitcommit: 65e4f760be73104e67847f77e834e7b5e065211b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "5429047"
---
# Implementar espacios de almacenamiento directo

> Se aplica a: Windows Server 2019, Windows Server 2016

Este tema proporciona instrucciones paso a paso para implementar [Espacios de almacenamiento directo](storage-spaces-direct-overview.md).

> [!Tip]
> ¿Buscas para adquirir la infraestructura hiperconvergida? Microsoft recomienda estas soluciones [Definidas por Software de Windows Server](https://microsoft.com/wssd) de nuestros socios. Son diseñados, ensamblados y validan con nuestra arquitectura de referencia para garantizar la compatibilidad y confiabilidad, por lo que obtienes y ejecutar rápidamente.

> [!Tip]
> Puedes usar las máquinas virtuales de Hyper-V, incluidas en Microsoft Azure, para [evaluar espacios de almacenamiento directo sin necesidad de hardware](storage-spaces-direct-in-vm.md). También puedes revisar los útiles [scripts de implementación de laboratorio rápida de Windows Server](https://aka.ms/wslab), que se usa para fines de formación.

## Antes de empezar

Revisa los [requisitos de hardware de espacios de almacenamiento directo](Storage-Spaces-Direct-Hardware-Requirements.md) y echar un vistazo a este documento para familiarizarse con el enfoque general y notas importantes asociadas con algunos pasos.

Recopilar la información siguiente:

- **Opción de implementación.** Espacios de almacenamiento directo admite [dos opciones de implementación: hiperconvergida y convergido](storage-spaces-direct-overview.md#deployment-options), también conocida como desagregada separa. Familiarizarte con las ventajas de cada una para decidir cuál es adecuado para TI. Los pasos 1 a 3 siguientes se aplican a ambas opciones de implementación. Paso 4 solo es necesario para la implementación convergida.

- **Nombres de servidor.** Familiarízate con directivas de nomenclatura de la organización para los equipos, archivos, rutas de acceso y otros recursos. Tendrás que proporcionar varios servidores, cada uno con nombres únicos.

- **Nombre de dominio.** Familiarízate con las directivas de la organización para los nombres de dominio y unirse a dominio.  Que puedas los servidores al dominio y tendrás que especificar el nombre de dominio. 

- **Redes de RDMA.** Hay dos tipos de protocolos RDMA: iWarp y RoCE. Ten en cuenta cuál usan de los adaptadores de red, y si RoCE, ten en cuenta también la versión (v1 o v2). Para RoCE, ten en cuenta también el modelo del conmutador de la parte superior del rack.

- **ID DE VLAN.** Ten en cuenta el ID de VLAN que se usará para los adaptadores de red de sistema operativo de administración en los servidores, si lo hay. Deberías poder obtenerlo del administrador de red.

## Paso 1: Implementar Windows Server

### Paso 1.1: Instalar el sistema operativo

El primer paso es instalar Windows Server en todos los servidores que se incluirán en el clúster. Espacios de almacenamiento directo requiere Windows Server 2016 Datacenter Edition. Puedes usar la opción de instalación de Server Core o servidor con experiencia de escritorio.

Cuando se instala Windows Server con el Asistente para instalación, puede elegir entre *Windows Server* (referido a Server Core) y *Windows Server (servidor con experiencia de escritorio)*, que es el equivalente de la opción de instalación *completa* está disponible en Windows Server 2012 R2. Si no elige, obtendrás la opción de instalación Server Core. Para obtener más información, consulta [las opciones de instalación de Windows Server 2016](../../get-started/Windows-Server-2016.md).

### Paso 1.2: Conectar a los servidores

Esta guía centra la opción de instalación de Server Core y de implementar y administrar remotamente desde un sistema de administración independiente, que debe tener:

- Windows Server 2016 con las mismas actualizaciones que los servidores que administra
- Conectividad de red a los servidores que administra
- Unido al mismo dominio o a un dominio de plena confianza
- Herramientas de administración remota del servidor (RSAT) y módulos de PowerShell para Hyper-V y clústeres de conmutación por error. Las herramientas RSAT y módulos de PowerShell están disponibles en Windows Server y pueden instalarse sin necesidad de instalar otras características. También puedes instalar las [Herramientas de administración remota del servidor](https://www.microsoft.com/download/details.aspx?id=45520) en un equipo de administración de Windows 10.

En el sistema de administración, instale el clúster de conmutación por error y las herramientas de administración de Hyper-V. Esto puede hacerse a través del Administrador del servidor mediante el uso del Asistente para **agregar roles y características**. En la página **Características**, seleccione **Herramientas de administración remota del servidor** y, después, seleccione las herramientas que desea instalar.

Especifique la sesión de PS y use el nombre del servidor o la dirección IP del nodo al que desea conectarse. Le solicite una contraseña después de ejecutar este comando, escriba la contraseña de administrador que especificó al configurar Windows.

   ```PowerShell
   Enter-PSSession -ComputerName <myComputerName> -Credential LocalHost\Administrator
   ```

   Este es un ejemplo de hacer lo mismo de manera que es más útil en scripts, en caso de que debes hacer esto más de una vez:

   ```PowerShell
   $myServer1 = "myServer-1"
   $user = "$myServer1\Administrator"

   Enter-PSSession -ComputerName $myServer1 -Credential $user
   ```

> [!TIP]
> Si estás implementando de forma remota desde un sistema de administración, es posible que obtengas un error como *WinRM no puede procesar la solicitud.* Para corregir esto, usar Windows PowerShell para agregar cada servidor a la lista de Hosts de confianza en el equipo de administración:  
>   
> `Set-Item WSMAN:\Localhost\Client\TrustedHosts -Value Server01 -Force`
>  
> Nota: la lista de hosts de confianza admite caracteres comodín, como `Server*`.
>
> Para ver la lista de Hosts de confianza, escribe `Get-Item WSMAN:\Localhost\Client\TrustedHosts`.  
>   
> Para vaciar la lista, escriba `Clear-Item WSMAN:\Localhost\Client\TrustedHost`.  

### Paso 1.3: Unirse al dominio y agregar cuentas de dominio

Hasta ahora que has configurado los servidores individuales con la cuenta de administrador local, `<ComputerName>\Administrator`.

Para administrar espacios de almacenamiento directo, tendrás que unirte a los servidores a un dominio y usar una cuenta de dominio de Active Directory Domain Services que se encuentra en el grupo de administradores en cada servidor.

En el sistema de administración, abra una consola de PowerShell con privilegios de administrador. Uso `Enter-PSSession` para conectarse a cada servidor y ejecuta el cmdlet siguiente, sustituyendo su propio nombre de equipo, nombre de dominio y las credenciales de dominio:

```PowerShell  
Add-Computer -NewName "Server01" -DomainName "contoso.com" -Credential "CONTOSO\User" -Restart -Force  
```

Si tu cuenta de administrador de almacenamiento no es un miembro del grupo de administradores de dominio, agregar su cuenta de administrador de almacenamiento para el grupo de administradores local en cada nodo - o aún mejor, agrega el grupo que usas para los administradores de almacenamiento. Puedes usar el comando siguiente (o escribir una función de Windows PowerShell para hacerlo, consulta [Usar PowerShell para agregar los usuarios del dominio a un grupo a Local](http://blogs.technet.com/b/heyscriptingguy/archive/2010/08/19/use-powershell-to-add-domain-users-to-a-local-group.aspx) para obtener más información):

```
Net localgroup Administrators <Domain\Account> /add
```

### Paso 1.4: Instalar roles y características

El siguiente paso es instalar roles de servidor en cada servidor. Puedes hacerlo mediante el uso de [Windows Admin Center](../../manage/windows-admin-center/use/manage-servers.md), [Administrador de servidores](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)), o PowerShell. Estas son las funciones para instalar:

- Clúster de conmutación por error
- Hyper-V
- Servidor de archivos (si lo desea hospedar los recursos compartidos de archivos, como una implementación convergida)
- Protocolo de puente del centro de datos (si usas RoCEv2 en lugar de adaptadores de red iWARP)
- RSAT-Clustering-PowerShell
- Hyper-V-PowerShell

Para instalar a través de PowerShell, usa el cmdlet [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature) . Puedes usar en un único servidor como este:

```PowerShell
Install-WindowsFeature -Name "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"
```

Para ejecutar el comando en todos los servidores del clúster al mismo tiempo, usa poco de script, modificar la lista de variables al principio de la secuencia de comandos para ajustar el entorno.

```PowerShell
# Fill in these variables with your values
$ServerList = "Server01", "Server02", "Server03", "Server04"
$FeatureList = "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"

# This part runs the Install-WindowsFeature cmdlet on all servers in $ServerList, passing the list of features into the scriptblock with the "Using" scope modifier so you don't have to hard-code them here.
Invoke-Command ($ServerList) {
    Install-WindowsFeature -Name $Using:Featurelist
}
```

## Paso2: Configurar la red

Si estás implementando espacios de almacenamiento directo dentro de las máquinas virtuales, omite esta sección.

Espacios de almacenamiento directo requiere gran ancho de banda, baja latencia de red entre servidores del clúster. Redes de al menos 10 GbE es necesario y se recomienda remoto acceso directo a memoria (RDMA). Puedes usar iWARP o RoCE siempre que tenga el logotipo de Windows Server 2016, pero iWARP normalmente es más fácil configurar.

> [!Important]
> Dependiendo de los equipos de red y, especialmente con RoCE v2, alguna configuración del conmutador de la parte superior del rack puede ser necesario. Configuración del conmutador correcto es importante para garantizar la confiabilidad y el rendimiento de espacios de almacenamiento directo.

Windows Server 2016 presenta switch embedded teaming (SET) en el conmutador virtual de Hyper-V. Esto permite que los mismos puertos NIC físicos que se usará para todo el tráfico de red mientras usas RDMA, reduciendo el número de puertos de NIC física necesaria. Switch embedded teaming se recomienda para espacios de almacenamiento directo.

Para que obtener instrucciones para configurar la red para espacios de almacenamiento directo, vea [Windows Server 2016 convergido NIC y Guía de implementación de invitado RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/S2D%20WS2016_ConvergedNIC_Configuration.docx).

## Paso 3: Configurar Espacios de almacenamiento directo

Los pasos siguientes se realizan en un sistema de administración que tenga la misma versión que los servidores que se están configurando. Los pasos siguientes no se ejecuta de forma remota mediante una sesión de PowerShell, sino en su lugar se ejecuta en una sesión de PowerShell local en el sistema de administración, con permisos administrativos.

### Paso 3.1: Limpias unidades de disco

Antes de habilitar espacios de almacenamiento directo, asegúrate de que las unidades son vacías: sin particiones antiguo u otros datos. Ejecutar el script siguiente, sustituyendo los nombres de equipo, para quitar todas las particiones antiguo u otros datos.

> [!Warning]
> Este script quitará permanentemente todos los datos de las unidades que no sean de la unidad de arranque del sistema operativo.

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

El resultado tendrá este aspecto, donde el **recuento** es el número de unidades de cada modelo en cada servidor:

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

### Paso 3.2: Valide el clúster

En este paso, podrá ejecutar la herramienta de validación de clúster para asegurarse de que los nodos de servidor están configurados correctamente para crear un clúster con espacios de almacenamiento directo. Validación de clúster cuando (`Test-Cluster`) es ejecutar antes de crear el clúster, se ejecutan las pruebas que compruebe que la configuración aparece adecuada para funcionar correctamente como un clúster de conmutación por error. En el ejemplo siguiente usa el `-Include` se especifican parámetro y, a continuación, en las categorías de pruebas. Esto garantiza que se incluyen en la validación las pruebas específicas de Espacios de almacenamiento directo.

Utilice el siguiente comando de PowerShell para validar un conjunto de servidores para su uso como un clúster de Espacios de almacenamiento directo.

```PowerShell
Test-Cluster –Node <MachineName1, MachineName2, MachineName3, MachineName4> –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
```

### Paso 3.3: Crear el clúster

En este paso, creará un clúster con los nodos que ha validado para la creación del clúster en el paso anterior mediante el siguiente cmdlet de PowerShell.

Al crear el clúster, obtendrá una advertencia que indica: "se han producido problemas mientras crear el rol en el que puede evitar que se inicie. Para obtener más información, consulte el archivo de informe siguiente." Puede omitir con seguridad esta advertencia. Es debido a no hay ningún disco disponible para el cuórum de clúster. Se recomienda configurar un testigo del recurso compartido de archivos o un testigo en la nube después de crear el clúster.

> [!Note]
> Si los servidores están usando direcciones IP estáticas, modifique el siguiente comando para reflejar la dirección IP estática agregando el parámetro siguiente y especificando la dirección IP: -StaticAddress &lt;X.X.X.X&gt;.
> En el siguiente comando, el marcador de posición ClusterName debe reemplazarse por un nombre netbios que sea único y con 15 caracteres como máximo.
> ```PowerShell
> New-Cluster –Name <ClusterName> –Node <MachineName1,MachineName2,MachineName3,MachineName4> –NoStorage
> ```

Después de crear el clúster, puede tardar tiempo la replicación de la entrada DNS para el nombre del clúster. El tiempo depende del entorno y de la configuración de replicación de DNS. Si la resolución del clúster no se realiza correctamente, en la mayoría de los casos puedes solucionarlo mediante el uso del nombre de máquina de un nodo que sea miembro activo del clúster en lugar del nombre de clúster.

### Paso 3.4: Configurar a un testigo de clúster

Te recomendamos que configure un testigo para el clúster, por lo que los clústeres con tres o más servidores pueden resistir dos servidores error o sin conexión. Una implementación de dos servidores requiere un testigo de clúster, de lo contrario cualquiera de los servidores desconectarse hace que el otro tampoco esté disponible. Con estos sistemas puede usar un recurso compartido de archivos como testigo o utilizar un testigo en la nube. 

Para obtener más información, consulta los temas siguientes:

- [Configurar y administrar el cuórum](../../failover-clustering/manage-cluster-quorum.md)
- [Implementación de un testigo en la nube para un clúster de conmutación por error](../../failover-clustering/deploy-cloud-witness.md)

### Paso 3.5: Habilitar Espacios de almacenamiento directo

Después de crear el clúster, use el `Enable-ClusterStorageSpacesDirect` cmdlet de PowerShell, que se pone el sistema de almacenamiento en el modo de espacios de almacenamiento directo y automáticamente lo siguiente:

-   **Crear un grupo:** Crea un único grupo grande que tiene un nombre como "S2D on Cluster1".

-   **Configurar las cachés de Espacios de almacenamiento directo:** si hay más de un tipo de soporte físico (unidad) disponible para usarlo en Espacios de almacenamiento directo, habilita el más rápido como dispositivo de memoria caché (lectura y escritura en la mayoría de los casos)

-   **Niveles:** Crea dos niveles como niveles predeterminados. Uno se denomina "Capacidad" y otro se denomina "Rendimiento". El cmdlet analiza los dispositivos y configura cada nivel con la mezcla de tipos de dispositivos y resistencia.

Desde el sistema de administración, en una ventana de comandos de PowerShell abierta con privilegios de administrador, inicie el siguiente comando. El nombre del clúster es el nombre del clúster que creó en los pasos anteriores. Si este comando se ejecuta localmente en uno de los nodos, no es necesario el parámetro -CimSession.

```PowerShell
Enable-ClusterStorageSpacesDirect –CimSession <ClusterName>
```

Para habilitar Espacios de almacenamiento directo con el comando anterior, también puede utilizar el nombre de nodo en lugar del nombre de clúster. Con el nombre de nodo puede ser más confiable debido a los retrasos de replicación de DNS que puedan surgir con el nombre de clúster recién creado.

Cuando finalice este comando, que puede tardar varios minutos, el sistema estará listo para crear volúmenes.

### Paso 3.6: Crear volúmenes

Te recomendamos que uses el `New-Volume` cmdlet ya que proporciona la experiencia más rápida y sencilla. Este cmdlet crea automáticamente por sí solo el disco virtual, lo particiona y formatea, crea el volumen con nombre coincidente y lo agrega a los volúmenes compartidos de clúster: todo en un paso sencillo.

Para obtener más información, echa un vistazo a [Crear volúmenes en Espacios de almacenamiento directo](create-volumes.md).

### Paso 3.7: La opción de habilitar la caché CSV

Opcionalmente, puedes habilitar la caché de (CSV) de volumen compartido de clúster que se usará la memoria del sistema (RAM) como una caché de nivel de bloque de escritura a través de las operaciones de lectura que ya no se almacenan en caché por el Administrador de la memoria caché de Windows. Esto puede mejorar el rendimiento de aplicaciones, como Hyper-V. La memoria caché CSV puede aumentar el rendimiento de solicitudes de lectura y también es útil para escenarios de servidor de archivos de escalabilidad horizontal.

Al habilitar la caché CSV, reduce la cantidad de memoria disponible para ejecutar máquinas virtuales en un clúster hiperconvergido, por lo que tendrás que equilibrar el rendimiento del almacenamiento con la memoria disponible para los discos duros virtuales.

Para establecer el tamaño de la memoria caché CSV, abre una sesión de PowerShell en el sistema de administración con una cuenta que tenga permisos de administrador en el clúster de almacenamiento y, a continuación, usar este script, cambiar el `$ClusterName` y `$CSVCacheSize` variables según corresponda (en este ejemplo establece un 2 GB CSV caché por servidor):

```PowerShell
$ClusterName = "StorageSpacesDirect1"
$CSVCacheSize = 2048 #Size in MB

Write-Output "Setting the CSV cache..."
(Get-Cluster $ClusterName).BlockCacheSize = $CSVCacheSize

$CSVCurrentCacheSize = (Get-Cluster $ClusterName).BlockCacheSize
Write-Output "$ClusterName CSV cache size: $CSVCurrentCacheSize MB"
```

Para obtener más información, consulta [usando el CSV en memoria caché de lectura](csv-cache.md).

### Paso 3.8: Implementar máquinas virtuales para implementaciones hiperconvergidas

Si vas a implementar un clúster hiperconvergido, el último paso es aprovisionar máquinas virtuales en el clúster de espacios de almacenamiento directo.

Los archivos de la máquina virtual se deben almacenar en el espacio de nombres de sistemas CSV (por ejemplo: c:\\ClusterStorage\\Volume1) al igual que las máquinas virtuales en clúster en clústeres de conmutación por error.

Puedes usar las herramientas integradas u otras herramientas para administrar el almacenamiento y las máquinas virtuales, como System Center Virtual Machine Manager.

## Paso 4: Implementar el servidor de archivos de escalabilidad horizontal para soluciones convergidas

Si vas a implementar una solución convergida, el siguiente paso es crear una instancia de servidor de archivos de escalabilidad horizontal y algunos recursos compartidos de archivos de instalación. Si vas a implementar un clúster hiperconvergido, haya terminado y no es necesario de esta sección.

### Paso 4.1: Crear el rol de servidor de archivos de escalabilidad horizontal

El siguiente paso en la configuración de los servicios de clúster para el servidor de archivos es crear el rol de servidor de archivos agrupados en clúster, que es cuando se crea la instancia de servidor de archivos de escalabilidad horizontal en el que se hospedan los recursos compartidos de archivos disponibles continuamente.

#### Para crear un rol de servidor de archivos de escalabilidad horizontal mediante el administrador del servidor

1.  En el Administrador de clústeres de conmutación por error, selecciona el clúster, ve a las **funciones**y, a continuación, haz clic en **Configurar rol**.<br>Aparece el Asistente para alta disponibilidad.
2.  En la página **Seleccionar rol** , haz clic en el **Servidor de archivos**.
3.  En la página de **Tipo de servidor de archivos** , haz clic en el **Servidor de archivos de escalabilidad horizontal para datos de la aplicación**.
4.  En la página de **Punto de acceso de cliente** , escribe un nombre para el servidor de archivos de escalabilidad horizontal.
5.  Comprueba que la función se ha configurado correctamente yendo a las **funciones** y confirmar que la columna **estado** muestra **ejecuta** junto a la función de servidor de archivos agrupados en clúster creado, tal como se muestra en la figura 1.

    ![Captura de pantalla del Administrador de clústeres de conmutación por error que muestra el servidor de archivos de escalabilidad horizontal](media\Hyper-converged-solution-using-Storage-Spaces-Direct-in-Windows-Server-2016\SOFS_in_FCM.png "Failover Cluster Manager showing the Scale-Out File Server")

     **Figura 1** Administrador de clústeres de conmutación por error, que muestra el servidor de archivos de escalabilidad horizontal con el estado de ejecución

> [!NOTE]
>  Después de crear el rol en el clúster, es posible que haya algunos red retrasos de propagación que podrían impedir la creación de recursos compartidos de archivos en él durante unos minutos, o potencialmente más.  
  
#### Para crear un rol de servidor de archivos de escalabilidad horizontal con Windows PowerShell

 En una sesión de Windows PowerShell que está conectada en el clúster de servidores de archivo, escribe los siguientes comandos para crear el rol de servidor de archivos de escalabilidad horizontal, cambiar *FSCLUSTER* para que coincida con el nombre del clúster y *SOFS* para que coincida con el nombre que quieres que el Rol de servidor de archivos de escalabilidad horizontal:

```PowerShell
Add-ClusterScaleOutFileServerRole -Name SOFS -Cluster FSCLUSTER
```

> [!NOTE]
>  Después de crear el rol en el clúster, es posible que haya algunos red retrasos de propagación que podrían impedir la creación de recursos compartidos de archivos en él durante unos minutos, o potencialmente más. Si la función SOFS se produce un error inmediatamente y no se inicia, podría deberse a uno de los que el objeto de equipo del clúster no tiene permiso para crear una cuenta de equipo para el rol SOFS. Para ayudar con ese, consulta esta entrada de blog: [Scale-Out archivo Server rol se produce un error para inicio con eventos identificadores 1205, 1069 y 1194](http://www.aidanfinn.com/?p=14142).

### Paso 4.2: Crear recursos compartidos de archivos

Cuando hayas creado tus discos virtuales y agregado a CSV, es el momento para crear recursos compartidos de archivos en ellos: recurso compartido de un archivo por CSV por disco virtual. System Center Virtual Machine Manager (VMM) es probablemente la forma handiest hacerlo porque se permisos para TI, pero si no lo tiene en su entorno, puedes usar Windows PowerShell para automatizar la implementación de parcialmente.

Usar los scripts que se incluyen en el script de [Configuración de recurso compartido de SMB para cargas de trabajo de Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) , que parcialmente automatiza el proceso de creación de grupos y recursos compartidos. Está escrito para las cargas de trabajo de Hyper-V para si vas a implementar otras cargas de trabajo, que es posible que tengas que modificar la configuración o realizar pasos adicionales después de crear los recursos compartidos. Por ejemplo, si estás usando Microsoft SQL Server, debe concederse a la cuenta de servicio de SQL Server control total sobre el recurso compartido y el sistema de archivos.

> [!NOTE]
>  Tendrás que actualizar la pertenencia al grupo al agregar nodos de clúster a menos que uses System Center Virtual Machine Manager para crear los recursos compartidos.

Para crear recursos compartidos de archivos mediante el uso de scripts de PowerShell, hacer lo siguiente:

1. Descarga los scripts que se incluyen en la [Configuración del recurso compartido de SMB para cargas de trabajo de Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) a uno de los nodos del clúster de servidores de archivos.
2. Abre una sesión de Windows PowerShell con credenciales de administrador de dominio en el sistema de administración y, a continuación, usa el siguiente script para crear un grupo de Active Directory para los objetos de equipo de Hyper-V, cambiar los valores de las variables según corresponda para tu entorno:

    ```PowerShell
    # Replace the values of these variables
    $HyperVClusterName = "Compute01"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of script itself
    CD $ScriptFolder
    .\ADGroupSetup.ps1 -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName -HyperVClusterName $HyperVClusterName
    ```
3. Abre una sesión de Windows PowerShell con credenciales de administrador en uno de los nodos de almacenamiento y, a continuación, usar el siguiente script para crear recursos compartidos para cada CSV y conceder permisos administrativos para los recursos compartidos para el grupo de administradores de dominio y el clúster de proceso.

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

### Delegación limitada de Kerberos de habilitar paso 4.3

Para configurar la delegación limitada de Kerberos para la administración de escenario remoto y aumentar la seguridad de migración en vivo, desde uno de los nodos del clúster de almacenamiento, usa el script de KCDSetup.ps1 incluido en la [Configuración del recurso compartido de SMB para cargas de trabajo de Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a). Este es un poco contenedor para el script:

```PowerShell
$HyperVClusterName = "Compute01"
$ScaleOutFSName = "SOFS"
$ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

CD $ScriptFolder
.\KCDSetup.ps1 -HyperVClusterName $HyperVClusterName -ScaleOutFSName $ScaleOutFSName -EnableLM
```

## Pasos siguientes

Después de implementar el servidor de archivos agrupados en clúster, se recomienda probar el rendimiento de la solución con cargas de trabajo sintéticas antes de mostrar las cargas de trabajo reales. Esto te permite confirmar que la solución se realiza correctamente y resolver los problemas persistentes antes de agregar la complejidad de las cargas de trabajo. Para obtener más información, consulta [Prueba almacenamiento espacios rendimiento con sintética cargas de trabajo](https://technet.microsoft.com/library/dn894707.aspx).

## Consulta también

-   [Espacios de almacenamiento directo en Windows Server 2016](storage-spaces-direct-overview.md)
-   [Descripción de la memoria caché de Espacios de almacenamiento directo](understand-the-cache.md)
-   [Planificación de volúmenes en Espacios de almacenamiento directo](plan-volumes.md)
-   [Tolerancia a errores de Espacios de almacenamiento](storage-spaces-fault-tolerance.md)
-   [Requisitos de hardware de Espacios de almacenamiento directo](Storage-Spaces-Direct-Hardware-Requirements.md)
-   [To RDMA, or not to RDMA – that is the question](https://blogs.technet.microsoft.com/filecab/2017/03/27/to-rdma-or-not-to-rdma-that-is-the-question/) (RDMA o no RDMA: esta es la cuestión) (blog de TechNet)
