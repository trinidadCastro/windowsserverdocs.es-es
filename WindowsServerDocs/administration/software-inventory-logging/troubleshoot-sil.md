---
title: Troubleshoot Software Inventory Logging (Resolver problemas del Registro de inventario de software)
description: Describe cómo resolver problemas comunes de implementación de registro de inventario de software.
ms.custom: na
ms.prod: windows-server
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
author: brentfor
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: fb6e6fbba835e049748ca8578f24a1ff7fc750bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382902"
---
# <a name="troubleshoot-software-inventory-logging"></a>Troubleshoot Software Inventory Logging (Resolver problemas del Registro de inventario de software) 

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

## <a name="understanding-sil"></a>Descripción de SIL

Antes de empezar a solucionar problemas de SIL, debe tener una buena comprensión de sus componentes y cómo funciona. Los vídeos siguientes proporcionan información general sobre el agregador de SIL y SIL y cómo usarlos para reenviar e informar de los datos de inventario:

1. [Introducción al registro de inventario de software (SIL) (10:57)](https://channel9.msdn.com/Blogs/Regular-IT-Guy/An-Introduction-to-Software-Inventory-Logging-SIL)

2. [Registro de inventario de software: Configuración del agregador de SIL (14:34)](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Setting-up-SIL-Aggregator)

3. [Registro de inventario de software: Habilitación del reenvío de SIL (7:20)](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Enabling-SIL-Forwarding)

## <a name="how-sil-data-flow-works"></a>Cómo funciona el flujo de datos de SIL

El marco de trabajo de SIL tiene dos componentes principales y dos canales de comunicación. El flujo de datos a través de ambos canales, y entre ambos componentes, es necesario para una implementación correcta de SIL (se supone que un entorno físico virtualizado o en la nube solo necesita uno de los canales de comunicación). Deberá comprender los componentes y el flujo de datos de SIL para implementarlo correctamente. Después de ver los vídeos de información general anteriores, habrá encontrado el diagrama arquitectónico que muestra los componentes y el flujo de datos a través de ambos canales. Las flechas naranja indican consultas remotas en WinRM, las flechas con guiones verdes indican las entradas HTTPS del agregador de SIL desde SIL en cada nodo final de WS:

![](../media/software-inventory-logging/image1.png)

Si encuentra un problema con SIL, es probable que esté relacionado con una interrupción en el flujo de datos a través de los canales y entre los componentes. A continuación se muestran los problemas más comunes relacionados con el flujo de datos, seguidos de los pasos de solución de problemas para resolver cada uno de los tres problemas siguientes:

-   **Problema de flujo de datos 1** : **no hay datos en el informe cuando se usa el cmdlet Publish-SilReport** (o que normalmente faltan datos).

-   **Problema de flujo de datos 2** : **demasiados servidores en un host desconocido** del informe.

-   **Problema de flujo de datos 3** : **demasiadas máquinas virtuales en hosts físicos enumerados como sistema operativo desconocido** en el informe, o un error generado al usar **Publish-SilData** en servidores de Windows que ejecutan SIL.

## <a name="troubleshooting-data-flow-issues"></a>Solucionar problemas de flujo de datos

Antes de empezar, necesitará saber cuánto tiempo hace que se inicie el agregador de SIL con el cmdlet **Start-SilAggregator** .

>[!IMPORTANT]
>No habrá ningún dato en el informe hasta que el cubo de datos de SQL se procese en la hora del sistema local de 3 A.M. No continúe con los pasos de solución de problemas hasta que el cubo haya procesado los datos.

Si está solucionando problemas de datos en el informe (o que faltan en el informe) que es más reciente que la última vez que se procesó el cubo o antes de que se haya procesado el cubo (para una instalación nueva), siga estos pasos para procesar el cubo de datos SQL en tiempo real. :

1. Inicie sesión como administrador de SQL Server y ejecute **SSMS** en un símbolo del sistema.
2. Conéctese al moto de base de datos.
3. Expanda **Agente SQL Server** y, a continuación, expanda **trabajos**.
4. Haga clic con el botón derecho en **SILStagingRefresh** y seleccione **iniciar trabajo en el paso**.
5. Haga clic en **iniciar** y espere a que se complete la barra de progreso de la actualización.
6. Abra PowerShell como administrador y ejecute el cmdlet **Publish-silreport-OpenReport** .

Si todavía no hay ningún dato en el informe, continúe con la solución de problemas de los tres problemas de flujo de datos.

### <a name="data-flow-issue-1"></a>Problema de flujo de datos 1 

#### <a name="no-data-in-the-report-when-using-the-publish-silreport-cmdlet-or-data-is-generally-missing"></a>No hay datos en el informe cuando se usa el cmdlet Publish-SilReport (o los datos faltan normalmente)

Si faltan datos, es probable que se deba a que el cubo de datos de SQL aún no se ha procesado. Si se ha procesado recientemente y cree que los datos que faltan deberían haber llegado al agregador antes del procesamiento del cubo, siga la ruta de acceso de los datos en orden inverso. Elija un host único y una máquina virtual única para solucionar los problemas. La ruta de acceso de datos en orden inverso sería **Sila Report** &lt; **Sila Database** &lt; **Sila local Directory** &lt; **Remote virtual host** o **WS VM Running SIL Agent/Task**.

#### <a name="check-to-see-if-data-is-in-the-database"></a>Compruebe si los datos están en la base de datos

Hay dos maneras de comprobar los datos: **PowerShell** o **SSMS**.

>[!Important]
>Si el cubo ha procesado al menos una vez debido a que SILA insertó datos en la base de datos, estos datos deben reflejarse en el informe. Si no hay ningún dato en la base de datos, se produce un error en el sondeo de los hosts físicos, o bien no se recibe nada a través de HTTPS o ambos.

 #### <a name="powershell"></a>PowerShell

1. Abra PowerShell como administrador y ejecute el cmdlet **Get-silvmhost** y, a continuación, ejecute **Get-silaggregator**.

    >[!NOTE]
    >La salida de **Get-silaggregator** siempre imita la **pestaña de detalles de Windows Server** del informe de Excel. No se espera un resultado diferente.

2. Ejecución **de Get-silvmhost**
   - Si no aparece ningún host, agregue hosts mediante el cmdlet **Add-silvmhost** .
   - Si los hosts aparecen como **desconocidos**, vaya al problema 2. 
   d: si se enumeran los hosts pero no hay ningún **valor de fecha y hora** en la columna de **sondeo reciente** , vaya al **problema 2** a continuación.

**Otros comandos relacionados**

**Get-SilAggregator-NombreDeEquipo &lt;FQDN de un servidor conocido que inserta&gt;datos**: Esto producirá información de la base de datos acerca de un equipo (VM) incluso antes de que el cubo se haya procesado. Por lo tanto, este cmdlet se puede usar para comprobar los datos de la base de datos para que un servidor de Windows Inserte datos SIL a través de HTTPS, antes o sin, el proceso del cubo en 3 A.M. (o si no ha actualizado el cubo en tiempo real tal y como se describe al principio de esta sección).

**Get-SilAggregator-VmHostName &lt;FQDN de un host físico sondeado en el que hay un valor en la columna de sondeo reciente al usar el cmdlet&gt;Get-SilVmHost**: Esto producirá información de la base de datos acerca de un host físico incluso antes de que el cubo se haya procesado.

#### <a name="ssms"></a>SSMS

n**comprobar los datos de los hosts que se sondean:**
 
1. Abra **SSMS** y conéctese a la **motor de base de datos**.
2. Expanda **bases**de datos, expanda la base de datos **SoftwareInventoryLogging** , expanda **tablas**, haga clic con el botón secundario en la tabla **HostInfo** y seleccione las primeras 1000 filas. 

    Si hay datos para uno o más hosts de la tabla, el sondeo de esos hosts se ha realizado correctamente al menos una vez.

   **Compruebe si hay datos de máquinas virtuales o servidores independientes que hayan insertado datos a través de HTTPS:** 

3. Abra **SSMS** y conéctese a la **motor de base de datos**.
   R2. Expanda **bases**de datos, expanda la base de datos **SoftwareInventoryLogging** , expanda **tablas**, haga clic con el botón secundario en la tabla **VMInfo** y seleccione las primeras 1000 filas. 

    >[!NOTE] 
    >Cada fila de una máquina virtual única representará un archivo **bmil** procesado correctamente insertado a través de HTTPS y procesado por el agregador de SIL. Los archivos bmil son archivos patentados usados por SIL, uno de los cuales crea cada instancia de SIL. tenga en cuenta que esto solo es necesario cuando se usan SIL y SILA en entornos virtuales. De lo contrario, solo es necesario el tráfico HTTPS.

   Todos los datos de la base de datos deben reflejarse en los informes de SIL una vez procesado el cubo.

### <a name="data-flow-issue-2"></a>Problema de flujo de datos 2
#### <a name="too-many-servers-under-unknown-host"></a>Hay demasiados servidores en el host desconocido

Esto es probable que se produzca en entornos virtuales cuando el agregador de SIL no sondea correctamente los hosts físicos que hospedan las máquinas virtuales.

1. Abra **PowerShell** como administrador y ejecute el cmdlet **Get-silvmhost** .

    -   Si los hosts aparecen como **desconocidos**, el cmdlet **Add-silvmhost** no funcionaba correctamente, normalmente debido a credenciales no válidas agregadas para el acceso a estos hosts (por lo tanto, desconocidos). Pero si se comprueban las credenciales, podría significar que la detección automática de **hosttype** y **hypervisortype** en el cmdlet **Add-silvmhost** no pudo reconocer la plataforma. Hay pasos de solución de problemas avanzados disponibles para estas situaciones, pero no se describen aquí (consulte los canales del agregador de EventViewer SIL).

    -   Si se enumeran los hosts y **hosttype** y **hypervisortype** aparecen con valores que no son **desconocidos**, es decir, Windows y HyperV, o Ubuntu y Xen, etc., pero no hay ningún **valor de fecha y hora** en la columna de **sondeo reciente** , entonces el sondeo todavía no se ha producido correctamente.

        -   Tendrá que esperar una hora después de agregar el host para que se produzca el sondeo (si se establece este intervalo en predeterminado, se puede comprobar mediante el cmdlet **Get-silaggregator** ).

        -   Si ha transcurrido una hora desde que se agregó el host, compruebe que se está ejecutando la tarea de sondeo: En **programador de tareas**, seleccione **agregador de registro de inventario de software** en **Microsoft** &gt; **Windows** y compruebe el historial de la tarea.

    -   Si se muestra un host, pero no hay ningún valor para **RecentPoll**, **HostType**o **HypervisorType**, esto se puede omitir en gran medida. Esto solo se producirá en entornos de HyperV. Estos datos proceden realmente de la máquina virtual de Windows Server, que identifican el host físico en el que se ejecuta a través de HTTPS. Esto puede ser útil para identificar una máquina virtual específica que está notificando, pero requiere la minería de la base de datos mediante el cmdlet **Get-SilAggregatorData** .

Una vez que los hosts sondean correctamente, podrá ver los datos de estos hosts físicos en la base de datos SILA, donde hay un **valor DateTime** en **sondeo reciente**. En la sección del problema 1 anterior se proporcionan los pasos para recuperar estos datos.

### <a name="data-flow-issue-3"></a>Problema de flujo de datos 3
#### <a name="too-many-physical-hosts-with-vms-listed-as-unknown-os"></a>Demasiados hosts físicos con máquinas virtuales enumeradas como sistema operativo desconocido

1. Elija un nodo de extremo (VM) de Windows Server que sepa que se encuentra en uno de estos hosts, inicie sesión como administrador.
2. Abra PowerShell como administrador.
3. Compruebe que **SilLogging** se está ejecutando mediante el cmdlet **Get-SilLogging** .
   - Si se está ejecutando, intente enviar manualmente los datos de SIL mediante **Publish-SilData**.

   - Si se produce un error:
     - Asegúrese de que el **TargetUri** tiene **https://** en la entrada.
     - Asegurarse de que se cumplen todos los requisitos previos 
     - Asegúrese de que están instaladas todas las actualizaciones necesarias para Windows Server (consulte los requisitos previos de SIL). Una manera rápida de comprobar (solo en WS 2012 R2) es buscarlos mediante el siguiente cmdlet: **Get-SilWindowsUpdate \*3060, \*3000**
     - Asegúrese de que el certificado que se usa para autenticarse con el agregador esté instalado en el almacén correcto en el servidor local que se va a inventariar con **SilLogging**.
     - En el agregador de SIL, asegúrese de que la huella digital del certificado que se usa para autenticarse con el agregador se agrega a la lista con el cmdlet **set-SilAggregator** **– AddCertificateThumbprint** .
     - Si se utilizan certificados de empresa, compruebe que el servidor con SIL habilitado se haya unido al dominio para el que se creó el certificado, o que se pueda comprobar de otro modo con una entidad de certificación raíz. Si un certificado no es de confianza en el equipo local que intenta reenviar datos en un agregador o insertarlos en él, esta acción producirá un error.

     Si todo lo anterior se ha comprobado y comprobado, pero el problema persiste:

     - Compruebe que el certificado usado para instalar el agregador de SIL sea correcto y que coincida con el nombre del propio servidor del agregador de SIL. Asimismo, si se usaron certificados de empresa para instalar el agregador de SIL, es posible que el agregador deba unirse al dominio donde se creó el certificado (estos pasos no son necesarios si otras máquinas realizan el reenvío correctamente al mismo agregador de SIL).

     -  Por último, puede comprobar la siguiente ubicación de los archivos SIL almacenados en caché en el servidor que intenta reenviar o introducir, **\Windows\System32\Logfiles\SIL**. Si **SilLogging** se ha iniciado y se ha estado ejecutando durante más de una hora, o se ha ejecutado recientemente **Publish-SilData** , y no hay archivos en este directorio, el registro del agregador se ha realizado correctamente.

Si no hay ningún error y no hay ningún resultado en la consola, los datos de la instalación/extracción del nodo final de Windows Server al agregador de SIL a través de HTTPS se han realizado correctamente. Para seguir la ruta de acceso de los datos, inicie sesión en el agregador de SIL como administrador y examine los archivos de datos que han llegado. Vaya a **archivos de programa (x86)** &gt; **Microsoft SIL Aggregator** &gt; Sila directorio. Puede ver los archivos de datos que llegan en tiempo real.

>[!NOTE] 
>Es posible que se haya transferido más de un archivo de datos con el cmdlet **Publish-SilData** . SIL en el nodo final almacenará en caché los errores de inserciones durante 30 días. En la siguiente operación de instalación correcta, todos los archivos de datos Irán al agregador para su procesamiento. De esta manera, un agregador de SIL recién configurado podría mostrar los datos de un nodo final bien antes de su propia configuración.

>[!NOTE] 
>Hay reglas que se SILA a continuación al procesar archivos de datos en el directorio SILA que solo son relevantes en situaciones de bajo tráfico. El tráfico elevado siempre desencadenará el procesamiento en tiempo real. El comportamiento predeterminado es que el procesamiento se iniciará una vez que lleguen 100 archivos en el directorio o después de 15 minutos. Al solucionar problemas de un extremo a otro en un entorno pequeño, a menudo es necesario esperar 15 minutos.

Una vez procesados estos archivos, verá los datos en la base de datos.
