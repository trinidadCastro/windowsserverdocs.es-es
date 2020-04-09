---
title: Optimizar IIS 10,0
description: Optimización del rendimiento recommmendations para servidores Web de IIS 10,0 en Windows Server 16
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: davso; ericam; yashi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 229c4f53578430e35a66e3dbe50f0d9a8e9ac2f5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851668"
---
# <a name="tuning-iis-100"></a>Optimizar IIS 10,0

Internet Information Services (IIS) 10,0 se incluye con Windows Server 2016. Usa un modelo de proceso similar al de IIS 8,5 e IIS 7,0. Un controlador Web en modo kernel (http. sys) recibe y enruta las solicitudes HTTP y satisface las solicitudes de su caché de respuesta. Los procesos de trabajo se registran para los subespacios de URL y http. sys enruta la solicitud al proceso adecuado (o conjunto de procesos para los grupos de aplicaciones).

HTTP. sys es responsable de la administración de conexiones y el control de solicitudes. La solicitud se puede atender desde la memoria caché HTTP. sys o pasarse a un proceso de trabajo para su mayor control. Se pueden configurar varios procesos de trabajo, lo que proporciona aislamiento a un costo reducido. Para obtener más información sobre cómo funciona el control de solicitudes, vea la ilustración siguiente:

![control de solicitudes en IIS 10,0](../../media/perftune-guide-iis-request-handling.png)

HTTP. sys incluye una caché de respuesta. Cuando una solicitud coincide con una entrada en la caché de respuesta, HTTP. sys envía la respuesta de caché directamente desde el modo kernel. Algunas plataformas de aplicaciones Web, como ASP.NET, proporcionan mecanismos para permitir que cualquier contenido dinámico se almacene en caché en la memoria caché del modo kernel. El controlador de archivos estáticos de IIS 10,0 almacena automáticamente en caché los archivos solicitados con frecuencia en http. sys.

Dado que un servidor Web tiene componentes en modo kernel y en modo de usuario, ambos componentes deben ajustarse para obtener un rendimiento óptimo. Por lo tanto, la optimización de IIS 10,0 para una carga de trabajo específica incluye la configuración de lo siguiente:

-   HTTP. sys y la caché del modo kernel asociada

-   Procesos de trabajo y IIS en modo de usuario, incluida la configuración del grupo de aplicaciones

-   Determinados parámetros de optimización que afectan al rendimiento

En las secciones siguientes se describe cómo configurar los aspectos del modo kernel y del modo de usuario de IIS 10,0.

## <a name="kernel-mode-settings"></a>Configuración de modo kernel

La configuración de HTTP. sys relacionada con el rendimiento se divide en dos amplias categorías: administración de caché y administración de conexiones y solicitudes. Toda la configuración del registro se almacena en la siguiente entrada del registro:

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters
```

**Tenga en cuenta**  si el servicio http ya se está ejecutando, debe reiniciarlo para que los cambios surtan efecto.

Ti 

## <a name="cache-management-settings"></a>Configuración de administración de caché

Una ventaja que proporciona HTTP. sys es una caché en modo kernel. Si la respuesta está en la caché de modo kernel, puede satisfacer una solicitud HTTP por completo desde el modo kernel, lo que reduce considerablemente el costo de la CPU de administrar la solicitud. Sin embargo, la memoria caché de modo kernel de IIS 10,0 se basa en la memoria física y el costo de una entrada es la memoria que ocupa.

Una entrada en la memoria caché solo es útil cuando se usa. Sin embargo, la entrada siempre consume memoria física, tanto si se utiliza la entrada como si no. Debe evaluar la utilidad de un elemento en la memoria caché (el ahorro de ser capaz de servirlo desde la memoria caché) y su costo (la memoria física ocupada) durante la vigencia de la entrada teniendo en cuenta los recursos disponibles (CPU y memoria física) y los requisitos de la carga de trabajo. HTTP. sys intenta mantener solo elementos útiles y a los que se accede activamente en la memoria caché, pero puede aumentar el rendimiento del servidor Web mediante la optimización de la memoria caché de HTTP. sys para cargas de trabajo concretas.

A continuación se muestran algunos valores útiles para la caché de modo kernel de HTTP. sys:

-   **UriEnableCache** Valor predeterminado: 1

    Un valor distinto de cero habilita la respuesta en modo kernel y el almacenamiento en caché de fragmentos. Para la mayoría de las cargas de trabajo, la memoria caché debe permanecer habilitada. Considere la posibilidad de deshabilitar la caché si espera una respuesta muy baja y almacenamiento en caché de fragmentos.

-   **UriMaxCacheMegabyteCount** Valor predeterminado: 0

    Un valor distinto de cero que especifica la memoria máxima disponible para la memoria caché en modo kernel. El valor predeterminado, 0, permite al sistema ajustar automáticamente la cantidad de memoria disponible para la memoria caché.

    **Nota:** Si se especifica el tamaño, solo se establece el máximo y el sistema podría no permitir que la memoria caché crezca hasta el tamaño máximo del conjunto.

    Ti 

-   **UriMaxUriBytes** Valor predeterminado: 262144 bytes (256 KB)

    Tamaño máximo de una entrada en la memoria caché del modo kernel. Las respuestas o fragmentos mayores que este no se almacenan en caché. Si tiene suficiente memoria, considere la posibilidad de aumentar el límite. Si la memoria es limitada y las entradas grandes están amontonando más pequeñas, puede resultar útil reducir el límite.

-   **UriScavengerPeriod** Valor predeterminado: 120 segundos

    La memoria caché HTTP. sys se examina periódicamente mediante un borrador y se quitan las entradas a las que no se tiene acceso entre los recorridos del borrador. La configuración del período de eliminación de registros obsoletos en un valor alto reduce el número de recorridos de limpieza. Sin embargo, el uso de memoria caché puede aumentar porque las entradas más antiguas y a las que se accede con menos frecuencia pueden permanecer en la memoria caché. Si se establece un período demasiado bajo, se realiza un análisis más frecuente de los registros obsoletos, lo que puede dar lugar a demasiados vaciados y a la renovación de la memoria caché.

## <a name="request-and-connection-management-settings"></a>Configuración de la administración de solicitudes y conexiones

En Windows Server 2016, HTTP. sys administra las conexiones automáticamente. Ya no se utiliza la siguiente configuración del registro:

-   **MaxConnections**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\MaxConnections
    ```

-   **IdleConnectionsHighMark**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\IdleConnectionsHighMark
    ```

-   **IdleConnectionsLowMark**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\IdleConnectionsLowMark
    ```

-   **IdleListTrimmerPeriod**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\IdleListTrimmerPeriod
    ```

-   **RequestBufferLookasideDepth**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\RequestBufferLookasideDepth
    ```

-   **InternalRequestLookasideDepth**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\InternalRequestLookasideDepth
    ```


## <a name="user-mode-settings"></a>Configuración de modo de usuario

La configuración de esta sección afecta al comportamiento del proceso de trabajo de IISÂ 10,0. La mayoría de estas opciones se pueden encontrar en el siguiente archivo de configuración XML:

% SystemRoot%\\system32\\Inetsrv\\config\\applicationHost. config

Use appcmd. exe, la consola de administración de IIS 10,0, los cmdlets de PowerShell WebAdministration o IISAdministration para cambiarlos. La mayoría de los valores se detectan automáticamente y no requieren el reinicio de los procesos de trabajo de IIS 10,0 o del servidor de aplicaciones Web. Para obtener más información sobre el archivo applicationHost. config, consulte [Introduction to ApplicationHost. config](https://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig).


## <a name="ideal-cpu-setting-for-numa-hardware"></a>Configuración de CPU ideal para el hardware NUMA

A partir de Windows 2016, IIS 10,0 admite la asignación de CPU ideal automática para los subprocesos del grupo de subprocesos para mejorar el rendimiento y la escalabilidad en el hardware NUMA. Esta característica está habilitada de forma predeterminada y se puede configurar mediante la siguiente clave del registro:

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\InetInfo\Parameters\ThreadPoolUseIdealCpu
```

Con esta característica habilitada, el administrador de subprocesos de IIS realiza el mejor esfuerzo para distribuir uniformemente los subprocesos del grupo de subprocesos de IIS en todas las CPU de todos los nodos NUMA en función de sus cargas actuales. En general, se recomienda mantener esta configuración predeterminada sin cambios para el hardware NUMA.

**Tenga en cuenta**  la configuración de CPU ideal es diferente de la configuración de asignación de nodos Numa del proceso de trabajo (NumaNodeAssignment y numaNodeAffinityMode) introducida en la [configuración de CPU para un grupo de aplicaciones](https://www.iis.net/configreference/system.applicationhost/applicationpools/add/cpu). La configuración de CPU ideal afecta a la manera en que IIS distribuye los subprocesos del grupo de subprocesos, mientras que la configuración de asignación del nodo NUMA del proceso de trabajo determina en qué nodo NUMA se inicia un proceso de trabajo.

## <a name="user-mode-cache-behavior-settings"></a>Configuración de comportamiento de caché en modo usuario

En esta sección se describen los valores que afectan al comportamiento de almacenamiento en caché en IISÂ 10,0. La memoria caché de modo de usuario se implementa como un módulo que escucha los eventos de almacenamiento en caché globales que genera la canalización integrada. Para deshabilitar completamente la caché de modo de usuario, quite el módulo FileCacheModule (cachfile. dll) de la lista de módulos instalados en la sección de configuración System. WebServer/globalModules de applicationHost. config.

**System. WebServer/Caching**

|Atributo|Descripción|Default|
|--- |--- |--- |
|Habilitado|Deshabilita la caché de IIS de modo de usuario cuando se establece en **false**. Cuando la tasa de aciertos de caché es muy pequeña, puede deshabilitar la caché por completo para evitar la sobrecarga asociada a la ruta de acceso del código de caché. Deshabilitar la caché en modo de usuario no deshabilita la caché en modo kernel.|True|
|enableKernelCache|Deshabilita la caché en modo kernel cuando se establece en **false**.|True|
|maxCacheSize|Limita el tamaño de la caché del modo de usuario de IIS al tamaño especificado en megabytes. IIS ajusta el valor predeterminado en función de la memoria disponible. Elija el valor detenidamente según el tamaño del conjunto de archivos a los que se accede con frecuencia en comparación con la cantidad de RAM o el espacio de direcciones del proceso de IIS.|0|
|maxResponseSize|Almacena en caché los archivos hasta el tamaño especificado. El valor real depende del número y el tamaño de los archivos más grandes del conjunto de datos frente a la memoria RAM disponible. Almacenar en caché los archivos grandes solicitados con frecuencia puede reducir el uso de CPU, el acceso a disco y las latencias asociadas.|262144|

## <a name="compression-behavior-settings"></a>Configuración del comportamiento de compresión

IIS a partir de 7,0 comprime el contenido estático de forma predeterminada. Además, la compresión de contenido dinámico está habilitada de forma predeterminada cuando se instala DynamicCompressionModule. La compresión reduce el uso de ancho de banda, pero aumenta el uso de CPU. El contenido comprimido se almacena en caché en la caché de modo kernel si es posible. A partir de 8,5, IIS permite controlar la compresión de forma independiente para el contenido estático y dinámico. El contenido estático normalmente hace referencia a contenido que no cambia, como archivos GIF o HTM. Normalmente, el contenido dinámico lo generan scripts o código en el servidor, es decir, páginas ASP.NET. Puede personalizar la clasificación de cualquier extensión concreta como estática o dinámica.

Para deshabilitar completamente la compresión, quite StaticCompressionModule y DynamicCompressionModule de la lista de módulos de la sección System. WebServer/globalModules de applicationHost. config.

**System. WebServer/httpCompression**

|Atributo|Descripción|Default|
|--- |--- |--- |
|staticCompression-EnableCpuUsage<br><br>staticCompression-DisableCpuUsage<br><br>dynamicCompression-EnableCpuUsage<br><br>dynamicCompression-DisableCpuUsage|Habilita o deshabilita la compresión si el porcentaje de uso de CPU actual va por encima o por debajo de los límites especificados.<br><br>A partir de IIS 7,0, la compresión se deshabilita automáticamente si la CPU de estado constante aumenta por encima del umbral de deshabilitación. La compresión se habilita si la CPU cae por debajo del umbral de habilitación.|50, 100, 50 y 90, respectivamente|
|directorio|Especifica el directorio en el que las versiones comprimidas de los archivos estáticos se almacenan temporalmente y se almacenan en caché. Considere la posibilidad de mover este directorio fuera de la unidad del sistema si se tiene acceso a él con frecuencia.|%SystemDrive%\inetpub\temp\IIS archivos comprimidos temporales|
|doDiskSpaceLimiting|Especifica si existe un límite para la cantidad de espacio en disco que pueden ocupar todos los archivos comprimidos. Los archivos comprimidos se almacenan en el directorio de compresión especificado por el atributo de **directorio** .|True|
|maxDiskSpaceUsage|Especifica el número de bytes de espacio en disco que pueden ocupar los archivos comprimidos en el directorio de compresión.<br><br>Es posible que sea necesario aumentar este valor si el tamaño total de todo el contenido comprimido es demasiado grande.|100 MB|

**System. WebServer/elemento urlcompression**

|Atributo|Descripción|Default|
|--- |--- |--- |
|doStaticCompression|Especifica si se comprime el contenido estático.|True|
|doDynamicCompression|Especifica si se comprime el contenido dinámico.|True|

**Nota:** En el caso de los servidores que ejecutan IIS 10,0 y que tienen un uso de CPU medio bajo, considere la posibilidad de habilitar la compresión para contenido dinámico, especialmente si las respuestas son grandes. En primer lugar, debe realizarse en un entorno de prueba para evaluar el efecto en el uso de la CPU desde la línea de base.


### <a name="tuning-the-default-document-list"></a>Optimizar la lista de documentos predeterminados

El módulo documento predeterminado controla las solicitudes HTTP para la raíz de un directorio y las convierte en solicitudes de un archivo específico, como default. htm o index. htm. En promedio, aroundÂ el 25 por ciento de todas las solicitudes en Internet pasan por la ruta de acceso predeterminada del documento. Esto varía significativamente en sitios individuales. Cuando una solicitud HTTP no especifica un nombre de archivo, el módulo documento predeterminado busca en la lista de documentos predeterminados permitidos cada nombre del sistema de archivos. Esto puede afectar negativamente al rendimiento, especialmente si al alcanzar el contenido es necesario realizar un recorrido de ida y vuelta de red o tocar un disco.

Puede evitar la sobrecarga mediante la deshabilitación selectiva de los documentos predeterminados y la reducción o el orden de la lista de documentos. En el caso de los sitios web que usan un documento predeterminado, debe reducir la lista solo a los tipos de documento predeterminados que se usan. Además, ordene la lista para que comience por el nombre de archivo de documento predeterminado al que se accede con mayor frecuencia.

Puede establecer de forma selectiva el comportamiento del documento predeterminado en direcciones URL específicas personalizando la configuración dentro de una etiqueta de ubicación en applicationHost. config o insertando un archivo Web. config directamente en el directorio de contenido. Esto permite un enfoque híbrido, que habilita los documentos predeterminados solo cuando son necesarios y establece la lista en el nombre de archivo correcto para cada dirección URL.

Para deshabilitar completamente los documentos predeterminados, quite DefaultDocumentModule de la lista de módulos de la sección System. WebServer/globalModules de applicationHost. config.

**System. WebServer/defaultDocument**

|Atributo|Descripción|Default|
|--- |--- |--- |
|enabled|Especifica que los documentos predeterminados están habilitados.|True|
|&lt;files&gt;, elemento|Especifica los nombres de archivo que se configuran como documentos predeterminados.|La lista predeterminada es default. htm, default. asp, index. htm, index. html, Iisstart. htm y default. aspx.|

## <a name="central-binary-logging"></a>Registro de binario central

Cuando la sesión de servidor tiene varios grupos de direcciones URL debajo, el proceso de creación de cientos de archivos de registro con formato para los grupos de direcciones URL individuales y la escritura de los datos de registro en un disco puede consumir rápidamente recursos valiosos de CPU y memoria, con lo que se crean problemas de rendimiento y escalabilidad. El registro binario centralizado reduce la cantidad de recursos del sistema que se usan para el registro, al mismo tiempo que proporciona datos de registro detallados para las organizaciones que lo requieren. El análisis de los registros de formato binario requiere una herramienta de procesamiento posterior.

Puede habilitar el registro de binario central estableciendo el atributo centralLogFileMode en CentralBinary y estableciendo el atributo **Enabled** en **true**. Considere la posibilidad de mover la ubicación del archivo de registro central a la partición del sistema y a una unidad de registro dedicada para evitar la contención entre las actividades del sistema y las actividades de registro.

**System. applicationHost/log**

|Atributo|Descripción|Default|
|--- |--- |--- |
|centralLogFileMode|Especifica el modo de registro de un servidor. Cambie este valor a CentralBinary para habilitar el registro central de binarios.|Sitio|

**System. applicationHost/log/centralBinaryLogFile**

|Atributo|Descripción|Default|
|--- |--- |--- |
|enabled|Especifica si está habilitado el registro central de archivos binarios.|False|
|directorio|Especifica el directorio en el que se escriben las entradas del registro.|%SystemDrive%\inetpub\logs\LogFiles|


## <a name="application-and-site-tunings"></a>Optimizaciones de aplicaciones y sitios

La configuración siguiente se relaciona con el grupo de aplicaciones y las optimizaciones de sitios.

**System. applicationHost/applicationPools/applicationPoolDefaults**

|Atributo|Descripción|Default|
|--- |--- |--- |
|queueLength|Indica a HTTP. sys el número de solicitudes que se ponen en cola para un grupo de aplicaciones antes de que se rechacen las solicitudes futuras. Cuando se supera el valor de esta propiedad, IIS rechaza las solicitudes posteriores con un error 503.<br><br>Considere la posibilidad de aumentar esto para las aplicaciones que se comunican con almacenes de datos back-end de latencia alta si se detectan errores 503.|1000|
|enable32BitAppOnWin64|Cuando es true, permite que una aplicación de 32 bits se ejecute en un equipo que tenga un procesador de 64 bits.<br><br>Considere la posibilidad de habilitar el modo de 32 bits si el consumo de memoria es un problema. Dado que los tamaños de puntero y los tamaños de instrucción son más pequeños, las aplicaciones de 32 bits usan menos memoria que las aplicaciones de 64 bits. El inconveniente de ejecutar aplicaciones de 32 bits en un equipo de 64 bits es que el espacio de direcciones del modo de usuario está limitado a 4 GB.|False|

**System. applicationHost/sites/VirtualDirectoryDefault**

|Atributo|Descripción|Default|
|--- |--- |--- |
|allowSubDirConfig|Especifica si IIS busca los archivos Web. config en los directorios de contenido inferiores al nivel actual (true) o no busca los archivos Web. config en los directorios de contenido inferiores al nivel actual (false). Al imponer una limitación sencilla, que permite la configuración solo en directorios virtuales, IISÂ 10,0 puede saber que, a menos que **/&lt;nombre&gt;. htm** sea un directorio virtual, no debe buscar un archivo de configuración. Omitir las operaciones de archivo adicionales puede mejorar significativamente el rendimiento de los sitios web que tienen un conjunto muy grande de contenido estático de acceso aleatorio.|True|

## <a name="managing-iis-100-modules"></a>Administrar módulos de IIS 10,0

IIS 10,0 se ha factorizado en varios módulos extensibles para el usuario con el fin de admitir una estructura modular. Esta factorización tiene un pequeño costo. Para cada módulo, la canalización integrada debe llamar al módulo para cada evento que sea relevante para el módulo. Esto sucede independientemente de si el módulo debe realizar algún trabajo. Puede conservar los ciclos de CPU y la memoria quitando todos los módulos que no son relevantes para un sitio Web determinado.

Un servidor Web que esté optimizado para archivos estáticos simples podría incluir solo los cinco módulos siguientes: UriCacheModule, HttpCacheModule, StaticFileModule, AnonymousAuthenticationModule y HttpLoggingModule.

Para quitar módulos de applicationHost. config, quite todas las referencias al módulo de las secciones System. WebServer/handlers y System. WebServer/modules, además de quitar la declaración de módulo en System. WebServer/globalModules.

## <a name="classic-asp-settings"></a>Configuración de ASP clásica

El importante costo de procesar una solicitud ASP clásica incluye inicializar un motor de scripts, compilar el script ASP solicitado en una plantilla de ASP y ejecutar la plantilla en el motor de scripts. Aunque el costo de ejecución de la plantilla depende de la complejidad del script ASP solicitado, el módulo ASP clásico de IIS puede almacenar en caché los motores de script en memoria y almacenar en memoria caché las plantillas en la memoria y el disco (solo si se desborda la caché de plantillas en memoria) para mejorar el rendimiento en escenarios enlazados a la CPU.

La siguiente configuración se usa para configurar la caché de la plantilla de ASP clásica y la caché del motor de scripts, y no afectan a la configuración de ASP.NET.

**System. WebServer/ASP/cache**

|Atributo|Descripción|Default|
|--- |--- |--- |
|diskTemplateCacheDirectory|Nombre del directorio que utiliza ASP para almacenar las plantillas compiladas cuando se desborda la caché en memoria.<br><br>Recomendación: se establece en un directorio que no se usa mucho, por ejemplo, una unidad que no se comparte con el sistema operativo, el registro de IIS u otro contenido al que se tiene acceso con frecuencia.|%SystemDrive%\inetpub\temp\ASP plantillas compiladas|
|maxDiskTemplateCacheFiles|Especifica el número máximo de plantillas ASP compiladas que se pueden almacenar en caché en el disco.<br><br>Recomendación: establézcalo en el valor máximo de 0x7FFFFFFF.|2000|
|scriptFileCacheSize|Este atributo especifica el número máximo de plantillas ASP compiladas que se pueden almacenar en la memoria caché.<br><br>Recomendación: establezca como mínimo el número de scripts ASP solicitados con frecuencia que atiende un grupo de aplicaciones. Si es posible, establezca en tantas plantillas ASP como permita el límite de memoria.|500|
|scriptEngineCacheMax|Especifica el número máximo de motores de script que se mantendrán almacenados en la memoria caché.<br><br>Recomendación: establezca como mínimo el número de scripts ASP solicitados con frecuencia que atiende un grupo de aplicaciones. Si es posible, establezca en tantos motores de script como permita el límite de memoria.|250|

**System. WebServer/ASP/Limits**

|Atributo|Descripción|Default|
|--- |--- |--- |
|processorThreadMax|Especifica el número máximo de subprocesos de trabajo por procesador que puede crear ASP. Aumente el valor de si la configuración actual es insuficiente para controlar la carga, lo que puede producir errores cuando atiende solicitudes o causa un uso bajo de los recursos de la CPU.|25|

**System. WebServer/ASP/ComPlus**

|Atributo|Descripción|Default|
|--- |--- |--- |
|executeInMta|Establézcalo en **true** si se detectan errores o errores mientras IIS atiende el contenido de ASP. Esto puede ocurrir, por ejemplo, cuando se hospedan varios sitios aislados en los que cada sitio se ejecuta en su propio proceso de trabajo. Normalmente, los errores se indican desde COM+ en el Visor de eventos. Esta configuración habilita el modelo de Apartamento multiproceso en ASP.|False|


## <a name="aspnet-concurrency-setting"></a>Configuración de simultaneidad de ASP.NET

### <a name="aspnet-35"></a>ASP.NET 3,5
De forma predeterminada, ASP.NET limita la simultaneidad de solicitudes para reducir el consumo de memoria de estado estable en el servidor. Es posible que las aplicaciones de simultaneidad altas deban ajustar algunas opciones para mejorar el rendimiento general. Puede cambiar esta configuración en el archivo Aspnet. config:

``` syntax
<system.web>
  <applicationPool maxConcurrentRequestsPerCPU="5000"/>
</system.web>
```

La configuración siguiente es útil para usar totalmente los recursos en un sistema:

-   **maxConcurrentRequestPerCpu** Valor predeterminado: 5000

    Esta configuración limita el número máximo de solicitudes ASP.NET que se ejecutan simultáneamente en un sistema. El valor predeterminado es conservador para reducir el consumo de memoria de las aplicaciones de ASP.NET. Considere la posibilidad de aumentar este límite en los sistemas que ejecutan aplicaciones que realizan operaciones de e/s sincrónicas largas. De lo contrario, los usuarios pueden experimentar una latencia elevada debido a errores de puesta en cola o de solicitud debido a la superación de los límites de cola bajo una carga elevada cuando se utiliza la configuración predeterminada.

### <a name="aspnet-46"></a>ASP.NET 4,6
Además de la configuración de maxConcurrentRequestPerCpu, ASP.NET 4,7 también proporciona opciones para mejorar el rendimiento en las aplicaciones que se basan en gran medida en la operación asincrónica. La configuración se puede cambiar en el archivo Aspnet. config.

``` syntax
<system.web>
  <applicationPool percentCpuLimit="90" percentCpuLimitMinActiveRequestPerCpu="100"/>
</system.web>
```

-   **percentCpuLimit** Valor predeterminado: 90 la solicitud asincrónica tiene algunos problemas de escalabilidad cuando se coloca una carga enorme (más allá de las capacidades de hardware) en este escenario. El problema se debe a la naturaleza de la asignación en escenarios asincrónicos. En estas condiciones, la asignación se realizará cuando se inicie la operación asincrónica y se consumirá cuando se complete. En ese momento, itâs es muy posible que los objetos se hayan pasado a la generación 1 o 2 por GC. Cuando esto sucede, el aumento de la carga mostrará el aumento de solicitudes por segundo (RPS) hasta un punto. Una vez que pasamos ese punto, el tiempo invertido en GC comenzará a convertirse en un problema y el RPS comenzará a DIP, con un efecto de escalado negativo. Para solucionar el problema, cuando el uso de CPU supere el valor de percentCpuLimit, las solicitudes se enviarán a la cola de ASP.NET Native.
-   **percentCpuLimitMinActiveRequestPerCpu** Valor predeterminado: 100 el límite de CPU (configuración percentCpuLimit) no se basa en el número de solicitudes, sino en el costo que tienen. Como resultado, puede haber pocas solicitudes de uso intensivo de la CPU que produzcan una copia de seguridad en la cola nativa sin ninguna manera de vaciarla de las solicitudes entrantes. Para solucionar este problme, percentCpuLimitMinActiveRequestPerCpu se puede usar para asegurarse de que se atiende un número mínimo de solicitudes antes de que se inicie la limitación.

## <a name="worker-process-and-recycling-options"></a>Opciones de proceso y reciclaje de trabajo

Puede configurar opciones para reciclar procesos de trabajo de IIS y proporcionar soluciones prácticas a situaciones o eventos agudas sin necesidad de intervención o restablecimiento de un servicio o equipo. Tales situaciones y eventos incluyen fugas de memoria, aumento de la carga de memoria o procesos de trabajo inactivos o sin respuesta. En condiciones normales, es posible que no se necesiten opciones de reciclaje y que se pueda desactivar el reciclaje, o bien se puede configurar el sistema para que se recicle con mucha frecuencia.

Puede habilitar el reciclaje de procesos para una aplicación determinada agregando atributos al elemento **reciclaje/periodicRestart** . El evento de reciclaje se puede desencadenar por varios eventos, como el uso de memoria, un número fijo de solicitudes y un período de tiempo fijo. Cuando se recicla un proceso de trabajo, se purgan las solicitudes en cola y en ejecución, y un nuevo proceso se inicia simultáneamente para atender las nuevas solicitudes. El elemento **reciclaje/periodicRestart** es por aplicación, lo que significa que cada uno de los atributos de la tabla siguiente tiene particiones por aplicación.

**System. applicationHost/applicationPools/ApplicationPoolDefaults/reciclaje/periodicRestart**

|Atributo|Descripción|Default|
|--- |--- |--- |
|memoria|Habilitar el reciclaje de procesos si el consumo de memoria virtual supera el límite especificado en kilobytes. Se trata de una configuración útil para equipos de 32 bits con un espacio de direcciones pequeño de 2 GB. Puede ayudar a evitar las solicitudes con error debido a errores de memoria insuficiente.|0|
|privateMemory|Habilitar el reciclaje de procesos si las asignaciones de memoria privadas superan un límite especificado en kilobytes.|0|
|solicitudes|Habilitar el reciclaje de procesos después de un determinado número de solicitudes.|0|
|time|Habilitar el reciclaje de procesos después de un período de tiempo especificado.|29:00:00|


## <a name="dynamic-worker-process-page-out-tuning"></a>Trabajo dinámico: optimización de la página del proceso

A partir de Windows Server 2012 R2, IIS ofrece la opción de configurar el proceso de trabajo para que se suspenda después de que haya estado inactivo durante un tiempo (además de la opción de finalizar, que existía desde IIS 7).

El propósito principal de las características de desactivación de procesos de trabajo inactivos y de finalización de procesos de trabajo inactivos es conservar el uso de memoria en el servidor, ya que un sitio puede consumir mucha memoria aunque esté sentado allí, escuchando. En función de la tecnología usada en el sitio (contenido estático frente a ASP.NET y otros marcos de trabajo), la memoria usada puede estar en cualquier parte entre unos 10 MB y cientos de MB, lo que significa que si el servidor está configurado con muchos sitios, la configuración de los valores más eficaces para los sitios puede mejorar drásticamente el rendimiento de los sitios activos y suspendidos.

Antes de entrar en detalles, debemos tener en cuenta que, si no hay ninguna restricción de memoria, probablemente sea mejor establecer simplemente los sitios para que nunca se suspendan o finalicen. Después de todo, thereâs poco valor al finalizar un proceso de trabajo si es el único en el equipo.

**Tenga en cuenta**  en caso de que el sitio ejecute código inestable, como código con una pérdida de memoria o, de lo contrario, inestable, establecer el sitio para que finalice en modo inactivo puede ser una alternativa rápida y desfasada para corregir el error de código. Esto no es algo que se fomente, pero, en una solución, puede ser mejor usar esta característica como un mecanismo de limpieza, mientras que una solución más permanente está en el trabajo.\]

Ti 

Otro factor a tener en cuenta es que si el sitio usa una gran cantidad de memoria, el propio proceso de suspensión realiza un peaje, ya que el equipo tiene que escribir los datos usados por el proceso de trabajo en el disco. Si el proceso de trabajo utiliza una gran cantidad de memoria, la suspensión podría ser más costosa que el costo de tener que esperar a que se inicie la copia de seguridad.

Para sacar el máximo partido de la característica de suspensión de procesos de trabajo, debe revisar los sitios en cada grupo de aplicaciones y decidir qué se debe suspender, que debe finalizar y que debe estar activo indefinidamente. Para cada acción y cada sitio, debe averiguar el período de tiempo de espera ideal.

Idealmente, los sitios que configurará para su suspensión o terminación son aquellos que tienen visitantes cada día, pero no suficientes para garantizar su mantenimiento activo en todo momento. Normalmente son sitios con unos 20 visitantes únicos de un día o menos. Puede analizar los patrones de tráfico mediante los archivos de registro del sitio y calcular el tráfico promedio diario.

Tenga en cuenta que una vez que un usuario específico se conecta al sitio, normalmente permanecerá en él durante al menos un tiempo, con lo que se realizan solicitudes adicionales, por lo que solo el recuento de solicitudes diarias podría no reflejar con precisión los patrones de tráfico reales. Para obtener una lectura más precisa, también puede usar una herramienta, como Microsoft Excel, para calcular el tiempo medio entre solicitudes. Por ejemplo:

||URL de solicitud|Tiempo de solicitud|Delta|
|--- |--- |--- |--- |
|1|/SourceSilverLight/Geosource.web/grosource.html|10:01||
|2|/SourceSilverLight/Geosource.web/sliverlight.js|10:10|0:09|
|3|/SourceSilverLight/Geosource.web/clientbin/geo/1.aspx|10:11|0:01|
|4|/lClientAccessPolicy.xml|10:12|0:01|
|5|/SourceSilverLight/GeosourcewebService/Service. asmx|10:23|0:11|
|6|/SourceSilverLight/Geosource. Web/GeoSearchServer... ¦.|11:50|1:27|
|7|/rest/Services/CachedServices/Silverlight_load_la... ¦|12:50|1:00|
|8|/rest/Services/CachedServices/Silverlight_basemap... ¦.|12:51|0:01|
|9|/rest/Services/DynamicService/Silverlight_basemap... ¦.|12:59|0:08|
|10|/rest/Services/CachedServices/Ortho_2004_cache. as...|13:40|0:41|
|11|/rest/Services/CachedServices/Ortho_2005_cache. js|13:40|0:00|
|12|/rest/Services/CachedServices/OrthoBaseEngine.aspx|13:41|0:01|

Sin embargo, la parte más difícil es averiguar qué configuración aplicar para tener sentido. En nuestro caso, el sitio obtiene un grupo de solicitudes de usuarios y en la tabla anterior se muestra que se ha producido un total de 4 sesiones únicas en un período de 4 horas. Con la configuración predeterminada de la suspensión de procesos de trabajo del grupo de aplicaciones, el sitio se terminaría después del tiempo de espera predeterminado de 20 minutos, lo que significa que cada uno de estos usuarios experimentaría el ciclo de la marcha del sitio. Esto lo convierte en un candidato ideal para la suspensión de procesos de trabajo, ya que, en la mayoría de los tiempos, el sitio está inactivo y, por tanto, suspenderlo ahorraría recursos y permitir que los usuarios lleguen al sitio casi al instante.

Una nota final y muy importante sobre esto es que el rendimiento del disco es fundamental para esta característica. Dado que la suspensión y el proceso de reactivación implican la escritura y la lectura de una gran cantidad de datos en el disco duro, se recomienda encarecidamente usar un disco rápido para ello. Las unidades de estado sólido (SSD) son ideales y muy recomendables para esto, y debe asegurarse de que el archivo de paginación de Windows está almacenado en él (si el propio sistema operativo no está instalado en la SSD, configure el sistema operativo para que le mueva el archivo de paginación).

Tanto si usa una SSD como si no, también se recomienda corregir el tamaño del archivo de paginación para que pueda escribir en él los datos de la página sin cambiar el tamaño de los archivos. El cambio de tamaño de los archivos de paginación puede producirse cuando el sistema operativo necesita almacenar los datos en el archivo de paginación, ya que, de forma predeterminada, Windows está configurado para ajustar automáticamente su tamaño en función de las necesidades. Al establecer el tamaño en uno fijo, puede evitar un cambio de tamaño y mejorar el rendimiento.

Para configurar un tamaño de archivo de página prefijado, debe calcular su tamaño ideal, que depende de cuántos sitios se van a suspender y cuánta memoria consumen. Si el promedio es 200 MB para un proceso de trabajo activo y tiene 500 sitios en los servidores que se van a suspender, el archivo de paginación debe tener al menos (200 \* 500) MB en el tamaño base del archivo de paginación (por lo que base + 100 GB en nuestro ejemplo).

**Nota:** Cuando los sitios se suspenden, consumirán aproximadamente 6 MB cada uno, por lo que en nuestro caso, el uso de memoria si se suspenden todos los sitios sería de aproximadamente 3 GB. Sin embargo, en realidad, es probable que youâre nunca se suspenda al mismo tiempo.

 
## <a name="transport-layer-security-tuning-parameters"></a>Parámetros de ajuste de seguridad de la capa de transporte

El uso de la seguridad de la capa de transporte (TLS) impone un costo de CPU adicional. El componente más caro de TLS es el costo de establecer un establecimiento de sesión porque implica un protocolo de enlace completo. La reconexión, el cifrado y el descifrado también se agregan al costo. Para mejorar el rendimiento de TLS, haga lo siguiente:

-   Habilite HTTP Keep-Alive para sesiones TLS. Esto elimina los costos del establecimiento de la sesión.

-   Reutilizar sesiones cuando sea necesario, especialmente con el tráfico no persistente.

-   Aplique el cifrado de forma selectiva solo a las páginas o partes del sitio que lo necesiten, en lugar de hacerlo en todo el sitio.

**Note**
-   Las claves mayores proporcionan más seguridad, pero también usan más tiempo de CPU.

-   Es posible que no sea necesario cifrar todos los componentes. Sin embargo, mezclar HTTP sin formato y HTTPS podría producir una advertencia emergente que indica que no todo el contenido de la página es seguro.

 
## <a name="internet-server-application-programming-interface-isapi"></a>Interfaz de programación de aplicaciones para servidores de Internet (ISAPI)

No es necesario ningún parámetro de ajuste especial para las aplicaciones ISAPI. Si escribe una extensión ISAPI privada, asegúrese de que está escrita para el rendimiento y el uso de recursos.

## <a name="managed-code-tuning-guidelines"></a>Directrices para la optimización de código administrado

El modelo de canalización integrada en IIS 10,0 permite un alto grado de flexibilidad y extensibilidad. Los módulos personalizados que se implementan en código nativo o administrado se pueden insertar en la canalización, o pueden reemplazar a los módulos existentes. Aunque este modelo de extensibilidad ofrece comodidad y simplicidad, debe tener cuidado antes de insertar nuevos módulos administrados que se enlazan a eventos globales. Agregar un módulo administrado global significa que todas las solicitudes, incluidas las solicitudes de archivos estáticos, deben tocar código administrado. Los módulos personalizados son susceptibles a eventos como la recolección de elementos no utilizados. Además, los módulos personalizados agregan un costo de CPU significativo debido a la serialización de datos entre código nativo y administrado. Si es posible, debe establecer la condición previa en managedHandler para el módulo administrado.

Para obtener un mejor rendimiento de inicio en frío, asegúrese de precompilar el sitio web de ASP.NET o aprovechar la característica de inicialización de aplicaciones de IIS para preparar la aplicación.

Si el estado de sesión no es necesario, asegúrese de que lo desactiva para cada página.

Si hay muchas operaciones enlazadas a e/s, intente usar la versión asincrónica de las API pertinentes, lo que le proporcionará una mayor escalabilidad.

Además, el uso de la caché de resultados correctamente también mejorará el rendimiento del sitio Web.

Al ejecutar varios hosts que contienen scripts de ASP.NET en modo aislado (un grupo de aplicaciones por sitio), supervise el uso de memoria. Asegúrese de que el servidor tiene suficiente memoria RAM para el número esperado de grupos de aplicaciones que se ejecutan simultáneamente. Considere la posibilidad de usar varios dominios de aplicación en lugar de varios procesos aislados.


## <a name="other-issues-that-affect-iis-performance"></a>Otros problemas que afectan al rendimiento de IIS

Los problemas siguientes pueden afectar al rendimiento de IIS:

-   Instalación de filtros que no reconocen la memoria caché

    La instalación de un filtro que no sea compatible con la memoria caché de HTTP hace que IIS deshabilite por completo el almacenamiento en caché, lo que provoca un bajo rendimiento. Los Filtros ISAPI que se escribieron antes de IISÂ 6,0 pueden producir este comportamiento.

-   Solicitudes de Common Gateway Interface (CGI)

    Por motivos de rendimiento, no se recomienda el uso de aplicaciones CGI para atender solicitudes con IIS. La creación y eliminación frecuente de procesos CGI conlleva una sobrecarga considerable. Las mejores alternativas incluyen el uso de FastCGI, scripts de aplicación ISAPI y scripts ASP o ASP.NET. El aislamiento está disponible para cada una de estas opciones.

## <a name="see-also"></a>Vea también
- [Ajuste del rendimiento del servidor Web](index.md) 
- [Optimización de HTTP 1.1/2](http-performance.md)
