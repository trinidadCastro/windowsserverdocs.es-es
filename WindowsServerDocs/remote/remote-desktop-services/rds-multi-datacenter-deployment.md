---
title: Centros de datos con redundancia geográfica RDS en Azure
description: Obtenga información sobre cómo crear una implementación de RDS que usa varios centros de datos para proporcionar alta disponibilidad en diferentes ubicaciones geográficas.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61c36528-cf47-4af0-83c1-a883f79a73a5
author: haley-rowland
ms.author: elizapo
ms.date: 06/14/2017
manager: dongill
ms.openlocfilehash: 2d12062f302c28a8124e0aa49af7f441e77ffe33
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222794"
---
# <a name="create-a-geo-redundant-multi-data-center-rds-deployment-for-disaster-recovery"></a>Crear un centro de implementación de RDS para la recuperación ante desastres con redundancia geográfica, múltiples datos

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016

Puede habilitar la recuperación ante desastres para la implementación de servicios de escritorio remoto mediante el aprovechamiento de varios centros de datos en Azure. A diferencia de una implementación de RDS de alta disponibilidad estándar (como se describe en el [arquitectura de servicios de escritorio remoto](desktop-hosting-logical-architecture.md)), que usa centros de datos en una sola región de Azure (por ejemplo, Europa occidental), una implementación de varios centros de datos usa datos los centros de varias ubicaciones geográficas, aumentar la disponibilidad de la implementación - centro de datos podrían no estar disponibles, pero es improbable que varias regiones se dejan de funcionar al mismo tiempo. Al implementar una arquitectura RDS con redundancia geográfica, puede habilitar la conmutación por error en el caso de error grave de toda una región.

Puede usar las instrucciones siguientes para aprovechar los servicios de infraestructura de Microsoft Azure y RDS para entregar licencias de acceso de suscriptor (sal) y de servicios de hospedaje de escritorio con redundancia geográfica para varios inquilinos a través de la [proveedor de servicios de Microsoft Programa de licencias de contrato (SPLA)](https://www.microsoft.com/hosting/licensing/splabenefits.aspx). También puede usar los pasos siguientes para crear un servicio de hospedaje con redundancia geográfica para sus propios empleados mediante [CAL de usuario de RDS derechos a través de Software Assurance extendidos](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf).

## <a name="logical-architecture-for-high-availability---single-and-multiple-regions"></a>Arquitectura lógica para lograr alta disponibilidad - único y varias regiones
La siguiente imagen muestra la arquitectura de una implementación de alta disponibilidad en una sola región de Azure:

![Una implementación de alta disponibilidad en una sola región de Azure](media/rds-ha-single-region.png)

La implementación consta de tres capas:

- Servicios de Azure: las interfaces de administración de Azure, incluidos el portal de Azure y las API y servicios de red públicos, como DNS y una dirección IP pública.
- Servicio: máquinas virtuales, redes, almacenamiento, servicios de Azure y servicios de rol de Windows Server de hospedaje de escritorio
- Azure Fabric: sistemas operativos de Windows Server que ejecuta el rol Hyper-V, se usa para la virtualización de servidores físicos, unidades de almacenamiento, los enrutadores y conmutadores de red. Mediante Azure Fabric le permite crear máquinas virtuales, redes, almacenamiento y las aplicaciones independientes de hardware subyacente.


En comparación, ésta es la arquitectura para una implementación que usa varios centros de datos de Azure:

![Una implementación de RDS que usa varias regiones de Azure](media/rds-ha-multi-region.png)

Toda la implementación de RDS se replica en una segunda región de Azure para crear una implementación con redundancia geográfica. Esta arquitectura emplea un modelo activo / pasivo, donde se ejecuta solo una implementación de RDS a la vez. Una conexión de red virtual a red virtual permite que los dos entornos comunicarse entre sí. Las implementaciones de RDS se basan en un único bosque de Active Directory/dominio y replican los servidores de AD a través de las dos implementaciones, los usuarios de significado pueden iniciar sesión en ninguna de las implementaciones con las mismas credenciales. Configuración de usuario y los datos almacenados en discos de perfil de usuario (UPD) se almacenan en un servidor de archivos de clúster de dos nodos espacios de almacenamiento directo de escalabilidad horizontal (SOFS). Un segundo clúster de espacios de almacenamiento directo idéntico se implementa en la segunda región (pasiva) y réplica de almacenamiento se usa para replicar los perfiles de usuario de activo a la implementación pasivo. Azure Traffic Manager se usa para dirigir automáticamente a cualquier implementación de los usuarios finales está activo actualmente - desde la perspectiva del usuario final, tener acceso a la implementación mediante una dirección URL única y no son conscientes de qué región termina usando.


Le *podría* crear una implementación de RDS sin alta disponibilidad en cada región, pero si se reinicia incluso una sola máquina virtual en una región, podría producirse una conmutación por error, lo que aumenta la probabilidad de conmutaciones por error que se producen con asociados efectos sobre el rendimiento.

## <a name="deployment-steps"></a>Pasos de implementación
Cree los siguientes recursos en Azure para crear una implementación de RDS con redundancia geográfica varios centros de datos:

1. Separan los dos grupos de recursos en dos regiones de Azure. Por ejemplo A RG (la implementación activa, RG significa "grupo de recursos") y RG B (la implementación pasiva).
2. Una implementación de Active Directory de alta disponibilidad en RG A. Puede usar el [nuevo dominio de AD con plantilla 2 de los controladores de dominio](https://azure.microsoft.com/resources/templates/active-directory-new-domain-ha-2-dc/) para crear la implementación.
3. Una implementación de RDS de alta disponibilidad en RG A. Use el [implementación mediante active directory de la granja de servidores RDS](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/) plantilla para crear la implementación de RDS básica y, a continuación, siga la información de [servicios de escritorio remoto - alta disponibilidad](rds-plan-high-availability.md) para configurar los demás componentes RDS de alta disponibilidad.
4. Una red virtual en B RG - Asegúrese de usar un espacio de direcciones que no se superpone a la implementación en RG A.
5. Un [conexión VNet a](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps) entre los grupos de recursos de dos.
6. Dos máquinas virtuales de AD en un conjunto de disponibilidad en B RG - Asegúrese de que los nombres de máquina virtual son diferentes de las máquinas virtuales de AD en RG A. Deploy dos VM de Windows Server 2016 en la disponibilidad de un única establecer, instalar el rol Servicios de dominio de Active Directory y, a continuación, promoverlos a la entrega continua de dominio rodillo de pintura en el dominio que creó en el paso 1.
7. Una segunda implementación de RDS de alta disponibilidad en RG B. 
   1. Use la [implementación mediante active directory de la granja de servidores RDS](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/) plantilla nuevo, pero esta vez haga los cambios siguientes. (Para personalizar la plantilla, selecciónela en la galería, haga clic en **implementar en Azure** y, a continuación, **Editar plantilla**.)
      1. Ajustar el espacio de direcciones de la IP privada del servidor DNS que corresponda a la red virtual en RG B. 
      
         Busque "dnsServerPrivateIp" en las variables. Editar la dirección IP de forma predeterminada (10.0.0.4) para que correspondan al espacio de direcciones definido en la red virtual en RG B.
   
      2. Editar los nombres de equipo para que no entren en conflicto con los de la implementación en RG A.
      
         Busque las máquinas virtuales en el **recursos** sección de la plantilla. Cambiar el **computerName** campo **osProfile**. Por ejemplo, puede convertirse en "puerta de enlace" "puerta de enlace **-b**"; "[concat ('rdsh-', copyIndex())]" puede convertirse en "[concat ('rdsh - b-', copyIndex())]" y "agente" puede convertirse en "broker **-b**".
      
         (También puede cambiar los nombres de las máquinas virtuales manualmente después de ejecutar la plantilla.)
   2. Como en el paso 3 anterior, use la información de [servicios de escritorio remoto: alta disponibilidad](rds-plan-high-availability.md) para configurar los demás componentes RDS de alta disponibilidad.
8. Un servidor de archivos de escalabilidad horizontal de espacios de almacenamiento directo con réplica de almacenamiento en las dos implementaciones. Use la [script de PowerShell](https://github.com/robotechredmond/301-s2d-sr-dr-md/tree/master/scripts) para implementar la [plantilla](https://github.com/robotechredmond/301-s2d-sr-dr-md) a través de los grupos de recursos.

   > [!NOTE]
   > Puede aprovisionar almacenamiento manualmente (en lugar de usar el script de PowerShell y la plantilla): 
   >1. Implementar un [SOFS de espacios de almacenamiento de dos nodos directo](rds-storage-spaces-direct-deployment.md) en el grupo de recursos A para almacenar los discos de perfil de usuario (UPD).
   >2. Implementación de una en segundo lugar, idénticos almacenamiento SOFS de espacios directo en RG B: asegúrese de usar la misma cantidad de almacenamiento en cada clúster.
   >3. Configurar [réplica de almacenamiento con replicación asincrónica](../../storage/storage-replica/cluster-to-cluster-storage-replication.md) entre los dos.

### <a name="enable-upds"></a>Habilitar los UPD
Réplica de almacenamiento replica datos desde un volumen de origen (asociado con la implementación de principal/activo) en un volumen de destino (asociado a la implementación de la base de datos secundaria/pasiva). Por diseño, el clúster de destino aparece como **en línea (sin acceso)** : réplica de almacenamiento desmonta los volúmenes de destino y sus puntos de montaje o letras de unidad. Esto significa que no habilitar los UPD para la implementación secundaria proporcionando la ruta de acceso del recurso compartido de archivo, porque no está montado el volumen. 

¿Desea más información acerca de cómo administrar la replicación? Desproteger [clúster al clúster de replicación de almacenamiento](../../storage/storage-replica/cluster-to-cluster-storage-replication.md).

Para habilitar los UPD en ambas implementaciones, realice lo siguiente:

1. Ejecute el [cmdlet Set-RDSessionCollectionConfiguration](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdsessioncollectionconfiguration) para habilitar los discos de perfil de usuario para la implementación principal (activo): proporcione una ruta de acceso al recurso compartido de archivos en el volumen de origen (que creó en el paso 7 en los pasos de implementación).
2. Invertir la dirección de la réplica de almacenamiento para que el volumen de destino se convierte en el volumen de origen (el volumen se monta y hace que sea accesible por la implementación secundaria). Puede ejecutar **Set-SRPartnership** cmdlet para hacer esto. Por ejemplo:

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```
3. Habilitar los discos de perfil de usuario en la implementación secundaria (pasivo). Use los mismos pasos que realizó para la implementación principal, en el paso 1.
4. Invertir la dirección de la réplica de almacenamiento nuevo, por lo que el volumen de origen original vuelve a estar el volumen de origen de la asociación de SR y la implementación principal puede tener acceso a recurso compartido de archivos. Por ejemplo:

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```


### <a name="azure-traffic-manager"></a>Administrador de tráfico de Azure 

Crear un [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview) de perfil y asegúrese de seleccionar el **prioridad** método de enrutamiento. Establecer los dos extremos a las direcciones IP públicas de cada implementación. En **configuración**, cambiar el protocolo HTTPS (en lugar de HTTP) y el puerto 443 (en lugar de 80). Tome nota de la **tiempo de vida de DNS**y configurarlo de forma adecuada para las necesidades de la conmutación por error. 

Tenga en cuenta que Traffic Manager requiere los puntos de conexión para devolver 200 OK en respuesta a una solicitud GET con el fin de marcarse como "correcto". El objeto publicIP creado a partir de las plantillas RDS funcionará, pero no agregue un anexo de ruta de acceso. En su lugar, puedes usar los usuarios finales la dirección URL del Administrador de tráfico con "/ RDWeb" anexado, por ejemplo: ```http://deployment.trafficmanager.net/RDWeb```

Mediante la implementación de Azure Traffic Manager con el método de enrutamiento de prioridad, evitar que los usuarios finales tengan acceso a la implementación pasiva, mientras que la implementación activa es funcional. Si los usuarios finales acceso a la implementación pasiva y no se ha cambiado la dirección de la réplica de almacenamiento para la conmutación por error, el inicio de sesión del usuario se bloquea como la implementación se prueba y se produce un error al acceder al recurso compartido de archivos en el clúster de espacios de almacenamiento directo pasivo, al final de la implementación se renunciar y proporcionar al usuario un perfil temporal.  

### <a name="deallocate-vms-to-save-resources"></a>Desasignar máquinas virtuales para ahorrar recursos 
Después de configurar ambas implementaciones, puede apagar y desasignar las máquinas virtuales de RDSH para ahorrar costos en estas máquinas virtuales y la infraestructura de RDS secundaria. El servidor SOFS de espacios de almacenamiento directo y AD máquinas virtuales debe permanecer siempre que se ejecutan en la implementación de la base de datos secundaria/pasivo para habilitar la sincronización de perfil y cuenta de usuario.  

Cuando se produce una conmutación por error, deberá iniciar las máquinas virtuales desasignadas. Esta configuración de implementación tiene la ventaja de que se va a menor costo, pero a costa del tiempo de conmutación por error. Si se produce un error grave en la implementación activa, tendrá que iniciar manualmente la implementación pasiva o tiene un script de automatización para detectar el error e iniciar la implementación pasiva automáticamente. En cualquier caso, puede tardar varios minutos para obtener la implementación pasiva funcionamiento y disponibles para los usuarios iniciar sesión, lo que produce un tiempo de inactividad para el servicio. Este tiempo de inactividad depende de la cantidad de tiempo se tarda en iniciar la infraestructura RDS y máquinas virtuales de RDSH (normalmente 2 a 4 minutos, si las máquinas virtuales se inician en paralelo en lugar de en serie) y el tiempo para poner en línea el clúster pasivo (que depende del tamaño del clúster normalmente de 2 a 4 minutos para un clúster de 2 nodos con 2 discos por nodo). 

### <a name="active-directory"></a>Active Directory 
Los servidores de Active Directory en cada implementación son réplicas dentro del mismo bosque o dominio. Active Directory tiene un protocolo de sincronización integrada para mantener sincronizados los controladores de dominio. Sin embargo, puede haber algún retraso para que si se agrega un nuevo usuario a un servidor de AD, puede tardar algún tiempo a replicar en todos los servidores de AD en las dos implementaciones. Por lo tanto, asegúrese de advertir a los usuarios no intenten iniciar sesión en inmediatamente después de que se agrega al dominio. 

### <a name="rd-license-server"></a>Servidor de licencias de escritorio remoto 
Proporcione un [de CAL por usuario de escritorio remoto](rds-client-access-license.md) para cada usuario con nombre que está autorizado para tener acceso a la implementación con redundancia geográfica. Distribuir la por usuario CAL uniformemente entre los dos servidores de licencias de escritorio remoto en la implementación activa. A continuación, duplicar estas CAL para los dos servidores de licencias de escritorio remoto en la implementación pasiva. Dado que las CAL se duplican entre la implementación de activa y pasiva en un momento dado solo una implementación puede estar activa con los usuarios se conectan; en caso contrario, infringe el contrato de licencia.  

### <a name="image-management"></a>Administración de imágenes 
Igual que actualizar las imágenes RDSH para proporcionar nuevas aplicaciones o actualizaciones de software, deberá actualizar por separado las colecciones de RDSH en cada implementación para mantener una experiencia de usuario común entre ambas implementaciones. Puede usar el [plantilla de la colección de actualización RDSH](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/), pero tenga en cuenta que la implementación pasiva infraestructura de RDS y máquinas virtuales RDSH deben estar ejecutándose para ejecutar la plantilla. 

## <a name="failover"></a>Conmutación por error

En el caso de la implementación activo / pasivo, conmutación por error requiere que inicie las máquinas virtuales de la implementación secundaria. Puede hacerlo manualmente o con un script de automatización. En el caso de una conmutación por error grave de los SOFS de espacios de almacenamiento directo, cambie la dirección de asociación de réplica de almacenamiento, para que el volumen de destino se convierte en el volumen de origen. Por ejemplo:

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```

Para más información en [clúster al clúster de replicación de almacenamiento](../../storage/storage-replica/cluster-to-cluster-storage-replication.md).

Azure Traffic Manager reconoce automáticamente que no se pudo la implementación principal y que la implementación secundaria es correcta (en las máquinas virtuales de puerta de enlace de escritorio remoto se iniciaron en RG B) y dirige el tráfico de usuario a la implementación secundaria. Los usuarios pueden usar la misma dirección URL de Traffic Manager para seguir trabajando con sus recursos remotos, disfrutar de una experiencia coherente. Tenga en cuenta que la caché DNS del cliente no se actualizará el registro durante el TTL establecido en la configuración de Azure Traffic Manager.

### <a name="test-failover"></a>Conmutaciones por error de prueba
En una asociación de réplica de almacenamiento, puede ser active sólo un volumen (origen) a la vez. Esto significa que al cambiar la dirección de la asociación de SR, el volumen en la implementación principal (grupo de recursos A) se convierte en el destino de replicación y, por tanto, está oculta. Por lo tanto, los usuarios que se conectan a RG A dejará de tener acceso a sus UPD almacenados en el SOFS en RG A. 

Para probar la conmutación por error al tiempo que permite a los usuarios seguir iniciando sesión:
1. Iniciar las máquinas virtuales de infraestructura y las máquinas virtuales RDSH en RG B.
2. Cambiar la dirección de la asociación de SR (clúster-b-s2d-c se convierte en el volumen de origen).
3. [Deshabilitar el punto de conexión](/azure/traffic-manager/traffic-manager-manage-endpoints#to-disable-an-endpoint) de RG A en el perfil de Traffic Manager de Azure para forzar el ATM para dirigir el tráfico a RG B. como alternativa, use un script de PowerShell:

   ```powershell
   Disable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA -Force
   ```

RG B ahora es la implementación principal activa. Para volver a RG A que la implementación principal:

1. Cambiar la dirección de la asociación de SR (clúster-a-s2d-c se convierte en el volumen de origen):

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```
2. Vuelva a habilitar el punto de conexión de RG A en el perfil de Traffic Manager de Azure:

   ```powershell
   Enable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA 
   ```

## <a name="considerations-for-on-premises-deployments"></a>Consideraciones para las implementaciones locales

Aunque una implementación local no pudo usar las plantillas de inicio rápido de Azure al que hace referencia en este artículo, puede implementar manualmente todos los roles de infraestructura. En una implementación local donde costo no depende de consumo de Azure, considere el uso de un modelo activo-activo para rápida conmutación por error.

Puede usar Azure Traffic Manager con puntos de conexión de forma local, pero requiere una suscripción de Azure. Como alternativa, para el DNS proporcionado a los usuarios finales, asignarles un registro CNAME que simplemente dirige a los usuarios a la implementación principal. En el caso de conmutación por error, modifique el registro CNAME de DNS para redirigir a la implementación secundaria. De este modo, el usuario final usa una sola dirección URL, al igual que con Azure Traffic Manager, que dirige al usuario a la implementación apropiada. 

Si está interesado en crear un modelo en-local-a-sitio de Azure, considere el uso de [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview).
