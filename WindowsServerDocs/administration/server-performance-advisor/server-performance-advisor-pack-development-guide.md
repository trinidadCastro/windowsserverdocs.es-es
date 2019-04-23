---
title: Guía de desarrollo de Server Performance Advisor Pack
description: Guía de desarrollo de Server Performance Advisor Pack
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c647e8a335aac924067d92dcb41ab4d17e0cceef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884866"
---
# <a name="server-performance-advisor-pack-development-guide"></a>Guía de desarrollo de Server Performance Advisor Pack

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 8, Windows 10

Esta guía de desarrollo de Microsoft Server Performance Advisor (SPA) proporciona directrices para ayudar a los desarrolladores y administradores del sistema desarrollar módulos de advisor para analizar el rendimiento del servidor.

Se supone que está familiarizado con los registros de rendimiento y alertas (PLA), los contadores de rendimiento, configuración del registro, Windows Management Instrumentation (WMI), Event Tracing for Windows (ETW) y Transact SQL (T-SQL).

Para obtener más información sobre el uso de SPA, consulte [manual del usuario de Server Performance Advisor](server-performance-advisor-users-guide.md).

## <a name="spa-advisor-pack-overview"></a>Información general SPA Advisor Pack


Un módulo del Asesor de actualizaciones normalmente está diseñado para un rol de servidor determinado y define lo siguiente:

* Datos que deben recopilarse a través de PLA, incluidos Windows Management Instrumentation (WMI), los contadores de rendimiento, configuración del registro, archivos y seguimiento de eventos para Windows (ETW)

* Reglas, que muestra las alertas y recomendaciones

* Datos que se mostrarán (datos sin procesar recopilados, los datos agregados o listas de los 10 principales)

* Estadísticas para ver un valor que cambia a lo largo del tiempo

* Valores de las estadísticas que pueden ser establecido tendencias

Un módulo del Asesor de actualizaciones incluye los siguientes elementos:

* **Metadatos XML** (ProvisionMetadata.xml)

    * [Los registros de rendimiento y alertas (PLA)](https://msdn.microsoft.com/library/windows/desktop/aa372635.aspx) conjunto de recopiladores de datos

    * diseño del informe

* **Secuencias de comandos SQL**

    * Un procedimiento almacenado principal

    * Objetos SQL, como procedimientos almacenados y funciones definidas por el usuario

* **Archivo de esquema ETW** (Schema.man) es opcional

### <a name="advisor-pack-workflow"></a>Flujo de trabajo de Advisor pack

![flujo de trabajo de Advisor pack](../media/server-performance-advisor/spa-dev-guide-workflow.png)

En este diagrama de flujo, los círculos verdes representan los módulos de advisor. Todos los demás círculos representan las fases que se ejecutan en el proceso del marco de SPA. SPA usa un paquete de advisor para recopilar datos, importar los datos en la base de datos, inicializar el entorno de ejecución y ejecutar scripts de SQL.

### <a name="collect-data"></a>Recopilar datos

Cuando un paquete de advisor se pone en cola para un servidor determinado mediante el uso de SPA, el módulo de recopilación de datos consulta el conjunto de recopiladores de datos XML desde el módulo del Asesor de actualizaciones y recopila datos del servidor de destino. Los datos sin procesar se almacenan en un recurso compartido de archivo especificado por el usuario. No se detendrá la recopilación de datos hasta que se supere la SPA designado por el usuario de duración de la ejecución.

### <a name="import-data-into-the-database"></a>importar datos en la base de datos

Una vez completada la recopilación de datos, cada tipo de datos se importa en una tabla correspondiente en la base de datos de SQL Server. Por ejemplo, se importan los valores del registro en una tabla denominada \#registryKeys.

importación de ETW archivo requiere un archivo de esquema ETW para descodificar el archivo .etl. El archivo de esquema ETW es un archivo XML. Se puede generar utilizando tracerpt.exe, que se incluye con Windows. El archivo de esquema ETW solo es necesario cuando el módulo del Asesor de actualizaciones debe importar los datos ETW.

### <a name="switch-to-low-user-rights"></a>Cambiar a los derechos de usuario con pocos

El marco SPA ajusta automáticamente privilegios para minimizar el nivel de acceso de seguridad necesarios. Porque los módulos de advisor se pueden desarrollados o modificar cualquier usuario, es posible que un paquete de advisor contener scripts SQL alterados. Para mitigar el riesgo de seguridad, debe ejecutarse con derechos de usuario bajo cualquier secuencia de comandos SQL para un paquete de advisor. Solo puede acceder a objetos de base de datos limitado, como tablas temporales y las API de SPA expuestas como procedimientos almacenados. Las secuencias de comandos SQL en un paquete de advisor pueden llamar a los procedimientos para interactuar con el marco SPA de almacenamiento.

### <a name="initialize-execution-environment"></a>Inicializar el entorno de ejecución

Módulos de Advisor pueden generar diferentes tipos de salida, como las notificaciones, recomendaciones, las tablas de hechos, las estadísticas y gráficos para las estadísticas. Los scripts SQL realizan determinados cálculos con los datos recopilados. Los resultados producidos se almacenan en tablas temporales mediante las API públicas de SPA. en la fase de inicialización, estas tablas temporales y otros recursos del sistema deben aprovisionarse.

### <a name="run-sql-scripts"></a>Ejecutar scripts de SQL

Hay un procedimiento almacenado principal, que es llamado por el desarrollador de módulo del Asesor de actualizaciones. El marco SPA llama a este procedimiento almacenado para iniciar el cálculo. El procedimiento almacenado consume los datos recopilados y comunica el resultado final para el marco SPA.

### <a name="switch-to-administrative-rights"></a>Cambiar a derechos administrativos

Para generar un informe, se necesitan derechos de administrador. Generación de informes controla por completo el SPA. Es menos probable que se ha alterado.

### <a name="generate-a-report"></a>Generar un informe

Antes de que finalice el procedimiento almacenado principal para un paquete de advisor, todos los resultados calculados, como las notificaciones y las estadísticas, no se conservan. Durante esta fase, el marco SPA transfiere los resultados finales de las tablas temporales a tablas en un formato determinado. Una vez completada esta fase, puede ver los informes mediante la consola SPA.

## <a name="authoring-an-advisor-pack"></a>Creación de un paquete de advisor


### <a name="quick-guidelines"></a>Directrices rápidas

El siguiente diagrama describe los pasos para desarrollar un módulo de advisor totalmente funcional. Esta sección incluye también ejemplos paso a paso para explicar mejor cada paso.

![proceso de desarrollo de Advisor pack](../media/server-performance-advisor/spa-dev-guide-dev-flowchart.png)

Un paquete de advisor normalmente está estructurado de la siguiente manera:

Paquete de Advisor

ProvisionMetadata.xml

Scripts

main.sql

func.sql

Schema.man

Cada paquete de advisor debe tener un archivo denominado ProvisionMetadata.xml. Define información de paquete básico del Asesor de actualizaciones, qué datos se deben recopilar, notificaciones, reglas, y cómo debe almacenarse y muestra el informe. El marco SPA usa esta información para generar una tabla temporal y, a continuación, transfiera los resultados en la tabla temporal a una tabla que pueden tener acceso los usuarios.

Todos los informan de secuencias de comandos SQL deben guardarse en una subcarpeta denominada **Scripts**. Para fines de mantenimiento, se recomienda que guarde los objetos de base de datos diferente en distintos archivos de SQL Server. Debe haber al menos un procedimiento almacenado como un punto de entrada principal.

> [!NOTE]
> El archivo schema.man no es necesario a menos que el paquete de advisor recopila seguimientos de ETW. Este archivo de esquema se utiliza para describir el esquema de los eventos ETW y para descodificar eventos de ETW.

### <a name="defining-basic-information"></a>Definir la información básica

Esta sección describen algunos de los elementos básicos que componen un paquete de advisor, incluidas ProvisionMetadata.xml y los atributos.

Este es un encabezado de ejemplo para el archivo ProvisionMetadata.xml:

``` syntax
<advisorPack
xmlns="http://microsoft.com/schemas/ServerPerformanceAdvisor/ap/2010"
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

### <a name="advisor-pack-version"></a>Versión del módulo del Asesor de actualizaciones

nombre del atributo: **versión**

Los desarrolladores de Advisor pack pueden definir las versiones principales y secundarias para el módulo del Asesor de actualizaciones:

* Una versión principal suele implica mejoras importantes. Los resultados generados por una versión anterior podrían no ser compatibles con la nueva. Se recomienda encarecidamente, incluida la versión principal en el nombre del módulo del Asesor de actualizaciones.

* SPA permite actualizaciones de versión secundaria cuando hay unos cambios mínimos con ningún problema de incompatibilidad de datos.

Para obtener más información acerca de las versiones, vea [temas avanzados](#bkmk-advancedtopics).

### <a name="script-entry-point"></a>Punto de entrada de secuencia de comandos

nombre del atributo: **reportScript**

El marco SPA busca el nombre del procedimiento almacenado principal desde el punto de entrada de secuencia de comandos y lo ejecuta de manera segura.

### <a name="other-attributes"></a>Otros atributos

Estos son algunos otros atributos que se pueden usar para identificar un paquete de advisor:

* Nombre para mostrar: **displayName**

* Descripción: **descripción**

* Autor: **autor**

* Versión de .NET Framework: **frameworkversion** (valor predeterminado es 3.0)

* Versión mínima del sistema operativo: **minOSversion** (reservado para extensibilidad futura)

* Pierde la notificación de eventos: **showEventLostWarning**

### <a href="" id="bkmk-definedatacollector"></a>Definir el conjunto de recopiladores de datos

Un conjunto de recopiladores de datos define los datos de rendimiento que el marco SPA debe recopilar desde el servidor de destino. Admite los contadores de rendimiento de configuración, WMI, registro, archivos desde el servidor de destino y ETW.

``` syntax
<advisorPack>
<dataSourceDefinition xmlns="http://microsoft.com/schemas/ServerPerformanceAdvisor/dc/2010">
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

El **duración** atributo de **&lt;dataCollectorSet /&gt;** en el ejemplo anterior, defina la duración de recopilación de datos (la unidad de tiempo es segundos). **duración** es un atributo necesario. Esta configuración controla la duración de la colección que se usa por los contadores de rendimiento y ETW.

### <a name="collect-registry-data"></a>Recopilar datos del registro

Puede recopilar datos del registro de los subárboles del registro siguiente:

* HKEY\_CLASES\_RAÍZ

* HKEY\_CURrenT\_CONFIG

* HKEY\_CURrenT\_USER

* HKEY\_LOCAL\_MÁQUINA

* HKEY\_USUARIOS

Para recopilar una configuración del registro, especifique la ruta de acceso completa al nombre del valor: HKEY\_LOCAL\_máquina\\MyKey\\MyValue

Para recopilar todos los valores en una clave del registro, especifique la ruta de acceso completa a la clave del registro: HKEY\_LOCAL\_máquina\\MyKey\\

Para recopilar todos los valores en una clave del registro y sus subclaves (PLA recursivamente recopila los datos del registro), use dos barras diagonales inversas para el último delimitador de ruta de acceso: HKEY\_LOCAL\_máquina\\MyKey\\\\

Para recopilar información del registro de un equipo remoto, incluya el nombre del equipo al principio de la ruta de acceso del registro: HKEY\_LOCAL\_máquina\\MyKey\\MyValue

Por ejemplo, puede tener una clave del registro que aparece como sigue:

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

Ejemplo 1: Devolver solo el PowerSchemes activa y sus valores:

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes</registryKey>
```

Ejemplo 2: Devuelve todos los pares clave / valor en esta ruta de acceso:

> [!NOTE]
> PLA se ejecuta con credenciales de usuario. Algunas claves del registro requieren credenciales administrativas. La enumeración se detiene cuando se produce un error al obtener acceso a cualquiera de las subclaves.

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\\</registryKey>
```

Se importarán todos los datos recopilados en una tabla temporal llamada  **\#registryKeys** antes de un informe SQL se ejecuta el script. La siguiente tabla muestra los resultados, por ejemplo 2:

KeyName | KeytypeId | Valor
------ | ----- | -------
HKEY_LOCAL_MACHINE...\PowerSchemes | 1 | db310065-829b-4671-9647-2261c00e86ef
\db310065-829b-4671-9647-2261c00e86ef\Description | 2 | |
\db310065-829b-4671-9647-2261c00e86ef\FriendlyName | 2 | Fuente de alimentación optimizado
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\ACSettingIndex | 4 | 180
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\DCSettingIndex | 4 | 30

El esquema para el **#registryKeys** tabla es una como se indica a continuación:

Nombre de columna | Tipo de datos SQL | Descripción
-------- | -------- | --------
KeyName | Nvarchar (300) no es NULL | Nombre de ruta completa de la clave del registro
KeytypeId | smallint no NULL | Id. de tipo interno
Valor | Nvarchar (4000) no es NULL | Todos los valores

El **KeytypeID** columna puede tener uno de los tipos siguientes:
Id. | Tipo
--- | ---
1 | Cadena
2 | expandString
3 | Binary
4 | DWord
5 | DWordBigEndian
6 | Vínculo
7 | MultipleString
8 | ResourceList
9 | FullResourceDescriptor
10 | ResourceRequirementslist
11 | QWORD

### <a name="collect-wmi"></a>Recopilar WMI

Puede agregar una consulta de WMI. Para obtener más información sobre cómo escribir consultas WMI, consulte [WQL (SQL para WMI)](https://msdn.microsoft.com/library/windows/desktop/aa394606.aspx). El ejemplo siguiente consulta una ubicación de archivo de página:

``` syntax
<path>Root\Cimv2:select * FROM Win32_PageFileUsage</path>
```

La consulta en el ejemplo anterior devuelve un registro:

Título | Name | PeakUsage
----- | ----- | -----
C:\pagefile.sys | C:\pagefile.sys | 215

Dado que WMI devuelve una tabla con columnas distintas, cuando se importan los datos recopilados en una base de datos, SPA lleva a cabo la normalización de datos y se agrega a las tablas siguientes:

**\#Tabla WMIObjects**

SequenceID | Espacio de nombres | ClassName | RelativePath | WmiqueryID
----- | ----- | ----- | ----- | -----
10 | Root\Cimv2 | Win32_PageFileUsage | Win32_PageFileUsage.Name=<br>C:\\pagefile.sys | 1

**\#Tabla WmiObjectsProperties**

Id. | query
--- | ---
1 | Root\Cimv2:select * FROM Win32_PageFileUsage

**\#Tabla WmiQueries**

Id. | query
--- | ---
1 | Root\Cimv2:select * FROM Win32_PageFileUsage

**\#Esquema de tabla WmiObjects**

Nombre de columna | Tipo de datos SQL | Descripción
--- | --- | ---
SequenceId | int no NULL | Correlacionar la fila y sus propiedades
Espacio de nombres | Nvarchar (200) no es NULL | Espacio de nombres WMI
ClassName | Nvarchar (200) no es NULL | Nombre de clase WMI
RelativePath | Nvarchar (500) no es NULL | Ruta de acceso relativa de WMI
WmiqueryId | int no NULL | Correlacionar la clave de #WmiQueries

**\#Esquema de tabla WmiObjectProperties**

Nombre de columna | Tipo de datos SQL | Descripción
--- | --- | ---
SequenceId | int no NULL | Correlacionar la fila y sus propiedades
Name | Nvarchar (1000) no es NULL | Nombre de propiedad
Valor | Nvarchar(4000) NULL | El valor de la propiedad actual

**\#Esquema de tabla WmiQueries**

Nombre de columna | Tipo de datos SQL | Descripción
--- | --- | ---
Id | int no NULL | > identificador de la consulta único
query | Nvarchar (4000) no es NULL | Cadena de consulta original en los metadatos de aprovisionamiento

### <a name="collect-performance-counters"></a>Recopilar contadores de rendimiento

Este es un ejemplo de cómo se recopila un contador de rendimiento s:

``` syntax
<performanceCounters interval="1">
  <performanceCounter>\PhysicalDisk(*)\Avg. Disk sec/Transfer</performanceCounter>
</performanceCounters>
```

El **intervalo** atributo es obligatorio global para todos los contadores de rendimiento. Define el intervalo (la unidad de tiempo es segundos) de recopilación de datos de rendimiento.

En el ejemplo anterior, contador \\PhysicalDisk (\*)\\promedio Segundos de disco/transferencia se va a consultar cada segundo.

Pueden existir dos instancias: **\_Total** y **0 C: D:**, y el resultado podría ser como sigue:

marca de tiempo | CategoryName | CounterName | Valor de la instancia de _Total | Valor de la instancia de 0 C: D:
---- | ---- | ---- | ---- | ----
13:45:52.630 | PhysicalDisk | Promedio Segundos de disco/transferencia | 0.00100008362473995 |0.00100008362473995
13:45:53.629 | PhysicalDisk | Promedio Segundos de disco/transferencia | 0.00280023414927187 | 0.00280023414927187
13:45:54.627 | PhysicalDisk | Promedio Segundos de disco/transferencia | 0.00385999853230048 | 0.00385999853230048
13:45:55.626 | PhysicalDisk | Promedio Segundos de disco/transferencia | 0.000933297607934224 | 0.000933297607934224

Para importar los datos a la base de datos, los datos se normalizarán en una tabla denominada  **\#PerformanceCounters**.

CategoryDisplayName | InstanceName | CounterdisplayName | Valor
---- | ---- | ---- | ----
PhysicalDisk | _Total | Promedio Segundos de disco/transferencia | 0.00100008362473995
PhysicalDisk | 0 C: D: | Promedio Segundos de disco/transferencia | 0.00100008362473995
PhysicalDisk | _Total | Promedio Segundos de disco/transferencia | 0.00280023414927187
PhysicalDisk | 0 C: D: | Promedio Segundos de disco/transferencia | 0.00280023414927187
PhysicalDisk | _Total | Promedio Segundos de disco/transferencia | 0.00385999853230048
PhysicalDisk | 0 C: D: | Promedio Segundos de disco/transferencia | 0.00385999853230048
PhysicalDisk | _Total | Promedio Segundos de disco/transferencia | 0.000933297607934224
PhysicalDisk | 0 C: D: | Promedio Segundos de disco/transferencia | 0.000933297607934224

**Tenga en cuenta** el localizados, como nombres, **CategoryDisplayName** y **CounterdisplayName**, varían según el idioma utilizado en el servidor de destino. Evite el uso de esos campos si desea crear un paquete de idioma neutro advisor.

**\#PerformanceCounters** esquema de tabla

Nombre de columna | Tipo de datos SQL | Descripción
---- | ---- | ---- | ----
marca de tiempo | datetime2(3) no NULL | La hora y fecha recopilados UNC
CategoryName | Nvarchar (200) no es NULL | Nombre de categoría
CategoryDisplayName | Nvarchar (200) no es NULL | Nombre de categoría localizados
InstanceName | Nvarchar(200) NULL | Nombre de instancia
CounterName | Nvarchar (200) no es NULL | Nombre del contador
CounterdisplayName | Nvarchar (200) no es NULL | Nombre de contador localizado
Valor | Float no NULL | El valor recopilado

### <a name="collect-files"></a>Recopilar archivos

Las rutas de acceso pueden ser absoluta o relativa. El nombre de archivo puede incluir el carácter comodín (\*) y el signo de interrogación (?). Por ejemplo, para recopilar todos los archivos en la carpeta temporal, puede especificar c:\\temp\\\*. El carácter comodín se aplica a los archivos en la carpeta especificada.

Si desea recopilar también los archivos de las subcarpetas de la carpeta especificada, utilice dos barras diagonales inversas para el último delimitador de carpeta, por ejemplo, c:\\temp\\\\\*.

Aquí s muestra un ejemplo que consulta el **applicationHost.config** archivo:

``` syntax
<path>%windir%\System32\inetsrv\config\applicationHost.config</path>
```

Los resultados pueden encontrarse en una tabla denominada  **\#archivos**, por ejemplo:

querypath | FullPath | Parentpath | nombreDeArchivo | Contenido
----- | ----- | ----- | ----- | -----
% windir %\.... \applicationHost.config |C:\Windows<br>\...\applicationHost.config | C:\Windows<br>\...\config | applicationHost.confi | 0x3C3F78

**\#Esquema de la tabla de archivos**

Nombre de columna | Tipo de datos SQL | Descripción
---- | ---- | ----
querypath | Nvarchar (300) no es NULL | Instrucción de consulta original
FullPath | Nvarchar (300) no es NULL | Ruta de acceso absoluta y nombre de archivo
Parentpath | Nvarchar (300) no es NULL | Ruta de acceso del archivo
nombreDeArchivo | Nvarchar (300) no es NULL | Nombre de archivo
Contenido | Varbinary (max) NULL | Contenido del archivo en formato binario

### <a name="defining-rules"></a>Definición de reglas

Después de recopilar datos suficientes mediante PLA desde un servidor de destino, el módulo del Asesor de actualizaciones puede usar estos datos para la validación y mostrar un rápido resumen a los administradores del sistema.

Las reglas proporcionan una visión general rápida sobre el servidor de rendimiento s. Se resaltan los problemas y proporcionan recomendaciones. Puede enumerar todas las reglas que desea validar un paquete de advisor. Por ejemplo, si desea desarrollar un módulo del Asesor de actualizaciones de sistema operativo de núcleo, podrían incluir las reglas posibles:

* Si el modo de energía de la CPU es el ahorro de energía

* Si el servidor está en un entorno virtualizado.

* Si hay presión de E/S de disco

Las reglas de contengan los siguientes elementos:

* Umbral dependiente (una parte de una regla configurable)

* Definición de reglas (alertas y recomendaciones)

Este es un ejemplo de una regla sencilla s:

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

Umbral es un factor configurable que permite a los administradores del sistema decidir cuándo debe mostrar una regla de una buena o un estado incorrecto. El ejemplo siguiente muestra una regla para detectar el espacio disponible en una unidad del sistema y una advertencia cuando el espacio disponible es inferior a 10 GB.

``` syntax
<threshold name="freediskSize" caption="Free Disk Size (GB)" description="Free Disk Size  value="10" />
```

Sin embargo, en este caso, el administrador del sistema tiene una unidad de disco duro más pequeñas. Piensa que 5 GB de espacio libre puede seguir siendo un buen estado y que no desea que aparezca una advertencia. El valor predeterminado de 10 a 5 a través de la consola SPA puede actualizar sin tener que entender cómo desarrollar un módulo de advisor.

Introducción a un umbral ayuda a los administradores del sistema cambiar rápidamente el valor sin tener que modificar el paquete de advisor.

En el ejemplo, todos los atributos excepto **descripción** son necesarios. Puede usar cualquier número de **valor**.

Un umbral se puede compartir entre las reglas.

### <a name="alerts-and-recommendations"></a>Alertas y recomendaciones

La definición de regla no implica ningún cálculo lógica. Define el aspecto de la interfaz de usuario y cómo SQL Server informa de secuencia de comandos comunica los resultados de la interfaz de usuario.

Una regla consta de tres partes:

* Alerta (título de la regla)

* Recomendación (aviso)

* Umbral asociado (información opcional sobre las dependencias)

Este es un ejemplo de una regla:

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

Puede definir tantos consejos como desee y, normalmente, definirá las recomendaciones. El **nivel** consejos que pueden ser **éxito** o **advertencia**.

Puede vincular a los umbrales tantas como desee. Incluso puede vincular a un umbral que sea irrelevante a la regla actual. La vinculación ayuda a la consola SPA administrar fácilmente los umbrales.

El nombre de la regla y las recomendaciones son claves, y son únicos dentro de su ámbito. No hay dos reglas pueden tener el mismo nombre, y no hay dos recomendaciones dentro de una regla pueden tener el mismo nombre. Estos nombres serán muy importante cuando se escribe un informe de secuencia de comandos SQL. Puede llamar a la \[dbo\].\[ SetNotification\] API para establecer el estado de la regla.

### <a name="defining-ui-display-elements"></a>Definir elementos de visualización de la interfaz de usuario

Después de que se definen las reglas, los administradores del sistema pueden ver el resumen del informe. Sin embargo, a menudo los administradores del sistema están interesados en los datos agregados, y desean comprobar los orígenes de datos que se usaron en las reglas de rendimiento.

Continuando con el ejemplo anterior, el usuario sabe si hay suficiente espacio libre en disco en la unidad del sistema. Los usuarios también pueden estar interesados en el tamaño real del espacio libre. Un grupo de valor único se usa para almacenar y mostrar los resultados de este tipo. Varios valores únicos se pueden agrupar y se muestra en una tabla en la consola de SPA. La tabla tiene solo dos columnas, nombre y valor, como se muestra aquí.

Name | Valor
---- | ----
Tamaño de disco libre en unidad del sistema (GB) | 100
Tamaño total del disco (GB) | 500 

Si un usuario desea ver una lista de todas las unidades de disco duro que están instalados en el servidor y su tamaño de disco, nos podríamos llamar a un valor de la lista, que tiene tres columnas y varias filas, como se muestra aquí.

Disk | Tamaño de disco libre (GB) | Tamaño total (GB)
---- | ---- | ----
0 | 100 | 500
1 | 20 | 320

En un módulo de advisor, podría haber muchas tablas (grupos de valor único y tablas de valores de lista). Podemos usar una sección para organizar y clasificar estas tablas.

En resumen, hay tres tipos de elementos de interfaz de usuario:

* [Secciones](#bkmk-ui-section)

* [Grupos de valor único](#bkmk-ui-svg)

* [lista de tablas de valor](#bkmk-ui-lvt)

Aquí s muestra un ejemplo que muestra los elementos de interfaz de usuario:

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

### <a href="" id="bkmk-ui-section"></a>Secciones

Una sección es exclusivamente para el diseño de interfaz de usuario. No participa en los cálculos lógicos. Cada informe solo contiene un conjunto de secciones de nivel superior que no tienen una sección primaria. Las secciones de nivel superior se presentan como pestañas en el informe. Las secciones pueden tener subsecciones, con un máximo de 10 niveles. Todas las subsecciones en las secciones de nivel superior se presentan en áreas expansibles. Una sección puede contener varias subsecciones, grupos de valor único y tablas de valores de lista. Grupos de valor único y tablas de valores de lista se presentan como tablas.

Este es un ejemplo de sección de nivel superior.

``` syntax
<section name="CPU" caption="CPU"/>
```

Un nombre de sección debe ser único. Se utiliza como una clave que se puede vincular a otras secciones, grupos de valor único y tablas de valores de lista.

El siguiente ejemplo tiene un atributo, **primario**, que apunta a la sección de la CPU. CPUFacts es un elemento secundario de la sección denominada CPU. **elemento primario** debe hacer referencia a un nombre de la sección anterior; de lo contrario, se puede producir en un bucle.

``` syntax
<section name="CPUFacts" caption="Facts" parent="CPU"/>
```

El siguiente grupo de valor único tiene un atributo, **sección**, y puede señalar a cualquier sección, según su diseño de interfaz de usuario.

``` syntax
<singleValue name="CPUInformation" section="CPUFacts" caption="Physical CPU Information"> </singleValue>
```

### <a name="data-types"></a>Tipos de datos

Un grupo de valor único y una tabla de valores de lista contienen distintos tipos de datos, por ejemplo, string, int y float. Dado que estos valores se almacenan en la base de datos de SQL Server, puede definir un tipo de datos SQL para cada propiedad de datos. Sin embargo, la definición de un tipo de datos SQL es bastante complicado. Debe especificar la longitud o precisión, que puede ser susceptibles de cambiar.

Para definir tipos de datos lógicos, puede usar el primer elemento secundario de  **&lt;reportDefinition /&gt;**, que es donde puede definir una asignación de tipo de datos SQL y el tipo lógico.

El siguiente ejemplo define dos tipos de datos. Uno es **cadena** y el otro es **companyCode**.

``` syntax
<datatype name="string" = sqltype="nvarchar(4000)" />
<datatype name="companyCode" sqltype="nvarchar(100)" />
```

Un nombre de tipo de datos puede ser cualquier cadena válida. Presentamos una lista de tipos de datos SQL permitidos:

* bigint

* archivo binario

* Bit

* char

* date

* datetime

* datetime2

* datetimeoffset

* decimal

* flotante

* entero

* ingresos

* nchar

* Numérico

* nvarchar

* real

* smalldatetime

* smallint

* smallmoney

* time

* tinyint

* uniqueidentifier

* varbinary

* varchar

Para obtener más información acerca de estos tipos de datos SQL, consulte [tipos de datos (Transact-SQL)](https://msdn.microsoft.com/library/ms187752.aspx).

### <a href="" id="bkmk-ui-svg"></a>Grupos de valor único

Un grupo de valor único agrupa varios valores únicos para presentar en una tabla, como se muestra aquí.

``` syntax
<singleValue name="Systemoverview" section="SystemoverviewSection" caption="Facts">
<value name="OsName" type="string" caption="Operating system" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

En el ejemplo anterior, hemos definido un grupo de valor único. Es un nodo secundario de la sección **SystemoverviewSection**. Este grupo tiene valores únicos, que son **OsName**, **Osversion**, y **OsLocation**.

Un valor único debe tener un atributo de nombre único global. En este ejemplo, el atributo de nombre único global es **Systemoverview**. El nombre único se usará para generar una vista de informe personalizado correspondiente. Cada vista contiene el prefijo **vw**, por ejemplo, vwSystemoverview.

Aunque puede definir varios grupos de valor único, no dos nombres de valor único pueden ser el mismo, aunque estén en diferentes grupos. Se usa el nombre de valor único por el informe de la secuencia de comandos SQL para establecer el valor en consecuencia.

Puede definir un tipo de datos para cada valor único. La entrada permitida para **tipo** se define en  **&lt;datatype /&gt;**. El informe final podría tener este aspecto:

**Facts**

Nombre | Valor
--- | ---
Sistema operativo | &lt;_un valor se establece mediante la secuencia de comandos de informe_&gt;
Versión del sistema operativo | &lt;_un valor se establece mediante la secuencia de comandos de informe_&gt;
Ubicación del sistema operativo | &lt;_un valor se establece mediante la secuencia de comandos de informe_&gt;

El **título** atributo de **&lt;valor /&gt;** se presenta en la primera columna. Valores de la columna de valor se establecen en el futuro por el informe de la secuencia de comandos a través de \[dbo\].\[ SetSingleValue\]. El **descripción** atributo de **&lt;valor /&gt;** se muestra en una información sobre herramientas. Normalmente, la información sobre herramientas muestra a los usuarios del origen de datos. Para obtener más información sobre herramientas, consulte [informaciones sobre herramientas](#bkmk-tooltips).

### <a href="" id="bkmk-ui-lvt"></a>lista de tablas de valor

Definir un valor de la lista es la misma definición de una tabla.

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical network adapter information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

El nombre del valor de lista debe ser único globalmente. Este nombre se convertirá en el nombre de una tabla temporal. En el ejemplo anterior, la tabla denominada \#NetworkAdapterInformation se crearán en la fase de inicialización del entorno de ejecución, que contiene todas las columnas que se describen. Al igual que un nombre de valor único, un nombre de valor de la lista también se usa como parte del nombre de la vista personalizada, por ejemplo, vwNetworkAdapterInformation.

@type de &lt;columna /&gt; se define mediante &lt;datatype /&gt;

La interfaz de usuario ficticio del informe final podría tener el aspecto siguiente:

**Información del adaptador de red físico**

Id. | Name | Tipo | Velocidad (Mbps) | Dirección MAC
--- | --- | --- | --- | ---
 | <br> | | |
 | | | |


El **título** atributo de &lt;columna /&gt; se muestra como un nombre de columna y el **descripción** atributo de &lt;columna /&gt; se muestra como información sobre herramientas el encabezado de columna correspondiente. Normalmente, la información sobre herramientas muestra al usuario el origen de datos. Para obtener más información, consulte [informaciones sobre herramientas](#bkmk-tooltips).

En algunos casos, una tabla puede tener muchas columnas y sólo unas pocas filas, por lo que el intercambio de las columnas y filas haría que la tabla buscar mucho mejores. Para intercambiar las columnas y filas, puede agregar el atributo de estilo siguiente:

``` syntax
<listValue style="Transpose"  
```

### <a name="defining-charting-elements"></a>Definición de elementos de gráficos

Puede elegir cualquier clave de las estadísticas y ver los valores en un gráfico histórico o un gráfico de tendencias. Hay dos tipos de estadísticas:

* **Estadísticas estáticas** un valor único, que se conoce en tiempo de diseño. Por ejemplo, el espacio libre en disco en una unidad del sistema sería una estadística estática.

* **Estadísticas dinámico** podría ser desconocido en tiempo de diseño. Por ejemplo, el uso medio de CPU de cada núcleo es una estadística dinámica porque no sabe cuántos núcleos de CPU podrían en el sistema en tiempo de diseño.

La clave de estadísticas tiene una limitación que los datos deben ser compatibles con el tipo de datos double. Puede ser un entero, decimal o una cadena que se puede convertir en tipo doble.

SPA utiliza un grupo de valor único para admitir estadísticas estáticas y una tabla de valores de lista para admitir las estadísticas dinámicas. Las secciones siguientes describen cómo definir estadística estático y las claves de estadísticas dinámico.

### <a name="static-statistics"></a>Estadísticas estáticas

Como se mencionó anteriormente, una estadística estática es un valor único. Lógicamente, cualquier valor individual se puede definir como una estadística estática. Sin embargo, tiene sentido para ver un valor único que no se puede convertir a un tipo de número. Para definir una estadística estática, simplemente puede agregar el atributo **trendable** para el único valor clave correspondiente como se muestra a continuación:

``` syntax
<value name="freediskSize" type="int" trendable="true"  
```

### <a name="dynamic-statistics"></a>Estadísticas dinámico

Las claves de estadísticas dinámico no se conocen en tiempo de diseño, por lo que se desconoce el número de valores posibles. Sin embargo, dado que los valores de lista se almacenan en varias filas, sería fácil de usar una tabla de valores de lista para almacenar estadísticas dinámico.

Por ejemplo, si es necesario mostrar los gráficos para el uso de CPU promedio de núcleos diferentes, podemos definir una tabla con columnas para **CpuId** y **AverageCpuUsage**:

``` syntax
<listValue name="CpuPerformance">
<column name="CpuId" type="string" caption="CPU ID" columntype="Key"/>
<column name="AverageCpuUsage" type="decimal" caption="Average" columntype="Value"/>
</listValue>
```

Otro atributo **columntype**, puede ser **clave**, **valor**, o **informativo**. Tipo de datos de la **clave** columna debe ser de tipo double o convertible doble. En un **clave** columna, no se puede insertar las mismas claves en una tabla. **Valor** o **informativo** columnas no tienen esta limitación.

Los valores de las estadísticas se almacenan en **valor** columnas.

**Informativo** son columnas como columnas normales en las tablas de valor de lista normal. **Informativo** es el tipo de columna predeterminado si no se especifica. Estas columnas no se afecta al número de claves de las estadísticas o participar en los cálculos de estadísticas.

Continuando con el ejemplo anterior, si un servidor tiene dos núcleos de CPU, el resultado de la tabla podría ser similar al siguiente:

CpuId | AverageCpuUsage
:---: | :---:
0 | 10
1 | 30

Al mismo tiempo, se generan claves de dos de las estadísticas por el marco SPA. Uno es para CPU 0 y el otro es para 1 CPU.

Como en el ejemplo siguiente se indica varios **valor** columnas con varios **clave** admiten columnas.

CounterName | InstanceName | Media | Sum
--- | :---: | :---: | :---:
% de tiempo de procesador | _Total | 10 | 20
% de tiempo de procesador | CPU0 | 20 | 30 

En este ejemplo, tiene dos **clave** columnas y dos **valor** columnas. SPA genera dos claves de las estadísticas para la columna promedio y otra de las dos claves para la columna de suma. Las claves de las estadísticas son:

* CounterName (% de tiempo de procesador) / InstanceName (\_Total) / promedio

* CounterName (% de tiempo de procesador) / InstanceName (CPU0) / promedio

* CounterName (% de tiempo de procesador) / InstanceName (\_Total) y Sum

* CounterName (% de tiempo de procesador) / InstanceName (CPU0) / suma

CounterName e InstanceName se combinan como una clave. La clave combinada no puede tener ninguna duplicación.

SPA genera muchas de las estadísticas claves. Algunos de ellos no podrían ser interesante para usted, y desea ocultarlos de la interfaz de usuario. SPA permite a los desarrolladores crear un filtro para mostrar solo las claves de estadísticas útiles.

el ejemplo anterior, los administradores del sistema solo pueden estar interesados en las claves en el que está InstanceName \_Total o CPU1. El filtro puede definirse como sigue:

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

**&lt;trendableKeyValues /&gt;**  se pueden definir en cualquier columna de clave. Si hay más de una columna de clave tiene un filtro configurado y se aplicará la lógica.

### <a name="developing-report-scripts"></a>Desarrollo de scripts de informe

Después de definen los metadatos de aprovisionamiento, podemos comenzar a escribir la secuencia de comandos de informe, que es un procedimiento almacenado de T-SQL.

Hay **nombre** y **reportScript** atributos en el encabezado de metadatos de aprovisionamiento, como se muestra aquí:

``` syntax
<advisorPack name="Microsoft.ServerPerformanceAdvisor.CoreOS.V1" reportScript="ReportScript"  
```

El script del informe principal se denomina combinando el **nombre** y **reportScript** atributos. En el ejemplo siguiente, será \[Microsoft.ServerPerformanceAdvisor.CoreOS.V2\].\[ ReportScript\].

``` syntax
create PROCEDURE [Microsoft.ServerPerformanceAdvisor.CoreOS.V2].[ReportScript] AS SET NOCOUNT ON

- Set alert and notification

- Prepare data for report view
```

El **nombre** atributo se usará como un nombre de esquema de base de datos, como un espacio de nombres. Esta regla se aplica a todos los demás objetos de base de datos que pertenecen al paquete actual del Asesor de actualizaciones, como el valor de la lista y los procedimientos almacenados.

Las ventajas de tener este nombre de esquema delante de los objetos de base de datos incluyen:

* Evitar conflictos de nomenclatura para los módulos diferentes advisor

* Esto proporciona más seguridad

En la base de datos de SQL Server, es el nombre del esquema predeterminado **dbo**. Credenciales del propietario de la base de datos normalmente son necesarias para usar objetos de base de datos en **dbo**. Si no se crea un esquema para cada módulo del Asesor de actualizaciones, es probable que dos módulos de advisor van a definir un valor de la lista con el mismo nombre. Esto debería ser irrelevante porque puede introducir un nombre de esquema para resolver este problema. Además, es mucho más fácil el Desaprovisionamiento de un paquete de advisor. Dado que el objeto del módulo del Asesor de actualizaciones pertenece a un esquema distinto **dbo**, esto permite la SPA utilizar un menor privilegio de usuario para tener acceso a ellos.

Una secuencia de comandos de informe normal hace lo siguiente:

* Acceso a los datos sin procesar recopilados

* Realiza cálculos basados en los datos sin procesar

* Alertas de los cambios y recomendaciones

* Prepara los datos para la vista de informe

### <a name="access-raw-collected-data"></a>Acceder a los datos sin procesar recopilados

Todos los datos recopilados se importa en las siguientes tablas correspondientes. Para obtener más información sobre el esquema de tabla, vea [definir el conjunto de recopiladores de datos](#bkmk-definedatacollector).

* Registro

    * \#registryKeys

* WMI

    * \#WMIObjects

    * \#WmiObjectProperties

    * \#WmiQueries

* Contador de rendimiento

    * \#PerformanceCounters

* Archivo

    * \#Archivos

* ETW

    * \#Eventos

    * \#EventProperties

### <a name="set-rule-status"></a>Establecer el estado de regla

El \[dbo\].\[ SetNotification\] API establece el estado de la regla, para que pueda ver un **éxito** o **advertencia** icono en la interfaz de usuario.

* @ruleName nvarchar(50)

* @adviceName nvarchar(50)

Los mensajes de alerta y la recomendación se almacenan en el archivo XML de metadatos de aprovisionamiento. Esto hace más fácil de administrar la secuencia de comandos de informe.

Inicialmente, el estado de cada regla es N/D. Puede utilizar esta API para establecer un estado de la regla si se especifica un nombre de consejos. El nivel de los consejos de nombre se usará como el estado de la regla.

Recuerde que hemos definido anteriormente la regla siguiente:

``` syntax
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on the system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on system drive.">Install the operating system on a larger disk.</advice>
</rule>
```

Suponiendo que el espacio disponible es inferior a 2 GB, necesitamos establecer la regla en el **advertencia** nivel. El script SQL será como sigue:

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

### <a name="get-threshold-value"></a>Obtener el valor de umbral

El \[dbo\].\[ GetThreshold\] API obtiene los umbrales:

* @key nvarchar(50)

* @value salida de punto flotante

> [!NOTE]
> Los umbrales son pares nombre / valor y puede hacer referencia en todas las reglas. Los administradores del sistema pueden utilizar la consola SPA para ajustar los umbrales.

 Continuando con el ejemplo anterior, para un umbral, la definición será como sigue:

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

La secuencia de comandos de informe se puede modificar tal como se muestra aquí:

``` syntax
DECLARE @freediskSize FLOat
exec dbo.GetThreshold N freediskSize , @freediskSize output

if (@freediskSizeInGB < @freediskSize)
 
```

### <a name="set-or-remove-the-single-value"></a>Establecer o quitar el único valor

El \[dbo\].\[ SetSingleValue\] API establece el valor único:

* @key nvarchar(50)

* @value SQL\_variant

Este valor puede ejecutar varias veces para la misma clave de valor único. El último valor se guarda.

El ejemplo siguiente muestra que algunos definen valores únicos:

``` syntax
<singleValue section="Systemoverview" caption="Facts">
<value name="OsName" type="string" caption="Operating System" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS Location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

A continuación, puede establecer el valor único como se muestra aquí:

``` syntax
exec dbo.SetSingleValue N OsName ,  Windows 7 
exec dbo.SetSingleValue N Osversion ,  6.1.7601 
exec dbo.SetSingleValue N OsLocation ,  c:\ 
```

En raras ocasiones, es posible que desea quitar el resultado que configuró anteriormente mediante el uso de la \[dbo\].\[ removeSingleValue\] API.

* @key nvarchar(50)

Puede usar el siguiente script para quitar previamente establecido valor.

``` syntax
exec dbo.removeSingleValue N Osversion 
```

### <a name="get-data-collection-information"></a>Obtenga información de la recopilación de datos

El \[dbo\].\[ GetDuration\] API Obtiene el usuario designado duración en segundos para la recopilación de datos:

* @duration salida de int

Aquí informar s muestra un ejemplo de script:

``` syntax
DECLARE @duration int
exec dbo.GetDuration @duration output
```

El \[dbo\].\[ GetInternal\] API Obtiene el intervalo de un contador de rendimiento. Podría devolver NULL si el informe actual no tiene información del contador de rendimiento.

* @interval salida de int

Aquí informar s muestra un ejemplo de script:

``` syntax
DECLARE @interval int
exec dbo.GetInterval @interval output
```

### <a name="set-a-list-value-table"></a>Establecer una tabla de valores de lista

No hay ninguna API para actualizar las tablas de valor de lista. Sin embargo, se pueden acceder directamente a las tablas de valor de la lista. en la fase de inicialización, se creará una tabla temporal correspondiente para cada valor de la lista.

El ejemplo siguiente muestra una tabla de valores de lista:

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical Network Adapter Information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

A continuación, puede escribir un script SQL para insertar, actualizar o eliminar los resultados:

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

Si no hay información adicional que desee comunicar a los administradores del sistema, puede escribir registros. Si no hay ningún registro para un informe determinado, se mostrará una notificación en amarillo en el encabezado del informe. El ejemplo siguiente muestra cómo escribir un registro:

``` syntax
exec dbo.WriteSystemLog N'Any information you want to show to the system administrators , N Warning 
```

El primer parámetro es el mensaje que desea mostrar en el registro. El segundo parámetro es el nivel de registro. Podría ser una entrada válida para el segundo parámetro **informativo**, **advertencia**, o **Error**.

### <a name="debug"></a>Depuración

Puede ejecutar la consola SPA en dos modos, depuración o lanzamiento. Modo de versión es el valor predeterminado y limpia los datos sin procesar recopilados una vez generado el informe. El modo de depuración mantiene todos los datos sin procesar en el recurso compartido de archivos y la base de datos, por lo que puede depurar el script de informe en el futuro.

**Para depurar un script de informe**

1.  Instale Microsoft SQL Server Management Studio (SSMS).

2.  Una vez que se inicia SSMS, conectarse a localhost\\SQLExpress. Tenga en cuenta que debe utilizar localhost, en lugar de. . En caso contrario, es posible que no pueda iniciar al depurador de SQL Server.

3.  Ejecute el siguiente script para habilitar el modo de depuración:

    ``` syntax
    USE SPADB
    UPdate dbo.Configurations
    SET Value = N'true'
    WHERE Name = N'Debugmode'
    ```

4.  Inicie la consola SPA y ejecutar el paquete del Asistente para la que desea depurar.

5.  Espere a que se complete la tarea. Si el informe se generó correctamente, vuelva a SSMS y para buscar la última tarea.

    ``` syntax
    select TOP 1 * FROM dbo.Tasks OrdER BY Id DESC
    ```

    Por ejemplo, podría ser el resultado:

    Id | SessionId | AdvisoryPackageId | ReportStatusId | LastUpdatetime | ThresholdversionId
    :---: | :---: | :---: | :---: | :---: | :---:
    12 | 17 | 1 | 2 | 2011-05-11 05:35:24.387 | 1

6.  Puede ejecutar el script siguiente tantas veces como desee ejecutar el script de informe para el Id. de 12:

    ``` syntax
    exec dbo.DebugReportScript 12
    ```

    **Tenga en cuenta** también puede presionar F11 para pasar a la instrucción anterior y la depuración.

     

Ejecutando \[dbo\].\[ DebugReportScript\] devuelve varios conjuntos de resultados, incluidas:

1.  Los mensajes de Microsoft SQL Server y los registros de módulo del Asesor de actualizaciones

2.  Resultados de las reglas

3.  Las estadísticas claves y valores

4.  Valores únicos

5.  Todas las tablas de valor de lista

## <a name="best-practices"></a>Procedimiento recomendado

### <a name="naming-convention-and-styles"></a>Estilos y la convención de nomenclatura

Pascal mayúsculas y minúsculas | Uso de mayúsculas y minúsculas | Mayúsculas
--- | ---- | ---
<ul><li>Nombres de ProvisionMetadata.xml</li><li>Procedimientos almacenados</li><li>Funciones</li><li>Nombres de vista</li><li>Nombres de tabla temporal</li></ul> | <ul><li>Nombres de parámetro</li><li>Variables locales</li></ul> | Uso de todas las palabras clave SQL reservada

### <a name="other-recommendations"></a>Otras recomendaciones

* Mover las piezas más lógicas a otros procedimientos almacenados y funciones definidas por el usuario.

* Hacer que el script principal tan breve como sea posible para fines de mantenimiento.

* Use el nombre completo del objeto SQL.

* Tratar el código SQL que distingue mayúsculas de minúsculas.

* Agregar **SET NOCOUNT ON** al principio de cada procedimiento almacenado.

* Considere el uso de las tablas temporales para transferir la enorme cantidad de datos.

* Considere el uso de **establecer XACT\_anulación ON** para finalizar el proceso si se produce un error.

* Incluir siempre el número de versión principal en el nombre para mostrar del Asistente para paquete.

## <a href="" id="bkmk-advancedtopics"></a>Temas avanzados

### <a name="run-multiple-advisor-packs-simultaneously"></a>Ejecutar simultáneamente varios módulos de advisor

SPA admite la ejecución de varios paquetes del Asesor de actualizaciones al mismo tiempo. Esto es especialmente útil cuando desea buscar en Internet Information Services (IIS) y el rendimiento del sistema operativo de núcleo al mismo tiempo. También se pueden usar muchos recopiladores de datos que se usan por el módulo IIS del Asesor de actualizaciones por el módulo de Asesor de actualizaciones de sistema operativo de núcleo. Cuando se ejecutan dos o más módulos de advisor en el mismo equipo de destino, SPA no recopila los mismos datos dos veces.

El ejemplo siguiente muestra el flujo de trabajo para ejecutar dos módulos de advisor.

![Ejecute varios paquetes de advisor](../media/server-performance-advisor/spa-dev-guide-multi-advisor-packs.png)

El conjunto de recopiladores de datos de fusión es solo para recopilar el contador de rendimiento y los orígenes de datos ETW. Se aplican las siguientes reglas de combinación:

1.  SPA toma la mayor duración como la nueva duración.

2.  Cuando hay conflictos de combinación, se siguen las reglas siguientes:

    1.  Tomar el intervalo mínimo como el nuevo intervalo.

    2.  Tome el superconjunto de los contadores de rendimiento. Por ejemplo, con **proceso (\*)\\% de tiempo de procesador** y **proceso (\*)\\\*,\\proceso (\*)\\ \***  devuelve más datos, por lo que **proceso (\*)\\% de tiempo de procesador** y **proceso (\*)\\ \***  se quita el conjunto de recopiladores de datos combinados.

### <a name="collect-dynamic-data"></a>Recopilar datos dinámicos

Necesidades SPA establece un recopilador de datos definidos en tiempo de diseño. No siempre es posible saber qué datos se necesitan para generar el informe porque no se conocen los datos dinámicos y la ruta de la consulta hasta que sus datos dependientes están disponibles.

Por ejemplo, si desea enumerar todos los nombres descriptivos de los adaptadores de red, primero debe consultar WMI para enumerar todos los adaptadores de red. Cada uno devuelve una ruta de clave del registro, tiene un objeto WMI donde almacena el nombre descriptivo. La ruta de acceso de clave del registro se desconoce en tiempo de diseño. En este caso, necesitamos datos dinámicos admite.

Para enumerar todos los adaptadores de red, puede usar la siguiente consulta WMI mediante Windows PowerShell:

``` syntax
Get-WmiObject -Namespace Root\Cimv2 -query "select PNPDeviceID FROM Win32_NetworkAdapter" | forEach-Object { Write-Output $_.PNPDeviceID }
```

Devuelve una lista de objetos de adaptador de red. Cada objeto tiene una propiedad denominada **PNPDeviceID**, que mantiene una ruta de acceso relativa del registro. S aquí un ejemplo de salida de la consulta anterior:

``` syntax
ROOT\*ISatAP\0001
PCI\VEN_8086&DEV_4238&SUBSYS_11118086&REV_35\4&372A6B86&0&00E4
ROOT\*IPHTTPS\0000
 
```

Para buscar el **FriendlyName** valor, abra el editor del registro y vaya a configuración del registro mediante la combinación **HKEY\_LOCAL\_máquina\\sistema\\ CurrentControlSet\\Enum\\**  con cada línea en el ejemplo anterior. , por ejemplo: **HKEY\_LOCAL\_máquina\\sistema\\CurrentControlSet\\Enum\\ raíz\\\*IPHTTPS\\0000**.

Para traducir los pasos anteriores en los metadatos de aprovisionamiento de SPA, agregue el script en el ejemplo de código siguiente:

``` syntax
<advisorPack>
<dataSourceDefinition xmlns="http://microsoft.com/schemas/ServerPerformanceAdvisor/dc/2010">
 <dataCollectorSet >
<registryKeys>
 ?<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\$(NetworkAdapter.PNPDeviceID)\FriendlyName</registryKey>
</registryKeys>
<managementpaths>
 ?<path name="NetworkAdapter">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
</managementpaths>
```

En este ejemplo, primero agregar una consulta WMI en managementpaths y definir el nombre de clave **adaptador de red**. A continuación, agregue una clave del registro y hacer referencia a **adaptador de red** utilizando la sintaxis, **$(NetworkAdapter.PNPDeviceID)**.

La siguiente tabla define si un recopilador de datos de SPA es compatible con los datos dinámicos y si se pueden hacer referencia a otros los recopiladores de datos:

Tipo de datos | Compatibilidad con los datos dinámicos | Puede hacer referencia a
--- | :---: | :---:
Clave del registro | Sí | Sí
WMI | Sí | Sí
Archivo | Sí | No
Contador de rendimiento | No | No
ETW | No | No

Para un recopilador de datos WMI, cada objeto WMI tiene muchos atributos adjuntos. Cualquier tipo de objeto WMI siempre tiene tres atributos: \_\_Espacio de nombres, \_ \_(clase), y \_ \_RELpath.

Para definir un recopilador de datos que se hace referencia por otros recopiladores de datos, asigne el **nombre** atributo con una clave única en el ProvisionMetadata.xml. Esta clave se usa por los recopiladores de datos dependientes para generar datos dinámicos.

Aquí s muestra un ejemplo para la clave del registro:

``` syntax
<registryKey  name="registry">HKEY_LOCAL_MACHINE </registryKey>
```

Y un ejemplo de WMI:

``` syntax
<path name="wmi">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
```

Para definir un recopilador de datos dependiente, se usa la sintaxis siguiente: $(*{name}*.*atributo {}*).

*{name}*  y *{atributo}* son marcadores de posición.

Cuando SPA recopila datos de un servidor de destino, reemplaza el patrón de $(\*.\*) con los datos reales los datos recopilados desde su recopilador de datos de referencia (clave del registro o WMI), por ejemplo:

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\$(registry.key)\ </registryKey>
<registryKey  name="registry">HKEY_LOCAL_MACHINE\$(wmi.Relativeregistrypath)\ </registryKey>
<path name="wmi"> </path>
<file>$(wmi.FileName)</file>
```

**Tenga en cuenta** SPA es compatible con una profundidad ilimitada de referencia, pero sea consciente de la sobrecarga de rendimiento si tiene demasiados niveles. Asegúrese de que no hay ninguna referencia circular o referencia a sí misma que no se admite.

### <a name="versioning-limitations"></a>limitaciones de las versiones

SPA es compatible con las actualizaciones de versión secundaria y restablecer. Estos procesos usan el mismo algoritmo. El proceso consiste en actualizar todos los objetos de base de datos y configuración del umbral, pero mantener los datos existentes. Esto se puede actualizar a una versión posterior o degradar a una versión anterior. Seleccione el módulo del Asesor de actualizaciones y, a continuación, haga clic en **restablecer** en el **Configurar módulos Advisor** cuadro de diálogo de SPA para restablecer o aplicar o las actualizaciones.

Esta característica es principalmente para las actualizaciones menores. No se puede cambiar drásticamente los elementos de visualización de la interfaz de usuario. Si desea realizar cambios significativos, se debe crear un módulo diferente del Asesor de actualizaciones. Debe incluir la versión principal en el nombre del módulo del Asesor de actualizaciones.

Son las limitaciones de las modificaciones de la versión secundaria que se **CANNOT** realice una de las siguientes acciones:

* Cambiar el nombre de esquema

* Cambiar el tipo de datos de cualquier grupo de valor único o las columnas de una tabla de valores de lista

* Agregar o quitar umbrales

* Agregar o quitar reglas

* Agregar o quitar consejos

* Agregar o quitar valores únicos

* Agregar o quitar valores de lista

* Agregar o quitar una columna de valores de lista

### <a href="" id="bkmk-tooltips"></a>Información sobre herramientas

Casi todos los **descripción** atributos se mostrarán como una información sobre herramientas en la consola de SPA.

para una tabla de valores de lista, se puede lograr una información sobre herramientas basado en filas agregando el atributo siguiente:

``` syntax
<listValue descriptionColumn="Description">
<column name="Name"/>
<column name="Description"/>
</listValue>
```

El **descriptionColumn** atributo hace referencia al nombre de la columna. En este ejemplo, la columna de descripción no se muestra como una columna física. Sin embargo, muestra como una información sobre herramientas cuando pasa el mouse sobre cada fila de la primera columna.

Se recomienda que la información sobre herramientas muestra el origen de datos al usuario. Estos son los formatos para mostrar los orígenes de datos:

Origen de datos | Formato | Ejemplo
--- | --- | ---
WMI | WMI: &lt;wmiclass&gt;/&lt;campo&gt; | WMI: Win32_OperatingSystem/Caption
Contador de rendimiento | PerfCounter: &lt;CategoryName&gt;/&lt;nombreDeInstancia&gt; | PerfCounter: Tiempo de procesador Process/%
Registro | registry: &lt;registerKey&gt; | Registro: HKLM\SOFTWARE\Microsoft<br>\\ASP.NET\\Rootver
Archivo de configuración | ConfigFile: &lt;FilePath&gt;\[; XPath: &lt;XPath&gt;\]<br>**Nota:**<br>XPath es opcional y es válido solo cuando el archivo es un archivo xml. | ConfigFile: windir %\\System32\\inetsrv\config\\applicationHost.config<br>Xpath: configuration&frasl;system.webServer<br>&frasl;httpProtocol&frasl;@allowKeepAlive
ETW | ETW: &lt;Proveedor /&gt;(palabras clave) | ETW: Seguimiento del Kernel de Windows (proceso, red)

### <a name="table-collation"></a>Intercalación de la tabla

Cuando un paquete de advisor se vuelve más complicado, puede crear sus propias tablas variables o tablas temporales para almacenar resultados intermedios en la secuencia de comandos de informe.

Las columnas de cadena de intercalación puede ser problemático debido a que la intercalación de la tabla que cree puede ser diferente a la que se crea mediante el marco SPA. Si correlacionar dos columnas de cadena en tablas diferentes, es posible que vea un error de intercalación. Para evitar este problema, siempre debe definir la cadena de una intercalación de columna como **SQL\_Latin1\_General\_CP1\_CI\_AS** al definir una tabla.

S aquí cómo definir una variable de tabla:

``` syntax
DECLARE @filesIO TABLE (
 Name nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS,
 AverageFileAccessvolume float,
 AverageFileAccessCount float,
 Filepath nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS
)
```

### <a name="collect-etw"></a>Recopilar ETW

S aquí cómo definir ETW en un archivo ProvisionMetadata.xml:

``` syntax
<dataSourceDefinition>
  <providers>
    <provider session="NT Kernel Logger" guid="{9E814AAD-3204-11D2-9A82-006008A86939}"/>
  </providers>
</dataSourceDefinition>
```

Los siguientes atributos de proveedor están disponibles para su uso para la recopilación de ETW:

Atributo | Tipo | Descripción
--- | --- | ---
guid | GUID | GUID del proveedor
sesión | string | Nombre de la sesión ETW (opcional, sólo se requiere para los eventos de kernel)
keywordsany | Hex | Todas las palabras clave (opcional, ningún prefijo 0 x)
keywordsAll | Hex | Todas las palabras clave (opcionales)
properties | Hex | Propiedades (opcionales)
level | Hex | Nivel (opcional)
bufferSize | Entero | Tamaño del búfer (opcional)
flushtime | Entero | Hora de vaciado (opcional)
maxBuffer | Entero | Máximo de búfer (opcional)
minBuffer | Entero | Mínimo de búfer (opcional)

Hay dos tablas de salida como se muestra aquí.

**\#Esquema de la tabla de eventos**

Nombre de columna | Tipo de datos SQL | Descripción
--- | --- | ---
SequenceID | int no NULL | Id. de secuencia de correlación
EventtypeId | int no NULL | Id. de tipo de evento (consulte [dbo]. [ EventTypes])
ProcessId | BigInt no NULL | Id. de proceso
ThreadId | BigInt no NULL | Id. de subproceso
marca de tiempo | datetime2 no NULL | marca de tiempo
Kerneltime | BigInt no NULL | Tiempo de kernel
Tiempos | BigInt no NULL | Tiempo de usuario

**\#Esquema de tabla EventProperties**

Nombre de columna | Tipo de datos SQL | Descripción
--- | --- | ---
SequenceID | int no NULL | Id. de secuencia de correlación
Name | Nvarchar(100) | Nombre de propiedad
Valor | Nvarchar(4000) | Valor

### <a name="etw-schema"></a>ETW schema

Mediante la ejecución tracerpt.exe en el archivo .etl, se puede generar un esquema ETW. Se genera un archivo schema.man. Dado que el formato del archivo .etl es dependiente del equipo, el siguiente script solo funciona en las situaciones siguientes:

1.  Ejecute el script en el equipo donde se recopila el archivo .etl correspondiente.

2.  O bien, ejecútelo en un equipo con el mismo sistema operativo y los componentes instalados.

``` syntax
tracerpt *.etl -export
```

## <a name="glossary"></a>Glosario


Los siguientes términos se usan en este documento:

**Paquete de Advisor**

Un módulo del Asesor de actualizaciones es una colección de metadatos y los scripts SQL que procesan los registros de rendimiento que se recopilan desde el servidor de destino. El módulo del Asesor de actualizaciones, a continuación, genera informes de los datos de registro de rendimiento. Los metadatos en el módulo de advisor definen los datos que se recopilan desde el servidor de destino para las medidas de rendimiento. Los metadatos también definen el conjunto de reglas, los umbrales y el formato de informe. A menudo, un paquete de advisor está escrito específicamente para un rol de servidor único, Internet, por ejemplo, servicios de Information (IIS).

**Consola SPA**

La consola SPA se refiere a SpaConsole.exe, que es la parte central de Server Performance Advisor. SPA no es necesario ejecutar en el servidor de destino que se está probando. La consola SPA contiene todas las interfaces de usuario para SPA, desde la configuración del proyecto para ejecutar el análisis y visualización de informes. Por diseño, SPA es una aplicación de dos niveles. La consola SPA contiene la capa de interfaz de usuario y la parte de la capa de lógica de negocios. La consola SPA programa y procesa las solicitudes de análisis de rendimiento.

**Marco de SPA**

SPA contiene dos partes principales, el marco de trabajo y los módulos de advisor. El marco SPA proporciona todos los la interfaces de usuario, el procesamiento del registro de rendimiento, configuración, control de errores y procedimientos de administración y las API de base de datos.

**Proyecto SPA**

Un proyecto SPA es una base de datos que contiene toda la información sobre los servidores de destino, los módulos de advisor e informes de análisis de rendimiento que se generan en los servidores de destino para los módulos de advisor. Puede comparar y ver los gráficos de historial y la tendencia dentro del mismo proyecto SPA. El usuario puede crear más de un proyecto. Los proyectos SPA son independientes entre sí y no hay ningún dato que se comparten entre varios proyectos.

**Servidor de destino**

El servidor de destino es el equipo físico o máquina virtual que ejecuta Windows Server con determinados roles de servidor, como IIS.

**Sesión de análisis de datos**

Una sesión de análisis de datos es un análisis de rendimiento en un servidor de destino específica. Una sesión de análisis de datos puede incluir varios módulos de advisor. Los conjuntos de recopiladores de datos de esos módulos de advisor se combinan en un conjunto de recopiladores de datos único. Todos los registros de rendimiento para una sesión de análisis de datos solo se recopilan durante el mismo período de tiempo. Analizar los informes generados por los módulos del Asesor de actualizaciones que se ejecutan en la misma sesión de análisis de datos puede ayudar a los usuarios a comprender la situación general del rendimiento e identificar las causas raíz de problemas de rendimiento.

**Seguimiento de eventos para Windows**

[Seguimiento de eventos](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) para Windows (ETW) es un sistema de seguimiento de alto rendimiento, baja sobrecarga y escalable que se proporciona en los sistemas operativos de Windows. Proporciona la generación de perfiles y funciones, que pueden utilizarse para solucionar una variedad de escenarios de depuración. SPA usa eventos ETW como un origen de datos de generación de informes de rendimiento. Para obtener información general sobre ETW, vea [Improve Debugging and Performance Tuning with ETW](https://msdn.microsoft.com/magazine/cc163437.aspx).

**Consulta de WMI**

Windows Management Instrumentation (WMI) es la infraestructura para administrar los datos y las operaciones en los sistemas operativos de Windows. Puede escribir scripts de WMI o las aplicaciones para automatizar tareas administrativas en equipos remotos. WMI también proporciona datos de administración de productos y otras partes del sistema operativo. SPA usa información de clase WMI y los puntos de datos como orígenes para generar informes de rendimiento.

**Contadores de rendimiento**

Contadores de rendimiento se utilizan para proporcionar información sobre el sistema operativo o una aplicación, servicio o controlador de rendimiento. Los datos del contador de rendimiento pueden ayudar a determinar cuellos de botella del sistema y optimizar el rendimiento del sistema y aplicación. El sistema operativo, red y dispositivos proporcionan datos del contador que puede consumir una aplicación para proporcionar a los usuarios una vista gráfica de buen rendimiento del sistema. SPA usa información del contador de rendimiento y los puntos de datos como orígenes para generar informes de rendimiento.

**Las alertas y registros de rendimiento**

Los registros de rendimiento y alertas (PLA) es un servicio integrado en el sistema operativo de Windows. Está diseñado para recopilar los registros de rendimiento y seguimientos, y también genera alertas de rendimiento cuando se cumplen determinados desencadenadores. PLA puede usarse para recopilar los contadores de rendimiento, seguimiento de eventos para Windows (ETW), las consultas WMI, claves del registro y configuración de los archivos. PLA también admite la recopilación de datos remotos a través de llamadas a procedimiento remoto (RPC). El usuario define un conjunto de recopiladores de datos, que incluye información acerca de los datos que se recopilen, frecuencia de recopilación de datos, duración de recopilación de datos, filtros y una ubicación para guardar los archivos de resultados. SPA utiliza PLA para recopilar todos los datos de rendimiento de los servidores de destino.

**Informe único**

Informe único es el informe SPA generada se basa en una sesión de análisis de datos para un módulo del Asesor de actualizaciones en un servidor de destino único. Puede contener las notificaciones y las diversas secciones de datos.

**Informe de Side-by-side**

Un informe en paralelo es un informe SPA que compara dos informes únicos para el mismo módulo de advisor. Los dos informes pueden generarse desde servidores de destino diferente o desde las ejecuciones de análisis de rendimiento independientes en el mismo servidor de destino. El informe side-by-side crea la capacidad de comparar dos informes para ayudar a los usuarios a identificar comportamientos anómalos o configuración en uno de los informes. Un informe en paralelo contiene las notificaciones y las diversas secciones de datos. En cada sección, datos de ambos informes están enumerado side-by-side.

**Gráfico de tendencias**

Un gráfico de tendencias es el informe SPA que se usa para investigar los patrones repetitivos de problemas de rendimiento. Muchos problemas de rendimiento repetitivas están provocados por cambios de carga de servidor programadas desde el servidor o desde los equipos cliente, lo que pueden suceder diaria o semanal. SPA proporciona un gráfico de tendencias de 24 horas y un gráfico de tendencias de 7 días para identificar estos problemas.

El usuario puede elegir una o más series de datos a la vez, que es un valor numérico en el informe, como **promedio de uso de CPU total**. más específicamente, un valor numérico es un valor escalar desde un único servidor que genera un único punto de acceso a una instancia de tiempo determinado. SPA agrupa esos valores en 24 grupos, uno por cada hora del día (siete para un informe de 7 días, uno para cada día de la semana). SPA calcula el promedio, mínimo, máximo y desviaciones estándar para cada grupo.

**Gráfico histórico**

Un gráfico histórico es el informe SPA que se usa para mostrar los cambios en ciertos valores numéricos dentro únicos informes para un servidor determinado y un par de pack de advisor con el tiempo. El usuario puede elegir varias series de datos y mostrarlos juntos en el gráfico para comprender la correlación entre las distintas series de datos histórico.

**Serie de datos**

Una serie de datos es datos numéricos que se recopilan desde el mismo origen de datos durante un período de tiempo. El mismo origen significa que los datos tienen que proceder del mismo servidor de destino, como la longitud de cola promedio de solicitud para IIS en un servidor.

**reglas**

Las reglas son combinaciones de lógica, umbrales y descripciones. Representan un posible problema de rendimiento. Cada paquete de advisor contiene varias reglas. Cada regla se desencadena mediante un proceso de generación de informes. Una regla aplica la lógica y los umbrales para los datos de informe único. Si se cumplen los criterios, se genera una notificación de advertencia. Si no, la notificación está establecida en el **Aceptar** estado. Si no se aplica la regla, la notificación se establece como no aplicable (**NA**) estado.

**Notificaciones**

Una notificación es la información que se muestra una regla a los usuarios. Incluye el estado de la regla (**Aceptar**, **NA**, o **advertencia**), el nombre de la regla y es posible, recomendaciones para solucionar los problemas de rendimiento.
