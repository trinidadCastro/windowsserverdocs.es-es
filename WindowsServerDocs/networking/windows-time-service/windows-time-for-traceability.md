---
ms.assetid: ''
title: Hora de Windows para la rastreabilidad
description: Las leyes en muchos sectores requieren sistemas para ser conforme a UTC.  Esto significa que puede recibir atestación desplazamiento de un sistema con respecto a UTC.
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: e25217feba45516cd0e9a3aa2bf1a2581d2087f5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838046"
---
# <a name="windows-time-for-traceability"></a>Hora de Windows para la rastreabilidad
>Se aplica a: Windows Server 2016 versión 1709 o posterior y Windows 10 versión 1703 o posterior


Las leyes en muchos sectores requieren sistemas para ser conforme a UTC.  Esto significa que puede recibir atestación desplazamiento de un sistema con respecto a UTC.  Para habilitar escenarios de cumplimiento normativo, Windows 10 (versión 1703 o posterior) y Windows Server 2016 (versión 1709 o versiones posterior) proporciona nuevos registros de eventos para proporcionar una imagen desde la perspectiva del sistema operativo para formar una comprensión de las acciones realizadas en el reloj del sistema.  Estos registros de eventos se generan continuamente para el servicio de hora de Windows y puede examinar o archivados para su análisis posterior.

Estos nuevos eventos permiten las siguientes preguntas que deben responderse:

* Se modificó el reloj del sistema
* Se ha modificado la frecuencia del reloj
* Se ha modificado la configuración del servicio hora de Windows

## <a name="availability"></a>Disponibilidad

Estas mejoras se incluyen en Windows 10 versión 1703 o posterior y Windows Server 2016 versión 1709 o versiones posterior.

## <a name="configuration"></a>Configuración

Para obtener esta característica es necesaria ninguna configuración.  Estos registros de eventos están habilitados de forma predeterminada y puede encontrarse en el evento Visor en el **aplicaciones y servicios Log\Microsoft\Windows\Time-Service\Operational** canal.


## <a name="list-of-event-logs"></a>Lista de los registros de eventos

La siguiente sección describen los eventos registrados para su uso en escenarios de seguimiento.

<!-- use tabs like the group policies -->
# <a name="257tab257"></a>[257](#tab/257)
Este evento se registra cuando el servicio de hora de Windows (W32Time) se inicia y se registra información acerca de la hora actual, contador actual, configuración de tiempo de ejecución, proveedores de hora y la velocidad de reloj actual.

|||
|---|---|
|Descripción del evento |Inicio del servicio |
|Detalles |Se produce durante el inicio de W32time |
|Datos registrados |<ul><li>Hora actual en UTC</li><li>Recuento de pasos actual</li><li>Configuración de W32Time</li><li>Configuración del proveedor de hora</li><li>Velocidad de reloj</li></ul> |
|Mecanismo de limitación  |Ninguno. Este evento se desencadena cada vez que se inicia el servicio. |

**Ejemplo:**
```
W32time service has started at 2018-02-27T04:25:17.156Z (UTC), System Tick Count 3132937.
```

**Comando:**

También se puede consultar esta información mediante los siguientes comandos

*Configuración de W32Time y proveedor de hora*
```
w32tm.exe /query /configuration
```

*Velocidad de reloj*
```
w32tm.exe /query /status /verbose
```


# <a name="258tab258"></a>[258](#tab/258)
Este evento se registra cuando el servicio de hora de Windows (W32Time) se ha detenido y registra información acerca de la hora y el contador actual.

|||
|---|---|
|Descripción del evento |Detención del servicio |
|Detalles |Se produce al apagar el equipo W32time |
|Datos registrados |<ul><li>Hora actual en UTC</li><li>Recuento de pasos actual</li></ul> |
|Mecanismo de limitación  |Ninguno. Este evento se desencadena cada vez que el servicio se detiene. |

**Texto de ejemplo:**
`W32time service is stopping at 2018-03-01T05:42:13.944Z (UTC), System Tick Count 6370250.`

# <a name="259tab259"></a>[259](#tab/259)
Este evento registra periódicamente su lista actual de orígenes de hora y su origen de tiempo seleccionado.  Además, registra el contador actual.  Este evento desencadena cada vez que cambia de un origen de hora.  Otros eventos enumerados más adelante en este documento proporcionan esta funcionalidad.

|||
|---|---|
|Descripción del evento |Estado periódico del proveedor de cliente NTP |
|Detalles |Lista de orígenes de hora (s) usado por el cliente de NTP |
|Datos registrados |<ul><li>Orígenes del tiempo disponible</li><li>El servidor de hora elegido referencia en el momento de inicio de sesión</li><li>Recuento de pasos actual</li></ul>  |
|Mecanismo de limitación  |Registra una vez cada 8 horas. |

**Texto de ejemplo:** Estado periódico de proveedor de cliente de NTP:

Cliente de NTP está recibiendo datos de tiempo de los siguientes servidores NTP:

Server1.fabrikam.com, 0 x 8 (ntp.m|0x8|[::]:123 -> [IPAddress]:123)server2.fabrikam.com,0x8 (ntp.m|0x8|[::]:123 -> [IPAddress]: 123);  y el servidor de hora de referencia elegido es Server1.fabrikam.com,0x8 (ntp.m|0x8|[::]:123 -> [IPAddress]: 123) (RefID:0x08d6648e63). Recuento de pasos del sistema 13187937

**Comando** también se puede consultar esta información mediante los siguientes comandos

*Identificar los elementos del mismo nivel*
`w32tm.exe /query /peers`

# <a name="260tab260"></a>[260](#tab/260)

|||
|---|---|
|Descripción del evento |Estado y la configuración del servicio de hora |
|Detalles |W32Time periódicamente los registros de su configuración y estado. Esto es el equivalente de llamar al método:<br><br>`w32tm /query /configuration /verbose`<br>O bien,<br>`w32tm /query /status /verbose` |
|Mecanismo de limitación  |Registra una vez cada 8 horas. |

# <a name="261tab261"></a>[261](#tab/261)
Esto registra cada instancia cuando se modifica la hora del sistema mediante la API de SetSystemTime.

|||
|---|---|
|Descripción del evento |Se establece la hora del sistema |
|Mecanismo de limitación  |Ninguno.<br><br>Esto debe ocurrir con poca frecuencia en sistemas con la sincronización de tiempo razonable, y queremos registrará cada vez que aparezca. Se omiten las configuración TimeJumpAuditOffset al registrar este evento ya que esa configuración estaba pensada para limitar los eventos en el registro de eventos del sistema de Windows. |

# <a name="262tab262"></a>[262](#tab/262)

|||
|---|---|
|Descripción del evento |Ajustadas la frecuencia de reloj del sistema |
|Detalles |Frecuencia del reloj del sistema se modifica constantemente W32time cuando el reloj está en estrecha sincronía. Queremos captar "razonablemente significativos" ajustes realizados a la frecuencia del reloj sin sobrepasar el registro de eventos. |
|Mecanismo de limitación  |Todos los ajustes siguientes TimeAdjustmentAuditThreshold de reloj (min = 128 parte por millón, predeterminado = 800 parte por millón) no se registran.<br><br>Cambio 2 PPM en frecuencia de reloj con granularidad actual produce 120 cambio µsec/s con una precisión de reloj de.<br><br>En un sistema sincronizado, la mayoría de los ajustes están por debajo de este nivel. Si desea más preciso de seguimiento, se puede ajustar esta configuración hacia abajo o puede usar PerfCounters, o ambos. |

# <a name="263tab263"></a>[263](#tab/263)

|||
|---|---|
|Descripción del evento |Cambio en la configuración del servicio de hora o una lista de proveedores de hora cargado. |
|Detalles |Volver a leer la configuración de W32time puede provocar determinados valores críticos como modificadas en memoria, lo que puede afectar a la precisión general de la sincronización de hora.<br><br>W32Time registra cada repetición cuando vuelve a leer la configuración que proporciona el posible impacto en la sincronización de hora. |
|Mecanismo de limitación  |Ninguno.<br><br>Este evento sólo se produce cuando un administrador o actualización de directiva de grupo cambia los proveedores de hora y, a continuación, desencadena W32time. Queremos registrar cada instancia de cambio de configuración. |


# <a name="264tab264"></a>[264](#tab/264)

|||
|---|---|
|Descripción del evento |Cambio en los orígenes de hora usados por el cliente de NTP |
|Detalles |NTP cliente registra un evento con el estado actual de los servidores de hora/pares cuando cambia el estado un vez server o del mismo nivel (**pendientes-> sincronización**, **-> sincronización inaccesible**, o a otras transiciones) |
|Mecanismo de limitación  |Frecuencia máxima: una sola vez cada 5 minutos para proteger el registro de problemas transitorios y la implementación de proveedor incorrecta. |

# <a name="265tab265"></a>[265](#tab/265)

|||
|---|---|
|Descripción del evento |Tiempo de origen o los satélites número cambios de servicio |
|Detalles |Origen de hora W32Time y el número de los satélites son factores importantes rastreabilidad de tiempo y se deben registrar los cambios realizados en ellos. Si W32time no tiene ningún origen de tiempo y no ha configurado como un origen de hora confiable, a continuación, dejará de mostrarse como un servidor horario y, por diseño responda a solicitudes con algunos parámetros no válidos. Este evento es fundamental realizar un seguimiento de los cambios de estado en una topología NTP. |
|Mecanismo de limitación  |Ninguno. |


# <a name="266tab266"></a>[266](#tab/266)

|||
|---|---|
|Descripción del evento |Se solicita una resincronización tiempo |
|Detalles |Esta operación se desencadena:<ul><li>Cuando se producen cambios en la red</li><li>Devuelve el sistema de suspensión o hibernación conectado</li><li>Cuando se no se han sincronizado durante mucho tiempo</li><li>Administrador emite el comando de resincronización</li></ul>Esta operación provoca una pérdida inmediata de precisión de la sincronización de hora específica porque provoca cliente NTP borrar los filtros. |
|Mecanismo de limitación  |Frecuencia máxima: una vez cada 5 minutos.<br><br>Es posible que una tarjeta de red en mal estado (o una secuencia de comandos deficiente) puede desencadenar esta operación varias veces y dar lugar a registros de introducción desbordados. Por lo tanto, la necesidad de limitar este evento.<br><br>Tenga en cuenta que sincronización de hora precisas tarda mucho más de 5 minutos para lograr y la limitación no pierde la información sobre el evento original que dieron lugar a pérdida de precisión de tiempo.  |

---
