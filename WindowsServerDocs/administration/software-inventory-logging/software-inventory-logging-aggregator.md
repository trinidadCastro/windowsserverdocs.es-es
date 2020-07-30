---
title: Agregador de Registro de inventario de software
description: Describe cómo instalar y administrar el agregador de registro de inventario de software
ms.prod: windows-server
ms.technology: manage-software-inventory-logging
ms.topic: article
ms.assetid: e4230a75-6bcd-47d9-ba92-a052a90a6abc
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 350187c0ad7490a0698e4a3b99ef710b632f6c6c
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2020
ms.locfileid: "87408844"
---
# <a name="software-inventory-logging-aggregator"></a>Agregador de Registro de inventario de software

> Se aplica a: Windows Server 2012 R2

## <a name="what-is-software-inventory-logging-aggregator"></a>¿Qué es el agregador de registro de inventario de software?

El agregador de Registro de inventario de software (SILA) recibe, agrega y genera informes básicos del número y los tipos de software de empresa de Microsoft instalado en servidores de Windows en un centro de datos.

SILA es software que se instala en Windows Server, pero no se incluye en la instalación de Windows Server. Para instalar el software, descárguelo primero de forma gratuita del Centro de descarga de Windows: [Software Inventory Logging Aggregator 1.0 for Windows Server](https://www.microsoft.com/download/details.aspx?id=49046)

El marco de Registro de inventario de software está concebido para reducir los costos operativos del inventario de software de Microsoft que se implementa en muchos servidores de un entorno de TI. Este marco consta de dos componentes, este agregador de SIL y la característica de Windows Server, que se incorporó en Windows Server 2012 R2, registro de inventario de software (SIL). Este agregador de Registro de inventario de software 1.0 se instala en un servidor y recibe los datos de inventario de cualquier servidor de Windows Server configurado para reenviarle datos a través de SIL. El diseño permite a los administradores de centros de datos habilitar SIL en imágenes maestras de Windows Server con el fin de distribuirlo a nivel general en su entorno.  Este paquete de software es el punto de destino y está pensado para que los clientes lo instalen en su entorno local y puedan registrar los datos de inventario de forma fácil a lo largo del tiempo. Este software también permite la creación periódica de informes de inventario básicos en Microsoft Excel. Los informes del agregador de Registro de inventario de software 1.0 incluyen recuentos de instalaciones de Windows Server, System Center y SQL Server.

> [!IMPORTANT]
> No se envía ningún dato a Microsoft mediante el uso de este software.

### <a name="data-sil-collects-over-time"></a>Recopilaciones de datos de SIL con el tiempo

Una vez implementado correctamente, se pueden ver los datos siguientes en el agregador SIL:

-   Instalaciones únicas de Windows Server en el centro de datos

-   FQDN

-   GUID de identificación

-   Número de procesadores físicos y núcleos

-   Número de procesadores virtuales (en el caso de una máquina virtual)

-   Modelo y tipo de procesadores físicos

-   Si está habilitada o no la tecnología hyper-threading en los procesadores físicos

-   Número de serie del chasis

-   Recuento de límite superior, e identidad, de máquinas virtuales de Windows Server que se ejecutan simultáneamente (si hay un host ejecutando un hipervisor) en cada host, con el tiempo

-   Recuento de límite superior, y nombre de host, del agente de System Center administrado que se ejecuta simultáneamente \( \) en cada host, con el tiempo

-   Nombre de los agentes de System Center instalados en las máquinas virtuales contados en marca de límite superior administrado \-

-   Recuento y ubicación de instalaciones de SQL Server con el tiempo \( solo SKU y ediciones que requieren una licencia\)

-   Listas de software instalado en Agregar o quitar programas

### <a name="who-will-use-sil"></a>¿Quién usará SIL?

-   **Administradores de centros de datos o profesionales de TI**, que buscan un método de bajo costo de recopilar datos valiosos de inventario de software, automáticamente, con el tiempo.

-   **Cios y controladores de finanzas**, que necesitan informar del uso del software empresarial de Microsoft en las implementaciones de TI de sus organizaciones.

## <a name="getting-started"></a>Introducción
**Requisitos previos**

Agregador de Registro de inventario de software (agregador SIL) en un servidor como mínimo para agregación e informes, en una máquina o en hardware físico):

-   **Windows Server 2012 R2** (Standard o Datacenter Edition)

-   **El rol de servidor IIS,** con .NET Framework 4.5, servicios de WCF y activación HTTP, todos en el mismo árbol de selección en el **Asistente para agregar roles y características**.

-   **SQL Server** 2012 Sp2 Standard Edition o SQL Server 2014 Standard Edition

-   **Microsoft Excel de 64 bits** 2013 (opcional para la instalación, pero necesario para la creación de informes)

-   Opcional: **VMware PowerCLI 5.5.0.5836** (se necesita en los entornos de VMware)

>[!Note]
>Cuando se usa Windows Management Framework, existe un problema de compatibilidad conocido con la versión 5,1 de WMF, solo en el agregador de SIL.  No es necesario superar la versión de WMF 4,0 en los servidores que tengan instalado el agregador de SIL.

El Registro de inventario de software (SIL) se incluye en las versiones de Windows Server con las siguientes actualizaciones instaladas:

-   **Windows Server 2016**o superior

-   **Windows Server 2012 R2** (Standard o Datacenter Edition)

    -   Actualización de Windows Server 2012 R2 **KB3000850** (noviembre de 2014)

    -   Actualización de Windows Server 2012 R2 **KB3060681** (junio de 2015) (Es posible que aparezca como actualización opcional en Windows Update).

### <a name="security-and-account-types"></a>Seguridad y tipos de cuenta
**Requisito de certificado**

SIL y el agregador de SIL utilizan certificados SSL para la comunicación autenticada. La implementación habitual consiste en instalar el agregador de SIL con un certificado (el nombre del servidor y del certificado deben coincidir) para hospedar el servicio web que recibe los datos de inventario. A continuación, los servidores de Windows de los que se va a realizar un inventario con la característica de SIL usarán un certificado de cliente diferente para insertar los datos en el agregador de SIL. Es necesario usar un cmdlet de PowerShell (Set-SilAggregator, más detalles a continuación) para agregar huellas digitales de certificado a la lista de certificados aprobados del agregador de SIL a partir de la cual el agregador aceptará los datos asociados. El agregador de SIL continuará con el procesamiento y la inserción en su base de datos tras la autenticación de cada carga de datos con un certificado. Para conocer detalles más concretos sobre su funcionamiento, consulte **Detalles de los cmdlets del agregador de SIL**.

### <a name="polling-account-setup"></a>Configuración de la cuenta de sondeo
Al agregar credenciales al agregador de SIL para habilitar las operaciones de sondeo, debe considerar el uso de una cuenta con privilegios mínimos. Además, como procedimiento de seguridad recomendado, no debe usar las mismas credenciales para todos los hosts de un centro de datos u otra implementación de ti.

En un host de Windows Server que quiera configurar para el sondeo por el agregador de SIL, y evitar el uso de un usuario del grupo Administradores, siga estos pasos para proporcionar acceso suficiente a una cuenta de usuario:

#### <a name="to-setup-a-polling-account"></a>Para configurar una cuenta de sondeo

1.  En el host de Hyper-V de Windows Server que quiere sondear con el agregador de SIL, cree una cuenta de usuario local mediante **Administración de equipos** en Windows (asegúrese de desactivar la casilla que fuerza un cambio de contraseña en el primer inicio de sesión).

2.  Agregue este usuario al grupo **Usuarios de administración remota**.

3.  Agregue este usuario al grupo **Administradores de Hyper-V**.

4.  Abra **WMIMgmt. msc** con **Start** -> **Run**.

5.  En la sección **Acciones** haga clic en **Más acciones** y seleccione **Propiedades**.

6.  Haga clic en **Seguridad**.

7.  Seleccione **espacio de nombres de cimv2** en la vista de árbol **Espacio de nombres**.

8.  Haga clic en **Seguridad** (botón).

9. Agregue el grupo **Usuarios de administración remota** con el formato **machinename\nombre de grupo**

10. Haga clic en **OK**.

11. De nuevo en la ventana de seguridad para **root\cimv2**, seleccione **Usuarios de administración remota**.

12. En la sección permisos de la parte inferior, asegúrese de que está activada la opción **Remote enable** .

13. Haga clic en **Aplicar** y en **Aceptar**.

14. Haga clic en **Aceptar** en la ventana **propiedades** .

### <a name="installing-sil-aggregator"></a>Instalación del agregador de SIL
Antes de instalar el agregador de SIL en Windows Server debe asegurarse de cumplir los siguientes requisitos:

-   **Tiene un certificado SSL válido** que desea usar para hospedar el servicio Web de este software.

    -   El certificado debe estar en el formato **.pfx** .

    -   El nombre del servidor y del certificado de Windows Server deben coincidir.

-   **Está instalado SQL Server Standard Edition**, o lo está en un servidor remoto que pretende usar con este software.

    -   El agregador de SIL funciona con SQL Server 2012 SP2 y SQL Server 2014. No hay nada que se salga de lo ordinario al realizar selecciones durante la instalación de SQL Server.

    -   La cuenta usada para instalar el agregador de SIL debe ser un rol sysadmin en SQL para poder crear la base de datos durante la instalación.

    -   La cuenta usada para instalar el agregador de SIL debe agregarse como administrador en SQL Analysis Services antes de instalarlo.

    -   Una vez instalado, el Agente SQL Server se debe configurar para ejecutarse automáticamente.

-   **El rol de IIS se agrega** con .Net Framework 4.5, servicios WCF y activación HTTP, todo en el mismo árbol de selección del **Asistente para agregar roles y características**.

-   Ha **iniciado sesión en el servidor con una cuenta que tiene privilegios administrativos** en el servidor.

-   **Inicie sesión en el servidor con una cuenta que tiene privilegios sysadmin en el servidor de SQL Server**, si desea la autenticación de Windows.

    O BIEN

    Si lo que desea es la autenticación de SQL, **tiene la contraseña de una cuenta que tenga privilegios administrativos de SQL**.

#### <a name="to-install-software-inventory-logging-aggregator"></a>Para instalar el agregador de Registro de inventario de software

1.  Haga doble clic en **Setup.exe** para iniciar la instalación.

2.  En la página de bienvenida, haga clic en **Siguiente**.

3.  Si acepta los términos de licencia, active la casilla aceptar el contrato y, a continuación, haga clic en **siguiente**.

4.  En **Elegir características**, seleccione **Instalar el agregador de Registro de inventario de software y el módulo de informe** y luego haga clic en **Siguiente**.

    Para más información acerca de cómo instalar el módulo de informe, consulte `Publish-SilReport` en la sección **Detalles de los cmdlets del agregador de SIL**.

5.  Cuando haya comprobado todos los requisitos previos, haga clic en **Siguiente**.

6.  En **Elegir un tipo de cuenta**, seleccione **usuario local** o **gMSA**, en función de su preferencia.

    Al elegir la opción de la cuenta de usuario local se creará un usuario local con una contraseña segura generada automáticamente. Esta cuenta se utilizará en todos los servicios y las operaciones de tareas del agregador de SIL en el servidor local.  Se recomienda utilizar cuentas de servicio administradas de grupo (gMSA) si el agregador forma parte de un dominio de Active Directory (Windows Server 2012 y versiones posteriores). Para más información sobre gMSA, consulte: [Group Managed Service Accounts Overview](https://technet.microsoft.com/library/hh831782.aspx).

    -   Si tiene previsto ejecutar la base de datos de SQL Server en un servidor distinto al del agregador de SIL, se debe utilizar la opción de la cuenta gMSA.

    -   No olvide reiniciar el servidor después de agregar la cuenta de equipo al grupo de seguridad habilitado para gMSA en Active Directory.

7.  En **Elegir un servidor SQL Server**, especifique el servidor SQL Server donde está instalada la instancia de SQL, o **localhost**, si está instalada en el servidor local.

    Solo se admite un agregador de SIL por instancia de SQL.

8.  Seleccione el tipo de autenticación y haga clic en **Comprobar SQL**.

9. Haga clic en **Siguiente** y luego en **Detalles del servidor de Internet Information Services**. A continuación, seleccione un número de puerto o mantenga el valor predeterminado.

10. Vaya a la ubicación del archivo **.pfx**, escriba la contraseña para dicho archivo y luego haga clic en **Siguiente**.

11. La última pantalla mostrará el progreso de la instalación. Una vez completada correctamente, haga clic en **Finalizar**.

### <a name="uninstalling-sil-aggregator"></a>Desinstalación del agregador de SIL

#### <a name="to-uninstall-software-inventory-logging-aggregator"></a>Para desinstalar el agregador de Registro de inventario de Software

1.  Abra **PowerShell** como administrador y escriba `Stop-SilAggregator` . Cuando vuelva a aparecer el símbolo del sistema, significa que el agregador de SIL se ha detenido.

    Por motivos de diseño, el agregador de SIL procesa archivos después de 20 minutos o cuando se han recibido 100 archivos.  En entornos a gran escala nunca se dará esta situación, pero a pequeña escala, algunos archivos pueden quedar en espera de ser procesados antes de que el agregador se pueda detener. Si el mantenimiento de estos archivos y datos es innecesario, utilice el parámetro `–Force`.

2.  Vaya a **Panel de control**, haga clic en **Programas y características**, **Desinstalar programas**, **Agregador de Registro de inventario de software** y luego haga clic en **Desinstalar**.

    El agregador de Registro de inventario de software abrirá una ventana donde se le pedirá que elija entre eliminar todos los datos de la base de datos o mantener todos los datos en la base de datos. La selección predeterminada es mantenerlos (si se requiere una reinstalación, puede crear una asociación a la base de datos existente para seleccionar a partir de donde lo dejó el agregador).

3.  Seleccione **Mantener** o **Eliminar** y luego haga clic en **Siguiente**.

4.  Después de que termine la barra de progreso, haga clic en **Finalizar**.

### <a name="start-using-sil-and-the-sil-aggregator"></a>Uso inicial de SIL y del agregador de SIL

#### <a name="introduction-to-sil-aggregator-powershell-cmdlets"></a>Introducción a los cmdlets de PowerShell del agregador de SIL
Los siguientes comandos se pueden ejecutar desde la consola de Windows PowerShell como administrador.

|Cmdlet de Windows PowerShell|Función|
|-----------------------------|------------|
|`Start-SilAggregator`|Inicia todos los servicios y tareas del agregador de Registro de inventario de software. El agregador lo necesita para recibir datos a través de HTTPS desde servidores con el registro de SIL iniciado.|
|`Stop-SilAggregator`|Detiene todos los servicios y tareas del agregador de Registro de inventario de software. Si las tareas o los servicios están en el medio de operaciones, podría haber un retraso en la finalización de este comando.|
|`Set-SilAggregator`|Permite a los administradores realizar cambios de configuración en el agregador de Registro de inventario de software.|
|`Add-SilVmHost`|Se utiliza para agregar nombres de host específicos, o una matriz de nombres de host, que se va a sondear en un intervalo normal de forma predeterminada, en \( intervalos de una hora \) .|
|`Remove-SilVmHost`|Se utiliza para quitar nombres de host específicos, o un grupo de nombres de host, del sondeo a intervalos regulares.|
|`Get-SilVMHost`|Se utiliza para recuperar la lista de hosts físicos que están configurados en el agregador de Registro de inventario de software para sondearse en las máquinas virtuales en curso que ejecutan datos de estado.|
|`Get-SILAggregatorData`|Se utiliza para recuperar datos de la base de datos en la consola de PowerShell.|
|`Publish-SilReport`|Se utiliza para crear informes de la base de datos de datos de Registro de inventario de software. **Nota:** El procesamiento de cubos en el agregador se produce una vez al día. Así que los datos capturados en el agregador no aparecerán en informes hasta el día siguiente.|

#### <a name="suggested-order-to-start"></a>Orden de inicio sugerido
Una vez que tenga instalado el agregador de Registro de inventario de software en el servidor, abra PowerShell como administrador.

-   En el agregador de SIL:

    -   Ejecute `Start-SilAggregator`:

        Esto es necesario para que el agregador reciba activamente los datos que se le reenvían por HTTPS desde los servidores que tiene (o que tendrá) configurados para realizar su inventario. Tenga en cuenta que aunque primero haya habilitado los servidores para reenviar datos a este agregador, no pasa nada, dado que almacenarán en caché sus cargas de datos de forma local durante 30 días. Una vez que el agregador, su "TargetUri" esté en funcionamiento, todos los datos almacenados en caché se reenviarán a la vez al agregador y se procesarán todos los datos.

    -   Ejecute `Add-SilVMHost`:

        Ejemplo: `add-silvmhost –vmhostname contoso1 –hostcredential get-credential`

        -   En este ejemplo, **contoso1** es el nombre de red (o dirección IP) del servidor host físico que desea que sondee el agregador en busca de actualizaciones periódicas para las máquinas virtuales que se ejecutan en él con el fin de realizar el seguimiento de estos datos con el tiempo. Get-Credential solicitará al usuario que ha iniciado sesión que especifique la cuenta que se usará para sondear este host desde ese punto en adelante. La ejecución del mismo comando, en el mismo host, le permitirá actualizar la cuenta usada en cualquier momento. Tenga cuidado con los cambios y la caducidad de la contraseña de la cuenta con el paso del tiempo. Si las credenciales cambian o caducan, se producirá un error de sondeo en el host.

        -   De forma predeterminada, el sondeo se iniciará cada hora, comenzando una hora después de que se ejecuta `Start-SilAggregator` o una hora después de que se agrega un nuevo host a la lista de sondeo.  El intervalo de sondeo puede cambiarse mediante el cmdlet `Set-SilAggregator cmdlet`.

        -   Este cmdlet detecta automáticamente en una lista predefinida de opciones (consulte la sección **Detalles de los cmdlets del agregador de SIL**), qué valor de HostType y de HyperVisorType es correcto para el host que está agregando. Si no es capaz de reconocer estas credenciales o las proporcionadas son incorrectas, se mostrará un aviso. Si acepta con una entrada de **S**, se agregará el host, aparecerá como **Desconocido**, pero no se sondeará.

    -   Ejecutar `Set-SilAggregator –AddCertificateThumbprint` "huella digital del certificado de cliente"

        Es necesario para recibir datos por HTTPS desde servidores de Windows con el registro de SIL habilitado. La huella digital se agregará a la lista de huellas digitales de las que el agregador de SIL aceptará datos. El agregador de SIL está diseñado para aceptar certificados de autenticación de cliente de empresa válidos. El certificado usado deberá instalarse en el almacén ** \\ Localmachine\MY (equipo Local-> personal**) en el servidor que reenvía los datos.

-   En los servidores de Windows de los que se va a realizar un inventario, abra PowerShell como administrador y ejecute estos comandos:

    -   Ejecute `Set-SilLogging –TargetUri "https://contososilaggregator" –CertificateThumbprint "your client certificate's thumbprint"`:

        -   Esto indicará a SIL en Windows Server adónde enviar los datos de inventario y qué certificado utilizar para la autenticación.

            > [!IMPORTANT]
            > Asegúrese de que "https://" se encuentra en el valor TargetUri.

        -   Se debe instalar el certificado de cliente de empresa con esta huella digital en **\localmachine\MY**, o bien utilizar **certmgr.msc** para instalar el certificado en el almacén **Equipo local -> Personal**.

            > [!IMPORTANT]
            > Si estos valores no son correctos, o si el certificado no está instalado en el almacén correcto (o no es válido), los reenvíos al destino darán error cuando se inicie el registro de SIL. Los datos se almacenarán en caché de manera local durante 30 días.

    -   Ejecute `Start-SilLogging`:

        Esto inicia el registro de SIL. Cada hora, a intervalos aleatorios de una hora, SIL reenvía sus datos de inventario al agregador especificado con el parámetro `–targeturi` . El primer reenvío será un conjunto completo de datos. Cada reenvío posterior será más de un "latido" con solo identificar los datos que no ha cambiado nada. Si hay algún cambio en el conjunto de datos, se reenviará otro conjunto completo de datos.

    -   Ejecute `Publish-SilData`:

        -   La primera vez que SIL se habilita para el registro, este paso es opcional.

        -   Este es un reenvío manual de una sola vez de un conjunto completo de datos.

        -   Si el registro de SIL lleva un tiempo iniciado y se designa un nuevo agregador de SIL con `Set-SilLogging`, es necesario ejecutar este cmdlet, una sola vez, para enviar un conjunto completo de datos al nuevo agregador.

Una vez que haya seguido estos pasos para agregar hosts físicos que ejecutan máquinas virtuales de Windows Server, y que ha habilitado el Registro de inventario de software (o registro de SIL) dentro de esos servidores de Windows, puede ejecutar `Publish-SilReport –OpenReport` en cualquier momento en el agregador de SIL (se requiere Excel 2013). Tenga en cuenta sin embargo, que el cubo de SQL Server Analysis Services solo realiza procesamientos una vez al día, así que los datos no están disponibles en los informes el mismo día.

## <a name="architectural-overview"></a>Información general acerca de la arquitectura
SIL funciona en los modos de inserción y extracción y consta de dos componentes que funcionan en paralelo: la característica de Registro de inventario de software (SIL) en Windows Server y el agregador de Registro de inventario de Software (SILA), un MSI descargable. Los servidores objeto de inventario insertan datos de inventario de software en el agregador de SIL mediante SIL a través de HTTPS. Y esto lo hacen cada hora en puntos aleatorios dentro de cada hora. A su vez, el agregador sondea, o consulta, los hosts de hipervisor físico para extraer datos de inventario de hardware cada hora. Las operaciones de inserción y extracción se deben configurar correctamente para permitir la funcionalidad completa de SIL, pero se pueden configurar en cualquier orden. Sin embargo, como el procesamiento de cubos en el agregador tiene lugar una vez al día, los datos capturados en él, bien mediante inserción o extracción, no aparecerán en los informes hasta el día siguiente.

![Diagrama del agregador de registro de inventario de software](../media/software-inventory-logging/SILA_Architecture.png)

> [!IMPORTANT]
> No se envía ningún dato a Microsoft mediante el uso de este software.

## <a name="enable-sil-on-multiple-servers"></a>Habilitación de SIL en varios servidores
Existen varias maneras de habilitar SIL en una infraestructura de servidores distribuidos, por ejemplo, en una nube privada de máquinas virtuales.  A continuación se muestra un ejemplo de una de las maneras de configurar imágenes de Windows Server para enviar automáticamente datos de inventario a un agregador de SIL cuando se inicia en la red por primera vez.

Ejecute los siguientes cmdlets en la consola de PowerShell como administrador en cada máquina virtual en ejecución, o equipo o dispositivo físico, con Windows Server instalado (consulte la sección **Requisitos previos**):

Necesitará un certificado SSL de cliente válido en formato .pfx para seguir estos pasos.  Deberá agregar la huella digital del certificado a su agregador SIL mediante el cmdlet `Set-SILAggregator –AddCertificateThumbprint`. No es necesario que el nombre de este certificado de cliente sea el mismo que el de su agregador SIL.

-   `$secpasswd = ConvertTo-SecureString "`**<password for the account with permissions to the network location holding your client pfx file>**`" -AsPlainText –Force`

-   `$mycreds = New-Object System.Management.Automation.PSCredential ("`**<user account with permissions to the network location holding your client  pfx file>**`", $secpasswd)`

-   `$driveLetters = ([int][char]'C')..([int][char]'Z') | % {[char]$_}`

-   `$occupiedDriveLetters = Get-Volume | % DriveLetter`

-   `$availableDriveLetters = $driveLetters | ? {$occupiedDriveLetters -notcontains $_}`

-   `$firstAvailableDriveLetter = $availableDriveLetters[0]`

-   `New-PSDrive -Name $firstAvailableDriveLetter -PSProvider filesystem -root`** < \\ server\path que contiene el archivo de certificado pfx>**`-credential $mycreds`

-   `Copy-Item ${firstAvailableDriveLetter}:\`**<archivo certificatename. pfx en el directorio de la nueva unidad> c:\<location of your choice>**

-   `Remove-PSDrive –Name $firstAvailableDriveLetter`

-   `$mypwd = ConvertTo-SecureString -String "`**<password for the certificate pfx file>**`" -Force –AsPlainText`

-   `Import-PfxCertificate -FilePath c:\`**<Location \\ certificatename. pfx>**`cert:\localMachine\my -Password $mypwd`

-   `Set-sillogging –targeturi "https://`**<machinename of your SIL Aggregator>** `–certificatethumbprint`

> [!NOTE]
> Use la huella digital del certificado del archivo PFX de cliente y agregue al agregador de SIL mediante el cmdlet **set-SilAggregator '-AddCertificateThumbprint** .

-   `Start-sillogging`

En aquellas ocasiones en que no sea posible comunicarse con un agregador de SIL, los datos de inventario de SIL se almacenarán en caché de forma local en servidores de Windows durante 30 días como máximo. Una vez que se realiza una inserción correcta en el agregador, todos los datos almacenados en caché se reenvían.

Agregue `Publish-SilData` a la lista anterior si se insertan datos de SIL en un nuevo agregador de SIL tras inserciones realizadas con éxito en un agregador antiguo (de forma que se envía un conjunto completo de datos de SIL, que el nuevo agregador necesitará para esta máquina).

## <a name="software-inventory-logging-aggregator-reports"></a>Informes del agregador de Registro de inventario de software
![Imagen del informe del agregador de registro de inventario de software](../media/software-inventory-logging/SILA_Report.png)

### <a name="cube-processing"></a>Proceso de cubo
En un agregador de Registro de inventario de software, el cubo de SQL Server Analysis Services se procesará una vez al día a las 3:00 a.m., hora del sistema local. Los informes reflejarán todos los datos hasta esa hora, y nada más después de esa hora en el mismo día.

### <a name="high-water-mark"></a>Límite máximo
Un aspecto fundamental de los informes del agregador de registro de inventario de software es la captura de lo que se conoce comúnmente como "límite superior" de servidores de Windows que se ejecutan simultáneamente. Esto se aplica a los recuentos de Windows Server y System Center de estos informes. En Windows Server, cada host físico tiene un punto en el tiempo (sin importar el tipo de sistema operativo que tenga), en el curso de un mes, en que la mayoría de las máquinas virtuales se ejecutan de manera simultánea. Este es el límite máximo para el mes. De manera adicional, en System Center, hay un punto en el tiempo del mes en que la mayoría de los servidores de Windows administrados se ejecutan a la vez por cada host físico (un servidor administrado se identifica cuando uno o más agentes de System Center están presentes). Solo el límite superior más reciente de cualquier host físico se mostrará en el informe. Después de ese límite, no se mostrará ningún dato más. Y es de suponer que, después de ese punto, el número de máquinas virtuales de Windows Server (pestañas de WS) o de servidores de Windows administrados (pestañas SC), ha descendido por debajo del límite superior. Esta manera de representar y realizar el seguimiento del uso tiene como fin ayudarle a planear la capacidad y a coordinar los modelos de licencia de estos productos.

En las pestañas relacionadas con SQL del informe, las instalaciones de SQL Server se cuentan de forma acumulativa. no por marca de agua HIG. Los totales son un recuento actualizado de las instalaciones de SQL Server.

> [!NOTE]
> El uso del Registro de inventario de software no sustituye a la obligación de informar de forma precisa del uso de software de Microsoft bajo los términos de licencia aplicables.

### <a name="poll-date-time"></a>Fecha y hora de sondeo
Cuando se utiliza el agregador de Registro de inventario de software, es importante comprender que en los recuentos de límite superior la agregación se basa en el sondeo. Es decir, solo un sondeo del host físico subyacente puede capturar un límite superior. Por lo tanto, los recuentos de límite superior están asociados directamente a una "fecha y hora de sondeo" correspondiente. Aunque el intervalo de sondeo es ajustable, la fidelidad de los límites superiores capturados se verá afectada si se usa un valor de intervalo más alto. Cuanto mayor sea el intervalo, menos representativos serán los datos del uso real.

### <a name="reports-are-month-by-month"></a>Los informes son por meses
Todos los informes, incluso los anuales, se representan como informes por meses. Los límites superiores, los totales y los datos de equipo se restablecen al comienzo de cada mes del calendario.

Los datos del informe que resultan afectados por el cambio a un nuevo mes son:

-   Todos los límites superiores de todos los hosts se restablecen al inicio de un nuevo mes.

-   Si el agregador recibe al menos una carga completa de una máquina virtual (mediante HTTPS), pero deja de recibir latidos, todos los sondeos del host subyacente en ese mes dan por supuesta la asociación entre el host, la máquina virtual y los datos de la máquina virtual dado que la máquina virtual está en ejecución o se detiene a lo largo del mes. Al inicio del nuevo mes, esta asociación se borra hasta que una carga completa o latido se recibe de esa máquina virtual.

### <a name="additional-notes-on-report-behavior"></a>Notas adicionales sobre el comportamiento de informes

-   Las pestañas de resumen sirven de listas de referencia rápida del inventario. Los hosts y sus máquinas virtuales se muestran en la misma columna.

-   Omitir todos los valores que estén atenuados o atenuados. Se trata de artefactos de la creación de informes del cubo SSAS.

-   Si una máquina virtual se muestra con "so desconocido", significa que el agregador no ha recibido una carga completa de datos de esa máquina virtual a través de SIL a través de HTTPS.

-   Las máquinas virtuales que aparecen en "host desconocido" son máquinas virtuales de Windows Server que reenvían correctamente datos de inventario a través de HTTPS al agregador, pero el agregador no sondea activamente o correctamente el host subyacente de esa máquina virtual. En estas entradas los recuentos siempre serán cero dado que el host subyacente es desconocido. Utilice el cmdlet `Add-SilVMHost`, con las credenciales correctas, para agregar el host (o todos los hosts) al agregador de SIL para el sondeo. Una vez que se han sondeado correctamente, los datos de la máquina virtual y los datos del host se asociarán en los informes y se moverán hacia adelante.

-   Todas las fechas y horas son locales con respecto a la hora del sistema y la configuración regional del agregador de SIL. Esto incluye los datos de inventario recibidos de los sistemas habilitados para SIL a través de HTTPS. Cuando se procesan estos archivos (a los 20 minutos como máximo de recibirse), los datos se insertan en la base de datos con la hora del sistema local.

-   "Agregador de SIL" se denotará en cualquier equipo servidor que tenga instalado el agregador de registro de inventario de software.

-   Si un host físico cambia el número de procesadores o la cantidad de memoria física, aparecerá una nueva fila en el informe junto con la fila antigua. El sondeo de actualizaciones cesará en la fila antigua y continuará en la nueva como si fuera un host recién agregado.

-   En las pestañas **Resumen** y **Detalle** , el total mostrado en las columnas de servidores de Windows que se ejecutan de manera simultánea o de servidores de Windows administrados indica todos los límites superiores de todos los hosts. Entre ellos se incluyen los servidores de Windows que no son hosts de hipervisor y que no tienen máquinas virtuales en ejecución, así como los servidores que pueden tener máquinas virtuales en ejecución pero que son "desconocidos", ya que no se reciben datos desde la máquina virtual desde SIL a través de HTTPS. Pero se incluyen en los totales por comodidad.

-   En la sección **SQL Server** de la pestaña **Panel**, el recuento total de instalaciones de SQL Server es un resumen de todas las ediciones totales en el panel.  Esto puede llevar a una discrepancia en el total que se observa en la pestaña **Detalles de SQL** en aquellos casos en los que hay varias ediciones de SQL instaladas en un mismo servidor.  El panel las contaría por separado en cada servidor y la pestaña de **detalles** no.  Cuando hay varias ediciones de SQL instaladas en un servidor, de Windows siempre se cuentan como una, de acuerdo con los términos de licencia.

-   En la sección **Windows Server** de la pestaña **Panel**, las filas **Otros hosts de hipervisor** y **Total de hosts de hipervisor** incluyen los hosts de Windows Server que se pueden estar ejecutando o no en Hyper-V.

### <a name="column-descriptions"></a>Descripciones de las columnas
A continuación se describe cada una de las columnas de la pestaña **Detalle de Windows Server** del informe basado en Excel que crea el agregador de SIL. Otras pestañas de datos son las mismas columnas o un subconjunto de estas. La única excepción sería el "recuento de instalaciones" en las pestañas SQL Server (consulte la sección **límite superior** ).

|Encabezado de columna|Descripción|
|-----------------|---------------|
|Mes natural|Los datos de los informes se agrupan por mes, los más recientes en primer lugar. Los datos del mes no se muestran en un orden específico.|
|Nombre de host|Nombre de red, o FQDN, del host físico que sondea correctamente el agregador de SIL.<p>Use el cmdlet Get-SilVMHost para buscar hosts que se han agregado pero que no se sondean, o ya no se sondean, correctamente. Se mostrará el último sondeo correcto.|
|Tipo de host|Fabricante del sistema operativo en el host físico.|
|Tipo de hipervisor|Fabricante del hipervisor en el host físico.|
|Fabricante del procesador|Fabricante de los procesadores en el host físico.|
|Modelo de procesador|Modelo de los procesadores en el host físico.|
|¿Está habilitada la tecnología Hyper-Threading?|Se muestra como verdadero o falso, según si está habilitada dicha tecnología en los procesadores del host físico.|
|Nombre de la máquina virtual|El nombre de red, o FQDN, de la máquina virtual de Windows Server. Si el agregador no ha recibido datos de esta máquina a través de HTTPS, se muestra el nombre descriptivo de la máquina virtual en el hipervisor.|
|Máquinas virtuales de Windows Server que se ejecutan de manera simultánea por host|Recuento de máquinas virtuales de Windows Server que se ejecutan de manera simultánea en el host. El número más alto en el mes para ese host es el recuento de límite superior enumerado y capturado en ese momento en el tiempo.<p>Consulte la sección **Límite superior** de esta documentación.<p>Los hosts físicos con Windows Server instalado, o con Windows Server instalado y ninguna máquina virtual de Windows Server en ejecución conocida, siempre tendrán un recuento de uno. Si al menos una máquina virtual de Windows Server conocida se está ejecutando en el host, y Windows Server se está ejecutando en el propio host, el sistema operativo del host no forma parte del recuento.|
|Recuento de procesadores físicos|Número de procesadores físicos instalados en el host físico.|
|Recuento de núcleos físicos|Número de núcleos de procesadores físicos instalados en el host físico.|
|Recuento de procesadores virtuales|Número de procesadores virtuales que Windows reconoce dentro de la máquina virtual. Este valor solo procede de los datos reenviados a través de HTTPS mediante SIL en un servidor de Windows.|
|Fecha y hora de sondeo|Fecha y hora del último punto de límite superior de máquinas virtuales de Windows Server que se ejecutan de forma simultánea en ese host físico.<p>Consulte la sección **fecha y hora de sondeo** de esta documentación.|
|Fecha y hora de la última máquina virtual vista|Fecha y hora en que el agregador recibió por última vez el inventario de datos de esta máquina virtual de Windows Server a través de HTTPS.|
|Fecha y hora del último host visto|Fecha y hora en que el agregador recibió por última vez el inventario de datos de este host físico de Windows Server a través de HTTPS.<p>Se admite tener hosts físicos, que ejecutan Windows Server y Hyper-V, para habilitar SIL y reenviar datos de inventario a un agregador de SIL a través de HTTPS.|

## <a name="sil-aggregator-cmdlets-detail"></a>Detalles de los cmdlets del agregador de SIL
A continuación se muestran los detalles de los cmdlets del agregador de SIL. Para ver la documentación completa de los cmdlets, consulte: [Cmdlets de PowerShell del agregador de SIL](https://technet.microsoft.com/library/mt548455.aspx)

### <a name="publish-silreport"></a>Publish-SilReport

-   Este cmdlet, que se usa tal cual, creará un informe de registro de inventario de software y lo colocará en el directorio de documentos del usuario que ha iniciado sesión (se requiere Excel 2013 en el equipo en el que se ejecuta el cmdlet).

-   Cuando se utiliza con el parámetro `–OpenReport`, se crea el informe y se abre en Excel para su visualización.

-   Observará que al instalar el agregador de SIL, hay una opción para instalar solamente el módulo de informe. Es posible instalar el módulo de informe en un sistema operativo cliente de Windows, como Windows 8.1 o Windows 10. De esta forma, un cliente ligero, como un equipo portátil o una tableta, se puede conectar a un servidor de base de datos del agregador de SIL para publicar informes de SIL directamente.

    -   En el ejemplo siguiente se le pedirán las credenciales para usar y conectarse a un servidor de base de datos del agregador de SIL llamado SILContoso, y creará y abrirá un informe de SIL en el equipo local.

        `Publish-SilReport -DBServerName "SILContoso" -DBServerCredential Get-Credential –OpenReport`

    -   Antes de conectarse la primera vez, en la mayoría de los casos deberá abrir un puerto en el firewall en el servidor de base de datos del agregador de SIL para permitir las conexiones. Los profesionales de TI querrán configurar esto de antemano para que sus responsables financieros y otros administradores de inventario puedan acceder a sus propios informes. Para ver los pasos de cómo hacerlo, consulte el siguiente vínculo. Un puerto habitual para SQL Server Analysis Services es 2383.

### <a name="add-silvmhost"></a>Add-SilVMHost
Cuando se usa el cmdlet `Add-SilVMHost` se admiten los siguientes tipos de host y versiones de hipervisor. Observe que no es necesario especificarlos. El cmdlet `Add-SilVMHost` detecta automáticamente una combinación admitida. Si no es capaz de realizar tal detección, o las credenciales proporcionadas son incorrectas, se muestra un aviso. Si el usuario acepta con una entrada "Y", se agregará el host, pero no se sondeará. Se agregará como "desconocido".

|Versión del hipervisor|Valor de HostType del agregador de SIL|Valor de HypervisorType del agregador de SIL|
|----------------------|-----------------------------------------|---------------------------------------|
|Windows Server, 2012 R2|Windows|HyperV|
|VMware 5.5|VMware|Esxi|
|Xen 4.x|Ubuntu, OpenSuse o CentOS|Xen|
|XenServer 6.2|Citrix|XenServer|
|KVM|Ubuntu, OpenSuse o CentOS|KVM|

También pueden servidor otras versiones de estos tipos y plataformas de hipervisor.  El agregador de SIL se distribuye con la siguiente versión de sshnet.  Esta biblioteca se utiliza para la comunicación con plataformas de virtualización basadas en Linux.

<pre>sshnet 2014.4.6-beta1
https://sshnet.codeplex.com/releases/view/120504
Copyright (c) 2010, RENCI</pre>

### <a name="get-silaggregator"></a>Get-SilAggregator
`Get-SilAggregator` proporciona información de configuración de la aplicación del agregador de Registro de inventario de Software. La siguiente salida de ejemplo muestra:

-   La aplicación se está ejecutando

-   El intervalo de sondeo es cada hora (se puede cambiar en incrementos de una hora)

-   Hora de inicio del sondeo

-   URI de destino que deben establecer otros equipos para reenviar datos a este agregador

-   Huellas digitales de certificado de las que este agregador acepta datos de SIL

-   Tipo de cuenta especificado en la instalación

    `PS C:\Windows\system32> Get-SilAggregator`

    ``

    `State          : Running HostPollIntervalInHours : Every 1 Hour(s)`

    `PollStartTime      : 8/24/2015 5:07:33 AM`

    `TargetURI        : https://SilContoso`

    `CertificateThumbprint  : 3efc6b8ce7d5eefba5107ede9d1caca550417452, 2dc4ea8bfb64b1246a8c1ffa1b701cd1042a3412`

    `UserProfile       : Local`

### <a name="set-silaggregator"></a>Set-SilAggregator
Con el cmdlet `Set-SilAggregator`, puede:

-   Cambiar el intervalo horario durante el que tendrá lugar el sondeo.

-   Cambiar la fecha y hora para que el sondeo se inicie en el intervalo especificado.

-   Agregar o quitar huellas digitales de certificados de las que el agregador de SIL aceptará datos o con las que está asociado.

### <a name="get-aggregatordata"></a>Get-AggregatorData

-   Cuando se utiliza solo, este cmdlet muestra el contenido de la pestaña Detalle de Windows Server de un informe de Excel del agregador de SIL.

-   Cuando se utiliza con parámetros, este cmdlet recupera datos directamente de la base de datos que tienen como fin ayudarle con los usos personalizados de la solución global de SIL.

-   Tenga en cuenta que los parámetros `–StartTime` y `–Endtime` mostrarán los datos de informe comprendidos entre el primer día del mes de la fecha de inicio y el último día del mes de la fecha de finalización.

![Imagen del cmdlet Get-AggregatorData completado](../media/software-inventory-logging/SILA_Get-SILAggregator.png)

### <a name="get-silvmhost"></a>Get-SilVMHost

-   Este cmdlet devuelve la lista de hosts físicos que están configurados para sondearse en el agregador, la fecha y la hora más recientes de sondeo correcto, el valor de HostType (o fabricante del sistema operativo) y el valor de HypervisorType (fabricante del hipervisor). Para más información sobre HostType y HypervisorType, consulte los detalles del cmdlet Add-SilVMHost.

    Si un host no tiene una fecha y una hora de sondeo, pero tiene valores admitidos de HostType y HypervisorType, significa que el sondeo no ha comenzado aún o que aún no se ha realizado sin errores.

-   Este cmdlet también muestra los nombres de host que se agregaron a través de los datos procedentes de las propias máquinas virtuales, si están disponibles en la máquina virtual. Estos aparecerán en la lista, pero no tendrán ningún HostType o HypervisorType. Estos datos pueden ayudar a identificar las máquinas virtuales y los hosts que podrían no estar configurados para el sondeo.

-   Utilice los parámetros `–StartTime` y `–EndTime` para que le ayuden a comprender los hosts que se agregaron por primera vez y los que se sondearon por última vez.

### <a name="remove-silvmhost"></a>Remove-SilVMHost

-   Este cmdlet quita cualquier host de la lista de hosts que se van a sondear. Si se quita un host, es posible que una máquina virtual del host lo vuelva a agregar a la lista, pero no se sondeará con las credenciales correctas especificadas con el cmdlet `Add-SilVMHost`.

-   Si se quita un host, se quitará del sondeo pero no de los informes. Como el sondeo cesará, el host no estará presente en los informes el mes o los meses siguientes.

-   Utilice los parámetros `–StartTime` y`–EndTime` de forma individual para que le ayuden con la eliminación de los grupos de hosts que se sondearon correctamente hasta o a partir de una fecha.

## <a name="avoid-these-errors-and-issues-with-sil-and-sil-aggregator-troubleshooting-guide"></a>Errores y problemas con SIL y el agregador de SIL (Guía de solución de problemas)

-   Cosas que se deben comprobar si los cmdlets `SilLogging` o `Publish-Sildata` cmdlet dan error:

    -   Asegúrese de que el **TargetUri** tiene **https://** en la entrada.

    -   Asegúrese de que están instaladas todas las actualizaciones necesarias para Windows Server (consulte los requisitos previos para SIL).  Una forma rápida de comprobarlo es buscarlos mediante el siguiente cmdlet:`Get-SilWindowsUpdate *3060*, *3000*`

    -   Asegúrese de que el certificado que se usa para autenticarse con el agregador esté instalado en el almacén correcto en el servidor local del que se va a realizar el inventario con SilLogging (consulte la sección de introducción).

    -   En el agregador de SIL, asegúrese de que la huella digital del certificado que se usa para autenticarse con el agregador esté agregada a la lista con el cmdlet `Set-SilAggregator –AddCertificateThumbprint` (consulte la sección de introducción).

    -   Si se utilizan certificados de empresa, compruebe que el servidor con SIL habilitado se haya unido al dominio para el que se creó el certificado, o que se pueda comprobar de otro modo con una entidad de certificación raíz. Si un certificado no es de confianza en el equipo local que intenta reenviar datos en un agregador o insertarlos en él, esta acción producirá un error.

    -   Sise han realizado todas estas comprobaciones, puede comprobar que el certificado utilizado para instalar el agregador de SIL sea correcto y que coincida con el nombre del servidor del agregador de SIL propiamente dicho (este paso es innecesario si otras máquinas están reenviando correctamente al mismo agregador de SIL).

    -   Puede comprobar la siguiente ubicación de los archivos de SIL almacenados en caché en el servidor que intenta reenviar/introducir, \windows\system32. \\ logfiles \\ SIL. Si `SilLogging` se ha iniciado y lleva ejecutándose más de una hora, o `Publish-SilData` se ha ejecutado recientemente, y no hay archivos en este directorio, significa que el registro en el agregador se ha realizado correctamente.

-   Confirme que el usuario que ha iniciado sesión tiene acceso a la base de datos SQL y a Analysis Services.

    -   Este es un paso obligatorio al instalar el agregador de SIL.

    -   Esto es necesario cuando se usa PowerShell de manera remota para administrar el agregador de SIL.

-   Para publicar los informes del agregador de SIL desde un sistema operativo de escritorio cliente:

    -   Utilice la opción para instalar solo el módulo de informe en el cliente de Windows (8.1 10).

    -   Si encuentra problemas al intentar crear un informe mediante PowerShell en modo remoto, es probable que deba tener abierto un puerto de firewall en al agregador de SIL al que intenta conectarse (consulte la `Publish-SilReport` sección Detalles de los cmdlets del agregador de SIL).

-   Al utilizar la opción de gMSA:

    -   No olvide reiniciar el servidor después de unirlo al grupo de máquinas habilitadas para gMSA en Active Directory.

    -   En el proceso de instalación, no use un dominio completo al escribir dominio\usuario. Por ejemplo, use **mydomain\gmsaaccount**. No escriba **midominio. <i></i> com\gmsaaccount**.

-   Al usar el marco de administración de Windows en su entorno:

    -   Asegúrese de que los servidores con SILA instalado no tienen WMF 5,1 instalado.  Es posible que se produzca un error en el registro de eventos con respecto a la DLL **' mpunits.dll '**.  Esto impedirá el funcionamiento adecuado.  SILA solo requiere WMF 4,0.

## <a name="managing-sil-over-time"></a>Administración de SIL con el tiempo

### <a name="uninstallreinstall-sil-aggregator"></a>Desinstalación y reinstalación del agregador de SIL
Si es necesario desinstalar y reinstalar el agregador de SIL, puede hacerlo sin perder los datos de inventario existentes e históricos. Solo tiene que desinstalarlo (siguiendo los pasos proporcionados en esta documentación) y seleccionar la opción para mantener la base de datos de Registro de inventario de software. A continuación, reinstale el agregador de SIL (siguiendo los pasos proporcionados en esta documentación) y seleccione la opción para asociarlo a una base de datos existente.

Después de realizar esta operación, es necesario actualizar las credenciales mediante el cmdlet `Add-SilVMHost` en todos los hosts que se sondearon anteriormente con el agregador de SIL (suponiendo que desee continuar recopilando datos de estos hosts). Además, para evitar entradas duplicadas del mismo host en los informes, es necesario volver a agregar los hosts que se van a sondear con la misma dirección de red que la agregada originalmente. Estos son los tres tipos de vmhostname compatibles que pueden usarse para agregar un host con el cmdlet `Add-SilVMHost`:

-   Dirección IP

-   Nombre de dominio completo (FQDN)

-   Nombre NetBIOS

### <a name="changing-sil-aggregators"></a>Cambio de los agregadores de SIL
Si desea empezar a realizar un inventario de los servidores de su entorno con un agregador de SIL diferentes, use simplemente el cmdlet SIL en estos servidores para cambiar el targeturi (y la huella digital del certificado si es necesario), `Set-SilLogging –TargetUri`. Tenga en cuenta que después de hacer esto es necesario utilizar el cmdlet `Publish-SilData` al menos una vez para reenviar un inventario completo al agregador de SIL recién especificado.

### <a name="changing-or-updating-certificates"></a>Cambio o actualización de los certificados.
**PASOS IMPORTANTES PARA EVITAR LA PÉRDIDA DE DATOS:** si es necesario cambiar el certificado que van a utilizar los servidores para reenviar datos a un agregador de SIL, pero el agregador de destino seguirá siendo el mismo, siga estos pasos para evitar la posible pérdida de datos en tránsito al agregador:

-   En el agregador de SIL, utilice el cmdlet `Set-SilAggregator –AddCertificateThumbprint` para agregar la nueva huella digital al agregador de SIL.

-   En TODOS los servidores que reenvían datos, instale el nuevo certificado que se usará en **\LOCALMACHINE\MY** con su método preferido.

-   En TODOS los servidores que reenvían datos, utilice el cmdlet `Set-SilLogging –CertificateThumbprint` para actualizar a la huella digital del nuevo certificado.

-   **Muy importante: solo después de que se hayan actualizado todos los servidores que reenvían datos, quite la antigua huella digital** del agregador de SIL mediante el cmdlet `Set-SilAggregator –RemoveCertificateThumbprint`. Si un servidor que reenvía datos continúa reenviando con un certificado antiguo que se ha eliminado del agregador de SIL, **se perderán datos** y no se insertarán en la base de datos del agregador. Esto solo afecta a escenarios en los que un servidor ha reenviado datos correctamente a un agregador de SIL y, a continuación, se quita el certificado de la lista de huellas digitales del agregador de SIL para aceptar datos de.

## <a name="release-notes"></a>Notas de la versión

-   Hay un problema conocido que evita que el agregador SIL procese la presencia de las instalaciones de SQL Server Standard Edition y que informe de ellas.  A continuación le indicamos los pasos para corregirlo:

    1.  Abra SQL Server Management Studio en el agregador SIL.

    2.  Conéctese al moto de base de datos.

    3.  En el árbol de selección, expanda la base de datos SoftwareInventoryLogging y, a continuación, Tables.

    4.  Haga clic con el botón secundario en **dbo. SqlServerEdition**y, a continuación, seleccione "**editar las primeras 200 filas**".

    5.  Cambie PropertyNumValue junto a "Standard Edition" a **2760240536** (de-1534726760).

    6.  Cierre la consulta para guardar el cambio.

    7.  Para cualquier servidor que ejecute SIL y que ya haya registrado datos en este agregador, es posible que necesite ejecutar una vez el cmdlet `Publish-SilData` para el agregador para que procese correctamente la presencia de SQL Server Standard Edition.

-   En los informes generados por SIL, todos los recuentos de núcleos de procesador incluyen el número de subprocesos de hyper-threading si esta tecnología está habilitada en el servidor físico.  Para obtener recuentos reales de núcleos físicos en servidores que tienen habilitada la característica de hyper-threading, es necesario reducir estos recuentos a la mitad.

-   Los totales de las filas (en la pestaña **Panel** ) y las columnas (en las pestañas **Resumen y detalle** ) con la etiqueta "se**ejecutan de forma simultánea**...", para Windows Server y System Center no coinciden exactamente entre las dos ubicaciones. En la pestaña **Panel** , es necesario agregar el valor "**dispositivos de Windows Server (sin máquinas virtuales conocidas**)" a la sección "**ejecutando de forma simultánea**..." valor para igualar este número en las pestañas **Resumen y detalle** .

-   Consulte **PASOS IMPORTANTES PARA EVITAR LA PÉRDIDA DE DATOS** al cambiar o actualizar los certificados en la sección **Administración de SIL a lo largo del tiempo** de esta documentación.

-   Aunque es posible agregar hosts de Windows Server 2008 R2 y Windows Server 2012 a la lista de hosts de sondeo, esta versión (1.0) del agregador de SIL solo admite el sondeo de hosts de Windows Server 2012 R2 basados en Windows o Hyper-V, lo que asegura el funcionamiento correcto de todas las características y funciones.  En especial, se sabe que al sondear hosts de Windows Server 2008 R2, puede que las máquinas virtuales y los hosts no coincidan en los informes del agregador de SIL.

## <a name="see-also"></a>Consulte también
[Software Inventory Logging Aggregator 1.0 for Windows Server](https://www.microsoft.com/download/details.aspx?id=49046)<br>
[Cmdlets de PowerShell del agregador de SIL](https://technet.microsoft.com/library/mt548455.aspx)<br>
[Cmdlets de PowerShell de SIL](https://technet.microsoft.com/library/dn283390.aspx)<br>
[Información general de SIL](https://technet.microsoft.com/library/dn268301.aspx)<br>
[Administración de NPS](https://technet.microsoft.com/library/dn383584.aspx)

