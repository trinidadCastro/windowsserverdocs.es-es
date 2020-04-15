---
title: Hora de Windows para rastreabilidad
description: Las normas de muchos sectores exigen que los sistemas puedan rastrearse según la hora UTC.  Esto significa que se pueda dar fe de la diferencia horaria de un sistema con respecto a UTC.
author: dcuomo
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 30952c7a15109ccdd8bcbb09d7c8dda44f716d5d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859818"
---
# <a name="windows-time-for-traceability"></a>Hora de Windows para rastreabilidad
>Se aplica a: Windows Server 2016, versión 1709 o posterior, y Windows 10, versión 1703 o posterior


Las normas de muchos sectores exigen que los sistemas puedan rastrearse según la hora UTC.  Esto significa que se pueda dar fe de la diferencia horaria de un sistema con respecto a UTC.  Para habilitar los escenarios de cumplimiento normativo, Windows 10 (versión 1703 o posterior) y Windows Server 2016 (versión 1709 o posterior) proporcionan nuevos registros de eventos para dar una imagen desde la perspectiva del sistema operativo con el objetivo de comprender las medidas adoptadas en el reloj del sistema.  Estos registros de eventos se generan continuamente para el servicio de hora de Windows y se pueden examinar o archivar para su posterior análisis.

Estos nuevos eventos permiten responder a las siguientes preguntas:

* ¿Se modificó el reloj del sistema?
* ¿Se modificó la frecuencia del reloj?
* ¿Se modificó la configuración del servicio de hora de Windows?

## <a name="availability"></a>Disponibilidad

Estas mejoras se incluyen en Windows 10, versión 1703 o posterior, y en Windows Server 2016, versión 1709 o posterior.

## <a name="configuration"></a>Configuración

No se necesita ninguna configuración para usar esta característica.  Estos registros de eventos están habilitados de forma predeterminada y se pueden encontrar en el visor de eventos, en el canal **Applications and Services Log\Microsoft\Windows\Time-Service\Operational**.


## <a name="list-of-event-logs"></a>Lista de registros de eventos

En la siguiente sección se describen los eventos registrados para su uso en escenarios de rastreabilidad.

# <a name="257"></a>[257](#tab/257)
Este evento se registra cuando se inicia el servicio de hora de Windows (W32Time) y registra información sobre la hora actual, el recuento de tics actual, la configuración de tiempo de ejecución, los proveedores de hora y la frecuencia actual del reloj.

|||
|---|---|
|Descripción del evento |Inicio del servicio |
|Detalles |Tiene lugar en el inicio de W32time |
|Datos registrados |<ul><li>Hora actual en UTC</li><li>Recuento de tics actual</li><li>Configuración de W32Time</li><li>Configuración del proveedor de hora</li><li>Frecuencia del reloj</li></ul> |
|Mecanismo de limitación  |Ninguna. Este evento se activa cada vez que se inicia el servicio. |

**Ejemplo:**
```
W32time service has started at 2018-02-27T04:25:17.156Z (UTC), System Tick Count 3132937.
```

**Comando:**

También se puede consultar esta información mediante los siguientes comandos.

*Configuración del proveedor de hora y W32Time*
```
w32tm.exe /query /configuration
```

*Frecuencia del reloj*
```
w32tm.exe /query /status /verbose
```


# <a name="258"></a>[258](#tab/258)
Este evento se registra cuando se detiene el servicio de hora de Windows (W32Time) y registra información sobre la hora actual y el recuento de tics.

|||
|---|---|
|Descripción del evento |Detención del servicio |
|Detalles |Tiene lugar en el cierre de W32time |
|Datos registrados |<ul><li>Hora actual en UTC</li><li>Recuento de tics actual</li></ul> |
|Mecanismo de limitación  |Ninguna. Este evento se activa cada vez que se detiene el servicio. |

**Texto de ejemplo:** 
`W32time service is stopping at 2018-03-01T05:42:13.944Z (UTC), System Tick Count 6370250.`

# <a name="259"></a>[259](#tab/259)
Este evento registra periódicamente su lista actual de orígenes de la hora y el origen de la hora elegido.  Además, registra el recuento actual de tics.  Este evento no se activa cada vez que cambia el origen de la hora.  Otros eventos que se enumeran más adelante en este documento proporcionan esta funcionalidad.

|||
|---|---|
|Descripción del evento |Estado periódico del proveedor del cliente NTP |
|Detalles |Lista de orígenes de hora usados por el cliente NTP |
|Datos registrados |<ul><li>Orígenes de la hora disponibles</li><li>El servidor de hora de referencia elegido en el momento del registro</li><li>Recuento de tics actual</li></ul>  |
|Mecanismo de limitación  |Se registra una vez cada 8 horas. |

**Texto de ejemplo:** Estado periódico del proveedor del cliente NTP:

El cliente NTP está recibiendo datos de hora de los siguientes servidores NTP:

server1.fabrikam.com,0x8 (ntp.m|0x8|[::]:123->[direcciónIP]:123)server2.fabrikam.com,0x8 (ntp.m|0x8|[::]:123->[IPAddress]:123); y el servidor de hora de referencia elegido es Server1.fabrikam.com,0x8 (ntp.m|0x8|[::]:123->[direcciónIP]:123) (RefID:0x08d6648e63). Recuento de tics del sistema 13187937

**Comando** También se puede consultar esta información mediante los siguientes comandos.

*Identificar homólogos*
`w32tm.exe /query /peers`

# <a name="260"></a>[260](#tab/260)

|||
|---|---|
|Descripción del evento |Configuración y estado del servicio de hora |
|Detalles |W32time registra periódicamente su configuración y su estado. Esto es el equivalente a llamar:<br><br>`w32tm /query /configuration /verbose`<br>O<br>`w32tm /query /status /verbose` |
|Mecanismo de limitación  |Se registra una vez cada 8 horas. |

# <a name="261"></a>[261](#tab/261)
Registra cada instancia cuando se modifica la hora del sistema mediante la API SetSystemTime.

|||
|---|---|
|Descripción del evento |La hora del sistema está establecida |
|Mecanismo de limitación  |Ninguna.<br><br>Esto debería ocurrir con poca frecuencia en sistemas con una sincronización razonable de la hora, y queremos que se registre cada vez que tenga lugar. Omitimos el valor de TimeJumpAuditOffset al registrar este evento, ya que la configuración se diseñó para limitar los eventos del registro de eventos del sistema Windows. |

# <a name="262"></a>[262](#tab/262)

|||
|---|---|
|Descripción del evento |Frecuencia ajustada del reloj del sistema |
|Detalles |W32time modifica constantemente la frecuencia del reloj del sistema cuando el reloj está en estrecha sincronización. Queremos capturar los ajustes "razonablemente significativos" que se realizaron en la frecuencia del reloj sin sobrecargar el registro de eventos. |
|Mecanismo de limitación  |No se registra ninguno de los ajustes de reloj por debajo de TimeAdjustmentAuditThreshold (mín. = 128 partes por millón, valor predeterminado = 800 partes por millón).<br><br>Un cambio de 2 ppm en la frecuencia del reloj con la granularidad actual produce un cambio de 120 μs/s en la precisión del reloj.<br><br>En un sistema sincronizado, la mayoría de los ajustes está por debajo de este nivel. Si quieres un seguimiento más preciso, este valor se puede ajustar a la baja o puedes usar PerfCounters, o bien puedes hacer ambas cosas. |

# <a name="263"></a>[263](#tab/263)

|||
|---|---|
|Descripción del evento |Cambio en la configuración del servicio de hora o en la lista de proveedores de hora cargados. |
|Detalles |Volver a leer la configuración de W32time puede hacer que ciertas configuraciones críticas se modifiquen en memoria, lo que puede afectar a la precisión general de la sincronización de la hora.<br><br>W32time registra cada incidencia cuando se vuelve a leer su configuración, lo que genera el impacto potencial en la sincronización de la hora. |
|Mecanismo de limitación  |Ninguna.<br><br>Este evento solo se produce cuando un administrador o actualización de GP cambian los proveedores de hora y, a continuación, inician W32time. Queremos registrar cada instancia de cambio de configuración. |


# <a name="264"></a>[264](#tab/264)

|||
|---|---|
|Descripción del evento |Cambio en los orígenes de hora usados por el cliente NTP |
|Detalles |El cliente NTP registra un evento con el estado actual de los servidores de hora u homólogos cuando cambia el estado del servidor de hora o del homólogo (**Pendiente -> Sincronización**, **Sincronización-> No accesible** u otras transiciones) |
|Mecanismo de limitación  |Frecuencia máxima: solo una vez cada 5 minutos para proteger el registro frente a problemas transitorios y a una implementación de proveedor incorrecta. |

# <a name="265"></a>[265](#tab/265)

|||
|---|---|
|Descripción del evento |Cambios en el origen o en el número de estrato del servicio de hora |
|Detalles |El origen de hora y el número de estrato de W32time son factores importantes en la rastreabilidad de la hora y se deben registrar los cambios en estos. Si W32time no tiene ningún origen de la hora y no lo has configurado como origen de la hora de confianza, dejará de anunciarse como servidor de hora y, por diseño, responderá a las solicitudes con algunos parámetros no válidos. Este evento es fundamental para el seguimiento de los cambios de estado en una topología NTP. |
|Mecanismo de limitación  |Ninguna. |


# <a name="266"></a>[266](#tab/266)

|||
|---|---|
|Descripción del evento |Se solicita la resincronización de la hora |
|Detalles |Esta operación se activa cuando:<ul><li>Se producen cambios en la red</li><li>El sistema vuelve del modo de espera o hibernación conectado</li><li>No hace mucho tiempo que no se sincroniza</li><li>El administrador emite el comando resync</li></ul>Esta operación produce una pérdida inmediata de la precisión detallada de la sincronización de la hora porque hace que el cliente NTP borre sus filtros. |
|Mecanismo de limitación  |Frecuencia máxima: una vez cada 5 minutos.<br><br>Es posible que una tarjeta de red defectuosa (o un mal script) pueda desencadenar esta operación repetidamente y provocar la sobrecarga de los registros. Por lo tanto, es necesario limitar este evento.<br><br>Ten en cuenta que la sincronización precisa de la hora tarda mucho más de 5 minutos, y la limitación no pierde información sobre el evento original que provocó la pérdida de precisión temporal.  |

---
