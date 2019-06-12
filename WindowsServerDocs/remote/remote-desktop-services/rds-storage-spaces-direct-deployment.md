---
title: Implementar un nodo de dos espacios SOFS de almacenamiento directo para el almacenamiento UPD en Azure
description: Aprenda a usar espacios de almacenamiento directo con RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1099f21d-5f07-475a-92dd-ad08bc155da1
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
manager: scottman
ms.openlocfilehash: 792c9320f6976a4fc7f2ccd235f66daa0cb19b19
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805192"
---
# <a name="deploy-a-two-node-storage-spaces-direct-scale-out-file-server-for-upd-storage-in-azure"></a>Implementar un servidor de archivos de escalabilidad horizontal de espacios de almacenamiento directo de dos nodos para el almacenamiento UPD en Azure

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016

Servicios de escritorio remoto (RDS) requiere un servidor de archivos unido al dominio para discos de perfil de usuario (UPD). Para implementar un servidor de archivos de escalabilidad horizontal unido al dominio de alta disponibilidad (SOFS) en Azure, use espacios de almacenamiento directo con Windows Server 2016. Si no está familiarizado con los UPD o servicios de escritorio remoto, consulte [Bienvenido a servicios de escritorio remoto](welcome-to-rds.md).

> [!NOTE] 
> Microsoft acaba de publicar un [plantilla de Azure para implementar un servidor de archivos de escalabilidad horizontal de espacios de almacenamiento directo](https://azure.microsoft.com/documentation/templates/301-storage-spaces-direct/)! Puede usar la plantilla para crear la implementación, o use los pasos de este artículo. 

Se recomienda implementar el SOFS con máquinas virtuales de la serie DS y discos de datos de premium storage, donde hay el mismo número y tamaño de los discos de datos en cada máquina virtual. Necesitará un mínimo de dos cuentas de almacenamiento. 

Para las pequeñas implementaciones, se recomienda un clúster de 2 nodos con un testigo en la nube, donde se refleja el volumen con 2 copias. Aumentar las pequeñas implementaciones mediante la adición de discos de datos. Aumentar las implementaciones más grandes mediante la adición de nodos (máquinas virtuales). 

Estas instrucciones son para una implementación de 2 nodos. La siguiente tabla muestra los tamaños de máquina virtual y disco que necesitará para almacenar los UPD para el número de usuarios en su negocio. 

| Usuarios | Total (GB) | Máquina virtual | # Discos | Tipo de disco | Tamaño del disco (GB) | Configuración   |
|-------|------------|----|---------|-----------|----------------|-----------------|
| 10    | 50         | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 25    | 125        | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 50    | 250        | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 100   | 500        | DS1 | 2       | P20       | 512            | 2 x (DS1 + 2 P20)  |
| 250   | 1250       | DS1 | 2       | P30       | 1024           | 2x(DS1 + 2 P30)  |
| 500   | 2500       | DS2 | 3       | P30       | 1024           | 2 x (DS2 + 3 P30)  |
| 1000  | 5000       | DS3 | 5       | P30       | 1024           | 2 x (DS3 + 5 P30)  |
| 2500  | 12500      | DS4 | 13      | P30       | 1024           | 2 x (DS4 + 13 P30) |
| 5000  | 25000      | DS5 | 25      | P30       | 1024           | 2 x (DS5 + 25 P30) | 

Siga estos pasos para crear un controlador de dominio (pusimos el nombre "Mis controladores de dominio" a continuación) y dos nodos de máquinas virtuales ("Mi-fsn1" y "my-fsn2") y configurar las máquinas virtuales para que sea un SOFS de espacios de almacenamiento de 2 nodos directo.

1. Crear un [suscripción de Microsoft Azure](https://azure.microsoft.com).
2. Inicie sesión en el [portal Azure](https://ms.portal.azure.com).
3. Crear un [cuenta de Azure storage](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account) en Azure Resource Manager. Cree en un grupo de recursos y use las siguientes configuraciones:
   - Modelo de implementación: Administrador de recursos
   - Tipo de cuenta de almacenamiento: Uso general
   - Nivel de rendimiento: Premium
   - Opción de replicación: LRS
4. Configurar un bosque de Active Directory con una plantilla de inicio rápido o implementar manualmente el bosque. 
   - Implementar mediante una plantilla de inicio rápido de Azure:
      - [Crear una máquina virtual de Azure con un nuevo bosque de AD](https://azure.microsoft.com/documentation/templates/active-directory-new-domain/)
      - [Crear un nuevo dominio de AD con 2 controladores de dominio](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/) (para lograr alta disponibilidad)
   - Manualmente [implementar el bosque](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/) con las siguientes configuraciones:
      - Crear la red virtual en el mismo grupo de recursos que la cuenta de almacenamiento.
      - Tamaño recomendado: DS2 (aumentar el tamaño si va a hospedar el controlador de dominio más objetos de dominio)
      - Usar una red virtual generada automáticamente.
      - Siga los pasos para instalar AD DS.
5. Configure los nodos del clúster de servidor de archivos. Puede hacerlo mediante la implementación de la [plantilla de Azure de clúster de Windows Server 2016 Storage SOFS de espacios directo](https://azure.microsoft.com/resources/templates/301-storage-spaces-direct/) o siguiendo los pasos del 6 al 11 para implementar manualmente.
6. Para configurar manualmente los nodos de clúster de servidor de archivos:
   1. Crear el primer nodo: 
      1. Crear una nueva máquina virtual con la imagen de Windows Server 2016. (Haga clic en **nuevo > máquinas virtuales > Windows Server 2016.** Seleccione **Resource Manager**y, a continuación, haga clic en **crear**.)
      2. Establezca la configuración básica de la manera siguiente:
         - Name: my-fsn1
         - Tipo de disco de máquina virtual SSD
         - Usar un grupo de recursos existente, lo que ha creado en el paso 3. 
      3. Tamaño: DS2, DS1, DS3, DS4 o DS5 según el usuario necesita (consulte la tabla al principio de estas instrucciones). Asegúrese de que esté seleccionada la compatibilidad de disco premium.
      4. Configuración: 
         - Cuenta de almacenamiento: Elija la cuenta de almacenamiento que creó en el paso 3.
         - Alta disponibilidad: crear un nuevo conjunto de disponibilidad. (Haga clic en **alta disponibilidad > Crear nueva**y, a continuación, escriba un nombre (por ejemplo, clúster s2d). Use los valores predeterminados para **dominios de actualización** y **dominios de error**.)
   2. Crear el segundo nodo. Repita el paso anterior con los cambios siguientes:
      - Nombre: my-fsn2
      - Alta disponibilidad: seleccione que el conjunto de disponibilidad creó anteriormente.  
7. [Conectar discos de datos](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-attach-disk-portal/) para el nodo de clúster de máquinas virtuales según el usuario necesita (como se muestra en la tabla anterior). Después de los datos se crean y se adjunta a la máquina virtual, Establece discos **hospedar caching** a **ninguno**.
8. Establecimiento de direcciones IP para todas las máquinas virtuales a **estático**. 
   1. En el grupo de recursos, seleccione una máquina virtual y, a continuación, haga clic en **interfaces de red** (bajo **configuración**). Seleccione la interfaz de red enumerados y, a continuación, haga clic en **configuraciones de IP**. Seleccione la configuración de IP enumerada, seleccione **estático**y, a continuación, haga clic en **guardar**.
   2. Tenga en cuenta la dirección IP privada (10.x.x.x) de dominio controlador (Mis controladores de dominio en nuestro ejemplo).
9. Establecer dirección del servidor DNS principal en la NIC de máquinas virtuales de nodos de clúster en el servidor de mi controlador de dominio. Seleccione la máquina virtual y, a continuación, haga clic en **Interfaces de red > servidores DNS > DNS personalizado**. Escriba la dirección IP privada que anotó anteriormente y, a continuación, haga clic en **guardar**.
10. Crear un [cuenta de almacenamiento para que el testigo en la nube](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness). (Si usa las instrucciones vinculadas, detendrá cuando llegue a "Configuración de nube testigo con conmutación por error de clúster GUI del Administrador de" - haremos todo lo que paso siguiente).
11. Configurar el servidor de archivos de espacios de almacenamiento directo. Conectarse a una máquina virtual del nodo y, a continuación, ejecute los siguientes cmdlets de Windows PowerShell.
    1. Instale la característica de agrupación en clústeres de conmutación por error y la característica de servidor de archivos en el nodo de clúster de servidor de archivos de dos máquinas virtuales:

       ```powershell
       $nodes = ("my-fsn1", "my-fsn2")
       icm $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools} 
       icm $nodes {Install-WindowsFeature FS-FileServer} 
       ```
    2. Validar el nodo de clúster de máquinas virtuales y crear un clúster SOFS 2 nodos:

       ```powershell
       Test-Cluster -node $nodes
       New-Cluster -Name MY-CL1 -Node $nodes –NoStorage –StaticAddress [new address within your addr space]
       ``` 
    3. Configurar al testigo en la nube. Usar la nube testigo cuenta acceso y el nombre clave de almacenamiento.

       ```powershell
       Set-ClusterQuorum –CloudWitness –AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> 
       ```
    4. Habilitar espacios de almacenamiento directo.

       ```powershell
       Enable-ClusterS2D 
       ```
      
    5. Crear un volumen de disco virtual.

       ```powershell
       New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 120GB 
       ```
       Para ver información sobre el volumen compartido de clúster en el clúster SOFS, ejecute el siguiente cmdlet:

       ```powershell
       Get-ClusterSharedVolume
       ```
   
    6. Crear el servidor de archivos de escalabilidad horizontal (SOFS):

       ```powershell
       Add-ClusterScaleOutFileServerRole -Name my-sofs1 -Cluster MY-CL1
       ```

    7. Cree un nuevo recurso compartido de archivos SMB en el clúster SOFS.

       ```powershell
       New-Item -Path C:\ClusterStorage\Volume1\Data -ItemType Directory
       New-SmbShare -Name UpdStorage -Path C:\ClusterStorage\Volume1\Data
       ```

Ahora tiene un recurso compartido en `\\my-sofs1\UpdStorage`, que se puede usar para el almacenamiento UPD cuando le [habilitar UPD](https://social.technet.microsoft.com/wiki/contents/articles/15304.installing-and-configuring-user-profile-disks-upd-in-windows-server-2012.aspx) para los usuarios. 
