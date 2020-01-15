---
title: Guía de desarrollo de Server Performance Advisor Pack
description: Guía de desarrollo de Server Performance Advisor Pack
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cdf812f862534ba8cd07d4558e424faf3c56c699
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947146"
---
# <a name="server-performance-advisor-pack-development-guide"></a>Guía de desarrollo de Server Performance Advisor Pack

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 8, Windows 10

Esta guía de desarrollo de Microsoft Server Performance Advisor (SPA) proporciona directrices para ayudar a los desarrolladores y administradores del sistema a desarrollar paquetes de Advisor para analizar el rendimiento del servidor.

Se supone que está familiarizado con Registros y alertas de rendimiento (PLA), los contadores de rendimiento, la configuración del registro, el Instrumental de administración de Windows (WMI), el seguimiento de eventos para Windows (ETW) y Transact SQL (T-SQL).

Para obtener más información sobre el uso de SPA, consulte el [manual del usuario de Server Performance Advisor](server-performance-advisor-users-guide.md).

## <a name="spa-advisor-pack-overview"></a>Información general del paquete de Advisor de SPA


Un paquete de Advisor se diseña normalmente para un rol de servidor determinado y define lo siguiente:

* Datos que se van a recopilar a través de PLA, incluidos Instrumental de administración de Windows (WMI), contadores de rendimiento, configuración del registro, archivos y seguimiento de eventos para Windows (ETW)

* Reglas, que muestra alertas y recomendaciones

* Datos que se van a mostrar (datos sin procesar recopilados, datos agregados o listas de 10 principales)

* Estadísticas para ver un valor que cambia a lo largo del tiempo

* Valores de estadísticas que se pueden aplicar a tendencias

Un módulo de asesor incluye los siguientes elementos:

* **Metadatos XML** (ProvisionMetadata. xml)

    * Conjunto de recopilador de datos de [registros y alertas de rendimiento (PLA)](https://msdn.microsoft.com/library/windows/desktop/aa372635.aspx)

    * Diseño del informe

* **Scripts SQL**

    * Un procedimiento almacenado principal

    * Objetos SQL, como procedimientos almacenados y funciones definidas por el usuario

* El **archivo de esquema ETW** (Schema. Man) es opcional.

### <a name="advisor-pack-workflow"></a>Flujo de trabajo del paquete de Advisor

![flujo de trabajo del paquete de Advisor](../media/server-performance-advisor/spa-dev-guide-workflow.png)

En este diagrama de flujo, los círculos verdes representan paquetes de Advisor. Todos los demás círculos representan las fases que se ejecutan en el proceso de la plataforma SPA. SPA usa un paquete de Advisor para recopilar datos, importarlos en la base de datos, inicializar el entorno de ejecución y ejecutar scripts SQL.

### <a name="collect-data"></a>Recopilar datos

Cuando un módulo de asesor se pone en cola para un servidor determinado mediante SPA, el módulo de recopilación de datos consulta el XML del conjunto de recopiladores de datos del paquete de Advisor y recopila datos del servidor de destino. Los datos sin procesar se almacenan en un recurso compartido de archivos especificado por el usuario. La recopilación de datos no se detendrá hasta que se supere la duración de la ejecución de SPA designada por el usuario.

### <a name="import-data-into-the-database"></a>importar datos en la base de datos

Una vez completada la recopilación de datos, cada tipo de datos se importa en una tabla correspondiente en la base de datos de SQL Server. Por ejemplo, los valores del registro se importan en una tabla denominada \#registryKeys.

la importación de un archivo ETW requiere un archivo de esquema ETW para descodificar el archivo. ETL. El archivo de esquema ETW es un archivo XML. Se puede generar mediante tracerpt. exe, que se incluye con Windows. El archivo de esquema ETW solo es necesario cuando el módulo de Advisor necesita importar datos ETW.

### <a name="switch-to-low-user-rights"></a>Cambiar a derechos de usuario reducidos

El marco de SPA ajusta automáticamente los privilegios para minimizar el nivel de acceso de seguridad necesario. Dado que cualquiera puede desarrollar o modificar los paquetes de Advisor, es posible que un paquete de asesores contenga scripts SQL alterados. Para mitigar el riesgo de seguridad, cualquier script SQL de un módulo de asesor debe ejecutarse con derechos de usuario reducidos. Solo puede acceder a objetos de base de datos limitados, como tablas temporales y API de SPA, que se exponen como procedimientos almacenados. Los scripts SQL de un paquete de asesores pueden llamar a esos procedimientos de almacenamiento para interactuar con el marco de SPA.

### <a name="initialize-execution-environment"></a>Inicializar entorno de ejecución

Los paquetes de asesores pueden generar diferentes tipos de resultados, como notificaciones, recomendaciones, tablas de hechos, estadísticas y gráficos para las estadísticas. Los scripts SQL realizan determinados cálculos con los datos recopilados. Los resultados generados se almacenan en tablas temporales a través de las API públicas de SPA. en la fase de inicialización, se deben aprovisionar estas tablas temporales y otros recursos del sistema.

### <a name="run-sql-scripts"></a>Ejecutar scripts SQL

Hay un procedimiento almacenado principal, denominado por el desarrollador del paquete de Advisor. El marco de SPA llama a este procedimiento almacenado para iniciar el cálculo. El procedimiento almacenado consume los datos recopilados y comunica el resultado final al marco SPA.

### <a name="switch-to-administrative-rights"></a>Cambiar a derechos administrativos

Los derechos de administrador son necesarios para generar un informe. SPA controla totalmente la generación de informes. Es menos probable que se manipule.

### <a name="generate-a-report"></a>Generación de un informe

Antes de que se complete el procedimiento almacenado principal para un módulo de asesor, no se conservarán todos los resultados calculados, como notificaciones y estadísticas. Durante esta fase, el marco de SPA transfiere los resultados finales de las tablas temporales a las tablas en un formato determinado. Una vez completada esta fase, puede ver los informes mediante la consola de SPA.

## <a name="authoring-an-advisor-pack"></a>Creación de un paquete de asesores


### <a name="quick-guidelines"></a>Instrucciones rápidas

En el diagrama de flujo siguiente se describen los pasos necesarios para desarrollar un paquete de Advisor totalmente funcional. En esta sección también se incluyen ejemplos paso a paso para explicar mejor cada paso.

![proceso de desarrollo del paquete de Advisor](../media/server-performance-advisor/spa-dev-guide-dev-flowchart.png)

Normalmente, un paquete de asesores está estructurado de la manera siguiente:

Paquete de Advisor

ProvisionMetadata. XML

Scripts

Main. SQL

FUNC. SQL

Schema. Man

Cada paquete de Advisor debe tener un archivo denominado ProvisionMetadata. Xml. Define la información básica del paquete de Advisor, los datos que se recopilan, las notificaciones y las reglas, y cómo se debe almacenar y mostrar el informe. El marco de SPA usa esta información para generar una tabla temporal y luego transferir los resultados de la tabla temporal a una tabla a la que los usuarios puedan tener acceso.

Todos los scripts SQL de informe deben guardarse en una subcarpeta denominada **scripts**. Para fines de mantenimiento, se recomienda guardar los distintos objetos de base de datos en distintos archivos de SQL Server. Debe haber al menos un procedimiento almacenado como punto de entrada principal.

> [!NOTE]
> El archivo Schema. Man no es necesario a menos que el módulo de Advisor recopile seguimientos de ETW. Este archivo de esquema se usa para describir el esquema de los eventos ETW y para descodificar los eventos ETW.

### <a name="defining-basic-information"></a>Definir información básica

En esta sección se describen algunos de los elementos básicos que componen un paquete de asesores, incluidos ProvisionMetadata. XML y los atributos.

A continuación se encuentra un encabezado de ejemplo para el archivo ProvisionMetadata. xml:

``` syntax
<advisorPack
xmlns="https://microsoft.com/schemas/ServerPerformanceAdvisor/ap/2010"
name="Microsoft.ServerPerformanceAdvisor.CoreOS.V2"
displayName="Microsoft CoreOS Advisor Pack V2"
description="Microsoft CoreOS Advisor Pack"
author="Microsoft"
version="1.0"
frameworkversion="3.0"
minOSversion="6.0"
reportScript="ReportScript">
</advisorPack>
```

### <a name="advisor-pack-version"></a>Versión del paquete de Advisor

nombre del atributo: **versión**

Los desarrolladores de Advisor Pack pueden definir versiones principales y secundarias para el paquete de Advisor:

* Normalmente, una versión principal implica mejoras significativas. Los resultados generados por una versión anterior podrían no ser compatibles con el nuevo. Se recomienda incluir la versión principal en el nombre del módulo de Advisor.

* SPA permite actualizaciones de versiones secundarias cuando solo hay cambios menores sin problemas de incompatibilidad de datos.

para obtener más información sobre el control de versiones, vea [temas avanzados](#bkmk-advancedtopics).

### <a name="script-entry-point"></a>Punto de entrada de script

nombre del atributo: **reportScript**

El marco de SPA busca el nombre del procedimiento almacenado principal desde el punto de entrada del script y lo ejecuta de forma segura.

### <a name="other-attributes"></a>Otros atributos

A continuación se muestran otros atributos que se pueden usar para identificar un paquete de Advisor:

* Nombre para mostrar: **displayName**

* Descripción: **Descripción**

* Autor: **autor**

* Versión de Framework: **frameworkVersion** (el valor predeterminado es 3,0)

* Versión mínima del sistema operativo: **minOSversion** (reservada para extensibilidad futura)

* Notificación de eventos perdidos: **showEventLostWarning**

### <a href="" id="bkmk-definedatacollector"></a>Definir el conjunto de recopiladores de datos

Un conjunto de recopiladores de datos define los datos de rendimiento que debe recopilar el marco de trabajo de SPA del servidor de destino. Admite la configuración del registro, WMI, contadores de rendimiento, archivos del servidor de destino y ETW.

``` syntax
<advisorPack>
<dataSourceDefinition xmlns="https://microsoft.com/schemas/ServerPerformanceAdvisor/dc/2010">
 <dataCollectorSet duration="10">
<registryKeys>
 ?<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\\</registryKey>
</registryKeys>
<managementpaths>
 ?<path>Root\Cimv2:select * FROM Win32_DiskDrive</path>
</managementpaths>
<performanceCounters interval="2">
 ?<performanceCounter>\PhysicalDisk(*)\Avg. Disk sec/Transfer</performanceCounter>
</performanceCounters>
<files>
 ?<path>%windir%\System32\inetsrv\config\applicationHost.config</path>
</files>
<providers>
 ?<provider session="NT Kernel Logger" guid="{9E814AAD-3204-11D2-9A82-006008A86939}" keywordsany="06010201" keywordsAll="00000000" level="00000000" />
</providers>
 </dataCollectorSet>
</dataSourceDefinition>
</advisorPack>
```

El atributo **Duration** de **&lt;dataCollectorSet/&gt;** en el ejemplo anterior define la duración de la recopilación de datos (la unidad de tiempo es segundos). **Duration** es un atributo requerido. Esta configuración controla la duración de la colección utilizada por los contadores de rendimiento y ETW.

### <a name="collect-registry-data"></a>Recopilar datos del registro

Puede recopilar datos del registro de los siguientes subárboles del registro:

* HKEY\_clases\_raíz

* HKEY\_configuración de\_actual

* HKEY\_usuario\_actual

* HKEY\_equipo de\_LOCAL

* HKEY\_usuarios

Para recopilar una configuración del registro, especifique la ruta de acceso completa al nombre del valor: HKEY\_equipo\_LOCAL\\MyKey\\valor de Value

Para recopilar todas las opciones de configuración de una clave del registro, especifique la ruta de acceso completa a la clave del registro: HKEY\_equipo\_LOCAL\\MyKey\\

Para recopilar todos los valores de una clave del registro y sus subclaves (PLA recopila de forma recursiva los datos del registro), use dos barras diagonales inversas para el último delimitador de ruta de acceso: HKEY\_máquina de\_LOCAL\\MyKey\\\\

Para recopilar información del registro desde un equipo remoto, incluya el nombre del equipo al principio de la ruta de acceso del registro: HKEY\_máquina\_LOCAL\\MyKey\\valor de la variable

Por ejemplo, puede tener una clave del registro que aparece de la manera siguiente:

``` syntax
Windows registry editor version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes]
"activePowerScheme"="db310065-829b-4671-9647-2261c00e86ef"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\db310065-829b-4671-9647-2261c00e86ef]
"Description"=
 FriendlyName = Power Source Optimized 

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\db310065-829b-4671-9647-2261c00e86ef \0012ee47-9041-4b5d-9b77-535fba8b1442\6738e2c4-e8a5-4a42-b16a-e040e769756e
"ACSettingIndex"=dword:000000b4
"DCSettingIndex"=dword:0000001e
```

Ejemplo 1: devolver solo el PowerSchemes activo y sus valores:

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes</registryKey>
```

Ejemplo 2: devuelve todos los pares clave-valor en esta ruta de acceso:

> [!NOTE]
> PLA se ejecuta en las credenciales de usuario. Algunas claves del registro requieren credenciales administrativas. La enumeración se detiene cuando no puede tener acceso a ninguna de las subclaves.

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\\</registryKey>
```

Todos los datos recopilados se importarán en una tabla temporal llamada **\#registryKeys** antes de que se ejecute un script de informe de SQL. En la tabla siguiente se muestran los resultados del ejemplo 2:

KeyName | KeytypeId | Valor
------ | ----- | -------
HKEY_LOCAL_MACHINE. ..\PowerSchemes | 1 | db310065-829b-4671-9647-2261c00e86ef
\db310065-829b-4671-9647-2261c00e86ef\Description | 2 | |
\db310065-829b-4671-9647-2261c00e86ef\FriendlyName | 2 | Fuente de alimentación optimizada
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\ACSettingIndex | 4 | 180
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\DCSettingIndex | 4 | 30

El esquema de la tabla **#registryKeys** es el siguiente:

Nombre de columna | Tipo de datos de SQL | Descripción
-------- | -------- | --------
KeyName | Nvarchar (300) NOT NULL | Nombre de la ruta de acceso completa de la clave del registro
KeytypeId | Smallint NOT NULL | Identificador de tipo interno
Valor | Nvarchar (4000) NOT NULL | Todos los valores

La columna **KeytypeID** puede tener uno de los siguientes tipos:

ID | Escribe
--- | ---
1 | Cadena
2 | expandString
3 | Binario
4 | DWord
5 | DWordBigEndian
6 | Vincular
7 | MultipleString
8 | Resourcelist
9 | FullResourceDescriptor
10 | ResourceRequirementslist
11 | Qword

### <a name="collect-wmi"></a>Recopilar WMI

Puede agregar cualquier consulta de WMI. Para obtener más información acerca de cómo escribir consultas WMI, consulte [WQL (SQL para WMI)](https://msdn.microsoft.com/library/windows/desktop/aa394606.aspx). En el ejemplo siguiente se consulta una ubicación de archivo de página:

``` syntax
<path>Root\Cimv2:select * FROM Win32_PageFileUsage</path>
```

La consulta del ejemplo anterior devuelve un registro:

Subtítulos | Nombre | PeakUsage
----- | ----- | -----
C:\pagefile.sys | C:\pagefile.sys | 215

Dado que WMI devuelve una tabla con columnas diferentes, cuando los datos recopilados se importan en una base de datos, SPA realiza la normalización de los datos y se agrega a las tablas siguientes:

**\#tabla WMIObjects**

SequenceID | Espacio de nombres | ClassName | Dan | WmiqueryID
----- | ----- | ----- | ----- | -----
10 | Root\Cimv2 | Win32_PageFileUsage | Win32_PageFileUsage. Name =<br>C:\\pagefile. sys | 1

**\#tabla WmiObjectsProperties**

ID | query
--- | ---
1 | Root\Cimv2: SELECT * FROM Win32_PageFileUsage

**\#tabla WmiQueries**

ID | query
--- | ---
1 | Root\Cimv2: SELECT * FROM Win32_PageFileUsage

**\#esquema de tabla WmiObjects**

Nombre de columna | Tipo de datos de SQL | Descripción
--- | --- | ---
SequenceId | Int NOT NULL | Correlacionar la fila y sus propiedades
Espacio de nombres | Nvarchar (200) NOT NULL | Espacio de nombres WMI
ClassName | Nvarchar (200) NOT NULL | Nombre de clase WMI
Dan | Nvarchar (500) NOT NULL | Ruta de acceso relativa de WMI
WmiqueryId | Int NOT NULL | Correlacionar la clave de #WmiQueries

**\#esquema de tabla WmiObjectProperties**

Nombre de columna | Tipo de datos de SQL | Descripción
--- | --- | ---
SequenceId | Int NOT NULL | Correlacionar la fila y sus propiedades
Nombre | Nvarchar (1000) NOT NULL | Nombre de propiedad
Valor | Nvarchar (4000) NULL | Valor de la propiedad actual.

**\#esquema de tabla WmiQueries**

Nombre de columna | Tipo de datos de SQL | Descripción
--- | --- | ---
Id | Int NOT NULL | IDENTIFICADOR único de la consulta >
query | Nvarchar (4000) NOT NULL | Cadena de consulta original en los metadatos de aprovisionamiento

### <a name="collect-performance-counters"></a>Recopilar contadores de rendimiento

Este es un ejemplo de cómo recopilar un contador de rendimiento:

``` syntax
<performanceCounters interval="1">
  <performanceCounter>\PhysicalDisk(*)\Avg. Disk sec/Transfer</performanceCounter>
</performanceCounters>
```

El atributo **Interval** es una configuración global necesaria para todos los contadores de rendimiento. Define el intervalo (la unidad de tiempo es segundos) de la recopilación de datos de rendimiento.

En el ejemplo anterior, el contador \\DiscoFísico (\*)\\promedio de segundos de disco/transferencia se consultará cada segundo.

Podría haber dos instancias: **\_total** y **0 C: D:** , y la salida podría ser la siguiente:

timestamp | CategoryName | CounterName | Valor de instancia de _Total | Valor de instancia de 0 C: D:
---- | ---- | ---- | ---- | ----
13:45:52.630 | PhysicalDisk | Promedio de segundos de disco/transferencia | 0.00100008362473995 |0.00100008362473995
13:45:53.629 | PhysicalDisk | Promedio de segundos de disco/transferencia | 0.00280023414927187 | 0.00280023414927187
13:45:54.627 | PhysicalDisk | Promedio de segundos de disco/transferencia | 0.00385999853230048 | 0.00385999853230048
13:45:55.626 | PhysicalDisk | Promedio de segundos de disco/transferencia | 0.000933297607934224 | 0.000933297607934224

Para importar los datos en la base de datos, los datos se normalizarán en una tabla denominada **\#PerformanceCounters**.

CategoryDisplayName | InstanceName | CounterdisplayName | Valor
---- | ---- | ---- | ----
PhysicalDisk | _Total | Promedio de segundos de disco/transferencia | 0.00100008362473995
PhysicalDisk | 0 C: D: | Promedio de segundos de disco/transferencia | 0.00100008362473995
PhysicalDisk | _Total | Promedio de segundos de disco/transferencia | 0.00280023414927187
PhysicalDisk | 0 C: D: | Promedio de segundos de disco/transferencia | 0.00280023414927187
PhysicalDisk | _Total | Promedio de segundos de disco/transferencia | 0.00385999853230048
PhysicalDisk | 0 C: D: | Promedio de segundos de disco/transferencia | 0.00385999853230048
PhysicalDisk | _Total | Promedio de segundos de disco/transferencia | 0.000933297607934224
PhysicalDisk | 0 C: D: | Promedio de segundos de disco/transferencia | 0.000933297607934224

**Nota:** Los nombres localizados, como **CategoryDisplayName** y **CounterdisplayName**, varían en función del idioma de visualización utilizado en el servidor de destino. Evite el uso de estos campos si desea crear un paquete de asesores independiente del lenguaje.

**\#** esquema de tabla PerformanceCounters

Nombre de columna | Tipo de datos de SQL | Descripción
---- | ---- | ---- | ----
timestamp | datetime2 (3) NOT NULL | Fecha y hora de recopilación en UNC
CategoryName | Nvarchar (200) NOT NULL | Nombre de la categoría
CategoryDisplayName | Nvarchar (200) NOT NULL | Nombre de categoría localizado
InstanceName | Nvarchar (200) NULL | Nombre de instancia
CounterName | Nvarchar (200) NOT NULL | Nombre del contador
CounterdisplayName | Nvarchar (200) NOT NULL | Nombre de contador localizado
Valor | Float NOT NULL | Valor recopilado

### <a name="collect-files"></a>Recopilar archivos

Las rutas de acceso pueden ser absolutas o relativas. El nombre de archivo puede incluir el carácter comodín (\*) y el signo de interrogación (?). Por ejemplo, para recopilar todos los archivos de la carpeta temporal, puede especificar c:\\Temp\\\*. El carácter comodín se aplica a los archivos de la carpeta especificada.

Si también desea recopilar archivos de las subcarpetas de la carpeta especificada, use dos barras diagonales inversas para el último delimitador de carpetas, por ejemplo, c:\\Temp\\\\\*.

Este es un ejemplo que consulta el archivo **ApplicationHost. config** :

``` syntax
<path>%windir%\System32\inetsrv\config\applicationHost.config</path>
```

Los resultados se pueden encontrar en una tabla denominada **\#archivos**, por ejemplo:

querypath | FullPath | Parentpath | nombreDeArchivo | Contenido
----- | ----- | ----- | ----- | -----
% WINDIR%\... \applicationHost.config |C:\Windows<br>\... \applicationHost.config | C:\Windows<br>\... \CONFIG | applicationHost. confi | 0x3C3F78

**\#esquema de tabla de archivos**

Nombre de columna | Tipo de datos de SQL | Descripción
---- | ---- | ----
querypath | Nvarchar (300) NOT NULL | Instrucción de consulta original
FullPath | Nvarchar (300) NOT NULL | Ruta de acceso de archivo absoluta y nombre de archivo
Parentpath | Nvarchar (300) NOT NULL | Ruta de acceso de archivo
nombreDeArchivo | Nvarchar (300) NOT NULL | Nombre del archivo
Contenido | Varbinary (MAX) NULL | Contenido de archivo en binario

### <a name="defining-rules"></a>Definición de reglas

Después de recopilar datos suficientes mediante PLA desde un servidor de destino, el paquete de Advisor puede usar estos datos para la validación y mostrar un resumen rápido a los administradores del sistema.

Las reglas proporcionan información general rápida sobre el rendimiento del servidor. Resaltan los problemas y proporcionan recomendaciones. Puede enumerar todas las reglas que desea validar para un paquete de Advisor. Por ejemplo, si desea desarrollar un paquete de asesor de sistema operativo básico, las posibles reglas podrían incluir:

* Si el modo de energía de la CPU está ahorrando energía

* Si el servidor está en un entorno virtualizado

* Si hay presión de e/s de disco

Las reglas contienen los siguientes elementos:

* Umbral dependiente (una parte configurable de una regla)

* Definición de la regla (alertas y recomendaciones)

Este es un ejemplo de una regla simple:

``` syntax
<advisorPack>

  <reportDefinition>
    <thresholds>
      <threshold  />
    </thresholds>
    <rules>
      <rule  />
      </rule>
    </rules>
  </reportDefinition>
</advisorPack>
```

### <a name="threshold"></a>Umbral

El umbral es un factor configurable que permite a los administradores del sistema decidir si una regla debe mostrar un estado correcto o incorrecto. En el ejemplo siguiente se muestra una regla para detectar el espacio disponible en una unidad del sistema y una advertencia cuando el espacio disponible es inferior a 10 GB.

``` syntax
<threshold name="freediskSize" caption="Free Disk Size (GB)" description="Free Disk Size  value="10" />
```

Sin embargo, en este caso, el administrador del sistema tiene una unidad de disco duro más pequeña. Considera que 5 GB de espacio libre todavía es una buena condición y no desea ver una advertencia. Puede actualizar el valor predeterminado de 10 a 5 a través de la consola de SPA sin tener que entender cómo desarrollar un paquete de Advisor.

La introducción de un umbral ayuda a los administradores del sistema a cambiar rápidamente el valor sin tener que modificar el paquete de Advisor.

En el ejemplo, se requieren todos los atributos excepto **Description** . Puede usar cualquier número como **valor**.

Se puede compartir un umbral entre las reglas.

### <a name="alerts-and-recommendations"></a>Alertas y recomendaciones

La definición de la regla no implica cálculos lógicos. Define cómo podría ser la interfaz de usuario y cómo el script del informe SQL Server comunica los resultados a la interfaz de usuario.

Una regla tiene tres partes:

* Alerta (título de la regla)

* Recomendación (consejos)

* Umbral asociado (información opcional sobre las dependencias)

A continuación se muestra un ejemplo de una regla:

``` syntax
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No Recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on system drive.">
Install OS on larger disk.</advice>
<dependencies>
 <threshold ref="freediskSize"/>
</dependencies>
</rule>
```

Puede definir tantos consejos como desee y, por lo general, definiría recomendaciones. El **nivel** de consejos puede ser **correcto** o **ADVERTENCIA**.

Puede vincular tantos umbrales como desee. Incluso puede vincularse a un umbral que sea irrelevante para la regla actual. La vinculación ayuda a la consola de SPA a administrar fácilmente los umbrales.

El nombre de la regla y las recomendaciones son claves, y son únicas en su ámbito. Dos reglas no pueden tener el mismo nombre y dos recomendaciones de una regla no pueden tener el mismo nombre. Estos nombres serán muy importantes cuando se escribe un informe de script de SQL. Puede llamar al \[DBO\].\[SetNotification\] API para establecer el estado de la regla.

### <a name="defining-ui-display-elements"></a>Definir elementos de visualización de la interfaz de usuario

Una vez definidas las reglas, los administradores del sistema pueden ver el resumen del informe. Sin embargo, a menudo los administradores del sistema están interesados en los datos agregados y desean comprobar los orígenes de datos que se usaron en las reglas de rendimiento.

Siguiendo con el ejemplo anterior, el usuario sabe si hay suficiente espacio libre en disco en la unidad del sistema. Los usuarios también pueden estar interesados en el tamaño real del espacio libre. Se usa un único grupo de valores para almacenar y mostrar estos resultados. Se pueden agrupar y mostrar varios valores únicos en una tabla en la consola de SPA. La tabla solo tiene dos columnas, nombre y valor, como se muestra aquí.

Nombre | Valor
---- | ----
Tamaño de disco libre en la unidad del sistema (GB) | 100
Tamaño total del disco instalado (GB) | 500 

Si un usuario desea ver una lista de todas las unidades de disco duro instaladas en el servidor y su tamaño de disco, podríamos llamar a un valor de lista, que tiene tres columnas y varias filas, como se muestra aquí.

Disk | Tamaño de disco libre (GB) | Tamaño total (GB)
---- | ---- | ----
0 | 100 | 500
1 | 20 | 320

En un paquete de Advisor, podría haber muchas tablas (grupos de valores únicos y tablas de valores de lista). Podemos usar una sección para organizar y clasificar estas tablas.

En Resumen, hay tres tipos de elementos de la interfaz de usuario:

* [Secciones](#bkmk-ui-section)

* [Grupos de valor único](#bkmk-ui-svg)

* [tablas de valores de lista](#bkmk-ui-lvt)

Este es un ejemplo que muestra los elementos de la interfaz de usuario:

``` syntax
<advisorPack>
<dataSourceDefinition/>
<reportDefinition>
 <datatypes>
<datatype .../>
 </datatypes>
 <thresholds/>
 <rule/>
 <sections>
<section .../>
 </sections>
 <singleValues>
<singleValue .../>
 </singleValues>
 <listValues>
<listValue .../>
 </listValues>
</reportDefinition>
</advisorPack>
```

### <a href="" id="bkmk-ui-section"></a>Sección

Una sección es exclusivamente para el diseño de la interfaz de usuario. No participa en los cálculos lógicos. Cada informe único contiene un conjunto de secciones de nivel superior que no tienen una sección principal. Las secciones de nivel superior se presentan como pestañas en el informe. Las secciones pueden tener subsecciones, con un máximo de 10 niveles. Todas las subsecciones de las secciones de nivel superior se muestran en áreas expansibles. Una sección puede contener varias subsecciones, grupos de valores únicos y tablas de valores de lista. Los grupos de valores únicos y las tablas de valores de lista se presentan como tablas.

Este es un ejemplo de la sección de nivel superior.

``` syntax
<section name="CPU" caption="CPU"/>
```

Un nombre de sección debe ser único. Se usa como una clave que se puede vincular a otras secciones, grupos de valores únicos y tablas de valores de lista.

El ejemplo siguiente tiene un atributo, un **elemento primario**y apunta a la sección CPU. CPUFacts es un elemento secundario de la sección denominada CPU. **Parent** debe hacer referencia a un nombre de sección anterior; de lo contrario, puede producir un bucle.

``` syntax
<section name="CPUFacts" caption="Facts" parent="CPU"/>
```

El siguiente grupo de valor único tiene un atributo, **sección**y puede apuntar a cualquier sección, en función del diseño de la interfaz de usuario.

``` syntax
<singleValue name="CPUInformation" section="CPUFacts" caption="Physical CPU Information"> </singleValue>
```

### <a name="data-types"></a>Tipo de datos

Un grupo de valores único y una tabla de valores de lista contienen distintos tipos de datos, como String, int y Float. Dado que estos valores se almacenan en la base de datos SQL Server, puede definir un tipo de datos SQL para cada propiedad de datos. Sin embargo, la definición de un tipo de datos SQL es bastante complicada. Tiene que especificar la longitud o la precisión, que puede ser susceptible de cambiar.

Para definir los tipos de datos lógicos, puede usar el primer elemento secundario de **&lt;reportDefinition/&gt;** , que es donde puede definir una asignación del tipo de datos SQL y el tipo lógico.

En el ejemplo siguiente se definen dos tipos de datos. Una es una **cadena** y la otra es **companyCode**.

``` syntax
<datatype name="string" = sqltype="nvarchar(4000)" />
<datatype name="companyCode" sqltype="nvarchar(100)" />
```

Un nombre de tipo de datos puede ser cualquier cadena válida. Esta es una lista de tipos de datos de SQL permitidos:

* bigint

* binario

* bits

* char

* fecha

* datetime

* datetime2

* datetimeoffset

* decimal

* flotante

* entero

* ingresos

* nchar

* numeric

* nvarchar

* reales

* smalldatetime

* smallint

* smallmoney

* time

* tinyint

* uniqueidentifier

* varbinary

* varchar

Para obtener más información acerca de estos tipos de datos de SQL, vea [tipos de datos (Transact-SQL)](https://msdn.microsoft.com/library/ms187752.aspx).

### <a href="" id="bkmk-ui-svg"></a>Grupos de valor único

Un grupo de valores único agrupa varios valores individuales para presentarlos en una tabla, como se muestra aquí.

``` syntax
<singleValue name="Systemoverview" section="SystemoverviewSection" caption="Facts">
<value name="OsName" type="string" caption="Operating system" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

En el ejemplo anterior, hemos definido un solo grupo de valores. Es un nodo secundario de la sección **SystemoverviewSection**. Este grupo tiene valores únicos, que son **OsName**, **OSVersion**y **OsLocation**.

Un único valor debe tener un atributo de nombre único global. En este ejemplo, el atributo global Unique name es **Systemoverview**. El nombre único se usará para generar una vista correspondiente para el informe personalizado. Cada vista contiene el prefijo **VW**, como vwSystemoverview.

Aunque puede definir varios grupos de valor único, dos nombres de valor único no pueden ser iguales, aunque estén en grupos diferentes. El informe de script de SQL usa el nombre del valor único para establecer el valor en consecuencia.

Puede definir un tipo de datos para cada valor único. La entrada permitida para el **tipo** se define en **&lt;tipo de datos/&gt;** . El informe final podría ser similar al siguiente:

**Hechos**

Nombre | Valor
--- | ---
Sistema operativo | &lt;se _establecerá un valor mediante el script de informe_&gt;
Versión del sistema operativo | &lt;se _establecerá un valor mediante el script de informe_&gt;
Ubicación del sistema operativo | &lt;se _establecerá un valor mediante el script de informe_&gt;

El atributo de **título** de **&lt;valor/&gt;** se presenta en la primera columna. El informe de script establece en el futuro los valores de la columna valor a través de \[DBO\].\[SetSingleValue\]. El atributo **Description** de **&lt;valor/&gt;** se muestra en una información sobre herramientas. Normalmente, la información sobre herramientas muestra a los usuarios el origen de los datos. Para obtener más información sobre la información sobre herramientas, consulte la [información sobre herramientas](#bkmk-tooltips).

### <a href="" id="bkmk-ui-lvt"></a>tablas de valores de lista

Definir un valor de lista es igual que definir una tabla.

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical network adapter information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

El nombre del valor de la lista debe ser único globalmente. Este nombre se convertirá en el nombre de una tabla temporal. En el ejemplo anterior, la tabla denominada \#NetworkAdapterInformation se creará en la fase de inicialización del entorno de ejecución, que contiene todas las columnas que se describen. De forma similar a un nombre de valor único, un nombre de valor de lista también se usa como parte del nombre de vista personalizado, por ejemplo, vwNetworkAdapterInformation.

@type de &lt;columna/&gt; se define mediante &lt;DataType/&gt;

La interfaz de usuario ficticia del informe final podría tener el aspecto siguiente:

**Información del adaptador de red físico**

ID | Nombre | Escribe | Velocidad (Mbps) | Dirección MAC
--- | --- | --- | --- | ---
 | <br> | | |
 | | | |


El atributo **Caption** de &lt;columna/&gt; se muestra como un nombre de columna y el atributo **description** de &lt;columna/&gt; se muestra como información sobre herramientas para el encabezado de columna correspondiente. Normalmente, la información sobre herramientas muestra al usuario el origen de los datos. Para obtener más información, vea [información sobre herramientas](#bkmk-tooltips).

En algunos casos, una tabla puede tener una gran cantidad de columnas y solo unas pocas filas, por lo que el intercambio de las columnas y las filas haría que la tabla fuera mucho mejor. Para intercambiar las columnas y filas, puede Agregar el siguiente atributo de estilo:

``` syntax
<listValue style="Transpose"  
```

### <a name="defining-charting-elements"></a>Definir elementos de gráficos

Puede elegir cualquier clave de estadísticas y ver los valores en un gráfico histórico o en un gráfico de tendencias. Hay dos tipos de estadísticas:

* **Estadísticas estáticas** Un valor único, que se conoce en tiempo de diseño. Por ejemplo, el espacio libre en disco en una unidad del sistema sería una estadística estática.

* **Estadísticas dinámicas** Podría ser desconocido en tiempo de diseño. Por ejemplo, el uso promedio de CPU de cada núcleo es una estadística dinámica porque no se sabe cuántos núcleos de CPU pueden tener en el sistema en tiempo de diseño.

La clave Statistics tiene una limitación de que los datos deben ser compatibles con el tipo de datos Double. Puede ser un entero, un valor decimal o una cadena que se pueda convertir en Double.

SPA usa un solo grupo de valores para admitir estadísticas estáticas y una tabla de valores de lista para admitir las estadísticas dinámicas. En las secciones siguientes se describe cómo definir las estadísticas estáticas y las claves de estadísticas dinámicas.

### <a name="static-statistics"></a>Estadísticas estáticas

Como se mencionó anteriormente, una estadística estática es un valor único. Lógicamente, cualquier valor único podría definirse como una estadística estática. Sin embargo, no tiene sentido ver un valor único que no se pueda convertir en un tipo de número. Para definir una estadística estática, basta con agregar el atributo **trendable** a la clave de valor único correspondiente, como se muestra a continuación:

``` syntax
<value name="freediskSize" type="int" trendable="true"  
```

### <a name="dynamic-statistics"></a>Estadísticas dinámicas

No se conocen las claves de estadísticas dinámicas en tiempo de diseño, por lo que se desconoce el número de valores posibles. Sin embargo, dado que los valores de lista se almacenan en varias filas, sería fácil usar una tabla de valores de lista para almacenar estadísticas dinámicas.

por ejemplo, si necesitamos mostrar gráficos para el uso medio de la CPU de distintos núcleos, podríamos definir una tabla con columnas para **CpuId** y **AverageCpuUsage**:

``` syntax
<listValue name="CpuPerformance">
<column name="CpuId" type="string" caption="CPU ID" columntype="Key"/>
<column name="AverageCpuUsage" type="decimal" caption="Average" columntype="Value"/>
</listValue>
```

Otro atributo, **columntype**, puede ser **key**, **Value**o **informativo**. El tipo de datos de la columna de **clave** debe ser Double o doubleable. En una columna de **clave** , no se pueden insertar las mismas claves en una tabla. Las columnas de **valor** o **informativas** no tienen esta limitación.

Los valores de las estadísticas se almacenan en columnas de **valores** .

Las columnas **informativas** son como las columnas ordinarias en las tablas de valores de lista normales. **Informativo** es el tipo de columna predeterminado si no se especifica uno. Dichas columnas no afectarán al número de claves de estadísticas ni participarán en cálculos relacionados con estadísticas.

Siguiendo con el ejemplo anterior, si un servidor tiene dos núcleos de CPU, el resultado de la tabla podría ser similar al siguiente:

CpuId | AverageCpuUsage
:---: | :---:
0 | 10
1 | 30

Al mismo tiempo, el marco de SPA genera dos claves de estadística. Uno es para CPU 0 y el otro es para CPU 1.

Como se indica en el ejemplo siguiente, se admiten varias columnas de **valor** con varias columnas de **clave** .

CounterName | InstanceName | Media | Sum
--- | :---: | :---: | :---:
% de tiempo de procesador | _Total | 10 | 20
% de tiempo de procesador | CPU0 | 20 | 30 

En este ejemplo, tiene dos columnas de **clave** y dos columnas de **valor** . SPA genera dos claves de estadísticas para la columna Average y otras dos claves para la columna sum. Las claves de estadísticas son:

* Nombre (% tiempo de procesador)/nombreDeInstancia (\_total)/promedio

* Nombre (% tiempo de procesador)/InstanceName (CPU0)/Average

* Nombre (% tiempo de procesador)/nombreDeInstancia (\_total)/Sum

* Nombre (% tiempo de procesador)/nombreDeInstancia (CPU0)/suma

Contraname e InstanceName se combinan como una clave. La clave combinada no puede tener ninguna duplicación.

SPA genera muchas claves de estadísticas. Es posible que algunas de ellas no le interesen y que desee ocultarlas desde la interfaz de usuario. SPA permite a los desarrolladores crear un filtro para mostrar solo claves de estadísticas útiles.

en el ejemplo anterior, los administradores del sistema solo pueden estar interesados en las claves en las que InstanceName es \_total o CPU1. El filtro se puede definir de la siguiente manera:

``` syntax
<listValue name="CpuPerformance">
<column name="CounterName" type="string" columntype="Key"/>
<column name="InstanceName" type="string" columntype="Key">
 <trendableKeyValues>
<value>_Total</value>
<value>CPU1</value>
 </trendableKeyValues>
</column>
<column name="Average" type="decimal" columntype="Value"/>
<column name="Sum" type="decimal" columntype="Value"/>
</listValue>
```

**&lt;trendableKeyValues/&gt;** se pueden definir en cualquier columna de clave. Si se ha configurado más de una columna de clave con un filtro de este tipo, se aplicará la lógica.

### <a name="developing-report-scripts"></a>Desarrollo de scripts de informe

Una vez definidos los metadatos de aprovisionamiento, podemos empezar a escribir el script de informe, que es un procedimiento almacenado de T-SQL.

Hay atributos **Name** y **reportScript** en el encabezado provision metadata, como se muestra aquí:

``` syntax
<advisorPack name="Microsoft.ServerPerformanceAdvisor.CoreOS.V1" reportScript="ReportScript"  
```

El script del informe principal se denomina mediante la combinación de los atributos **Name** y **reportScript** . En el ejemplo siguiente, se \[rá Microsoft. ServerPerformanceAdvisor. coreos. V2\].\[ReportScript\].

``` syntax
create PROCEDURE [Microsoft.ServerPerformanceAdvisor.CoreOS.V2].[ReportScript] AS SET NOCOUNT ON

- Set alert and notification

- Prepare data for report view
```

El atributo **Name** se usará como nombre de esquema de la base de datos, como un espacio de nombres. Esta regla se aplica a todos los demás objetos de base de datos que pertenecen al paquete de SiteAdvisor actual, como el valor de lista y los procedimientos almacenados.

Entre las ventajas de tener este nombre de esquema delante de los objetos de base de datos se incluyen:

* Evitar conflictos de nomenclatura para diferentes paquetes de asesores

* Proporcionar mayor seguridad

En la base de datos de SQL Server, el nombre de esquema predeterminado es **DBO**. Las credenciales de propietario de la base de datos suelen ser necesarias para operar objetos de base de datos en **DBO**. Si no se crea un esquema para cada paquete de Advisor, es probable que dos paquetes de asesores definirán un valor de lista con el mismo nombre. Esto debe ser irrelevante, ya que puede introducir un nombre de esquema para resolver este problema. Además, el desaprovisionamiento de un paquete de Advisor es mucho más sencillo. Dado que el objeto del paquete de Advisor pertenece a un esquema que no es **DBO**, esto permite a Spa usar un privilegio de usuario inferior para acceder a ellos.

Un script de informe normal hace lo siguiente:

* Obtiene acceso a los datos recopilados sin procesar

* Realiza cálculos basados en los datos sin procesar

* Cambios de alertas y recomendaciones

* Prepara los datos para la vista de informe

### <a name="access-raw-collected-data"></a>Acceder a datos recopilados sin procesar

Todos los datos recopilados se importan en las siguientes tablas correspondientes. Para obtener más información sobre el esquema de tabla, vea [definir el conjunto de recopiladores de datos](#bkmk-definedatacollector).

* registry

    * \#registryKeys

* WMI

    * \#WMIObjects

    * \#WmiObjectProperties

    * \#WmiQueries

* Contador de rendimiento

    * \#PerformanceCounters

* Archivo

    * Archivos de \#

* ETW

    * Eventos de \#

    * \#EventProperties

### <a name="set-rule-status"></a>Establecer el estado de la regla

\]\[DBO.\[API de SetNotification\] establece el estado de la regla, por lo que puede ver un icono de **éxito** o de **ADVERTENCIA** en la interfaz de usuario.

* @ruleName nvarchar (50)

* @adviceName nvarchar (50)

Los mensajes de alerta y recomendación se almacenan en el archivo XML de metadatos de aprovisionamiento. Esto hace que el script del informe sea más fácil de administrar.

Inicialmente, cada estado de la regla es N/A. Puede usar esta API para establecer el estado de una regla mediante la especificación de un nombre de Consejo. El nivel del nombre del Consejo se usará como estado de la regla.

Recuerde que hemos definido la siguiente regla anteriormente:

``` syntax
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on the system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on system drive.">Install the operating system on a larger disk.</advice>
</rule>
```

Suponiendo que el espacio disponible es inferior a 2 GB, es necesario establecer la regla en el nivel de **ADVERTENCIA** . La secuencia de comandos SQL será la siguiente:

``` syntax
if (@freediskSizeInGB < 2)
BEGIN
    exec dbo.SetNotification N'freediskSize', N'WarningAdvice'
END
ELSE
BEGIN
    exec dbo.SetNotification N'freediskSize', N'SuccessAdvice'
END 
```

### <a name="get-threshold-value"></a>Obtiene el valor de umbral

\]\[DBO.\[API de\] GetThreshold obtiene los umbrales:

* @key nvarchar (50)

* salida de @value Float

> [!NOTE]
> Los umbrales son pares nombre-valor, y se puede hacer referencia a ellos en cualquier regla. Los administradores del sistema pueden usar la consola de SPA para ajustar los umbrales.

 Siguiendo con el ejemplo anterior, para un umbral, la definición será la siguiente:

``` syntax
<thresholds>
  <threshold name="freediskSize" caption="Free Disk Size (GB)" description="Free Disk Size  value="10" />
</thresholds>
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on the system drive.">
Install the operating system on a larger disk.</advice>
<dependencies>
 <threshold ref="freediskSize"/>
</dependencies>
</rule>
```

El script del informe puede modificarse como se muestra aquí:

``` syntax
DECLARE @freediskSize FLOat
exec dbo.GetThreshold N freediskSize , @freediskSize output

if (@freediskSizeInGB < @freediskSize)

```

### <a name="set-or-remove-the-single-value"></a>Establecer o quitar el valor único

\]\[DBO.\[SetSingleValue\] API establece el valor único:

* @key nvarchar (50)

* @value SQL\_Variant

Este valor se puede ejecutar varias veces para la misma clave de valor único. Se guarda el último valor.

En el ejemplo siguiente se muestran algunos valores únicos definidos:

``` syntax
<singleValue section="Systemoverview" caption="Facts">
<value name="OsName" type="string" caption="Operating System" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS Location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

Después, puede establecer el valor único como se muestra aquí:

``` syntax
exec dbo.SetSingleValue N OsName ,  Windows 7 
exec dbo.SetSingleValue N Osversion ,  6.1.7601 
exec dbo.SetSingleValue N OsLocation ,  c:\ 
```

En raras ocasiones, puede que desee quitar el resultado que estableció anteriormente mediante el \[DBO\].\[removeSingleValue\] API.

* @key nvarchar (50)

Puede usar el siguiente script para quitar el valor establecido anteriormente.

``` syntax
exec dbo.removeSingleValue N Osversion 
```

### <a name="get-data-collection-information"></a>Obtener información de recopilación de datos

\]\[DBO.\[GetDuration\] API obtiene la duración designada por el usuario en segundos para la recopilación de datos:

* salida de @duration int

Aquí se muestra un script de informe de ejemplo:

``` syntax
DECLARE @duration int
exec dbo.GetDuration @duration output
```

\]\[DBO.\[GetInternal\] API obtiene el intervalo de un contador de rendimiento. Podría devolver NULL si el informe actual no tiene información de contador de rendimiento.

* salida de @interval int

Aquí se muestra un script de informe de ejemplo:

``` syntax
DECLARE @interval int
exec dbo.GetInterval @interval output
```

### <a name="set-a-list-value-table"></a>Establecer una tabla de valores de lista

No hay ninguna API para actualizar las tablas de valores de lista. Sin embargo, puede tener acceso directamente a las tablas de valores de lista. en la fase de inicialización, se creará una tabla temporal correspondiente para cada valor de la lista.

En el ejemplo siguiente se muestra una tabla de valores de lista:

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical Network Adapter Information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

Después, puede escribir un script SQL para insertar, actualizar o eliminar los resultados:

``` syntax
INSERT INTO #NetworkAdapterInformation (
  NetworkAdapterId,
  NetworkAdapterName,
  type,
  Speed,
  MACaddress
)
VALUES (

)
```

## <a name="development-and-debugging"></a>Desarrollo y depuración


### <a name="writing-logs"></a>Escribir registros

Si hay más información que desee comunicar con los administradores del sistema, puede escribir registros. Si hay algún registro para un informe determinado, se mostrará un banner amarillo en el encabezado del informe. En el ejemplo siguiente se muestra cómo se puede escribir un registro:

``` syntax
exec dbo.WriteSystemLog N'Any information you want to show to the system administrators , N Warning 
```

El primer parámetro es el mensaje que desea mostrar en el registro. El segundo parámetro es el nivel de registro. La entrada válida para el segundo parámetro podría ser **informativa**, **ADVERTENCIA**o **error**.

### <a name="debug"></a>Depuración

La consola de SPA puede ejecutarse en dos modos, depurar o liberar. El modo de lanzamiento es el valor predeterminado y limpia todos los datos sin procesar recopilados después de que se genere el informe. El modo de depuración mantiene todos los datos sin procesar en el recurso compartido de archivos y la base de datos, de modo que puede depurar el script de informe en el futuro.

**Para depurar un script de informe**

1.  Instale Microsoft SQL Server Management Studio (SSMS).

2.  Una vez iniciado SSMS, conéctese a localhost\\SQLExpress. Tenga en cuenta que debe usar localhost, en lugar de. . De lo contrario, es posible que no pueda iniciar el depurador en SQL Server.

3.  Ejecute el siguiente script para habilitar el modo de depuración:

    ``` syntax
    USE SPADB
    UPdate dbo.Configurations
    SET Value = N'true'
    WHERE Name = N'Debugmode'
    ```

4.  Inicie la consola de SPA y ejecute el paquete de Advisor que desea depurar.

5.  Espere a que se complete la tarea. Si el informe se genera correctamente, cambie de nuevo a SSMS y busque la tarea más reciente.

    ``` syntax
    select TOP 1 * FROM dbo.Tasks OrdER BY Id DESC
    ```

    Por ejemplo, la salida podría ser:

    Id | SessionId | AdvisoryPackageId | ReportStatusId | LastUpdatetime | ThresholdversionId
    :---: | :---: | :---: | :---: | :---: | :---:
    12 | 17 | 1 | 2 | 2011-05-11 05:35:24.387 | 1

6.  Puede ejecutar el siguiente script tantas veces como desee para ejecutar el script de informe para el ID. 12:

    ``` syntax
    exec dbo.DebugReportScript 12
    ```

    **Nota:** También puede presionar F11 para ir a la instrucción anterior y depurar.



Ejecutar \[DBO\].\[DebugReportScript\] devuelve varios conjuntos de resultados, entre los que se incluyen:

1.  Mensajes de Microsoft SQL Server y registros del paquete de Advisor

2.  Resultados de las reglas

3.  Claves y valores de estadísticas

4.  Valores únicos

5.  Todas las tablas de valores de lista

## <a name="best-practices"></a>Procedimiento recomendado

### <a name="naming-convention-and-styles"></a>Estilos y Convención de nomenclatura

|                                                                 Mayúsculas y minúsculas Pascal                                                                 |                       Grafía Camel                        |             Mayúsculas             |
|-----------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|-----------------------------------|
| <ul><li>Nombres en ProvisionMetadata. XML</li><li>Procedimientos almacenados</li><li>Funciones</li><li>Nombres de vista</li><li>Nombres de tablas temporales</li></ul> | <ul><li>Nombres de parámetro:</li><li>Variables locales</li></ul> | Usar para todas las palabras clave reservadas de SQL |

### <a name="other-recommendations"></a>Otras recomendaciones

* Trasladar la mayoría de las piezas lógicas a otros procedimientos almacenados y funciones definidas por el usuario.

* Haga que el script principal sea lo más breve posible con fines de mantenimiento.

* Use el nombre completo del objeto SQL.

* Trate el código SQL para distinguir mayúsculas de minúsculas.

* Agregue **set NOCOUNT** al principio de cada procedimiento almacenado.

* Considere la posibilidad de usar tablas temporales para transferir una gran cantidad de datos.

* Considere la posibilidad **de utilizar Set XACT\_Abort on** para finalizar el proceso si se produce un error.

* Incluya siempre el número de versión principal en el nombre para mostrar del paquete de Advisor.

## <a href="" id="bkmk-advancedtopics"></a>Temas avanzados

### <a name="run-multiple-advisor-packs-simultaneously"></a>Ejecutar varios paquetes de asesores simultáneamente

SPA admite la ejecución de varios paquetes de asesores al mismo tiempo. Esto es especialmente útil cuando desea ver el rendimiento del sistema operativo de Internet Information Services (IIS) y el sistema operativo principal al mismo tiempo. Muchos recopiladores de datos que usa el paquete de asesor de IIS también pueden ser usados por el paquete principal de Core Advisor. Cuando dos o más paquetes de Advisor se ejecutan en el mismo equipo de destino, SPA no recopila los mismos datos dos veces.

En el ejemplo siguiente se muestra el flujo de trabajo para ejecutar dos paquetes de Advisor.

![ejecutar varios paquetes de asesores](../media/server-performance-advisor/spa-dev-guide-multi-advisor-packs.png)

El conjunto de recopiladores de datos de fusión es solo para recopilar el contador de rendimiento y los orígenes de datos ETW. Se aplican las siguientes reglas de combinación:

1. SPA toma la mayor duración como la nueva duración.

2. Cuando hay conflictos de combinación, se siguen las siguientes reglas:

   1. Tome el intervalo más pequeño como el nuevo intervalo.

   2. Tome el superconjunto de los contadores de rendimiento. Por ejemplo, con **Process (\*)\\% Processor Time** and **Process (\*)\\\*,\\Process (\*)\\** \\* devuelve más datos, por lo que **Process (\*)\\% Processor Time** and **Process (\*)\\** \\* se quita del conjunto de recopiladores de datos combinados.

### <a name="collect-dynamic-data"></a>Recopilar datos dinámicos

SPA necesita un conjunto de recopiladores de datos definido en tiempo de diseño. No siempre es posible saber qué datos son necesarios para la generación de informes, ya que los datos dinámicos y la ruta de acceso de la consulta no se conocen hasta que los datos dependientes están disponibles.

por ejemplo, si desea mostrar todos los nombres descriptivos de los adaptadores de red, primero debe consultar a WMI para enumerar todos los adaptadores de red. Cada objeto WMI devuelto tiene una ruta de acceso de clave del registro, donde almacena el nombre descriptivo. La ruta de acceso de la clave del registro se desconoce en tiempo de diseño. En este caso, se necesita compatibilidad con datos dinámicos.

Para enumerar todos los adaptadores de red, puede usar la siguiente consulta WMI mediante Windows PowerShell:

``` syntax
Get-WmiObject -Namespace Root\Cimv2 -query "select PNPDeviceID FROM Win32_NetworkAdapter" | forEach-Object { Write-Output $_.PNPDeviceID }
```

Devuelve una lista de objetos de adaptador de red. Cada objeto tiene una propiedad denominada **PNPDeviceID**, que mantiene una ruta de acceso de clave del registro relativa. Aquí se muestra una salida de ejemplo de la consulta anterior:

``` syntax
ROOT\*ISatAP\0001
PCI\VEN_8086&DEV_4238&SUBSYS_11118086&REV_35\4&372A6B86&0&00E4
ROOT\*IPHTTPS\0000

```

Para buscar el valor **FriendlyName** , abra el editor del registro y vaya a la configuración del registro combinando **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\enum\\** con cada línea del ejemplo anterior. , por ejemplo: **HKEY\_LOCAL\_equipo\\sistema\\CurrentControlSet\\Enum\\ raíz\\IPHTTPS \*0000**.

Para convertir los pasos anteriores en SPA aprovisionar metadatos, agregue el script en el ejemplo de código siguiente:

``` syntax
<advisorPack>
<dataSourceDefinition xmlns="https://microsoft.com/schemas/ServerPerformanceAdvisor/dc/2010">
 <dataCollectorSet >
<registryKeys>
 ?<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\$(NetworkAdapter.PNPDeviceID)\FriendlyName</registryKey>
</registryKeys>
<managementpaths>
 ?<path name="NetworkAdapter">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
</managementpaths>
```

En este ejemplo, primero se agrega una consulta WMI en managementpaths y se define el nombre de clave de **adaptador**de la aplicación. A continuación, agregue una clave del registro y haga referencia a la función de **adaptador** mediante la sintaxis **$ (adaptador de sistema. PNPDeviceID)** .

En la tabla siguiente se define si un recopilador de datos de SPA admite datos dinámicos y si otros recopiladores de datos pueden hacer referencia a ellos:

Tipo de datos | Compatibilidad con datos dinámicos | Se puede hacer referencia
--- | :---: | :---:
clave del Registro | Sí | Sí
WMI | Sí | Sí
Archivo | Sí | No
Contador de rendimiento | No | No
ETW | No | No

Para un recopilador de datos WMI, cada objeto WMI tiene muchos atributos adjuntos. Cualquier tipo de objeto WMI siempre tiene tres atributos: \_\_espacio de nombres, \_clase \_y \_\_RELpath.

Para definir un recopilador de datos al que hagan referencia otros recopiladores de datos, asigne el atributo **Name** con una clave única en ProvisionMetadata. Xml. Esta clave la usan los recopiladores de datos dependientes para generar datos dinámicos.

Aquí se muestra un ejemplo de la clave del registro:

``` syntax
<registryKey  name="registry">HKEY_LOCAL_MACHINE </registryKey>
```

Y un ejemplo de WMI:

``` syntax
<path name="wmi">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
```

Para definir un recopilador de datos dependiente, se usa la sintaxis siguiente: $ ( *{Name}* . *{Attribute}* ).

*{Name}* y *{Attribute}* son marcadores de posición.

Cuando SPA recopila datos de un servidor de destino, reemplaza dinámicamente el patrón $ (\*.\*) con los datos recopilados reales de su recopilador de datos de referencia (clave del registro/WMI), por ejemplo:

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\$(registry.key)\ </registryKey>
<registryKey  name="registry">HKEY_LOCAL_MACHINE\$(wmi.Relativeregistrypath)\ </registryKey>
<path name="wmi"> </path>
<file>$(wmi.FileName)</file>
```

**Nota:** SPA admite una profundidad ilimitada de referencia, pero tenga en cuenta la sobrecarga de rendimiento si tiene demasiados niveles. Asegúrese de que no hay ninguna referencia circular o autoreferencia que no se admita.

### <a name="versioning-limitations"></a>limitaciones de las versiones

SPA admite el restablecimiento y las actualizaciones de versión secundaria. Estos procesos usan el mismo algoritmo. El proceso es actualizar todos los objetos de base de datos y la configuración de umbral, pero conservar los datos existentes. Esto puede actualizarse a una versión superior o degradar a una versión anterior. Seleccione el paquete de Advisor y, a continuación, haga clic en **restablecer** en el cuadro de diálogo **configurar paquetes de Advisor** en Spa para restablecer o aplicar las actualizaciones.

Esta característica es principalmente para las actualizaciones secundarias. No se pueden cambiar drásticamente los elementos de visualización de la interfaz de usuario. Si desea realizar cambios importantes, tendrá que crear un paquete de Advisor diferente. Debe incluir la versión principal en el nombre del módulo de Advisor.

Las limitaciones de las modificaciones de la versión secundaria son las siguientes:

* Cambiar el nombre del esquema

* Cambiar el tipo de datos de cualquier grupo de valor único o de las columnas de una tabla de valores de lista

* Adición o eliminación de umbrales

* Adición o eliminación de reglas

* Agregar o quitar consejos

* Agregar o quitar valores únicos

* Agregar o quitar valores de lista

* Agregar o quitar una columna de valores de lista

### <a href="" id="bkmk-tooltips"></a>Información sobre herramientas

Casi todos los atributos de **Descripción** se mostrarán como información sobre herramientas en la consola de spa.

en el caso de una tabla de valores de lista, se puede lograr una información sobre herramientas basada en filas agregando el atributo siguiente:

``` syntax
<listValue descriptionColumn="Description">
<column name="Name"/>
<column name="Description"/>
</listValue>
```

El atributo **descriptionColumn** hace referencia al nombre de la columna. En este ejemplo, la columna Description no se muestra como una columna física. Sin embargo, se muestra como información sobre herramientas al pasar el puntero sobre cada fila de la primera columna.

Se recomienda que la información sobre herramientas muestre el origen de datos al usuario. Estos son los formatos para mostrar los orígenes de datos:

Origen de datos | Formato | Ejemplo
--- | --- | ---
WMI | WMI: &lt;WMIClass&gt;/campo &lt;&gt; | WMI: Win32_OperatingSystem/Caption
Contador de rendimiento | PerfCounter: &lt;CategoryName&gt;/&lt;InstanceName&gt; | PerfCounter: proceso/% de tiempo de procesador
registry | registro: &lt;registerKey&gt; | registro: Hklm\software\microsoft.<br>\\ASP.NET\\Rootver
Archivo de configuración | ConfigFile: &lt;FilePath&gt;\[; XPath: &lt;&gt;XPath \]<br>**Note**<br>XPath es opcional y solo es válido cuando el archivo es un archivo XML. | ConfigFile: WINDIR%\\system32\\inetsrv\config\\applicationHost. config<br>XPath: Configuration&frasl;System. WebServer<br>&frasl;httpProtocol&frasl;@allowKeepAlive
ETW | ETW: &lt;proveedor/&gt;(palabras clave) | ETW: seguimiento del kernel de Windows (proceso, neto)

### <a name="table-collation"></a>Intercalación de tabla

Cuando un módulo de asesor se vuelve más complicado, puede crear sus propias tablas de variables o tablas temporales para almacenar los resultados intermedios en el script de informe.

La intercalación de columnas de cadena puede ser problemática porque la intercalación de la tabla que cree podría ser diferente de la que creó el marco de SPA. Si correlaciona dos columnas de cadena en tablas diferentes, es posible que vea un error de intercalación. Para evitar este problema, siempre debe definir la cadena para una intercalación de columna como **SQL\_Latin1\_General\_CP1\_CI\_como** cuando defina una tabla.

Aquí se explica cómo definir una tabla de variables:

``` syntax
DECLARE @filesIO TABLE (
 Name nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS,
 AverageFileAccessvolume float,
 AverageFileAccessCount float,
 Filepath nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS
)
```

### <a name="collect-etw"></a>Recopilar ETW

Aquí se explica cómo definir ETW en un archivo ProvisionMetadata. xml:

``` syntax
<dataSourceDefinition>
  <providers>
    <provider session="NT Kernel Logger" guid="{9E814AAD-3204-11D2-9A82-006008A86939}"/>
  </providers>
</dataSourceDefinition>
```

Los siguientes atributos de proveedor se pueden usar para recopilar ETW:

Atributo | Escribe | Descripción
--- | --- | ---
guid | GUID | GUID de proveedor
sesión | cadena | Nombre de sesión de ETW (opcional, solo es necesario para eventos de kernel)
keywordsany | Hexadecimal | Cualquier palabra clave (opcional, sin prefijo 0x)
keywordsAll | Hexadecimal | Todas las palabras clave (opcional)
propiedades | Hexadecimal | Propiedades (opcional)
level | Hexadecimal | Nivel (opcional)
bufferSize | Entero | Tamaño de búfer (opcional)
flushtime | Entero | Tiempo de vaciado (opcional)
maxBuffer | Entero | Búfer máximo (opcional)
minBuffer | Entero | Búfer mínimo (opcional)

Hay dos tablas de salida, como se muestra aquí.

**esquema de tabla de eventos \#**

Nombre de columna | Tipo de datos de SQL | Descripción
--- | --- | ---
SequenceID | Int NOT NULL | IDENTIFICADOR de secuencia de correlación
EventtypeId | Int NOT NULL | IDENTIFICADOR de tipo de evento (consulte [dbo]. [ Eventtypes])
ProcessId | BigInt no NULL | Id. de proceso
ThreadId | BigInt no NULL | Id. de subproceso
timestamp | datetime2 no es NULL | timestamp
Kerneltime | BigInt no NULL | Tiempo del kernel
Usertime | BigInt no NULL | Tiempo de usuario

**\#esquema de tabla EventProperties**

Nombre de columna | Tipo de datos de SQL | Descripción
--- | --- | ---
SequenceID | Int NOT NULL | IDENTIFICADOR de secuencia de correlación
Nombre | Nvarchar(100) | Nombre de propiedad
Valor | Nvarchar(4000) | Valor

### <a name="etw-schema"></a>Esquema ETW

Un esquema ETW se puede generar ejecutando tracerpt. exe en el archivo. ETL. Se genera un archivo Schema. Man. Dado que el formato del archivo. ETL depende del equipo, el siguiente script solo funciona en las siguientes situaciones:

1.  Ejecute el script en el equipo donde se recopila el archivo. ETL correspondiente.

2.  O bien, ejecute el script en un equipo con el mismo sistema operativo y los mismos componentes instalados.

``` syntax
tracerpt *.etl -export
```

## <a name="glossary"></a>glosario


En este documento se usan los siguientes términos:

**Paquete de Advisor**

Un paquete de Advisor es una colección de metadatos y scripts SQL que procesan los registros de rendimiento que se recopilan desde el servidor de destino. Después, el paquete de Advisor genera informes a partir de los datos del registro de rendimiento. Los metadatos del paquete de Advisor definen los datos que se van a recopilar del servidor de destino para las mediciones de rendimiento. Los metadatos también definen el conjunto de reglas, los umbrales y el formato del informe. A menudo, un paquete de Advisor se escribe específicamente para un rol de servidor único, por ejemplo, Internet Information Services (IIS).

**Consola de SPA**

La consola de SPA hace referencia a SpaConsole. exe, que es la parte central de Server Performance Advisor. SPA no necesita ejecutarse en el servidor de destino que está probando. La consola de SPA contiene todas las interfaces de usuario para SPA, desde la configuración del proyecto hasta la ejecución de análisis y la visualización de informes. Por diseño, SPA es una aplicación de dos niveles. La consola de SPA contiene la capa de la interfaz de usuario y parte de la capa de lógica empresarial. La consola de SPA programa y procesa las solicitudes de análisis de rendimiento.

**Plataforma SPA**

SPA contiene dos partes principales, el marco de trabajo y los paquetes de asesores. El marco de SPA proporciona todas las interfaces de usuario, el procesamiento de registros de rendimiento, la configuración, el control de errores y las API de base de datos, y los procedimientos de administración.

**Proyecto SPA**

Un proyecto SPA es una base de datos que contiene toda la información sobre los servidores de destino, los paquetes de asesores y los informes de análisis de rendimiento que se generan en los servidores de destino para los paquetes de Advisor. Puede comparar y ver el historial y los gráficos de tendencias en el mismo proyecto de SPA. El usuario puede crear más de un proyecto. Los proyectos de SPA son independientes entre sí y no hay ningún dato compartido entre los proyectos.

**Servidor de destino**

El servidor de destino es el equipo físico o la máquina virtual que ejecuta Windows Server con determinados roles de servidor, como IIS.

**Sesión de análisis de datos**

Una sesión de análisis de datos es un análisis de rendimiento en un servidor de destino específico. Una sesión de análisis de datos puede incluir varios paquetes de Advisor. Los conjuntos de recopiladores de datos de esos paquetes de asesores se combinan en un único conjunto de recopiladores de datos. Todos los registros de rendimiento de una sola sesión de análisis de datos se recopilan durante el mismo período de tiempo. Analizar los informes generados por los paquetes de Advisor que se ejecutan en la misma sesión de análisis de datos puede ayudar a los usuarios a comprender la situación de rendimiento general e identificar las causas principales de los problemas de rendimiento.

**Seguimiento de eventos para Windows**

[Seguimiento de eventos](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) para Windows (ETW) es un sistema de seguimiento escalable y de alto rendimiento que se proporciona en los sistemas operativos Windows. Proporciona capacidades de generación de perfiles y depuración, que se pueden utilizar para solucionar diversos escenarios. SPA usa eventos ETW como origen de datos para generar los informes de rendimiento. Para obtener información general sobre ETW, vea [mejorar la depuración y el ajuste del rendimiento con ETW](https://msdn.microsoft.com/magazine/cc163437.aspx).

**Consulta WMI**

Instrumental de administración de Windows (WMI) es la infraestructura de datos y operaciones de administración de los sistemas operativos Windows. Puede escribir scripts o aplicaciones de WMI para automatizar las tareas administrativas en equipos remotos. WMI también proporciona datos de administración a otras partes del sistema operativo y a los productos. SPA usa información de clase WMI y puntos de datos como orígenes para generar informes de rendimiento.

**Contadores de rendimiento**

Los contadores de rendimiento se utilizan para proporcionar información sobre cómo funcionan el sistema operativo o una aplicación, un servicio o un controlador. Los datos del contador de rendimiento pueden ayudar a determinar los cuellos de botella del sistema y ajustar el rendimiento del sistema y de las aplicaciones. El sistema operativo, la red y los dispositivos proporcionan datos de contador que una aplicación puede usar para proporcionar a los usuarios una vista gráfica de cómo funciona el sistema. SPA usa información de contadores de rendimiento y puntos de datos como orígenes para generar informes de rendimiento.

**Registros y alertas de rendimiento**

Registros y alertas de rendimiento (PLA) es un servicio integrado en el sistema operativo Windows. Está diseñado para recopilar registros y seguimientos de rendimiento, y también genera alertas de rendimiento cuando se cumplen determinados desencadenadores. PLA se puede usar para recopilar contadores de rendimiento, seguimiento de eventos para Windows (ETW), consultas WMI, claves del registro y archivos de configuración. PLA también admite la recopilación remota de datos a través de llamadas a procedimientos remotos (RPC). El usuario define un conjunto de recopiladores de datos, que incluye información sobre los datos que se van a recopilar, la frecuencia de recopilación de datos, la duración de la recopilación de datos, los filtros y una ubicación para guardar los archivos de resultados. SPA usa PLA para recopilar todos los datos de rendimiento de los servidores de destino.

**Informe único**

Un único informe es el informe SPA que se genera en función de una sesión de análisis de datos para un paquete de Advisor en un solo servidor de destino. Puede contener notificaciones y varias secciones de datos.

**Informe en paralelo**

Un informe en paralelo es un informe SPA que compara dos informes únicos para el mismo paquete de Advisor. Los dos informes pueden generarse desde distintos servidores de destino o desde diferentes ejecuciones de análisis de rendimiento en el mismo servidor de destino. El informe en paralelo crea la capacidad de comparar dos informes para ayudar a los usuarios a identificar comportamientos anómalos o valores de configuración en uno de los informes. Un informe en paralelo contiene notificaciones y varias secciones de datos. En cada sección, los datos de ambos informes se enumeran en paralelo.

**Gráfico de tendencias**

Un gráfico de tendencias es el informe de SPA que se usa para investigar patrones repetitivos de problemas de rendimiento. Muchos problemas de rendimiento repetitivos están causados por cambios de carga del servidor programados desde el servidor o desde equipos cliente, que pueden producirse diaria o semanalmente. SPA proporciona un gráfico de tendencias de 24 horas y un gráfico de tendencias de 7 días para identificar estos problemas.

El usuario puede elegir una o varias series de datos a la vez, que es un valor numérico dentro del único informe, como el **uso total de CPU promedio**. más concretamente, un valor numérico es un valor escalar de un único servidor generado por un único punto de actividad en una instancia de tiempo determinada. SPA agrupa esos valores en 24 grupos, uno por cada hora del día (siete para un informe de 7 días, uno para cada día de la semana). SPA calcula el promedio, el mínimo, el máximo y las desviaciones estándar para cada grupo.

**Gráfico histórico**

Un gráfico histórico es el informe de SPA que se usa para mostrar los cambios en determinados valores numéricos dentro de informes únicos para un par de paquete de asesor y servidor determinado a lo largo del tiempo. El usuario puede elegir varias series de datos y mostrarlas juntas en el gráfico histórico para comprender la correlación entre diferentes series de datos.

**Serie de datos**

Una serie de datos son datos numéricos que se recopilan del mismo origen de datos durante un período de tiempo. El mismo origen significa que los datos tienen que provienen del mismo servidor de destino, como la longitud media de la cola de solicitudes para IIS en un servidor.

**Reglas**

Las reglas son combinaciones de lógica, umbrales y descripciones. Representan un posible problema de rendimiento. Cada paquete de Advisor contiene varias reglas. Cada regla se desencadena mediante un proceso de generación de informes. Una regla aplica la lógica y los umbrales a los datos en un único informe. Si se cumplen los criterios, se genera una notificación de advertencia. Si no es así, la notificación se establece en el estado **correcto** . Si no se aplica la regla, la notificación se establece en el estado no aplicable (**na**).

**Notificaciones**

Una notificación es la información que una regla muestra a los usuarios. Incluye el estado de la regla (**correcto**, **na**o **Warning**), el nombre de la regla y las posibles recomendaciones para solucionar los problemas de rendimiento.
