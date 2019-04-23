---
ms.assetid: 6086947f-f9ef-4e18-9f07-6c7c81d7002c
title: Configuración y herramientas del servicio Hora de Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 10/16/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 7cf3b3f2bb9a2c9f95c50aa6a7b7690f89cdd0af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840666"
---
# <a name="windows-time-service-tools-and-settings"></a>Configuración y herramientas del servicio Hora de Windows
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o posterior

En este tema, obtendrá información sobre herramientas y configuración de servicio de hora de Windows (W32Time). 

Si sólo desea sincronizar la hora de un equipo cliente unido al dominio, consulte [configurar un equipo cliente para la sincronización de hora automática de dominios](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29). Para obtener temas adicionales sobre cómo configurar el servicio de hora de Windows, consulte [dónde encontrar la información de configuración de Windows tiempo Service](https://docs.microsoft.com/windows-server/networking/windows-time-service/windows-time-service-top).  
  
>[!CAUTION]  
>No debe usar el comando Net time para configurar o establecer el tiempo cuando se ejecuta el servicio de hora de Windows.  
>
>Además, en equipos más antiguos que ejecutan Windows XP o anterior, el /querysntp hora Net comando muestra el nombre de un servidor de protocolo de tiempo de red (NTP) con el que un equipo está configurado para sincronizar, pero ese servidor NTP solo se usa cuando es cliente de hora del equipo configurar como NTP o AllSync. En desuso ya que ese comando.  
  
La mayoría de los equipos de miembros de dominio tienen un tipo de cliente de la hora de NT5DS, lo que significa que sincronicen la hora de la jerarquía de dominios. La excepción a esto sólo típica es el controlador de dominio que funciona como el maestro de operaciones de emulador PDC (controlador) de dominio principal del dominio de raíz del bosque, que normalmente está configurado para sincronizar la hora con un origen de hora externo. Para ver la configuración de cliente de tiempo de un equipo, ejecute el comando de W32tm /query /configuration desde un símbolo del sistema con privilegios elevados en a partir de Windows Server 2008 y Windows Vista y leer el **tipo** línea en la salida del comando. Para obtener más información, consulte [cómo funciona el servicio de Windows tiempo](https://docs.microsoft.com/windows-server/networking/windows-time-service/How-the-Windows-Time-Service-Works). Puede ejecutar el comando **reg query HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters** y leer el valor de **NtpServer** en la salida del comando.  
  
> [!IMPORTANT]  
> Antes de Windows Server 2016, el servicio W32Time no se diseñó para satisfacer las necesidades de la aplicación dependiente del tiempo.  Sin embargo, las actualizaciones de Windows Server 2016 ahora le permiten implementar una solución de 1 ms precisión en el dominio.  Consulte [Windows 2016 preciso momento](accurate-time.md) y [límite de soporte técnico para configurar el servicio de hora de Windows para entornos de alta precisión](https://docs.microsoft.com/windows-server/networking/windows-time-service/support-boundary) para obtener más información.  
  
## <a name="windows-time-service-tools"></a>Herramientas del servicio de hora de Windows  
Las siguientes herramientas están asociadas con el servicio de hora de Windows.  
  
#### <a name="w32tmexe-windows-time"></a>W32tm.exe: Hora de Windows  
**Categoría**  

Esta herramienta se instala como parte de Windows XP, Windows Vista, Windows 7, Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y las instalaciones predeterminadas de Windows Server 2008 R2.  
  
**Compatibilidad de versiones**  
  
Esta herramienta funciona en Windows XP, Windows Vista, Windows 7, Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y las instalaciones predeterminadas de Windows Server 2008 R2.  
  
W32tm.exe sirve para configurar las opciones de servicio de hora de Windows. También puede utilizarse para diagnosticar problemas con el servicio de hora. W32tm.exe es la herramienta de línea de comandos preferida para la configuración, supervisión o el servicio de hora de Windows de solución de problemas.  
  
Las tablas siguientes describen los parámetros que se usan con W32tm.exe.  
  
**Parámetros de W32tm.exe principal**  
  
|Parámetro|Descripción|  
|-------------|---------------|  
|W32tm /?|Ayuda de línea de comandos de W32tm|  
|W32tm /register|Registra el servicio de tiempo para ejecutarse como un servicio y agrega la configuración predeterminada en el registro.|  
|W32tm / anular el registro|Anula el registro del servicio de hora y quita toda la información de configuración del registro.|  
|w32tm /monitor<br /><br />[/ domain:<domain name>] [/ equipos:<name>[,<name>[,<name>...]]] [/ subprocesos:<num>]|dominio: especifica qué dominio para supervisar. Si se proporciona ningún nombre de dominio o el dominio ni equipos opción se especifica, se usa el dominio predeterminado. Esta opción se puede usar más de una vez.<br /><br />equipos - supervisa la lista especificada de equipos. Los nombres de equipo están separados por comas, sin espacios en blanco. Si va precedido de un nombre de un ' *', se trata como un PDC. Esta opción se puede usar más de una vez.<br /><br />subprocesos: especifica el número de equipos para analizar de forma simultánea. El valor predeterminado es 3. Intervalo permitido es 1-50.|  
|w32tm /ntte <NT time epoch>|Convertir una hora del sistema NT, en (10 ^ -7) s intervalos de 0, h 1 de enero de 1601, en un formato legible.|  
|w32tm /ntpte <NTP time epoch>|Convertir una hora de NTP en (2 ^ -32) s intervalos de 0, h 1 - enero de 1900, en un formato legible.|  
|w32tm /resync<br /><br />[/ equipo:<computer>]<br /><br />[/nowait]<br /><br />[/rediscover]<br /><br />[/soft]|Indica que un equipo que debe volver a sincronizar su reloj tan pronto como sea posible, lanzar todas las estadísticas de error acumuladas.<br /><br />equipo:<computer> : especifica el equipo que debe volver a sincronizar. Si no se especifica, se volverá a sincronizar el equipo local.<br /><br />NOWAIT - no esperan que la resincronización se produzcan; devolver inmediatamente. De lo contrario, espere la resincronización se complete antes de devolver.<br /><br />volver a detectar: detectar la configuración de red y volver a detectar los orígenes de red y luego volver a sincronizar.<br /><br />Soft - volver a sincronizar con las estadísticas de error existentes. No es útil, proporcionado por compatibilidad.|  
|w32tm /stripchart<br /><br />/computer:<target><br /><br />[/ período:<refresh>]<br /><br />[/dataonly]<br /><br />[/ samples:<count>]<br/><br/>[/rdtsc]<br/>|Mostrar un gráfico de barras de desplazamiento entre este equipo y el otro equipo.<br /><br />equipo:<target> -el equipo para que se comparará la diferencia.<br /><br />período:<refresh> : el tiempo transcurrido entre muestras, en segundos. El valor predeterminado es de 2 segundos.<br /><br />DataOnly - mostrar solo los datos sin gráficos.<br /><br />ejemplos:<count> : recopile <count> ejemplos, a continuación, detenga. Si no se especifica, se van a recopilar muestras hasta **Ctrl + C** está presionado.<br/><br/>RDTSC: para cada ejemplo, esta opción imprime los valores separados por comas, junto con los encabezados RdtscStart, RdtscEnd, FileTime, RoundtripDelay, NtpOffset en lugar del gráfico de texto.<br/><ul><li>[RdtscStart – RDTSC (contador de marca de tiempo de lectura)](https://en.wikipedia.org/wiki/Time_Stamp_Counter) valor recopila justo antes de que se generó la solicitud NTP.</li><li>RdtscEnd: valor RDTSC (contador de marca de tiempo de lectura) recopilado justo después de que se recibe y procesa la respuesta NTP.</li><li>FileTime: valor FILETIME Local usado en la solicitud NTP.</li><li>RoundtripDelay: tiempo transcurrido en segundos entre la generación de la solicitud NTP y procesar la respuesta recibida de NTP, calculado según los cálculos de ida y vuelta NTP.</li><li>NTPOffset: tiempo de desplazamiento en segundos entre el equipo local y el servidor NTP, se calcula según los cálculos de desplazamiento de NTP.</li></ul>|
|w32tm /config<br /><br />[/ equipo:<target>]<br /><br />[/update]<br /><br />[/manualpeerlist:<peers>]<br /><br />[/syncfromflags:<source>]<br /><br />[/LocalClockDispersion:<seconds>]<br /><br />[/ reliable: (Sí&#124;n)]<br /><br />[/ largephaseoffset:<milliseconds>]|equipo:<target> -ajusta la configuración de <target>. Si no se especifica, el valor predeterminado es el equipo local.<br /><br />Update - notifica al servicio de tiempo que ha cambiado la configuración, causando los cambios surtan efecto.<br /><br />manualpeerlist:<peers> -establece la lista manual del mismo nivel en <peers>, que es una lista delimitada por espacios de direcciones DNS o IP. Al especificar varios elementos del mismo nivel, esta opción debe ir entre comillas.<br /><br />syncfromflags:<source> -establece los orígenes que el cliente NTP debe sincronizarse desde. <source> debe ser una lista separada por comas de estas palabras clave (no distingue mayúsculas de minúsculas):<br /><br />MANUAL - incluyen elementos del mismo nivel en la lista del manual del mismo nivel.<br /><br />DOMHIER - sincronizar desde un controlador de dominio (DC) en la jerarquía de dominios.<br /><br />LocalClockDispersion:<seconds> -configura la precisión del reloj interno que W32Time supondrá al tiempo no puede adquirir de sus orígenes configurados.<br /><br />confiable: (Sí&#124;n): configura si este equipo es un origen de hora confiable.<br /><br />Esta configuración solo es significativa en los controladores de dominio.<br /><br />Sí: este equipo es un servicio de hora confiable.<br /><br />NO, este equipo no es un servicio de hora confiable.<br /><br />largephaseoffset:<milliseconds> -establece la diferencia horaria entre local y el tiempo que W32Time considerará un pico de red.|  
|w32tm /tz|Mostrar la configuración de zona horaria actual.|  
|w32tm /dumpreg<br /><br />[/ subclave:<key>]<br /><br />[/ equipo:<target>]|Mostrar los valores asociados con una clave del Registro determinada.<br /><br />La clave predeterminada es HKLM\System\CurrentControlSet\Services\W32Time<br /><br />(la clave raíz para el servicio de hora).<br /><br />subclave:<key> -muestra los valores asociados a la subclave <key> de la clave predeterminada.<br /><br />equipo:<target> -configuración del registro para el equipo de consulta <target>|  
|w32tm /query [/ equipo:<target>] {/ source &#124; /configuration &#124; /homólogos &#124; /Status} [/ verbose]|Este parámetro disponible por primera vez en las versiones de cliente de la hora de Windows de Windows Vista y Windows Server 2008.<br /><br />Mostrar información de servicio de hora de Windows de un equipo.<br /><br />**equipo:<target>**  -consultar la información de **<target>**. Si no se especifica, el valor predeterminado es el equipo local.<br /><br />**Origen** -mostrar el origen de hora.<br /><br />**Configuración** -mostrar la configuración del tiempo de ejecución y de dónde procede la configuración. En el modo detallado, mostrar la indefinido o sin usar configuración demasiado.<br /><br />**elementos del mismo nivel** -mostrar una lista de elementos del mismo nivel y su estado.<br /><br />**estado** -estado del servicio de hora de Windows para mostrar.<br /><br />**detallado** -establecer el modo detallado para mostrar más información.|  
|w32tm /debug {/disable &#124; {/enable /file:<name> /size:<bytes> /entries:<value> [/truncate]}}|Este parámetro disponible por primera vez en las versiones de cliente de la hora de Windows de Windows Vista y Windows Server 2008.<br /><br />Habilitar o deshabilitar el registro privado del servicio de hora de Windows del equipo local.<br /><br />**deshabilitar** -deshabilitar el registro privado.<br /><br />**habilitar** -habilitar el registro privado.<br /><br />-   **archivo:<name>**  -especifique el nombre de archivo absoluta.<br />-   **tamaño:<bytes>**  -especifique el tamaño máximo para el registro circular.<br />-   **entradas:<value>**  -contiene una lista de marcas, especificado por el número y separados por comas, que especifican los tipos de información que se deben registrar. Números válidos son 0 y 300. Un intervalo de números es válido, además de números únicos, como 0-100,103 106. El valor 0-300 es para toda la información de registro.<br /><br />**truncar** -truncar el archivo si existe.|  

---  
Para obtener más información acerca de **W32tm.exe**, consulte Ayuda y soporte técnico de Windows XP, Windows Vista, Windows 7, Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y Windows Server 2008 R2.  
  
## <a name="windows-time-service-registry-entries"></a>Entradas de registro del servicio de hora de Windows  
Las siguientes entradas del registro asociadas con el servicio de hora de Windows.  
  
Esta información se proporciona como referencia para su uso en la solución de problemas o comprobando que se aplica la configuración necesaria. Se recomienda que no editar directamente el registro a menos que haya ninguna otra alternativa. Las modificaciones del registro no se validan mediante el editor del registro o mediante Windows antes de que se aplican, y como resultado, se pueden almacenar valores incorrectos. Esto puede dar lugar a errores irrecuperables en el sistema.  
  
Cuando sea posible, usar Directiva de grupo u otras herramientas de Windows, como Microsoft Management Console (MMC), para realizar tareas en lugar de modificar directamente el registro. Si tienes que modificar el registro, ten mucha precaución.  
  
> [!WARNING]  
> Algunos de los valores preestablecidos que se configuran en el archivo de plantilla administrativa de sistema (System.adm) para la configuración de directiva de grupo (GPO) son diferentes de las entradas de registro predeterminado correspondiente. Si tiene previsto utilizar un GPO para configurar cualquier valor de tiempo de Windows, asegúrese de que revisar [valores del valor preestablecido para la configuración de directiva de grupo de servicio de hora de Windows son diferentes de las entradas correspondientes de registro de servicio de hora de Windows en Windows Server 2003 ](https://go.microsoft.com/fwlink/?LinkId=186066). Este problema se aplica a Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 y Windows Server 2003.  
  
Número de entradas del registro para el servicio de hora de Windows es los mismos que la configuración de directiva de grupo del mismo nombre. La configuración de directiva de grupo se corresponde con las entradas del registro del mismo nombre que se encuentra en:  
  
>**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\\**

  
Hay varias claves del registro en esta ubicación del registro. La configuración de hora de Windows se almacena en los valores de todas estas claves:
* [Parámetros](#Parameters)
* [configuración](#Configuration)
* [NtpClient](#NtpClient)
* [NtpServer](#NtpServer)
  

Muchos de los valores en la sección W32Time del registro se utilizan internamente W32Time para almacenar información. Estos valores no se deben cambiar manualmente en cualquier momento. No modifique la configuración de esta sección a menos que está familiarizado con la configuración y está seguro de que el nuevo valor funcionarán según lo previsto. Las siguientes entradas del registro se encuentran en:

**HKLM\SYSTEM\CurrentControlSet\Services\W32Time**  

Cuando se crea una directiva, la configuración se configura en la siguiente ubicación, que no tiene prioridad sobre la ubicación siguiente:

**HKLM\SOFTWARE\Policies\Microsoft\Windows\W32time**

La clave W32time se crea con la directiva.  Cuando se quita la directiva, también se quita esta clave.

La otra ubicación predeterminada:

**HKLM\SYSTEM\CurrentControlSet\Services\W32time**

Algunos de los parámetros se almacenan en ciclos de reloj en el registro y algunas son en segundos. Para convertir la hora de ciclos de reloj en segundos:  
  
-   1 minuto = 60 seg  
  
-   1 s = 1000 ms.  
  
-   1 ms = 10 000 ciclos de reloj en un sistema de Windows, tal y como se describe en [DateTime.Ticks Property](https://docs.microsoft.com/dotnet/api/system.datetime.ticks?redirectedfrom=MSDN&view=netframework-4.7.2#System_DateTime_Ticks).  
  
Por ejemplo, 5 minutos se convertiría en 5 * 60\*1000\*3000000000 = 10000 ciclos de reloj. 

Todas las versiones incluyen Windows 7, Windows 8, Windows 10, Windows Server 2008 y Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016.  Algunas entradas solo están disponibles en las versiones más recientes de Windows.


#### <a name="hklmsystemcurrentcontrolsetservicesw32timeparameters"></a>HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters

|Entrada del registro|Versión|Descripción|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|Todos|Entrada indica que se permiten las combinaciones de modo no estándar en la sincronización entre equipos del mismo nivel. El valor predeterminado para los miembros del dominio es 1. El valor predeterminado para servidores y clientes independientes es 1.|
|NtpServer|Todos|Entrada especifica una lista delimitada por espacios de pares de los que un equipo obtiene las marcas de tiempo, que consta de uno o varios nombres DNS o direcciones IP por línea. Cada nombre DNS o dirección IP que aparece debe ser único. Los equipos conectados a un dominio deben sincronizar con un origen de hora más confiable, como el reloj de tiempo oficial de Estados Unidos.  <ul><li>0 x 01 SpecialInterval </li><li>0x02 UseAsFallbackOnly</li><li>0 x 04 SymmetricActive - para obtener más información acerca de este modo, consulte [servidor horario de Windows: 3.3 modos de funcionamiento](https://go.microsoft.com/fwlink/?LinkId=208012).</li><li>0x08 Client</li></ul><br />No hay ningún valor predeterminado para esta entrada del registro en los miembros del dominio. El valor predeterminado en servidores y clientes independientes es time.windows.com,0x1.<br /><br />Nota: Para obtener más información sobre los servidores NTP disponibles, consulte [artículo de Microsoft Knowledge Base 262680: una lista de los servidores de hora de Protocolo Simple de tiempo de red (SNTP) que están disponibles en Internet](https://go.microsoft.com/fwlink/?LinkId=186067)|
|ServiceDll|Todos|Entrada se mantiene por W32Time. Contiene los datos reservados que se usan el sistema operativo de Windows y los cambios realizados en esta configuración pueden causar resultados imprevisibles. La ubicación predeterminada de este archivo DLL en miembros del dominio y los clientes independientes y servidores es % windir%\System32\W32Time.dll.  |
|ServiceMain|Todos|Entrada se mantiene por W32Time. Contiene los datos reservados que se usan el sistema operativo de Windows y los cambios realizados en esta configuración pueden causar resultados imprevisibles. El valor predeterminado en los miembros del dominio es SvchostEntry_W32Time. El valor predeterminado en servidores y clientes independientes es SvchostEntry_W32Time.  "|
|Tipo|Todos|Entrada indica que se empareja para aceptar la sincronización de:  <ul><li>**NoSync**. El servicio de hora no se sincroniza con otros orígenes.</li><li>**NTP.** El servicio de hora se sincroniza desde los servidores especificados en el **NtpServer**. entrada de registro.</li><li>**NT5DS**. El servicio de hora de sincronización de la jerarquía de dominios.  </li><li>**AllSync**. El servicio de hora utiliza todos los mecanismos de sincronización disponibles.  </li></ul>El valor predeterminado en los miembros del dominio es **NT5DS**. El valor predeterminado en servidores y clientes independientes es **NTP**.   |
---
#### <a name="hklmsystemcurrentcontrolsetservicesw32timeconfig"></a>HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config

|Entrada del registro|Versión|Descripción|
|------------------------------------|---------------|----------------------------|
|AnnounceFlags|Todos|Entrada controla si este equipo está marcado como un servidor horario confiable. Un equipo no está marcado como confiables a menos que también se marca como un servidor horario.<br /> -0 x 00 no es un servidor horario  <br /> -servidor de tiempo siempre es 0 x 01  <br /> -servidor de hora automática de 0 x 02  <br /> -servidor de hora confiable siempre es 0 x 04  <br /> -servidor de hora confiable automática de 0 x 08  <br />El valor predeterminado para los miembros del dominio es 10. El valor predeterminado para servidores y clientes independientes es 10.|
|EventLogFlags|Todos|Entrada controla los eventos que registra el servicio de hora.  <br />-Tiempo de salto: 0x1  <br />-Cambio de código fuente: 0x2  <br />El valor predeterminado en los miembros del dominio es 2. El valor predeterminado en servidores y clientes independientes es 2.  |
|FrequencyCorrectRate|Todos|Entrada controla la velocidad a la que se ha corregido el reloj. Si este valor es demasiado pequeño, el reloj no es estable y overcorrects. Si el valor es demasiado grande, el reloj tarda mucho tiempo para sincronizar. El valor predeterminado en los miembros del dominio es 4. El valor predeterminado en servidores y clientes independientes es 4.  <br /><br />Tenga en cuenta que 0 es un valor no válido para la entrada de registro FrequencyCorrectRate. En Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y los equipos de Windows Server 2008 R2, si el valor se establece en 0 el servicio de hora de Windows cambiará automáticamente, a 1.  |
|HoldPeriod|Todos|Entrada controla el período de tiempo para el que se deshabilita la detección de pico para abreviar el reloj local en la sincronización. Un pico es un ejemplo de tiempo que indica ese tiempo está desactivado de un número de segundos y normalmente se recibe después de que se han devuelto los ejemplos de buen momento coherente. El valor predeterminado en los miembros del dominio es 5. El valor predeterminado en servidores y clientes independientes es 5.  |
|LargePhaseOffset|Todos|Entrada especifica que un tiempo de desplazamiento mayor o igual que este valor en 10<sup>-7</sup> segundos se considera un pico. Una interrupción de la red como una gran cantidad de tráfico puede provocar un aumento. Un pico se omitirá a menos que se conserva durante un largo período de tiempo. El valor predeterminado en los miembros del dominio es 50000000. El valor predeterminado en servidores y clientes independientes es 50000000.  |
|LastClockRate|Todos|Entrada se mantiene por W32Time. Contiene los datos reservados que se usan el sistema operativo de Windows y los cambios realizados en esta configuración pueden causar resultados imprevisibles. El valor predeterminado en los miembros del dominio es 156250. El valor predeterminado en servidores y clientes independientes es 156250.  |
|LocalClockDispersion|Todos|Entrada controla la dispersión (en segundos) que se debe suponer cuando la única vez origen es el reloj CMOS integrado. El valor predeterminado en los miembros del dominio es 10. El valor predeterminado en servidores y clientes independientes es 10.|
|MaxAllowedPhaseOffset|Todos|Entrada especifica el desplazamiento máximo (en segundos) para el que W32Time intenta ajustar el reloj del equipo mediante el uso de la velocidad del reloj. Cuando el desplazamiento supera esta velocidad, W32Time configura el reloj del equipo directamente. El valor predeterminado para los miembros del dominio es 300. El valor predeterminado para servidores y clientes independientes es 1.  [Consulte a continuación para obtener más información](#MaxAllowedPhaseOffset).|
|MaxClockRate|Todos|Entrada se mantiene por W32Time. Contiene los datos reservados que se usan el sistema operativo de Windows y los cambios realizados en esta configuración pueden causar resultados imprevisibles. El valor predeterminado para los miembros del dominio es 155860. El valor predeterminado para servidores y clientes independientes es 155860.  |
|MaxNegPhaseCorrection|Todos|Entrada especifica la corrección de tiempo negativo más grande en segundos que hace el servicio. Si el servicio determina que es necesario un cambio más grande, registra un evento en su lugar. Caso especial: 0xFFFFFFFF significa que siempre hace la corrección de hora. El valor predeterminado para los miembros del dominio es 0xFFFFFFFF. El valor predeterminado para servidores y clientes independientes es 54.000 (15 horas).  |
|MaxPollInterval|Todos|Entrada especifica el intervalo mayor, en log2 segundos, permitido para el intervalo de sondeo del sistema. Tenga en cuenta que mientras un sistema debe sondear según el intervalo programado, un proveedor puede negar a producir muestras cuando lo solicita. El valor predeterminado para los controladores de dominio es 10. El valor predeterminado para los miembros del dominio es 15. El valor predeterminado para servidores y clientes independientes es 15.  |
|MaxPosPhaseCorrection|Todos|Entrada especifica la corrección más grande de tiempo positivo en segundos que hace el servicio. Si el servicio determina que es necesario un cambio más grande, registra un evento en su lugar. Caso especial: 0xFFFFFFFF significa que siempre hace la corrección de hora. El valor predeterminado para los miembros del dominio es 0xFFFFFFFF. El valor predeterminado para servidores y clientes independientes es 54.000 (15 horas).  |
|MinClockRate|Todos|Entrada se mantiene por W32Time. Contiene los datos reservados que se usan el sistema operativo de Windows y los cambios realizados en esta configuración pueden causar resultados imprevisibles. El valor predeterminado para los miembros del dominio es 155860. El valor predeterminado para servidores y clientes independientes es 155860.  |
|MinPollInterval|Todos|Entrada especifica el intervalo mínimo, en log2 segundos, permitido para el intervalo de sondeo del sistema. Tenga en cuenta que mientras un sistema no solicita ejemplos con más frecuencia que esto, un proveedor puede producir muestras en los momentos que no sea el intervalo programado. El valor predeterminado para los controladores de dominio es 6. El valor predeterminado para los miembros del dominio es 10. El valor predeterminado para servidores y clientes independientes es 10.  |
|PhaseCorrectRate|Todos|Entrada controla la velocidad a la que se ha corregido el error de fase. Especificar un valor pequeño corrige el error de fase rápidamente, pero puede provocar que el reloj se vuelva inestable. Si el valor es demasiado grande, tarda más tiempo para corregir el error de fase. <br /><br />El valor predeterminado en los miembros del dominio es 1. El valor predeterminado en servidores y clientes independientes es 7.<br /><br />Nota: 0 es un valor no válido para la entrada de registro PhaseCorrectRate. En Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y los equipos de Windows Server 2008 R2, si el valor se establece en 0, el servicio de hora de Windows lo cambiará automáticamente en 1.  |
|PollAdjustFactor|Todos|Entrada controla la decisión para aumentar o disminuir el intervalo de sondeo para el sistema. Cuanto mayor sea el valor, menor será la cantidad de error que hace que se puede reducir el intervalo de sondeo. El valor predeterminado en los miembros del dominio es 5. El valor predeterminado en servidores y clientes independientes es 5. |
|SpikeWatchPeriod|Todos|Entrada especifica la cantidad de tiempo que un desplazamiento de sospechoso debe conservar antes de que se acepte como correcto (en segundos). El valor predeterminado en los miembros del dominio es 900. El valor predeterminado en clientes independientes y estaciones de trabajo es 900.  |
|TimeJumpAuditOffset|Todos|Entero sin signo que indica el umbral de auditoría de salto de tiempo, en segundos. Si el servicio de hora ajusta el reloj local estableciendo directamente el reloj y la corrección de hora es mayor que este valor, el servicio de hora registra un evento de auditoría.|
|UpdateInterval|Todos|Entrada especifica el número de ciclos de reloj entre los ajustes de corrección de fase. El valor predeterminado para los controladores de dominio es 100. El valor predeterminado para los miembros del dominio es 30 000. El valor predeterminado para servidores y clientes independientes es 360,000.  <br /><br />**NOTA**: Cero es un valor no válido para la entrada de registro UpdateInterval. En equipos que ejecutan Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y Windows Server 2008 R2, si el valor se establece en 0 el servicio de hora de Windows lo cambiará automáticamente en 1.<br /><br />Las siguientes tres entradas del registro no forman parte de la configuración predeterminada de W32Time pero se pueden agregar al registro para obtener las funcionalidades de registro mayor. La información registrada en el registro de eventos del sistema se puede modificar cambiando el valor de la configuración de EventLogFlags en el Editor de objetos de directiva de grupo. De forma predeterminada, el servicio de hora crea un registro en el Visor de eventos cada vez que TI cambia a un nuevo origen de hora.<br /><br />**ADVERTENCIA**: Algunos de los valores preestablecidos que se configuran en el archivo de plantilla administrativa de sistema (System.adm) para la configuración de directiva de grupo (GPO) son diferentes de las entradas de registro predeterminado correspondiente. Si tiene previsto utilizar un GPO para configurar cualquier valor de tiempo de Windows, asegúrese de que revisar [valores del valor preestablecido para la configuración de directiva de grupo de servicio de hora de Windows son diferentes de las entradas correspondientes de registro de servicio de hora de Windows en Windows Server 2003 ](https://go.microsoft.com/fwlink/?LinkId=186066). Este problema se aplica a Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 y Windows Server 2003. |
|UtilizeSslTimeData|Registrar las compilación 1511 de Windows 10|Entrada de 1 indica que el W32Time utilizará varias marcas de tiempo SSL para propagar un reloj que es muy poco precisa.|
---
Con el fin de habilitar el registro de W32Time se deben agregar las entradas del registro siguientes:  

|Entrada del registro|Versión|Descripción|
|------------------------------------|---------------|----------------------------|
|FileLogEntries|Todos|Entrada controla la cantidad de entradas que creó en el archivo de registro de la hora de Windows. El valor predeterminado es none, lo que no registra ninguna actividad en tiempo de Windows. Los valores válidos son 0 y 300. Este valor no afecta a las entradas de registro de eventos que normalmente se crean por hora de Windows|
|FileLogName|Todos|Entrada controla la ubicación y el nombre de archivo del registro de hora de Windows. El valor predeterminado es en blanco y no se debe cambiar a menos que **FileLogEntries** se cambia. Un valor válido es una ruta de acceso completa y nombre de archivo que va a usar para crear el archivo de registro de hora de Windows. Este valor no afecta a las entradas de registro de eventos que normalmente se crean por hora de Windows.  |
|FileLogSize|Todos|Entrada controla el comportamiento del registro circular de archivos de registro de la hora de Windows. Cuando **FileLogEntries** y **FileLogName** están definidos, entrada define el tamaño, en bytes, para permitir que el archivo de registro alcanzar antes de sobrescribir las entradas más antiguas del registro con nuevas entradas. Use el valor mayor o 1000000 para esta configuración. Este valor no afecta a las entradas de registro de eventos que normalmente se crean por hora de Windows.  |

---

#### <a name="hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpclient"></a>HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient

|Entrada del registro|Versión|Descripción|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|Todos|Entrada indica que se permiten las combinaciones de modo no estándar en la sincronización entre equipos del mismo nivel. El valor predeterminado para los miembros del dominio es 1. El valor predeterminado para servidores y clientes independientes es 1.|
|CompatibilityFlags|Todos|Entrada especifica los valores y las marcas de compatibilidad siguientes: <br /><br />-   DispersionInvalid: 0x00000001  <br />-   IgnoreFutureRefTimeStamp: 0x00000002  <br /> -   AutodetectWin2K: 0x80000000  <br />-   AutodetectWin2KStage2: 0x40000000  <br /><br />El valor predeterminado para los miembros del dominio es 0 x 80000000. El valor predeterminado para servidores y clientes independientes es 0 x 80000000.  |
|CrossSiteSyncFlags|Todos|Entrada determina si el servicio elige asociados de sincronización fuera del dominio del equipo. Las opciones y los valores son:  <br /><br />-None: 0  <br />-PdcOnly: 1  <br />-Todo: 2  <br /><br />Este valor se omite si no se establece el valor de NT5DS. El valor predeterminado para los miembros del dominio es 2. El valor predeterminado para servidores y clientes independientes es 2.  |
|DllName|Todos|Entrada especifica la ubicación del archivo DLL para el proveedor de hora.  <br /><br />La ubicación predeterminada de este archivo DLL en miembros del dominio y los clientes independientes y servidores es % windir%\System32\W32Time.dll.|
|Enabled|Todos|Entrada indica si el proveedor NtpClient está habilitado en el servicio de hora actual.  <br /><ul><li>Sí 1</li><li>N 0</li></ul>El valor predeterminado en los miembros del dominio es 1. El valor predeterminado en servidores y clientes independientes es 1.|
|EventLogFlags|Todos|Entrada especifica los eventos registrados por el servicio de hora de Windows.<ul><li>cambios de accesibilidad de 0 x 1</li><li>0 x 2 muestra grandes sesgo (Esto es aplicable a Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y Windows Server 2008 R2 sólo)</li></ul>El valor predeterminado en los miembros del dominio es 0 x 1. El valor predeterminado en servidores y clientes independientes es 0 x 1.|
|InputProvider|Todos|Entrada indica si se debe habilitar NtpClient como un InputProvider, que obtiene la información de tiempo de la NtpServer. El NtpServer es un servidor horario que responde a solicitudes de tiempo del cliente en la red mediante la devolución de muestras de tiempo que son útiles para sincronizar el reloj local. <ul><li>Sí = 1  </li><li>No = 0 </li></ul><p>Valor predeterminado para los miembros del dominio y los clientes independientes: 1  |
|LargeSampleSkew|Todos|Entrada especifica el sesgo de ejemplo grande para el registro en segundos. Para cumplir con las especificaciones de seguridad y Exchange Commission (SEC), se debe establecer en tres segundos. Eventos se registrarán para esta configuración solo cuando EventLogFlags se configura explícitamente para 0 x 2 muestra gran asimetría. El valor predeterminado en los miembros del dominio es 3. El valor predeterminado en servidores y clientes independientes es 3.  |
|ResolvePeerBackOffMaxTimes|Todos|Entrada especifica el número máximo de veces que duplicar el intervalo de espera cuando se repite intenta localizar un punto para sincronizar con error. Un valor de cero significa que el intervalo de espera siempre es el mínimo. El valor predeterminado en los miembros del dominio es 7. El valor predeterminado en servidores y clientes independientes es 7. |
|ResolvePeerBackoffMinutes|Todos|Entrada especifica el intervalo de espera en minutos, antes de intentar buscar un elemento del mismo nivel para sincronizar con inicial. El valor predeterminado en los miembros del dominio es 15. El valor predeterminado en servidores y clientes independientes es 15.  |
|SpecialPollInterval|Todos|Entrada especifica el intervalo de sondeo especial en segundos para los interlocutores manuales. Cuando se habilita la marca SpecialInterval 0 x 1, determinar W32Time usa este intervalo de sondeo en lugar de un intervalo de sondeo por el sistema operativo. El valor predeterminado en los miembros del dominio es 3.600. El valor predeterminado en servidores y clientes independientes es 604.800.<br/><br/>Nuevo para la versión 1702, SpecialPollInterval contiene los valores del registro MinPollInterval y MaxPollInterval Config.|
|SpecialPollTimeRemaining|Todos|Entrada se mantiene por W32Time. Contiene los datos reservados que se usan el sistema operativo de Windows. Especifica el tiempo en segundos antes de W32Time sincronizará de nuevo una vez reiniciado el equipo. Los cambios realizados en esta configuración pueden producir resultados imprevisibles. El valor predeterminado en ambos miembros del dominio y en los servidores y clientes independientes se deja en blanco.  |

---

#### <a name="hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpserver"></a>HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer

|Entrada del registro|Versión|Descripción|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|Todos|Entrada indica que se permiten las combinaciones de modo no estándar en la sincronización entre clientes y servidores. El valor predeterminado para los miembros del dominio es 1. El valor predeterminado para servidores y clientes independientes es 1.|
|DllName|Todos|Entrada especifica la ubicación del archivo DLL para el proveedor de hora.<br /><br />La ubicación predeterminada de este archivo DLL en miembros del dominio y los clientes independientes y servidores es % windir%\System32\W32Time.dll.  |
|Enabled|Todos|Entrada indica si el proveedor NtpServer está habilitado en el servicio de hora actual. <ul><li>Sí 1</li><li>N 0</li></ul>El valor predeterminado en los miembros del dominio es 1. El valor predeterminado en servidores y clientes independientes es 1.  |
|InputProvider|Todos|Entrada indica si se debe habilitar NtpClient como un InputProvider, que obtiene la información de tiempo de la NtpServer. El NtpServer es un servidor horario que responde a solicitudes de tiempo del cliente en la red mediante la devolución de muestras de tiempo que son útiles para sincronizar el reloj local. <ul><li>Sí = 1  </li><li>No = 0 </li></ul><p>Valor predeterminado para los miembros del dominio y los clientes independientes: 1  |

---

#### <a name="maxallowedphaseoffset-information"></a>Información de MaxAllowedPhaseOffset
En el orden de W32Time establecer gradualmente el reloj del equipo, el desplazamiento debe ser menor que el **MaxAllowedPhaseOffset** de valor y cumplir la siguiente ecuación al mismo tiempo:  

```  
|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2  
``` 
El CurrentTimeOffset se mide en ciclos de reloj, donde 1 ms = 10.000 marcas en un sistema de Windows del reloj.  
  
SystemClockRate y PhaseCorrectRate también se miden en ciclos de reloj. Para obtener el SystemClockRate, puede usar el siguiente comando y convertirlo de segundos a ciclos de reloj mediante la fórmula de segundos * 1000\*10000:  
  
```  
W32tm /query /status /verbose  
ClockRate: 0.0156000s  
```  
  
SystemclockRate es la velocidad del reloj del sistema. Con 156000 segundos como ejemplo, el SystemclockRate podría ser = 0.0156000 * 1000 \* 156000 = 10000 ciclos de reloj.  
  
MaxAllowedPhaseOffset también está en segundos. Para convertir para ciclos de reloj, multiplique MaxAllowedPhaseOffset * 1000\*10000.  
  
Los dos ejemplos siguientes muestran cómo aplicar  
  
**Ejemplo 1**: Hora difiere de 4 minutos (por ejemplo, el tiempo es de 11:05 AM y la muestra de tiempo procedentes del mismo nivel y considera correcto es 11:09 AM).  
```
phasecorrectRate = 1  
  
UpdateInterval = 30000 (clock ticks)  
  
systemclockRate = 156000 (clock ticks)  
  
MaxAllowedPhaseOffset = 10min = 600 seconds = 600*1000\*10000=6000000000 clock ticks  
  
|currentTimeOffset| = 4mins = 4*60\*1000\*10000 = 2400000000 ticks  
  
Is CurrentTimeOffset < MaxAllowedPhaseOffset?  
  
2400000000 < 6000000000 = TRUE  
```
¿Y sí lo cumple la ecuación anterior? 
```
(|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2)  
  
Is 2,400,000,000 / (30000*1) < 156000/2  
  
Is 80,000 < 78,000  
  
NO/FALSE  
```  
Por lo tanto W32tm establecería el reloj volver inmediatamente.  
  
> [!NOTE]  
> En este caso, si desea que se retrase el reloj lentamente, deberá ajustar los valores de PhaseCorrectRate o updateInterval en el registro de así a garantizar que los resultados de la ecuación en TRUE.  
  
**Ejemplo 2**: Hora difiere de 3 minutos.  
```  
phasecorrectRate = 1  
  
UpdateInterval = 30000 (clock ticks)  
  
systemclockRate = 156000 (clock ticks)  
  
MaxAllowedPhaseOffset = 10min = 600 seconds = 600*1000\*10000=6000000000 clock ticks  
  
currentTimeOffset = 3mins = 3*60\*1000\*10000 = 1800000000 clock ticks  
  
Is CurrentTimeOffset < MaxAllowedPhaseOffset?  
  
1800000000 < 6000000000 = TRUE  
```  
¿Y sí lo cumple la ecuación anterior?
```
(|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2)  
  
Is 3 mins (1,800,000,000) / (30000*1) < 156000/2  
  
Is 60,000 < 78,000  
  
YES/TRUE  
```  
En este caso el reloj se establecerá atrás lentamente.  
  
## <a name="windows-time-service-group-policy-settings"></a>Configuración de directiva de grupo de servicio de hora de Windows  
Puede configurar la mayoría de los parámetros de W32Time utilizando el Editor de objetos de directiva de grupo. Esto incluye la configuración de un equipo para que sea un NTPServer o NTPClient, configurar el mecanismo de sincronización de hora y configurar un equipo para que sea un origen de hora confiable.  
  
> [!NOTE]  
> Configuración de directiva de grupo para el servicio de hora de Windows se puede configurar en controladores de dominio de Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y Windows Server 2008 R2 y se puede aplicar solo a los equipos que ejecutan Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 y Windows Server 2008 R2.  
  
Puede encontrar la directiva de grupo configuración utiliza para configurar W32Time en el complemento Editor de objetos de directiva de grupo en las siguientes ubicaciones:  
  
-   Servicio de hora de equipo equipo\Plantillas Templates\System\Windows  
  
    Configurar **opciones de configuración globales** aquí.  
  
-   Equipo Configuración del equipo\Plantillas Templates\System\Windows tiempo Service\Time proveedores  
  
    Configurar **Windows NTP cliente** estas opciones.  
  
    Habilitar **Windows NTP cliente** aquí.  
  
    Habilitar **servidor NTP de Windows** aquí.  
  
> [!WARNING]  
> Algunos de los valores preestablecidos que se configuran en el archivo de plantilla administrativa de sistema (System.adm) para la configuración de directiva de grupo (GPO) son diferentes de las entradas de registro predeterminado correspondiente. Si tiene previsto utilizar un GPO para configurar cualquier valor de tiempo de Windows, asegúrese de que revisar [valores del valor preestablecido para la configuración de directiva de grupo de servicio de hora de Windows son diferentes de las entradas correspondientes de registro de servicio de hora de Windows en Windows Server 2003 ](https://go.microsoft.com/fwlink/?LinkId=186066). Este problema se aplica a Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 y Windows Server 2003.  
  
La tabla siguiente muestra la configuración de directiva de grupo global que está asociada con el servicio de hora de Windows y el valor establecido previamente asociado con cada opción de configuración. Para obtener más información sobre cada opción, vea las entradas del registro correspondiente en "[entradas de registro del servicio de hora de Windows](#w2k3tr_times_tools_uhlp)" anteriormente en este tema. Las siguientes opciones se encuentran en un único GPO llamado **opciones de configuración globales**.  
  
**Configuración de directiva de grupo global asociado a la hora de Windows**  
  
|Configuración de directiva de grupo|Valor predefinido|  
|------------------------|------------------|  
|AnnounceFlags|10|  
|EventLogFlags|2|  
|FrequencyCorrectRate|4|  
|HoldPeriod|5|  
|LargePhaseOffset|1280000|  
|LocalClockDispersion|10|  
|MaxAllowedPhaseOffset|300|  
|MaxNegPhaseCorrection|54 000 (15 horas)|  
|MaxPollInterval|15|  
|MaxPosPhaseCorrection|54 000 (15 horas)|  
|MinPollInterval|10|  
|PhaseCorrectRate|7|  
|PollAdjustFactor|5|  
|SpikeWatchPeriod|90|  
|UpdateInterval|100|  
  
En la tabla siguiente se enumera las opciones disponibles para el **configurar el cliente de Windows NTP** GPO y los valores predefinidos que están asociados con el servicio de hora de Windows. Para obtener más información sobre cada opción, vea las entradas del registro correspondiente en "[entradas de registro del servicio de hora de Windows](#w2k3tr_times_tools_uhlp)" anteriormente en este tema.  
  
**Configuración de directiva de grupo de clientes NTP asociado a la hora de Windows**  
  
|Configuración de directiva de grupo|Valor predeterminado|  
|------------------------|-----------------|  
|NtpServer|time.windows.com,0x1|  
|Tipo|Opciones predeterminadas:<br /><br />-   **NTP.** Utilizar en equipos que no están unidos a un dominio.<br />-   **NT5DS.** Utilizar en equipos que están unidos a un dominio.|  
|CrossSiteSyncFlags|2|  
|ResolvePeerBackoffMinutes|15|  
|ResolvePeerBackoffMaxTimes|7|  
|SpecialPollInterval|3600|  
|EventLogFlags|0|  
  
## <a name="network-ports-used-by-the-windows-time-service"></a>Puertos de red usados por el servicio de hora de Windows  
Hora de Windows sigue la especificación de NTP, lo que requiere el uso del puerto UDP 123 para todas las comunicaciones de sincronización de hora. Este puerto está reservado por hora de Windows y queda reservada en todo momento. Cada vez que el equipo sincroniza su reloj o proporciona tiempo a otro equipo, que la comunicación se realiza en el puerto UDP 123.  
  
> [!NOTE]  
> Si tiene un equipo con varios adaptadores de red (también denominados equipo de host múltiple), no se puede habilitar selectivamente el servicio de hora de Windows en función del adaptador de red.  
  
## <a name="related-information"></a>Información relacionada  
Los siguientes recursos contienen información adicional que es relevante para esta sección.  
  
-   RFC *1305* en la base de datos RFC de IETF  
