---
ms.assetid: 0cd1ac70-532c-416d-9de6-6f920a300a45
title: Implementación de un testigo en la nube para un clúster de conmutación por error
manager: lizross
ms.author: jgerend
ms.topic: article
author: JasonGerend
ms.date: 01/18/2019
description: 'Cómo usar Microsoft Azure para hospedar el testigo para un clúster de conmutación por error de Windows Server en la nube: también se ha aprendido a implementar un testigo en la nube.'
ms.openlocfilehash: fa0fee044b0a5e702cb56816bf9a878f209d6117
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87993014"
---
# <a name="deploy-a-cloud-witness-for-a-failover-cluster"></a>Implementación de un testigo en la nube para un clúster de conmutación por error

> Se aplica a: Windows Server 2019, Windows Server 2016

El testigo en la nube es un tipo de testigo de cuórum de clúster de conmutación por error que usa Microsoft Azure para proporcionar un voto en el cuórum de clúster. En este tema se proporciona información general sobre la característica de testigo en la nube, los escenarios que admite e instrucciones sobre cómo configurar un testigo en la nube para un clúster de conmutación por error.

## <a name="cloud-witness-overview"></a><a name="CloudWitnessOverview"></a>Información general del testigo en la nube

En la figura 1 se muestra una configuración de cuórum de clúster de conmutación por error extendida de varios sitios con Windows Server 2016. En esta configuración de ejemplo (Ilustración 1), hay 2 nodos en dos centros de recursos (denominados sitios). Tenga en cuenta que un clúster puede abarcar más de 2 centros de recursos. Además, cada centro de información puede tener más de 2 nodos. Una configuración típica de cuórum de clúster en esta instalación (acuerdo de nivel de servicio de conmutación por error automática) asigna a cada nodo un voto. Se da un voto adicional al testigo de cuórum para permitir que el clúster siga ejecutándose incluso si uno de los centros de Datacenter experimenta una interrupción del suministro eléctrico. La expresión matemática es sencilla: hay 5 votos totales y se necesitan 3 votos para el clúster para mantenerla en ejecución.

![Testigo de recurso compartido de archivos en un tercer sitio independiente con 2 nodos en otros dos sitios](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_1.png "Recurso compartido de archivos") 
 **Figura 1: uso de un testigo de recurso compartido de archivos como testigo de cuórum**

En caso de que se produzca una interrupción de la alimentación en un centro de centros de recursos, para dar la misma oportunidad para que el clúster de otro centro de recursos lo siga ejecutando, se recomienda hospedar el testigo de cuórum en una ubicación distinta a la de los dos centros de recursos. Normalmente, esto significa requerir un tercer centro de recursos independiente (sitio) para hospedar un servidor de archivos que realiza una copia de seguridad del recurso compartido de archivos que se usa como testigo de cuórum (testigo de recurso compartido de archivos).

La mayoría de las organizaciones no tienen un tercer centro de recursos independiente que hospede el servidor de archivos de respaldo del testigo de recurso compartido de archivos. Esto significa que las organizaciones hospedan principalmente el servidor de archivos en uno de los dos centros de recursos, que por extensión, hace que el centro de centros de recursos sea el centro de recursos principal. En un escenario en el que hay una interrupción del suministro eléctrico en el centro de centros de recursos principal, el clúster se desactivaría, ya que el otro centro de centros de recursos solo tendría 2 votos, que está por debajo de la mayoría del cuórum de 3 votos necesarios. En el caso de los clientes que tienen un tercer centro de centros de trabajo independiente para hospedar el servidor de archivos, es una sobrecarga para mantener el servidor de archivos de alta disponibilidad respaldando el testigo del recurso compartido de archivos. Hospedar máquinas virtuales en la nube pública que tienen el servidor de archivos para el testigo del recurso compartido de archivos que se ejecuta en el SO invitado es una sobrecarga significativa en cuanto a la configuración & mantenimiento.

Testigo en la nube es un nuevo tipo de testigo de cuórum del clúster de conmutación por error que aprovecha Microsoft Azure como punto de arbitraje (Figura 2). Utiliza Azure Blob Storage para leer y escribir un archivo de BLOB que se usa como punto de arbitraje en el caso de la resolución de cerebro dividido.

Este enfoque tiene importantes ventajas:
1. Aprovecha Microsoft Azure (no es necesario para tercer centro de recursos independiente).
2. Usa Azure Blob Storage disponibles estándar (sin sobrecarga de mantenimiento adicional de las máquinas virtuales hospedadas en la nube pública).
3. Se puede usar la misma cuenta de Azure Storage para varios clústeres (un archivo de BLOB por clúster; identificador único de clúster usado como nombre de archivo de BLOB).
4. $Cost muy baja en la cuenta de almacenamiento (datos muy pequeños escritos por cada archivo de BLOB, el archivo de BLOB se actualiza solo una vez cuando cambia el estado de los nodos del clúster).
5. Tipo de recurso de testigo de nube integrado.

![Diagrama que ilustra un clúster extendido de varios sitios con el testigo en la nube como un testigo de cuórum ](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_2.png)
 **figura 2: clústeres extendidos de varios sitios con testigo en la nube como testigo de cuórum**

Como se muestra en la figura 2, no hay ningún tercer sitio independiente que sea necesario. El testigo en la nube, como cualquier otro testigo de cuórum, obtiene un voto y puede participar en los cálculos de cuórum.

## <a name="cloud-witness-supported-scenarios-for-single-witness-type"></a><a name="CloudWitnessSupportedScenarios"></a>Testigo en la nube: escenarios admitidos para el tipo de testigo único
Si tiene una implementación de clúster de conmutación por error, donde todos los nodos pueden tener acceso a Internet (por extensión de Azure), se recomienda configurar un testigo en la nube como recurso de testigo de cuórum.

Algunos de los escenarios compatibles con el uso del testigo en la nube como testigo de quórum son los siguientes:
- Recuperación ante desastres: clústeres de varios sitios extendidos (consulte la figura 2).
- Clústeres de conmutación por error sin almacenamiento compartido (SQL Always On etc.).
- Clústeres de conmutación por error que se ejecutan en el SO invitado hospedado en Microsoft Azure rol de máquina virtual (o cualquier otra nube pública).
- Clústeres de conmutación por error que se ejecutan dentro del sistema operativo invitado de Virtual Machines hospedadas en nubes privadas.
- Clústeres de almacenamiento con o sin almacenamiento compartido, como clústeres de servidores de archivos de escalabilidad horizontal.
- Clústeres de sucursales pequeñas (incluso clústeres de 2 nodos)

A partir de Windows Server 2012 R2, se recomienda configurar siempre un testigo, ya que el clúster administra automáticamente el voto del testigo y los nodos votan con el Cuórum dinámico.

## <a name="set-up-a-cloud-witness-for-a-cluster"></a><a name="CloudWitnessSetUp"></a>Configuración de un testigo en la nube para un clúster
Para configurar un testigo en la nube como testigo de quórum para el clúster, siga estos pasos:
1. Creación de una cuenta de Azure Storage para usarla como un testigo en la nube
2. Configure el testigo de nube como testigo de quórum para el clúster.

## <a name="create-an-azure-storage-account-to-use-as-a-cloud-witness"></a>Creación de una cuenta de Azure Storage para usarla como un testigo en la nube

En esta sección se describe cómo crear una cuenta de almacenamiento y ver y copiar las direcciones URL del extremo y las claves de acceso para esa cuenta.

Para configurar el testigo de la nube, debe tener una cuenta de Azure Storage válida que se pueda usar para almacenar el archivo de BLOB (se usa para el arbitraje). El testigo en la nube crea un contenedor conocido **msft-Cloud-Witness** en la cuenta de almacenamiento de Microsoft. El testigo en la nube escribe un único archivo de BLOB con el identificador único del clúster correspondiente que se usa como nombre de archivo del archivo de BLOB en este contenedor **msft-Cloud-Witness** . Esto significa que puede usar la misma cuenta de Microsoft Azure Storage para configurar un testigo en la nube para varios clústeres diferentes.

Cuando se usa la misma cuenta de Azure Storage para configurar el testigo de nube para varios clústeres diferentes, se crea automáticamente un solo contenedor **msft-Cloud-Witness** . Este contenedor contendrá un archivo de un solo BLOB por clúster.

### <a name="to-create-an-azure-storage-account"></a>Para crear una cuenta de almacenamiento de Azure

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. En el menú del concentrador, seleccione Nuevo -> Datos y almacenamiento -> Cuenta de almacenamiento.
3. En la página crear una cuenta de almacenamiento, haga lo siguiente:
    1. Escriba un nombre para la cuenta de almacenamiento.
    <br>Los nombres de las cuentas de almacenamiento deben tener entre 3 y 24 caracteres, y solo pueden incluir números y letras en minúscula. El nombre de la cuenta de almacenamiento también debe ser único en Azure.

    2. En **tipo de cuenta**, seleccione **uso general**.
    <br>No se puede usar una cuenta de almacenamiento de blobs para un testigo en la nube.
    3. En **Rendimiento**, seleccione **Estándar**.
    <br>No se puede usar Azure Premium Storage para un testigo en la nube.
    2. En **replicación**, seleccione **almacenamiento con redundancia local (LRS)** .
    <br>Los clústeres de conmutación por error usan el archivo de BLOB como punto de arbitraje, lo que requiere algunas garantías de coherencia al leer los datos. Por lo tanto, debe seleccionar **el almacenamiento con redundancia local para el** tipo de **replicación** .

### <a name="view-and-copy-storage-access-keys-for-your-azure-storage-account"></a>Visualización y copia de las claves de acceso de almacenamiento de la cuenta de Azure Storage

Cuando se crea una cuenta de Microsoft Azure Storage, está asociada a dos claves de acceso que se generan automáticamente: la clave de acceso principal y la clave de acceso secundaria. Para crear por primera vez un testigo en la nube, use la **clave de acceso principal**. No hay ninguna restricción respecto a la clave que se va a usar para el testigo en la nube.

#### <a name="to-view-and-copy-storage-access-keys"></a>Para ver y copiar las claves de acceso de almacenamiento

En Azure portal, vaya a la cuenta de almacenamiento, haga clic en **toda la configuración** y luego haga clic en **claves de acceso** para ver, copiar y regenerar las claves de acceso de la cuenta. La hoja claves de acceso también incluye cadenas de conexión preconfiguradas con las claves principal y secundaria que se pueden copiar para usarlas en las aplicaciones (consulte la figura 4).

![Instantánea del cuadro de diálogo administrar claves de acceso en Microsoft Azure ](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_4.png)
 **figura 4: claves de acceso de almacenamiento**

### <a name="view-and-copy-endpoint-url-links"></a>Ver y copiar vínculos de URL de punto de conexión
Al crear una cuenta de almacenamiento, se generan las siguientes direcciones URL con el formato:`https://<Storage Account Name>.<Storage Type>.<Endpoint>`

El testigo de la nube siempre usa el **BLOB** como el tipo de almacenamiento. Azure usa **. Core.Windows.net** como punto de conexión. Al configurar el testigo de la nube, es posible que se configure con un punto de conexión diferente según el escenario (por ejemplo, el centro de recursos de Microsoft Azure en China tiene un punto de conexión diferente).

> [!NOTE]
> El recurso de testigo en la nube genera automáticamente la dirección URL del punto de conexión y no hay ningún paso adicional de configuración necesario para la dirección URL.

#### <a name="to-view-and-copy-endpoint-url-links"></a>Para ver y copiar los vínculos de la dirección URL del extremo
En Azure portal, vaya a la cuenta de almacenamiento, haga clic en **toda la configuración** y, luego, haga clic en **propiedades** para ver y copiar las direcciones URL del punto de conexión (consulte la figura 5).

![Instantánea de los vínculos de punto de conexión de testigo de nube ](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_5.png)
 **figura 5: vínculos URL de punto de conexión de testigo de nube**

Para obtener más información acerca de la creación y administración de cuentas de Azure Storage, consulte [acerca de las cuentas de Azure Storage](/azure/storage/common/storage-account-create)

## <a name="configure-cloud-witness-as-a-quorum-witness-for-your-cluster"></a>Configuración de un testigo de nube como testigo de quórum para el clúster
La configuración del testigo en la nube está bien integrada en el Asistente para configuración de Cuórum existente integrado en el Administrador de clústeres de conmutación por error.

### <a name="to-configure-cloud-witness-as-a-quorum-witness"></a>Para configurar el testigo de nube como testigo de Cuórum
1. Inicie Administrador de clústeres de conmutación por error.
2. Haga clic con el botón derecho en el clúster: > **más acciones**  ->  **configurar el cuórum de clúster** (consulte la figura 6). Se inicia el Asistente para configurar Cuórum de clúster.
    ![Instantánea de la ruta de acceso del menú a la configuración del Cuórum de clúster de configurar en la Administrador de clústeres de conmutación por error UI ](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_7.png) **figura 6. Configuración de Cuórum de clúster**

3. En la página **seleccionar configuraciones de cuórum** , seleccione **seleccionar el testigo de cuórum** (consulte la figura 7).

    ![Instantánea del botón de radio "seleccionar el testigo de quotrum" en el Asistente para Cuórum de clúster ](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_8.png) **figura 7. Seleccionar la configuración de Cuórum**

4. En la página **seleccionar testigo de quórum** , seleccione **configurar un testigo en la nube** (consulte la figura 8).

    ![Instantánea del botón de radio correspondiente para seleccionar un testigo en la nube ](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_9.png) **figura 8. Seleccionar el testigo de Cuórum**

5. En la página **configurar el testigo** de la nube, escriba la siguiente información:
   1. (Parámetro obligatorio) Azure Storage nombre de la cuenta.
   2. (Parámetro obligatorio) Clave de acceso correspondiente a la cuenta de almacenamiento.
       1. Al crear por primera vez, use la clave de acceso principal (véase la figura 5).
       2. Al girar la clave de acceso principal, use la clave de acceso secundaria (consulte la figura 5).
   3. (Parámetro opcional) Si piensa usar un punto de conexión de servicio de Azure distinto (por ejemplo, el servicio Microsoft Azure en China), actualice el nombre del servidor de extremo.

      ![Instantánea del panel Configuración de testigo en la nube en el Asistente para Cuórum de clúster ](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_10.png)
       **figura 9: configuración de un testigo** en la nube

6. Tras configurar correctamente el testigo en la nube, puede ver el recurso testigo recién creado en el complemento Administrador de clústeres de conmutación por error (consulte la figura 10).

    ![Configuración correcta del testigo en la nube ](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_11.png) **figura 10: configuración correcta del testigo en la nube**

### <a name="configuring-cloud-witness-using-powershell"></a>Configuración de un testigo en la nube con PowerShell
El comando de PowerShell Set-ClusterQuorum existente tiene nuevos parámetros adicionales correspondientes al testigo en la nube.

Puede configurar el testigo en la nube con el [`Set-ClusterQuorum`](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461013(v=technet.10)) siguiente comando de PowerShell:

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
```

En caso de que necesite usar un punto de conexión diferente (poco frecuente):

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> -Endpoint <servername>
```

### <a name="azure-storage-account-considerations-with-cloud-witness"></a>Consideraciones de Azure Storage cuenta con el testigo en la nube
Al configurar un testigo en la nube como testigo de quórum para el clúster de conmutación por error, tenga en cuenta lo siguiente:
* En lugar de almacenar la clave de acceso, el clúster de conmutación por error generará y almacenará de forma segura un token de seguridad de acceso compartido (SAS).
* El token de SAS generado es válido siempre y cuando la clave de acceso siga siendo válida. Al rotar la clave de acceso principal, es importante actualizar primero el testigo en la nube (en todos los clústeres que usen esa cuenta de almacenamiento) con la clave de acceso secundaria antes de volver a generar la clave de acceso principal.
* El testigo en la nube usa la interfaz de REST de HTTPS del servicio de cuenta de Azure Storage. Esto significa que requiere que el Puerto HTTPS esté abierto en todos los nodos del clúster.

### <a name="proxy-considerations-with-cloud-witness"></a>Consideraciones del proxy con el testigo en la nube
El testigo en la nube usa HTTPS (puerto predeterminado 443) para establecer la comunicación con el servicio BLOB de Azure. Asegúrese de que el Puerto HTTPS es accesible a través del proxy de red.

## <a name="see-also"></a>Consulte también
- [Novedades de los clústeres de conmutación por error de Windows Server](whats-new-in-failover-clustering.md)