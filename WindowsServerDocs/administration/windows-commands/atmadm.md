---
title: atmadm
description: Tema de referencia del comando Atmadm, que supervisa las conexiones y direcciones registradas por el administrador de llamadas atM en una red de modo de transferencia asincrónico (atM).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 37156c2e-c4d4-4fd8-a03d-245fb60bf996
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32dad00e5a4d03c905f95c48e112f512a9dbc2e5
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718918"
---
# <a name="atmadm"></a>atmadm

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Supervisa las conexiones y direcciones registradas por el administrador de llamadas atM en una red de modo de transferencia asincrónico (atM). Puede usar **Atmadm** para mostrar estadísticas de llamadas entrantes y salientes en adaptadores atM. Si se usa sin parámetros, **Atmadm** muestra las estadísticas para supervisar el estado de las conexiones atM activas.

## <a name="syntax"></a>Sintaxis

```
atmadm [/c][/a][/s]
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ------- | -------- |
| /C | Muestra información de llamadas para todas las conexiones actuales con el adaptador de red atM instalado en este equipo. |
| /a | Muestra la dirección de punto de acceso de servicio de red atM (NSAP) registrada para cada adaptador instalado en este equipo. |
| /s | Muestra estadísticas para supervisar el estado de las conexiones atM activas. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="remarks"></a>Observaciones

- El comando **Atmadm/c** produce una salida similar a la siguiente:

    ```
    Windows atM call Manager Statistics
    atM Connections on Interface : [009] Olicom atM PCI 155 Adapter
       Connection   VPI/VCI   remote address/
                              Media Parameters (rates in bytes/sec)
       In  PMP SVC    0/193   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 0,Avg 0,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       Out P-P SVC    0/192   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       In  PMP SVC    0/191   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 0,Avg 0,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       Out P-P SVC    0/190   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       In  P-P SVC    0/475   47000580FFE1000000F21A2E180000C110081501
                              Tx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9188
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 9188
       Out PMP SVC    0/194   47000580FFE1000000F21A2E180000C110081501 (0)
                              Tx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9180
                              Rx:UBR,Peak 0,Avg 0,MaxSdu 0
       Out P-P SVC    0/474   4700918100000000613E5BFE010000C110081500
                              Tx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9188
                              Rx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9188
       In  PMP SVC    0/195   47000580FFE1000000F21A2E180000C110081500
                              Tx:UBR,Peak 0,Avg 0,MaxSdu 0
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 9180
    ```

    La tabla siguiente contiene descripciones de cada elemento de la salida de ejemplo de **Atmadm/c** .

    | Tipo de datos | Pantalla | Descripción |
    | -------- | --------- | -------- |
    | Información de conexión | Dentro/fuera | Dirección de la llamada. **En** , es el adaptador de red atM desde otro dispositivo.  **Out** es del adaptador de red atM a otro dispositivo. |
    | PRIMARIO | Llamada de punto a multipunto. |
    | P-P | Llamada punto a punto. |
    | SVC | La conexión se encuentra en un circuito virtual conmutado. |
    | PERMANENTE | La conexión se encuentra en un circuito virtual permanente. |
    | Información de VPI/VCI | VPI/VCI | Ruta de acceso virtual y canal virtual de la llamada entrante o saliente. |
    | Dirección remota/parámetros de medios | 47000580FFE1000000F21A2E180000C110081500 | Dirección NSAP del dispositivo atM que llama **(en)** o **(out)** . |
    | TX | El parámetro **TX** incluye los tres elementos siguientes:<ul><li>Tipo de velocidad de bits predeterminado o especificado (UBR, CBR, VBR o ABR)</li><li>Velocidad de línea predeterminada o especificada</li><li>Tamaño de la unidad de datos de servicio (SDU) especificado.</li></ul> |
    | Rx | El parámetro **RX** incluye los tres elementos siguientes:<ul><li>Tipo de velocidad de bits predeterminado o especificado (UBR, CBR, VBR o ABR)</li><li>Velocidad de línea predeterminada o especificada</li><li>Tamaño de SDU especificado.</li></ul> |

- El comando **Atmadm/a** produce una salida similar a la siguiente:

    ```
    Windows atM call Manager Statistics
    atM addresses for Interface : [009] Olicom atM PCI 155 Adapter
    47000580FFE1000000F21A2E180000C110081500
    ```

- El comando **Atmadm/s** produce una salida similar a la siguiente:

    ```
    Windows atM call Manager Statistics
    atM call Manager statistics for Interface : [009] Olicom atM PCI 155 Adapter
    Current active calls                        = 4
    Total successful Incoming calls             = 1332
    Total successful Outgoing calls             = 1297
    Unsuccessful Incoming calls                 = 1
    Unsuccessful Outgoing calls                 = 1
    calls Closed by remote                      = 1302
    calls Closed Locally                        = 1323
    Signaling and ILMI Packets Sent            = 33655
    Signaling and ILMI Packets Received        = 34989
    ```

    La tabla siguiente contiene descripciones de cada elemento en el resultado del ejemplo **Atmadm/s** .

    | Estadística del administrador de llamadas | Descripción |
    | ------------- | -------- |
    | Llamadas activas actuales | Llama actualmente a activa en el adaptador atM instalado en este equipo. |
    | Total de llamadas entrantes correctas | Llamadas recibidas correctamente desde otros dispositivos en esta red atM. |
    | Total de llamadas salientes correctas | Llamadas completadas correctamente a otros dispositivos atM de esta red desde este equipo. |
    | Llamadas entrantes incorrectas | Llamadas entrantes que no pudieron conectarse a este equipo. |
    | Llamadas salientes incorrectas | Llamadas salientes que no pudieron conectarse a otro dispositivo de la red. |
    | Llamadas cerradas por remoto | Llamadas cerradas por un dispositivo remoto en la red. |
    | Llamadas cerradas localmente | Llamadas cerradas por este equipo. |
    | Paquetes de señalización e ILMI enviados | Número de paquetes de la interfaz de administración local (ILMI) integrados enviados al conmutador al que este equipo está intentando conectarse. |
    | Paquetes de señalización e ILMI recibidos | Número de paquetes de ILMI recibidos del conmutador atM. |

## <a name="examples"></a>Ejemplos

Para mostrar información de llamada de todas las conexiones actuales al adaptador de red atM instalado en este equipo, escriba:

```
atmadm /c
```

Para mostrar la dirección de punto de acceso de servicio de red atM (NSAP) registrada para cada adaptador instalado en este equipo, escriba:

```
atmadm /a
```

Para mostrar las estadísticas para supervisar el estado de las conexiones atM activas, escriba:

```
atmadm /s
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
