---
title: Escenarios de recuperación ante desastres para las infraestructuras Hiperconvergidas
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 03/29/2018
description: En este artículo se describe los escenarios disponibles actualmente para la recuperación ante desastres de Microsoft HCI (espacios de almacenamiento directo)
ms.localizationpriority: medium
ms.openlocfilehash: c844c56c3a1717658bcdb970e78d45b5cdda861c
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453120"
---
# <a name="disaster-recovery-with-storage-spaces-direct"></a>Recuperación ante desastres con espacios de almacenamiento directo

> Se aplica a: Windows Server 2019, Windows Server 2016

Este tema se proporcionan escenarios sobre cómo infraestructuras hiperconvergidas (HCL) se pueden configurar para la recuperación ante desastres.

Numerosas empresas ejecutan hiperconvergido soluciones y planeación de un desastre permite a permanecer en o regresar rápidamente a producción si se produce un desastre. Hay varias maneras de configurar HCI para recuperación ante desastres y este documento explica las opciones que están disponibles hoy en día.

Cuando las discusiones de restaurar la disponibilidad si se produce un desastre giran en torno a lo que se conoce como el objetivo de tiempo de recuperación (RTO). Se trata de la duración del tiempo de destino donde deben restaurarse los servicios para evitar las consecuencias inaceptables para el negocio. En algunos casos, este proceso puede realizarse automáticamente producción restaurado casi inmediatamente. En otros casos, la intervención manual del administrador debe aparecer para restaurar los servicios.

Hoy en día son las opciones de recuperación ante desastres con un hiperconvergido:

1. Varios clústeres de réplica de almacenamiento de uso
2. Réplica de Hyper-V entre los clústeres
3. Copia de seguridad y restauración

## <a name="multiple-clusters-utilizing-storage-replica"></a>Varios clústeres de réplica de almacenamiento de uso

[Réplica de almacenamiento](../storage-replica/storage-replica-overview.md) habilita la replicación de volúmenes y admite la replicación sincrónica y asincrónica. Al elegir entre usar la replicación sincrónica o asincrónica, debe considerar el objetivo de punto de recuperación (RPO). El objetivo de punto de recuperación es la cantidad de posible pérdida de datos que esté dispuesto a acumulando antes de que se considere importante pérdida. Si se decide por la replicación sincrónica, escribirá secuencialmente en ambos extremos al mismo tiempo. Si se decide por asincrónica, escrituras se replicarán muy rápido pero todavía se podrían perder. Debe considerar el uso de la aplicación o el archivo para ver cuál funciona mejor para usted.

Réplica de almacenamiento es un mecanismo de copia de nivel de bloque frente a nivel de archivo; es decir, no importa qué tipos de datos que se replican. Esto hace que una opción popular para infraestructuras hiperconvergidas. Réplica de almacenamiento también puede usar diferentes tipos de unidades entre los asociados de replicación, por lo que tener todo el almacenamiento de un tipo en uno HCI y otro almacenamiento de tipo en el otro es perfectamente correcto. 

Una importante capacidad de réplica de almacenamiento es que se puede ejecutar en Azure, así como en el entorno local. Puede configurar en el entorno local a local, en Azure, o incluso en el entorno local a Azure (o viceversa).

En este escenario, hay dos clústeres independientes independientes. Para configurar la réplica de almacenamiento entre HCI, puede seguir los pasos descritos en [replicación de almacenamiento de clúster a clúster](../storage-replica/cluster-to-cluster-storage-replication.md).

![Diagrama de la replicación de almacenamiento](media/storage-spaces-direct-disaster-recovery/Disaster-Recovery-Figure1.png)

Se aplican las consideraciones siguientes al implementar la réplica de almacenamiento. 

1.  Configurar la replicación se realiza fuera de la agrupación en clústeres de conmutación por error. 
2.  La elección del método de replicación será depende de los requisitos de RPO y la latencia de red. Sincrónico replica los datos en redes de baja latencia con coherencia de bloqueo para no garantizar pérdida de datos en un momento del error. Asincrónico replica los datos a través de redes con latencias más altas, cada sitio, pero no pueden tener copias idénticas en un momento del error. 
3.  En el caso de un desastre, las conmutaciones por error entre los clústeres no son automáticas y deben combinarse manualmente a través de los cmdlets de PowerShell de réplica de almacenamiento. En el diagrama anterior, ClusterA es el principal y ClusterB es el secundario. Si ClusterA deja de funcionar, deberá establecer manualmente ClusterB como principal antes de poner los recursos de. Una vez ClusterA copia de seguridad, necesitaría facilitar la base de datos secundaria. Una vez que se sincronizó todos los datos de seguridad, realizar el cambio e intercambiar los roles de vuelta a la forma en que se habían establecido originalmente.
4.  Puesto que la réplica de almacenamiento solo replica los datos, una nueva máquina virtual o la escala horizontal servidor de archivos (SOFS) utilizando estos datos deberá crearse dentro del Administrador de clústeres de conmutación por error en el asociado de réplica.

Réplica de almacenamiento puede usarse si tiene máquinas virtuales o un SOFS que se ejecutan en el clúster. Poner los recursos en línea en la réplica HCI puede ser manual o automatizada mediante el uso de scripting de PowerShell.

## <a name="hyper-v-replica"></a>Réplica de Hyper-V

[Réplica de Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica) proporciona replicación a nivel de máquina virtual para la recuperación ante desastres en las infraestructuras hiperconvergidas. ¿Qué réplica de Hyper-V puede hacer es poner una máquina virtual y la replicará en un sitio secundario o Azure (réplica). A continuación, desde el sitio secundario, réplica de Hyper-V puede replicar la máquina virtual en un tercer (réplica extendida).

![Diagrama de la replicación de Hyper-V](media/storage-spaces-direct-disaster-recovery/Disaster-Recovery-Figure2.png)

Con la réplica de Hyper-V, se tiene en cuenta la replicación de Hyper-v. Al habilitar una máquina virtual para la replicación en primer lugar, hay tres opciones de cómo desea que la copia inicial para enviarse a los clústeres de réplica correspondiente.

1.  Enviar la copia inicial a través de la red
2.  Enviar la copia inicial en medios externos, por lo que pueden copiarse en el servidor de forma manual
3.  Usar una máquina virtual existente que ya ha creado en los hosts de réplica

La otra opción es para cuando desee que debe llevar a cabo la replicación inicial.

1.  Iniciar inmediatamente la replicación
2.  Programar una hora para cuando realiza la replicación inicial. 

Otras consideraciones que necesitará son:

- Del ¿qué VHD/VHDX que desee replicar. Puede elegir replicar todas ellas o solo uno de ellos.
- Número de puntos de recuperación que desea guardar. Si desea tener varias opciones sobre qué punto en el tiempo que desea restaurar, desearía especificar cuántas desea. Si solo desea un punto de restauración, puede elegir también.
- ¿Con qué frecuencia desea que el servicio de instantáneas de volumen (VSS) replique una instantánea incremental.
- ¿Con qué frecuencia los cambios se replican (30 segundos, 5 minutos, 15 minutos).

Cuando HCI participan en la réplica de Hyper-V, debe tener la [agente de réplica de Hyper-V](https://blogs.technet.microsoft.com/virtualization/2012/03/27/why-is-the-hyper-v-replica-broker-required/) recurso creado en cada clúster. Este recurso realiza varias acciones:

1.  Proporciona un espacio de nombres único para cada clúster de réplica de Hyper-V para conectarse a.
2.  Determina en qué nodo de ese clúster de la réplica (o el servidor réplica extendido) residirá en cuando recibe por primera vez la copia.
3.  Realiza un seguimiento de qué nodo posee la réplica (o el servidor réplica extendido) en caso de que la máquina virtual se mueve a otro nodo. Debe realizar el seguimiento para que cuando produce la replicación, la información pueda enviar al nodo correcto.

## <a name="backup-and-restore"></a>Copia de seguridad y restauración

Una opción de recuperación ante desastres tradicionales que no está hablada mucho, pero es tan importante es el error de todo el clúster o un nodo del clúster. Cualquiera de las opciones con este escenario hace uso de copia de seguridad de Windows NT. 

Siempre es una recomendación para tener copias de seguridad periódicas de las infraestructuras hiperconvergidas. Mientras se ejecuta el servicio de clúster, si realizar una copia de estado del sistema, la base de datos de registro del clúster sería una parte de esa copia de seguridad. Restaurar la base de datos o el clúster tiene dos métodos diferentes (no autoritativo y autorización).

### <a name="non-authoritative"></a>No autoritativa

Una restauración no autoritativa puede realizarse mediante la copia de seguridad de Windows NT y equivale a una restauración completa de solo el nodo de clúster. Si solo necesita restaurar un nodo de clúster (y la base de datos de registro de clúster) y todos los clúster información actual buena, podría restaurar mediante no autoritativa. Restauraciones no autoritativas pueden realizarse a través de la interfaz de la copia de seguridad de Windows NT o la línea de comandos WBADMIN. EXE.

Una vez que restaure el nodo, permite unirse al clúster. Lo que ocurrirá es que iré al clúster de ejecución existente y actualizar toda su información con lo que está actualmente no existe.

### <a name="authoritative"></a>Autorizado

Una restauración autoritativa de la configuración del clúster, por otro lado, toma la configuración del clúster en el tiempo. Este tipo de restauración debe realizarse solo cuando se ha perdido y necesita restaurar la información del clúster. Por ejemplo, alguien elimina accidentalmente un servidor de archivos que contiene los recursos compartidos de más de 1.000 y necesita volver. Completar una restauración autoritativa del clúster requiere que la copia de seguridad puede ejecutar desde la línea de comandos.

Cuando se inicia una restauración autoritativa de un nodo de clúster, se detiene el servicio de clúster en todos los demás nodos en la vista del clúster y se inmoviliza la configuración del clúster. Esto es por eso es fundamental que se puede iniciar primero el servicio de clúster en el nodo en el que se ejecutó la restauración para que el clúster está formado mediante la nueva copia de la configuración del clúster.

Para ejecutar a través de una restauración autoritativa, se pueden realizar los pasos siguientes.

1.  Ejecute WBADMIN. EXE desde un símbolo del sistema administrativo para obtener la versión más reciente de las copias de seguridad que desea instalar y asegúrese de que el estado del sistema es uno de los componentes se pueden restaurar.

    ```powershell
    Wbadmin get versions
    ```

2.  Determinar si la copia de seguridad de la versión que tiene tiene la información del registro de clúster en ella como un componente. Hay algunos elementos que necesita de este comando, la versión y la aplicación o el componente para su uso en el paso 3. Para la versión, por ejemplo, la copia de seguridad se realizó el 3 de enero de 2018 a las 2:04 a.m. y esto es lo que necesita restaurar.

    ```powershell
    wbadmin get items -backuptarget:\\backupserver\location
    ```

3.  Inicie la restauración autoritativa para recuperar solo la versión de clúster del registro que necesita. 

    ```powershell
    wbadmin start recovery -version:01/03/2018-02:04 -itemtype:app -items:cluster
    ```

Una vez que ha realizado la restauración, este nodo debe ser uno para que se inicie el servicio de clúster en primer lugar y formar el clúster. Todos los demás nodos tendría que iniciar y unirse al clúster.

## <a name="summary"></a>Resumen 

Para sumar todo esto, la recuperación ante desastres hiperconvergido es algo que debe planificarse cuidadosamente. Hay varios escenarios que mejor se adapte a sus necesidades y se deben probar exhaustivamente. Un elemento a tener en cuenta es que, si está familiarizado con los clústeres de conmutación por error en el pasado, los clústeres extendidos han sido una opción muy popular durante los años. Se ha producido un poco de un cambio de diseño con la solución hiperconvergida y se basa en la resistencia. Si pierde dos nodos en un clúster hiperconvergido, todo el clúster se dejan de funcionar. Este es el caso, en un entorno hiperconvergido, no se admite el escenario de stretch.


