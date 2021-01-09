---
title: Ajuste del umbral de red de la línea de base de conmutación por error
description: En este artículo se presentan soluciones para ajustar el umbral de las redes de clústeres de conmutación por error.
ms.date: 05/28/2020
author: Deland-Han
ms.author: delhan
ms.topic: troubleshooting
ms.openlocfilehash: 5cce39ea42af57bbfd400427f763dfca5da75d7b
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040305"
---
# <a name="iaas-with-sql-server---tuning-failover-cluster-network-thresholds"></a>IaaS con umbrales de red de clústeres de conmutación por error de optimización de SQL Server

En este artículo se presentan soluciones para ajustar el umbral de las redes de clústeres de conmutación por error.

## <a name="symptom"></a>Síntoma

Cuando se ejecutan nodos de clúster de conmutación por error de Windows en IaaS con un SQL Server Always On grupo de disponibilidad, se recomienda cambiar la configuración de clúster a un estado de supervisión más flexible. La configuración del clúster fuera de la caja es restrictiva y podría provocar interrupciones innecesarias. La configuración predeterminada está diseñada para redes locales muy optimizadas y no tiene en cuenta la posibilidad de una latencia inducida causada por un entorno de varios inquilinos como Windows Azure (IaaS).

Los clústeres de conmutación por error de Windows Server están supervisando constantemente las conexiones de red y el estado de los nodos de un clúster de Windows.  Si un nodo no es accesible a través de la red, se toma la acción de recuperación para recuperar y poner las aplicaciones y servicios en línea en otro nodo del clúster. La latencia en la comunicación entre los nodos del clúster puede producir el siguiente error:

> Error 1135 (registro de eventos del sistema)

El nodo de clúster **nodo1** se quitó de la pertenencia al clúster de conmutación por error activa. Es posible que se haya detenido el Servicio de clúster en este nodo. Esto también podría deberse a que el nodo ha perdido la comunicación con otros nodos activos en el clúster de conmutación por error. Ejecute el Asistente para validar una configuración para comprobar la configuración de red. Si la condición persiste, compruebe si hay errores de hardware o software relacionados con los adaptadores de red de este nodo. Compruebe también si hay errores en otros componentes de red a los que está conectado el nodo, como concentradores, conmutadores o puentes.

Ejemplo de cluster. log:

```console
0000ab34.00004e64::2014/06/10-07:54:34.099 DBG   [NETFTAPI] Signaled NetftRemoteUnreachable event, local address 10.xx.x.xxx:3343 remote address 10.x.xx.xx:3343
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] got event: Remote endpoint 10.xx.xx.xxx:~3343~ unreachable from 10.xx.x.xx:~3343~
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Marking Route from 10.xxx.xxx.xxxx:~3343~ to 10.xxx.xx.xxxx:~3343~ as down
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [NDP] Checking to see if all routes for route (virtual) local fexx::xxx:5dxx:xxxx:3xxx:~0~ to remote xxx::cxxx:xxxd:xxx:dxxx:~0~ are down
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [NDP] All routes for route (virtual) local fxxx::xxxx:5xxx:xxxx:3xxx:~0~ to remote fexx::xxxx:xxxx:xxxx:xxxx:~0~ are down
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [CORE] Node 8: executing node 12 failed handlers on a dedicated thread
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [NODE] Node 8: Cleaning up connections for n12.
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [Nodename] Clearing 0 unsent and 15 unacknowledged messages.
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [NODE] Node 8: n12 node object is closing its connections
0000ab34.00008b68::2014/06/10-07:54:34.099 INFO  [DCM] HandleNetftRemoteRouteChange
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Route history 1: Old: 05.936, Message: Response, Route sequence: 150415, Received sequence: 150415, Heartbeats counter/threshold: 5/5, Error: Success, NtStatus: 0 Timestamp: 2014/06/10-07:54:28.000, Ticks since last sending: 4
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [NODE] Node 8: closing n12 node object channels
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Route history 2: Old: 06.434, Message: Request, Route sequence: 150414, Received sequence: 150402, Heartbeats counter/threshold: 5/5, Error: Success, NtStatus: 0 Timestamp: 2014/06/10-07:54:27.665, Ticks since last sending: 36
0000ab34.0000a8ac::2014/06/10-07:54:34.099 INFO  [DCM] HandleRequest: dcm/netftRouteChange
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Route history 3: Old: 06.934, Message: Response, Route sequence: 150414, Received sequence: 150414, Heartbeats counter/threshold: 5/5, Error: Success, NtStatus: 0 Timestamp: 2014/06/10-07:54:27.165, Ticks since last sending: 4
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Route history 4: Old: 07.434, Message: Request, Route sequence: 150413, Received sequence: 150401, Heartbeats counter/threshold: 5/5, Error: Success, NtStatus: 0 Timestamp: 2014/06/10-07:54:26.664, Ticks since last sending: 36
```

```console
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <realLocal>10.xxx.xx.xxx:~3343~</realLocal>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <realRemote>10.xxx.xx.xxx:~3343~</realRemote>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <virtualLocal>fexx::xxxx:xxxx:xxxx:xxxx:~0~</virtualLocal>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <virtualRemote>fexx::xxxx:xxxx:xxxx:xxxx:~0~</virtualRemote>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <Delay>1000</Delay>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <Threshold>5</Threshold>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <Priority>140481</Priority>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <Attributes>2147483649</Attributes>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO  </struct mscs::FaultTolerantRoute>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO   removed
```

```console
0000ab34.0000a7c0::2014/06/10-07:54:38.433 ERR   [QUORUM] Node 8: Lost quorum (3 4 5 6 7 8)
0000ab34.0000a7c0::2014/06/10-07:54:38.433 ERR   [QUORUM] Node 8: goingAway: 0, core.IsServiceShutdown: 0
0000ab34.0000a7c0::2014/06/10-07:54:38.433 ERR   lost quorum (status = 5925)
```

## <a name="cause"></a>Causa

Hay dos opciones de configuración que se usan para configurar el estado de Conectividad del clúster.

**Delay** : define la frecuencia con la que se envían los latidos de clúster entre los nodos.  El retraso es el número de segundos antes de que se envíe el siguiente latido.  En el mismo clúster, pueden producirse retrasos diferentes entre los nodos de la misma subred y entre los nodos, que se encuentran en subredes diferentes.

**Umbral** : define el número de latidos que se omiten antes de que el clúster realice la acción de recuperación.  El umbral es un número de latidos.  En el mismo clúster, puede haber diferentes umbrales entre los nodos de la misma subred y entre los nodos que se encuentran en subredes diferentes.

De forma predeterminada, Windows Server 2016 establece **SameSubnetThreshold** en 10 y **SameSubnetDelay** en 1000 ms. Por ejemplo, si se produce un error en la supervisión de la conectividad durante 10 segundos, se alcanza el umbral de conmutación por error, lo que da lugar a que el nodo se quite de la pertenencia al clúster. Esto hace que los recursos se muevan a otro nodo disponible en el clúster. Se informará de los errores del clúster, incluido el error 1135 del clúster (anterior).

## <a name="resolution"></a>Resolución

En un entorno de IaaS, relajar las opciones de configuración de red de clústeres.

### <a name="steps-to-verify-current-configuration"></a>Pasos para comprobar la configuración actual

Compruebe los valores de configuración de la red del clúster actual y use el comando Get-Cluster:

```powershell
C:\Windows\system32> get-cluster | fl *subnet*
```

Valores predeterminados, mínimo, máximo y recomendado para cada sistema operativo compatible

| Descripción | SO | Min | Max | Valor predeterminado | Recomendado |
|--|--|--|--|--|--|
| CrossSubnetThreshold | 2008 R2 | 3 | 20 | 5 | 20 |
| Umbral de CrossSubnet | 2012 | 3 | 120 | 5 | 20 |
| Umbral de CrossSubnet | 2012 R2 | 3 | 120 | 5 | 20 |
| Umbral de CrossSubnet | 2016 | 3 | 120 | 20 | 20 |
| Umbral de SameSubnet | 2008 R2 | 3 | 10 | 5 | 10 |
| Umbral de SameSubnet | 2012 | 3 | 120 | 5 | 10 |
| Umbral de SameSubnet | 2012 R2 | 3 | 120 | 5 | 10 |
| SameSubnetThreshold | 2016 | 3 | 120 | 10 | 10 |

Los valores del umbral reflejan las recomendaciones actuales sobre el ámbito de la implementación como se describe en el artículo siguiente:

[Ajustar los umbrales de red de clústeres de conmutación por error en Windows Server 2012 R2](https://support.microsoft.com/en-us/help/3153887/fine-tuning-failover-cluster-network-thresholds-in-windows-server-2012)

El **umbral** define el número de latidos que se omiten antes de que el clúster realice la acción de recuperación.  El umbral es un número de latidos.  En el mismo clúster, puede haber diferentes umbrales entre los nodos de la misma subred y entre los nodos, que se encuentran en subredes diferentes.

## <a name="recommendations-for-changing-to-more-relaxed-settings-for-multi-tenant-environments-like-azure-iaas"></a>Recomendaciones para cambiar a una configuración más relajada para entornos de varios inquilinos como Azure (IaaS)

> [!NOTE]
> El aumento de la resistencia del entorno de clúster mediante el ajuste de los valores de configuración de la red del clúster puede provocar un aumento del tiempo de inactividad. Para obtener más información, consulte [ajustar umbrales de red de clústeres de conmutación por error](https://techcommunity.microsoft.com/t5/failover-clustering/tuning-failover-cluster-network-thresholds/ba-p/371834).

1. Modifique a una configuración más relajada:

    > [!NOTE]
    > Cambiar el umbral del clúster surtirá efecto inmediatamente, no tendrá que reiniciar el clúster ni los recursos.

    Se recomienda la siguiente configuración para la misma subred y para implementaciones entre regiones de grupos de disponibilidad.

    ```powershell
    C:\Windows\system32> (get-cluster).SameSubnetThreshold = 20
    ```

    ```powershell
    C:\Windows\system32> (get-cluster).CrossSubnetThreshold = 20
    ```

2. Compruebe los cambios:

    ```powershell
    C:\Windows\system32> get-cluster | fl *subnet*
    ```

    :::image type="content" source="media/iaas-sql-failover-cluster/cmd.png" alt-text="cmd" border="false":::

## <a name="references"></a>Referencias

Para obtener más información sobre la optimización de los valores de configuración de red de clúster de Windows, consulte [Tuning failover Cluster Network thresholds](https://techcommunity.microsoft.com/t5/failover-clustering/tuning-failover-cluster-network-thresholds/ba-p/371834).

Para obtener información sobre el uso de cluster.exe para optimizar la configuración de red de clústeres de Windows, consulte [How to Configure Cluster Networks for a failover Cluster](/previous-versions/office/exchange-server-2007/bb690953(v=exchg.80)?redirectedfrom=MSDN).
