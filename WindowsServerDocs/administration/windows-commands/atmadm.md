---
title: atmadm
description: Tema de los comandos de Windows para **atmadm** -supervisa las conexiones y las direcciones que están registradas por del atM llamar a Manager en una red de transferencia asincrónico (atM) de modo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 37156c2e-c4d4-4fd8-a03d-245fb60bf996
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a8a8883bad9aa2abdc90d5db5702ef42f46c8a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831776"
---
# <a name="atmadm"></a>atmadm

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supervisa las conexiones y las direcciones que están registradas por del atM llamar a Manager en una red de transferencia asincrónico (atM) de modo. Puede usar **atmadm** para mostrar las estadísticas para las llamadas entrantes y salientes de los adaptadores atM. Se utiliza sin parámetros, **atmadm** muestra las estadísticas para supervisar el estado de las conexiones activas de atM. 

## <a name="syntax"></a>Sintaxis

```
atmadm [/c][/a][/s]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|/c|Muestra información de llamadas para todas las conexiones actuales para el adaptador de red atM instalado en este equipo.|
|/a|Muestra el acceso al servicio de red de atM registrados puntos de dirección (NSAP) para cada adaptador instalado en este equipo.|
|/s|Muestra las estadísticas para supervisar el estado de las conexiones activas de atM.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

- El **atmadm /c** comando produce un resultado similar al siguiente:

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
    
    En la tabla siguiente contiene descripciones de cada elemento de la **atmadm /c** salida de ejemplo.
    
    |tipo de datos|Presentación en pantalla|Descripción|
    |--------|---------|--------|
    |Información de conexión|Entrada/salida|dirección de la llamada.  **En** es el adaptador de red de atM desde otro dispositivo.  **Out** es desde el adaptador de red atM a otro dispositivo.|
    ||PMP|Llamada de punto a punto.|
    ||P-P|Llamada de punto a punto.|
    ||SVC|La conexión es con un circuito virtual se ha cambiado.|
    ||PVC|La conexión es en un circuito virtual permanente.|
    |Información VPI/VCI|VPI/VCI|Ruta de acceso virtual y el canal virtual de la llamada entrante o saliente.|
    |los parámetros de dirección/multimedia remoto|47000580FFE1000000F21A2E180000C110081500|Dirección NSAP de la llamada a **(In)** o llamado **(Out)** dispositivo atM.|
    ||**Tx**|El **Tx** parámetro incluye los tres elementos siguientes:<br /><br />-Default o especifica el tipo de velocidad de bits (UBR, CBR, VBR o ABR)<br />-Default o especifica la velocidad de línea<br />-Tamaño de unidad (SDU) de datos de servicio especificado|
    ||**Rx**|El **Rx** parámetro incluye los tres elementos siguientes:<br /><br />-Default o especifica el tipo de velocidad de bits (UBR, CBR, VBR o ABR)<br />-Default o especifica la velocidad de línea<br />-Tamaño de SDU especificado|
    
- El **atmadm /a** comando produce un resultado similar al siguiente:

    ```
    Windows atM call Manager Statistics
    atM addresses for Interface : [009] Olicom atM PCI 155 Adapter
    47000580FFE1000000F21A2E180000C110081500
    ```
    
- El **atmadm /s** comando produce un resultado similar al siguiente:

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
    
    En la tabla siguiente contiene descripciones de cada elemento de la **atmadm /s** salida de ejemplo.
    
    |llamada de estadística del administrador|Descripción|
    |-------------|--------|
    |Llamadas activas actualmente|llamadas activas actualmente en el adaptador de atM instalado en este equipo.|
    |Total de llamadas entrantes correctas|llamadas que recibe correctamente desde otros dispositivos en esta red atM.|
    |Llamadas salientes correctas|llamadas realizadas correctamente a otros dispositivos atM en esta red desde este equipo.|
    |Llamadas entrantes fallidas|Llamadas entrantes que no se pudo conectar a este equipo.|
    |Llamadas salientes fallidas|Llamadas salientes que no se pudo conectar a otro dispositivo en la red.|
    |llamadas cerrado de forma remota|llamadas cerradas por un dispositivo remoto en la red.|
    |llama a cerrado localmente|llamadas cerradas por este equipo.|
    |Señalización y conexiones los paquetes enviados|Número de paquetes de interfaz (conexiones) de administración local integrada enviados al conmutador para que este equipo está intentando conectarse.|
    |Señalización y los paquetes de conexiones recibidos|Número de paquetes de conexiones recibidos desde el conmutador de atM.|
    
## <a name="BKMK_Examples"></a>Ejemplos

Para mostrar información de llamadas para todas las conexiones actuales para el adaptador de red atM instalado en este equipo, escriba:

```
atmadm /c
```

Para mostrar la dirección de punto (NSAP) atM registrada acceso de servicio de red para cada adaptador instalado en este equipo, escriba:

```
atmadm /a
```

Para mostrar las estadísticas para supervisar el estado de las conexiones atM activas, escriba:

```
atmadm /s
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
