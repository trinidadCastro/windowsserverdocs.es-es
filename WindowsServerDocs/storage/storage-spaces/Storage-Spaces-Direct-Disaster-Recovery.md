---
title: Escenarios de recuperación ante desastres de infraestructura Hiperconvergida
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 03/29/2018
description: En este artículo se describe los escenarios disponibles actualmente para la recuperación ante desastres de Microsoft HCI (espacios de almacenamiento directo)
ms.localizationpriority: medium
ms.openlocfilehash: 32bbf02ca78d5c6a2147162768c984d0e0b27e36
ms.sourcegitcommit: dfd25348ea3587e09ea8c2224237a3e8078422ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "4678614"
---
# Recuperación ante desastres con espacios de almacenamiento directo

> Se aplica a: Windows Server 2019, Windows Server 2016

Este tema proporciona los escenarios en la infraestructura hiperconvergida (HCI) pueden configurarse para la recuperación ante desastres.

Numerosas empresas ejecutan soluciones hiperconvergidas y planificación de un desastre proporciona la capacidad para permanecer en o volver rápidamente a la producción si se produce un desastre. Hay varias formas de configurar HCI para la recuperación ante desastres y este documento explican las opciones que están disponibles hoy en día.

Cuando las discusiones de restauración de disponibilidad si se produce desastre giran en torno a lo que se conoce como el objetivo de tiempo de recuperación (RTO). Se trata de la duración de tiempo que deben restaurarse servicios para evitar las consecuencias inaceptables a la empresa de destino. En algunos casos, este proceso puede producirse automáticamente con la producción restaurado casi inmediatamente. En otros casos, la intervención manual del administrador debe ocurrir para restaurar los servicios.

Las opciones de recuperación ante desastres con un hiperconvergida hoy en día son:

1. Varios clústeres mediante réplica de almacenamiento
2. Réplica de Hyper-V entre clústeres
3. Realizar una copia de seguridad y restaurarla

## Varios clústeres mediante réplica de almacenamiento

[Réplica de almacenamiento](../storage-replica/storage-replica-overview.md) permite la replicación de volúmenes y admite la replicación sincrónica y asincrónica. Al elegir entre mediante la replicación sincrónica o asincrónica, debes considerar el objetivo de punto de recuperación (RPO). El objetivo de punto de recuperación es la cantidad de posible pérdida de datos que está dispuestos a incurrir en antes de que se considera importante pérdida. Si optas por la replicación sincrónica, lo escribirá secuencialmente en ambos extremos al mismo tiempo. Si optas por asincrónica, las escrituras se replicarán muy rápido, pero aún se podrían perder. Debes considerar el uso de la aplicación o el archivo para ver qué funciona mejor para TI.

Réplica de almacenamiento es un mecanismo de copia de nivel de bloque frente a nivel de archivo; es decir, no importa qué tipos de datos que se replica. Esto hace que una opción popular para la infraestructura hiperconvergida. Réplica de almacenamiento también puede utilizar diferentes tipos de unidades entre los socios de replicación, por lo tanto, tener todo el almacenamiento de información de un tipo en un HCI y otro almacenamiento de tipo en el otro es correcto. 

Una funcionalidad importante de réplica de almacenamiento es que se puede ejecutar en Azure, así como local. Puedes configurar local a local, Azure a Azure, o incluso locales a Azure (o viceversa).

En este escenario, hay dos clústeres independientes independientes. Para configurar la réplica de almacenamiento entre HCI, puedes seguir los pasos de [replicación de almacenamiento de clúster a clúster](../storage-replica/cluster-to-cluster-storage-replication.md).

![Diagrama de replicación de almacenamiento](media\storage-spaces-direct-disaster-recovery\Disaster-Recovery-Figure1.png)

Las consideraciones siguientes se aplican al implementar la réplica de almacenamiento. 

1.  Configurar la replicación se realiza fuera de clústeres de conmutación por error. 
2.  Elegir el método de replicación será depende de la latencia de red y los requisitos de RPO. Sincrónico replica los datos en redes de latencia baja coherencia de bloqueo para garantizar que ninguna pérdida de datos en un momento del error. Asincrónica se replica los datos a través de redes con latencias superiores, pero cada sitio no pueden tener copias idénticas en un momento del error. 
3.  En el caso de un desastre, conmutaciones por error entre los clústeres no son automáticas y que se organiza manualmente a través de los cmdlets de PowerShell de réplica de almacenamiento. En el diagrama anterior, ClusterA es el principal y ClusterB es la secundaria. Si ClusterA deja de funcionar, sería necesario establecer manualmente ClusterB como principal antes de que puede hacer que aparezca los recursos. Una vez que se ClusterA copia de seguridad, sería necesario para que sea secundaria. Una vez que todos los datos se haya sincronizado arriba, realizar el cambio y los roles de volver a la forma en que se hubieran establecido originalmente de intercambio.
4.  Dado que réplica de almacenamiento solo se replica los datos, una nueva máquina virtual o la escala de un vistazo a servidor de archivos (SOFS) que estos datos se deben crearse dentro del Administrador de clústeres de conmutación por error en el asociado de réplica.

Réplica de almacenamiento puede usarse si tienes las máquinas virtuales o una SOFS que se ejecutan en el clúster. Llevar los recursos en línea en la réplica HCI puede ser manual o automática mediante el uso de scripting de PowerShell.

## Réplica de Hyper-V

[Réplica de Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica) proporciona replicación a nivel de máquina virtual para la recuperación ante desastres en las infraestructuras hiperconvergidas. Lo que puede hacer réplica de Hyper-V es tomar una máquina virtual y replicar a un sitio secundario o Azure (réplica). A continuación, desde el sitio secundario, réplica de Hyper-V puede replicar la máquina virtual a un tercer (réplica extendida).

![Diagrama de replicación de Hyper-V](media\storage-spaces-direct-disaster-recovery\Disaster-Recovery-Figure2.png)

Con réplica de Hyper-V, la replicación se ocupa de Hyper-V. Cuando se habilita en primer lugar una máquina virtual para la replicación, existen tres opciones para cómo deseas la copia inicial que se enviará a los clústeres de réplica correspondiente.

1.  Enviar la copia inicial a través de la red
2.  Enviar la copia inicial a medios externos para que se puede copiar en el servidor de forma manual
3.  Usar una máquina virtual existente ya creada en los hosts de réplica

La otra opción es para cuando desee que debe realizarse la replicación inicial.

1.  Iniciar la replicación inmediatamente
2.  Programar una hora para cuando realiza la replicación inicial. 

Otras consideraciones que tendrás que son:

- Del ¿qué VHD/VHDX que desee replicar. Puedes elegir replicar todas ellas o solo uno de ellos.
- Número de puntos de recuperación que desea guardar. Si quieres tener varias opciones sobre qué punto en el tiempo que va a restaurar, podría necesario especificar cuántas quieres. Si solo quieres que un punto de restauración, puedes elegir también.
- ¿Con qué frecuencia desea que el servicio de instantáneas de volumen (VSS) replicar una instantánea incremental.
- ¿Con qué frecuencia los cambios se replican (30 segundos de 5 minutos, 15 minutos).

Cuando HCI participar en réplica de Hyper-V, debe tener el recurso de [Agente de réplica de Hyper-V](https://blogs.technet.microsoft.com/virtualization/2012/03/27/why-is-the-hyper-v-replica-broker-required/) que se creó en cada clúster. Este recurso hace varias cosas:

1.  Proporciona un único espacio de nombres para cada clúster de réplica de Hyper-V para conectarse a.
2.  Determina qué nodo del clúster la réplica (o réplica extendida) reside en cuando recibe la copia en primer lugar.
3.  Realiza un seguimiento de qué nodo posee la réplica (o réplica extendida) en caso de que la máquina virtual se mueve a otro nodo. Debe realizar un seguimiento para que cuando realiza la replicación, puede enviar la información en el nodo adecuado.

## Copia de seguridad y restauración

Una opción de recuperación ante desastres tradicionales que no es muy parecida hablada pero es tan importante es el error de todo el clúster o un nodo del clúster. Cualquiera de las opciones con este escenario hace uso de copia de seguridad de Windows NT. 

Siempre es una recomendación tener copias de seguridad periódicas de la infraestructura hiperconvergida. Mientras se ejecuta el servicio de clúster, si tienes que realizar una copia de estado del sistema, la base de datos de registro de clúster sería una parte de copia de seguridad. Restauración de la base de datos o el clúster tiene dos métodos diferentes (no autoritativos y autoritativos).

### No autoritativa

Una restauración no autoritativa puede realizarse mediante la copia de seguridad de Windows NT y equivale a una restauración completa de simplemente el nodo de clúster. Si solo necesitas restaurar un nodo de clúster (y la base de datos de registro de clúster) y todos los clústeres información actual buena, se restaurarán uso no autorizado. Restaura no autorizado puede hacerse a través de la interfaz de copia de seguridad de Windows NT o la línea de comandos WBADMIN. EXE.

Una vez que se restaura el nodo, permiten unirse al clúster. Lo que sucede es que se ve un vistazo al clúster ejecución existente y actualizar toda su información con lo que está actualmente allí.

### Autoridad

Una restauración de la configuración del clúster, por otro lado, toma la configuración de clúster en el tiempo. Este tipo de restauración solo debe realizarse cuando se ha perdido y necesidades restaura la información del clúster. Por ejemplo, un usuario había eliminado accidentalmente un servidor de archivos que encuentra recursos compartidos de más de 1.000 y necesites volver. Completar una restauración del clúster requiere que se ejecute una copia de seguridad desde la línea de comandos.

Cuando se inicia una restauración en un nodo de clúster, se detiene el servicio de clúster en todos los otros nodos en la vista de clúster y la configuración del clúster se inmoviliza. Esto es por eso es fundamental que el servicio de clúster en el nodo en el que se ejecutó la restauración iniciarse en primer lugar para que el clúster se forma con la nueva copia de la configuración del clúster.

Para ejecutar a través de una restauración, se pueden realizar los siguientes pasos.

1.  Ejecutar WBADMIN. EXE desde un símbolo del sistema administrativo para obtener la versión más reciente de las copias de seguridad que quieres instalar y asegurarse de que el estado del sistema es uno de los componentes puede restaurar.

    ```powershell
    Wbadmin get versions
    ```

2.  Determinar si la copia de seguridad de la versión que tienes tiene la información de registro de clúster en él como un componente. Hay un par elementos que necesitará de este comando, la versión y la aplicación o componente para su uso en el paso 3. Para la versión, por ejemplo, la copia de seguridad ha realizado 3 de enero de 2018 a las 2:04 a.m. y este es el que tienes que restaurar.

    ```powershell
    wbadmin get items -backuptarget:\\backupserver\location
    ```

3.  Inicia la restauración autoritativa para recuperar solo la versión de registro de clúster que necesitas. 

    ```powershell
    wbadmin start recovery -version:01/03/2018-02:04 -itemtype:app -items:cluster
    ```

Una vez que ha realizado la restauración, este nodo debe ser la persona para iniciar el servicio de clúster en primer lugar y formar el clúster. Todos los demás nodos, a continuación, tendría que pueden iniciarse y unirse al clúster.

## Resumen 

Para sumar todo esto, la recuperación ante desastres hiperconvergida es algo que debe ser planeado cuidadosamente. Hay varios escenarios que mejor se adapte a tus necesidades y se deben probar minuciosamente. Un elemento en cuenta es que si estás familiarizado con los clústeres de conmutación por error en el pasado, clústeres extendidos han sido una opción muy popular a través de los años. Se ha producido un poco de un cambio de diseño con la solución hiperconvergida y se basa en la resistencia. Si pierdes dos nodos en un clúster hiperconvergido, todo el clúster pasará hacia abajo. Siendo este el caso, en un entorno hiperconvergida, no se admite el escenario extendido.


