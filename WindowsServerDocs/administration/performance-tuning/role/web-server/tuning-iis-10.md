---
title: Optimización de IIS 10.0
description: Recommmendations para los servidores web de IIS 10.0 en Windows Server 16 de optimización del rendimiento
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: DavSo; Ericam; YaShi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 16ea5c857d99e8a69f528e81178911236341dbb8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869856"
---
# <a name="tuning-iis-100"></a>Optimización de IIS 10.0

Internet Information Services (IIS) 10.0 se incluye con Windows Server 2016. Usa un modelo de proceso similar de IIS 8.5 e IIS 7.0. Un controlador de web de modo kernel (http.sys) recibe y enruta las solicitudes HTTP y satisface las solicitudes de su caché de respuesta. Registre los procesos de trabajo para subespacios de dirección URL y http.sys enruta la solicitud al proceso adecuado (o conjunto de procesos para los grupos de aplicaciones).

HTTP.sys es responsable de la administración de conexiones y control de la solicitud. La solicitud se sirven desde la memoria caché HTTP.sys o pasa a un proceso de trabajo para controlar aún más. Pueden configurar varios procesos de trabajo, que proporciona aislamiento con un menor costo. Para obtener más información acerca de cómo funciona el control de solicitudes, consulte la figura siguiente:

![solicitudes en iis 10.0](../../media/perftune-guide-iis-request-handling.png)

HTTP.sys se incluye una memoria caché la respuesta. Cuando una solicitud coincide con una entrada en la caché de respuestas, HTTP.sys envía la respuesta de la caché directamente desde el modo kernel. Algunas plataformas de aplicación web, como ASP.NET, ofrecen mecanismos para habilitar cualquier contenido dinámico que se almacenen en la memoria caché de modo kernel. El controlador de archivo estático en IIS 10.0 automáticamente almacena en caché los archivos solicitados con frecuencia en http.sys.

Dado que un servidor web tiene componentes de modo de núcleo y modo de usuario, se deben ajustar ambos componentes para un rendimiento óptimo. Por lo tanto, la optimización de IIS 10.0 para una carga de trabajo específica incluye la configuración siguiente:

-   HTTP.sys y la memoria caché de modo de kernel asociados

-   Los procesos de trabajo y de IIS de modo de usuario, incluida la configuración del grupo de aplicación

-   Ciertos parámetros de ajuste que afectan al rendimiento

Las siguientes secciones explican cómo configurar los aspectos de modo de núcleo y modo de usuario de IIS 10.0.

## <a name="kernel-mode-settings"></a>Configuración del modo de kernel

Configuración relacionada con el rendimiento de HTTP.sys se dividen en dos amplias categorías: administración y la solicitud de conexión y administración de caché. Toda la configuración del registro se almacena en la entrada del registro siguiente:

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters
```

**Tenga en cuenta**   si ya se está ejecutando el servicio HTTP, debe reiniciar para que los cambios surtan efecto.

Â 

## <a name="cache-management-settings"></a>Configuración de administración de memoria caché

Una ventaja que ofrece HTTP.sys es una caché de modo kernel. Si la respuesta está en la caché de modo kernel, puede satisfacer una solicitud HTTP completamente desde el modo de núcleo, lo que reduce significativamente el costo de CPU de la solicitud de control. Sin embargo, la memoria caché de modo de núcleo de IIS 10.0 se basa en la memoria física y el costo de una entrada es la memoria que ocupa.

Una entrada en la memoria caché es útil sólo cuando se utiliza. Sin embargo, la entrada siempre consume memoria física, si usa la entrada. Debe evaluar la utilidad de un elemento en la memoria caché (los ahorros de poder utilizarlo desde la memoria caché) y su costo (la memoria física ocupada) durante la vigencia de la entrada teniendo en cuenta los recursos disponibles (CPU y memoria física) y la carga de trabajo requisitos. HTTP.sys intenta mantener solo elementos útiles activamente a los que accede en la memoria caché, pero pueden aumentar el rendimiento del servidor web por la optimización de la memoria caché de HTTP.sys para cargas de trabajo determinados.

Estas son algunas configuraciones bastante útiles para la caché de modo kernel de HTTP.sys:

-   **UriEnableCache** valor predeterminado: 1

    Un valor distinto de cero permite la respuesta de modo kernel y el almacenamiento en caché. La mayoría de las cargas de trabajo, la memoria caché debe permanecer habilitada. Considere la posibilidad de deshabilitar la caché si espera una respuesta muy baja y un fragmento de almacenamiento en caché.

-   **UriMaxCacheMegabyteCount** Default value: 0

    Un valor distinto de cero que especifica la memoria máxima que está disponible para la caché de modo kernel. El valor predeterminado, 0, permite al sistema ajustar automáticamente la cantidad de memoria está disponible en la memoria caché.

    **Tenga en cuenta** especificando el tamaño de establece sólo el máximo y el sistema no permiten la memoria caché de alcanzar el tamaño máximo del conjunto.

    Â 

-   **UriMaxUriBytes** valor predeterminado: 262144 bytes (256 KB)

    El tamaño máximo de una entrada en la caché de modo kernel. No se almacenan en las respuestas o fragmentos más grandes. Si tiene suficiente memoria, considere la posibilidad de aumentar el límite. Si la memoria es limitada y otros más pequeños son exclusión entradas grandes, puede resultar útil reducir el límite.

-   **UriScavengerPeriod** valor predeterminado: 120 segundos

    La caché HTTP.sys se analiza periódicamente mediante un análisis y las entradas que no tienen acceso entre los exámenes de compactación se quitan. Establecimiento del período de compactación en un valor alto reduce el número de exámenes de compactación. Sin embargo, puede aumentar el uso de memoria caché porque las entradas más antiguas y a los que accede con menos frecuencia pueden permanecer en la memoria caché. Al establecer el período demasiado baja hace que los exámenes de compactación más frecuentes, y puede dar lugar a demasiados vaciados y renovación de caché.

## <a name="request-and-connection-management-settings"></a>Configuración de administración de conexión y solicitud

En Windows Server 2016, HTTP.sys administra las conexiones automáticamente. Ya no se usa la configuración del registro siguiente:

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


## <a name="user-mode-settings"></a>Configuración del modo de usuario

La configuración de esta sección afecta al comportamiento de proceso de trabajo de IISÂ 10.0. La mayoría de estas opciones se puede encontrar en el archivo de configuración XML siguiente:

%SystemRoot%\\system32\\inetsrv\\config\\applicationHost.config

Usar Appcmd.exe, la consola de administración de IIS 10.0, el WebAdministration o los Cmdlets de PowerShell IISAdministration para cambiarlos. Mayoría de las configuraciones se detecta automáticamente y no requieren un reinicio del servidor de aplicaciones web o los procesos de trabajo de IIS 10.0. Para obtener más información sobre el archivo applicationHost.config, consulte [Introducción a ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig).


## <a name="ideal-cpu-setting-for-numa-hardware"></a>Configuración de la CPU idóneo para hardware NUMA

A partir de 2016 de Windows, IIS 10.0 admite la asignación automática de la CPU ideal para sus subprocesos de grupo de subprocesos mejorar el rendimiento y escalabilidad en hardware NUMA. Esta característica está habilitada de forma predeterminada y puede configurarse a través de la clave del registro siguiente:

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\InetInfo\Parameters\ThreadPoolUseIdealCpu
```

Con esta característica habilitada, el Administrador de IIS subproceso hace su mejor esfuerzo para distribuir uniformemente los subprocesos de grupo IIS en todas las CPU en todos los nodos NUMA en función de sus cargas actuales. En general, se recomienda mantener esta configuración ha cambiado para hardware NUMA predeterminada.

**Tenga en cuenta**   es diferente desde el trabajo proceso NUMA nodo Configuración de asignación (numaNodeAssignment y numaNodeAffinityMode) introducida en la configuración de la CPU ideal [configuración de la CPU para un grupo de aplicaciones](https://www.iis.net/configreference/system.applicationhost/applicationpools/add/cpu). La configuración de la CPU ideal afecta al modo en que IIS distribuye sus subprocesos de grupo, mientras que la configuración de asignación de nodo de trabajo proceso NUMA determina en qué nodo NUMA se inicia un proceso de trabajo.

## <a name="user-mode-cache-behavior-settings"></a>Configuración de comportamiento de caché de modo de usuario

En esta sección se describe la configuración que afectan al comportamiento de almacenamiento en caché en IISÂ 10.0. La caché de modo de usuario se implementa como un módulo que realiza escuchas para los eventos de almacenamiento en caché globales que son generados por la canalización integrada. Para deshabilitar completamente la caché de modo de usuario, quite el módulo de FileCacheModule (cachfile.dll) en la lista de los módulos instalados en la sección de configuración system.webServer/globalModules en el archivo applicationHost.config.

**system.webServer/caching**

|Atributo|Descripción|Default|
|--- |--- |--- |
|Enabled|Deshabilita la caché IIS de modo de usuario cuando se establece en **False**. Cuando la memoria caché alcanza la tasa es muy pequeño, que puede deshabilitar la memoria caché completamente para evitar la sobrecarga que está asociada con la ruta de acceso del código de memoria caché. Deshabilitar la caché de modo de usuario no deshabilita la caché de modo kernel.|True|
|enableKernelCache|Deshabilita la caché de modo kernel cuando se establece en **False**.|True|
|maxCacheSize|Limita el tamaño de caché de modo de usuario IIS al tamaño especificado en Megabytes. IIS ajusta el valor predeterminado dependiendo de la memoria disponible. Elija el valor con cuidado según el tamaño del conjunto de frecuentemente tiene acceso a archivos frente a la cantidad de RAM o el espacio de direcciones del proceso IIS.|0|
|maxResponseSize|Almacena en caché los archivos hasta el tamaño especificado. El valor real depende del número y tamaño de los archivos más grandes en el conjunto de datos frente a la memoria RAM disponible. Almacenamiento en caché grande, pueden reducir solicitados con frecuencia los archivos de uso de CPU, acceso al disco y las latencias asociadas.|262144|

## <a name="compression-behavior-settings"></a>Configuración de comportamiento de compresión

A partir de 7.0 de IIS comprime contenido estático de forma predeterminada. Además, la compresión de contenido dinámico está habilitada de forma predeterminada cuando se instala el DynamicCompressionModule. La compresión reduce el uso de ancho de banda, pero aumenta el uso de CPU. Contenido comprimido se almacena en la caché de modo kernel si es posible. A partir de 8.5, IIS permite la compresión se controlan de forma independiente para el contenido estático y dinámico. Contenido estático suele hacer referencia a contenido que no cambia, como archivos GIF o HTM. Contenido dinámico se genera normalmente mediante scripts o código en el servidor, es decir, las páginas ASP.NET. Puede personalizar la clasificación de cualquier extensión determinada como estática o dinámica.

Para deshabilitar completamente la compresión, quite StaticCompressionModule y DynamicCompressionModule desde la lista de módulos en la sección system.webServer/globalModules en applicationHost.config.

**system.webServer/httpCompression**

|Atributo|Descripción|Default|
|--- |--- |--- |
|staticCompression-EnableCpuUsage<br><br>staticCompression-DisableCpuUsage<br><br>dynamicCompression-EnableCpuUsage<br><br>dynamicCompression-DisableCpuUsage|Habilita o deshabilita la compresión si el porcentaje actual de uso de CPU está por encima o debajo de los límites especificados.<br><br>A partir de IIS 7.0, la compresión se deshabilita automáticamente si estable CPU aumenta por encima del umbral de deshabilitación. La compresión está habilitada si CPU cae por debajo del umbral de habilitación.|50, 50, 100 y 90, respectivamente|
|Directorio|Especifica el directorio en el que las versiones comprimidas de los archivos estáticos temporalmente se almacenan y almacenar en caché. Considere la posibilidad de mover este directorio fuera de la unidad del sistema si se tiene acceso con frecuencia.|Archivos temporales comprimidos %SystemDrive%\inetpub\temp\IIS|
|doDiskSpaceLimiting|Especifica si existe un límite de espacio en disco pueden ocupar todos los archivos comprimidos. Archivos comprimidos se almacenan en el directorio de compresión especificado por el **directory** atributo.|True|
|maxDiskSpaceUsage|Especifica el número de bytes de espacio en disco que pueden ocupar los archivos comprimidos en el directorio de compresión.<br><br>Esta configuración es posible que tenga que aumentar si el tamaño total de todo el contenido comprimido es demasiado grande.|100 MB|

**system.webServer/urlCompression**

|Atributo|Descripción|Default|
|--- |--- |--- |
|doStaticCompression|Especifica si se comprime contenido estático.|True|
|doDynamicCompression|Especifica si se comprime el contenido dinámico.|True|

**Tenga en cuenta** para los servidores que ejecutan IIS 10.0 que tienen poco uso promedio de CPU, considere la posibilidad de habilitar la compresión de contenido dinámico, especialmente si las respuestas son grandes. En primer lugar debe realizarse en un entorno de prueba para evaluar el efecto sobre el uso de CPU desde la línea base.


### <a name="tuning-the-default-document-list"></a>Optimización de la lista de documentos predeterminados

El módulo de documento predeterminado controla las solicitudes HTTP para la raíz de un directorio y las traduce en las solicitudes de un archivo específico, como Default.htm o Index.htm. En promedio, aroundÂ 25 por ciento de todas las solicitudes de Internet pasan por la ruta de acceso de documento predeterminado. Esto varía considerablemente para sitios individuales. Cuando una solicitud HTTP no especifica un nombre de archivo, el módulo de documento predeterminada busca en la lista de documentos predeterminado permitido para cada nombre en el sistema de archivos. Esto puede afectar negativamente el rendimiento, especialmente si alcanzar el contenido requiere realizar una red de ida y vuelta de ida y vuelta o tocando un disco.

Puede evitar la sobrecarga deshabilitando selectivamente documentos predeterminados y al reducir o la ordenación de la lista de documentos. Para los sitios Web que usan un documento predeterminado, debe reducir la lista solo los tipos de documento predeterminado que se usan. Además, ordenar la lista para que comience con más frecuencia a nombre de archivo de documento predeterminado que se tiene acceso.

Selectivamente puede establecer el comportamiento de documento predeterminado en direcciones URL concretas mediante la personalización de la configuración dentro de una etiqueta de ubicación en el archivo applicationHost.config o insertando un archivo web.config directamente en el directorio de contenido. Esto permite un enfoque híbrido, lo que permite a los documentos predeterminados sólo donde son necesarios y establece el nombre de la lista para el archivo correcto para cada dirección URL.

Para deshabilitar completamente los documentos predeterminados, quite DefaultDocumentModule de la lista de módulos en la sección system.webServer/globalModules en applicationHost.config.

**system.webServer/defaultDocument**

|Atributo|Descripción|Default|
|--- |--- |--- |
|enabled|Especifica que los documentos predeterminados están habilitados.|True|
|&lt;archivos&gt; elemento|Especifica los nombres de archivo que están configurados como documentos predeterminados.|La lista predeterminada es Default.htm, Default.asp, Index.htm, Index.html, Iisstart.htm y Default.aspx.|

## <a name="central-binary-logging"></a>Registro binario central

Cuando la sesión del servidor tiene numerosos grupos de direcciones URL en él, el proceso de crear cientos de archivos de registro con formato de los grupos de direcciones URL individuales y escribir los datos de registro en un disco puede consumir rápidamente valiosos recursos de CPU y memoria, creando así rendimiento y problemas de escalabilidad. El registro binario centralizado minimiza la cantidad de recursos del sistema que se usan para el registro, mientras que al mismo tiempo que proporciona datos detallados de registro para las organizaciones que lo requieran. Análisis de registros en formato binario, requiere una herramienta posteriores al procesamiento.

Puede habilitar el registro binario central estableciendo el atributo centralLogFileMode a CentralBinary y estableciendo el **habilitado** atributo **True**. Considere la posibilidad de mover la ubicación del archivo de registro central desactivar la partición del sistema y en una unidad de registro dedicado para evitar la contención entre las actividades del sistema y las actividades de registro.

**system.applicationHost/log**

|Atributo|Descripción|Default|
|--- |--- |--- |
|centralLogFileMode|Especifica el modo de registro para un servidor. Cambie este valor a CentralBinary para habilitar el registro binario central.|Sitio|

**system.applicationHost/log/centralBinaryLogFile**

|Atributo|Descripción|Default|
|--- |--- |--- |
|enabled|Especifica si está habilitado el registro binario central.|False|
|Directorio|Especifica el directorio donde se escriben las entradas del registro.|%SystemDrive%\inetpub\logs\LogFiles|


## <a name="application-and-site-tunings"></a>Ajustes de aplicación y el sitio

Las siguientes opciones están relacionados con los ajustes del sitio y el grupo de aplicaciones.

**system.applicationHost/applicationPools/applicationPoolDefaults**

|Atributo|Descripción|Default|
|--- |--- |--- |
|queueLength|Indica a HTTP.sys cuántas solicitudes se ponen en cola para un grupo de aplicaciones antes de que se rechazan las solicitudes futuras. Cuando se supera el valor de esta propiedad, IIS rechaza las solicitudes posteriores con un error 503.<br><br>Considere la posibilidad de aumentar este valor para las aplicaciones que se comunican con los almacenes de datos de back-end de alta latencia si se observan 503 errores.|1000|
|enable32BitAppOnWin64|Cuando sea True, permite que una aplicación de 32 bits se ejecute en un equipo que tiene un procesador de 64 bits.<br><br>Considere la posibilidad de habilitar el modo de 32 bits si el consumo de memoria es una preocupación. Dado que los tamaños de puntero y tamaños de instrucción son más pequeños, las aplicaciones de 32 bits utilizan menos memoria que las aplicaciones de 64 bits. El inconveniente de que se ejecutan las aplicaciones de 32 bits en un equipo de 64 bits es que el espacio de direcciones de modo de usuario está limitado a 4 GB.|False|

**system.applicationHost/sites/VirtualDirectoryDefault**

|Atributo|Descripción|Default|
|--- |--- |--- |
|allowSubDirConfig|Especifica si IIS busca los archivos web.config en directorios de contenido inferiores actual nivel (True) o no busca los archivos web.config en directorios de contenido inferiores al nivel actual (False). Si se imponen una limitación simple, lo que permite la configuración solo en los directorios virtuales, 10.0 IISÂ puede saber que, a menos que  **/ &lt;nombre&gt;.htm** es un directorio virtual, no debe buscar un archivo de configuración. Omitiendo las operaciones de archivo adicionales puede mejorar significativamente el rendimiento de sitios Web que tienen un gran conjunto de contenido estático a los que accede al azar.|True|

## <a name="managing-iis-100-modules"></a>Administración de módulos de IIS 10.0

IIS 10.0 se ha factorizado en varios módulos extensibles de usuario para admitir una estructura modular. Esta factorización tiene un pequeño costo. Para cada módulo la canalización integrada debe llamar al módulo para cada evento que es relevante para el módulo. Esto ocurre independientemente de si el módulo debe realizar cualquier trabajo. Puede ahorrar ciclos de CPU y memoria mediante la eliminación de todos los módulos que no son relevantes para un determinado sitio Web.

Un servidor web que está optimizado para los archivos estáticos simple puede incluir solo los cinco módulos siguientes: UriCacheModule, HttpCacheModule, StaticFileModule, AnonymousAuthenticationModule y HttpLoggingModule.

Para quitar los módulos de applicationHost.config, quite todas las referencias al módulo de las secciones system.webServer/handlers y System.webServer/Modules además de quitar la declaración de módulo en system.webServer/globalModules.

## <a name="classic-asp-settings"></a>Configuración de ASP clásico

El costo de procesar una solicitud ASP clásica principal incluye la inicialización de un motor de scripts, compilar el script ASP solicitado en una plantilla de ASP y ejecutar la plantilla en el motor de scripts. Mientras que el costo de ejecución de la plantilla depende de la complejidad de la secuencia de comandos ASP solicitado, módulo ASP clásico de IIS puede almacenar en caché los motores de script en las plantillas de memoria y memoria caché en memoria y disco (sólo si se desborda la caché de plantilla en memoria) para mejorar el rendimiento en Escenarios de enlazado a la CPU.

Las siguientes opciones se usan para configurar la memoria caché de plantillas ASP clásico y la memoria caché del motor de secuencia de comandos, y no afectan a la configuración de ASP.NET.

**system.webServer/asp/cache**

|Atributo|Descripción|Default|
|--- |--- |--- |
|diskTemplateCacheDirectory|El nombre del directorio que utiliza ASP para almacenar plantillas compiladas cuando se desborda la caché en memoria.<br><br>Recomendación: Conjunto a un directorio que no es muy había utilizado, por ejemplo, una unidad que no se comparte con el registro del sistema operativo, IIS, u otros han accedido a contenido con frecuencia.|%SystemDrive%\inetpub\temp\ASP plantillas compiladas|
|maxDiskTemplateCacheFiles|Especifica el número máximo de plantillas ASP compiladas que pueden almacenarse en caché en el disco.<br><br>Recomendación: Se establece en el valor máximo de 0x7FFFFFFF.|2000|
|scriptFileCacheSize|Este atributo especifica el número máximo de plantillas ASP compiladas que pueden almacenarse en caché en memoria.<br><br>Recomendación: Se establece en al menos tantos como el número de solicitados con frecuencia secuencias de comandos ASP atendidas por un grupo de aplicaciones. Si es posible, se establece en todas las plantillas ASP como permitir que los límites de memoria.|500|
|scriptEngineCacheMax|Especifica el número máximo de motores de script que se mantendrá en la memoria caché.<br><br>Recomendación: Se establece en al menos tantos como el número de solicitados con frecuencia secuencias de comandos ASP atendidas por un grupo de aplicaciones. Si es posible, establezca como muchos motores de script como el límite de memoria permite.|250|

**system.webServer/asp/limits**

|Atributo|Descripción|Default|
|--- |--- |--- |
|processorThreadMax|Especifica el número máximo de subprocesos de trabajo que puede crear ASP por cada procesador. Aumentar si la configuración actual no es suficiente para controlar la carga, lo que puede provocar errores cuando atiende las solicitudes o que debajo del uso de recursos de CPU.|25|

**system.webServer/asp/comPlus**

|Atributo|Descripción|Default|
|--- |--- |--- |
|executeInMta|Establecido en **True** si se detectan errores mientras que IIS es servir contenido ASP. Esto puede ocurrir, por ejemplo, al hospedar varios sitios aislados en que cada sitio se ejecuta en su propio proceso de trabajo. Normalmente, se informa de errores de COM + en el Visor de eventos. Esta configuración habilita el modelo de apartamento multiproceso en ASP.|False|


## <a name="aspnet-concurrency-setting"></a>Configuración de simultaneidad de ASP.NET

### <a name="aspnet-35"></a>ASP.NET 3.5
De forma predeterminada, ASP.NET limita la simultaneidad de la solicitud para reducir el consumo de memoria de estado estable en el servidor. Las aplicaciones de alta simultaneidad podrían necesitar ajustar algunas opciones para mejorar el rendimiento general. Puede cambiar esta configuración en el archivo aspnet.config:

``` syntax
<system.web>
  <applicationPool maxConcurrentRequestsPerCPU="5000"/>
</system.web>
```

Es útil para poder usar los recursos en un sistema de la siguiente configuración:

-   **maxConcurrentRequestPerCpu** valor predeterminado: 5000

    Esta configuración limita el número máximo de las solicitudes ASP.NET en ejecución simultáneamente en un sistema. El valor predeterminado es conservador para reducir el consumo de memoria de las aplicaciones ASP.NET. Considere la posibilidad de aumentar este límite en sistemas que ejecutan aplicaciones que realizan operaciones de E/S sincrónicas, larga. En caso contrario, los usuarios pueden experimentar una latencia alta debido a errores de solicitud o puesta en cola debido a la superación de los límites de cola bajo una carga alta cuando se usa la configuración predeterminada.

### <a name="aspnet-46"></a>ASP.NET 4.6
Además de la configuración maxConcurrentRequestPerCpu, ASP.NET 4.7 también proporciona opciones para mejorar el rendimiento en las aplicaciones que dependen mucho de operación asincrónica. La configuración puede cambiarse en el archivo aspnet.config.

``` syntax
<system.web>
  <applicationPool percentCpuLimit="90" percentCpuLimitMinActiveRequestPerCpu="100"/>
</system.web>
```

-   **percentCpuLimit** valor predeterminado: Solicitud asincrónica 90 tiene algunos problemas de escalabilidad cuando una carga enorme (más allá de las capacidades de hardware) se coloca en el escenario de este tipo. El problema es debido a la naturaleza de la asignación en escenarios asincrónicos. En estas condiciones, la asignación se realizará cuando se inicia la operación asincrónica, y se usarán cuando se complete. Entonces, s de es muy posible los objetos se han movido a la generación 1 o 2 por GC. Cuando esto sucede, aumenta la carga mostrará aumento en la solicitud por segundo (rps) hasta un punto. Una vez que pasamos ese punto, se iniciará el tiempo empleado en GC para convertirse en un problema y descienden empezará a dip, tener un efecto negativo de la escala. Para corregir el problema, cuando el uso de cpu supera el valor percentCpuLimit, las solicitudes se enviarán a la cola nativa de ASP.NET.
-   **percentCpuLimitMinActiveRequestPerCpu** valor predeterminado: Limitación de CPU 100 (valor percentCpuLimit) no se basa en el número de solicitudes, pero en cuánto cuesta son. Como resultado, podría haber unas cuantas solicitudes intensivo de CPU que ocasiona una copia de seguridad en la cola nativa sin ninguna forma vaciarla aparte de las solicitudes entrantes. Para solucionar este problme, percentCpuLimitMinActiveRequestPerCpu puede usarse para garantizar que una cantidad mínima de las solicitudes se atienden antes de la limitación se activa.

## <a name="worker-process-and-recycling-options"></a>Proceso de trabajo y las opciones de reciclaje

Puede configurar las opciones de reciclaje de procesos de trabajo IIS y proporcionar soluciones prácticas a situaciones agudas o eventos sin necesidad de intervención o restablecer un equipo o servicio. Estas situaciones y los eventos incluyen las pérdidas de memoria, aumenta la carga de memoria o los procesos de trabajo no responde o inactivo. En condiciones normales, reciclaje opciones que no sean necesarios y reciclaje puede desactivarse o el sistema puede configurarse para reciclar con muy poca frecuencia.

Puede habilitar el reciclaje de procesos para una aplicación determinada mediante la adición de atributos para el **reciclado/periodicRestart** elemento. El evento de reciclaje puede activarse mediante varios eventos que incluyen el uso de memoria, un número fijo de solicitudes y un período de tiempo fijo. Cuando se recicla un proceso de trabajo, se agotan las solicitudes en cola y en ejecución y simultáneamente se inicia un nuevo proceso para atender nuevas solicitudes. El **reciclado/periodicRestart** elemento es por aplicación, lo que significa que cada atributo en la tabla siguiente se crean particiones por la aplicación.

**system.applicationHost/applicationPools/ApplicationPoolDefaults/recycling/periodicRestart**

|Atributo|Descripción|Default|
|--- |--- |--- |
|memoria|Habilitar el reciclaje de procesos, si el consumo de memoria virtual supera el límite especificado en kilobytes. Se trata de una configuración útil para los equipos de 32 bits que tienen un pequeño, el espacio de direcciones de 2 GB. Puede ayudar a evitar las solicitudes con error debido a errores de memoria insuficiente.|0|
|privateMemory|Habilitar el reciclaje de procesos si las asignaciones de memoria privada superan un límite especificado en kilobytes.|0|
|requests|Habilitar el reciclaje de procesos después de cierto número de solicitudes.|0|
|time|Habilitar el reciclaje de procesos después de un período de tiempo especificado.|29:00:00|


## <a name="dynamic-worker-process-page-out-tuning"></a>Optimización dinámica de procesos de trabajo fuera de la página

A partir de Windows Server 2012 R2, IIS ofrece la opción de configuración de proceso de trabajo para suspender después de haber estado inactivas durante un tiempo (además de la opción de terminate, que existe desde IIS 7).

El propósito principal de page-out del proceso de trabajo inactivos y características de terminación de procesos de trabajador inactivo es conservar el uso de memoria en el servidor, puesto que un sitio puede consumir una gran cantidad de memoria, incluso si lo es simplemente sentado allí, escucha. Según la tecnología utilizada en el sitio (estático frente a contenido ASP.NET vs otros marcos), la memoria usada puede ser cualquier valor de alrededor de 10 MB a cientos de MB, y esto significa que si el servidor está configurado con muchos sitios, averiguar la configuración más eficaz para los sitios pueden mejorar drásticamente el rendimiento de sitios activos y suspendidos.

Antes de entrar en detalles, debemos tener en cuenta que si no hay ninguna restricción de memoria, probablemente es mejor establecer simplemente los sitios nunca suspender o finalizar. Después de todo, s thereâ poco valor en un trabajo de terminación procesar si es la única en el equipo.

**Tenga en cuenta**   en caso de que ejecute código inestable, como el código con una pérdida de memoria, el sitio o inestable en caso contrario, establecer el sitio para terminar en inactividad puede ser una alternativa a corregir el error en el código desprolijo. Esto no es algo que se recomienda, pero en un estudio, puede ser mejor usar esta característica como un mecanismo de limpieza, mientras que una solución más permanente que se encuentra en curso.\]

Â 

Otro factor a considerar es que si el sitio utiliza una gran cantidad de memoria, a continuación, el propio proceso de suspensión toma un número de teléfono, ya que el equipo tiene que escribir los datos utilizados por el proceso de trabajo en el disco. Si el proceso de trabajo está usando un gran bloque de memoria, podría ser más costosa que el costo de tener que esperar a que comience la copia de seguridad, a continuación, la suspensión.

Para hacer lo mejor de la característica de suspensión del proceso de trabajo, deberá revisar los sitios en cada grupo de aplicaciones y decidir que debe suspenderse, que debe finalizar y que debe estar activa indefinidamente. Para cada sitio y cada acción, deberá averiguar el tiempo de espera ideal.

Idealmente, los sitios que configure para la suspensión o finalización son aquellos que tienen los visitantes cada día, pero no hay suficientes justificar mantenerla activa la todo el tiempo. Estos suelen ser sitios con aproximadamente 20 visitantes únicos de un día o menos. Puede analizar los patrones de tráfico mediante archivos de registro de la carpeta del sitio y calcular el tráfico diario promedio.

Tenga en cuenta que una vez que un usuario específico se conecta al sitio, normalmente permanecerá en ella durante al menos un tiempo, realizar solicitudes adicionales y así que contar solicitudes diarias pueden no reflejar exactamente los patrones de tráfico real. Para obtener una lectura más precisa, también puede usar una herramienta como Microsoft Excel, para calcular el promedio de tiempo entre las solicitudes. Por ejemplo:

||URL de solicitud|Tiempo de solicitud|Delta|
|--- |--- |--- |--- |
|1|/SourceSilverLight/Geosource.web/grosource.html|10:01||
|2|/SourceSilverLight/Geosource.web/sliverlight.js|10:10|0:09|
|3|/SourceSilverLight/Geosource.web/clientbin/geo/1.aspx|10:11|0:01|
|4|/lClientAccessPolicy.xml|10:12|0:01|
|5|/ SourceSilverLight/GeosourcewebService/Service.asmx|10:23|0:11|
|6|/ SourceSilverLight/Geosource.web/GeoSearchServer...¦.|11:50|1:27|
|7|/rest/Services/CachedServices/Silverlight_load_la...¦|12:50|1:00|
|8|/rest/Services/CachedServices/Silverlight_basemap...¦.|12:51|0:01|
|9|/rest/Services/DynamicService/ Silverlight_basemap...¦.|12:59|0:08|
|10|/rest/Services/CachedServices/Ortho_2004_cache.as...|13:40|0:41|
|11|/rest/Services/CachedServices/Ortho_2005_cache.js|13:40|0:00|
|12|/rest/Services/CachedServices/OrthoBaseEngine.aspx|13:41|0:01|

La parte difícil, sin embargo, es averiguar qué configuración se aplique para facilitar la lectura. En nuestro caso, el sitio Obtiene una serie de solicitudes de usuarios y la tabla anterior muestra que se ha producido un total de 4 sesiones únicas en un período de 4 horas. Con la configuración predeterminada para la suspensión del proceso de trabajo del grupo de aplicaciones, se terminaba con el sitio después de que el tiempo de espera predeterminado de 20 minutos, lo que significa cada uno de estos usuarios experimentará el ciclo de giro del sitio. Esto facilita un candidato ideal para la suspensión del proceso de trabajo, porque la mayoría del tiempo, el sitio está inactivo, por lo que su suspensión desea conservar los recursos y permitir que los usuarios acceder al sitio casi al instante.

Una nota final y muy importante acerca de esto es que el rendimiento de disco es fundamental para esta característica. Dado que la suspensión y reactivación proceso implican escribir y leer gran cantidad de datos en el disco duro, se recomienda encarecidamente usar discos rápidos para esto. Unidades de estado sólido (SSD) son ideal y muy recomendables para esto, y debe asegurarse de que el archivo de paginación de Windows se almacena en ella (si el propio sistema operativo no está instalado en la SSD, configurar el sistema operativo para mover el archivo de paginación a él).

Si utiliza una SSD o no, también se recomienda corregir el tamaño de la página archivo para dar cabida a escribir los datos de página horizontal en él sin cambiar el tamaño de archivo. Cambiar el tamaño de archivo de paginación puede producirse cuando el sistema operativo necesita almacenar datos en el archivo de paginación, porque de forma predeterminada, se configura Windows ajustar automáticamente su tamaño según necesite. Estableciendo el tamaño a uno fija, puede evitar que el cambio de tamaño y mejorar mucho el rendimiento.

Para configurar un tamaño fijo previamente, se necesitan calcular su tamaño ideal, que depende de cuántos sitios se puede suspender y la cantidad de memoria que consumen. Si el promedio es de 200 MB para un proceso de trabajo activos y tiene 500 sitios en los servidores que se pueden suspender, entonces el archivo de paginación debe ser como mínimo (200 \* 500) MB frente al tamaño de base de la página de archivo (por lo tanto, base + 100 GB en nuestro ejemplo).

**Tenga en cuenta** cuando se suspenden los sitios, consumirá aproximadamente 6 MB cada uno, por lo que en nuestro caso, el uso de memoria si se suspenden todos los sitios sería alrededor de 3 GB. En realidad, sin embargo, hace falta re probablemente nunca se va a hacer que se suspende al mismo tiempo.

 
## <a name="transport-layer-security-tuning-parameters"></a>Parámetros de ajuste de seguridad de capa de transporte

El uso de la seguridad de capa de transporte (TLS) impone el costo de CPU adicional. El componente más caro de TLS es el costo de establecer un establecimiento de la sesión, ya que implica un protocolo de enlace completo. La reconexión, cifrado y descifrado también agregan al costo. Para mejorar el rendimiento de TLS, realice lo siguiente:

-   Habilitar HTTP abiertas para las sesiones TLS. Esto elimina los costos de establecimiento de sesión.

-   Volver a usar sesiones cuando sea apropiado, especialmente con el tráfico que no es persistente.

-   Aplicar cifrado de forma selectiva sólo a las páginas o partes del sitio que lo necesitan, sino a todo el sitio.

**Nota:**
-   Las claves más largas proporcionan mayor seguridad, pero también usan más tiempo de CPU.

-   Todos los componentes no es posible que necesitan cifrarse. Sin embargo, podría producir la mezcla normal HTTP y HTTPS en un menú emergente de advertencia que no todo el contenido en la página es seguro.

 
## <a name="internet-server-application-programming-interface-isapi"></a>Interfaz de programación de aplicaciones de servidor de Internet (ISAPI)

No hay parámetros de ajuste especiales son necesarias para las aplicaciones ISAPI. Si escribe una extensión ISAPI privada, asegúrese de que está escrito para el rendimiento y el uso de recursos.

## <a name="managed-code-tuning-guidelines"></a>Directrices para la optimización de código administrado

El modelo de canalización integrada en IIS 10.0 permite un alto grado de flexibilidad y extensibilidad. Módulos personalizados que se implementan en código nativo o administrado se pueden insertar en la canalización, o pueden reemplazar módulos existentes. Aunque este modelo de extensibilidad ofrece la comodidad y la simplicidad, debe tener cuidado antes de insertar nuevos módulos administrados que se enlazan en eventos globales. Adición de que un módulo administrado global significa que todas las solicitudes, incluidas las solicitudes de archivo estático, se deben tocar el código administrado. Módulos personalizados son susceptibles a los eventos como la recolección. Además, los módulos personalizados agregar costo significativo de CPU debido a la serialización de datos entre código nativo y administrado. Si es posible, debe establecer la condición previa en managedHandler de módulo administrado.

Para obtener un mejor rendimiento de inicio en frío, asegúrese de que precompilar la función de inicialización de la aplicación de IIS ASP.NET sitio web también puede aprovechar para preparar la aplicación.

Si no se necesita el estado de sesión, asegúrese de que desactivar para cada página.

Si hay muchas E/S enlaza las operaciones, intente usar la versión asincrónica de las API relevantes que ofrecen mejor escalabilidad.

También usa memoria caché de resultados correctamente se también mejorar el rendimiento de su sitio web.

Al ejecutar varios hosts que contienen scripts ASP.NET en modo aislado (un grupo de aplicaciones por sitio), supervise el uso de memoria. Asegúrese de que el servidor tiene suficiente RAM para el número esperado de los grupos de aplicaciones en ejecución simultánea. Considere el uso de varios dominios de aplicación en lugar de varios procesos aislados.


## <a name="other-issues-that-affect-iis-performance"></a>Otros problemas que afectan al rendimiento de IIS

Los siguientes problemas pueden afectar al rendimiento de IIS:

-   Instalación de filtros que no son conscientes de la memoria caché

    La instalación de un filtro que no es compatible con HTTP-cache hace que IIS deshabilitar completamente el almacenamiento en caché, lo que da como resultado un rendimiento deficiente. Filtros ISAPI que se escribieron antes IISÂ 6.0 pueden producir este comportamiento.

-   Solicita Common Gateway Interface (CGI)

    Por motivos de rendimiento, no se recomienda el uso de las aplicaciones CGI para atender las solicitudes con IIS. Con frecuencia, crear y eliminar los procesos CGI implica una sobrecarga considerable. Mejores alternativas incluyen el uso de FastCGI, secuencias de comandos de aplicación de ISAPI y ASP o ASP.NET. Aislamiento está disponible para cada una de estas opciones.

# <a name="see-also"></a>Vea también
- [Optimización del rendimiento de servidor de Web](index.md) 
- [HTTP 1.1/2 optimización](http-performance.md)
