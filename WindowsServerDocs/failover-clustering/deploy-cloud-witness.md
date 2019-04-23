---
ms.assetid: 0cd1ac70-532c-416d-9de6-6f920a300a45
title: Implementación de un testigo en la nube para un clúster de conmutación por error
ms.prod: windows-server-threshold
manager: eldenc
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.topic: article
author: JasonGerend
ms.date: 01/18/2019
description: Cómo usar Microsoft Azure para hospedar el testigo de un clúster de conmutación por error de Windows Server en la nube, también conocido como cómo implementar un testigo en la nube.
ms.openlocfilehash: f7e1c84e54f08044a772f06e591588c1add33026
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857986"
---
# <a name="deploy-a-cloud-witness-for-a-failover-cluster"></a>Implementación de un testigo en la nube para un clúster de conmutación por error

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

Testigo en la nube es un tipo de testigo de quórum de clúster de conmutación por error que usa Microsoft Azure para proporcionar un voto de quórum de clúster. En este tema se proporciona información general de la característica de testigo en la nube, los escenarios que admite e instrucciones sobre cómo configurar a un testigo en la nube para un clúster de conmutación por error.

## <a name="CloudWitnessOverview"></a>Información general de testigo en la nube

Figura 1 ilustra una configuración de quórum de clúster de conmutación por error extendida de varios sitio con Windows Server 2016. En este ejemplo de configuración (figura 1), hay 2 nodos en 2 centros de datos (denominados sitios). Tenga en cuenta, es posible que un clúster para abarcar más de 2 centros de datos. Además, cada centro de datos puede tener más de 2 nodos. Una configuración de quórum de clúster típico de este programa de instalación (conmutación por error automática SLA) proporciona un voto de cada nodo. Uno voto adicional se proporciona para el testigo de quórum para permitir que el clúster para mantener en funcionamiento incluso si uno del centro de datos experimenta un corte del suministro eléctrico. La matemática es simple: hay un total de 5 votos y necesita 3 votos para el clúster garantizar su funcionamiento.  

![Testigo de recurso compartido de archivo en otro tercer sitio con 2 nodos en 2 otros sitios](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_1.png "testigo del recurso compartido de archivos")  
**Figura 1: Uso de un testigo de recurso compartido de archivos como un testigo de quórum**  

En el caso de interrupción de energía en un centro de datos, para darle la oportunidad de igual para el clúster en otro centro de datos para garantizar su funcionamiento, se recomienda para hospedar al testigo de quórum en una ubicación distinta de los dos centros de datos. Esto normalmente significa que requieren una tercera separa centro de datos (sitio) para hospedar un servidor de archivos que respalda el recurso compartido de archivos que se usa como el testigo de quórum (testigo de recurso compartido de archivos).  

Mayoría de las organizaciones no es necesario un tercer separar el centro de datos que se va a hospedar el testigo del recurso compartido de archivos de seguridad de servidor de archivos. Esto significa que las organizaciones principalmente hospedan el servidor de archivos en uno de los dos centros de datos, que, por extensión, hace que ese centro de datos en el centro de datos principal. En un escenario donde hay suministro eléctrico en el centro de datos principal, el clúster se dejan de funcionar como el centro de datos de otro solo tendría 2 votos que se encuentra debajo de la mayoría del cuórum de 3 votos es necesario. Para los clientes que tienen el tercer centro de datos independiente para hospedar el servidor de archivos, es una sobrecarga para mantener el servidor de archivos de alta disponibilidad del testigo del recurso compartido de archivos de seguridad. Hospedaje de máquinas virtuales en la nube pública que tiene el servidor de archivos para testigo de recurso compartido de archivo que se ejecuta en el SO invitado es una sobrecarga considerable en cuanto a la instalación y mantenimiento.  

Testigo en la nube es un nuevo tipo de testigo de quórum de clúster de conmutación por error que utiliza Microsoft Azure como punto de arbitraje (figura 2). Azure Blob Storage usa para leer o escribir un archivo de blob que se usa como punto de arbitraje en el caso de resolución de cerebro dividido.  

Hay importantes ventajas que este enfoque:
1. Aprovecha Microsoft Azure (sin necesidad de centro de datos independiente en tercer lugar).  
2. Usa estándar disponible Azure Blob Storage (ninguna sobrecarga adicional de mantenimiento de máquinas virtuales hospedadas en la nube pública).  
3. Misma cuenta de almacenamiento de Azure puede utilizarse para varios clústeres (archivo de un blob por clúster; Id. exclusivo de clúster usado como nombre de archivo de blob).  
4. $Cost en curso muy bajos para la cuenta de almacenamiento (muy pequeños datos escritos por el archivo de blob, archivo de blob se actualiza solo una vez cuando se cambia el estado de los nodos del clúster).  
5. Tipo de recurso de testigo en la nube integrado.  

![Diagrama que ilustra un clúster extendido de varios sitios con testigo en la nube como un testigo de quórum](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_2.png)  
**Figura 2: Estirado clústeres de varios sitios con testigo en la nube como un testigo de quórum**  

Tal como se muestra en la figura 2, no hay ningún tercer sitio independiente que se necesita. Testigo en la nube, al igual que cualquier otro testigo de cuórum, obtiene un voto y puede participar en los cálculos de quórum.  

## <a name="CloudWitnessSupportedScenarios"></a>Testigo en la nube: Escenarios admitidos para el tipo de testigo único
Si tiene una implementación de clúster de conmutación por error, donde todos los nodos pueden conectarse a internet (mediante la extensión de Azure), se recomienda que configure un testigo en la nube como el recurso de testigo de quórum.  

Algunos de los escenarios que admiten el uso de testigo en la nube como un testigo de quórum son los siguientes:  
- Recuperación ante desastres estira los clústeres de varios sitios (consulte la figura 2).  
- Clústeres de conmutación por error sin almacenamiento compartido (SQL siempre en etcetera.).  
- Clústeres de conmutación por error que se ejecuta dentro del SO invitado hospedado en el rol de máquina Virtual de Microsoft Azure (o cualquier otra nube pública).  
- Clústeres de conmutación por error que se ejecuta dentro del sistema operativo de las máquinas virtuales invitadas hospedados en nubes privadas.
- Almacenamiento de clústeres con o sin almacenamiento compartido, como clústeres de servidor de archivos de escalabilidad horizontal.  
- Clústeres de sucursal pequeño (incluso 2 nodos de clústeres)  

A partir de Windows Server 2012 R2, se recomienda configurar siempre un testigo como el clúster administra automáticamente el voto de testigo y votan los nodos con quórum dinámico.  

## <a name="CloudWitnessSetUp"></a> Configurar un testigo en la nube para un clúster
Para configurar un testigo en la nube como un testigo de quórum para el clúster, complete los pasos siguientes:
1. Crear una cuenta de almacenamiento de Azure para usarla como un testigo en la nube
2. Configure el testigo en la nube como un testigo de quórum para el clúster.

## <a name="create-an-azure-storage-account-to-use-as-a-cloud-witness"></a>Crear una cuenta de almacenamiento de Azure para usarla como un testigo en la nube

En esta sección se describe cómo crear un punto de conexión de almacenamiento cuenta y la visualización y copia las direcciones URL y claves para esa cuenta de acceso.

Para configurar el testigo en la nube, debe tener una cuenta válida de Azure Storage que se puede usar para almacenar el archivo de blob (se usa para el arbitraje). Testigo en la nube crea un contenedor conocido **testigo msft-en la nube** bajo la cuenta de almacenamiento de Microsoft. En la nube escrituras de testigo del clúster un solo archivo de blob con los correspondientes del Id. exclusivo usado como nombre de archivo del archivo de blob bajo este **testigo msft-en la nube** contenedor. Esto significa que puede usar la misma cuenta de almacenamiento de Microsoft Azure para configurar a un testigo en la nube para varios clústeres diferentes.

Cuando se usa la misma cuenta de almacenamiento de Azure para configurar el testigo en la nube para varios diferentes clústeres de una sola **testigo msft-en la nube** contenedor se crea automáticamente. Este contenedor contendrá el archivo de blob de uno por clúster.

### <a name="to-create-an-azure-storage-account"></a>Para crear una cuenta de almacenamiento de Azure

1. Inicie sesión en el [Portal Azure](http://portal.azure.com).
2. En el menú del concentrador, seleccione Nuevo -> datos y almacenamiento -> cuenta de almacenamiento.
3. En la creación de una página de la cuenta de almacenamiento, realice lo siguiente:
    1. Escriba un nombre para la cuenta de almacenamiento.
    <br>Los nombres de cuenta de almacenamiento deben tener entre 3 y 24 caracteres de longitud y pueden contener números y letras minúsculas solamente. El nombre de cuenta de almacenamiento también debe ser único dentro de Azure.
        
    2. Para **tipo de cuenta**, seleccione **de propósito General**.
    <br>No se puede usar una cuenta de almacenamiento de blobs para un testigo en la nube.
    3. Para **rendimiento**, seleccione **estándar**.
    <br>No se puede usar Azure Premium Storage para un testigo en la nube.
    2. Para **replicación**, seleccione **almacenamiento con redundancia local (LRS)** .
    <br>Agrupación en clústeres de conmutación por error, se usa el archivo de blob como punto de arbitraje, lo que requiere algunas garantías de coherencia al leer los datos. Por lo tanto, debe seleccionar **almacenamiento con redundancia local** para **replicación** tipo.

### <a name="view-and-copy-storage-access-keys-for-your-azure-storage-account"></a>Ver y copiar las claves de acceso de almacenamiento para la cuenta de almacenamiento de Azure

Cuando se crea una cuenta de Microsoft Azure Storage, está asociado con dos claves de acceso que se generan automáticamente - clave de acceso principal y clave de acceso secundaria. Para crear por primera vez un testigo en la nube, use el **clave de acceso principal**. No hay ninguna restricción con respecto a qué clave que se usará para el testigo en la nube.  

#### <a name="to-view-and-copy-storage-access-keys"></a>Para ver y copiar las claves de acceso de almacenamiento

En el Portal de Azure, vaya a la cuenta de almacenamiento, haga clic en **toda la configuración** y, a continuación, haga clic en **claves de acceso** para ver, copiar y regenerar las claves de acceso de cuenta. La hoja claves de acceso también incluye cadenas de conexión configuradas previamente con las claves principales y secundarias que puede copiar para usarlas en sus aplicaciones (consulte la figura 4).

![Instantánea del cuadro de diálogo Administrar claves de acceso en Microsoft Azure](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_4.png)  
**Figura 4: Claves de acceso de almacenamiento**

### <a name="view-and-copy-endpoint-url-links"></a>Ver y copiar vínculos de dirección URL de punto de conexión  
Cuando se crea una cuenta de almacenamiento, las direcciones URL siguientes se generan con el formato: `https://<Storage Account Name>.<Storage Type>.<Endpoint>`  

Testigo en la nube siempre usa **Blob** como el tipo de almacenamiento. Azure usa **. core.windows.net** como punto de conexión. Al configurar el testigo en la nube, es posible que lo configura con un punto de conexión diferente según el escenario (por ejemplo el centro de datos de Microsoft Azure en China tiene un punto de conexión diferente).  

> [!NOTE]  
> La dirección URL del extremo se genera automáticamente el recurso de testigo en la nube y no hay ningún paso adicional de configuración necesarios para la dirección URL.  

#### <a name="to-view-and-copy-endpoint-url-links"></a>Para ver y copiar la dirección URL del extremo vincula
En el Portal de Azure, vaya a la cuenta de almacenamiento, haga clic en **toda la configuración** y, a continuación, haga clic en **propiedades** para ver y copiar las direcciones URL de punto de conexión (consulte la figura 5).  

![Instantánea de los vínculos de punto de conexión del testigo en la nube](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_5.png)  
**Figura 5: Vínculos de dirección URL de punto de conexión de testigo de nube**

Para obtener más información sobre cómo crear y administrar cuentas de almacenamiento de Azure, consulte [sobre cuentas de almacenamiento de Azure](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/)

## <a name="configure-cloud-witness-as-a-quorum-witness-for-your-cluster"></a>Configurar el testigo en la nube como un testigo de quórum del clúster
Configuración del testigo en la nube es bien integrado en el Asistente de configuración de quórum existente creado en el Administrador de clústeres de conmutación por error.  

### <a name="to-configure-cloud-witness-as-a-quorum-witness"></a>Para configurar el testigo en la nube como un testigo de quórum
1. Inicie el Administrador de clúster de conmutación por error.
2. Con el botón secundario del clúster -> **más acciones** -> **configurar opciones de quórum de clúster** (consulte la figura 6). Esto inicia al Asistente para configurar quórum de clúster.  
    ![Instantánea de la ruta de acceso de menú para configurar opciones de quórum de clúster en la UI del Administrador de clústeres de conmutación por error](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_7.png) **figura 6. Configuración de quórum de clúster**

3. En el **seleccione las configuraciones de quórum** , seleccione **selección del testigo de quórum** (consulte la figura 7).  

    ![Botón en el Asistente para Cuórum de clúster de la opción de instantánea de 'Seleccionar el testigo quotrum'](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_8.png)  
    **Figura 7. Seleccione la configuración de quórum**

4. En el **seleccionar testigo de quórum** , seleccione **configurar un testigo en la nube** (consulte la figura 8).  

    ![Instantánea del botón de radio correspondiente para seleccionar a un testigo en la nube](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_9.png)  
    **Figura 8. Selección del testigo de quórum**  

5. En el **configurar testigo en la nube** , escriba la siguiente información:  
    1. (Parámetro requerido) Nombre de la cuenta de almacenamiento de Azure.  
    2. (Parámetro requerido) Clave de acceso correspondiente a la cuenta de almacenamiento.  
        1. Cuando se crea por primera vez, utilice la clave de acceso principal (consulte la figura 5)  
        2. Al girar la clave de acceso principal, use la clave de acceso secundaria (consulte la figura 5)  
    3. (Parámetro opcional) Si piensa utilizar un punto de conexión de servicio de Azure diferente (por ejemplo, el servicio de Microsoft Azure en China), a continuación, actualice el nombre del servidor de punto de conexión.  

    ![Instantánea del panel de configuración de testigo en la nube en el Asistente para Cuórum de clúster](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_10.png)  
    **Figura 9: Configure el testigo en la nube**

6. Tras la correcta configuración del testigo en la nube, puede ver el recurso de testigo recién creado en el Administrador de clústeres de conmutación por error de complemento (Véase la figura 10).

    ![Configuración correcta de testigo en la nube](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_11.png)  
    **Figura 10: Configuración correcta de testigo en la nube**

### <a name="configuring-cloud-witness-using-powershell"></a>Configuración del testigo en la nube con PowerShell  
El comando de PowerShell Set-ClusterQuorum existente tiene nuevos parámetros adicionales correspondientes a testigo en la nube.  

Puede configurar testigo en la nube mediante el [ `Set-ClusterQuorum` ](https://technet.microsoft.com/library/ee461013.aspx) el siguiente comando de PowerShell:  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
```

En caso de deba usar un punto de conexión diferente (poco frecuente):  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> -Endpoint <servername>  
```

### <a name="azure-storage-account-considerations-with-cloud-witness"></a>Consideraciones de la cuenta de almacenamiento de Azure con testigo en la nube  
Al configurar a un testigo en la nube como un testigo de quórum para el clúster de conmutación por error, tenga en cuenta lo siguiente:
* En lugar de almacenar la clave de acceso, el clúster de conmutación por error generará y almacenar de forma segura un token de seguridad de acceso compartido (SAS).  
* El token SAS generado es válido siempre y cuando la clave de acceso sigue siendo válida. Al girar la clave de acceso principal, es importante actualizar primero el testigo en la nube (en todos los clústeres que estén utilizando esa cuenta de almacenamiento) con la clave de acceso secundaria antes de regenerar la clave de acceso principal.  
* Testigo en la nube utiliza la interfaz de HTTPS REST del servicio en la cuenta de almacenamiento de Azure. Esto significa que necesita el puerto HTTPS estén abiertos en todos los nodos del clúster.

### <a name="proxy-considerations-with-cloud-witness"></a>Consideraciones sobre el proxy con el testigo en la nube  
Testigo en la nube utiliza HTTPS (puerto predeterminado 443) para establecer comunicación con el servicio Azure blob. Asegúrese de que el puerto HTTPS es accesible a través del Proxy de red.

## <a name="see-also"></a>Vea también
- [Novedades de la conmutación por error en Windows Server](whats-new-in-failover-clustering.md)
