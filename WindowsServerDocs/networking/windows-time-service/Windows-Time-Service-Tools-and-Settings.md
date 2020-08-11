---
ms.assetid: 6086947f-f9ef-4e18-9f07-6c7c81d7002c
title: Configuración y herramientas del servicio de hora de Windows
author: Teresa-Motiv
ms.author: v-tea
ms.date: 02/24/2020
ms.topic: article
ms.openlocfilehash: cb549f951865a065c70a6bfbfa9d49faf71ffd97
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87989791"
---
# <a name="windows-time-service-tools-and-settings"></a>Configuración y herramientas del servicio de hora de Windows

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o posterior

En este tema, obtendrás información sobre las herramientas y la configuración del servicio Hora de Windows (W32Time).

Si quieres sincronizar la hora de solo un equipo cliente unido a un dominio, consulta [Configuración de un equipo cliente para la sincronización automática de la hora del dominio](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29). Para ver otros temas sobre la configuración del servicio Hora de Windows, consulta [Dónde encontrar la información de configuración del servicio Hora de Windows](windows-time-service-top.md).

> [!CAUTION]
> No debes usar el comando **Tiempo neto** para configurar ni establecer la hora mientras se ejecuta el servicio de hora de Windows.
>
> Además, en los equipos antiguos que ejecutan Windows XP o versiones anteriores, en el comando **Net time /querysntp** se muestra el nombre de un servidor de Protocolo de tiempo de redes (NTP) con el que un equipo está configurado para sincronizarse, pero ese servidor NTP solo se usa cuando el cliente de hora del equipo está configurado como NTP o AllSync. Posteriormente, el comando ha quedado en desuso.

La mayoría de los equipos miembros de dominio tienen un tipo de cliente de hora de NT5DS, lo que significa que sincronizan la hora desde la jerarquía de dominios. La única excepción habitual a esto es el controlador de dominio que funciona como maestro de operaciones del emulador del controlador de dominio principal (PDC) del dominio raíz del bosque. Normalmente, el maestro de operaciones del emulador PDC está configurado para sincronizar la hora con un origen de hora externo. Para ver la configuración del cliente de hora de un equipo (a partir de Windows Server 2008 y Windows Vista), ejecuta el comando **W32tm /query /configuration** desde un símbolo del sistema con privilegios elevados, y lee la línea **Type** en la salida del comando. Para obtener más información, consulta [Funcionamiento del servicio Hora de Windows](How-the-Windows-Time-Service-Works.md). Además, puedes ejecutar el comando **reg query HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters** y leer el valor de **NtpServer** en la salida del comando.

> [!IMPORTANT]
> Antes de Windows Server 2016, el servicio W32Time no estaba diseñado para satisfacer las necesidades de las aplicaciones sujetas a limitaciones temporales. Sin embargo, las actualizaciones de Windows Server 2016 ahora permiten implementar una solución con una precisión de un milisegundo en el dominio. Para más información, consulta [Hora precisa para Windows Server 2016](accurate-time.md) y [Límite de soporte para configurar el servicio Hora de Windows para entornos de alta precisión](support-boundary.md).

## <a name="windows-time-service-tools"></a>Herramientas del servicio de hora de Windows

La siguiente herramienta está asociada con el servicio de hora de Windows.

### <a name="w32tmexe-windows-time"></a>W32tm.exe: Hora de Windows

**Categoría**

Esta herramienta forma parte de la instalación predeterminada de Windows (Windows XP y versiones posteriores) y de Windows Server (Windows Server 2003 y versiones posteriores).

**Compatibilidad de versiones**

Esta herramienta funciona en la instalación predeterminada de Windows (Windows XP y versiones posteriores) y de Windows Server (Windows Server 2003 y versiones posteriores).

Puedes usar W32tm.exe para configurar el servicio de hora de Windows y para diagnosticar los problemas del servicio de hora. W32tm.exe es la herramienta de línea de comandos preferida para configurar, supervisar o solucionar problemas del servicio de hora de Windows.

En las tablas siguientes se describen los parámetros que se pueden usar con W32tm.exe.

**Parámetros principales de W32tm.exe**

|Parámetro |Descripción |
| --- | --- |
|**w32tm /?** |Muestra la ayuda de la línea de comandos de W32tm. |
|**w32tm /register** |Registra el servicio de hora para que se ejecute como un servicio y agrega la información de configuración predeterminada al Registro. |
|**w32tm /unregister** |Anula el registro del servicio de hora y quita toda su información de configuración del Registro. |
|**w32tm /monitor [/domain:\<*domain name*>] [/computers:\<*name*>[,\<*name*>[,\<*name*>...]]] [/threads:\<*num*>]** |Supervisa el servicio de hora de Windows.<p>**/domain**: especifica el dominio que se va a supervisar. Si no se proporciona ningún nombre de dominio o no se especifican las opciones **/domain** ni **/computers**, se usa el dominio predeterminado. Esta opción se puede usar más de una vez.<p>**/computers**: supervisa la lista de equipos especificada. Los nombres de los equipos se separan con comas, no con espacios. Si un nombre tiene el prefijo **\*** , se trata como un PDC. Esta opción se puede usar más de una vez.<p>**/threads**: especifica el número de equipos que se van a analizar simultáneamente. El valor predeterminado es tres. El intervalo permitido es de 1-50. |
|**w32tm /ntte \<NT *time epoch*>** |Convierte una hora del sistema Windows NT (medida en intervalos de 10<sup>-7</sup> segundos desde 0h 1 ene 1601) en un formato legible. |
|**w32tm /ntpte \<NTP *time epoch*>** |Convierte una hora NTP (medida en intervalos de 2<sup>-32</sup> segundos desde 0h 1 ene 1900) en un formato legible. |
|**w32tm /resync [/computer:\<*computer*>] [/nowait] [/rediscover] [/soft]** |Indica a un equipo que debe volver a sincronizar el reloj lo antes posible y genera todas las estadísticas de error acumuladas.<p>**/computer:\<*computer*>** : especifica el equipo que debe volver a sincronizarse. Si no se especifica, se volverá a sincronizar el equipo local.<p>**/nowait**: no esperes a que se produzca la resincronización; vuelve inmediatamente. De lo contrario, espera a que la resincronización se complete antes de volver.<p>**/rediscover**: vuelve a detectar la configuración de red y los orígenes de red; a continuación, vuelve realizar la sincronización.<p>**/soft**: vuelve a realizar la sincronización con las estadísticas de error existentes. No es útil, se proporciona con fines de compatibilidad. |
|**w32tm /stripchart /computer:\<*target*> [/period:\<*refresh*>] [/dataonly] [/samples:\<*count*>] [/rdtsc]** |Muestra un gráfico de bandas del desplazamiento entre este equipo y otro.<p>**/computer:\<*target*>** : el equipo donde se medirá el desplazamiento.<p>**/period:\<*refresh*>** : Tiempo transcurrido entre muestras, en segundos. El valor predeterminado es dos segundos.<p>**/dataonly**: solo muestra los datos, sin gráficos.<p>**/samples:\<*count*>** : recopila muestras de \<*count*> y luego se detiene. Si no se especifica, las muestras se recopilarán hasta que se presione **Ctrl + C**.<br/><br/>**/rdtsc**: para cada muestra, esta opción imprime valores separados por comas, junto con los encabezados **RdtscStart**, **RdtscEnd**, **FileTime**, **RoundtripDelay** y **NtpOffset**, en lugar del gráfico de texto.<br/><ul><li>**RdtscStart**: valor [RDTSC (contador de marca de hora de lectura)](https://en.wikipedia.org/wiki/Time_Stamp_Counter) recopilado justo antes de generar la solicitud NTP.</li><li>**RdtscEnd**: valor de RDTSC recopilado justo después de la recepción y el procesamiento de la respuesta NTP.</li><li>**FileTime**: valor de FILETIME local usado en la solicitud NTP.</li><li>**RoundtripDelay**: tiempo transcurrido en segundos entre la generación de la solicitud NTP y el procesamiento de la respuesta NTP recibida, obtenido de los cálculos del recorrido de ida y vuelta NTP.</li><li>**NTPOffset**: desfase de hora en segundos entre el equipo local y el servidor NTP, obtenido de los cálculos de desfase NTP.</li></ul> |
|**w32tm /config [/computer:\<*target*>] [/update] [/manualpeerlist:\<*peers*>] [/syncfromflags:\<*source*>] [/LocalClockDispersion:\<*seconds*>] [/reliable:(YES\|NO)] [/largephaseoffset:\<*milliseconds*>]** |**/computer:\<*target*>** : ajusta la configuración de \<*target*>. Si no se especifica, el valor predeterminado es el equipo local.<p>**/update**: notifica al servicio de hora que la configuración ha cambiado y hace que los cambios surtan efecto.<p>**/manualpeerlist:\<*peers*>** : establece la lista manual de elementos del mismo nivel en \<*peers*>, que es una lista delimitada por espacios de direcciones IP o DNS. Al especificar varios elementos del mismo nivel, esta opción debe ir entre comillas.<p>**/syncfromflags:\<*source*>** : establece los orígenes desde los cuales debe sincronizarse el cliente NTP. \<*source*> debe ser una lista separada por comas de estas palabras clave (no distingue mayúsculas de minúsculas):<ul><li>**MANUAL**: incluye elementos del mismo nivel de la lista manual de elementos del mismo nivel.</li><li>**DOMHIER**: realiza la sincronización desde un controlador de dominio (DC) en la jerarquía de dominios.</li></ul>**/LocalClockDispersion:\<*seconds*>** : configura la precisión del reloj interno que W32Time adoptará cuando no pueda adquirir la hora de sus orígenes configurados.<p>**/reliable:(YES\|NO)** : establece si este equipo es un origen de hora de confianza. Esta configuración solo tiene sentido en controladores de dominio.<ul><li>**YES**: este equipo es un servicio de hora de confianza.</li><li>**NO**: este equipo no es un servicio de hora de confianza.</li></ul>**/largephaseoffset:\<*milliseconds*>** : establece la diferencia horaria entre la hora local y la de la red, que W32Time considerará un pico. |
|**w32tm /tz** |Muestra la configuración de zona horaria actual. |
|**w32tm /dumpreg [/subkey:\<*key*>] [/computer:\<*target*>]** |Muestra los valores asociados a una clave del Registro determinada.<p>La clave predeterminada es **HKLM\System\CurrentControlSet\Services\W32Time** (clave raíz para el servicio de hora).<p>**/subkey:\<*key*>** : muestra los valores asociados a la subclave <key> de la clave predeterminada.<p>**/computer:\<*target*>** : consulta la configuración del Registro para el equipo \<*target*>. |
|**w32tm /query [/computer:\<*target*>] {/source \| /configuration \| /peers \| /status} [/verbose]** |Muestra la información del servicio de hora de Windows de un equipo. Este parámetro primero estaba disponible en el cliente de hora de Windows de Windows Vista y Windows Server 2008.<p>**/computer:\<*target*>** : consulta la información de \<*target*>. Si no se especifica, el valor predeterminado es el equipo local.<p>**/source**: muestra el origen de la hora.<p>**/configuration**: muestra la configuración del tiempo de ejecución y la procedencia de la configuración. En el modo detallado, se muestra también el valor sin definir o sin usar.<p>**/peers**: muestra una lista de elementos del mismo nivel y su estado.<p>**/status**: muestra el estado del servicio de hora de Windows.<p>**/verbose**: establece el modo detallado para mostrar más información. |
|**w32tm /debug {/disable \| {/enable /file:\<*name*> /size:/<*bytes*> /entries:\<*value*> [/truncate]}}** |Habilita o deshabilita el registro privado del servicio de hora de Windows del equipo local. Este parámetro primero estaba disponible en el cliente de hora de Windows de Windows Vista y Windows Server 2008.<p>**/disable**: deshabilita el registro privado.<p>**/enable**: habilita el registro privado.<ul><li>**file:\<*name*>** : especifica el nombre de archivo absoluto.</li><li>**size:\<*bytes*>** : especifica el tamaño máximo del registro circular.</li><li>**entries:\<*value*>** : contiene una lista de marcas, especificadas por número y separadas por comas, que indican los tipos de información que se deben registrar. Los números válidos son de 0 a 300. Se admiten tanto un intervalo de números como números individuales (por ejemplo, 0-100,103,106). El valor 0-300 es para registrar toda la información.</li></ul>**/truncate**: trunca el archivo si existe. |

Para más información sobre **W32tm.exe**, consulta la [Ayuda de Windows](https://support.microsoft.com/hub/4338813/windows-help?os=windows-10).

**Ejemplos**

Si quieres establecer el cliente de hora de Windows local para que apunte a dos servidores horarios distintos, uno denominado ntpserver.contoso.com y otro clock.adatum.com, escribe el siguiente comando en la línea de comandos y presiona ENTRAR:

```cmd
w32tm /config /manualpeerlist:"ntpserver.contoso.com clock.adatum.com" /syncfromflags:manual /update
```

Para obtener una lista de servidores NTP válidos que están disponibles en Internet para la sincronización de hora externa, consulta [Una lista de los servidores horarios que utilizan el Protocolo simple de tiempo de redes (SNTP) y que están disponibles en Internet](https://go.microsoft.com/fwlink/?linkid=60401).

Si quieres comprobar la configuración del cliente de hora de Windows desde un equipo cliente basado en Windows que tiene el nombre de host CONTOSOW1, ejecuta el siguiente comando:

```cmd
W32tm /query /computer:contosoW1 /configuration
```

La salida de este comando es una lista de parámetros de configuración establecidos para el cliente de hora de Windows.

> [!IMPORTANT]
> [Windows Server 2016 ha mejorado los algoritmos de sincronización de hora](https://aka.ms/WS2016Time) para cumplir con las especificaciones RFC. Por lo tanto, si deseas establecer el cliente de hora de Windows local para que apunte a varios elementos del mismo nivel, se recomienda encarecidamente preparar tres o más servidores horarios diferentes.
>
> Si solo tienes dos servidores horarios, debes especificar la marca **UseAsFallbackOnly** (0X2) para anular la prioridad de uno de ellos. Por ejemplo, si quieres priorizar ntpserver.contoso.com ante clock.adatum.com, ejecuta el siguiente comando.
> ```cmd
> w32tm /config /manualpeerlist:"ntpserver.contoso.com,0x8 clock.adatum.com,0xa" /syncfromflags:manual /update
> ```
> Para conocer el significado de la marca especificada, consulta, [Entradas de la subclave "HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters"](#parameters).

## <a name="using-group-policy-to-configure-the-windows-time-service"></a>Uso de la directiva de grupo para configurar el servicio de hora de Windows

El servicio de hora de Windows almacena varias propiedades de configuración como entradas del Registro. Puedes usar objetos de la directiva de grupo para configurar la mayor parte de esta información. Por ejemplo, puedes usar GPO para configurar un equipo para que sea NTPServer o NTPClient, configurar el mecanismo de sincronización de la hora y configurar un equipo para que sea un origen de hora confiable.

> [!NOTE]
> La configuración de la directiva de grupo del servicio de hora de Windows se puede configurar en los controladores de dominio de Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y Windows Server 2008 R2 y solo se puede aplicar a equipos que ejecutan Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y Windows Server 2008 R2.

Windows almacena la información de la directiva de servicio de hora de Windows en el archivo de plantilla administrativa W32Time.admx, en **Configuración del equipo\Plantillas administrativas\Sistema\Servicio Hora de Windows**. Almacena la información de configuración que definen las directivas en el Registro y las usa para configurar las entradas del Registro para el servicio de hora de Windows. Como resultado, los valores definidos por directiva de grupo sobrescriben cualquier valor existente previamente en la sección del servicio de hora de Windows del Registro.

> [!IMPORTANT]
> Algunas de las configuraciones de GPO preestablecidas difieren de las entradas del Registro predeterminadas correspondientes. Si tienes previsto usar un GPO para configurar cualquier opción de Hora de Windows, asegúrate de revisar [Valores preestablecidos para la configuración de directiva de grupo del servicio de hora de Windows son diferentes de las correspondientes entradas del registro servicio hora de Windows en Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=186066). Este tema se aplica a Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 y Windows Server 2003.

Por ejemplo, supongamos que editas la configuración de directiva en la directiva **Configurar el cliente NTP de Windows**.

Los cambios se almacenan en la siguiente ubicación de la plantilla administrativa:

> **Configuración del equipo\Plantillas administrativas\Sistema\Servicio de hora de Windows\Configurar el cliente NTP de Windows**

Windows carga esta configuración en el área de directivas del Registro en la siguiente subclave:

> **HKLM\Software\Policies\Microsoft\W32time\TimeProviders\NtpClient**

A continuación, Windows usa la configuración de directiva para configurar las entradas del Registro del servicio de hora de Windows relacionadas en la siguiente subclave:

> **HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Time Providers\NTPClient\\**

En la tabla siguiente se enumeran las directivas que se pueden configurar para el servicio de hora de Windows y las subclaves del Registro a las que afectan esas directivas.

> [!NOTE]
> Cuando se quita una configuración de directiva de grupo, Windows quita la entrada correspondiente del área de directivas del Registro.

|Directiva<sup>1</sup> |Ubicaciones en el Registro<sup>2,</sup> <sup>3</sup> |
| --- | --- |
|Parámetros de configuración global |W32Time<br />W32Time\Config<br />W32Time\Parameters |
|Proveedores de hora\Configurar el cliente NTP de Windows |W32Time\TimeProviders\NtpClient |
|Proveedores de hora\Habilitar el cliente NTP de Windows |W32Time\TimeProviders\NtpClient |
|Proveedores de hora\Habilitar el servidor NTP de Windows |W32Time\TimeProviders\NtpServer |

> <sup>1</sup> Ruta de acceso de la categoría: **Configuración del equipo\Plantillas administrativas\Sistema\Servicio Hora de Windows**
> <sup>2</sup> Subclave: **HKLM\SOFTWARE\Policies\Microsoft**
> <sup>3</sup> Subclave: **HKLM\SYSTEM\CurrentControlSet\Services**

## <a name="enabling-w32time-logging"></a>Habilitación del registro de W32Time

Las siguientes tres entradas del Registro no forman parte de la configuración predeterminada de W32Time, pero se pueden agregar al Registro para obtener una mayor capacidad de registro. La información registrada en el registro de eventos del sistema se puede modificar cambiando el valor de la configuración **EventLogFlags** en el Editor de objetos de directiva de grupo. De forma predeterminada, el servicio de hora registra un evento cada vez que cambia a un nuevo origen de hora.

Para habilitar el registro de W32Time, agrega las siguientes entradas de Registro :

|Entrada del Registro |Versiones |Descripción |
| --- | --- | --- |
|**FileLogEntries** |Todas las versiones |Controla el número de entradas creadas en el archivo de registro de hora de Windows. El valor predeterminado es none, que no registra ninguna actividad de Hora de Windows. Los valores válidos son de **0** a **300**. Este valor no afecta a las entradas del registro de eventos que crea normalmente Hora de Windows. |
|**FileLogName** |Todas las versiones |Controla la ubicación y el nombre de archivo del registro de hora de Windows. El valor predeterminado está en blanco y no debe cambiarse a menos que se modifique **FileLogEntries**. Un valor válido es una ruta de acceso completa y un nombre de archivo que Hora de Windows usará para crear el archivo de registro. Este valor no afecta a las entradas del registro de eventos que crea normalmente Hora de Windows. |
|**FileLogSize** |Todas las versiones |Controla el comportamiento del registro circular de los archivos de registro de hora de Windows. Cuando se definen **FileLogEntries** y **FileLogName**, Entry define el tamaño, en bytes, para permitir la cobertura del archivo de registro antes de sobrescribir las entradas de registro más antiguas con nuevas entradas. Usa un valor igual o mayor que **1000000** para esta configuración. Este valor no afecta a las entradas del registro de eventos que crea normalmente Hora de Windows. |

## <a name="configuring-how-windows-time-service-resets-the-computer-clock"></a>Configuración del modo en que el servicio de hora de Windows restablece el reloj del equipo

Para que W32Time establezca el reloj del equipo gradualmente, el desfase debe ser menor que el valor de **MaxAllowedPhaseOffset** y satisfacer la siguiente ecuación al mismo tiempo:

- Windows Server 2016 y versiones posteriores:

  > |**CurrentTimeOffset**| &divide; (16 &times; **PhaseCorrectRate** &times; **pollIntervalInSeconds**) &le; **SystemClockRate** &divide; 2

- Windows Server 2012 R2 y versiones anteriores:

  > |**CurrentTimeOffset**| &divide; (**PhaseCorrectRate** &times; **UpdateInterval**) &le; **SystemClockRate** &divide; 2

El valor de **CurrentTimeOffset** se mide en ciclos del reloj, donde 1 ms = 10 000 ciclos de reloj en un sistema Windows.

**SystemClockRate** y **PhaseCorrectRate** también se miden en tics del reloj. Para obtener el valor de **SystemClockRate**, puedes usar el siguiente comando y convertirlo de segundos a ciclos de reloj mediante la fórmula de *segundos* &times; 1000 &times; 10 000:

```cmd
W32tm /query /status /verbose
ClockRate: 0.0156000s
```

**SystemClockRate** es la velocidad del reloj en el sistema. Si tomamos como ejemplo 156 000 segundos, el valor de **SystemclockRate** sería = 0,0156000 &times; 1000 &times; 10 000 = 156 000 ciclos de reloj.

**MaxAllowedPhaseOffset** también se mide en segundos. Para convertirlo a ciclos de reloj, multiplica **MaxAllowedPhaseOffset** &times; 1000 &times;10 000.

En los siguientes ejemplos se muestra cómo aplicar estos cálculos cuando se usa Windows Server 2012 R2 o una versión anterior.

**Ejemplo 1: La hora difiere en cuatro minutos**

Tu hora es 11:05 y la hora de muestra que has recibido de un elemento del mismo nivel y que se considera correcta es 11:09.

> **PhaseCorrectRate** = 1
>
> **UpdateInterval** = 30 000 ciclos de reloj
>
> **SystemClockRate** = 156 000 ciclos de reloj
>
> **MaxAllowedPhaseOffset** = 10 min = 600 segundos = 600 &times; 1000 &times; 10 000 = 6 000 000 000 ciclos de reloj
>
> |**CurrentTimeOffset**| = 4 min = 4 &times; 60 &times; 1000 &times; 10 000 = 2 400 000 000 ciclos de reloj
>
> ¿Es **CurrentTimeOffset** &le; **MaxAllowedPhaseOffset**?
>
> 2 400 000 000 &le; 6 000 000 000: TRUE

¿Cumple con la ecuación anterior?

> (|**CurrentTimeOffset**| &divide; (**PhaseCorrectRate** &times; **UpdateInterval**) &le; **SystemClockRate** &divide; 2)

Es 2 400 000 000/(30 000 &times; 1) &le; 156 000 &divide; 2

80 000 &le; 78 000: FALSE

Por lo tanto, W32tm retrasaría el reloj inmediatamente.

> [!NOTE]
> En este caso, si quieres retrasar el reloj lentamente, también tendrás que ajustar los valores de **PhaseCorrectRate** o **UpdateInterval** en el Registro para asegurarte de que el resultado de la ecuación sea **TRUE**.

**Ejemplo 2: La hora difiere en tres minutos**

> **PhaseCorrectRate** = 1
>
> **UpdateInterval** = 30 000 ciclos de reloj
>
> SystemClockRate = 156 000 ciclos de reloj
>
> **MaxAllowedPhaseOffset** = 10 min = 600 segundos = 600 &times; 1000 &times; 10 000 = 6 000 000 000 ciclos de reloj
>
> |**CurrentTimeOffset**| = 3 min = 3 &times; 60 &times; 1000 &times; 10 000 = 1 800 000 000 ciclos de reloj
>
> ¿Es |**CurrentTimeOffset**| &le; **MaxAllowedPhaseOffset**?
>
> 1 800 000 000 &le; 6 000 000 000: TRUE

¿Cumple con la ecuación anterior?

> (|**CurrentTimeOffset**| &divide; (**PhaseCorrectRate** &times; **UpdateInterval**) &le; **SystemClockRate** &divide; 2)
>
> Es 3 min &times; (1 800 000 000) &divide; (30 000 &times; 1) &le; 156 000 &divide; 2
>
> Es 60 000 &le; 78 000: TRUE

En este caso, el reloj se retrasará lentamente.

## <a name="reference-windows-time-service-registry-entries"></a>Referencia: Entradas del Registro del servicio de hora de Windows

> [!WARNING]
> La información sobre estas entradas del Registro se proporciona como una referencia para la resolución de problemas o para comprobar que se aplica la configuración necesaria. W32Time usa internamente muchos de los valores de la sección W32Time del Registro para almacenar información. No cambies manualmente estos valores. El Editor del Registro o Windows no validan las modificaciones del Registro antes de que se apliquen. Si el Registro contiene valores no válidos, es posible que Windows experimente errores irrecuperables.

El servicio de hora de Windows almacena información en las subclaves del Registro siguientes:

- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config**](#config)
- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters**](#parameters)
- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient**](#ntpclient)
- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer**](#ntpserver)

Además, para solucionar problemas, puedes [agregar entradas para configurar los registros](#enabling-w32time-logging).

En las tablas siguientes, "Todas las versiones" hace referencia a las versiones de Windows que incluyen Windows 7, Windows 8, Windows 10, Windows Server 2008 y Windows Server 2008 R2, Windows Server 2012 y Windows Server 2012 R2, Windows Server 2016 y Windows Server 2019. Algunas entradas solo están disponibles en versiones posteriores de Windows.

> [!NOTE]
> Algunos de los parámetros del Registro se miden en ciclos de reloj y otros en segundos. Para convertir la hora de ciclos de reloj a segundos, usa estos factores de conversión:
> - 1 minuto = 60 s
> - 1 s = 1000 ms
> - 1 ms = 10 000 tics del reloj en un sistema Windows, tal como se describe en [Propiedad DateTime.Ticks](/dotnet/api/system.datetime.ticks).
>
> Por ejemplo, 5 minutos se convierten en 5 &times; 60 &times; 1000 &times; 10 000 = 3 000 000 000 ciclos de reloj.

### <a name="hklmsystemcurrentcontrolsetservicesw32timeconfig-subkey-entries"></a><a id="config"></a>Entradas de la subclave "HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config"

|Entrada del Registro |Versiones |Descripción |
| --- | --- | --- |
|**AnnounceFlags** |Todas las versiones |Controla si este equipo está marcado como un servidor de hora de confianza. Un equipo no está marcado como confiable a menos que también esté marcado como servidor horario.<ul><li>**0x00**. No es un servidor horario.</li><li>**0x01**. Siempre es un servidor horario.</li><li>**0x02**. Servidor horario automático.</li><li>**0x04**. Siempre es un servidor horario de confianza.</li><li>**0x08**. Servidor horario de confianza automático.</li></ul><br />El valor predeterminado para los miembros del dominio es **10**. El valor predeterminado para los servidores y clientes independientes es **10**. |
|**ChainDisable** | |Controla si el mecanismo de encadenamiento está deshabilitado o no. Si el encadenamiento está deshabilitado (establecido en 0), un controlador de dominio de solo lectura (RODC) se puede sincronizar con cualquier controlador de dominio, pero los hosts que no tengan su contraseña almacenada en caché en RODC no podrán sincronizarse con RODC. Se trata de un valor booleano y cuyo valor predeterminado es **0**.|
|**ChainEntryTimeout** | |Especifica la cantidad máxima de tiempo que una entrada puede permanecer en la tabla de encadenamiento antes de que se considere que la entrada ha expirado. Las entradas expiradas pueden quitarse cuando se procesa la siguiente solicitud o respuesta. El valor predeterminado es **16** (segundos). |
|**ChainLoggingRate** | |Controla la frecuencia con la que un evento que indica el número de intentos de encadenamiento correctos e incorrectos se registra en el Registro del sistema en el Visor de eventos. El valor predeterminado es **30** (minutos). |
|**ChainMaxEntries** | |Controla el número máximo de entradas permitidas en la tabla de encadenamiento. Si la tabla de encadenamiento está llena y no se puede quitar ninguna entrada expirada, se descartan las solicitudes entrantes. El valor predeterminado es **128** (entradas). |
|**ChainMaxHostEntries** | |Controla el número máximo de entradas que se permiten en la tabla de encadenamiento de un host determinado. El valor predeterminado es **4** (entradas). |
|**ClockAdjustmentAuditLimit** |Windows Server 2016 versión 1709 y versiones posteriores; Windows 10 versión 1709 y versiones posteriores |Especifica los ajustes del reloj local más pequeños que se pueden registrar en el registro de eventos del servicio W32time en el equipo de destino. El valor predeterminado es **800** (partes por millón de PPM). |
|**ClockHoldoverPeriod** |Windows Server 2016 versión 1709 y versiones posteriores; Windows 10 versión 1709 y versiones posteriores |Indica el número máximo de segundos durante los que un reloj del sistema puede mantener nominalmente su precisión sin sincronizarse con un origen de hora. Si este período de tiempo pasa sin que W32time obtenga nuevas muestras de ninguno de sus proveedores de entrada, W32time inicia una nueva detección de orígenes de hora. Default: 7 800 segundos. |
|**EventLogFlags** |Todas las versiones |Controla los eventos que registra el servicio de hora.<ul><li>**0x1**. Salto de hora</li><li>**0x2**. Cambio de origen</li></ul>El valor predeterminado en los miembros del dominio es **2**. El valor predeterminado en los servidores y clientes independientes es **2**. |
|**FrequencyCorrectRate** |Todas las versiones |Controla la velocidad a la que se corrige el reloj. Si este valor es demasiado pequeño, el reloj es inestable y se sobrecorrige. Si el valor es demasiado grande, el reloj tarda mucho tiempo en sincronizarse. El valor predeterminado en los miembros del dominio es **4**. El valor predeterminado en los servidores y clientes independientes es **4**.<p>**Note** <br />Cero no es un valor válido para la entrada del Registro **FrequencyCorrectRate**. En equipos con Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y Windows Server 2008 R2, si el valor se establece en **0**, el servicio de hora de Windows lo cambia automáticamente a **1**. |
|**HoldPeriod** |Todas las versiones |Controla el período de tiempo durante el que se deshabilita la detección de picos para que el reloj local se sincronice rápidamente. Un pico es una muestra de tiempo que indica que el tiempo está fuera de un número de segundos y se recibe normalmente después de la devolución constante de muestras de tiempo correctas. El valor predeterminado en los miembros del dominio es **5**. El valor predeterminado en los servidores y clientes independientes es **5**. |
|**LargePhaseOffset** |Todas las versiones |Especifica que un desfase de hora mayor o igual que este valor en 10<sup>-7</sup> segundos se considera un pico. Una interrupción de la red, como una gran cantidad de tráfico, puede provocar un pico. Un pido se omitirá a menos que se mantenga durante un largo período de tiempo. El valor predeterminado en los miembros del dominio es **50000000**. El valor predeterminado en los servidores y clientes independientes es **50000000**. |
|**LastClockRate** |Todas las versiones |W32Time la mantiene. Contiene datos reservados que se usan en el sistema operativo Windows y los cambios que se realicen en esta configuración pueden producir resultados imprevisibles. El valor predeterminado en los miembros del dominio es **156250**. El valor predeterminado en los servidores y clientes independientes es **156250**. |
|**LocalClockDispersion** |Todas las versiones |Controla la dispersión (en segundos) que debes asumir cuando el único origen de hora es el reloj CMOS integrado. El valor predeterminado en los miembros del dominio es **10**. El valor predeterminado en los servidores y clientes independientes es **10**. |
|**MaxAllowedPhaseOffset** |Todas las versiones |Especifica el desfase máximo (en segundos) que W32Time intenta ajustar el reloj del equipo con la velocidad del reloj. Si el desfase supera esta velocidad, W32Time establece el reloj del equipo directamente. El valor predeterminado para los miembros del dominio es **300**. El valor predeterminado para los servidores y clientes independientes es **1**. Para más información, consulta [Configuración del modo en que el servicio de hora de Windows restablece el reloj del equipo](#configuring-how-windows-time-service-resets-the-computer-clock). |
|**MaxClockRate** |Todas las versiones |W32Time la mantiene. Contiene datos reservados que se usan en el sistema operativo Windows y los cambios que se realicen en esta configuración pueden producir resultados imprevisibles. El valor predeterminado para los miembros del dominio es **155860**. El valor predeterminado para los servidores y clientes independientes es **155860**. |
|**MaxNegPhaseCorrection** |Todas las versiones |Especifica la corrección de tiempo negativa más grande, en segundos, que realiza el servicio. Si el servicio determina que se necesita un cambio mayor que este, registra un evento en su lugar.<p>**Note**<br />El valor **0xFFFFFFFF** es un caso especial. Este valor significa que el servicio siempre corrige la hora.<p>El valor predeterminado para los miembros del dominio es **0xFFFFFFFF**. El valor predeterminado de los servidores y clientes independientes es **54 000** (15 horas).|
|**MaxPollInterval** |Todas las versiones |Especifica el intervalo más grande, en log2 segundos, permitidos para el intervalo de sondeo del sistema. Ten en cuenta que mientras que un sistema debe realizar un sondeo según el intervalo programado, un proveedor puede rechazar la generación de muestras si se le solicita. El valor predeterminado para los controladores de dominio es **10**. El valor predeterminado para los miembros del dominio es **15**. El valor predeterminado para los servidores y clientes independientes es **15**. |
|**MaxPosPhaseCorrection** |Todas las versiones |Especifica la corrección de tiempo positiva más grande en segundos que realiza el servicio. Si el servicio determina que se necesita un cambio mayor que este, registra un evento en su lugar.<p>**Note**<br />El valor **0xFFFFFFFF** es un caso especial. Este valor significa que el servicio siempre corrige la hora.<p>El valor predeterminado para los miembros del dominio es **0xFFFFFFFF**. El valor predeterminado de los servidores y clientes independientes es **54 000** (15 horas). |
|**MinClockRate** |Todas las versiones |W32Time la mantiene. Contiene datos reservados que se usan en el sistema operativo Windows y los cambios que se realicen en esta configuración pueden producir resultados imprevisibles. El valor predeterminado para los miembros del dominio es **155860**. El valor predeterminado para los servidores y clientes independientes es **155860**. |
|**MinPollInterval** |Todas las versiones |Especifica el intervalo más pequeño, en log2 segundos, permitido para el intervalo de sondeo del sistema. Ten en cuenta que mientras que un sistema no solicita muestras con más frecuencia que la indicada, un proveedor puede generar muestras en momentos distintos del intervalo programado. El valor predeterminado para los controladores de dominio es **6**. El valor predeterminado para los miembros del dominio es **10**. El valor predeterminado para los servidores y clientes independientes es **10**. |
|**PhaseCorrectRate** |Todas las versiones |Controla la velocidad a la que se corrige el error de fase. Si se especifica un valor pequeño, se corrige el error de fase rápidamente, pero es posible que el reloj se vuelva inestable. Si el valor es demasiado grande, se tarda más tiempo en corregir el error de fase.<p>El valor predeterminado en los miembros del dominio es **1**. El valor predeterminado en los servidores y clientes independientes es **7**.<p>**Note**<br />Cero no es un valor válido para la entrada del Registro **PhaseCorrectRate**. En equipos con Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y Windows Server 2008 R2, si el valor se establece en **0**, el servicio de hora de Windows lo cambia automáticamente a **1**. |
|**PollAdjustFactor** |Todas las versiones |Controla la decisión de aumentar o disminuir el intervalo de sondeo del sistema. Cuanto mayor sea el valor, menor será la cantidad de error que causará una reducción del intervalo de sondeo. El valor predeterminado en los miembros del dominio es **5**. El valor predeterminado en los servidores y clientes independientes es **5**. |
|**RequireSecureTimeSyncRequests** |Windows 8 y versiones posteriores |Controla si el controlador de dominio responderá o no a las solicitudes de sincronización de la hora que usan protocolos de autenticación más antiguos. Si se habilita (se establece en **1**), el controlador de dominio no responderá a las solicitudes que usen dichos protocolos. Se trata de un valor booleano y cuyo valor predeterminado es **0**. |
|**SpikeWatchPeriod** |Todas las versiones |Especifica la cantidad de tiempo que un desfase sospechoso debe persistir antes de que se acepte como correcto (en segundos). El valor predeterminado en los miembros del dominio es **900**. El valor predeterminado en los servidores y clientes independientes es **900**. |
|**TimeJumpAuditOffset** |Todas las versiones |Entero sin signo que indica el umbral de auditoría de salto de hora, en segundos. Si el servicio de hora ajusta el reloj local mediante el ajuste directo del reloj, y la corrección de tiempo es mayor que este valor, el servicio de hora registra un evento de auditoría. |
|**UpdateInterval** |Todas las versiones |Especifica el número de ciclos de reloj entre los ajustes de corrección de fase. El valor predeterminado para los controladores de dominio es **100**. El valor predeterminado para los miembros del dominio es **30 000**. El valor predeterminado para los servidores y clientes independientes es **360 000**.<p>**Note**<br />Cero no es un valor válido para la entrada del Registro **UpdateInterval**. En equipos que ejecutan Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y Windows Server 2008 R2, si el valor se establece en **0**, el servicio de hora de Windows lo cambia automáticamente a **1**.|
|**UtilizeSslTimeData** |Versiones de Windows posteriores a Windows 10 compilación 1511 |El valor **1** indica que W32Time usa varias marcas de tiempo de SSL para inicializar un reloj que es extremadamente impreciso. |

### <a name="hklmsystemcurrentcontrolsetservicesw32timeparameters-subkey-entries"></a><a id="parameters"></a>Entradas de la subclave "HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters"

| Entrada del Registro | Versiones | Descripción |
| --- | --- | --- |
|**AllowNonstandardModeCombinations** |Todas las versiones |Indica que se permiten combinaciones de modos no estándar en la sincronización entre elementos del mismo nivel. El valor predeterminado para los miembros del dominio es **1**. El valor predeterminado para los servidores y clientes independientes es **1**. |
|**NtpServer** |Todas las versiones |Especifica una lista delimitada por espacios de los elementos del mismo nivel de los que un equipo obtiene las marcas de tiempo, que consta de uno o varios nombres DNS o direcciones IP por línea. Cada nombre DNS o dirección IP de la lista debe ser único. Los equipos conectados a un dominio deben sincronizarse con un origen de hora más confiable, como el reloj oficial de la hora de los EE. UU.  <ul><li>0x01 SpecialInterval </li><li>0x02 UseAsFallbackOnly</li><li>0x04 SymmetricActive: para obtener más información sobre este modo, consulta el artículo sobre los [modos de funcionamiento del servidor de Hora de Windows](https://go.microsoft.com/fwlink/?LinkId=208012) (en inglés).</li><li>0x08 Client</li></ul><br />No hay ningún valor predeterminado para esta entrada del Registro en los miembros del dominio. El valor predeterminado en los servidores y clientes independientes es time.windows.com,0x1.<p>**Note**<br />Para más información sobre los servidores NTP disponibles, consulta KB 262680 [Una lista de los servidores horarios que utilizan el Protocolo simple de tiempo de redes (SNTP) y que están disponibles en Internet](https://support.microsoft.com/help/262680/a-list-of-the-simple-network-time-protocol-sntp-time-servers-that-are). |
|**ServiceDll** |Todas las versiones |W32Time la mantiene. Contiene datos reservados que se usan en el sistema operativo Windows y los cambios que se realicen en esta configuración pueden producir resultados imprevisibles. La ubicación predeterminada de este archivo DLL en los miembros del dominio y en los servidores y clientes independientes es %windir%\System32\W32Time.dll. |
|**ServiceMain** |Todas las versiones |W32Time la mantiene. Contiene datos reservados que se usan en el sistema operativo Windows y los cambios que se realicen en esta configuración pueden producir resultados imprevisibles. El valor predeterminado en los miembros del dominio es **SvchostEntry_W32Time**. El valor predeterminado en los servidores y clientes independientes es **SvchostEntry_W32Time**. |
|**Type** |Todas las versiones |Indica desde qué elementos del mismo nivel se aceptará la sincronización:  <ul><li>**NoSync**. El servicio de hora no se sincroniza con otros orígenes.</li><li>**NTP**. El servicio de hora se sincroniza desde los servidores especificados en la entrada del Registro **NtpServer** Entrada del Registro.</li><li>**NT5DS**. El servicio de hora se sincroniza desde la jerarquía de dominios.  </li><li>**AllSync**. El servicio de hora usa todos los mecanismos de sincronización disponibles.  </li></ul>El valor predeterminado en los miembros del dominio es **NT5DS**. El valor predeterminado en los servidores y clientes independientes es **NTP**. |

### <a name="hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpclient-subkey-entries"></a><a id="ntpclient"></a>Entradas de la clave "HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient"

|Entrada del Registro |Version |Descripción |
| --- | --- | --- |
|**AllowNonstandardModeCombinations** |Todas las versiones |Indica que se permiten combinaciones de modos no estándar en la sincronización entre elementos del mismo nivel. El valor predeterminado para los miembros del dominio es **1**. El valor predeterminado para los servidores y clientes independientes es **1**.|
|**CompatibilityFlags** |Todas las versiones |Especifica los valores y las marcas de compatibilidad siguientes:<ul><li>**0x00000001**: DispersionInvalid</li><li>**0x00000002**: IgnoreFutureRefTimeStamp</li><li>**0x80000000**: AutodetectWin2K</li><li>**0x40000000**: AutodetectWin2KStage2</li></ul>El valor predeterminado para los miembros del dominio es **0x80000000**. El valor predeterminado para los servidores y clientes independientes es **0x80000000**. |
|**CrossSiteSyncFlags** |Todas las versiones |Determina si el servicio elige asociados de sincronización fuera del dominio del equipo. Las opciones y los valores son:<ul><li>**0**: None</li><li>**1**: PdcOnly</li><li>**2**: All</li></ul>Este valor se omite si no se establece el valor de NT5DS. El valor predeterminado para los miembros del dominio es **2**. El valor predeterminado para los servidores y clientes independientes es **2**. |
|**DllName** |Todas las versiones |Especifica la ubicación del archivo DLL para el proveedor de hora.<p>La ubicación predeterminada de este archivo DLL en los miembros del dominio y en los servidores y clientes independientes es **%windir%\System32\W32Time.dll**. |
|**Habilitado** |Todas las versiones |Indica si el proveedor NtpClient está habilitado en el servicio de hora actual.<ul><li>**1**: Sí</li><li>**0**: No</li></ul>El valor predeterminado en los miembros del dominio es **1**. El valor predeterminado en los servidores y clientes independientes es **1**.|
|**EventLogFlags** |Todas las versiones |Especifica los eventos registrados por el servicio de hora de Windows.<ul><li>**0x1**: cambios de disponibilidad</li><li>**0x2**: sesgo de muestra grande (solo es aplicable a Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y Windows Server 2008 R2)</li></ul>El valor predeterminado en los miembros del dominio es **0x1**. El valor predeterminado en los servidores y clientes independientes es **0x1**.|
|**InputProvider** |Todas las versiones |Indica si se debe habilitar NtpClient como InputProvider, que obtiene la información de hora de NtpServer. NtpServer es un servidor de hora que responde a las solicitudes de hora del cliente en la red, para lo cual devuelve muestras de hora que son útiles para sincronizar el reloj local. <ul><li>**1**: Sí</li><li>**0**: No</li></ul>El valor predeterminado para los miembros del dominio y los clientes independientes es **1**. |
|**LargeSampleSkew** |Todas las versiones |Especifica el sesgo de muestras grande del registro, en segundos. Para cumplir con las especificaciones de la Comisión de Valores y Bolsa de EE. UU. (SEC), debe establecerse en tres segundos. Los eventos se registrarán para esta opción solo cuando **EventLogFlags** se configure explícitamente para el sesgo de muestra grande de 0x2. El valor predeterminado en los miembros del dominio es 3. El valor predeterminado en los servidores y clientes independientes es 3. |
|**ResolvePeerBackOffMaxTimes** |Todas las versiones |Especifica el número máximo de veces que se debe duplicar el intervalo de espera cuando se intenta buscar repetidamente un elemento del mismo nivel para la sincronización sin éxito. Un valor de cero significa que el intervalo de espera siempre es el mínimo. El valor predeterminado en los miembros del dominio es **7**. El valor predeterminado en los servidores y clientes independientes es **7**. |
|**ResolvePeerBackoffMinutes** |Todas las versiones |Especifica el intervalo inicial que hay que esperar, en minutos, antes de intentar buscar un elemento del mismo nivel para la sincronización. El valor predeterminado en los miembros del dominio es **15**. El valor predeterminado en los servidores y clientes independientes es **15**.  |
|**SpecialPollInterval** |Todas las versiones |Especifica el intervalo de sondeo especial, en segundos, para los elementos del mismo nivel manuales. Cuando se habilita la marca **SpecialInterval** 0x1, W32Time usa este intervalo de sondeo en lugar de un intervalo de sondeo determinado por el sistema operativo. El valor predeterminado en los miembros del dominio es **3600**. El valor predeterminado en los servidores y clientes independientes es **604 800**.<br/><br/>Nuevo en la compilación 1703: **SpecialPollInterval** se incluye en los valores del Registro Config **MinPollInterval** y **MaxPollInterval**.|
|**SpecialPollTimeRemaining** |Todas las versiones |W32Time la mantiene. Contiene datos reservados que se usan en el sistema operativo Windows. Especifica el tiempo, en segundos, antes de que W32Time se vuelva a sincronizar después de reiniciar el equipo. Cualquier cambio en esta opción puede provocar resultados imprevisibles. El valor predeterminado en ambos miembros del dominio y en los servidores y clientes independientes se deja en blanco. |

### <a name="hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpserver-subkey-entries"></a><a id="ntpserver"></a>Entradas de la clave "HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer"

|Entrada del Registro |Versiones |Descripción |
| --- | --- | --- |
|**AllowNonstandardModeCombinations** |Todas las versiones |Indica que se permiten combinaciones de modos no estándar en la sincronización entre clientes y servidores. El valor predeterminado para los miembros del dominio es **1**. El valor predeterminado para los servidores y clientes independientes es **1**. |
|**DllName** |Todas las versiones |Especifica la ubicación del archivo DLL para el proveedor de hora. La ubicación predeterminada de este archivo DLL en los miembros del dominio y en los servidores y clientes independientes es **%windir%\System32\W32Time.dll**.  |
|**Habilitado** |Todas las versiones |Indica si el proveedor NtpServer está habilitado en el servicio de hora actual. <ul><li>**1**: Sí</li><li>**0**: No</li></ul>El valor predeterminado en los miembros del dominio es **1**. El valor predeterminado en los servidores y clientes independientes es **1**. |
|**InputProvider** |Todas las versiones |Indica si se debe habilitar NtpClient como InputProvider, que obtiene la información de hora de NtpServer. NtpServer es un servidor de hora que responde a las solicitudes de hora del cliente en la red, para lo cual devuelve muestras de hora que son útiles para sincronizar el reloj local. <ul><li>**1**: Sí</li><li>**0**: No = 0 </li></ul>Valor predeterminado para los miembros del dominio y los clientes independientes: 1 |

## <a name="reference-pre-set-values-for-the-windows-time-service-gpo-settings"></a>Referencia: valores predefinidos para la configuración del GPO del servicio de hora de Windows

En la tabla siguiente se enumeran las opciones de la directiva de grupo globales asociadas al servicio Hora de Windows y el valor preestablecido asociado a cada opción. Para más información sobre cada configuración, consulta las entradas del registro correspondientes en [Referencia: Entradas del registro del servicio de hora de Windows](#reference-windows-time-service-registry-entries) anteriormente en este artículo. Las siguientes opciones de configuración se incluyen en un solo GPO denominado **Parámetros de configuración global**.

### <a name="pre-set-values-for-global-group-policy-settings"></a>Valores preestablecidos para la configuración de "Directiva de grupo global"

|Configuración de directiva de grupo|Valor preestablecido|
| --- | --- |
|**AnnounceFlags**|**10**|
|**EventLogFlags**|**2**|
|**FrequencyCorrectRate**|**4**|
|**HoldPeriod**|**5**|
|**LargePhaseOffset**|**1 280 000**|
|**LocalClockDispersion**|**10**|
|**MaxAllowedPhaseOffset**|**300**|
|**MaxNegPhaseCorrection**|**54 000** (15 horas)|
|**MaxPollInterval**|**15**|
|**MaxPosPhaseCorrection**|**54 000** (15 horas)|
|**MinPollInterval**|**10**|
|**PhaseCorrectRate**|**7**|
|**PollAdjustFactor**|**5**|
|**SpikeWatchPeriod**|**90**|
|**UpdateInterval**|**100**|

### <a name="pre-set-values-for-configure-windows-ntp-client-settings"></a>Valores predefinidos para la configuración de "Configurar el cliente NTP de Windows"

En la tabla siguiente se enumeran las opciones de configuración disponibles para el GPO **Configurar el cliente NTP de Windows** y los valores predefinidos asociados al servicio Hora de Windows. Para más información sobre cada configuración, consulta las entradas del registro correspondientes en [Referencia: Entradas del registro del servicio de hora de Windows](#reference-windows-time-service-registry-entries) anteriormente en este artículo.

|Configuración de directiva de grupo|Valor preestablecido|
|------------------------|-----------------|
|**NtpServer**|time.windows.com, **0x1**|
|**Type**|Opciones predeterminadas:<ul><li>**NTP.** Úsala en equipos que no estén unidos a un dominio.</li><li>**NT5DS.** Úsala en equipos que estén unidos a un dominio.</li></ul>|
|**CrossSiteSyncFlags**|**2**|
|**ResolvePeerBackoffMinutes**|**15**|
|**ResolvePeerBackoffMaxTimes**|**7**|
|**SpecialPollInterval**|**3600**|
|**EventLogFlags**|**0**|

## <a name="reference-network-ports-that-the-windows-time-service-uses"></a>Referencia: Puertos de red que usa el servicio hora de Windows

Hora de Windows sigue la especificación NTP, que requiere el uso del puerto UDP 123 para todas las comunicaciones de sincronización de hora. Este puerto está reservado por Hora de Windows y permanece reservado en todo momento. Cada vez que el equipo sincroniza el reloj o proporciona la hora a otro equipo, la comunicación se realiza en el puerto UDP 123.

> [!NOTE]
> Si tienes un equipo con varios adaptadores de red (también denominado equipo de *host múltiple*), no puedes habilitar de forma selectiva el servicio de hora de Windows basado en el adaptador de red.

## <a name="related-information"></a>Información relacionada

Los recursos siguientes contienen información adicional relevante para esta sección.

- RFC *1305* en la base de datos RFC de IETF