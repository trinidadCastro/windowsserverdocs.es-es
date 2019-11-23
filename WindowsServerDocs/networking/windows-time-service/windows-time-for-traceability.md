---
ms.assetid: ''
title: Hora de Windows para rastreabilidad
description: Las regulaciones en muchos sectores requieren que se pueda realizar un seguimiento de los sistemas a UTC.  Esto significa que se puede atestiguar el desplazamiento de un sistema con respecto a la hora UTC.
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 307739042426088fa92c50e6ea4dc5d2a744f15a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405205"
---
# <a name="windows-time-for-traceability"></a>Hora de Windows para rastreabilidad
>Se aplica a: Windows Server 2016 versión 1709 o posterior y Windows 10 versión 1703 o posterior


Las regulaciones en muchos sectores requieren que se pueda realizar un seguimiento de los sistemas a UTC.  Esto significa que se puede atestiguar el desplazamiento de un sistema con respecto a la hora UTC.  Para habilitar escenarios de cumplimiento normativo, Windows 10 (versión 1703 o posterior) y Windows Server 2016 (versión 1709 o posterior) proporciona nuevos registros de eventos para proporcionar una imagen desde la perspectiva del sistema operativo para crear una descripción de las acciones realizadas en el reloj del sistema.  Estos registros de eventos se generan continuamente para el servicio de hora de Windows y se pueden examinar o archivar para su posterior análisis.

Estos nuevos eventos permiten responder a las siguientes preguntas:

* Se modificó el reloj del sistema
* Se modificó la frecuencia del reloj
* Se modificó la configuración del servicio de hora de Windows

## <a name="availability"></a>Disponibilidad

Estas mejoras se incluyen en la versión 1703 o posterior de Windows 10, y en la versión 1709 o posterior de Windows Server 2016.

## <a name="configuration"></a>Configuración

No es necesaria ninguna configuración para realizar esta característica.  Estos registros de eventos están habilitados de forma predeterminada y se pueden encontrar en el visor de eventos en el canal de **Log\Microsoft\Windows\Time-Service\Operational de aplicaciones y servicios** .


## <a name="list-of-event-logs"></a>Lista de registros de eventos

En la siguiente sección se describen los eventos registrados para su uso en escenarios de rastreabilidad.

# <a name="257tab257"></a>[257](#tab/257)
Este evento se registra cuando se inicia el servicio de hora de Windows (W32Time) y registra información sobre la hora actual, el recuento de pasos actual, la configuración de tiempo de ejecución, los proveedores de hora y la tasa de reloj actual.

|||
|---|---|
|Descripción del evento |Inicio del servicio |
|Detalles |Tiene lugar en el inicio de W32time |
|Datos registrados |<ul><li>Hora actual en UTC</li><li>Recuento de pasos actual</li><li>Configuración de W32Time</li><li>Configuración del proveedor de hora</li><li>Velocidad de reloj</li></ul> |
|Mecanismo de limitación  |Ninguno. Este evento se desencadena cada vez que se inicia el servicio. |

**Ejemplo:**
```
W32time service has started at 2018-02-27T04:25:17.156Z (UTC), System Tick Count 3132937.
```

**Comando:**

Esta información también se puede consultar mediante los siguientes comandos.

*W32Time y configuración del proveedor de hora*
```
w32tm.exe /query /configuration
```

*Velocidad de reloj*
```
w32tm.exe /query /status /verbose
```


# <a name="258tab258"></a>[258](#tab/258)
Este evento se registra cuando se detiene el servicio de hora de Windows (W32Time) y registra información sobre la hora actual y el recuento de pasos.

|||
|---|---|
|Descripción del evento |Detención del servicio |
|Detalles |Tiene lugar en el cierre de W32time |
|Datos registrados |<ul><li>Hora actual en UTC</li><li>Recuento de pasos actual</li></ul> |
|Mecanismo de limitación  |Ninguno. Este evento se desencadena cada vez que se detiene el servicio. |

**Texto de ejemplo:** 
`W32time service is stopping at 2018-03-01T05:42:13.944Z (UTC), System Tick Count 6370250.`

# <a name="259tab259"></a>[259](#tab/259)
Este evento registra periódicamente su lista actual de orígenes de hora y su origen de hora elegido.  Además, registra el recuento de pasos actual.  Este evento no se desencadena cada vez que cambia el origen de hora.  Otros eventos que se enumeran más adelante en este documento proporcionan esta funcionalidad.

|||
|---|---|
|Descripción del evento |Estado periódico del proveedor de cliente NTP |
|Detalles |Lista de orígenes de hora utilizados por el cliente NTP |
|Datos registrados |<ul><li>Orígenes de hora disponibles</li><li>El servidor de hora de referencia elegido en el momento del registro</li><li>Recuento de pasos actual</li></ul>  |
|Mecanismo de limitación  |Se registra una vez cada 8 horas. |

**Texto de ejemplo:** Estado periódico del proveedor de cliente NTP:

El cliente NTP está recibiendo datos de hora de los siguientes servidores NTP:

Server1. fabrikam. com, 0x8 (NTP. m | 0x8 | [::]: 123-> [IPAddress]: 123) servidor2. fabrikam. com, 0x8 (NTP. m | 0x8 | [::]: 123-> [IPAddress]: 123);  y el servidor de hora de referencia elegido es server1. fabrikam. com, 0x8 (NTP. m | 0x8 | [::]: 123-> [IPAddress]: 123) (RefID: 0x08d6648e63). Recuento de TICs del sistema 13187937

**Comando** Esta información también se puede consultar mediante los siguientes comandos.

*Identificar los elementos del mismo nivel*
`w32tm.exe /query /peers`

# <a name="260tab260"></a>[260](#tab/260)

|||
|---|---|
|Descripción del evento |Estado y configuración del servicio de hora |
|Detalles |W32time registra periódicamente su configuración y su estado. Este es el equivalente a llamar a:<br><br>`w32tm /query /configuration /verbose`<br>O bien,<br>`w32tm /query /status /verbose` |
|Mecanismo de limitación  |Se registra una vez cada 8 horas. |

# <a name="261tab261"></a>[261](#tab/261)
Esto registra cada instancia cuando se modifica la hora del sistema mediante la API de SetSystemTime.

|||
|---|---|
|Descripción del evento |La hora del sistema está establecida |
|Mecanismo de limitación  |Ninguno.<br><br>Esto debería ocurrir con poca frecuencia en sistemas con una sincronización de hora razonable y queremos registrarlo cada vez que se produzca. Se omite el valor de TimeJumpAuditOffset al registrar este evento, ya que la configuración se diseñó para limitar los eventos del registro de eventos del sistema de Windows. |

# <a name="262tab262"></a>[262](#tab/262)

|||
|---|---|
|Descripción del evento |Frecuencia de reloj del sistema ajustada |
|Detalles |W32time modifica constantemente la frecuencia del reloj del sistema cuando el reloj está en la sincronización de cierre. Queremos capturar los ajustes "razonablemente significativos" que se realizaron en la frecuencia del reloj sin sobreejecutar el registro de eventos. |
|Mecanismo de limitación  |No se registran todos los ajustes de reloj por debajo de TimeAdjustmentAuditThreshold (min = 128 parte por millón, valor predeterminado = 800 parte por millón).<br><br>2 el cambio de la frecuencia del reloj con la granularidad actual produce un cambio de 120 μsec/s en la precisión del reloj.<br><br>En un sistema sincronizado, la mayoría de los ajustes están por debajo de este nivel. Si desea realizar un seguimiento más preciso, este valor se puede ajustar o puede usar PerfCounters, o bien puede hacer ambas cosas. |

# <a name="263tab263"></a>[263](#tab/263)

|||
|---|---|
|Descripción del evento |Cambiar en la configuración del servicio de hora o en la lista de proveedores de hora cargados. |
|Detalles |La relectura de la configuración de W32time puede hacer que ciertas configuraciones críticas se modifiquen en memoria, lo que puede afectar a la precisión general de la sincronización de la hora.<br><br>W32time registra cada repetición cuando se vuelve a leer su configuración, lo que da como resultado el impacto potencial en la sincronización de la hora. |
|Mecanismo de limitación  |Ninguno.<br><br>Este evento solo se produce cuando una actualización de administrador o GP cambia los proveedores de hora y, a continuación, inicia W32time. Queremos registrar cada instancia de cambio de configuración. |


# <a name="264tab264"></a>[264](#tab/264)

|||
|---|---|
|Descripción del evento |Cambio en los orígenes de tiempo usados por el cliente NTP |
|Detalles |El cliente NTP registra un evento con el estado actual de los servidores de hora o del mismo nivel cuando cambia el estado del servidor o del mismo nivel (**pendiente > sincronización**, sincronización **> inaccesible**u otras transiciones) |
|Mecanismo de limitación  |Frecuencia máxima: solo una vez cada 5 minutos para proteger el registro de problemas transitorios e implementación de proveedor incorrecta. |

# <a name="265tab265"></a>[265](#tab/265)

|||
|---|---|
|Descripción del evento |Cambios en el origen o el número de estrato del servicio de hora |
|Detalles |El origen de hora de W32time y el número de estrato son factores importantes en el seguimiento de tiempo y se deben registrar los cambios en estos. Si W32time no tiene ningún origen de tiempo y no se ha configurado como un origen de hora confiable, dejará de anunciarse como un servidor horario y, por diseño, responderá a las solicitudes con algunos parámetros no válidos. Este evento es fundamental para realizar el seguimiento de los cambios de estado en una topología NTP. |
|Mecanismo de limitación  |Ninguno. |


# <a name="266tab266"></a>[266](#tab/266)

|||
|---|---|
|Descripción del evento |Se solicita la resincronización de tiempo |
|Detalles |Esta operación se desencadena:<ul><li>Cuando se producen cambios en la red</li><li>El sistema vuelve del modo de espera o hibernación conectado</li><li>Cuando no se ha sincronizado durante mucho tiempo</li><li>El administrador emite el comando Resync</li></ul>Esta operación produce una pérdida inmediata de precisión de la sincronización de tiempo específica porque hace que el cliente NTP borre sus filtros. |
|Mecanismo de limitación  |Frecuencia máxima: una vez cada 5 minutos.<br><br>Es posible que una tarjeta de red defectuosa (o un script pobre) pueda desencadenar esta operación repetidamente y provocar que los registros se sobrecarguen. Por lo tanto, es necesario limitar este evento.<br><br>Tenga en cuenta que la sincronización de hora precisa tarda mucho más de 5 minutos en lograrse, y la limitación no pierde información sobre el evento original que resultó en la pérdida de precisión temporal.  |

---
