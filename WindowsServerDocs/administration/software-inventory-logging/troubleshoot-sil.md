---
title: Troubleshoot Software Inventory Logging (Resolver problemas del Registro de inventario de software)
description: Describe cómo resolver problemas comunes de implementación para el registro de inventario de Software.
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
author: brentfor
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 6ea8a336e2c40b55e2ad6b508d89db7dcb668315
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832856"
---
# <a name="troubleshoot-software-inventory-logging"></a>Troubleshoot Software Inventory Logging (Resolver problemas del Registro de inventario de software) 

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

##<a name="understanding-sil"></a>Descripción de SIL

Antes de iniciar la solución de problemas de SIL, debe tener un buen conocimiento de sus componentes y su funcionamiento. Los siguientes vídeos proporcionan una visión general de SIL y el agregador de SIL y cómo usarlas para reenviar y datos de inventario de informes:

1. [Una introducción al inventario de Software (SIL) (10:57) de registro](https://channel9.msdn.com/Blogs/Regular-IT-Guy/An-Introduction-to-Software-Inventory-Logging-SIL)

2. [Software de registro de inventario: Configurar el agregador de SIL (14:34)](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Setting-up-SIL-Aggregator)

3. [Software de registro de inventario: Habilitar el reenvío de SIL (7:20)](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Enabling-SIL-Forwarding)

##<a name="how-sil-data-flow-works"></a>Flujo de datos SIL de cómo funciona

El marco SIL tiene dos componentes principales y dos canales de comunicación. Flujo de datos a través de ambos canales y entre los dos componentes, es necesario para una implementación correcta de SIL (se presupone un entorno virtualizado o en la nube,--entornos físicos puramente solo tiene uno de los canales de comunicación). Deberá comprender los componentes y flujo de datos de SIL para implementarlo correctamente. Después de ver los vídeos de introducción anteriores, habrá visto el diagrama arquitectónico que ilustra los componentes y el flujo de datos a través de ambos canales. Flechas naranjas indican las consultas remotas a través de WinRM, flechas discontinuas verdes indican que HTTPS se publica en el agregador de SIL de SIL en cada nodo de final de WS:

![](../media/software-inventory-logging/image1.png)

Si se produce un problema con SIL, es probable que se relacionados a una interrupción en el flujo de datos a través de los canales y entre los componentes. Estos son los problemas más comunes relacionados con flujo de datos, seguido los pasos de solución de problemas para resolver cada uno de los tres problemas en la sección siguiente:

-   **Problema 1 de flujo de datos** – **ningún dato en el informe cuando se usa el cmdlet Publish-SilReport** (o datos generalmente que faltan).

-   **Problema 2 del flujo de datos** – **demasiadas muchos servidores de Host desconocido** en el informe.

-   **Problema 3 de flujo de datos** – **demasiadas muchas máquinas virtuales en hosts físicos aparece como desconocido del sistema operativo** en el informe o un error producido al utilizar **Publish-SilData** en servidores de Windows que ejecutan SIL.

##<a name="troubleshooting-data-flow-issues"></a>Problemas de flujo de datos

Antes de comenzar, necesitará saber cuánto tiempo hace una introducción del agregador de SIL el **Start-SilAggregator** cmdlet.

>[!IMPORTANT]
>No habrá ningún dato en el informe hasta que se procese el cubo de datos SQL en tiempo de 3: 00 A.M. del sistema local. No continúe con los pasos de solución de problemas hasta que el cubo procesado datos.

Si está solucionando los datos en el informe (o que faltan en el informe) que es más recientes que la última vez que procesa el cubo, o antes de que alguna vez ha procesado el cubo (para una nueva instalación), siga estos pasos para procesar el cubo de datos SQL en tiempo real :

1. Inicie sesión como administrador de SQL Server y ejecute **SSMS** en un símbolo del sistema.
2. Conéctese al moto de base de datos.
3. Expanda **del Agente SQL Server** y, a continuación, expanda **trabajos**.
4. Haga clic en **SILStagingRefresh** y, a continuación, seleccione **iniciar trabajo en el paso**.
5. Haga clic en **iniciar** y espere a que la barra de progreso de actualización en completarse.
6. Abra PowerShell como administrador y ejecute el **silreport publicar - openreport** cmdlet.

Si todavía no hay ningún dato en el informe, continúe con los problemas de flujo de datos de tres.

###<a name="data-flow-issue-1"></a>Problema de flujo de datos 1 

####<a name="no-data-in-the-report-when-using-the-publish-silreport-cmdlet-or-data-is-generally-missing"></a>No hay datos en el informe cuando se usa el cmdlet Publish-SilReport (o datos no está presente con carácter general)

Si faltan datos, es probablemente debido a no tener procesado aún el cubo de datos SQL. Si ha procesado recientemente y cree que deberían han llegado datos que faltan en el agregador antes del procesamiento del cubo, siga la ruta de acceso de los datos en orden inverso. Elija un host único y una máquina virtual única para solucionar problemas. La ruta de acceso de datos en orden inverso sería **SILA informe** &lt; **base de datos SILA** &lt; **directorio local SILA** &lt;  **host físico remoto** o **VM WS ejecutando tarea de agente/SIL**.

####<a name="check-to-see-if-data-is-in-the-database"></a>Compruebe si los datos se están en la base de datos

Hay dos maneras de comprobar si hay datos: **PowerShell** o **SSMS**.

>[!Important]
>Si el cubo procesado al menos una vez desde SILA inserta datos en la base de datos, estos datos deben reflejarse en el informe. Si no hay ningún dato en la base de datos, a continuación, sondeo de los hosts físicos se producen errores o nada se recibe a través de HTTPS o ambos.

 ####<a name="powershell"></a>PowerShell

1. Abra PowerShell como administrador y ejecute el **get-silvmhost** cmdlet y, a continuación, ejecute **get-silaggregator**.

    >[!NOTE]
    >La salida de **get-silaggregator** siempre imita el **ficha Detalles de Windows Server** del informe de Excel. No se espera un resultado diferente.

2. Ejecute **get-silvmhost**
 - Si no aparece ningún host, a continuación, agregue hosts mediante la **agregar-silvmhost** cmdlet.
 - Si no aparece como hosts **desconocido**, a continuación, vaya a 2 del problema. d - si se muestran los hosts pero no hay ningún **datetime** bajo el **sondeo reciente** columna y, a continuación, vaya a **problema 2** a continuación.

**Otros comandos relacionados**

**Get-SilAggregator - Computername &lt;fqdn de un servidor conocido insertando datos&gt;**: Esto generará información de la base de datos sobre un equipo (VM), incluso antes de que haya procesado el cubo. Por lo tanto, este cmdlet puede usarse para comprobar en los datos de la base de datos para un servidor de Windows que se insertan datos de SIL a través de HTTPS, antes o sin el proceso de cubo a las 3 A.M. (o si no ha actualizado el cubo en tiempo real como se describe al principio de esta sección).

**Get-SilAggregator - VmHostName &lt;fqdn de un sondeo host físico donde hay un valor en la columna de una encuesta reciente cuando se usa el cmdlet Get-SilVmHost&gt;**: Esto generará información de la base de datos sobre un host físico, incluso antes de que haya procesado el cubo.

####<a name="ssms"></a>SSMS

n**comprobar si hay datos de los hosts que se sondea:**
 
1. Abra **SSMS** y conéctese a la **motor de base de datos**.
2. Expanda **bases de datos**, expanda el **SoftwareInventoryLogging** de base de datos, expanda **tablas**, haga clic en el **HostInfo** tabla y, a continuación, Seleccionar las primeras 1000 filas. 

    Si no hay datos para uno o más hosts de la tabla, a continuación, sondea los hosts ha tenido éxito al menos una vez.

 **Comprueba si los datos de las máquinas virtuales o servidores independientes, que se han insertado datos a través de HTTPS:** 

1. Abra **SSMS** y conéctese a la **motor de base de datos**.
a2. Expanda **bases de datos**, expanda el **SoftwareInventoryLogging** de base de datos, expanda **tablas**, haga clic en el **VMInfo** tabla y, a continuación, Seleccionar las 1000 filas superiores. 

    >[!NOTE] 
    >Cada fila de una máquina virtual única representará una procesan **bmil** archivo transmitiendo a través de HTTPS y el agregador de SIL procesa correctamente. Bmil archivos son archivos de su propiedad usando SIL, se crea cada nuestro por cada instancia SIL tenga en cuenta que esto solo es necesario cuando se utilizan en entornos virtuales SIL y SILA. En caso contrario, solo el tráfico HTTPS es necesario o previsto).

 Todos los datos de la base de datos deben reflejarse en los informes SIL se haya procesado el cubo.

###<a name="data-flow-issue-2"></a>Problema de flujo de datos 2
####<a name="too-many-servers-under-unknown-host"></a>Hay demasiados servidores de Host desconocido

Esto es probable que se produzca en los entornos virtuales al agregador de SIL no está correctamente sondear hosts físicos que hospedan las máquinas virtuales.

1. Abra **PowerShell** como administrador y ejecute el **get-silvmhost** cmdlet.

    -   Si no aparece como hosts **desconocido**, el **agregar-silvmhost** cmdlet no funcionó correctamente, normalmente debido a credenciales no válidas agregadas para el acceso a estos hosts (por lo tanto, desconocido). Pero si se comprueban las credenciales, puede significar la detección automática de **hosttype** y **hypervisortype** en el **agregar-silvmhost** cmdlet no pudo reconocer el plataforma. Son que avanzadas disponibles para estas situaciones de pasos para solucionar problemas, pero no se tratan aquí (comprobación de los canales del Visor de eventos del agregador de SIL).

    -   Si se muestran los hosts, y **hosttype** y **hypervisortype** se muestran con valores que no son **desconocido**, es decir, Windows y Hyper-v, o Ubuntu y Xen, etc., pero no hay ningún **datetime** en **sondeo reciente** columna, sondeo no correctamente se produjo todavía.

        -   Deberá esperar una hora después de agregar el host para el sondeo para que se produzca (suponiendo que este intervalo se establece en el valor predeterminado: se puede comprobar mediante la **get-silaggregator** cmdlet).

        -   Si ha sido una hora desde que se agregó el host, compruebe que se está ejecutando la tarea de sondeo: En **programador de tareas**, seleccione **Software Inventory Logging Aggregator** en **Microsoft** &gt; **Windows** y comprobar el historial de la tarea.

    -   Si aparece un host, pero no hay ningún valor para **RecentPoll**, **HostType**, o **HypervisorType**, esto puede omitirse en gran medida. Esto solo ocurre en entornos de Hyper-v. Estos datos proceden realmente de la máquina virtual Windows Server, que identifica el host físico que se está ejecutando a través de HTTPS. Esto puede ser útil para identificar una máquina virtual específica que está informando, pero requiere el uso de la base de datos de minería de datos la **Get SilAggregatorData** cmdlet.

Una vez que los hosts sondean correctamente, podrá ver los datos para estos hosts físicos de la base de datos SILA donde hay un **datetime** en **sondeo reciente**. La sección 1 problema anterior proporciona pasos para recuperar estos datos.

###<a name="data-flow-issue-3"></a>Problema de flujo de datos 3
####<a name="too-many-physical-hosts-with-vms-listed-as-unknown-os"></a>Demasiados hosts físicos con máquinas virtuales que se muestran como sistema operativo desconocido

1. Elija un servidor Windows nodo final (VM) que sabe que está en uno de estos hosts, inicio de sesión como administrador.
2. Abra PowerShell como administrador.
3. Comprobar **SilLogging** se ejecuta mediante el **Get-SilLogging** cmdlet.
 - Si está ejecutando, intente insertar manualmente los datos SIL mediante el uso de **Publish-SilData**.

  - Si se produce un error:
     - Asegúrese del **targeturi** tiene **https://** en la entrada.
     - Asegúrese de que se cumplen todos los requisitos previos 
     - Asegúrese de que se instalan todas las actualizaciones necesarias para Windows Server (consulte los requisitos previos para SIL). Es una forma rápida de comprobar (en WS 2012 R2) en buscarlas mediante el siguiente cmdlet: **Get-SilWindowsUpdate \*3060, \*3000**
     - Asegúrese de que el certificado que se usa para autenticarse con el agregador está instalado en el almacén correcto en el servidor local para realizar el inventario con **SilLogging**.
     - En el agregador de SIL, asegúrese de que la huella digital del certificado que se usa para autenticarse con el agregador se agrega a la lista mediante el **Set-SilAggregator** **– AddCertificateThumbprint** cmdlet.
     - Si se utilizan certificados de empresa, compruebe que el servidor con SIL habilitado se haya unido al dominio para el que se creó el certificado, o que se pueda comprobar de otro modo con una entidad de certificación raíz. Si un certificado no es de confianza en el equipo local que intenta reenviar datos en un agregador o insertarlos en él, esta acción producirá un error.

    Si todo lo anterior se han comprobado y comprobado, pero el problema persiste:

    - Compruebe que el certificado usado para instalar el agregador de SIL sea correcto y coincide con el nombre del propio servidor del agregador de SIL. Asimismo, si se usaron certificados de empresa para instalar el agregador de SIL, es posible que el agregador deba unirse al dominio donde se creó el certificado (estos pasos no son necesarios si otras máquinas realizan el reenvío correctamente al mismo agregador de SIL).

    -  Por último, puede comprobar la siguiente ubicación para los archivos SIL almacenados en caché en el servidor intenta reenviar o insertar, **\Windows\System32\Logfiles\SIL**. Si **SilLogging** se ha iniciado y se ha estado ejecutando durante más de una hora, o **Publish-SilData** se ha ejecutado recientemente, y hay en este directorio, a ningún archivo de registro en el agregador Exitoso.

Si no hay ningún error y ninguna salida en la consola, a continuación, los datos push/publicar desde el nodo final de Windows Server en el agregador de SIL a través de HTTPS fue correcta. Siga la ruta de acceso del inicio de sesión hacia delante, datos en el agregador de SIL como administrador y examinar los archivos de datos que se recibieron. Vaya a **archivos de programa (x86)** &gt; **agregador de SIL Microsoft** &gt; directory SILA. Puede ver los archivos de datos que llegan en tiempo real.

>[!NOTE] 
>Se han transferido más de un archivo de datos con el **Publish-SilData** cmdlet. SIL en el nodo final almacenará en memoria caché de inserciones con error para hasta 30 días. En la siguiente inserción correcta todos los archivos de datos pasará al agregador para su procesamiento. De esta manera, una copia del agregador de SIL recién establecida puede mostrar datos desde un nodo final antes de su propio programa de instalación.

>[!NOTE] 
>Existen reglas que SILA sigue al procesar archivos de datos en el directorio SILA que solo son pertinentes en situaciones de poco tráfico. Mucho tráfico siempre activará el procesamiento en tiempo real. El comportamiento predeterminado es que el procesamiento comenzará cualquiera después de 100 archivos llegan en el directorio, o después de 15 minutos. Para solucionar el problema to-end en un entorno pequeño, es necesario esperar 15 minutos.

Después de procesan estos archivos, verá los datos de la base de datos.
