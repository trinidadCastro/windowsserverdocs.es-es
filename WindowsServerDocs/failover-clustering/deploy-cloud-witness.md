---
ms.assetid: 0cd1ac70-532c-416d-9de6-6f920a300a45
title: "Implementar a un testigo de nube para un clúster de conmutación por error"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.topic: article
author: JasonGerend
ms.date: 10/2/2017
description: "Cómo usar Microsoft Azure para hospedar el testigo para un clúster de conmutación por error de Windows Server en la nube, también conocido como cómo implementar un testigo de nube."
ms.openlocfilehash: 564c6668fcc80a8bd1531c05c142996689de8154
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="deploy-a-cloud-witness-for-a-failover-cluster"></a>Implementar a un testigo de nube para un clúster de conmutación por error

> Se aplica a: se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Nube auxiliar es un nuevo tipo de testigo de quórum de clúster de conmutación por error que se introdujo en Windows Server 2016. Este tema proporciona una visión general de la función auxiliar de la nube, los escenarios que admite e instrucciones sobre cómo configurar a un testigo de nube para un clúster de conmutación por error que se ejecuta Windows Server 2016.

## <a name="CloudWitnessOverview"></a>Introducción a auxiliares en la nube

Figura 1 muestra una configuración de quórum de clúster de conmutación por error extendida de múltiples sitios con Windows Server 2016. En este ejemplo de configuración (figura 1), hay 2 nodos en 2 centros de datos (denominados sitios). Ten en cuenta que es posible que un clúster que abarque más de 2 centros de datos. Además, cada centro de datos puede tener más de 2 nodos. Una configuración de quórum de clúster típica de este programa de instalación (conmutación automática por error SLA) proporciona a cada nodo voto. Uno testigo quórum dispone de voto extra para permitir el clúster para mantener un funcionamiento incluso si del centro de datos experimenta una interrupción de la energía. La expresión matemática es simple: hay 5 votos totales y necesitas 3 votos para el clúster que siga ejecutándose.  

![Testigo de recurso compartido de archivos en un tercer independiente del sitio con los 2 nodos en 2 otros sitios](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_1.png "testigo del recurso compartido de archivos")  
**Figura 1: Uso de un testigo del recurso compartido de archivos como testigo quórum**  

En el caso de interrupción de energía en un centro de datos, para dar la oportunidad de igual para el clúster en otro centro de datos para mantener en ejecución, se recomienda para hospedar al testigo quórum en una ubicación distinta de los dos centros de datos. Esto normalmente significa que requieren una tercera separa datacenter (sitio) para hospedar un servidor de archivos que se está realizando copias el recurso compartido de archivos que se usa como testigo quórum (testigo de recurso compartido de archivos).  

La mayoría de las organizaciones no tienen una tercera separar el centro de datos que se hospedará el testigo del recurso compartido de archivos de respaldo de servidor de archivos. Esto significa que las organizaciones hospedar principalmente el servidor de archivos en uno de los dos centros de datos, por extensión, lo que hace que ese centro de datos del centro de datos principal. En un escenario donde hay corte de energía en el centro de datos principal, el clúster iría hacia abajo que el centro de datos otro solo tendría 2 votos que está por debajo de la mayoría de quórum de 3 votos es necesario. Para los clientes que tienen tercer datacenter independiente para hospedar el servidor de archivos, es una sobrecarga para mantener el servidor de archivos altamente disponible testigo del recurso compartido de archivos de respaldo. Hospedaje de máquinas virtuales en la nube pública que tienen el servidor de archivos de testigo de recurso compartido de archivos ejecutándose en el SO invitado es una sobrecarga significativa en términos del programa de instalación y mantenimiento.  

Nube auxiliar es un nuevo tipo de testigo de quórum de clúster de conmutación por error que saca provecho de Microsoft Azure como punto de arbitraje (figura 2). Usa el almacenamiento de blobs de Azure a un archivo de blob que, a continuación, se utiliza como punto de arbitraje en el caso de resolución de división del núcleo de lectura y escritura.  

Hay importantes ventajas que este enfoque:
1. Saca provecho de Microsoft Azure (no es necesario para el centro de datos diferente en tercer lugar).  
2. Usa el estándar almacenamiento de blobs de Azure disponible (ninguna sobrecarga de mantenimiento adicionales de equipos virtuales alojados en la nube pública).  
3. Misma cuenta de almacenamiento de Azure puede usarse para varios clústeres (archivo de un blob por clúster; utilizado como nombre de archivo del blob de identificador único de clúster).  
4. $Cost continua muy bajo para la cuenta de almacenamiento (muy pequeños datos escritos por cada archivo de blob, actualiza el archivo de blob solo una vez cuando cambia el estado de los nodos del clúster).  
5. Tipo de recurso de nube auxiliares integrado.  

![Diagrama que ilustra un clúster estirado múltiples sitios con testigo de nube como testigo quórum](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_2.png)  
**Figura 2: Múltiples sitios estira clústeres con testigo de nube como testigo quórum**  

Como se muestra en la figura 2, no hay ningún sitio independiente de terceros que se necesita. Testigo de nube, como cualquier otro testigo quórum, obtiene un voto y participa en los cálculos de quórum.  

## <a name="CloudWitnessSupportedScenarios"></a>Auxiliar de nube: Tipo auxiliares solo admite escenarios
Si tienes una implementación de clúster de conmutación por error, donde todos los nodos pueden conectarse a internet (por extensión de Azure), se recomienda que configure un testigo de nube como los recursos de quórum auxiliar.  

Algunos de los escenarios que se admiten usan de nube auxiliar como un testigo quórum son los siguientes:  
- La recuperación ante desastres estirado clústeres de varios sitios (consulta la figura 2).  
- Clústeres de conmutación por error sin almacenamiento compartido (SQL siempre en etcetera.).  
- Clústeres de conmutación por error que se ejecutan dentro de SO invitado hospedado en función de máquina Virtual de Microsoft Azure (o cualquier otro nube pública).  
- Clústeres de conmutación por error que se ejecutan dentro de invitado OS de equipos virtuales alojados en las nubes privadas.
- Almacenamiento clústeres con o sin el almacenamiento compartido, tales como los clústeres de servidor de archivos de escalado.  
- Clústeres de sucursales de pequeño (2 nodos incluso clústeres)  

A partir de Windows Server 2012 R2, se recomienda configurar siempre un testigo como el clúster administra automáticamente el voto auxiliares y los nodos voten con quórum dinámico.  

## <a name="CloudWitnessSetUp"></a>Configurar un testigo de nube para un clúster
Para configurar un testigo de nube como testigo quórum para el clúster, completa los siguientes pasos:
1. Crear una cuenta de almacenamiento de Azure para usar como un testigo de nube
2. Configurar al testigo de nube como testigo quórum para el clúster.

## <a name="create-an-azure-storage-account-to-use-as-a-cloud-witness"></a>Crear una cuenta de almacenamiento de Azure para usar como un testigo de nube

Esta sección describe cómo crear las direcciones URL de un extremo de cuenta y la vista y la copia de almacenamiento y acceso a claves de esa cuenta.

Para configurar a testigo de nube, debes tener una cuenta válida de almacenamiento de Azure que puede usarse para almacenar el archivo de blob (usado para arbitraje). Auxiliar de nube crea un contenedor conocido **testigo de nube msft** en la cuenta de almacenamiento de Microsoft. Nube auxiliares escrituras clúster de un archivo de blob solo con correspondiente del Id. único que se usa como el nombre de archivo del archivo de blob en virtud de este **testigo de nube msft** contenedor. Esto significa que puedes usar la misma cuenta de almacenamiento de Microsoft Azure para configurar a un testigo de nube para varios clústeres diferentes.

Cuando usas la misma cuenta de almacenamiento de Azure para configurar testigo de nube de varios diferentes clústeres de una sola **testigo de nube msft** contenedor se crea automáticamente. Este contenedor contendrá el blob de un archivo por clúster.

### <a name="to-create-an-azure-storage-account"></a>Para crear una cuenta de almacenamiento de Azure

1. Inicia sesión en el [Portal de Azure](http://portal.azure.com).
2. En el menú de navegación centralizada, selecciona Nuevo -> datos y almacenamiento -> cuenta de almacenamiento.
3. En el crear una página de cuenta de almacenamiento, haz lo siguiente:
    1. Escribe un nombre para tu cuenta de almacenamiento.
    <br>Los nombres de cuenta de almacenamiento deben tener entre 3 y 24 caracteres de longitud y pueden contener números y letras minúsculas solo. El nombre de cuenta de almacenamiento también debe ser único dentro de Azure.
        
    2. Para **cuenta kind**, selecciona **generales**.
    <br>No puedes usar una cuenta de almacenamiento de Blob para un testigo de nube.
    3. Para **rendimiento**, selecciona **estándar**.
    <br>No puedes usar el almacenamiento de Azure Premium para un testigo de nube.
    2. Para **replicación**, selecciona **almacenamiento redundante local (LRS)**.
    <br>Clústeres de conmutación por error, se usa el archivo de blob como punto de arbitraje, lo que requiere algunos garantías coherencia al leer los datos. Debes seleccionar como **almacenamiento redundante local** para **replicación** tipo.

### <a name="view-and-copy-storage-access-keys-for-your-azure-storage-account"></a>Ver y copiar las teclas de acceso de almacenamiento de tu cuenta de almacenamiento de Azure

Cuando creas una cuenta de almacenamiento de Microsoft Azure, se asocia con dos teclas de acceso que se generan automáticamente - tecla de acceso principal y la clave de acceso secundarias. Para crear una primera vez testigo de nube, usa el **clave principal de acceso**. No hay ninguna restricción sobre qué clave para testigo de nube.  

#### <a name="to-view-and-copy-storage-access-keys"></a>Para ver y copiar las teclas de acceso de almacenamiento

En el Portal de Azure, ve a tu cuenta de almacenamiento, haz clic en **toda la configuración** y, a continuación, haz clic en **las teclas de acceso** para ver, copiar y volver a generar las claves de acceso de la cuenta. El módulo de las teclas de acceso también incluye cadenas de conexión preconfigurado mediante las teclas principales y secundarias que se pueden copiar para usar en tus aplicaciones (consulta la figura 4).

![Instantánea del cuadro de diálogo Administrar claves de acceso en Microsoft Azure](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_4.png)  
**La figura 4: Las teclas de acceso de almacenamiento**

### <a name="view-and-copy-endpoint-url-links"></a>Ver y copiar los vínculos de la dirección URL de extremo  
Cuando creas una cuenta de almacenamiento, se generan las siguientes direcciones URL con el formato: `https://<Storage Account Name>.<Storage Type>.<Endpoint>`  

Auxiliar de nube siempre usa **Blob** como el tipo de almacenamiento. Usa Azure **. core.windows.net** como el punto final. Cuando configures a testigo de nube, es posible que lo configure con un extremo diferente según el escenario (por ejemplo el centro de datos de Microsoft Azure en China tiene un extremo diferente).  

> [!NOTE]  
> Recursos de nube auxiliares genera automáticamente la dirección URL de extremo y no hay ningún paso de configuración adicional necesario para la dirección URL.  

#### <a name="to-view-and-copy-endpoint-url-links"></a>Para ver y copiar los vínculos de la dirección URL de extremo
En el Portal de Azure, ve a tu cuenta de almacenamiento, haz clic en **toda la configuración** y, a continuación, haz clic en **propiedades** ver y copiar las direcciones URL de extremo (consulta la figura 5).  

![Instantánea de los vínculos de extremo testigo de nube](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_5.png)  
**Figura 5: Vínculos de dirección URL de extremo testigo de nube**

Para obtener más información sobre la creación y administración de cuentas de almacenamiento de Azure, consulta [sobre cuentas de almacenamiento de Azure](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/)

## <a name="configure-cloud-witness-as-a-quorum-witness-for-your-cluster"></a>Configurar a testigo de nube como testigo quórum para el clúster
Configuración de nube auxiliar es bien integrado en el Asistente de configuración quórum existente integrado en el Administrador de clúster de conmutación por error.  

### <a name="to-configure-cloud-witness-as-a-quorum-witness"></a>Para configurar a testigo de nube como testigo quórum
1. Inicia el Administrador de clúster de conmutación por error.
2. Con el botón derecho el clúster -> **más acciones** -> **establecer la configuración de clúster quórum** (consulta la figura 6). Esto inicia al Asistente para configurar quórum de clúster.  
    ![Instantánea de la ruta de acceso menú Configue clúster quórum configuración en la UI del Administrador de clúster de conmutación por error](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_7.png)**figura 6. Configuración de quórum de clúster**

3. En la **selecciona las configuraciones de quórum**, seleccione **selecciona testigo quórum** (consulta la figura 7).  

    ![En el Asistente para clúster quórum botón de radio de instantánea de la 'testigo quotrum seleccionar'](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_8.png)  
    **Figura 7. Selecciona la configuración de quórum**

4. En la **selecciona quórum auxiliares**, seleccione **configurar un testigo de nube** (consulta la figura 8).  

    ![Instantánea del botón de opción adecuado para seleccionar a un testigo de nube](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_9.png)  
    **Figura 8. Selecciona al testigo quórum**  

5. En la **configurar testigo de nube**, escriba la siguiente información:  
    1. (Parámetro obligatorio) Nombre de cuenta de almacenamiento de Azure.  
    2. (Parámetro obligatorio) Tecla de acceso correspondiente a la cuenta de almacenamiento.  
        1. Al crear por primera vez, usa la clave principal de acceso (consulta la figura 5)  
        2. Al girar la clave principal de acceso, usar tecla de acceso secundarias (consulta la figura 5)  
    3. (Parámetro opcional) Si quieres utilizar un extremo de otro servicio de Azure (por ejemplo, el servicio de Microsoft Azure en China), a continuación, actualiza el nombre de servidor del extremo.  

    ![Instantánea del panel de configuración testigo de nube en el Asistente para quórum de clúster](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_10.png)  
    **Figura 9: Configurar a su testigo de nube**

6. Tras la configuración correcta de nube auxiliares, puedes ver el recurso testigo recién creado en el Administrador de clúster de conmutación por error complemento (consulta la figura 10).

    ![Configuración correcta de nube auxiliares](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_11.png)  
    **Figura 10: La configuración correcta de nube auxiliares**

### <a name="configuring-cloud-witness-using-powershell"></a>Configurar testigo de nube con PowerShell  
El comando de PowerShell Set-ClusterQuorum existente tiene nuevos parámetros adicionales correspondientes a la nube auxiliar.  

Puede configurar mediante testigo de nube el [`Set-ClusterQuorum`](https://technet.microsoft.com/library/ee461013.aspx)siguiente comando de PowerShell:  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
```

En caso de que necesitas usar un extremo diferente (raros):  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> -Endpoint <servername>  
```

### <a name="azure-storage-account-considerations-with-cloud-witness"></a>Consideraciones de cuenta de almacenamiento Azure con testigo de nube  
Cuando configures a un testigo de nube como testigo quórum para el clúster de conmutación por error, ten en cuenta lo siguiente:
* En lugar de almacenar la clave de acceso, el clúster de conmutación por error generará y almacenar de forma segura un token de seguridad de acceso compartido (SAS).  
* El token de asociaciones de seguridad generado es válido, como la tecla de acceso sigue siendo válida. Al girar la clave principal de acceso, es importante actualizar primero el testigo de nube (en todos los clústeres de que están usando esa cuenta de almacenamiento) con la tecla de acceso secundario antes de volver a generar la clave principal de acceso.  
* Auxiliar de nube usa interfaz REST de HTTPS de servicio de la cuenta de almacenamiento de Azure. Esto significa que requiere el puerto HTTPS que se abra en todos los nodos del clúster.

### <a name="proxy-considerations-with-cloud-witness"></a>Consideraciones de proxy con testigo de nube  
Auxiliar de nube usa HTTPS (puerto 443 predeterminado) para establecer comunicación con el servicio de blobs de Azure. Asegúrate de que el puerto HTTPS sea accesible a través de Proxy de red.

## <a name="see-also"></a>Consulta también
- [Novedades de conmutación por error en Windows Server](whats-new-in-failover-clustering.md)
