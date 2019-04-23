---
title: Utilice los contadores de rendimiento para diagnosticar problemas de la capacidad de respuesta de aplicación en los Hosts de sesión de escritorio remoto
description: ¿Se ejecuta demasiado lento su aplicación en RDS? Obtenga información sobre los contadores de rendimiento que puede utilizar para diagnosticar problemas de rendimiento de aplicaciones en RDSH
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 09/19/2018
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 241b2b776a68cf5aec68a4d331201a07f0e5ea53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844656"
---
# <a name="use-performance-counters-to-diagnose-app-performance-problems-on-remote-desktop-session-hosts"></a>Utilice los contadores de rendimiento para diagnosticar problemas de rendimiento de aplicaciones en Hosts de sesión de escritorio remoto

Uno de los problemas más difíciles de diagnosticar es rendimiento deficiente de las aplicaciones, las aplicaciones se ejecutan lentamente o no responde. Tradicionalmente, inicie el diagnóstico mediante la recopilación de CPU, memoria, entrada/salida de disco y otras métricas y, a continuación, usar herramientas como Windows Performance Analyzer para intentar averiguar qué está causando el problema. Por desgracia en la mayoría de los casos estos datos no ayuda a identificar la causa raíz porque los contadores de consumo de recursos tienen variaciones frecuentes y grandes. Esto resulta difícil de leer los datos y correlacionarlos con el problema notificado. Para ayudarle a más resolución rápidamente los problemas de rendimiento de aplicaciones, hemos agregado algunos nuevos contadores de rendimiento (disponible [descargar](#download-windows-server-insider-software) a través de la [Windows Insider programa](https://insider.windows.com)) flujos de entrada de ese usuario de la medida.

El contador de retraso de entrada del usuario puede ayudarle a identificar rápidamente la causa raíz para RDP experiencias de usuario final incorrectos. Este contador mide cuánto tiempo cualquier entrada (por ejemplo, el uso del mouse o teclado) del usuario permanece en la cola antes de recoge un proceso y el contador funciona en sesiones locales y remotas.

La siguiente imagen muestra una representación aproximada del flujo de entrada de usuario de cliente para la aplicación.

![Escritorio remoto: flujos de entrada de usuario desde el cliente de escritorio remoto a los usuarios a la aplicación](.\media\rds-user-input.png)

El contador de retraso de entrada del usuario mide la diferencia máxima (dentro de un intervalo de tiempo) entre la entrada que se está poniendo en cola y cuando lo recoge la aplicación en un [bucle de mensajes tradicional](https://msdn.microsoft.com/library/windows/desktop/ms644927.aspx#loop), tal y como se muestra en el siguiente diagrama de flujo:

![Entrada de usuario de escritorio remoto: flujo de contador de rendimiento de retraso](.\media\rds-user-input-delay.png)

Un detalle importante de este contador es el que informa el retraso máximo de usuarios entrada dentro de un intervalo configurable. Este es el mayor tiempo que tarda una entrada llegar a la aplicación, que puede afectar a la velocidad de importantes y visibles acciones como escribir.

Por ejemplo, en la tabla siguiente, el retraso de entrada del usuario se notificarían como 1.000 ms dentro de este intervalo. El contador registra el usuario más lento de entrada retraso en el intervalo porque la percepción del usuario de "lento" viene determinada por la hora de entrada más lenta (el máximo) experimentan, no la velocidad promedio de todas las entradas total.

|Número| 0 | 1 | 2 |
|------|---|---|---|
|Retraso |16 ms| 20 ms| 1000 ms|

## <a name="enable-and-use-the-new-performance-counters"></a>Habilitar y usar los nuevos contadores de rendimiento

Para utilizar estos nuevos contadores de rendimiento, primero debe habilitar una clave del registro, ejecute este comando:

```
reg add "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "EnableLagCounter" /t REG_DWORD /d 0x1 /f
```

>[!NOTE]
> Si usa Windows 10, versión 1809 o posterior o Windows Server 2019 o versiones posteriores, no tendrá que habilitar la clave del registro.

A continuación, reinicie el servidor. A continuación, abra al Monitor de rendimiento y seleccione el signo más (+), como se muestra en la siguiente captura de pantalla.

![Contador de rendimiento retraso de la entrada de escritorio remoto: una captura de pantalla que muestra cómo agregar el usuario](.\media\rds-add-user-input-counter-screen.png)

Después de hacer eso, debería ver el cuadro de diálogo Agregar contadores, donde puede seleccionar **retraso de entrada de usuario por proceso** o **retraso de entrada del usuario por sesión**.

![Escritorio remoto: una captura de pantalla que muestra cómo agregar la entrada de retraso del usuario por sesión](.\media\rds-user-delay-per-session.png)

![Escritorio remoto: una captura de pantalla que muestra cómo agregar el retraso de entrada de usuario por proceso](.\media\rds-user-delay-per-process.png)

Si selecciona **retraso de entrada de usuario por proceso**, verá el **instancias del objeto seleccionado** (en otras palabras, los procesos) en ```SessionID:ProcessID <Process Image>``` formato.

Por ejemplo, si la aplicación de calculadora se está ejecutando en un [Id. de sesión 1](https://msdn.microsoft.com/library/ms524326.aspx), verá ```1:4232 <Calculator.exe>```.

> [!NOTE]
> No todos los procesos se incluyen. No verá todos los procesos que se ejecutan como sistema.

El contador empiece a enviar notificaciones de retraso de entrada del usuario en cuanto lo agregue. Tenga en cuenta que la escala máxima se establece en 100 (ms) de forma predeterminada. 

![Escritorio remoto: un ejemplo de actividad para el retraso de la entrada de usuario por proceso en el Monitor de rendimiento](.\media\rds-sample-user-input-delay-perfmon.png)

A continuación, echemos un vistazo a la **retraso de entrada del usuario por sesión**. Hay instancias para cada identificador de sesión y sus contadores muestran el retraso de entrada de usuario de cualquier proceso dentro de la sesión especificada. Además, hay dos instancias denominadas "Max" (el retraso entrada máximo de usuarios en todas las sesiones) y "Average" (la acorss de promedio todas las sesiones).

Esta tabla muestra un ejemplo visual de estas instancias. (Puede obtener la misma información en el Monitor de rendimiento mediante el cambio para el tipo de gráfico del informe.)

|Tipo de contador|Nombre de instancia|Retraso notificada (ms)|
|---------------|-------------|-------------------|
|Retraso de entrada de usuario por proceso|1:4232 < Calculator.exe >|  200|
|Retraso de entrada de usuario por proceso|2:1000 < Calculator.exe >|  16|
|Retraso de entrada de usuario por proceso|1: 2 000 < Calculator.exe >|  32|
|Retraso de la entrada de usuario por sesión|1|    200|
|Retraso de la entrada de usuario por sesión|2|    16|
|Retraso de la entrada de usuario por sesión|Media|  108|
|Retraso de la entrada de usuario por sesión|Máx.|  200|

## <a name="counters-used-in-an-overloaded-system"></a>Contadores que se usan en un sistema sobrecargado

Ahora Echemos un vistazo a lo que verá en el informe si se degrada el rendimiento de una aplicación. El siguiente gráfico muestra las lecturas de los usuarios que trabajan de forma remota en Microsoft Word. En este caso, el rendimiento del servidor RDSH empeora con el tiempo como más de los usuarios inician sesión.

![Escritorio remoto: un gráfico de rendimiento de ejemplo para el servidor RDSH ejecutando Microsoft Word](.\media\rds-user-input-perf-graph.png)

Aquí le mostramos cómo leer las líneas del gráfico:

- La línea rosa muestra el número de sesiones que inician sesión en el servidor.
- La línea roja es el uso de CPU.
- La línea verde es el retraso máximo de usuarios entrada en todas las sesiones.
- (Se muestra como negro en este gráfico) en la línea azul representa el retraso de entrada de usuario medio en todas las sesiones.

Observará que hay una correlación entre los picos de CPU y el retraso de entrada del usuario, como la CPU obtiene más uso, el usuario de entrada aumenta de retraso. Además, como más usuarios se agregan a del sistema, el uso de CPU obtiene más cercano al 100%, dando lugar a picos de retraso de entrada de usuario más frecuentes. Si bien este contador es muy útil en casos donde el servidor se ejecuta fuera de los recursos, también puede usar para realizar el seguimiento de retraso de entrada del usuario relacionada con una aplicación específica.

## <a name="configuration-options"></a>Opciones de configuración

Algo importante a recordar cuando use este contador de rendimiento es que informa de retraso de entrada del usuario en un intervalo de 1000 ms de forma predeterminada. Si establece la propiedad de intervalo de ejemplo de contador de rendimiento (como se muestra en la siguiente captura de pantalla) en algo diferente, el valor notificado es incorrecto.

![Escritorio remoto: las propiedades para el monitor de rendimiento](.\media\rds-user-input-perfmon-properties.png)

Para solucionar este problema, puede establecer la siguiente clave del registro para que coincida con el intervalo (en milisegundos) que desea usar. Por ejemplo, si cambiamos ejemplo cada x segundos a 5 segundos, es necesario establecer esta clave a 5000 ms.

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]

"LagCounterInterval"=dword:00005000
```

>[!NOTE]
>Si usa Windows 10, versión 1809 o posterior o Windows Server 2019 o versiones posteriores, no es necesario establecer LagCounterInterval para corregir el contador de rendimiento.

También hemos agregado un par de claves que puede resultarle útiles en la misma clave del registro:

**LagCounterImageNameFirst** : establezca esta clave en `DWORD 1` (valor predeterminado 0 o la clave no existe). Esto cambia los nombres de contador al "Nombre de imagen < SessionID:ProcessId >". Por ejemplo, "explorador < 1:7964 >". Esto es útil si desea ordenar por nombre de la imagen.

**LagCounterShowUnknown** : establezca esta clave en `DWORD 1` (valor predeterminado 0 o la clave no existe). Esto muestra todos los procesos que se ejecutan como servicios o del sistema. Se mostrarán algunos procesos con su sesión establecido como "?."

Este es su aspecto si activa las dos claves:

![Escritorio remoto: el monitor de rendimiento con las dos claves en](.\media\rds-user-input-delay-with-two-counters.png)

## <a name="using-the-new-counters-with-non-microsoft-tools"></a>Uso de los nuevos contadores con herramientas ajenas a Microsoft

Herramientas de supervisión pueden consumir este contador mediante el [API Perfmon](https://msdn.microsoft.com/library/windows/desktop/aa371903.aspx).

## <a name="download-windows-server-insider-software"></a>Descargar el software de Windows Server Insider

Insider registrado puede navegar directamente a la [página de descarga de Windows Server Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver) para obtener el software más reciente de Insider descargas.  Para obtener información sobre cómo registrar como una persona interna, consulte [Introducción a Server](https://insider.windows.com/en-us/for-business-getting-started-server/).

## <a name="share-your-feedback"></a>Comparta sus comentarios

Puede enviar comentarios para esta característica a través del centro de comentarios. Seleccione **aplicaciones > todas las demás aplicaciones** e incluya "los contadores de rendimiento de RDS, el monitor de rendimiento" en el título de la publicación.

Para obtener ideas de características general, visite la [página UserVoice de RDS](https://aka.ms/uservoice-rds).
