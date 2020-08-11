---
title: Implementar un SOFS en Espacios de almacenamiento directo de dos nodos para almacenar UPD en Azure
description: Aprende a usar Espacios de almacenamiento directo con RDS.
ms.topic: article
ms.assetid: 1099f21d-5f07-475a-92dd-ad08bc155da1
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
manager: scottman
ms.openlocfilehash: 92359ee40bb33cd720515788060eae47d8915715
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954852"
---
# <a name="deploy-a-two-node-storage-spaces-direct-scale-out-file-server-for-upd-storage-in-azure"></a>Implementar un servidor de archivos de escalabilidad horizontal de Espacios de almacenamiento directo de dos nodos para almacenar UPD en Azure

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Los Servicios de Escritorio remoto (RDS) requieren un servidor de archivos unido al dominio para los discos de perfil de usuario (UPD). Para implementar un servidor de archivos de escalabilidad horizontal unido al dominio de alta disponibilidad (SOFS) en Azure, usa Espacios de almacenamiento directo con Windows Server 2016. Si no estás familiarizado con los UPD o los Servicios de Escritorio Remoto, consulta [Bienvenida a Servicios de Escritorio remoto](welcome-to-rds.md).

> [!NOTE]
> Microsoft acaba de publicar una [plantilla de Azure para implementar un servidor de archivos de escalabilidad horizontal de Espacios de almacenamiento directo](https://azure.microsoft.com/documentation/templates/301-storage-spaces-direct/). Puedes usar la plantilla para crear tu implementación o seguir los pasos de este artículo.

Es recomendable implementar el SOFS con VM de la serie DS y discos de datos de almacenamiento premium, donde hay la misma cantidad de discos de datos en cada VM; además, estos discos tienen el mismo tamaño. Necesitarás un mínimo de dos cuentas de almacenamiento. 

Para realizar implementaciones pequeñas, te recomendamos un clúster de dos nodos con un testigo en la nube, donde el volumen se refleja con dos copias. Aumenta el tamaño de pequeñas implementaciones agregando discos de datos. Crea implementaciones más grandes agregando nodos (VM). 

Estas instrucciones son para una implementación de dos nodos. En la siguiente tabla se muestran los tamaños de VM y disco que necesitarás para almacenar los UPD en función del número de usuarios en tu negocio. 

| Usuarios | Total (GB) | Máquina virtual | # discos | Tipo de disco | Tamaño del disco (GB) | Configuración   |
|-------|------------|----|---------|-----------|----------------|-----------------|
| 10    | 50         | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 25    | 125        | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 50    | 250        | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 100   | 500        | DS1 | 2       | P20       | 512            | 2x(DS1 + 2 P20)  |
| 250   | 1250       | DS1 | 2       | P30       | 1024           | 2x(DS1 + 2 P30)  |
| 500   | 2500       | DS2 | 3       | P30       | 1024           | 2x(DS2 + 3 P30)  |
| 1000  | 5000       | DS3 | 5       | P30       | 1024           | 2x(DS3 + 5 P30)  |
| 2500  | 12500      | DS4 | 13      | P30       | 1024           | 2x(DS4 + 13 P30) |
| 5000  | 25000      | DS5 | 25      | P30       | 1024           | 2x(DS5 + 25 P30) |

Sigue estos pasos para crear un controlador de dominio (el nuestro tiene el nombre "my-dc") y dos nodos de VM ("my-fsn1" y "my-fsn2") y configura las VM para que sean un SOFS de Espacios de almacenamiento directo de dos nodos.

1. Crea una [suscripción a Microsoft Azure](https://azure.microsoft.com).
2. Inicia sesión en [Azure Portal](https://ms.portal.azure.com).
3. Crea una [cuenta de Azure Storage](/azure/storage/common/storage-account-create#create-a-storage-account) en Azure Resource Manager. Créala en un grupo de recursos nuevo y usa las siguientes configuraciones:
   - Modelo de implementación: Resource Manager
   - Tipo de cuenta de almacenamiento: Uso general
   - Nivel de rendimiento: Premium
   - Opción de replicación: LRS
4. Configura un bosque de Active Directory mediante una plantilla de inicio rápido o implementando el bosque manualmente.
   - Para realizar la implementación con una plantilla de inicio rápido de Azure:
      - [Create an Azure VM with a new AD forest](https://azure.microsoft.com/documentation/templates/active-directory-new-domain/) (Crear una VM de Azure con un nuevo bosque de AD)
      - [Create a new AD domain with 2 domain controllers](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/) (Crear un nuevo dominio de AD con dos controladores de dominio) (para alta disponibilidad)
   - Para [implementar el bosque](../../identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100.md) de forma manual con las siguientes configuraciones:
      - Crea la red virtual en el mismo grupo de recursos que la cuenta de almacenamiento.
      - Tamaño recomendado: DS2 (aumenta el tamaño si el controlador de dominio va a hospedar más objetos de dominio)
      - Usa una red virtual generada automáticamente.
      - Sigue los pasos indicados para instalar AD DS.
5. Configura los nodos del clúster de servidores de archivos. Para ello, implementa la [plantilla de Azure del clúster de SOFS de Espacios de almacenamiento directo para Windows Server 2016](https://azure.microsoft.com/resources/templates/301-storage-spaces-direct/) o sigue los pasos 6-11 para realizar la implementación manual.
6. Para configurar manualmente los nodos del clúster del servidor de archivos:
   1. Crea el primer nodo:
      1. Crea una nueva máquina virtual mediante la imagen de Windows Server 2016. (Haz clic en **Nuevo > Máquinas virtuales > Windows Server 2016.** Selecciona **Resource Manager** y, a continuación, haz clic en **Crear**.)
      2. Establece la configuración básica de la manera siguiente:
         - Nombre: my-fsn1
         - SSD del tipo de disco de VM
         - Usa un grupo de recursos existente, por ejemplo, el que ya has creado en el paso 3.
      3. Tamaño: DS1, DS2, DS3, DS4 o DS5 según las necesidades del usuario (consulta la tabla al comienzo de estas instrucciones). Asegúrate de que se haya seleccionado la compatibilidad con discos premium.
      4. Configuración:
         - Cuenta de almacenamiento: Elige la cuenta de almacenamiento que creaste en el paso tres.
         - Alta disponibilidad: crea un nuevo conjunto de disponibilidad. (Haz clic en **Alta disponibilidad > Crear nuevo** y escribe un nombre (por ejemplo, s2d-cluster). Usa los valores predeterminados para **Actualizar dominios** y **Dominios de error**.)
   2. Crea el segundo nodo. Repite el paso anterior con los siguientes cambios:
      - Nombre: my-fsn2
      - Alta disponibilidad: selecciona el conjunto de disponibilidad que creaste anteriormente.
7. [Adjunta los discos de datos](/azure/virtual-machines/windows/attach-managed-disk-portal) a las VM del nodo de clúster según tus necesidades de usuario (tal como se indica en la tabla anterior). Después de crear los discos de datos y adjuntarlos a la máquina virtual, configura el **almacenamiento en caché del servidor** como **Ninguno**.
8. Asimismo, debes establecer las direcciones IP de todas las VM con el estado **Estática**.
   1. En el grupo de recursos, selecciona una VM y, a continuación, haz clic en **Interfaces de red** (está en la sección **Configuración**). Selecciona la interfaz de red en la lista y, a continuación, haz clic en **Configuraciones de IP**. Selecciona la configuración de IP en la lista, selecciona **Estático** y, a continuación, haz clic en **Guardar**.
   2. Ten en cuenta que la dirección IP privada (10.x.x.x) del controlador de dominio (que es my-dc, en nuestro ejemplo).
9. Establece la dirección del servidor DNS principal en las NIC de las VM del nodo de clúster en el servidor my-dc. Selecciona la VM y haz clic en **Interfaces de red > Servidores DNS > DNS personalizado**. Escribe la dirección IP privada que anotaste anteriormente y haz clic en **Guardar**.
10. Crea una [cuenta de almacenamiento de Azure para que sea tu testigo en la nube](../../failover-clustering/deploy-cloud-witness.md). (Si usas las instrucciones vinculadas, detente cuando llegues a "Configuración del testigo en la nube con la GUI del administrador de clústeres de conmutación por error"; haremos ese paso a continuación).
11. Configura el servidor de archivos de Espacios de almacenamiento directo. Conéctate a una VM de nodo y ejecuta los siguientes cmdlets de Windows PowerShell.
    1. Instala la característica para la agrupación de clústeres de conmutación por error y la característica del servidor de archivos en las dos VM del nodo de clústeres del servidor de archivos:

       ```powershell
       $nodes = ("my-fsn1", "my-fsn2")
       icm $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools}
       icm $nodes {Install-WindowsFeature FS-FileServer}
       ```
    2. Valida las VM del nodo de clústeres y crea un clúster de SOFS de dos nodos:

       ```powershell
       Test-Cluster -node $nodes
       New-Cluster -Name MY-CL1 -Node $nodes –NoStorage –StaticAddress [new address within your addr space]
       ```
    3. Configura el testigo en la nube. Usa tu nombre de cuenta de almacenamiento en la nube y la clave de acceso.

       ```powershell
       Set-ClusterQuorum –CloudWitness –AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
       ```
    4. Habilita Espacios de almacenamiento directo.

       ```powershell
       Enable-ClusterS2D
       ```

    5. Crea un volumen de disco virtual.

       ```powershell
       New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 120GB
       ```
       Para ver información sobre el volumen compartido de clúster en el clúster SOFS, ejecuta el siguiente cmdlet:

       ```powershell
       Get-ClusterSharedVolume
       ```

    6. Crea un servidor de archivos de escalabilidad horizontal (SOFS):

       ```powershell
       Add-ClusterScaleOutFileServerRole -Name my-sofs1 -Cluster MY-CL1
       ```

    7. Crea un nuevo recurso compartido de archivos SMB en el clúster SOFS.

       ```powershell
       New-Item -Path C:\ClusterStorage\VDisk01\Data -ItemType Directory
       New-SmbShare -Name UpdStorage -Path C:\ClusterStorage\VDisk01\Data
       ```

Ya tienes un recurso compartido en `\\my-sofs1\UpdStorage`, que se puede usar para el almacenamiento de UPD cuando [habilites UPD](https://techcommunity.microsoft.com/t5/ask-the-performance-team/migrating-user-profile-disks-in-remote-desktop-services/ba-p/375630) para los usuarios.

