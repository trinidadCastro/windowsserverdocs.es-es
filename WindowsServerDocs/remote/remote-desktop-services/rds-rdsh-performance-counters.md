---
title: Usar los contadores de rendimiento para diagnosticar problemas de respuesta de la aplicación en Hosts de sesión de escritorio remoto
description: ¿Se ejecuta la aplicación lento RDS? Obtén información sobre los contadores de rendimiento que puedes usar para diagnosticar problemas de rendimiento de la aplicación en RDSH
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
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133721"
---
# Usar los contadores de rendimiento para diagnosticar problemas de rendimiento de la aplicación en Hosts de sesión de escritorio remoto

Uno de los problemas más difíciles de diagnosticar es el rendimiento de la aplicación, las aplicaciones se ejecutan lenta o no responde. Tradicionalmente, puedes iniciar el diagnóstico mediante la recopilación de CPU, memoria, la entrada y salida en disco y otras métricas y, a continuación, usan herramientas como Windows Performance Analyzer para intentar averiguar cuál es la causa del problema. Por desgracia en la mayoría de los casos estos datos no te ayudarán a identificar la causa porque los contadores de consumo de recursos tienen variaciones de uso frecuentes y grandes. Esto facilita el disco duro leer los datos y correlacionar con el problema notificado. Para más rápidamente solucionar los problemas de rendimiento de la aplicación, hemos agregado algunas nuevas los contadores de rendimiento (disponible [para descarga](#download-windows-server-insider-software) a través del [Programa Windows Insider](https://insider.windows.com)) que miden flujos de entrada de usuario.

El contador de retraso de entrada de usuario puede ayudarte a identificar rápidamente la causa de RDP experiencias del usuario final incorrecto. Este contador mide cuánto tiempo cualquier entrada (por ejemplo, el uso del mouse o teclado) del usuario permanece en la cola antes de se ha seleccionado por un proceso y el contador funciona en sesiones locales y remotas.

La siguiente imagen muestra una representación aproximada del flujo de entrada de usuario de cliente para la aplicación.

![Escritorio remoto - flujos de entrada de usuario desde el cliente de escritorio remoto de los usuarios a la aplicación](.\media\rds-user-input.png)

El contador de retraso de entrada de usuario mide el delta max (dentro de un intervalo de tiempo) entre la entrada que se ponen en cola y cuando se selecciona la aplicación en un [bucle de mensajes tradicionales](https://msdn.microsoft.com/library/windows/desktop/ms644927.aspx#loop), como se muestra en el siguiente diagrama de flujo:

![Escritorio remoto: flujo de contador de rendimiento de retraso de entrada de usuario](.\media\rds-user-input-delay.png)

Un detalle importante de este contador es que notifica el retraso de entrada de usuario máximo dentro de un intervalo configurable. Esto es lo más larga que tarda una entrada llegar a la aplicación, que puede afectar a la velocidad de acciones importantes y visibles por ejemplo, escribiendo.

Por ejemplo, en la siguiente tabla, se podría notifique el retraso de entrada de usuario como ms 1000 dentro de este intervalo. El contador informa al usuario más lento de entrada retraso en el intervalo porque la percepción del usuario de "lento" viene determinada por el momento de entrada más lenta (el máximo) la experiencia, no la velocidad promedio de todas las entradas total.

|Número| 0 | 1 | 2 |
|------|---|---|---|
|Retraso |16 ms| 20 ms| 1.000 ms|

## Habilitar y usar los nuevos contadores de rendimiento

Para usar estos nuevos contadores de rendimiento, primero debes habilitar una clave del registro mediante la ejecución de este comando:

```
reg add "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "EnableLagCounter" /t REG_DWORD /d 0x1 /f
```

>[!NOTE]
> Si estás usando Windows 10, versión 1809 o posterior, o Windows Server 2019 o una versión posterior, no tendrás que habilitar la clave del registro.

A continuación, reinicia el servidor. A continuación, abre al Monitor de rendimiento y selecciona el signo más (+), como se muestra en la siguiente captura de pantalla.

![Contador de rendimiento de retraso de entrada de escritorio remoto: una captura de pantalla que muestra cómo agregar el usuario](.\media\rds-add-user-input-counter-screen.png)

Después de esto, deberías ver el cuadro de diálogo Agregar contadores, donde puedes seleccionar **Retraso de entrada de usuario por proceso** o **Retraso de entrada de usuario por cada sesión**.

![Escritorio remoto - una captura de pantalla que muestra cómo agregar el retraso de entrada de usuario por cada sesión](.\media\rds-user-delay-per-session.png)

![Escritorio remoto - una captura de pantalla que muestra cómo agregar el retraso de entrada de usuario por proceso](.\media\rds-user-delay-per-process.png)

Si seleccionas el **Retraso de entrada de usuario por proceso**, podrás ver las **instancias del objeto seleccionado** (en otras palabras, los procesos) en ```SessionID:ProcessID <Process Image>``` formato.

Por ejemplo, si se ejecuta la aplicación Calculadora en una [sesión de Id. de 1](https://msdn.microsoft.com/library/ms524326.aspx), verás ```1:4232 <Calculator.exe>```.

> [!NOTE]
> No todos los procesos se incluyen. No verás los procesos que se ejecutan como sistema.

El contador inicia informes retraso de entrada de usuario tan pronto como para agregarlo. Ten en cuenta que la escala máxima se establece en 100 (ms) de manera predeterminada. 

![Escritorio remoto: un ejemplo de actividad para el retraso de entrada de usuario por proceso en el Monitor de rendimiento](.\media\rds-sample-user-input-delay-perfmon.png)

A continuación, echemos un vistazo al **Retraso de entrada de usuario por cada sesión**. Hay instancias para cada identificador de sesión y contadores de mostrarán el retraso de entrada de usuario de cualquier proceso dentro de la sesión especificada. Además, hay dos instancias denominadas "Max" (el número máximo de usuarios entrada de retraso en todas las sesiones) y "Promedio" (la acorss de promedio todas las sesiones).

En esta tabla se muestra un ejemplo de estas instancias de visual. (Puedes obtener la misma información en el Monitor de rendimiento al cambiar el tipo de gráfico del informe.)

|Tipo de contador|Nombre de la instancia|Retraso notificado (ms)|
|---------------|-------------|-------------------|
|Retraso de entrada de usuario por proceso|1:4232 < Calculator.exe >|  200|
|Retraso de entrada de usuario por proceso|2:1000 < Calculator.exe >|  16|
|Retraso de entrada de usuario por proceso|1: 2 000 < Calculator.exe >|  32|
|Retraso de entrada de usuario por cada sesión|1|    200|
|Retraso de entrada de usuario por cada sesión|2|    16|
|Retraso de entrada de usuario por cada sesión|Average|  108|
|Retraso de entrada de usuario por cada sesión|Max|  200|

## Contadores utilizados en un sistema sobrecargado

Ahora Echemos un vistazo a lo que verás en el informe si se degrada el rendimiento de una aplicación. El gráfico siguiente muestra las lecturas de los usuarios que trabajan de forma remota en Microsoft Word. En este caso, el rendimiento del servidor RDSH disminuye con el tiempo como más usuarios iniciar sesión.

![Escritorio remoto: un gráfico de rendimiento de ejemplo para el servidor RDSH ejecutando Microsoft Word](.\media\rds-user-input-perf-graph.png)

Aquí te mostramos cómo leer las líneas del gráfico:

- La línea rosa muestra el número de sesiones iniciado sesión en el servidor.
- La línea roja es el uso de CPU.
- La línea verde es el retraso de entrada de usuario máximo en todas las sesiones.
- La línea azul (que se muestra como negro en este gráfico) representa el retraso de entrada de usuario medio en todas las sesiones.

Verás que hay una correlación entre los picos de CPU y retraso de entrada del usuario, como la CPU obtiene más uso, el usuario aumenta de retraso de entrada. Además, ya que se agregarán más usuarios al sistema, el uso de CPU obtiene más cerca del 100%, lo que los picos de retraso de entrada de usuario con más frecuencia. Aunque este contador es muy útil en casos donde el servidor se ejecuta fuera de los recursos, también puedes usar para realizar un seguimiento de retraso de entrada de usuario relacionados con una aplicación específica.

## Opciones de configuración

Un aspecto importante que recordar al utilizar el contador de rendimiento es que notifica retraso de entrada del usuario en un intervalo de 1000 ms de manera predeterminada. Si estableces la propiedad de intervalo de ejemplo de contador de rendimiento (como se muestra en la siguiente captura de pantalla) a un valor diferente, el valor notificado es incorrecto.

![Escritorio remoto: las propiedades del Monitor de rendimiento](.\media\rds-user-input-perfmon-properties.png)

Para corregir esto, puedes establecer la siguiente clave del registro para que coincida con el intervalo (en milisegundos) que quieres usar. Por ejemplo, si cambiamos muestra cada x segundos a 5 segundos, necesitamos establecer esta clave para ms 5000.

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]

"LagCounterInterval"=dword:00005000
```

>[!NOTE]
>Si estás usando Windows 10, versión 1809 o posterior, o Windows Server 2019 o una versión posterior, no es necesario establecer LagCounterInterval para corregir el contador de rendimiento.

También hemos agregado un par de claves que podrían resultar útiles en la misma clave del registro:

**LagCounterImageNameFirst** : establece esta clave en `DWORD 1` (valor predeterminado 0 o la clave no existe). Esto cambia los nombres de contador a "Nombre de imagen < SessionID:ProcessId >". Por ejemplo, "explorer < 1:7964 >". Esto es útil si quieres ordenar por nombre de la imagen.

**LagCounterShowUnknown** : establece esta clave en `DWORD 1` (valor predeterminado 0 o la clave no existe). Muestra los procesos que se ejecutan como servicios o del sistema. Algunos procesos se mostrarán con su sesión establece como "?."

Este es su aspecto si activar ambas claves:

![Escritorio remoto - con ambas claves en el monitor de rendimiento](.\media\rds-user-input-delay-with-two-counters.png)

## Uso de los contadores de nuevo con herramientas ajenas a Microsoft

Herramientas de supervisión pueden consumir este contador con la [API de Monitor de rendimiento](https://msdn.microsoft.com/library/windows/desktop/aa371903.aspx).

## Descargar el software de Windows Server Insider

Los Insider registrados pueden navegar directamente a la [página de descarga de Windows Server Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver) para obtener las descargas de software más recientes de Insider.  Para obtener información sobre cómo registrarse como usuario de Insider, consulta la [Introducción de servidor](https://insider.windows.com/en-us/for-business-getting-started-server/).

## Comparte tus comentarios

Puedes enviar comentarios de esta característica a través del centro de opiniones. Selecciona **aplicaciones > todas las demás aplicaciones** e incluir "contadores de rendimiento de RDS: monitor de rendimiento" en el título del anuncio.

Para obtener ideas de característica generales, visite la [página de RDS UserVoice](https://aka.ms/uservoice-rds).
