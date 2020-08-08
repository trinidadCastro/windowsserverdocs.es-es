---
title: Escenarios de recuperación ante desastres para la infraestructura hiperconvergida
manager: eldenc
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 03/29/2018
description: En este artículo se describen los escenarios disponibles hoy en día para la recuperación ante desastres de HCl de Microsoft (Espacios de almacenamiento directo)
ms.localizationpriority: medium
ms.openlocfilehash: e154cd4bbb5039e2a35237ec2a4644ebecff8d06
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87961149"
---
# <a name="disaster-recovery-with-storage-spaces-direct"></a>Recuperación ante desastres con Espacios de almacenamiento directo

> Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se proporcionan escenarios en los que se puede configurar la infraestructura hiperconvergida (HCl) para la recuperación ante desastres.

Numerosas empresas ejecutan soluciones hiperconvergidas y la planeación de un desastre ofrece la posibilidad de permanecer en producción rápidamente o volver a ella si se produjera un desastre. Hay varias maneras de configurar HCI para la recuperación ante desastres y en este documento se explican las opciones disponibles hoy en día.

Cuando se realiza un análisis de la disponibilidad si se produce un desastre, gira en torno a lo que se conoce como objetivo de tiempo de recuperación (RTO). Este es el tiempo de espera en el que se deben restaurar los servicios para evitar consecuencias inaceptables en el negocio. En algunos casos, este proceso puede producirse automáticamente con una restauración de producción casi inmediatamente. En otros casos, se debe realizar la intervención manual del administrador para restaurar los servicios.

Las opciones de recuperación ante desastres con una hiperconvergida actualmente son:

1. Varios clústeres que usan réplica de almacenamiento
2. Réplica de Hyper-V entre clústeres
3. Copias de seguridad y restauración

## <a name="multiple-clusters-utilizing-storage-replica"></a>Varios clústeres que usan réplica de almacenamiento

[Réplica de almacenamiento](../storage-replica/storage-replica-overview.md) permite la replicación de volúmenes y admite la replicación sincrónica y asincrónica. Al elegir entre usar la replicación sincrónica o asincrónica, debe tener en cuenta el objetivo de punto de recuperación (RPO). El objetivo de punto de recuperación es la cantidad de posible pérdida de datos que está dispuesto a incurrir antes de que se considere una pérdida importante. Si realiza la replicación sincrónica, se escribirá secuencialmente en ambos extremos al mismo tiempo. Si realiza operaciones asincrónicas, las escrituras se replicarán muy rápido, pero se podrían perder. Debe tener en cuenta el uso de la aplicación o el archivo para ver cuál es el mejor trabajo para usted.

Réplica de almacenamiento es un mecanismo de copia de nivel de bloque frente al nivel de archivo; es decir, no importa qué tipos de datos se están replicando. Esto lo convierte en una opción popular para la infraestructura hiperconvergida. Réplica de almacenamiento también puede emplear distintos tipos de unidades entre los asociados de replicación, por lo que tener todo el almacenamiento de tipo en un HCI y otro almacenamiento de tipo en el otro es perfectamente adecuado.

Una funcionalidad importante de réplica de almacenamiento es que se puede ejecutar en Azure y en el entorno local. Puede configurar de forma local en el entorno local, Azure en Azure o, incluso, en el entorno local a Azure (o viceversa).

En este escenario, hay dos clústeres independientes independientes. Para configurar la réplica de almacenamiento entre HCI, puede seguir los pasos de [replicación de almacenamiento de clúster a clúster](../storage-replica/cluster-to-cluster-storage-replication.md).

![Diagrama de replicación de almacenamiento](media/storage-spaces-direct-disaster-recovery/Disaster-Recovery-Figure1.png)

Al implementar réplica de almacenamiento se aplican las consideraciones siguientes.

1.    La configuración de la replicación se realiza fuera de los clústeres de conmutación por error.
2.    La elección del método de replicación dependerá de los requisitos de RPO y latencia de red. Sincrónico replica los datos en redes de baja latencia con coherencia de bloqueo para garantizar que no se produzcan pérdidas de datos en el momento del error. Asincrónica replica los datos a través de redes con latencias mayores, pero es posible que cada sitio no tenga copias idénticas en un momento del error.
3.    En el caso de un desastre, las conmutaciones por error entre los clústeres no son automáticas y deben orquestarse manualmente a través de los cmdlets de PowerShell de réplica de almacenamiento. En el diagrama anterior, Clustera es el principal y ClusterB es el secundario. Si Clustera deja de funcionar, tendrá que establecer manualmente ClusterB como principal para poder poner los recursos en marcha. Una vez que se realiza una copia de seguridad de Clustera, deberá convertirlo en secundario. Una vez que todos los datos se han sincronizado, realice el cambio y intercambie los roles de nuevo a la manera en que se establecieron originalmente.
4.    Puesto que réplica de almacenamiento solo replica los datos, es necesario crear una nueva máquina virtual o Escalabilidad horizontal servidor de archivos (SOFS) que usa estos datos dentro de Administrador de clústeres de conmutación por error en el asociado de réplica.

La réplica de almacenamiento se puede usar si tiene máquinas virtuales o un SOFS que se ejecuta en el clúster. Poner los recursos en línea en la réplica de HCl puede ser manual o automatizado mediante el uso de scripting de PowerShell.

## <a name="hyper-v-replica"></a>Réplica de Hyper-V

[Réplica de Hyper-V](../../virtualization/hyper-v/manage/set-up-hyper-v-replica.md) proporciona replicación de nivel de máquina virtual para la recuperación ante desastres en infraestructuras hiperconvergidas. Lo que puede hacer la réplica de Hyper-V es tomar una máquina virtual y replicarla en un sitio secundario o en Azure (réplica). A continuación, desde el sitio secundario, la réplica de Hyper-V puede replicar la máquina virtual en una tercera (réplica extendida).

![Diagrama de replicación de Hyper-V](media/storage-spaces-direct-disaster-recovery/Disaster-Recovery-Figure2.png)

Con la réplica de Hyper-V, Hyper-V se encarga de la replicación. La primera vez que se habilita una máquina virtual para la replicación, hay tres opciones para el modo en que desea que se envíe la copia inicial a los clústeres de réplica correspondientes.

1.    Enviar la copia inicial a través de la red
2.    Enviar la copia inicial a medios externos para que se pueda copiar manualmente en el servidor
3.    Usar una máquina virtual existente ya creada en los hosts de réplica

La otra opción es para cuando desea que se produzca esta replicación inicial.

1.    Iniciar la replicación inmediatamente
2.    Programe una hora en la que tenga lugar la replicación inicial.

Otras consideraciones que necesitará son:

- Qué VHD/VHDX quiere replicar. Puede optar por replicar todos ellos o solo uno de ellos.
- Número de puntos de recuperación que desea guardar. Si desea tener varias opciones sobre el momento dado que desea restaurar, querrá especificar el número de veces que desee. Si solo desea un punto de restauración, puede elegirlo.
- La frecuencia con la que desea que el Servicio de instantáneas de volumen (VSS) replique una instantánea incremental.
- Frecuencia con la que se replican los cambios (30 segundos, 5 minutos, 15 minutos).

Cuando HCI participa en la réplica de Hyper-V, debe tener el recurso del [agente de réplicas de Hyper-v](https://techcommunity.microsoft.com/t5/virtualization/bg-p/Virtualization) creado en cada clúster. Este recurso realiza varias acciones:

1.    Proporciona un espacio de nombres único para cada clúster en el que la réplica de Hyper-V se conecta.
2.    Determina qué nodo de ese clúster residirá la réplica (o réplica extendida) cuando reciba la copia por primera vez.
3.    Realiza un seguimiento del nodo que posee la réplica (o réplica extendida) en caso de que la máquina virtual se mueva a otro nodo. Es necesario realizar un seguimiento de esto para que cuando tenga lugar la replicación, pueda enviar la información al nodo adecuado.

## <a name="backup-and-restore"></a>Copia de seguridad y restauración

Una opción de recuperación ante desastres tradicional que no se habla mucho pero es igual de importante es el error de todo el clúster o de un nodo del clúster. Cualquiera de las opciones de este escenario hace uso de copias de seguridad de Windows NT.

Siempre es una recomendación tener copias de seguridad periódicas de la infraestructura hiperconvergida. Mientras se ejecuta el servicio de clúster, si realiza una copia de seguridad del estado del sistema, la base de datos del registro del clúster formará parte de esa copia de seguridad. La restauración del clúster o de la base de datos tiene dos métodos diferentes (no autoritativos y autoritativos).

### <a name="non-authoritative"></a>No autoritativo

Una restauración no autoritativa puede realizarse mediante la copia de seguridad de Windows NT y equivale a una restauración completa solo del nodo del clúster. Si solo necesita restaurar un nodo de clúster (y la base de datos del registro del clúster) y toda la información del clúster actual, debe realizar la restauración mediante no autoritativo. Las restauraciones no autoritativas se pueden realizar a través de la interfaz de copia de seguridad de Windows NT o la WBADMIN.EXE de línea de comandos.

Una vez que restaure el nodo, deje que se una al clúster. Lo que ocurrirá es que pasará al clúster en ejecución existente y actualizará toda su información con lo que hay actualmente.

### <a name="authoritative"></a>Restaur

Por otro lado, una restauración autoritativa de la configuración del clúster lleva al tiempo la configuración del clúster. Este tipo de restauración solo se debe realizar cuando se ha perdido la información del clúster y es necesario restaurarla. Por ejemplo, alguien eliminó accidentalmente un servidor de archivos que contenía más de 1000 recursos compartidos y los necesita de nuevo. Completar una restauración autoritativa del clúster requiere que se ejecute la copia de seguridad desde la línea de comandos.

Cuando se inicia una restauración autoritativa en un nodo de clúster, el servicio de clúster se detiene en todos los demás nodos de la vista de clúster y la configuración del clúster está inmovilizada. Esta es la razón por la que es fundamental que el servicio de clúster en el nodo en el que se ejecutó la restauración se inicie primero, por lo que el clúster se forma con la nueva copia de la configuración del clúster.

Para ejecutar una restauración autoritativa, se pueden realizar los siguientes pasos.

1. Ejecute WBADMIN.EXE desde un símbolo del sistema administrativo para obtener la versión más reciente de las copias de seguridad que desea instalar y asegúrese de que el estado del sistema es uno de los componentes que se pueden restaurar.

    ```powershell
    wbadmin get versions
    ```

2. Determine si la copia de seguridad de la versión que tiene contiene la información del registro del clúster como componente. Hay un par de elementos que necesitará de este comando, la versión y la aplicación o componente que se usará en el paso 3. Por ejemplo, para la versión, Imagine que la copia de seguridad se realizó el 3 de enero de 2018 a las 2:04am y esta es la que necesita restaurar.

    ```powershell
    wbadmin get items -backuptarget:\\backupserver\location
    ```

3. Inicie la restauración autoritativa para recuperar solo la versión del registro del clúster que necesite.

    ```powershell
    wbadmin start recovery -version:01/03/2018-02:04 -itemtype:app -items:cluster
    ```

Una vez que se realiza la restauración, este nodo debe ser el primero en iniciar el servicio de clúster y formar el clúster. A continuación, todos los demás nodos deben iniciarse y unirse al clúster.

## <a name="summary"></a>Resumen

Para sumar todo esto, la recuperación ante desastres hiperconvergida es algo que debe planearse con cuidado. Hay varios escenarios que se adaptan mejor a sus necesidades y deben probarse exhaustivamente. Un elemento a tener en cuenta es que si está familiarizado con los clústeres de conmutación por error en el pasado, los clústeres extendidos han sido una opción muy popular a lo largo de los años. Hubo un poco de un cambio de diseño con la solución hiperconvergida y se basa en la resistencia. Si pierde dos nodos en un clúster hiperconvergido, todo el clúster dejará de funcionar. En este caso, en un entorno hiperconvergido, no se admite el escenario de Stretch.
