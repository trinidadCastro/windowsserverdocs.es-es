---
title: evntcmd
description: Artículo de referencia para el comando evntcmd, que configura la traducción de eventos a capturas, destinos de captura o ambos según la información de un archivo de configuración.
ms.topic: article
ms.assetid: c1aabb74-76e7-4304-95a6-50ad87e92fd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9610608000ce106ec321f721a4181658453d6348
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87890541"
---
# <a name="evntcmd"></a>evntcmd

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Configura la traducción de eventos a capturas, destinos de captura o ambos según la información de un archivo de configuración.

## <a name="syntax"></a>Sintaxis

```
evntcmd [/s <computername>] [/v <verbositylevel>] [/n] <filename>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| modificado`<computername>` | Especifica, por nombre, el equipo en el que desea configurar la traducción de eventos a capturas, destinos de captura o ambos. Si no especifica un equipo, la configuración se produce en el equipo local. |
| /v`<verbositylevel>` | Especifica los tipos de mensajes de estado que aparecen como capturas y se configuran los destinos de captura. Este parámetro debe ser un entero comprendido entre 0 y 10. Si especifica 10, aparecerán todos los tipos de mensajes, incluidos los mensajes de seguimiento y las advertencias sobre si la configuración de la captura fue correcta. Si especifica 0, no aparece ningún mensaje. |
| /n | Especifica que el servicio SNMP no debe reiniciarse si este equipo recibe cambios de configuración de captura. |
| `<filename>` | Especifica, por nombre, el archivo de configuración que contiene información sobre la traducción de eventos a capturas y destinos de captura que desea configurar. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Si desea configurar capturas pero no destinos de captura, puede crear un archivo de configuración válido mediante el traductor de eventos para captura, que es una utilidad gráfica. Si tiene el servicio SNMP instalado, puede iniciar el evento para capturar el traductor escribiendo **evntwin** en el símbolo del sistema. Una vez definidas las capturas que desee, haga clic en **exportar** para crear un archivo adecuado para su uso con **evntcmd**. Puede usar el traductor de eventos para interceptar para crear fácilmente un archivo de configuración y, a continuación, utilizar el archivo de configuración con **evntcmd** en el símbolo del sistema para configurar rápidamente las capturas en varios equipos.

- La sintaxis para configurar una captura es la siguiente:

  ```
  #pragma add <eventlogfile> <eventsource> <eventID> [<count> [<period>]]
  ```

  Donde se cumple el texto siguiente:

    - **#pragma** debe aparecer al principio de cada entrada en el archivo.

    - El parámetro **Add** especifica que desea agregar un evento a la configuración de captura.

    - Los parámetros **eventlogfile**, **EventSource**y **EventID** son obligatorios y, donde **eventlogfile** especifica el archivo en el que se registra el evento, **EventSource** especifica la aplicación que genera el evento y **EventID** especifica el número único que identifica cada evento.

    Para determinar qué valores corresponden a cada evento, inicie el evento para capturar el traductor escribiendo **evntwin** en el símbolo del sistema. Haga clic en **personalizada**y, a continuación, en **Editar**. En **orígenes de eventos**, examine las carpetas hasta que encuentre el evento que desea configurar, haga clic en él y, a continuación, haga clic en **Agregar**. La información sobre el origen del evento, el archivo de registro de eventos y el ID. de evento aparecen en **origen, registro**y **identificador específico**de la captura, respectivamente.

    - El parámetro **Count** es opcional y especifica el número de veces que se debe producir el evento antes de que se envíe un mensaje de captura. Si no usa este parámetro, el mensaje de captura se envía una vez que el evento se produce una vez.

    - El parámetro **period** es opcional, pero requiere que use el parámetro **Count** . El parámetro **period** especifica un período de tiempo (en segundos) durante el cual se debe producir el evento el número de veces especificado con el parámetro **Count** antes de que se envíe un mensaje de captura. Si no usa este parámetro, se envía un mensaje de captura una vez que se produce el evento el número de veces especificado con el parámetro de ***recuento*** , independientemente del tiempo transcurrido entre repeticiones.

- La sintaxis para quitar una captura es la siguiente:

  ```
  #pragma delete <eventlogfile> <eventsource> <eventID>
  ```

  Donde se cumple el texto siguiente:

    - **#pragma** debe aparecer al principio de cada entrada en el archivo.

    - La **eliminación** del parámetro especifica que desea quitar un evento para la configuración de captura.

    - Los parámetros **eventlogfile**, **EventSource**y **EventID** son obligatorios y, donde **eventlogfile** especifica el archivo en el que se registra el evento, **EventSource** especifica la aplicación que genera el evento y **EventID** especifica el número único que identifica cada evento.

    Para determinar qué valores corresponden a cada evento, inicie el evento para capturar el traductor escribiendo **evntwin** en el símbolo del sistema. Haga clic en **personalizada**y, a continuación, en **Editar**. En **orígenes de eventos**, examine las carpetas hasta que encuentre el evento que desea configurar, haga clic en él y, a continuación, haga clic en **Agregar**. La información sobre el origen del evento, el archivo de registro de eventos y el ID. de evento aparecen en **origen, registro**y **identificador específico**de la captura, respectivamente.

- La sintaxis para configurar un destino de captura es la siguiente:

  ```
  #pragma add_TRAP_DEST <communityname> <hostID>
  ```

  Donde se cumple el texto siguiente:

    - **#pragma** debe aparecer al principio de cada entrada en el archivo.

    - El parámetro **add_TRAP_DEST** especifica que desea que los mensajes de captura se envíen a un host especificado dentro de una comunidad.

    - El parámetro **communityname** especifica, por nombre, la comunidad en la que se envían los mensajes de captura.

    - El parámetro **hostID** especifica, por nombre o dirección IP, el host al que desea que se envíen los mensajes de captura.

- La sintaxis para quitar un destino de captura es la siguiente:

  ```
  #pragma delete_TRAP_DEST <communityname> <hostID>
  ```

  Donde se cumple el texto siguiente:

    - **#pragma** debe aparecer al principio de cada entrada en el archivo.

    - El parámetro **delete_TRAP_DEST** especifica que no desea que los mensajes de captura se envíen a un host especificado dentro de una comunidad.

    - El parámetro **communityname** especifica, por nombre, la comunidad a la que no se deben enviar mensajes de captura.

    - El parámetro **hostID** especifica, por nombre o dirección IP, el host al que no quiere que se envíen los mensajes de captura.

### <a name="examples"></a>Ejemplos

En los siguientes ejemplos se muestran entradas en el archivo de configuración para el comando **evntcmd** . No se han diseñado para que se escriban en el símbolo del sistema.

Para enviar un mensaje de captura si se reinicia el servicio registro de eventos, escriba:

```
#pragma add System Eventlog 2147489653
```

Para enviar un mensaje de captura si el servicio registro de eventos se reinicia dos veces en tres minutos, escriba:

```
#pragma add System Eventlog 2147489653 2 180
```

Para detener el envío de un mensaje de captura cada vez que se reinicie el servicio registro de eventos, escriba:

```
#pragma delete System Eventlog 2147489653
```

Para enviar mensajes de captura dentro de la comunidad denominada *Public* al host con la dirección IP *192.168.100.100*, escriba:

```
#pragma add_TRAP_DEST public 192.168.100.100
```

Para enviar mensajes de captura dentro de la comunidad denominada *privado* al host denominado *Host1*, escriba:

```
#pragma add_TRAP_DEST private Host1
```

Para dejar de enviar mensajes de captura dentro de la comunidad denominada *privado* al mismo equipo en el que está configurando los destinos de captura, escriba:

```
#pragma delete_TRAP_DEST private localhost
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
