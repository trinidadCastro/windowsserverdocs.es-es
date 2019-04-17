---
ms.assetid: 6086947f-f9ef-4e18-9f07-6c7c81d7002c
title: Herramientas de servicio hora de Windows y configuración
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 70b7ee4a9955e023d1664a3c29295a22cd5dc75b
ms.sourcegitcommit: fd6a46b702b235f38d90814dd769106ab37cd61b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
# <a name="windows-time-service-tools-and-settings"></a>Herramientas de servicio hora de Windows y configuración

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


**En esta sección**  
  
-   [Herramientas de servicios de tiempo de Windows](#w2k3tr_times_tools_dyax)  
  
-   [Entradas de registro del servicio de hora de Windows](#w2k3tr_times_tools_uhlp)  
  
-   [Configuración de directiva de grupo de servicio hora de Windows](#w2k3tr_times_tools_vwtt)  
  
-   [Puertos de red usados por el servicio hora de Windows](#w2k3tr_times_tools_suxb)  
  
-   [Información relacionada](#w2k3tr_times_tools_qoep)  
  
> [!NOTE]  
> Este tema contiene información sobre herramientas y opciones de configuración para el servicio hora de Windows (W32Time). Si sólo desea sincronizar la hora de un equipo cliente Unidos a un dominio, consulta [configurar un equipo cliente para la sincronización de hora automática dominio](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29). Para otros temas sobre cómo configurar el servicio hora de Windows, consulta la lista de temas en la sección [dónde encontrar la información de configuración de Windows tiempo servicio](https://technet.microsoft.com/library/cc773061.aspx).  
  
> [!CAUTION]  
> No debes usar el comando Net time para configurar o establecer el tiempo de ejecución cuando el servicio hora de Windows.  

Además, en equipos antiguos que ejecutan Windows XP o versiones anteriores, el comando Net time /querysntp muestra el nombre de un servidor de protocolo de tiempo de red (NTP) con la que un equipo está configurado para sincronizar, pero se usa ese servidor NTP solo cuando el cliente de tiempo del equipo está configurado como NTP o AllSync. Ese comando ya está en desuso.  
  
La mayoría de los equipos de miembro de dominio tienen un tipo de cliente de tiempo de NT5DS, lo que significa que sincronicen la hora de la jerarquía de dominios. La excepción a esto solo típica es el controlador de dominio que funciona como el maestro de operaciones de emulador de dominio principal (PDC) del controlador de dominio de raíz del bosque, que generalmente está configurado para sincronizar la hora con una fuente externa. Para ver la configuración de cliente de tiempo de un equipo, ejecuta el W32tm//query /configuration comando desde un símbolo del sistema con privilegios elevados en a partir de Windows Server 2008 y Windows Vista y leer el **tipo** línea en el resultado del comando. Para obtener más información, consulta [cómo funciona el servicio de Windows tiempo ](https://go.microsoft.com/fwlink/?LinkId=117753). Puedes ejecutar el comando **reg consulta HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters** y leer el valor de **NtpServer** en el resultado del comando.  
  
> [!IMPORTANT]  
> Antes de Windows Server 2016, el servicio W32Time no se diseñó para satisfacer las necesidades de aplicación sujetos a limitación temporal.  Sin embargo, las actualizaciones de Windows Server 2016 ahora te permiten implementar una solución para 1 ms precisión en su dominio.  Consulta [Windows 2016 preciso tiempo](accurate-time.md) y [límite de soporte para configurar el servicio hora de Windows para entornos de alta precisión](https://go.microsoft.com/fwlink/?LinkID=179459) para obtener más información.  
  
## <a name="w2k3tr_times_tools_dyax"></a>Herramientas de servicios de tiempo de Windows  
Las siguientes herramientas están asociadas con el servicio hora de Windows.  
  
#### <a name="w32tmexe-windows-time"></a>W32tm.exe: hora de Windows  
**Categoría**  

Esta herramienta se instala como parte de Windows XP, Windows Vista, Windows 7, Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y las instalaciones predeterminadas de Windows Server 2008 R2.  
  
**Compatibilidad de la versión**  
  
Esta herramienta funciona en Windows XP, Windows Vista, Windows 7, Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y las instalaciones predeterminadas de Windows Server 2008 R2.  
  
W32tm.exe se usa para configurar el servicio hora de Windows. También puede usarse para diagnosticar problemas con el servicio de tiempo. W32tm.exe es la herramienta de línea de comandos preferida para configurar, supervisar o solucionar problemas del servicio hora de Windows.  
  
Las tablas siguientes describen los parámetros que se usan con W32tm.exe.  
  
**Parámetros de W32tm.exe principales**  
  
|Parámetro|Descripción|  
|-------------|---------------|  
|¿W32tm /?|Ayuda de la línea de comandos de W32tm|  
|W32tm//register|Registra el servicio de tiempo para ejecutarse como un servicio y agrega la configuración predeterminada en el registro.|  
|W32tm//unregister|Anula el registro del servicio de hora y quita toda la información de configuración del registro.|  
|w32tm//monitor<br /><br />[/domain:<domain name>] [/computers:<name>[,<name>[,<name>...]]] [/Threads:<num>]|dominio: especifica qué dominio para supervisar. Si no se proporciona ningún nombre de dominio o el dominio ni equipos opción se especifica, se usa el dominio predeterminado. Esta opción puede usarse más de una vez.<br /><br />Equipos - supervisa la lista de equipos determinada. Nombres de equipo están separados por comas, sin espacios. Si tiene un prefijo con un nombre de un ' *', se trata como un PDC. Esta opción puede usarse más de una vez.<br /><br />Los subprocesos: especifica el número de equipos para analizar simultáneamente. El valor predeterminado es 3. Intervalo permitido es 1 al 50.|  
|w32tm /ntte <NT time epoch>|Convertir una hora del sistema NT, en (10 ^ -7) s intervalos desde 0 h 1 de enero de 1601, en un formato legible.|  
|w32tm /ntpte <NTP time epoch>|Convertir un tiempo NTP, en (2 ^ -32) s intervalos desde 0 h 1 - enero de 1900, en un formato legible.|  
|w32tm//resync<br /><br />[/computer:<computer>]<br /><br />[/nowait]<br /><br />[/Rediscover]<br /><br />[/Soft]|Indica que un equipo que debe volver a sincronizar el reloj tan pronto como sea posible, iniciar un vistazo a todas las estadísticas de error acumuladas.<br /><br />equipo:<computer> -especifica el equipo que debe volver a sincronizar. Si no se especifica, se volverá a sincronizar en el equipo local.<br /><br />NOWAIT - no esperar la volver a sincronizar que se produzcan; vuelve inmediatamente. De lo contrario, espere a que el volver a sincronizar en completarse antes de devolverlo.<br /><br />redescubre: detectar la configuración de red y descubrir orígenes de red y después volver a sincronizar.<br /><br />Suave - volver a sincronizar con las estadísticas de error existentes. No es útil, se proporciona para la compatibilidad.|  
|w32tm /stripchart<br /><br />/computer:<target><br /><br />[/Period:<refresh>]<br /><br />[/dataonly]<br /><br />[/Samples:<count>]<br/><br/>[/rdtsc]<br/>|Muestra un gráfico de barras de desplazamiento entre el equipo y otro equipo.<br /><br />equipo:<target> -el equipo para medir el desplazamiento contra.<br /><br />período:<refresh> : el tiempo entre los ejemplos, en segundos. El valor predeterminado es de 2 segundos.<br /><br />DataOnly - mostrar solo los datos sin gráficos.<br /><br />muestras:<count> - recopila <count>muestras, a continuación, se detenga. Si no se especifica, se recopilarán muestras hasta **CTRL+c** está presionado.<br/><br/>RDTSC: para cada muestra, esta opción imprime valores separados por comas junto con los encabezados RdtscStart, RdtscEnd, FileTime, RoundtripDelay, NtpOffset en lugar del gráfico de texto.<br/><ul><li>[RdtscStart – RDTSC (contador de marca de tiempo de lectura)](https://en.wikipedia.org/wiki/Time_Stamp_Counter) valor recopilada justo antes de que se generó la solicitud NTP.</li><li>RdtscEnd: valor RDTSC (contador de marca de tiempo de lectura) recopilado justo después de que recibió y procesó la respuesta NTP.</li><li>FileTime: valor FILETIME Local que se usa en la solicitud NTP.</li><li>RoundtripDelay: el tiempo transcurrido en segundos entre genera la solicitud NTP y procesamiento de la respuesta NTP recibida, calculado según los cálculos de ida y vuelta NTP.</li><li>NTPOffset: desplazamiento en segundos entre el equipo local y el servidor NTP, el tiempo se calcula según los cálculos de desplazamiento NTP.</li></ul>|
|w32tm /config<br /><br />[/computer:<target>]<br /><br />[/Update]<br /><br />[/manualpeerlist:<peers>]<br /><br />[/syncfromflags:<source>]<br /><br />[/LocalClockDispersion:<seconds>]<br /><br />[/Reliable: (Sí & #124; NO)]<br /><br />[/largephaseoffset:<milliseconds>]|equipo:<target> -ajusta la configuración de <target>. Si no se especifica, el valor predeterminado es el equipo local.<br /><br />Update - notifica al servicio de tiempo que ha cambiado la configuración, lo que hace que los cambios surtan efecto.<br /><br />manualpeerlist:<peers> -establece la lista manual del mismo nivel en <peers>, que es una lista delimitada por espacios de direcciones IP o de DNS. Al especificar a varios interlocutores, esta opción debe estar entre comillas.<br /><br />syncfromflags:<source> -establece los orígenes debe sincronizar el cliente NTP desde. <source> Debe ser una lista separada por comas de estas palabras clave (no entre mayúsculas y minúsculas):<br /><br />MANUAL - incluyen sistemas del mismo nivel de la lista manual del mismo nivel.<br /><br />DOMHIER - sincronizar desde un controlador de dominio (corriente continua) en la jerarquía de dominios.<br /><br />LocalClockDispersion:<seconds> -configura la precisión de los internos reloj que supone W32Time cuando no puede adquirir el tiempo que transcurre desde ha configurado orígenes.<br /><br />confiable: (Sí & #124; NO): establece si este equipo es un origen de hora confiable.<br /><br />Esta configuración es solo sean significativa en controladores de dominio.<br /><br />Sí, este equipo es un servicio de hora confiable.<br /><br />NO - este equipo no es un servicio de hora confiable.<br /><br />largephaseoffset:<milliseconds> -establece la diferencia de tiempo entre local y de tiempo que W32Time se considera un pico de red.|  
|w32tm /tz|Mostrar la configuración de zona horaria actual.|  
|w32tm /dumpreg<br /><br />[/Subkey:<key>]<br /><br />[/computer:<target>]|Mostrar los valores asociados con una clave del registro dado.<br /><br />La tecla predeterminada es HKLM\System\CurrentControlSet\Services\W32Time<br /><br />(la clave raíz para el servicio de tiempo).<br /><br />subclave:<key> -muestra los valores asociados con la subclave <key>de la clave de forma predeterminada.<br /><br />equipo:<target> -consulta la configuración del registro para el equipo <target>|  
|w32tm//query [/computer:<target>] {/source & #124; /configuration & #124; /Peers & #124; /status} [/verbose]|Este parámetro se puso disponible en las versiones de cliente de la hora de Windows de Windows Vista y Windows Server 2008.<br /><br />Mostrar información de servicio hora de Windows de un equipo.<br /><br />**equipo:<target>** -consultar la información de **<target>**. Si no se especifica, el valor predeterminado es el equipo local.<br /><br />**Origen** -mostrar el origen de tiempo.<br /><br />**Configuración** -mostrar la configuración de tiempo de ejecución y la configuración de procedencia. En el modo detallado, mostrar el definido o no utilizados configuración demasiado.<br /><br />**sistemas del mismo nivel** -mostrar una lista de sistemas del mismo nivel y su estado.<br /><br />**estado** -estado del servicio hora de Windows de la pantalla.<br /><br />**detallado** -establecer el modo para mostrar información más detallado.|  
|w32tm//debug {/disable & #124; {/Enable /file:<name> /size:<bytes> /entries:<value> [/truncate]}}|Este parámetro se puso disponible en las versiones de cliente de la hora de Windows de Windows Vista y Windows Server 2008.<br /><br />Habilitar o deshabilitar el registro de privada del servicio hora de Windows del equipo local.<br /><br />**deshabilitar** -deshabilitar el registro de privada.<br /><br />**habilitar** -habilitar el registro privado.<br /><br />-   **archivo:<name>** -especifica el nombre de archivo absolutas.<br />-   **tamaño:<bytes>** -especificar el tamaño máximo de registro circular.<br />-   **entradas:<value>** -contiene una lista de indicadores, especificada por el número y separados por comas, que especifica los tipos de información que se va a registrar. Números válidos son de 0a 300. Un intervalo de números es válido, además de números único, como 0-100,103 106. El valor 0-300 es para toda la información de registro.<br /><br />**truncar** -truncar el archivo si existe.|  
  
Para obtener más información sobre **W32tm.exe**, consulta la Ayuda y soporte técnico en Windows XP, Windows Vista, Windows 7, Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y Windows Server 2008 R2.  
  
## <a name="w2k3tr_times_tools_uhlp"></a>Entradas de registro del servicio de hora de Windows  
Las siguientes entradas del registro están asociadas con el servicio hora de Windows.  
  
Esta información se proporciona como una referencia para su uso en la solución de problemas o verificar que se aplica la configuración necesaria. Se recomienda que no modifica directamente el registro a menos que no haya ninguna otra alternativa. Modificaciones en el registro no se validan por el editor del registro o por Windows antes de que se aplican y, como resultado, se pueden almacenar valores incorrectos. Esto puede producir errores irrecuperables en el sistema.  
  
Cuando sea posible, usa la directiva de grupo u otras herramientas de Windows, como Microsoft Management Console (MMC) para realizar tareas en lugar de modificar el registro directamente. Si debes editar el registro, Ten mucho cuidado.  
  
> [!WARNING]  
> Algunos de los valores preestablecidos que están configurados en el sistema administrativas plantilla archivo (System.adm) para la configuración de directiva de grupo (GPO) del objeto es diferente de las entradas del registro de forma predeterminada correspondiente. Si tienes previsto usar un GPO para cualquier configuración de hora de Windows, asegúrate de revisar [valores de valor predeterminado para la configuración de directiva de grupo del servicio hora de Windows son diferentes de las entradas de registro de servicio hora de Windows correspondientes en Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=186066). Este problema se aplica a Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 y Windows Server 2003.  
  
Varias entradas de registro para el servicio hora de Windows son los mismos que la configuración de directiva de grupo del mismo nombre. La configuración de directiva de grupo corresponde a las entradas del registro el mismo nombre que se encuentra en:  
  
>**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\\**

  
En esta ubicación del registro, hay varias claves del registro. La configuración de hora de Windows se almacena en los valores en todas estas claves:
* [Parámetros](#Parameters)
* [Configuración](#Configuration)
* [NtpClient](#NtpClient)
* [NtpServer](#NtpServer)
  
> [!NOTE]  
> Muchos de los valores en la sección W32Time del registro se usan internamente W32Time para almacenar información. Estos valores no se deben cambiar manualmente en cualquier momento. No modifique la configuración de esta sección a menos que está familiarizados con la configuración y está seguro de que el nuevo valor funcionará según lo esperado. Las siguientes entradas del registro se encuentran en:

>>**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\\**  
  
Algunos de los parámetros se almacenan en los pasos de reloj en el registro y otros en segundos. Para convertir la hora de pasos de reloj en segundos:  
  
-   1 minuto = 60s  
  
-   1s = 1000 ms  
  
-   1 ms = 10 000 ciclos de reloj en un sistema de Windows, como se describe en [propiedad DateTime.Ticks ](https://msdn.microsoft.com/en-us/library/system.datetime.ticks.aspx).  
  
Por ejemplo, sería 5 minutos a 5 * 60\ * 1000\ * 10000 = 3000000000 reloj pasos. 

Todas las versiones incluyen Windows 7, Windows 8, Windows 10, Windows Server 2008 y Windows Server 2008 R2, Windows Server 2012, Windows Server 2012R2, Windows Server 2016.  Algunas entradas solo están disponibles en las versiones más recientes de Windows.


#### <a name="Parameters"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\Parameters

|Entrada del registro|Versión|Descripción|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|Todos los|Esta entrada indica que se pueden usar combinaciones de modo no estándar en la sincronización entre los sistemas del mismo nivel. El valor predeterminado de los miembros del dominio es 1. El valor predeterminado de los servidores y clientes independientes es 1.|
|NtpServer|Todos los|Esta entrada especifica una lista delimitada por espacios de sistemas del mismo nivel de que un equipo obtiene las marcas de tiempo, que consta de uno o más nombres DNS o direcciones IP por línea. Cada nombre DNS o la dirección IP que aparece debe ser única. Equipos conectados a un dominio deben sincronizar con un origen de tiempo más fiable, como el reloj de tiempo oficial de Estados Unidos.  <ul><li>0 x 01 SpecialInterval </li><li>0 x 02 UseAsFallbackOnly</li><li>0 x 04 SymmetricActive - para obtener más información acerca de este modo, consulta [servidor horario de Windows: 3,3 modos de funcionamiento ](https://go.microsoft.com/fwlink/?LinkId=208012).</li><li>0 x 08 cliente</li></ul><br />No hay un valor predeterminado de esta entrada del registro en los miembros del dominio. El valor predeterminado en los servidores y clientes independientes es time.windows.com, 0 x 1.<br /><br />Nota: Para obtener más información sobre los servidores de NTP disponibles, consulta [artículo de Microsoft Knowledge Base 262680 - una lista de los servidores de tiempo del Protocolo Simple de tiempo de red (SNTP) que están disponibles en Internet](https://go.microsoft.com/fwlink/?LinkId=186067)|
|ServiceDll|Todos los|Esta entrada es mantenida por W32Time. Contiene datos reservado utilizado por el sistema operativo Windows y los cambios realizados en esta configuración pueden provocar resultados impredecibles. La ubicación predeterminada para este archivo DLL en los miembros del dominio y los clientes independientes y servidores es % windir%\System32\W32Time.dll.  |
|ServiceMain|Todos los|Esta entrada es mantenida por W32Time. Contiene datos reservado utilizado por el sistema operativo Windows y los cambios realizados en esta configuración pueden provocar resultados impredecibles. El valor predeterminado de los miembros del dominio es SvchostEntry_W32Time. El valor predeterminado en los servidores y clientes independientes es SvchostEntry_W32Time.  "|
|Tipo|Todos los|Esta entrada indica que los sistemas del mismo nivel para aceptar la sincronización de:  <ul><li>**NoSync**. El servicio de hora no se sincroniza con otros orígenes.</li><li>**NTP.** Sincroniza el servicio hora de los servidores especificados en la **NtpServer**. Entrada de registro.</li><li>**Nt5DS**. Sincroniza el servicio de la jerarquía de dominios.  </li><li>**AllSync**. El servicio hora de usa todos los mecanismos de sincronización disponibles.  </li></ul>El valor predeterminado de los miembros del dominio es **NT5DS**. El valor predeterminado en los servidores y clientes independientes es **NTP**.   |

#### <a name="Configuration"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\Config

|Entrada del registro|Versión|Descripción|
|------------------------------------|---------------|----------------------------|
|AnnounceFlags|Todos los|Esta entrada controla si este equipo está marcado como un servidor horario confiable. Un equipo no está marcado como confiable a menos que también está marcado como un servidor horario.<br /> -0 x 00 no es un servidor horario  <br /> -servidor horario de 0 x 01 siempre  <br /> -servidor de 0 x 02 hora automática  <br /> -servidor horario confiable de 0 x 04 siempre  <br /> -servidor de 0 x 08 hora confiable automática  <br />El valor predeterminado de los miembros del dominio es 10. El valor predeterminado de los servidores y clientes independientes es 10.|
|EventLogFlags|Todos los|Esta entrada controla los eventos que inicie el servicio de hora.  <br />-Tiempo de accesos directos: 0 x 1  <br />-Cambio de origen: 0 x 2  <br />El valor predeterminado de los miembros del dominio es 2. El valor predeterminado en los servidores y clientes independientes es 2.  |
|FrequencyCorrectRate|Todos los|Esta entrada controla la velocidad a la que se ha corregido el reloj. Si este valor es demasiado pequeño, el reloj no es estable y overcorrects. Si el valor es demasiado grande, el reloj tarda demasiado tiempo a sincronizar. El valor predeterminado de los miembros del dominio es 4. El valor predeterminado en los servidores y clientes independientes es 4.  <br /><br />Ten en cuenta que 0 es un valor válido para la entrada del registro FrequencyCorrectRate. En Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y Windows Server 2008 R2 equipos, si el valor se establece en 0 el servicio hora de Windows cambiará automáticamente se a 1.  |
|HoldPeriod|Todos los|Esta entrada controla el período de tiempo para el que se deshabilita la detección de especial para poner el reloj local en sincronización rápidamente. Un pico es un ejemplo de tiempo que indique ese momento es desactivar un número de segundos y normalmente se recibe después de ejemplos de buen momento se han devuelto de forma coherente. El valor predeterminado de los miembros del dominio es 5. El valor predeterminado en los servidores y clientes independientes es 5.  |
|LargePhaseOffset|Todos los|Esta entrada especifica que uno offset mayor que o igual a este valor en 10<sup>-7</sup> segundos se considera un pico. Una interrupción de red como una gran cantidad de tráfico podría provocar un pico. A menos que persista durante un largo período de tiempo, se omitirá un pico. El valor predeterminado de los miembros del dominio es 50000000. El valor predeterminado en los servidores y clientes independientes es 50000000.  |
|LastClockRate|Todos los|Esta entrada es mantenida por W32Time. Contiene datos reservado utilizado por el sistema operativo Windows y los cambios realizados en esta configuración pueden provocar resultados impredecibles. El valor predeterminado de los miembros del dominio es 156250. El valor predeterminado en los servidores y clientes independientes es 156250.  |
|LocalClockDispersion|Todos los|Esta entrada controla la dispersión (en segundos) que debe asumir cuando la única vez origen es el reloj CMOS integrado. El valor predeterminado de los miembros del dominio es 10. El valor predeterminado en los servidores y clientes independientes es 10.|
|MaxAllowedPhaseOffset|Todos los|Esta entrada especifica el desplazamiento máximo (en segundos) que W32Time intenta ajustar el reloj del equipo mediante el uso de la velocidad del reloj. Cuando el desplazamiento supera esta velocidad, W32Time establece directamente el reloj del equipo. El valor predeterminado de los miembros del dominio es 300. El valor predeterminado de los servidores y clientes independientes es 1.  [Consulta a continuación para obtener más información](#MaxAllowedPhaseOffset).|
|MaxClockRate|Todos los|Esta entrada es mantenida por W32Time. Contiene datos reservado utilizado por el sistema operativo Windows y los cambios realizados en esta configuración pueden provocar resultados impredecibles. El valor predeterminado de los miembros del dominio es 155860. El valor predeterminado de los servidores y clientes independientes es 155860.  |
|MaxNegPhaseCorrection|Todos los|Esta entrada especifica la corrección tiempo negativo más grande en segundos, que hace que el servicio. Si el servicio determina que se requiere un cambio más grande, registra un evento. Caso especial: 0xFFFFFFFF significa que siempre se hace tiempo corrección. El valor predeterminado de los miembros del dominio es 0xFFFFFFFF. El valor predeterminado de los servidores y clientes independientes es 54.000 (15 horas).  |
|MaxPollInterval|Todos los|Esta entrada especifica el intervalo mayor, en segundos, necesario log2 permitidos para el intervalo de sondeo del sistema. Ten en cuenta que, mientras que un sistema debe sondear según el intervalo programado, un proveedor puede rechazar a producir muestras cuando lo solicite. El valor predeterminado de los controladores de dominio es 10. El valor predeterminado de los miembros del dominio es 15. El valor predeterminado de los servidores y clientes independientes es 15.  |
|MaxPosPhaseCorrection|Todos los|Esta entrada especifica la corrección de hora positiva más grande en segundos, que hace que el servicio. Si el servicio determina que se requiere un cambio más grande, registra un evento. Caso especial: 0xFFFFFFFF significa que siempre se hace tiempo corrección. El valor predeterminado de los miembros del dominio es 0xFFFFFFFF. El valor predeterminado de los servidores y clientes independientes es 54.000 (15 horas).  |
|MinClockRate|Todos los|Esta entrada es mantenida por W32Time. Contiene datos reservado utilizado por el sistema operativo Windows y los cambios realizados en esta configuración pueden provocar resultados impredecibles. El valor predeterminado de los miembros del dominio es 155860. El valor predeterminado de los servidores y clientes independientes es 155860.  |
|MinPollInterval|Todos los|Esta entrada especifica el intervalo mínimo, en segundos, necesario log2 permitidos para el intervalo de sondeo del sistema. Ten en cuenta que mientras un sistema con más frecuencia que esto no solicitar muestras, un proveedor puede producir muestras no solo en el intervalo programado. El valor predeterminado de los controladores de dominio es 6. El valor predeterminado de los miembros del dominio es 10. El valor predeterminado de los servidores y clientes independientes es 10.  |
|PhaseCorrectRate|Todos los|Esta entrada controla la velocidad a la que se corrige el error de fase. Especifica un valor pequeño corrige el error de fase rápidamente, pero puede provocar que el reloj se vuelva inestable. Si el valor es demasiado grande, necesita más tiempo para corregir el error de fase. <br /><br />El valor predeterminado de los miembros del dominio es 1. El valor predeterminado en los servidores y clientes independientes es 7.<br /><br />Nota: 0 es un valor no válido para la entrada del registro PhaseCorrectRate. En Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y Windows Server 2008 R2 equipos, si el valor se establece en 0, el servicio hora de Windows lo cambiará automáticamente a 1.  |
|PollAdjustFactor|Todos los|Esta entrada controla la decisión para aumentar o disminuir el intervalo de sondeo para el sistema. Cuanto mayor sea el valor, menor será la cantidad de error que hace que el intervalo de sondeo disminuir. El valor predeterminado de los miembros del dominio es 5. El valor predeterminado en los servidores y clientes independientes es 5. |
|SpikeWatchPeriod|Todos los|Esta entrada especifica la cantidad de tiempo que debe conservar un desplazamiento sospechoso antes de ser aceptada como correcto (en segundos). El valor predeterminado de los miembros del dominio es 900. El valor predeterminado de los clientes independientes y estaciones de trabajo es 900.  |
|TimeJumpAuditOffset|Todos los|Un entero sin signo que indica el umbral de auditoría de accesos directos de tiempo, en segundos. Si el servicio hora de ajusta el reloj local estableciendo directamente el reloj y la corrección de hora es mucho más que este valor, el servicio hora de registra un evento de auditoría.|
|UpdateInterval|Todos los|Esta entrada especifica el número de pasos de reloj entre los ajustes de corrección de fase. El valor predeterminado de los controladores de dominio es 100. El valor predeterminado de los miembros del dominio es 30.000. El valor predeterminado de los servidores y clientes independientes es 360,000.  <br /><br />**Ten en cuenta**: cero es un valor válido para la entrada del registro UpdateInterval. En equipos que ejecutan Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y Windows Server 2008 R2, si el valor se establece en 0 el servicio hora de Windows lo cambiará automáticamente a 1.<br /><br />Las siguientes entradas del tres registro no forman parte de la configuración predeterminada de W32Time pero pueden agregarse en el registro para obtener las funcionalidades de aumento del registro. La información registrada en el registro de eventos del sistema puede modificarse cambiando el valor de la configuración de EventLogFlags en el Editor de objetos de directiva de grupo. De manera predeterminada, el servicio hora de crea un registro en el Visor de eventos cada vez que TI cambia a un nuevo origen de tiempo.<br /><br />**ADVERTENCIA**: algunos de los valores preestablecidos que están configurados en el sistema administrativas plantilla archivo (System.adm) para la configuración de directiva de grupo (GPO) del objeto es distinta de los correspondientes predeterminado entradas del registro. Si tienes previsto usar un GPO para cualquier configuración de hora de Windows, asegúrate de revisar [valores de valor predeterminado para la configuración de directiva de grupo del servicio hora de Windows son diferentes de las entradas de registro de servicio hora de Windows correspondientes en Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=186066). Este problema se aplica a Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 y Windows Server 2003. |
|UtilizeSslTimeData|Publicar en Windows 10 versión 1511|Entrada de 1 indica que la W32Time podrán usar varias marcas de tiempo SSL para inicializar un reloj que es muy poco precisa.|

Deben agregarse las siguientes entradas del registro para habilitar el registro de W32Time:  

|Entrada del registro|Versión|Descripción|
|------------------------------------|---------------|----------------------------|
|FileLogEntries|Todos los|Esta entrada controla la cantidad de entradas que creaste en el archivo de registro de la hora de Windows. El valor predeterminado es ninguno, lo que no registra cualquier actividad de la hora de Windows. Los valores válidos son de 0a 300. Este valor no afecta a las entradas de registro de eventos creadas normalmente por hora de Windows|
|FileLogName|Todos los|Esta entrada controla la ubicación y el nombre de archivo de registro de la hora de Windows. El valor predeterminado está en blanco y no se puede cambiar a menos que **FileLogEntries** ha cambiado. Un valor válido es una ruta de acceso completa y el nombre de archivo que se usará el tiempo de Windows para crear el archivo de registro. Este valor no afecta a las entradas de registro de eventos creadas normalmente por hora de Windows.  |
|FileLogSize|Todos los|Esta entrada controla el comportamiento del registro circular de archivos de registro de la hora de Windows. Cuando **FileLogEntries** y **FileLogName** están definidas, esta entrada define el tamaño, en bytes, para permitir que el archivo de registro llegar a antes de sobrescribir las entradas de registro más antiguas con entradas nuevas. Cualquier número positivo es válido y se recomienda 3000000. Este valor no afecta a las entradas de registro de eventos creadas normalmente por hora de Windows.  |



#### <a name="NtpClient"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient

|Entrada del registro|Versión|Descripción|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|Todos los|Esta entrada indica que se pueden usar combinaciones de modo no estándar en la sincronización entre los sistemas del mismo nivel. El valor predeterminado de los miembros del dominio es 1. El valor predeterminado de los servidores y clientes independientes es 1.|
|CompatibilityFlags|Todos los|Esta entrada especifica las siguientes marcas de compatibilidad y los valores: <br /><br />-DispersionInvalid: 0 x 00000001  <br />-IgnoreFutureRefTimeStamp: 0 x 00000002  <br /> -AutodetectWin2K: 0 x 80000000  <br />-AutodetectWin2KStage2: 0 x 40000000  <br /><br />El valor predeterminado de los miembros del dominio es 0 x 80000000. El valor predeterminado de los servidores y clientes independientes es 0 x 80000000.  |
|CrossSiteSyncFlags|Todos los|Esta entrada determina si el servicio elige asociados de sincronización fuera del dominio del equipo. Las opciones y los valores son:  <br /><br />-Ninguno: 0  <br />-PdcOnly: 1  <br />-Todo: 2  <br /><br />Este valor se omite si no se establece el valor NT5DS. El valor predeterminado de los miembros del dominio es 2. El valor predeterminado de los servidores y clientes independientes es 2.  |
|DllName|Todos los|Esta entrada especifica la ubicación de la DLL del proveedor de tiempo.  <br /><br />La ubicación predeterminada para este archivo DLL en los miembros del dominio y los clientes independientes y servidores es % windir%\System32\W32Time.dll.|
|Habilitado|Todos los|Esta entrada indica si el proveedor NtpClient está habilitado en el servicio de la hora actual.  <br /><ul><li>Sí 1</li><li>No 0</li></ul>El valor predeterminado de los miembros del dominio es 1. El valor predeterminado en los servidores y clientes independientes es 1.|
|EventLogFlags|Todos los|Esta entrada especifica los eventos registrados por el servicio hora de Windows.<ul><li>Cambios de accesibilidad de 0 x 1</li><li>0 x 2 muestra grandes sesgo (Esto es aplicable a Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y Windows Server 2008 R2 solo)</li></ul>El valor predeterminado de los miembros del dominio es 0 x 1. El valor predeterminado en los servidores y clientes independientes es 0 x 1.|
|InputProvider|Todos los|Esta entrada indica si el proveedor de NtpClient está habilitado.  <ul><li>Sí 1  </li><li>No 0 </li></ul>El valor predeterminado de los miembros del dominio es 1. El valor predeterminado en los servidores y clientes independientes es 1.  |
|LargeSampleSkew|Todos los|Esta entrada especifica el sesgo muestra grandes para iniciar sesión en segundos. Para cumplir con las especificaciones de seguridad y la Comisión de Exchange (s), esto debe establecerse en tres segundos. Se registrará eventos para esta configuración solo cuando EventLogFlags se configura explícitamente para 0 x 2 muestra grandes sesgo. El valor predeterminado de los miembros del dominio es 3. El valor predeterminado en los servidores y clientes independientes es 3.  |
|ResolvePeerBackOffMaxTimes|Todos los|Esta entrada especifica el número máximo de veces para duplicar el intervalo de espera cuando repite intenta localizar un sistema del mismo nivel para sincronizar con error. Un valor de cero significa que el intervalo de espera es siempre el mínimo. El valor predeterminado de los miembros del dominio es 7. El valor predeterminado en los servidores y clientes independientes es 7. |
|ResolvePeerBackoffMinutes|Todos los|Esta entrada especifica el intervalo inicial de espera, en minutos, antes de intentar localizar un sistema del mismo nivel para sincronizar con. El valor predeterminado de los miembros del dominio es 15. El valor predeterminado en los servidores y clientes independientes es 15.  |
|SpecialPollInterval|Todos los|Esta entrada especifica el intervalo de sondeo especial en segundos manual dispositivos del mismo nivel. Cuando se habilita la marca SpecialInterval 0 x 1, W32Time usa este intervalo de sondeo en lugar de un intervalo de sondeo determina el sistema operativo. El valor predeterminado de los miembros del dominio es 3.600. El valor predeterminado en los servidores y clientes independientes es 604.800.<br/><br/>Nuevo para la compilación 1702, SpecialPollInterval contiene los valores del registro MinPollInterval y MaxPollInterval Config.|
|SpecialPollTimeRemaining|Todos los|Esta entrada es mantenida por W32Time. Contiene datos reservado utilizado por el sistema operativo Windows. Especifica el tiempo en segundos antes de que se volverá a sincronizar W32Time tras reiniciar el equipo. Los cambios realizados en esta configuración pueden provocar resultados impredecibles. El valor predeterminado en los miembros del dominio y en los servidores y clientes independientes se deja en blanco.  |

#### <a name="NtpServer"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer

|Entrada del registro|Versión|Descripción|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|Todos los|Esta entrada indica que se permiten combinaciones de modo no estándar en la sincronización entre clientes y servidores. El valor predeterminado de los miembros del dominio es 1. El valor predeterminado de los servidores y clientes independientes es 1.|
|DllName|Todos los|Esta entrada especifica la ubicación de la DLL del proveedor de tiempo.<br /><br />La ubicación predeterminada para este archivo DLL en los miembros del dominio y los clientes independientes y servidores es % windir%\System32\W32Time.dll.  |
|Habilitado|Todos los|Esta entrada indica si el proveedor de NtpServer está habilitado en el servicio de la hora actual. <ul><li>Sí 1</li><li>No 0</li></ul>El valor predeterminado de los miembros del dominio es 1. El valor predeterminado en los servidores y clientes independientes es 1.  |
|InputProvider|Todos los|Esta entrada indica si el proveedor de NtpServer está habilitado.  <ul><li>Sí 1  </li><li>No 0 </li></ul>El valor predeterminado de los miembros del dominio es 1. El valor predeterminado en los servidores y clientes independientes es 1.  |

#### <a name="MaxAllowedPhaseOffset"></a>Información de MaxAllowedPhaseOffset
Para W32Time establecer el reloj del equipo de forma gradual, el desplazamiento debe ser menor que el valor de MaxAllowedPhaseOffset y cumplan con la siguiente ecuación al mismo tiempo:  
```  
|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2  
``` 
La CurrentTimeOffset se mide en ciclos de reloj, donde 1 ms = 10000 marcas en un sistema Windows del reloj.  
  
SystemClockRate y PhaseCorrectRate también se miden en ciclos de reloj. Para obtener la SystemClockRate, puedes usar el siguiente comando y convertirlo de segundos para que los relojes de los pasos con la fórmula de segundos * 1000\ * 10000:  
  
```  
W32tm /query /status /verbose  
ClockRate: 0.0156000s  
```  
  
SystemclockRate es la velocidad del reloj del sistema. Con 156000 segundos como ejemplo, el SystemclockRate podría ser = 0.0156000 * 1000 \ * 10000 = 156000 reloj pasos.  
  
También es MaxAllowedPhaseOffset en segundos. Para convertir para que los relojes de los pasos, multiplica MaxAllowedPhaseOffset * 1000\ * 10000.  
  
Los dos ejemplos siguientes muestran cómo aplicar  
  
**Ejemplo 1**: hora difiere de 4 minutos (por ejemplo, el tiempo es 11:05 AM y la muestra de tiempo recibido de un sistema del mismo nivel y cree que correcto es 11:09A.M.).  
```
phasecorrectRate = 1  
  
UpdateInterval = 30000 (clock ticks)  
  
systemclockRate = 156000 (clock ticks)  
  
MaxAllowedPhaseOffset = 10min = 600 seconds = 600*1000\*10000=6000000000 clock ticks  
  
|currentTimeOffset| = 4mins = 4*60\*1000\*10000 = 2400000000 ticks  
  
Is CurrentTimeOffset < MaxAllowedPhaseOffset?  
  
2400000000 < 6000000000 = TRUE  
```
¿Y cumplan con la ecuación anterior? 
```
(|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2)  
  
Is 2,400,000,000 / (30000*1) < 156000/2  
  
Is 100,000 < 78,000  
  
NO/FALSE  
```  
Por lo tanto W32tm establecería el reloj vuelve inmediatamente.  
  
> [!NOTE]  
> En este caso, si quieres configurar el reloj atrás lentamente, necesitará ajustar los valores de PhaseCorrectRate o updateInterval en el registro así como garantizar los resultados de la ecuación en "true".  
  
**Ejemplo 2**: hora difiere de 3 minutos.  
```  
phasecorrectRate = 1  
  
UpdateInterval = 30000 (clock ticks)  
  
systemclockRate = 156000 (clock ticks)  
  
MaxAllowedPhaseOffset = 10min = 600 seconds = 600*1000\*10000=6000000000 clock ticks  
  
currentTimeOffset = 3mins = 3*60\*1000\*10000 = 1800000000 clock ticks  
  
Is CurrentTimeOffset < MaxAllowedPhaseOffset?  
  
1800000000 < 6000000000 = TRUE  
```  
¿Y cumplan con la ecuación anterior?
```
(|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2)  
  
Is 3 mins (1,800,000,000) / (30000*1) < 156000/2  
  
Is 60,000 < 78,000  
  
YES/TRUE  
```  
En este caso el reloj se establecerá atrás lentamente.  
  
## <a name="w2k3tr_times_tools_vwtt"></a>Configuración de directiva de grupo de servicio hora de Windows  
Puede configurar la mayoría de los parámetros de W32Time mediante el Editor de objetos de directiva de grupo. Esto incluye la configuración de un equipo para que sea una NTPServer o NTPClient, configurar el mecanismo de sincronización de hora y configurar un equipo para ser un origen de hora confiable.  
  
> [!NOTE]  
> Configuración de directiva de grupo para el servicio hora de Windows puede configurarse en Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y Windows Server 2008 R2 controladores de dominio y se puede aplicar solo a equipos que ejecutan Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y Windows Server 2008 R2.  
  
Puedes encontrar la directiva de grupo de configuración se usa para configurar W32Time en el complemento Editor de objetos de directiva de grupo en las siguientes ubicaciones:  
  
-   Servicio hora de equipo equipo\Plantillas Templates\System\Windows  
  
    Configurar **opciones de configuración Global** aquí.  
  
-   Equipo equipo\Plantillas Templates\System\Windows tiempo Service\Time proveedores  
  
    Configurar **cliente de Windows NTP** configuración aquí.  
  
    Habilitar **cliente de Windows NTP** aquí.  
  
    Habilitar **Windows NTP Server** aquí.  
  
> [!WARNING]  
> Algunos de los valores preestablecidos que están configurados en el sistema administrativas plantilla archivo (System.adm) para la configuración de directiva de grupo (GPO) del objeto es diferente de las entradas del registro de forma predeterminada correspondiente. Si tienes previsto usar un GPO para cualquier configuración de hora de Windows, asegúrate de revisar [valores de valor predeterminado para la configuración de directiva de grupo del servicio hora de Windows son diferentes de las entradas de registro de servicio hora de Windows correspondientes en Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=186066). Este problema se aplica a Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 y Windows Server 2003.  
  
La siguiente tabla enumera la configuración global de directiva de grupo que está asociada con el servicio hora de Windows y el valor predeterminado asociado a cada configuración. Para obtener más información sobre cada opción, consulta las entradas del registro correspondientes en "[entradas de registro de servicio de Windows hora](#w2k3tr_times_tools_uhlp)" anteriormente en este tema. La siguiente configuración se encuentra en un único GPO llamado **opciones de configuración Global**.  
  
**Configuración de directiva de grupo global con asociados a la hora de Windows**  
  
|Configuración de directiva de grupo|Valor predeterminado|  
|------------------------|------------------|  
|AnnounceFlags|10|  
|EventLogFlags|2|  
|FrequencyCorrectRate|4|  
|HoldPeriod|5|  
|LargePhaseOffset|1280000|  
|LocalClockDispersion|10|  
|MaxAllowedPhaseOffset|300|  
|MaxNegPhaseCorrection|54.000 (15 horas)|  
|MaxPollInterval|15|  
|MaxPosPhaseCorrection|54.000 (15 horas)|  
|MinPollInterval|10|  
|PhaseCorrectRate|7|  
|PollAdjustFactor|5|  
|SpikeWatchPeriod|90|  
|UpdateInterval|100|  
  
La siguiente tabla enumera las opciones de configuración disponibles para el **configurar el cliente de Windows NTP** GPO y los valores preestablecidos que están asociados con el servicio hora de Windows. Para obtener más información sobre cada opción, consulta las entradas del registro correspondientes en "[entradas de registro de servicio de Windows hora](#w2k3tr_times_tools_uhlp)" anteriormente en este tema.  
  
**Configuración de directiva de grupo de cliente de NTP asociada a la hora de Windows**  
  
|Configuración de directiva de grupo|Valor predeterminado|  
|------------------------|-----------------|  
|NtpServer|time.windows.com, 0 x 1|  
|Tipo|Opciones predeterminadas:<br /><br />-   **NTP.** Se usa en los equipos que no están unidos a un dominio.<br />-   **NT5DS.** Se usa en los equipos que están unidos a un dominio.|  
|CrossSiteSyncFlags|2|  
|ResolvePeerBackoffMinutes|15|  
|ResolvePeerBackoffMaxTimes|7|  
|SpecialPollInterval|3600|  
|EventLogFlags|0|  
  
## <a name="w2k3tr_times_tools_suxb"></a>Puertos de red usados por el servicio hora de Windows  
Hora de Windows sigue la especificación de NTP, lo que requiere el uso de puerto UDP 123 para todas las comunicaciones de sincronización de hora. Este puerto está reservado por hora de Windows y permanece reservado en todo momento. Siempre que el equipo sincroniza su reloj o proporciona tiempo a otro equipo, dicha comunicación se realiza en el puerto UDP 123.  
  
> [!NOTE]  
> Si tienes un equipo con varios adaptadores de red (también llamados un equipo host múltiple), no se puede habilitar selectivamente el servicio basado en el adaptador de red.  
  
## <a name="w2k3tr_times_tools_qoep"></a>Información relacionada  
Los siguientes recursos contienen información adicional que sea relevante para esta sección.  
  
-   RFC *1305* en la base de datos de IETF RFC  

