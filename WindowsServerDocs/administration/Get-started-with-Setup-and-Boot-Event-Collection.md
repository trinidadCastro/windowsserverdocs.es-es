---
title: Introducción a la configuración y la recopilación de eventos de arranque
description: Configuración de recopiladores y destinos de la recopilación de eventos de instalación y arranque
manager: DonGill
ms.localizationpriority: medium
ms.date: 10/16/2017
ms.topic: get-started-article
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247b3f8
author: jaimeo
ms.author: jaimeo
ms.openlocfilehash: e5e18ed5f5cc4cba319042f1a5da84acae8e5fd5
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87879539"
---
# <a name="get-started-with-setup-and-boot-event-collection"></a>Introducción a la configuración y la recopilación de eventos de arranque

> Se aplica a: Windows Server

## <a name="overview"></a>Información general
La configuración y la recopilación de eventos de arranque es una característica nueva de Windows Server 2016 que le permite designar un equipo recopilador que puede recopilar diversos eventos importantes que se producen en otros equipos cuando arrancan o pasan por el proceso de instalación. Puedes analizar, más adelante, los eventos recopilados con Visor de eventos, Analizador de mensajes, Wevtutil o cmdlets de Windows PowerShell.

Anteriormente, estos eventos han sido imposibles de supervisar porque la infraestructura necesaria para recopilarlos no existe hasta que un equipo ya está configurado. Entre los tipos de eventos de instalación y de arranque que puede supervisar se incluyen los siguientes:

-   Carga de módulos y controladores de kernel

-   Enumeración de los dispositivos y la inicialización de sus controladores (incluidos los dispositivos como el tipo de CPU)

-   Comprobación y montaje de sistemas de archivos

-   Inicio de archivos ejecutables

-   Inicio y finalización de las actualizaciones del sistema

-   Los puntos en los que el sistema está disponible para el inicio de sesión, establece la conexión con un controlador de dominio, la finalización del servicio se inicia y la disponibilidad de los recursos compartidos de red

El equipo del recopilador debe ejecutar Windows Server 2016 (puede estar en cualquier servidor con experiencia de escritorio o modo Server Core). El equipo de destino debe estar ejecutando Windows 10 o Windows Server 2016. También puede ejecutar este servicio en una máquina virtual que esté hospedada en un equipo que **no** ejecute Windows Server 2016. Se sabe que funcionan las siguientes combinaciones de equipos de destino y recopilador virtualizado:

|Host de virtualización|Máquina virtual del recopilador|Máquina virtual de destino|
|-----------------------|-----------------------------|--------------------------|
|Windows 8.1|sí|sí|
|Windows 10|sí|sí|
|Windows Server 2016|sí|sí|
|Windows Server 2012 R2|sí|No|

## <a name="installing-the-collector-service"></a>Instalación del servicio Recopilador
A partir de Windows Server 2016, el servicio Recopilador de eventos está disponible como una característica opcional. En esta versión, puede instalarlo mediante DISM.exe con este comando en un símbolo del sistema de Windows PowerShell con privilegios elevados:

`dism /online /enable-feature /featurename:SetupAndBootEventCollection`

Este comando crea un servicio denominado BootEventCollector y lo inicia con un archivo de configuración vacío.

Compruebe que la instalación se realiza correctamente. para ello, compruebe `get-service -displayname *boot*` . El recopilador de eventos de arranque debe estar en ejecución. Se ejecuta bajo la cuenta de servicio de red y crea un archivo de configuración vacío (Active.xml) en **%SystemDrive%\ProgramData\Microsoft\BootEventCollector\Config**.

También puede instalar el servicio de recopilación de eventos de instalación y de arranque con el Asistente para agregar roles y características en Administrador del servidor.

## <a name="configuration"></a>Configuración
Debe configurar dos elementos para recopilar los eventos de configuración y de arranque.

-   En los equipos de destino que enviarán los eventos (es decir, los equipos cuya configuración y arranque desea supervisar), habilite el transporte KDNET/EVENT-NET y habilite el reenvío de eventos.

-   En el equipo del recopilador, especifique los equipos de los que desea aceptar eventos y dónde guardarlos.

> [!NOTE]
> No se puede configurar un equipo para enviar los eventos de inicio o de arranque a sí mismo. Pero si desea supervisar dos equipos, puede configurarlos para que envíen los eventos entre sí.

### <a name="configuring-a-target-computer"></a>Configuración de un equipo de destino
En cada equipo de destino, primero se habilita el transporte KDNET/EVENT-NET, se habilita el envío de eventos ETW a través del transporte y, a continuación, se reinicia el equipo de destino. EVENT-NET es un protocolo de transporte en el kernel que es similar a KDNET (el protocolo del depurador del kernel). EVENT-NET solo transmite eventos y no permite el acceso al depurador. Estos dos protocolos son mutuamente excluyentes; solo puede habilitar una de ellas a la vez.

Puede habilitar el transporte de eventos de forma remota (con Windows PowerShell) o localmente.

##### <a name="to-enable-event-transport-remotely"></a>Para habilitar el transporte de eventos de forma remota

1.  Si ya ha configurado la comunicación remota de Windows PowerShell en el equipo de destino, vaya al paso 3. Si no es así, en el equipo de destino, abra un símbolo del sistema y ejecute el siguiente comando:

    winrm quickconfig

2.  Responda a los mensajes y, a continuación, reinicie el equipo de destino. Si los equipos de destino no están en el mismo dominio que el equipo del recopilador, es posible que tenga que definirlos como hosts de confianza. Para ello:

3.  En el equipo del recopilador, ejecute cualquiera de estos comandos:

    -   En un símbolo del sistema de Windows PowerShell: `Set-Item -Force WSMan:\localhost\Client\TrustedHosts <target1>,<target2>,...` , seguido de `Set-Item -Force WSMan:\localhost\Client\AllowUnencrypted true` Where \<target1> , etc. son los nombres o las direcciones IP de los equipos de destino.

    -   O bien, en un símbolo del sistema: **WinRM Set WinRM/config/Client @ {TrustedHosts = \<target1> , \<target2> ,...; AllowUnencrypted = true}**

        > [!IMPORTANT]
        > Esto configura la comunicación sin cifrar, por lo que no debe hacerlo fuera de un entorno de laboratorio.

4.  Pruebe la conexión remota; para ello, vaya al equipo del recopilador y ejecute uno de estos comandos de Windows PowerShell:

    Si el equipo de destino está en el mismo dominio que el equipo del recopilador, ejecute`New-PSSession -Computer <target> | Remove-PSSession`

    Si el equipo de destino no está en el mismo dominio, ejecute `New-PSSession -Computer  <target>  -Credential Administrator | Remove-PSSession` , lo que le solicitará las credenciales.

    Si el comando no devuelve nada, la comunicación remota se realizó correctamente.

5.  En el equipo de destino, abra un símbolo del sistema de Windows PowerShell con privilegios elevados y ejecute este comando:

    `Enable-SbecBcd -ComputerName <target_name> -CollectorIP <ip> -CollectorPort <port> -Key <a.b.c.d>`

    Aquí <target_name> es el nombre del equipo de destino, \<ip> es la dirección IP del equipo del recopilador. \<port>es el número de puerto en el que se ejecutará el recopilador. La clave <a. b. c. d> es una clave de cifrado necesaria para la comunicación, que consta de cuatro cadenas alfanuméricas separadas por puntos. Esta misma clave se usa en el equipo del recopilador. Si no especifica una clave, el sistema genera una clave aleatoria; lo necesitará para el equipo del recopilador, por lo que debe anotarlo.

6.  Si ya tiene un equipo del recopilador configurado, actualice el archivo de configuración en el equipo del recopilador con la información del nuevo equipo de destino. Consulte la sección configuración del equipo del recopilador para obtener más información.

##### <a name="to-enable-event-transport-locally-on-the-target-computer"></a>Para habilitar el transporte de eventos localmente en el equipo de destino

1.  Inicie un símbolo del sistema con privilegios elevados y, a continuación, ejecute estos comandos:

    **bcdedit/Event sí**

    **bcdedit/eventsettings net HostIP: 1.2.3.4 Port: 50000 Key: a.b.c. d**

    Aquí 1.2.3.4 es un ejemplo: Reemplácelo por la dirección IP del equipo del recopilador. Reemplace también 50000 por el número de puerto en el que se ejecutará el recopilador y a a.b.c. d con la clave de cifrado necesaria para la comunicación. Esta misma clave se usa en el equipo del recopilador. Si no especifica una clave, el sistema genera una clave aleatoria; lo necesitará para el equipo del recopilador, por lo que debe anotarlo.

2.  Si ya tiene un equipo del recopilador configurado, actualice el archivo de configuración en el equipo del recopilador con la información del nuevo equipo de destino. Consulte la sección configuración del equipo del recopilador para obtener más información.

**Ahora que el propio transporte de eventos está habilitado, debe permitir que el sistema envíe realmente eventos ETW a través de ese transporte.**

##### <a name="to-enable-sending-of-etw-events-through-the-transport-remotely"></a>Para habilitar el envío de eventos ETW a través del transporte de forma remota

1.  En el equipo del recopilador, abra un símbolo del sistema de Windows PowerShell con privilegios elevados.

2.  Ejecute `Enable-SbecAutologger -ComputerName <target_name>` , donde <target_name> es el nombre del equipo de destino.

Si no puede configurar la comunicación remota de Windows PowerShell, siempre puede habilitar el envío de eventos directamente en el equipo de destino.

##### <a name="to-enable-sending-of-etw-events-through-the-transport-locally"></a>Para habilitar el envío de eventos ETW a través del transporte de forma local

1.  En el equipo de destino, inicie Regedit.exe y busque esta clave del registro:

    **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\wmi\autologger**. Las distintas sesiones de registro se enumeran como subclaves en esta clave. La **plataforma de instalación**, el **registrador de kernel de NT**y **Microsoft-Windows-Setup** son opciones posibles para su uso con la configuración y la recopilación de eventos de arranque, pero la opción recomendada es **EventLog-System**. Estas claves se detallan en [configurar e iniciar una sesión de registrador automático](https://msdn.microsoft.com/library/windows/desktop/aa363687(v=vs.85).aspx).

2.  En la clave EventLog-System, cambie el valor de **LogFileMode** de **0x10000180** a **0x10080180**. Para obtener más información sobre los detalles de esta configuración, vea [constantes del modo de registro](https://msdn.microsoft.com/library/windows/desktop/aa364080(v=vs.85).aspx).

3.  Opcionalmente, también puede habilitar el reenvío de los datos de comprobación de errores al equipo del recopilador. Para ello, busque la clave del registro HKEY_LOCAL_MACHINE administrador de \SYSTEM\CurrentControlSet\Control\Session y cree el **filtro de impresión de depuración** de clave con un valor de **0x1**.

4.  Reinicie el equipo de destino.

### <a name="choosing-the-network-adapter"></a>Elección del adaptador de red
Si el equipo de destino tiene más de un adaptador de red, el controlador KDNET elegirá el primero que se admita. Puede especificar un adaptador de red determinado que se usará para reenviar eventos de instalación con estos pasos:

##### <a name="to-specify-a-network-adapter"></a>Para especificar un adaptador de red

1.  En el equipo de destino, abra Device Manager, expanda **adaptadores de red**, busque el adaptador de red que desea usar y haga clic con el botón secundario en él.

2.  En el menú que se abre, haga clic en **propiedades**y, a continuación, haga clic en la pestaña **detalles** . Expanda el menú del campo de **propiedad** , desplácese para buscar **información de ubicación** (la lista probablemente no esté en orden alfabético) y haga clic en ella. El valor será una cadena con el formato **PCI bus X, dispositivo Y, función Z**. Tome nota de X. Y. Z; Estos son los parámetros de bus que necesita para el siguiente comando.

3.  Ejecute uno de estos comandos:

    Desde un símbolo del sistema de Windows PowerShell con privilegios elevados:`Enable-SbecBcd -ComputerName <target_name> -CollectorIP <ip> -CollectorPort <port> -Key <a.b.c.d> -BusParams <X.Y.Z>`

    Desde un símbolo del sistema con privilegios elevados: **bcdedit/eventsettings net HostIP: AAA Puerto: 50000 Key: BBB busparams: X. Y. Z**

### <a name="validate-target-computer-configuration"></a>Validar la configuración del equipo de destino
Para comprobar la configuración en el equipo de destino, abra un símbolo del sistema con privilegios elevados y ejecute **bcdedit/enum**. Cuando haya terminado, ejecute **bcdedit/eventsettings**. Puede hacer doble comprobación de los siguientes valores:

-   Key

-   Debugtype = NET

-   HostIP =\<IP address of the collector>

-   Puerto = \<port number you specified for the collector to use>

-   DHCP = sí

Compruebe también que ha habilitado **bcdedit/Event**, ya que **/Debug** y **/Event** son mutuamente excluyentes. Solo puede ejecutar una u otra. Del mismo modo, no puede mezclar/eventsettings con/debug o/dbgsettings con/Event.

Tenga en cuenta también que la recopilación de eventos no funciona si se establece en un puerto serie.

### <a name="configuring-the-collector-computer"></a>Configuración del equipo del recopilador
El servicio Recopilador recibe los eventos y los guarda en archivos ETL. Estos archivos ETL pueden ser leídos por otras herramientas, como Visor de eventos, el analizador de mensajes, wevtutil y los cmdlets de Windows PowerShell.

Dado que el formato ETW no permite especificar el nombre del equipo de destino, los eventos de cada equipo de destino deben guardarse en un archivo independiente. Las herramientas de visualización pueden mostrar un nombre de equipo, pero será el nombre del equipo en el que se ejecuta la herramienta.

Más exactamente, cada equipo de destino tiene asignado un anillo de archivos ETL. Cada nombre de archivo incluye un índice entre 000 y un valor máximo que se configura (hasta 999). Cuando el archivo alcanza el tamaño máximo configurado, cambia la escritura de eventos en el siguiente archivo. Después del archivo más alto posible, vuelve a cambiar al índice de archivo 000. De esta manera, los archivos se reciclan automáticamente, lo que limita el uso del espacio en disco. También puede establecer directivas de retención externas adicionales para limitar aún más el uso del disco; por ejemplo, puede eliminar archivos con una antigüedad superior a un número de días establecido.

Los archivos ETL recopilados normalmente se conservan en el directorio **c:\ProgramData\Microsoft\BootEventCollector\Etl** (que puede tener subdirectorios adicionales). Puede buscar el archivo de registro más reciente si los ordena por la hora de la última modificación. También hay un registro de estado (normalmente en **c:\ProgramData\Microsoft\BootEventCollector\Logs**), que registra cada vez que el recopilador cambia de escritura en un archivo nuevo.

También hay un registro de recopilador, que registra información sobre el recopilador. Puede mantener este registro en el formato ETW (en el que se envían los eventos al servicio de registro de Windows; este es el valor predeterminado) o en un archivo (normalmente en **c:\ProgramData\Microsoft\BootEventCollector\Logs**). El uso de un archivo puede ser útil si desea habilitar los modos detallados que producen una gran cantidad de datos. También puede establecer el registro para escribir en una salida estándar mediante la ejecución del recopilador desde la línea de comandos.

**Crear el archivo de configuración del recopilador**

Cuando se habilita el servicio, se crean tres archivos de configuración XML y se almacenan en **c:\ProgramData\Microsoft\BootEventCollector\Config**:

-   **Active.xml** Este archivo contiene la configuración activa actual del servicio Recopilador.  Justo después de la instalación, este archivo tiene el mismo contenido que Empty.xml. Cuando se establece una nueva configuración de recopilador, se guarda en este archivo.

-   **Empty.xml** Este archivo contiene los elementos de configuración mínimos necesarios con sus valores predeterminados establecidos. No habilita ninguna colección; solo permite que el servicio de recopilador se inicie en un modo inactivo.

-   **Example.xml** Este archivo proporciona ejemplos y explicaciones de los posibles elementos de configuración.

**Elección de un límite de tamaño de archivo**

Una de las decisiones que debe tomar es establecer un límite de tamaño de archivo. El mejor límite de tamaño de archivo depende del volumen de eventos esperado y del espacio disponible en disco. Los archivos más pequeños son más cómodos desde el punto de vista de la limpieza de los datos antiguos. Sin embargo, cada archivo conlleva la sobrecarga de un encabezado de 64 KB y la lectura de muchos archivos para obtener el historial combinado puede no ser conveniente. El límite de tamaño de archivo mínimo absoluto es 256 KB. Un límite de tamaño de archivo práctico razonable debe ser superior a 1 MB y 10 MB es probablemente un buen valor típico. Un límite más alto podría ser razonable si espera muchos eventos.

Hay varios detalles que hay que tener en cuenta con respecto al archivo de configuración:

-   La dirección del equipo de destino. Puede usar su dirección IPv4, una dirección MAC o un GUID de SMBIOS. Tenga en cuenta estos factores al elegir la dirección que se va a usar:

    -   La dirección IPv4 funciona mejor con la asignación estática de las direcciones IP. Sin embargo, incluso las direcciones IP estáticas deben estar disponibles a través de DHCP.

    -   Una dirección MAC o un GUID de SMBIOS resulta útil cuando se conocen de antemano pero las direcciones IP se asignan de forma dinámica.

    -   El protocolo EVENT-NET no admite las direcciones IPv6.

    -   Es posible especificar varias maneras de identificar el equipo. Por ejemplo, si el hardware físico está a punto de ser reemplazado, puede escribir tanto la dirección MAC antigua como la nueva y se aceptarán.

-   La clave de cifrado utilizada para la comunicación con el equipo del recopilador

-   Nombre del equipo de destino. Puede usar la dirección IP, el nombre de host o cualquier otro nombre como nombre de equipo.

-   El nombre del archivo ETL que se va a usar y la configuración de tamaño de anillo para él.

##### <a name="to-create-the-configuration-file"></a>Para crear el archivo de configuración

1.  Abra un símbolo del sistema de Windows PowerShell con privilegios elevados y cambie los directorios a%SystemDrive%\ProgramData\Microsoft\BootEventCollector\Config.

2.  Escriba `notepad .\newconfig.xml` y presione Entrar.

3.  Copie esta configuración de ejemplo en la ventana del Bloc de notas:

    ```
    <collector configVersionMajor=1 statuslog=c:\ProgramData\Microsoft\BootEventCollector\Logs\statuslog.xml>
      <common>
        <collectorport value=50000/>
        <forwarder type=etl>
          <set name=file value=c:\ProgramData\Microsoft\BootEventCollector\Etl\{computer}\{computer}_{#3}.etl/>
          <set name=size value=10mb/>
          <set name=nfiles value=10/>
          <set name=toxml value=none/>
        </forwarder>
        <target>
          <ipv4 value=192.168.1.1/>
          <key value=a.b.c.d/>
          <computer value=computer1/>
        </target>
        <target>
          <ipv4 value=192.168.1.2/>
          <key value=d1.e2.f3.g4/>
          <computer value=computer2/>
        </target>
      </common>
    </collector>
    ```

    > [!NOTE]
    > El nodo raíz es \<collector> . Sus atributos especifican la versión de la sintaxis del archivo de configuración y el nombre del archivo de registro de estado.
    >
    > El \<common> elemento agrupa varios destinos que especifican los elementos de configuración comunes para ellos, de forma muy parecida a como se puede usar un grupo de usuarios para especificar los permisos comunes para varios usuarios.
    >
    > El \<collectorport> elemento define el número de puerto UDP en el que el recopilador escuchará los datos entrantes. Este es el mismo puerto que se especificó en el paso de configuración de destino para Bcdedit. El recopilador solo admite un puerto y todos los destinos deben conectarse al mismo puerto.
    >
    > El \<forwarder> elemento especifica cómo se reenviarán los eventos ETW recibidos de los equipos de destino. Solo hay un tipo de reenviador, que los escribe en los archivos ETL. Los parámetros especifican el patrón de nombre de archivo, el límite de tamaño para cada archivo del anillo y el tamaño del anillo para cada equipo. El valor ToXml especifica que los eventos ETW se escribirán en el formato binario a medida que se recibieron, sin conversión a XML. Vea la sección conversión de eventos XML para obtener información acerca de cómo conferir los eventos a XML o no. El patrón de nombre de archivo contiene estas sustituciones: {Computer} para el nombre de equipo y {#3} para el índice de archivo en el anillo.
    >
    > En este archivo de ejemplo, dos equipos de destino se definen con el \<target> elemento. Cada definición especifica la dirección IP con \<ipv4> , pero también puede usar la dirección Mac (por ejemplo, `<mac value=11:22:33:44:55:66/>` o `<mac value=11-22-33-44-55-66/>` ) o el GUID de SMBIOS (por ejemplo, `<guid value={269076F9-4B77-46E1-B03B-CA5003775B88}/>` ) para identificar el equipo de destino. Tenga en cuenta también la clave de cifrado (la misma que se especificó o generó con bcdedit en el equipo de destino) y el nombre del equipo.

4. Escriba los detalles de cada equipo de destino como un \<target> elemento independiente en el archivo de configuración y, a continuación, guarde Newconfig.xml y cierre el Bloc de notas.

5. Aplique la nueva configuración con `$result = (Get-Content .\newconfig.xml | Set-SbecActiveConfig); $result` . La salida debe volver con el campo Success true. Si obtiene otro resultado, consulte la sección solución de problemas de este tema.

Siempre puede comprobar la configuración activa actual con `(Get-SbecActiveConfig).text` .

Puede realizar una comprobación de validez en el archivo de configuración con `$result = (Get-Content .\newconfig.xml | Check-SbecConfig); $result` .

Aunque el comando de Windows PowerShell para aplicar una nueva configuración actualiza automáticamente el servicio sin que sea necesario reiniciarlo, siempre puede reiniciar el servicio con cualquiera de estos comandos:

- Con Windows PowerShell:`Restart-Service BootEventCollector`

- En un símbolo del sistema normal: **SC STOP BootEventCollector; SC Start BootEventCollector**

## <a name="configuring-nano-server-as-a-target-computer"></a>Configuración de nano Server como un equipo de destino

A veces, la interfaz mínima que ofrece nano Server puede dificultar el diagnóstico de problemas con ella. Puede configurar la imagen de nano Server para que participe en el programa de instalación y la recopilación de eventos de arranque automáticamente, enviando datos de diagnóstico a un equipo del recopilador sin necesidad de intervención del usuario. Para ello, realice los pasos siguientes:

### <a name="to-configure-nano-server-as-a-target-computer"></a>Para configurar nano Server como un equipo de destino

1. Cree la imagen básica de nano Server. Consulte [Introducción con nano Server](https://technet.microsoft.com/library/mt126167.aspx) para obtener más información.

2. Configure un equipo del recopilador como en la sección configuración del equipo del recopilador de este tema.

3. Agregue las claves del registro del registrador automático para habilitar el envío de mensajes de diagnóstico. Para ello, debe montar el disco duro virtual de nano Server creado en el paso 1, cargar el subárbol del registro y, a continuación, agregar determinadas claves del registro. En este ejemplo, la imagen de nano Server está en C:\NanoServer; la ruta de acceso podría ser diferente, por lo que debe ajustar los pasos según corresponda.

    1. En el equipo del recopilador, copie el **. Carpeta \Windows\System32\WindowsPowerShell\v1.0\Modules\BootEventCollector** y péguela en el **. \Windows\System32\WindowsPowerShell\v1.0\Modules** en el equipo que está usando para modificar el disco duro virtual de nano Server.

    2. Inicie una consola de Windows PowerShell con permisos elevados y ejecute `Import-Module BootEventCollector` .

    3. Actualice el registro de VHD de nano Server para habilitar los registradores automáticos. Para ello, ejecute `Enable-SbecAutoLogger -Path C:\NanoServer\Workloads\IncludingWorkloads.vhd` . Esto agrega una lista básica de los eventos de instalación e inicio más habituales. puede investigar otros usuarios en el [control de sesiones de seguimiento de eventos](https://msdn.microsoft.com/library/windows/desktop/aa363694(v=vs.85).aspx).

4. Actualice la configuración de BCD en la imagen de nano Server para habilitar la marca de eventos y establecer el equipo del recopilador para asegurarse de que los eventos de diagnóstico se envían al servidor correcto. Tenga en cuenta la dirección IPv4 del equipo del recopilador, el puerto TCP y la clave de cifrado que configuró en el archivo de Active.XML del recopilador (descrito en otro lugar de este tema). Use este comando en una consola de Windows PowerShell con permisos elevados:`Enable-SbecBcd -Path C:\NanoServer\Workloads\IncludingWorkloads.vhd -CollectorIp 192.168.100.1 -CollectorPort 50000 -Key a.b.c.d`

5. Actualice el equipo del recopilador para recibir el evento enviado por el equipo de nano Server agregando el intervalo de direcciones IPv4, la dirección IPv4 específica o la dirección MAC de nano Server al archivo Active.XML en el equipo del recopilador (consulte la sección configuración del equipo del recopilador de este tema).

## <a name="starting-the-event-collector-service"></a>Inicio del servicio Recopilador de eventos

Una vez que se guarda un archivo de configuración válido en el equipo del recopilador y se configura un equipo de destino, tan pronto como se reinicia el equipo de destino, se establece la conexión con el recopilador y se recopilan los eventos.

El registro del servicio Recopilador en sí (que es diferente de los datos de configuración y arranque recopilados por el servicio) se puede encontrar en Microsoft-Windows-BootEvent-Collector/admin. Para una interfaz gráfica para los eventos, use Visor de eventos. Crear una nueva vista; expanda **registros de aplicaciones y servicios**y, a continuación, expanda **Microsoft** y **Windows**. Busque **BootEvent-Collector**, expándalo y busque **admin**.

- Con Windows PowerShell:`Get-WinEvent -LogName Microsoft-Windows-BootEvent-Collector/Admin`

- En un símbolo del sistema normal: **wevtutil qe Microsoft-Windows-BootEvent-Collector/admin**

## <a name="troubleshooting"></a>Solución de problemas

### <a name="troubleshooting-installation-of-the-feature"></a>Solución de problemas de instalación de la característica

| Error | Descripción del error | Síntoma | Posible problema |
|--|--|--|--|
| Dism.exe | 87 | No se reconoce la opción de nombre de característica en este contexto |  Esto puede ocurrir si se escribe mal el nombre de la característica. Compruebe que tiene la ortografía correcta y vuelva a intentarlo. Confirme que esta característica está disponible en la versión del sistema operativo que está usando. En Windows PowerShell, ejecute `dism /online /get-features &#124; ?{$_ -match boot}`. Si no se devuelve ninguna coincidencia, es probable que esté ejecutando una versión que no admita esta característica. |
| Dism.exe | 0x800f080c | La característica `<name>` es desconocida. | Lo mismo que antes. |

### <a name="troubleshooting-the-collector"></a>Solución de problemas del recopilador

**Registro:** El recopilador registra sus propios eventos como proveedor ETW Microsoft-Windows-BootEvent-Collector. Es el primer lugar donde debe buscar la solución de problemas con el recopilador. Puede encontrarlos en Visor de eventos en registros de aplicaciones y servicios > Microsoft > Windows > BootEvent-Collector > admin, o puede leerlos en una ventana de comandos con cualquiera de estos comandos:

En un símbolo del sistema normal: **wevtutil qe Microsoft-Windows-BootEvent-Collector/admin**

En un símbolo del sistema de Windows PowerShell: `Get-WinEvent -LogName Microsoft-Windows-BootEvent-Collector/Admin` (puede anexar `-Oldest` para devolver la lista en orden cronológico con los eventos más antiguos primero)

Puede ajustar el nivel de detalle en los registros de error, a través de advertencia, información (valor predeterminado), detallado y depuración. Los niveles más detallados de la información son útiles para diagnosticar problemas con los equipos de destino que no se conectan, pero pueden generar una gran cantidad de datos, por lo que puede usarlos con cuidado.

Establezca el nivel de registro mínimo en el \<collector> elemento del archivo de configuración. Por ejemplo: <Collector configVersionMajor = 1 minlog \= verbose>.

El nivel detallado registra un registro para cada paquete recibido a medida que se procesa. El nivel de depuración agrega más detalles de procesamiento y vuelca también el contenido de todos los paquetes ETW recibidos.

En el nivel de depuración, puede ser útil escribir el registro en un archivo en lugar de intentar verlo en el sistema de registro habitual. Para ello, agregue un elemento adicional en el \<collector> elemento del archivo de configuración:

<Collector configVersionMajor = 1 minlog =c:\ProgramData\Microsoft\BootEventCollector\Logs\log.txt de registro de depuración \=>

 **Un enfoque sugerido para solucionar problemas del recopilador:**

1. En primer lugar, compruebe que el Recopilador ha recibido la conexión del destino (solo creará el archivo cuando el destino empiece a enviar los mensajes) con
   ```
   Get-SbecForwarding
   ```
   Si devuelve una conexión desde este destino, el problema podría estar en la configuración del registrador automático. Si no devuelve nada, el problema se debe a la conexión KDNET para empezar. Para diagnosticar problemas de conexión de KDNET, pruebe a comprobar la conexión desde ambos extremos (es decir, desde el recopilador y desde el destino).

2. Para ver los diagnósticos extendidos del recopilador, agréguelo al \<collector> elemento del archivo de configuración:\<collector ... minlog=verbose>
   Esto habilitará los mensajes sobre cada paquete recibido.
3. Compruebe si se ha recibido algún paquete. Opcionalmente, puede que desee escribir el registro en modo detallado directamente en un archivo en lugar de usar ETW. Para ello, agregue esto al \<collector> elemento del archivo de configuración:\<collector ... minlog=verbose log=c:\ProgramData\Microsoft\BootEventCollector\Logs\log.txt>

4. Compruebe los registros de eventos en busca de mensajes sobre los paquetes recibidos. Compruebe si se ha recibido algún paquete. Si los paquetes se han recibido pero son incorrectos, consulte los mensajes de eventos para obtener más información.
5. En el lado de destino, KDNET escribe información de diagnóstico en el registro. Busque mensajes en **HKLM\SYSTEM\CurrentControlSet\Services\kdnet** .
   KdInitStatus (DWORD) = 0 si se ejecuta correctamente y muestra un código de error en el error KdInitErrorString = explicación del error (también contiene mensajes informativos si no hay errores)

6. Ejecute Ipconfig.exe en el destino y compruebe el nombre del dispositivo que notifica. Si KDNET se carga correctamente, el nombre del dispositivo debe ser similar a kdnic en lugar del nombre de la tarjeta del proveedor original.
7. Compruebe si DHCP está configurado para el destino. KDNET requiere DHCP.
8. Confirme que el recopilador está en la misma red que el destino. Si no es así, compruebe si el enrutamiento está configurado correctamente, especialmente la configuración de puerta de enlace predeterminada para DHCP.


**Estado de conexión**

Puede consultar la lista actual de conexiones establecidas e información sobre dónde se reenvían los datos `Get-SbecForwarding` .

También puede obtener el historial reciente de los cambios de estado en las conexiones con `Get-SbecHistory` .

### <a name="troubleshooting-setting-a-new-configuration"></a>Solución de problemas de configuración de una nueva configuración
Si aplicó la configuración con el comando de Windows PowerShell `$result = (Get-Content .\newconfig.xml | Set-SbecActiveConfig); $result` , la variable `$result` contendrá información sobre la implementación. Puede consultar esta variable para obtener información diferente de ella:

Obtener información acerca de los errores con `$result.ErrorString` . Si se indica algún error aquí, no se habrá aplicado la nueva configuración y no se modificará la configuración anterior.

Obtener advertencias con `$result.WarningString` .

Obtenga información sobre los detalles de la configuración con `$result.InfoString` .

Puede obtener el resultado completo con `$result | fl *` .
Como alternativa, si no desea guardar el resultado en una variable, puede usar `Get-Content .\newconfig.xml | Set-SbecActiveConfig | fl *` .

### <a name="troubleshooting-target-computers"></a>Solucionar problemas de equipos de destino

| Error | Descripción del error | Posible problema |
|--|--|--|
| Equipo de destino | El destino no se está conectando al recopilador | El equipo de destino no se reinició después de configurarlo. Reinicie el equipo de destino. El equipo de destino tiene una configuración de BCD incorrecta. Compruebe la configuración en la sección validar la configuración del equipo de destino. Corríjalo según sea necesario y, a continuación, reinicie el equipo de destino.<p>El controlador KDNET/EVENT-NET no pudo conectarse a un adaptador de red o está conectado al adaptador de red equivocado. En Windows PowerShell, ejecute `gwmi Win32_NetworkAdapter` y Compruebe la salida de una con ServiceName **kdnic**. Si se ha seleccionado el adaptador de red equivocado, vuelva a realizar los pasos de para especificar un adaptador de red. Si el adaptador de red no aparece en absoluto, podría deberse a que el controlador no es compatible con ninguno de los adaptadores de red.<p>**Vea también:** Un enfoque sugerido para solucionar problemas del recopilador anterior, especialmente los pasos 5 a 8. |
| Recopilador | Ya no veo eventos después de migrar la máquina virtual en la que se hospeda el recopilador. | Compruebe que la dirección IP del equipo del recopilador no ha cambiado. Si es así, revise para habilitar el envío de eventos ETW a través del transporte de forma remota. |
| Recopilador | Los archivos ETL no se crean. | `Get-SbecForwarding`muestra que el destino se ha conectado, sin errores, pero no se han creado los archivos ETL. | Es probable que el equipo de destino no haya enviado todavía ningún dato; Los archivos ETL solo se crean cuando se reciben datos. |
| Recopilador | Un evento no se muestra en el archivo ETL. | El equipo de destino ha enviado un evento, pero cuando el archivo ETL se lee con Visor de eventos del analizador de mensajes, el evento no está presente. El evento aún puede estar en el búfer. Los eventos no se escriben en el archivo ETL hasta que se recopila un búfer de 64 KB completo o un tiempo de espera de aproximadamente 10-15 segundos sin eventos nuevos. Espere a que el tiempo de espera expire o vacíe los búferes con `Save-SbecInstance` .<p>El manifiesto de eventos no está disponible en el equipo del recopilador o en el equipo en el que se ejecuta el analizador de Visor de eventos o de mensajes.  En este caso, es posible que el recopilador no pueda procesar el evento (Compruebe el registro del recopilador) o que el visor no pueda mostrarlo.  Es recomendable que todos los manifiestos estén instalados en el equipo del recopilador e instale las actualizaciones en el equipo del recopilador antes de instalarlos en los equipos de destino. |
