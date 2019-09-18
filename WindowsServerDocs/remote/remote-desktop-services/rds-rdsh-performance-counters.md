---
title: Uso de contadores de rendimiento para diagnosticar problemas de capacidad de respuesta de las aplicaciones en los hosts de sesión de Escritorio remoto
description: ¿Funcionan muy lentamente las aplicaciones en la sesión de Escritorio remoto? Obtén información acerca de los contadores de rendimiento que puede utilizar para diagnosticar problemas de rendimiento de aplicaciones en RDSH
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 07/11/2019
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 3eb1e4b6da971d788383b8facbf8bbcbe00a5953
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870910"
---
# <a name="use-performance-counters-to-diagnose-app-performance-problems-on-remote-desktop-session-hosts"></a>Uso de contadores de rendimiento para diagnosticar problemas de rendimiento de las aplicaciones en los hosts de sesión de Escritorio remoto

> Se aplica a: Windows Server 2019, Windows 10

Uno de los problemas más difíciles de diagnosticar es que el rendimiento de las aplicaciones sea deficiente (las aplicaciones funcionan con lentitud o no responden). Tradicionalmente, el primer paso del diagnóstico es recopilar los datos de la CPU, memoria, entrada/salida del disco, y otras métricas y, después, usar herramientas como Windows Performance Analyzer para intentar averiguar qué es lo que provoca el problema. Por desgracia, en la mayoría de los casos, estos datos no ayudan a identificar la causa raíz, ya que los contadores de consumo de recursos tienen variaciones frecuentes y grandes, lo que dificulta la lectura de los datos y su correlación con el problema notificado. Para ayudarle a solucionar rápidamente los problemas de rendimiento de las aplicaciones, hemos agregado varios contadores de rendimiento nuevos (se pueden [descargar](#download-windows-server-insider-software) a través del [programa Windows Insider](https://insider.windows.com)) que miden los flujos de entrada del usuario.

>[!NOTE]
>El contador de retraso de entrada de usuario solo es compatible con:
> - Windows Server 2019 o posterior
> - Windows 10, versión 1809 o posterior

El contador User Input Delay puede ayudarle a identificar rápidamente la causa raíz de los problemas que tienen los usuarios finales en RDP. Este contador mide el tiempo que una entrada de usuario cualquiera (por ejemplo, el uso del ratón o del teclado) permanece en la cola antes de que la recoja un proceso y el contador funciona en sesiones tanto locales como remotas.

La siguiente imagen muestra una representación somera del flujo de entrada del usuario del cliente a la aplicación.

![Escritorio remoto: flujos de entrada de usuario desde el cliente de Escritorio remoto del usuario a la aplicación](./media/rds-user-input.png)

El contador de retraso de entrada del usuario mide la diferencia máxima (en un intervalo dado) entre la entrada que está en la cola y el momento en que la recoge la aplicación en un [bucle de mensajes tradicional](https://msdn.microsoft.com/library/windows/desktop/ms644927.aspx#loop), tal como se muestra en el siguiente diagrama de flujo:

![Escritorio remoto: flujo del contador de rendimiento de User Input Delay (Retraso de entrada del usuario)](./media/rds-user-input-delay.png)

Un detalle importante de este contador es que indica el retraso máximo de la entrada del usuario en un intervalo configurable. Es el tiempo máximo que tarda una entrada en llegar a la aplicación, lo que puede afectar a la velocidad de acciones importantes y visibles, como escribir.

Por ejemplo, en la siguiente tabla, la demora de la entrada del usuario se notificaría como 1000 ms dentro de este intervalo. El contador registra el retraso de entrada del más lento del intervalo porque la percepción del usuario de "lento" viene determinada por el máximo tiempo de entrada que experimentan, no por la velocidad media de todas las entradas totales.

|Número| 0 | 1 | 2 |
|------|---|---|---|
|Retraso |16 ms| 20 ms| 1000 ms|

## <a name="enable-and-use-the-new-performance-counters"></a>Habilitación y uso de los nuevos contadores de rendimiento

Para utilizar estos nuevos contadores de rendimiento, primero se debe habilitar una clave del Registro, para lo que hay que ejecutar este comando:

```
reg add "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "EnableLagCounter" /t REG_DWORD /d 0x1 /f
```

>[!NOTE]
> Si se usa la versión 1809 de Windows 10, o cualquier versión posterior, o Windows Server 2019, o cualquier versión posteriores, no será preciso habilitar la clave del Registro.

A continuación, reinicie el servidor. Luego, abre al Monitor de rendimiento y selecciona el signo más (+), como se muestra en la siguiente captura de pantalla.

![Escritorio remoto: captura de pantalla que muestra cómo agregar el contador de rendimiento User Input Delay (Retraso de entrada del usuario)](./media/rds-add-user-input-counter-screen.png)

Tras ello, deberías ver el cuadro de diálogo Agregar contadores, donde puedes seleccionar **User Input Delay per Process** (Retraso de entrada del usuario por proceso) o **User Input Delay per Session** (Retraso de entrada de usuario por sesión).

![Escritorio remoto: captura de pantalla que muestra cómo agregar el retraso de la entrada del usuario por sesión](./media/rds-user-delay-per-session.png)

![Escritorio remoto: captura de pantalla que muestra cómo agregar el retraso de la entrada del usuario por proceso](./media/rds-user-delay-per-process.png)

Si seleccionas **User Input Delay per Process** (Retraso de entrada del usuario por proceso), verás **Instances of the selected object** (Instancias del objeto seleccionado), es decir, los procesos, en formato ```SessionID:ProcessID <Process Image>```.

Por ejemplo, si la aplicación Calculadora se ejecuta en [Session ID 1](https://msdn.microsoft.com/library/ms524326.aspx), verá ```1:4232 <Calculator.exe>```.

> [!NOTE]
> No están incluidos todos los procesos. No verás ningún proceso que se ejecute como SISTEMA.

El contador empieza a notificar el retraso en la entrada del usuario en cuanto se agrega. Ten en cuenta que la escala máxima se establece en 100 (ms) de manera predeterminada. 

![Escritorio remoto: un ejemplo de actividad de Retraso de entrada del usuario por proceso en el Monitor de rendimiento](./media/rds-sample-user-input-delay-perfmon.png)

A continuación, se va a examinar el **retraso de entrada del usuario por sesión**. Hay instancias de cada identificador de sesión y sus contadores muestran el retraso de entrada de usuario de todos los procesos dentro de la sesión especificada. Además, hay dos instancias denominadas "Max" (el retraso máximo en la entrada de usuario en todas las sesiones) y "Average" (el promedio de promedio todas las sesiones).

Esta tabla muestra un ejemplo visual de estas instancias. (La misma información se puede obtener en el Monitor de rendimiento si se cambia al tipo de gráfico de informe.)

|Tipo de contador|Nombre de instancia|Retraso notificado (ms)|
|---------------|-------------|-------------------|
|Retraso de entrada de usuario por proceso|1:4232 <Calculator.exe>|  200|
|Retraso de entrada de usuario por proceso|2:1000 <Calculator.exe>|  16|
|Retraso de entrada de usuario por proceso|1:2000 <Calculator.exe>|  32|
|Retraso de entrada de usuario por sesión|1|    200|
|Retraso de entrada de usuario por sesión|2|    16|
|Retraso de entrada de usuario por sesión|Media|  108|
|Retraso de entrada de usuario por sesión|Máx.|  200|

## <a name="counters-used-in-an-overloaded-system"></a>Contadores que se usan en un sistema sobrecargado

Ahora vamos a examinar lo que verá en el informe si empeora el rendimiento de una aplicación. El siguiente gráfico muestra las lecturas de usuarios que trabajan de forma remota en Microsoft Word. En este caso, el rendimiento del servidor de RDSH empeora con el paso del tiempo, ya que inician sesión más usuarios.

![Escritorio remoto: un gráfico de rendimiento de ejemplo del servidor de RDSH en el que se ejecuta Microsoft Word](./media/rds-user-input-perf-graph.png)

Así es como se deben leer las líneas del gráfico:

- La línea rosa muestra el número de sesiones que se han iniciado en el servidor.
- La línea roja es el uso de la CPU.
- La línea verde es el retraso máximo en la entrada del usuario en todas las sesiones.
- La línea azul (que se muestra como negra en este gráfico) representa el promedio de retraso de entrada de usuario en todas las sesiones.

Observarás que hay una correlación entre los picos de la CPU y el retraso de la entrada del usuario (al usarse más la CPU, el retraso de la entrada del usuario aumenta). Además, como se agregan más usuarios al sistema, el uso de la CPU se acerca al 100 %, lo que dando lugar a picos de retraso de la entrada del usuario más frecuentes. Aunque este contador es muy útil en aquellos casos en los que el servidor se queda sin recursos, también se puede usar para realizar un seguimiento del retraso de la entrada del usuario, en relación con un programa específico.

## <a name="configuration-options"></a>Opciones de configuración

Algo importante que no hay que olvidar cuando se usa este contador de rendimiento es que informa del retraso de la entrada del usuario en un intervalo de 1000 ms de manera predeterminada. Si establece otro valor para la propiedad del intervalo de ejemplo del contador de rendimiento (como se muestra en la siguiente captura de pantalla), el valor notificado será incorrecto.

![Escritorio remoto: las propiedades del monitor de rendimiento](./media/rds-user-input-perfmon-properties.png)

Para solucionar este problema, puede establecer la siguiente clave del Registro de forma que coincida con el intervalo (en milisegundos) que quieres usar. Por ejemplo, si se cambia Muestreo cada x segundos a 5 segundos, será preciso establecer esta clave en 5000 ms.

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]

"LagCounterInterval"=dword:00005000
```

>[!NOTE]
>Si se usa la versión 1809 de Windows 10, o cualquier versión posterior, o Windows Server 2019, o cualquier versión posterior, no es preciso establecer el valor de LagCounterInterval para corregir el contador de rendimiento.

También hemos agregado un par de claves que quizás te resulten útiles en la misma clave del Registro:

**LagCounterImageNameFirst**: establece esta clave en `DWORD 1` (el valor predeterminado es 0 o la clave no existe). Esto cambia los nombres de los contadores a "Nombre de imagen <SessionID:ProcessId >". Por ejemplo, "explorer < 1:7964 >". Esto resulta útil para ordenar por nombre de imagen.

**LagCounterShowUnknown**: establece esta clave en `DWORD 1` (el valor predeterminado es 0 o la clave no existe). Muestra todos los procesos que se ejecutan como servicios o como SISTEMA. Algunos de ellos mostrarán que su sesión se ha establecido como "?".

Este es su aspecto si se activan las dos claves:

![Escritorio remoto: el monitor de rendimiento con las dos claves activadas](./media/rds-user-input-delay-with-two-counters.png)

## <a name="using-the-new-counters-with-non-microsoft-tools"></a>Uso de los nuevos contadores con herramientas ajenas a Microsoft

Las herramientas de supervisión pueden consumir este contador mediante el uso de [API Perfmon](https://msdn.microsoft.com/library/windows/desktop/aa371903.aspx).

## <a name="download-windows-server-insider-software"></a>Descarga del software Windows Server Insider

Los usuarios registrados de Insider puede ir directamente a la [página de descarga de Windows Server Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver) para obtener las descargas más recientes del software Insider.  Para aprender a registrarse como usuario de Insider, consulte [Introducción a Windows Server](https://insider.windows.com/en-us/for-business-getting-started-server/).

## <a name="share-your-feedback"></a>Comparte tus comentarios

Puedes enviar tus comentarios acerca de esta característica a través del Centro de opiniones. Selecciona **Aplicaciones > Todas las demás aplicaciones** e incluya "contadores de rendimiento de RDS (Monitor de rendimiento)" en el título de la publicación.

Para obtener ideas acerca de las características generales, visite la [página de UserVoice de RDS](https://aka.ms/uservoice-rds).
