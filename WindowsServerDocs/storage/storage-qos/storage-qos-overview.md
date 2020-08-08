---
title: Calidad de servicio de almacenamiento
manager: dongill
ms.author: JGerend
ms.topic: get-started-article
ms.assetid: 8dcb8cf9-0e08-4fdd-9d7e-ec577ce8d8a0
author: kumudd
ms.date: 10/10/2016
ms.openlocfilehash: ced2fe051f0595e8333aa2704889cc88ec1304ac
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87994727"
---
# <a name="storage-quality-of-service"></a>Calidad de servicio de almacenamiento

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

La calidad de servicio (QoS) de almacenamiento en Windows Server 2016 ofrece un medio de supervisar y administrar centralmente el rendimiento del almacenamiento para máquinas virtuales con Hyper-V y los roles de servidor de archivos de escalabilidad horizontal. La característica mejora automáticamente la imparcialidad de los recursos de almacenamiento entre varias máquinas virtuales que usan el mismo clúster de servidores de archivos y permite la configuración de objetivos de rendimiento máximos y mínimos basados en directiva en unidades de E/S por segundo normalizadas.

Puede utilizar la calidad de servicio de almacenamiento en Windows Server 2016 para realizar lo siguiente:

-   **Mitigar los problemas de «vecino ruidoso».** De forma predeterminada, la calidad de servicio de almacenamiento garantiza que una sola máquina virtual no puede consumir todos los recursos de almacenamiento y privar a otras máquinas virtuales del ancho de banda de almacenamiento.

-   **Supervisar el rendimiento del almacenamiento de extremo a extremo.** En cuanto se inician las máquinas virtuales almacenadas en un servidor de archivos de escalabilidad horizontal, se comienza a supervisar su rendimiento. Es posible visualizar los detalles de rendimiento de todas las máquinas virtuales en ejecución y la configuración del clúster de servidores de archivos de escalabilidad horizontal desde una sola ubicación.

-   **Administrar E/S de almacenamiento por necesidades empresariales de carga de trabajo** Las directivas de calidad de servicio de almacenamiento definen mínimos y máximos de rendimiento para las máquinas virtuales y garantizan que se cumplen. Así se consigue un rendimiento coherente entre las máquinas virtuales, incluso en entornos con gran densidad y sobreaprovisionados. Si no se pueden cumplir las directivas, hay alertas disponibles para realizar el seguimiento de las máquinas virtuales que no siguen ninguna directiva o tienen asignadas directivas no válidas.

En este documento se describe cómo se puede beneficiar su negocio de la nueva funcionalidad de calidad de servicio de almacenamiento. Para seguirlo, se supone que tiene conocimientos prácticos anteriores de Windows Server, clústeres de conmutación por error de Windows Server, el servidor de archivos de escalabilidad horizontal, Hyper-V y Windows PowerShell.

## <a name="overview"></a><a name="BKMK_Overview"></a>Información general
En esta sección se describen los requisitos para usar la calidad de servicio de almacenamiento y, además, se incluye información general de una solución definida por software que usa calidad de servicio de almacenamiento y una lista de términos relacionados con la calidad de servicio de almacenamiento.

### <a name="storage-qos-requirements"></a><a name="BKMK_Requirements"></a>Requisitos de la calidad de servicio de almacenamiento
La calidad de servicio de almacenamiento admite dos escenarios de implementación:

-   **Hyper-V con un servidor de archivos de escalabilidad horizontal**. Este escenario requiere lo siguiente:

    -   Un clúster de almacenamiento que sea un clúster de servidores de archivos de escalabilidad horizontal

    -   Un clúster de proceso que tenga al menos un servidor con el rol de Hyper-V habilitado

    En el caso de la calidad de servicio de almacenamiento, el clúster de conmutación por error es necesario en los servidores de almacenamiento, pero no es obligatorio que los servidores de proceso estén en un clúster de conmutación por error. Todos los servidores (que se usan para Almacenamiento y Proceso) deben estar ejecutando Windows Server 2016.

    Si no tiene un clúster de servidores de archivos de escalabilidad horizontal implementado con fines de evaluación, compile uno usando los servidores o las máquinas virtuales existentes; para ello, siga las instrucciones detalladas contenidas en [Windows Server 2012 R2 Storage: Step-by-step with Storage Spaces, SMB Scale-Out and Shared VHDX (Physical)](/archive/blogs/josebda/windows-server-2012-r2-storage-step-by-step-with-storage-spaces-smb-scale-out-and-shared-vhdx-physical) (Almacenamiento de Windows Server 2012 R2: paso a paso con espacios de almacenamiento, Escala horizontal SMB y VHDX compartido [físico]).

-   **Hyper-V con volúmenes compartidos de clúster.** Este escenario presenta dos requisitos:

    -   Un clúster de proceso con el rol Hyper-V habilitado

    -   Hyper-V con volúmenes compartidos de clúster (CSV) para almacenamiento

El clúster de conmutación por error es necesario. Todos los servidores deben ejecutar la misma versión de Windows Server 2016.

### <a name="using-storage-qos-in-a-software-defined-storage-solution"></a><a name="BKMK_SolutionOverview"></a>Uso de la calidad de servicio de almacenamiento en una solución de almacenamiento definida por software
La calidad de servicio de almacenamiento está integrada en la solución de almacenamiento definida por software de Microsoft que proporcionan el servidor de archivos de escalabilidad horizontal e Hyper-V. El servidor de archivos de escalabilidad horizontal expone recursos compartidos de archivos a los servidores de Hyper-V mediante el protocolo SMB3. Se ha agregado un nuevo Administrador de directivas para el clúster de servidor de archivos, que proporciona supervisión de rendimiento para el almacenamiento central.

![Servidor de archivos de escalabilidad horizontal y QoS de almacenamiento](media/overview-Clustering_SOFSStorageQoS.png)

**Ilustración 1: uso de la calidad de servicio de almacenamiento en una solución de almacenamiento definida por software en un servidor de archivos de escalabilidad horizontal**

En cuanto los servidores de Hyper-V inician las máquinas virtuales, estas empiezan a supervisarse por el Administrador de directivas. El Administrador de directivas comunica la directiva de calidad de servicio de almacenamiento y los límites o reservas sobre el servidor de Hyper-V, que controla el rendimiento de la máquina virtual según corresponda.

Cuando hay cambios en las directivas de calidad de servicio de almacenamiento o en las demandas de rendimiento de las máquinas virtuales, el Administrador de directivas se lo notifica a los servidores de Hyper-V para ajustar su comportamiento. Este bucle de retroalimentación garantiza que todos los discos duros virtuales de las máquinas virtuales tengan un rendimiento constante de acuerdo con la definición de las directivas de calidad de servicio de almacenamiento.

### <a name="glossary"></a><a name="BKMK_Glossary"></a>Glosario

|Término|Descripción|
|--------|---------------|
|E/S por segundo normalizada|Todo el uso de almacenamiento se mide en "E/S por segundo normalizada".  Se trata de un recuento de las operaciones de entrada y salida de almacenamiento por segundo.  Toda E/S con un tamaño de 8 KB o inferior se considera una E/S por segundo normalizada.  Toda E/S con un tamaño superior a 8 KB se trata como varias E/S por segundo normalizadas. Por ejemplo, una solicitud de 256 KB se trata como 32 E/S por segundo normalizadas.<p>Windows Server 2016 incluye la capacidad de especificar el tamaño que se utiliza para normalizar las E/S.  En el clúster de almacenamiento, se puede especificar el tamaño normalizado para que surta efecto en los cálculos de normalización de todo el clúster.  El valor predeterminado sigue siendo 8 KB.|
|Flujo|Cada identificador de archivos abierto por un servidor de Hyper-V en un archivo VHD o VHDX se considera un "flujo". Si una máquina virtual tiene dos discos duros virtuales conectados, tendrá 1 flujo al clúster de servidores de archivos por archivo. Si se comparte un VHDX con varias máquinas virtuales, este tendrá un 1 flujo por cada máquina virtual.|
|InitiatorName|Nombre de la máquina virtual que se notifica al servidor de archivos de escalabilidad horizontal para cada flujo.|
|InitiatorID|Un identificador que coincide con el identificador de la máquina virtual.  Siempre puede utilizarse para identificar de manera única máquinas virtuales de flujos individuales, incluso si las máquinas virtuales tienen el mismo valor en InitiatorName.|
|Directiva|Las directivas de calidad de servicio de almacenamiento se almacenan en la base de datos del clúster y tienen las siguientes propiedades: PolicyId, MinimumIOPS, MaximumIOPS, ParentPolicy y PolicyType.|
|PolicyId|Identificador único de una directiva.  Se genera de forma predeterminada, pero puede especificarse si se desea.|
|MinimumIOPS|La E/S por segundo mínima normalizada que ofrecerá una directiva.  También conocido como "Reserva".|
|MaximumIOPS|La E/S por segundo máxima normalizada que estará limitada por una directiva.  También se conoce como "Límite".|
|Agregado |Tipo de directiva donde los valores MinimumIOPS y MaximumIOPS y el ancho de banda se comparten entre todos los flujos asignados a la directiva. Los VHD asignados a la directiva en ese sistema de almacenamiento tienen una sola asignación de ancho de banda de E/S para compartir entre todos.|
|Dedicado|Tipo de directiva donde los valores MinimumIOPS y MaximumIOPS y el ancho de banda se administran para VHD/VHDx individuales.|

## <a name="how-to-set-up-storage-qos-and-monitor-basic-performance"></a><a name="BKMK_SetUpQoS"></a>Configuración de la calidad de servicio de almacenamiento y supervisión del rendimiento básico
En esta sección se describe cómo habilitar la nueva característica de calidad de servicio de almacenamiento y cómo supervisar el rendimiento de almacenamiento sin aplicar las directivas personalizadas.

### <a name="set-up-storage-qos-on-a-storage-cluster"></a><a name="BKMK_SetupStorageQoSonStorageCluster"></a>Configuración de la calidad de servicio de almacenamiento en un clúster de almacenamiento
En esta sección se describe cómo habilitar la calidad de servicio de almacenamiento en un clúster de conmutación por error o un servidor de archivos de escalabilidad horizontal nuevo o existente que ejecuta Windows Server 2016.

#### <a name="set-up-storage-qos-on-a-new-installation"></a>Configuración de la calidad de servicio de almacenamiento en una instalación nueva
Si ha configurado tanto un nuevo clúster de conmutación por error como un volumen compartido de clúster en Windows Server 2016, la característica de calidad de servicio de almacenamiento se configurará automáticamente.

#### <a name="verify-storage-qos-installation"></a>Comprobación de la instalación de la calidad de servicio de almacenamiento
Después de haber creado un clúster de conmutación por error y configurado un disco CSV, aparece el **recurso de calidad de servicio de almacenamiento** como un recurso de clúster principal y es visible en el Administrador de clústeres de conmutación por error y en Windows PowerShell. La intención es que el sistema de clúster de conmutación por error administre este recurso y no sea necesario realizar ninguna acción en él.  Lo mostramos en el Administrador de clústeres de conmutación por error y PowerShell para mantener la coherencia con los demás recursos de sistema del clúster de conmutación por error, como el nuevo Servicio de mantenimiento.

![El recurso de QoS de almacenamiento aparece en Recursos principales de clúster](media/overview-Clustering_StorageQoSFCM.png)

**Ilustración 2: recurso de calidad de servicio de almacenamiento como recurso de clúster principal en el Administrador de clústeres de conmutación por error**

Utilice el siguiente cmdlet de PowerShell para ver el estado del recurso de la calidad de servicio de almacenamiento.

```PowerShell
PS C:\> Get-ClusterResource -Name "Storage Qos Resource"

Name                   State      OwnerGroup        ResourceType
----                   -----      ----------        ------------
Storage Qos Resource   Online     Cluster Group     Storage QoS Policy Manager
```

### <a name="set-up-storage-qos-on-a-compute-cluster"></a><a name="BKMK_SetupStorageQoSonComputeCluster"></a>Configuración de la calidad de servicio de almacenamiento en un clúster de proceso
El rol de Hyper-V en Windows Server 2016 tiene compatibilidad integrada para la calidad de servicio de almacenamiento y se configura de forma predeterminada.

#### <a name="install-remote-administration-tools-to-manage-storage-qos-policies-from-remote-computers"></a>Instalación de las herramientas de administración remota para administrar las directivas de calidad de servicio de almacenamiento de los equipos remotos
Puede administrar las directivas de calidad de servicio de almacenamiento y supervisar los flujos de los host de proceso por medio de las Herramientas de administración remota del servidor.  Puede encontrarlas como características opcionales en todas las instalaciones de Windows Server 2016, y pueden descargarse por separado para Windows 10 en el sitio web del [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=45520).

La característica opcional **RSAT-Clustering** incluye el módulo de Windows PowerShell para la administración remota de los clústeres de conmutación por error, como la calidad de servicio de almacenamiento.

-   Windows PowerShell: Add-WindowsFeature RSAT-Clustering

La característica opcional **RSAT-Hyper-V-Tools** incluye el módulo de Windows PowerShell para la administración remota de Hyper-V.

-   Windows PowerShell: Add-WindowsFeature RSAT-Hyper-V-Tools

#### <a name="deploy-virtual-machines-to-run-workloads-for-testing"></a>Implementación de máquinas virtuales para ejecutar cargas de trabajo con fines de prueba
Necesitará algunas máquinas virtuales almacenadas en el servidor de archivos de escalabilidad horizontal con cargas de trabajo relevantes.  Si desea obtener algunas sugerencias acerca de cómo simular la carga y realizar pruebas de esfuerzo, vea la página siguiente de una herramienta recomendada (DiskSpd) y algunos ejemplos de su uso: [DiskSpd, PowerShell and storage performance: measuring IOPs, throughput and latency for both local disks and SMB file shares](/archive/blogs/josebda/diskspd-powershell-and-storage-performance-measuring-iops-throughput-and-latency-for-both-local-disks-and-smb-file-shares) (DiskSpd, PowerShell y rendimiento del almacenamiento: medir E/S por segundo, rendimiento y latencia para discos locales y recursos compartidos de archivos de SMB).

Los escenarios de ejemplo que se incluyen en esta guía contienen cinco máquinas virtuales. BuildVM1, BuildVM2, BuildVM3 y BuildVM4 están ejecutando una carga de trabajo de escritorio con demandas de almacenamiento de bajas a moderadas. TestVm1 está ejecutando una prueba comparativa de procesamiento de transacciones en línea con una demanda de almacenamiento alta.

### <a name="view-current-storage-performance-metrics"></a>Visualización de las métricas de rendimiento de almacenamiento actuales
Esta sección incluye:

-   La consulta de flujos mediante el cmdlet `Get-StorageQosFlow`.

-   La visualización del rendimiento para un volumen mediante el cmdlet `Get-StorageQosVolume`.

#### <a name="query-flows-using-the-get-storageqosflow-cmdlet"></a>Flujos de consulta mediante el cmdlet Get-StorageQosFlow

El cmdlet Get-StorageQosFlow muestra todos los flujos actuales iniciados por servidores de Hyper-V. Todos los datos se recopilan por el clúster de servidores de archivos de escalabilidad horizontal; por tanto, el cmdlet puede usarse en cualquier nodo de dicho clúster o en un servidor remoto mediante el parámetro -CimSession.

**El siguiente comando de ejemplo muestra cómo ver todos los archivos abiertos por Hyper-V en el servidor mediante Get-StorageQoSFlow**.

```PowerShell
PS C:\> Get-StorageQosFlow

InitiatorName    InitiatorNodeNam StorageNodeName  FilePath        Status
                 e
-------------    ---------------- ---------------  --------        ------
                                  plang-fs3.pla... C:\ClusterSt... Ok
                                  plang-fs2.pla... C:\ClusterSt... Ok
                                  plang-fs1.pla... C:\ClusterSt... Ok
                                  plang-fs3.pla... C:\ClusterSt... Ok
                                  plang-fs2.pla... C:\ClusterSt... Ok
                                  plang-fs1.pla... C:\ClusterSt... Ok
TR20-VMM         plang-z400.pl... plang-fs1.pla... C:\ClusterSt... Ok
BuildVM4         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok
WinOltp1         plang-c1.plan... plang-fs1.pla... C:\ClusterSt... Ok
BuildVM3         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok
BuildVM1         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok
TR20-VMM         plang-z400.pl... plang-fs1.pla... C:\ClusterSt... Ok
BuildVM2         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok
TR20-VMM         plang-z400.pl... plang-fs1.pla... C:\ClusterSt... Ok
                                  plang-fs3.pla... C:\ClusterSt... Ok
                                  plang-fs2.pla... C:\ClusterSt... Ok
BuildVM4         plang-c2.plan... plang-fs2.pla... C:\ClusterSt... Ok
WinOltp1         plang-c1.plan... plang-fs2.pla... C:\ClusterSt... Ok
BuildVM3         plang-c2.plan... plang-fs2.pla... C:\ClusterSt... Ok
WinOltp1         plang-c1.plan... plang-fs2.pla... C:\ClusterSt... Ok
                                  plang-fs1.pla... C:\ClusterSt... Ok
```

El comando de ejemplo siguiente presenta un formato que le lleva a mostrar el nombre de máquina virtual, el nombre de host de Hyper-V, la E/S por segundo y el nombre de archivo de VHD, ordenados por la E/S por segundo.

```PowerShell
PS C:\> Get-StorageQosFlow | Sort-Object StorageNodeIOPs -Descending | ft InitiatorName, @{Expression={$_.InitiatorNodeName.Substring(0,$_.InitiatorNodeName.IndexOf('.'))};Label="InitiatorNodeName"}, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize

InitiatorName InitiatorNodeName StorageNodeIOPs Status File
------------- ----------------- --------------- ------ ----
WinOltp1      plang-c1                     3482     Ok IOMETER.VHDX
BuildVM2      plang-c2                      544     Ok BUILDVM2.VHDX
BuildVM1      plang-c2                      497     Ok BUILDVM1.VHDX
BuildVM4      plang-c2                      316     Ok BUILDVM4.VHDX
BuildVM3      plang-c2                      296     Ok BUILDVM3.VHDX
BuildVM4      plang-c2                      195     Ok WIN8RTM_ENTERPRISE_VL_BU...
TR20-VMM      plang-z400                    156     Ok DATA1.VHDX
BuildVM3      plang-c2                       81     Ok WIN8RTM_ENTERPRISE_VL_BU...
WinOltp1      plang-c1                       65     Ok BOOT.VHDX
                                             18     Ok DefaultFlow
                                             12     Ok DefaultFlow
WinOltp1      plang-c1                        4     Ok 9914.0.AMD64FRE.WINMAIN....
TR20-VMM      plang-z400                      4     Ok DATA2.VHDX
TR20-VMM      plang-z400                      3     Ok BOOT.VHDX
                                              0     Ok DefaultFlow
                                              0     Ok DefaultFlow
                                              0     Ok DefaultFlow
                                              0     Ok DefaultFlow
                                              0     Ok DefaultFlow
                                              0     Ok DefaultFlow
                                              0     Ok DefaultFlow
```

El siguiente comando de ejemplo muestra cómo filtrar los flujos en función del valor InitiatorName para encontrar fácilmente el rendimiento de almacenamiento y la configuración de una máquina virtual determinada.

```PowerShell
PS C:\> Get-StorageQosFlow -InitiatorName BuildVm1 | Format-List

FilePath           : C:\ClusterStorage\Volume2\SHARES\TWO\BUILDWORKLOAD\BUILDVM1.V
                     HDX
FlowId             : ebfecb54-e47a-5a2d-8ec0-0940994ff21c
InitiatorId        : ae4e3dd0-3bde-42ef-b035-9064309e6fec
InitiatorIOPS      : 464
InitiatorLatency   : 26.2684
InitiatorName      : BuildVM1
InitiatorNodeName  : plang-c2.plang.nttest.microsoft.com
Interval           : 300000
Limit              : 500
PolicyId           : b145e63a-3c9e-48a8-87f8-1dfc2abfe5f4
Reservation        : 500
Status             : Ok
StorageNodeIOPS    : 475
StorageNodeLatency : 7.9725
StorageNodeName    : plang-fs1.plang.nttest.microsoft.com
TimeStamp          : 2/12/2015 2:58:49 PM
VolumeId           : 4d91fc3a-1a1e-4917-86f6-54853b2a6787
PSComputerName     :
MaximumIops        : 500
MinimumIops        : 500
```

Los datos devueltos por el cmdlet `Get-StorageQosFlow` incluyen lo siguiente:

-   El nombre de host de Hyper-V (InitiatorNodeName).

-   El nombre de la máquina virtual y su identificador (InitiatorName e InitiatorId)

-   El rendimiento medio reciente observado por el host de Hyper-V para el disco virtual (InitiatorIOPS, InitiatorLatency)

-   El rendimiento medio reciente observado por el clúster de almacenamiento para el disco virtual (StorageNodeIOPS, StorageNodeLatency)

-   La directiva actual que se va a aplicar al archivo, si hay alguna, y la configuración resultante (PolicyId, Reservation, Limit)

-   Estado de la directiva

    -   **Ok**: no hay problemas

    -   InsufficientThroughput: se aplica una directiva, pero no se puede entregar la E/S por segundo mínima.  Esto puede suceder si el valor mínimo para una máquina virtual, o para todas las máquinas virtuales juntas, es superior a lo que volumen de almacenamiento puede ofrecer.

    -   **UnknownPolicyId**: se asignó una directiva a la máquina virtual en el host de Hyper-V, pero no está en el servidor de archivos.  Esta directiva debe quitarse de la configuración de la máquina virtual, o debe crearse una directiva coincidente en el clúster de servidores de archivos.

#### <a name="view-performance-for-a-volume-using-get-storageqosvolume"></a>Visualización del rendimiento para un volumen mediante Get-StorageQosVolume
Las métricas de rendimiento de almacenamiento también se recopilan en un nivel de volumen por almacenamiento, además de las métricas de rendimiento por flujo.  De este modo, es más fácil ver el uso promedio total en E/S por segundo normalizada, latencia y límites y reservas agregadas que se aplican a un volumen.

```PowerShell
PS C:\> Get-StorageQosVolume | Format-List

Interval       : 300000
IOPS           : 0
Latency        : 0
Limit          : 0
Reservation    : 0
Status         : Ok
TimeStamp      : 2/12/2015 2:59:38 PM
VolumeId       : 434f561f-88ae-46c0-a152-8c6641561504
PSComputerName :
MaximumIops    : 0
MinimumIops    : 0

Interval       : 300000
IOPS           : 1097
Latency        : 3.1524
Limit          : 0
Reservation    : 1568
Status         : Ok
TimeStamp      : 2/12/2015 2:59:38 PM
VolumeId       : 4d91fc3a-1a1e-4917-86f6-54853b2a6787
PSComputerName :
MaximumIops    : 0
MinimumIops    : 1568

Interval       : 300000
IOPS           : 5354
Latency        : 6.5084
Limit          : 0
Reservation    : 781
Status         : Ok
TimeStamp      : 2/12/2015 2:59:38 PM
VolumeId       : 0d2fd367-8d74-4146-9934-306726913dda
PSComputerName :
MaximumIops    : 0
MinimumIops    : 781
```

## <a name="how-to-create-and-monitor-storage-qos-policies"></a><a name="BKMK_CreateQoSPolicies"></a>Creación y supervisión de directivas de calidad de servicio de almacenamiento
En esta sección se describe cómo crear directivas de calidad de servicio de almacenamiento, aplicar estas directivas a máquinas virtuales y supervisar un clúster de almacenamiento una vez aplicadas.

### <a name="create-storage-qos-policies"></a>Creación de directivas de calidad de servicio de almacenamiento
Las directivas de calidad de servicio de almacenamiento se definen y administran en el clúster de servidores de archivos de escalabilidad horizontal.  Puede crear tantas directivas como sea necesario para las implementaciones flexibles (hasta 10 000 por clúster de almacenamiento).

Cada archivo VHD/VHDX asignado a una máquina virtual puede configurarse con una directiva. Los diferentes archivos y máquinas virtuales pueden usar la misma directiva, o bien cada uno se puede configurar con directivas independientes.  Si se configuran varios archivos VHD/VHDX o varias máquinas virtuales con la misma directiva, estos se agregarán juntos y compartirán en gran medida los valores MinimumIOPS y MaximumIOPS. Si utiliza directivas independientes para varios archivos VHD/VHDX o máquinas virtuales, los valores mínimo y máximo se supervisan por separado en cada caso.

Si crea varias directivas similares para diferentes máquinas virtuales y las máquinas virtuales tienen una igual demanda de almacenamiento, estas recibirán una cuota similar de E/S por segundo.  Si una máquina virtual requiere más y otra menos, la E/S por segundo se ajustará a esa demanda.

### <a name="types-of-storage-qos-policies"></a>Tipos de directivas de calidad de servicio de almacenamiento
Hay dos tipos de directivas: "Agregado" (anteriormente conocido como "Instancia única") y "Dedicado" (anteriormente conocido como "Instancias múltiples"). Las directivas de tipo agregado aplican valores máximos y mínimos para el conjunto combinado de archivos VHD/VHDX y máquinas virtuales al que se aplican. En efecto, comparten un conjunto especificado de E/S por segundo y ancho de banda. Las directivas de tipo dedicado aplican los valores mínimo y máximo para cada VHD/VHDx por separado. Esto facilita la creación de una única directiva que aplica límites similares a varios archivos VHD/VHDx.

Por ejemplo, si crea una directiva de tipo agregado con un mínimo de 300 E/S por segundo y un máximo de 500 E/S por segundo. Si se aplica esta directiva a 5 distintos archivos VHD/VHDx diferentes, se asegura de que los 5 archivos VHD/VHDx combinados tendrán al menos 300 E/S por segundo (si existe demanda y el sistema de almacenamiento puede proporcionar ese rendimiento) y no más de 500 E/S por segundo. Si los archivos VHD/VHDx tienen una demanda alta similar de E/S por segundo y el sistema de almacenamiento puede soportarla, cada archivo VHD/VHDx obtendrá sobre 100 E/S por segundo.

Sin embargo, si crea una directiva de tipo dedicado con límites similares y la aplica a los archivos VHD/VHDx en 5 máquinas virtuales diferentes, cada máquina virtual obtendrá por lo menos 300 E/S por segundo y no más de 500 E/S por segundo. Si las máquinas virtuales tienen una demanda alta similar de E/S por segundo y el sistema de almacenamiento puede soportarla, cada máquina virtual obtendrá sobre 500 E/S por segundo. .  Si una de las máquinas virtuales tiene varios archivos VHD/VHDx con la misma directiva de varias instancias configurada, estas compartirán el límite para que el total de E/S de la máquina virtual de archivos con esa directiva no supere los límites.

Por lo tanto, si tiene un grupo de archivos VHD/VHDx que desea que presenten las mismas características de rendimiento y no desea tener que crear varias directivas similares, puede utilizar una única directiva de tipo dedicado y aplicarla a los archivos de cada máquina virtual.

Mantenga el número de archivos VHD/VHDx asignados a una sola directiva agregada a 20 o menos.  Este tipo de Directiva se diseñó para realizar agregaciones con algunas máquinas virtuales en un clúster.

### <a name="create-and-apply-a-dedicated-policy"></a>Creación y aplicación de una directiva de tipo dedicado
En primer lugar, utilice el cmdlet `New-StorageQosPolicy` para crear una directiva en el servidor de archivos de escalabilidad horizontal tal como se muestra en el ejemplo siguiente:

```PowerShell
$desktopVmPolicy = New-StorageQosPolicy -Name Desktop -PolicyType Dedicated -MinimumIops 100 -MaximumIops 200
```

A continuación, aplíquela a las unidades de disco duro de las máquinas virtuales adecuadas en el servidor de Hyper-V.  Tenga en cuenta el valor PolicyId del paso anterior o almacénelo en una variable en los scripts.

En el servidor de archivos de escalabilidad horizontal, cree con PowerShell una directiva de calidad de servicio de almacenamiento y obtenga su identificador de directiva, tal como se muestra en el ejemplo siguiente:

```PowerShell
PS C:\> $desktopVmPolicy = New-StorageQosPolicy -Name Desktop -PolicyType Dedicated -MinimumIops 100 -MaximumIops 200

C:\> $desktopVmPolicy.PolicyId

Guid
----
cd6e6b87-fb13-492b-9103-41c6f631f8e0
```

En el servidor de Hyper-V, establezca con PowerShell la directiva de calidad de servicio de almacenamiento mediante el identificador de directiva, tal como se muestra en el ejemplo siguiente:

```PowerShell
Get-VM -Name Build* | Get-VMHardDiskDrive | Set-VMHardDiskDrive -QoSPolicyID cd6e6b87-fb13-492b-9103-41c6f631f8e0
```

### <a name="confirm-that-the-policies-are-applied"></a>Confirmación de la aplicación de las directivas
Use el cmdlet de PowerShell `Get-StorageQosFlow` para confirmar que se han aplicado los valores MinimumIOPs y MaximumIOPs a los flujos adecuados, tal como se muestra en el ejemplo siguiente.

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName |
 ft InitiatorName, Status, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize

InitiatorName Status MinimumIops MaximumIops StorageNodeIOPs Status File
------------- ------ ----------- ----------- --------------- ------ ----
BuildVM1          Ok         100         200             250     Ok BUILDVM1.VHDX
BuildVM2          Ok         100         200             251     Ok BUILDVM2.VHDX
BuildVM3          Ok         100         200             252     Ok BUILDVM3.VHDX
BuildVM4          Ok         100         200             233     Ok BUILDVM4.VHDX
TR20-VMM          Ok          33         666               1     Ok DATA2.VHDX
TR20-VMM          Ok          33         666               5     Ok DATA1.VHDX
TR20-VMM          Ok          33         666               4     Ok BOOT.VHDX
WinOltp1          Ok           0           0               0     Ok 9914.0.AMD6...
WinOltp1          Ok           0           0            5166     Ok IOMETER.VHDX
WinOltp1          Ok           0           0               0     Ok BOOT.VHDX
```

En el servidor de Hyper-V, también puede utilizar el script proporcionado **Get-VMHardDiskDrivePolicy.ps1** para ver qué directiva se aplica a una unidad de disco duro virtual.

```PowerShell
PS C:\> Get-VM -Name BuildVM1 | Get-VMHardDiskDrive | Format-List

Path                          : \\plang-fs.plang.nttest.microsoft.com\two\BuildWorkload
                                \BuildVM1.vhdx
DiskNumber                    :
MaximumIOPS                   : 0
MinimumIOPS                   : 0
QoSPolicyID                   : cd6e6b87-fb13-492b-9103-41c6f631f8e0
SupportPersistentReservations : False
ControllerLocation            : 0
ControllerNumber              : 0
ControllerType                : IDE
PoolName                      : Primordial
Name                          : Hard Drive
Id                            : Microsoft:AE4E3DD0-3BDE-42EF-B035-9064309E6FEC\83F8638B
                                -8DCA-4152-9EDA-2CA8B33039B4\0\0\D
VMId                          : ae4e3dd0-3bde-42ef-b035-9064309e6fec
VMName                        : BuildVM1
VMSnapshotId                  : 00000000-0000-0000-0000-000000000000
VMSnapshotName                :
ComputerName                  : PLANG-C2
IsDeleted                     : False
```

### <a name="query-for-storage-qos-policies"></a>Consulta de directivas de calidad de servicio de almacenamiento
`Get-StorageQosPolicy`muestra todas las directivas configuradas y su estado en un Servidor de archivos de escalabilidad horizontal.

```PowerShell
PS C:\> Get-StorageQosPolicy

Name                 MinimumIops          MaximumIops          Status
----                 -----------          -----------          ------
Default              0                    0                    Ok
Limit500             0                    500                  Ok
SilverVm             500                  500                  Ok
Desktop              100                  200                  Ok
Limit500             0                    0                    Ok
VMM                  100                  2000                 Ok
Vdi                  1                    100                  Ok
```

El estado puede cambiar con el tiempo según el rendimiento del sistema.

-   **Ok**: todos los flujos que usan esa directiva reciben la cantidad especificada por el valor MinimumIOPs.

-   **InsufficientThroughput**: uno o varios de los flujos que usan esa directiva no reciben la cantidad especificada por el valor MinimumIOPs.

También puede canalizar una directiva a `Get-StorageQosPolicy` para obtener el estado de todos los flujos configurados para usar la directiva como sigue:

```PowerShell
PS C:\> Get-StorageQosPolicy -Name Desktop | Get-StorageQosFlow | ft InitiatorName, *IOPS, Status, FilePath -AutoSize

InitiatorName MaximumIops MinimumIops InitiatorIOPS StorageNodeIOPS Status FilePat
                                                                           h
------------- ----------- ----------- ------------- --------------- ------ -------
BuildVM4              100          50           187              17     Ok C:\C...
BuildVM3              100          50           194              25     Ok C:\C...
BuildVM1              200         100           195             196     Ok C:\C...
BuildVM2              200         100           193             192     Ok C:\C...
BuildVM4              200         100           187             169     Ok C:\C...
BuildVM3              200         100           194             169     Ok C:\C...
```

### <a name="create-an-aggregated-policy"></a>Creación de una directiva de tipo agregado
Es posible utilizar directivas de tipo agregado si desea que varios discos duros virtuales compartan un único grupo de E/S por segundo y ancho de banda.  Por ejemplo, si se aplica la misma directiva de tipo agregado a discos duros de dos máquinas virtuales, el mínimo se dividirá entre ellos según la demanda.  Ambos discos tendrán un mínimo combinado garantizado, y juntos no superarán el máximo de E/S por segundo o el ancho de banda especificado.

También podría utilizarse el mismo enfoque para proporcionar una única asignación para todos los archivos VHD/VHDx para las máquinas virtuales que componen un servicio o que pertenecen a un inquilino en un entorno de varios hosts.

No hay ninguna diferencia en el proceso de crear directivas de tipo dedicado o agregado, salvo el valor PolicyType especificado.

En el ejemplo siguiente se muestra cómo crear una directiva de calidad de servicio de almacenamiento de tipo agregado y obtener su valor policyID en un servidor de archivos de escalabilidad horizontal:

```PowerShell
PS C:\> $highPerf = New-StorageQosPolicy -Name SqlWorkload -MinimumIops 1000 -MaximumIops 5000 -PolicyType Aggregated
[plang-fs]: PS C:\Users\plang\Documents> $highPerf.PolicyId

Guid
----
7e2f3e73-1ae4-4710-8219-0769a4aba072
```

En el ejemplo siguiente se muestra cómo aplicar la directiva de calidad de servicio de almacenamiento en un servidor Hyper-V usando el valor policyID obtenido en el ejemplo anterior:

```PowerShell
PS C:\> Get-VM -Name WinOltp1 | Get-VMHardDiskDrive | Set-VMHardDiskDrive -QoSPolicyID 7e2f3e73-1ae4-4710-8219-0769a4aba072
```

En el ejemplo siguiente se muestra cómo ver los efectos de la directiva de calidad de servicio de almacenamiento desde el servidor de archivos:

```PowerShell
PS C:\> Get-StorageQosFlow -InitiatorName WinOltp1 | format-list InitiatorName, PolicyId, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, FilePath

InitiatorName   : WinOltp1
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072
MinimumIops     : 250
MaximumIops     : 1250
StorageNodeIOPs : 0
FilePath        : C:\ClusterStorage\Volume2\SHARES\TWO\BASEVHD\9914.0.AMD64FRE.WIN
                  MAIN.141218-1718_SERVER_SERVERDATACENTER_EN-US.VHDX

InitiatorName   : WinOltp1
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072
MinimumIops     : 250
MaximumIops     : 1250
StorageNodeIOPs : 0
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\BOOT.VHDX

InitiatorName   : WinOltp1
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072
MinimumIops     : 1000
MaximumIops     : 5000
StorageNodeIOPs : 4550
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX
PS C:\> Get-StorageQosFlow -InitiatorName WinOltp1 | for
mat-list InitiatorName, PolicyId, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, FilePath

InitiatorName   : WinOltp1
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072
MinimumIops     : 250
MaximumIops     : 1250
StorageNodeIOPs : 0
FilePath        : C:\ClusterStorage\Volume2\SHARES\TWO\BASEVHD\9914.0.AMD64FRE.WIN
                  MAIN.141218-1718_SERVER_SERVERDATACENTER_EN-US.VHDX

InitiatorName   : WinOltp1
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072
MinimumIops     : 250
MaximumIops     : 1250
StorageNodeIOPs : 0
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\BOOT.VHDX

InitiatorName   : WinOltp1
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072
MinimumIops     : 1000
MaximumIops     : 5000
StorageNodeIOPs : 4550
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX
```

Cada disco duro virtual tendrá el valor MinimumIOPs y MaximumIOPs y MaximumIobandwidth ajustado según su carga.  Esto garantiza que la cantidad total de ancho de banda utilizada para el grupo de discos permanezca dentro del intervalo definido por la directiva.  En el ejemplo anterior, los dos primeros discos están inactivos, y el tercero tiene permitido utilizar hasta el máximo de E/S por segundo.  Si los dos primeros discos empiezan a emitir de nuevo E/S, el máximo de E/S por segundo del tercer disco se reducirá automáticamente.

### <a name="modify-an-existing-policy"></a>Modificación de una directiva existente
Después de crear una directiva, es posible cambiar las propiedades de Name, MinimumIOPs, MaximumIOPs y MaximumIoBandwidth.  Sin embargo, no se puede cambiar el tipo de directiva (agregado/dedicado) una vez creada esta.

El siguiente cmdlet de Windows PowerShell muestra cómo cambiar la propiedad MaximumIOPs para una directiva existente:

```PowerShell
[DBG]: PS C:\demoscripts>> Get-StorageQosPolicy -Name SqlWorkload | Set-StorageQosPolicy -MaximumIops 6000
```

El siguiente cmdlet comprueba el cambio:

```PowerShell
PS C:\> Get-StorageQosPolicy -Name SqlWorkload

Name                    MinimumIops            MaximumIops            Status
----                    -----------            -----------            ------
SqlWorkload             1000                   6000                   Ok

[plang-fs1]: PS C:\Users\plang\Documents> Get-StorageQosPolicy -Name SqlWorkload | Get-Storag
eQosFlow | Format-Table InitiatorName, PolicyId, MaximumIops, MinimumIops, StorageNodeIops -A
utoSize

InitiatorName PolicyId                             MaximumIops MinimumIops StorageNodeIops
------------- --------                             ----------- ----------- ---------------
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072        1500         250               0
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072        1500         250               0
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072        6000        1000            4507
```

## <a name="how-to-identify-and-address-common-issues"></a><a name="BKMK_KnownIssues"></a>Identificación y resolución de problemas habituales
En esta sección se describe cómo detectar las máquinas virtuales con directivas de calidad de servicio de almacenamiento no válidas, cómo volver a crear una directiva coincidente, cómo quitar una directiva de una máquina virtual y cómo identificar las máquinas virtuales que no cumplen los requisitos de la directiva de calidad de servicio de almacenamiento.

### <a name="identify-virtual-machines-with-invalid-policies"></a><a name="BKMK_FindingVMsWithInvalidPolicies"></a>Identificación de máquinas virtuales con directivas no válidas

Si se elimina una directiva desde el servidor de archivos antes de quitarla de una máquina virtual, la máquina virtual se seguirá ejecutando como si no se hubiera aplicado ninguna directiva.

```PowerShell
PS C:\> Get-StorageQosPolicy -Name SqlWorkload | Remove-StorageQosPolicy

Confirm
Are you sure you want to perform this action?
Performing the operation "DeletePolicy" on target "MSFT_StorageQoSPolicy (PolicyId =
"7e2f3e73-1ae4-4710-8219-0769a4aba072")".
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [?] Help (default is "Y"):
```

El estado de los flujos aparecerá ahora como "UnknownPolicyId"

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, Status, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize

InitiatorName          Status MinimumIops MaximumIops StorageNodeIOPs          Status File
-------------          ------ ----------- ----------- ---------------          ------ ----
                           Ok           0           0               0              Ok Def...
                           Ok           0           0              10              Ok Def...
                           Ok           0           0              13              Ok Def...
                           Ok           0           0               0              Ok Def...
                           Ok           0           0               0              Ok Def...
                           Ok           0           0               0              Ok Def...
                           Ok           0           0               0              Ok Def...
                           Ok           0           0               0              Ok Def...
                           Ok           0           0               0              Ok Def...
BuildVM1                   Ok         100         200             193              Ok BUI...
BuildVM2                   Ok         100         200             196              Ok BUI...
BuildVM3                   Ok          50          64              17              Ok WIN...
BuildVM3                   Ok          50         136             179              Ok BUI...
BuildVM4                   Ok          50         100              23              Ok WIN...
BuildVM4                   Ok         100         200             173              Ok BUI...
TR20-VMM                   Ok          33         666               2              Ok DAT...
TR20-VMM                   Ok          25         975               3              Ok DAT...
TR20-VMM                   Ok          75        1025              12              Ok BOO...
WinOltp1      UnknownPolicyId           0           0               0 UnknownPolicyId 991...
WinOltp1      UnknownPolicyId           0           0            4926 UnknownPolicyId IOM...
WinOltp1      UnknownPolicyId           0           0               0 UnknownPolicyId BOO...
```

#### <a name="recreate-a-matching-storage-qos-policy"></a><a name="BKMK_RecreateMatchingPolicy"></a>Nueva creación de una directiva de calidad de servicio de almacenamiento coincidente
Si se eliminó por error una directiva, puede crear una nueva utilizando el valor PolicyId anterior.  En primer lugar, obtenga el valor PolicyId necesario.

```PowerShell
PS C:\> Get-StorageQosFlow -Status UnknownPolicyId | ft InitiatorName, PolicyId -AutoSize

InitiatorName PolicyId
------------- --------
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072
```

A continuación, cree una nueva directiva con ese valor PolicyId.

```PowerShell
PS C:\> New-StorageQosPolicy -PolicyId 7e2f3e73-1ae4-4710-8219-0769a4aba072 -PolicyType Aggregated -Name RestoredPolicy -MinimumIops 100 -MaximumIops 2000

Name                    MinimumIops            MaximumIops            Status
----                    -----------            -----------            ------
RestoredPolicy          100                    2000                   Ok
```

Por último, compruebe que se ha aplicado.

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, Status, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize

InitiatorName Status MinimumIops MaximumIops StorageNodeIOPs Status File
------------- ------ ----------- ----------- --------------- ------ ----
                  Ok           0           0               0     Ok DefaultFlow
                  Ok           0           0               8     Ok DefaultFlow
                  Ok           0           0               9     Ok DefaultFlow
                  Ok           0           0               0     Ok DefaultFlow
                  Ok           0           0               0     Ok DefaultFlow
                  Ok           0           0               0     Ok DefaultFlow
                  Ok           0           0               0     Ok DefaultFlow
                  Ok           0           0               0     Ok DefaultFlow
                  Ok           0           0               0     Ok DefaultFlow
BuildVM1          Ok         100         200             192     Ok BUILDVM1.VHDX
BuildVM2          Ok         100         200             193     Ok BUILDVM2.VHDX
BuildVM3          Ok          50         100              24     Ok WIN8RTM_ENTERPRISE_VL...
BuildVM3          Ok         100         200             166     Ok BUILDVM3.VHDX
BuildVM4          Ok          50         100              12     Ok WIN8RTM_ENTERPRISE_VL...
BuildVM4          Ok         100         200             178     Ok BUILDVM4.VHDX
TR20-VMM          Ok          33         666               2     Ok DATA2.VHDX
TR20-VMM          Ok          33         666               2     Ok DATA1.VHDX
TR20-VMM          Ok          33         666              10     Ok BOOT.VHDX
WinOltp1          Ok          25         500               0     Ok 9914.0.AMD64FRE.WINMA...
```

#### <a name="remove-storage-qos-policies"></a><a name="BKMK_RemovePolicyFromVM"></a>Eliminación de directivas de calidad de servicio de almacenamiento

Si la directiva se ha quitado de forma intencionada, o si se ha importado una máquina virtual con una directiva que no necesita, puede quitarla.

```PowerShell
PS C:\> Get-VM -Name WinOltp1 | Get-VMHardDiskDrive | Set-VMHardDiskDrive -QoSPolicyID $null
```

Una vez que el valor PolicyId se quite de la configuración de disco duro virtual, el estado será "Ok" y no se aplicará ningún mínimo ni máximo.

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize

InitiatorName MinimumIops MaximumIops StorageNodeIOPs Status File
------------- ----------- ----------- --------------- ------ ----
                        0           0               0     Ok DefaultFlow
                        0           0              16     Ok DefaultFlow
                        0           0              12     Ok DefaultFlow
                        0           0               0     Ok DefaultFlow
                        0           0               0     Ok DefaultFlow
                        0           0               0     Ok DefaultFlow
                        0           0               0     Ok DefaultFlow
                        0           0               0     Ok DefaultFlow
                        0           0               0     Ok DefaultFlow
BuildVM1              100         200             197     Ok BUILDVM1.VHDX
BuildVM2              100         200             192     Ok BUILDVM2.VHDX
BuildVM3                9           9              23     Ok WIN8RTM_ENTERPRISE_VL_BUILDW...
BuildVM3               91         191             171     Ok BUILDVM3.VHDX
BuildVM4                8           8              18     Ok WIN8RTM_ENTERPRISE_VL_BUILDW...
BuildVM4               92         192             163     Ok BUILDVM4.VHDX
TR20-VMM               33         666               2     Ok DATA2.VHDX
TR20-VMM               33         666               1     Ok DATA1.VHDX
TR20-VMM               33         666               5     Ok BOOT.VHDX
WinOltp1                0           0               0     Ok 9914.0.AMD64FRE.WINMAIN.1412...
WinOltp1                0           0            1811     Ok IOMETER.VHDX
WinOltp1                0           0               0     Ok BOOT.VHDX
```

### <a name="find-virtual-machines-that-are-not-meeting-storage-qos-policies"></a><a name="BKMK_VMsThatDoNotMeetStorageQoSPoilicies"></a>Búsqueda de máquinas virtuales que no cumplen las directivas de calidad de servicio de almacenamiento
El estado **InsufficientThroughput** se asigna a los flujos que cumplen estos requisitos:

-   Tienen una cantidad mínima de E/S por segundo definida por directiva.

-   Están iniciando la E/S a una tasa que coincide con el mínimo o lo supera.

-   No alcanzan la tasa de E/S por segundo mínima.

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize

InitiatorName MinimumIops MaximumIops StorageNodeIOPs                 Status File
------------- ----------- ----------- ---------------                 ------ ----
                        0           0               0                     Ok DefaultFlow
                        0           0               0                     Ok DefaultFlow
                        0           0              15                     Ok DefaultFlow
                        0           0               0                     Ok DefaultFlow
                        0           0               0                     Ok DefaultFlow
                        0           0               0                     Ok DefaultFlow
                        0           0               0                     Ok DefaultFlow
                        0           0               0                     Ok DefaultFlow
                        0           0               0                     Ok DefaultFlow
BuildVM3               50         100              20                     Ok WIN8RTM_ENTE...
BuildVM3              100         200             174                     Ok BUILDVM3.VHDX
BuildVM4               50         100              11                     Ok WIN8RTM_ENTE...
BuildVM4              100         200             188                     Ok BUILDVM4.VHDX
TR20-VMM               33         666               3                     Ok DATA1.VHDX
TR20-VMM               78        1032             180                     Ok BOOT.VHDX
TR20-VMM               22         968               4                     Ok DATA2.VHDX
WinOltp1             3750        5000               0                     Ok 9914.0.AMD64...
WinOltp1            15000       20000           11679 InsufficientThroughput IOMETER.VHDX
WinOltp1             3750        5000               0                     Ok BOOT.VHDX
```

Puede determinar flujos para cualquier estado, incluido **InsufficientThroughput**, tal como se muestra en el ejemplo siguiente:

```PowerShell
PS C:\> Get-StorageQosFlow -Status InsufficientThroughput | fl

FilePath           : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX
FlowId             : 1ca356ff-fd33-5b5d-b60a-2c8659dc803e
InitiatorId        : 2ceabcef-2eba-4f1b-9e66-10f960b50bbf
InitiatorIOPS      : 12168
InitiatorLatency   : 22.983
InitiatorName      : WinOltp1
InitiatorNodeName  : plang-c1.plang.nttest.microsoft.com
Interval           : 300000
Limit              : 20000
PolicyId           : 5d1bf221-c8f0-4368-abcf-aa139e8a7c72
Reservation        : 15000
Status             : InsufficientThroughput
StorageNodeIOPS    : 12181
StorageNodeLatency : 22.0514
StorageNodeName    : plang-fs2.plang.nttest.microsoft.com
TimeStamp          : 2/13/2015 12:07:30 PM
VolumeId           : 0d2fd367-8d74-4146-9934-306726913dda
PSComputerName     :
MaximumIops        : 20000
MinimumIops        : 15000
```

## <a name="monitor-health-using-storage-qos"></a><a name="BKMK_Health"></a>Supervisión del estado con la calidad de servicio de almacenamiento
El nuevo servicio de mantenimiento simplifica la supervisión del clúster de almacenamiento, proporcionando un único lugar para buscar eventos accionables en cualquiera de los nodos. En esta sección se describe cómo supervisar el estado de su clúster de almacenamiento con el cmdlet `debug-storagesubsystem`.

### <a name="view-storage-status-with-debug-storagesubsystem"></a>Visualización del estado de almacenamiento con Debug-StorageSubSystem
Los espacios de almacenamiento en clúster también proporcionan información sobre el estado del clúster de almacenamiento en una sola ubicación. Esto puede servir de ayuda a los administradores para detectar rápidamente los problemas existentes en las implementaciones de almacenamiento y supervisar las incidencias cuando llegan o se desestiman.

#### <a name="vm-with-invalid-policy"></a>Máquina virtual con directiva no válida
Las máquinas virtuales con directivas no válidas también se notifican a través de la supervisión de estado del subsistema de almacenamiento.  Este ejemplo procede del mismo estado que el descrito en la sección [Búsqueda de máquinas virtuales con directivas no válidas](#BKMK_FindingVMsWithInvalidPolicies) de este documento.

```PowerShell
C:\> Get-StorageSubSystem -FriendlyName Clustered* | Debug-StorageSubSystem

EventTime                 :
FaultId                   : 0d16d034-9f15-4920-a305-f9852abf47c3
FaultingObject            :
FaultingObjectDescription : Storage QoS Policy 5d1bf221-c8f0-4368-abcf-aa139e8a7c72
FaultingObjectLocation    :
FaultType                 : Storage QoS policy used by consumer does not exist.
PerceivedSeverity         : Minor
Reason                    : One or more storage consumers (usually Virtual Machines) are
                            using a non-existent policy with id
                            5d1bf221-c8f0-4368-abcf-aa139e8a7c72. Consumer details:

                            Flow ID: 1ca356ff-fd33-5b5d-b60a-2c8659dc803e
                            Initiator ID: 2ceabcef-2eba-4f1b-9e66-10f960b50bbf
                            Initiator Name: WinOltp1
                            Initiator Node: plang-c1.plang.nttest.microsoft.com
                            File Path:
                            C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX
RecommendedActions        : {Reconfigure the storage consumers (usually Virtual Machines)
                            to use a valid policy., Recreate any missing Storage QoS
                            policies.}
PSComputerName            :
```

#### <a name="lost-redundancy-for-a-storage-spaces-virtual-disk"></a>Pérdida de redundancia para un disco virtual de espacios de almacenamiento

En este ejemplo, un espacio de almacenamiento en clúster tiene un disco virtual creado como un reflejo triple.  Se quitó un disco con errores del sistema, pero no se agregó un disco de reemplazo.  El subsistema de almacenamiento está informando de una pérdida de redundancia con el valor **Warning** en HealthStatus, pero OperationalStatus presenta **OK** porque el volumen está aún en línea.

```PowerShell
PS C:\> Get-StorageSubSystem -FriendlyName Clustered*

FriendlyName                   HealthStatus                   OperationalStatus
------------                   ------------                   -----------------
Clustered Windows Storage o... Warning                        OK

[plang-fs1]: PS C:\Users\plang\Documents> Get-StorageSubSystem -FriendlyName Clustered* | Deb
ug-StorageSubSystem

EventTime                 :
FaultId                   : dfb4b672-22a6-4229-b2ed-c29d7485bede
FaultingObject            :
FaultingObjectDescription : Virtual disk 'Two'
FaultingObjectLocation    :
FaultType                 : VirtualDiskDegradedFaultType
PerceivedSeverity         : Minor
Reason                    : Virtual disk 'Two' does not have enough redundancy remaining to
                            successfully repair or regenerate its data.
RecommendedActions        : {Rebalance the pool, replace failed physical disks, or add new
                            physical disks to the storage pool, then repair the virtual
                            disk.}
PSComputerName            :
```

### <a name="sample-script-for-continuous-monitoring-of-storage-qos"></a>Script de ejemplo para la supervisión continua de la calidad de servicio de almacenamiento

Esta sección incluye un script de ejemplo que muestra cómo se pueden supervisar los errores comunes mediante un script de WMI.  Se ha diseñado como punto de partida para que los desarrolladores recuperen eventos de estado en tiempo real.

**Script de ejemplo:**

```PowerShell
param($cimSession)
# Register and display events
Register-CimIndicationEvent -Namespace root\microsoft\windows\storage -ClassName msft_storagefaultevent -CimSession $cimSession

while ($true)
{
     $e = (Wait-Event)
     $e.SourceEventArgs.NewEvent
     Remove-Event $e.SourceIdentifier
}
```

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="how-do-i-retain-a-storage-qos-policy-being-enforced-for-my-virtual-machine-if-i-move-its-vhdvhdx-files-to-another-storage-cluster"></a>¿Cómo se puede mantener una directiva de calidad de servicio de almacenamiento aplicada a mi máquina virtual si muevo sus archivos VHD/VHDx a otro clúster de almacenamiento?

La configuración del archivo VHD/VHDx que especifica la directiva es el GUID de un identificador de directiva.  Cuando se crea una directiva, el GUID puede especificarse mediante el parámetro **PolicyID**.  Si no se especifica este parámetro, se crea un GUID aleatorio.  Por lo tanto, puede obtener el valor PolicyID en el clúster de almacenamiento en el que las máquinas virtuales almacenan actualmente sus archivos VHD/VHDx y crear una directiva idéntica en el clúster de almacenamiento de destino y, a continuación, especificar que se cree con el mismo GUID.  Cuando se muevan los archivos de máquinas virtuales a los nuevos clústeres de almacenamiento, la directiva con el mismo GUID surtirá efecto.

System Center Virtual Machine Manager puede utilizarse para aplicar directivas en varios clústeres de almacenamiento, lo que facilita mucho esta situación.
### <a name="if-i-change-the-storage-qos-policy-why-dont-i-see-it-take-effect-immediately-when-i-run-get-storageqosflow"></a>Si se cambia la directiva de calidad de servicio de almacenamiento, ¿por qué no veo que surta efecto de inmediato al ejecutar Get-StorageQoSFlow?

Si tiene un flujo que llega al máximo de una directiva y cambia la directiva para establecer un límite inferior o superior y, a continuación, determina de inmediato la latencia/la E/S por segundo/el ancho de banda de los flujos que usan los cmdlets de PowerShell, transcurrirán hasta 5 minutos hasta que surtan efecto total los cambios de la directiva en los flujos.  Los nuevos límites entrarán en vigor en pocos segundos, pero el cmdlet **Get-StorgeQoSFlow** de PowerShell utiliza un promedio de cada contador con una ventana deslizante de 5 minutos.  De no ser así, si se mostrase un valor actual y se ejecutara el cmdlet de PowerShell varias veces consecutivas, podrían aparecer valores muy diferentes porque los valores de E/S por segundo y las latencias pueden fluctuar considerablemente de un segundo a otro.

### <a name="what-new-functionality-was-added-in-windows-server-2016"></a><a name="BKMK_Updates"></a>¿Qué nueva funcionalidad se ha agregado en Windows Server 2016?

En Windows Server 2016, se ha cambiado el nombre del tipo de directiva de calidad de servicio de almacenamiento.  El tipo de directiva **Instancias múltiples** se denomina ahora **Dedicado** y el tipo **Instancia única** se denomina **Agregado**. También se ha modificado el comportamiento de administración de las directivas dedicadas: los archivos VHD/VHDX dentro de la misma máquina virtual que tienen aplicada la misma directiva de tipo **Dedicado** no compartirán las asignaciones de E/S.

Hay dos nuevas características de QoS de almacenamiento en Windows Server 2016:

-   **Ancho de banda máximo**

    La calidad de servicio de almacenamiento de Windows Server 2016 introduce la posibilidad de especificar el ancho de banda máximo que pueden consumir los flujos asignados a la directiva.  El parámetro cuando se especifica en los cmdlets **StorageQosPolicy** es **MaximumIOBandwidth** y el resultado se expresa en bytes por segundo.
    Si se establecen en una directiva los valores **MaximimIops** y **MaximumIOBandwidth**, los dos estarán en vigor y aquel que sea el primero al que lleguen los flujos limitará la E/S de estos.

-   **La normalización de E/S por segundo es configurable.**

    La calidad de servicio de almacenamiento utiliza la normalización de E/S por segundo.  Como valor predeterminado, se usa un tamaño de normalización de 8K.  La calidad de servicio de almacenamiento de Windows Server 2016 introduce la posibilidad de especificar un tamaño de normalización diferente para el clúster de almacenamiento.  Este tamaño de normalización afecta a todos los flujos del clúster de almacenamiento y surte efecto de inmediato (a los pocos segundos) una vez que se cambia.  El valor mínimo es 1 KB y el máximo es 4 GB (se recomienda no establecer más de 4 MB, ya que no es habitual tener más de 4 MB de E/S).

    Debe tener en cuenta que el mismo rendimiento/patrón de E/S muestra diferentes números de E/S por segundo en el resultado de la calidad de servicio de almacenamiento cuando se cambia la normalización de E/S por segundo debido al cambio en el cálculo de la normalización.  Si se comparan las E/S por segundo entre grupos de almacenamiento, también puede comprobar qué valor de normalización está usando cada uno, ya que esto afectará a la E/S por segundo normalizada que se notifica.

#### <a name="example-1-creating-a-new-policy-and-viewing-the-maximum-bandwidth-on-the-storage-cluster"></a>Ejemplo 1: creación de una nueva directiva y visualización del ancho de banda máximo en el clúster de almacenamiento
En PowerShell, puede especificar las unidades en que se expresa una cantidad.  En el ejemplo siguiente, 10 MB es el valor de ancho de banda máximo.  La calidad de servicio de almacenamiento lo convertirá y lo guardará como bytes por segundo. De este modo, 10 MB se convierten en 10 485 760 bytes por segundo.

```PowerShell
PS C:\Windows\system32> New-StorageQosPolicy -Name HR_VMs -MaximumIops 1000 -MinimumIops 20 -MaximumIOBandwidth 10MB

Name   MinimumIops MaximumIops MaximumIOBandwidth Status
----   ----------- ----------- ------------------ ------
HR_VMs 20          1000        10485760           Ok

PS C:\Windows\system32> Get-StorageQosPolicy

Name    MinimumIops MaximumIops MaximumIOBandwidth Status
----    ----------- ----------- ------------------ ------
Default 0           0           0                  Ok
HR_VMs  20          1000        10485760           Ok

PS C:\Windows\system32> Get-StorageQoSFlow | fL InitiatorName,FilePath,InitiatorIOPS,InitiatorLatency,InitiatorBandwidth

InitiatorName      : testsQoS
FilePath           : C:\ClusterStorage\Volume2\TESTSQOS\VIRTUAL HARD DISKS\TESTSQOS.VHDX
InitiatorIOPS      : 5
InitiatorLatency   : 1.5455
InitiatorBandwidth : 37888
```

#### <a name="example-2-get-iops-normalization-settings-and-specify--a-new-value"></a>Ejemplo 2: obtención de la configuración de la normalización de E/S por segundo y especificación de un valor nuevo

En el ejemplo siguiente se muestra cómo obtener la configuración de la normalización de E/S por segundo de los clústeres de almacenamiento (valor predeterminado de 8 KB), luego establecerlo en 32 KB y, finalmente, mostrarlo de nuevo.  Tenga en cuenta que, en este ejemplo, se especifica "32 KB", ya que PowerShell permite especificar la unidad en lugar de requerir la conversión a bytes.   El resultado muestra el valor en bytes por segundo.

```PowerShell
PS C:\Windows\system32> Get-StorageQosPolicyStore

IOPSNormalizationSize
---------------------
8192

PS C:\Windows\system32> Set-StorageQosPolicyStore -IOPSNormalizationSize 32KB
PS C:\Windows\system32> Get-StorageQosPolicyStore

IOPSNormalizationSize
---------------------
32768
```

## <a name="see-also"></a>Consulte también
- [Windows Server 2016](../../index.yml)
- [Réplica de almacenamiento en Windows Server 2016](../storage-replica/storage-replica-overview.md)
- [Espacios de almacenamiento directo en Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)