---
title: Introducción a la recopilación de eventos de configuración y arranque
description: Instalación de recopiladores y objetivos de Recopilación de eventos de configuración y arranque
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-sbec
ms.localizationpriority: high
ms.date: 10/16/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247b3f8
author: jaimeo
ms.author: jaimeo
ms.openlocfilehash: c21c6f188a95caac79b461300bcc3d6e318f58d8
ms.sourcegitcommit: 8e2903c9b58646840eedd63b47a9bba6c6a06bf7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "1934416"
---
# <a name="get-started-with-setup-and-boot-event-collection"></a>Introducción a Recopilación de eventos de configuración y arranque

>Se aplica a: Windows Server

  
## <a name="overview"></a>Introducción  
Recopilación de eventos de configuración y arranque es una característica nueva de Windows Server 2016 que permite designar un equipo "recopilador" que puede recopilar una gran variedad de eventos importantes que se producen en otros equipos cuándo arrancan o pasan por el proceso de instalación. Puedes analizar, más adelante, los eventos recopilados con cmdlets de Visor de eventos, Analizador de mensajes, Wevtutil o Windows PowerShell.  
  
Anteriormente, ha sido imposible supervisar estos eventos debido a que la infraestructura necesaria para recopilarlos no existe hasta que un equipo ya está configurado. Los tipos de eventos de configuración y arranque que puedes supervisar incluyen:  
  
-   Carga de módulos de kernel y controladores  
  
-   Enumeración de dispositivos e inicialización de los controladores (incluidos "dispositivos" como tipo de CPU)  
  
-   Verificación y montaje de sistemas de archivos  
  
-   Inicio de archivos ejecutables  
  
-   Inicio y finalización de actualizaciones del sistema  
  
-   Los puntos cuando el sistema está disponible para el inicio de sesión, establece la conexión con un controlador de dominio, se inicia la finalización del servicio y la disponibilidad de recursos compartidos de red  
  
El equipo del recopilador debe ejecutar Windows Server 2016 (puede ser en cualquiera de los servidores con el modo Server Core o Servidor con Experiencia de escritorio). El equipo de destino debe ejecutar Windows 10 o Windows Server 2016. También puede ejecutar este servicio en una máquina virtual alojada en un equipo que **no** ejecute Windows Server 2016. Se conocen las siguientes combinaciones de recopilador virtualizado y equipos de destino para que funcione:  
  
|Host de virtualización|Máquina virtual de recopilador|Máquina virtual de destino|  
|-----------------------|-----------------------------|--------------------------|  
|Windows 8.1|sí|sí|  
|Windows 10|sí|sí|  
|WindowsServer2016|sí|sí|  
|WindowsServer2012R2|sí|no|  
  
## <a name="installing-the-collector-service"></a>Instalación del servicio de recopilador  
A partir de la 2016 de Windows Server, el servicio de recopilador de eventos está disponible como una característica opcional. En esta versión, puedes instalarlo mediante DISM.exe con este comando en un símbolo del sistema de Windows PowerShell con privilegios elevados:  
  
`dism /online /enable-feature /featurename:SetupAndBootEventCollection`  
  
Este comando crea un servicio denominado BootEventCollector y comienza con un archivo de configuración vacío.  
  
Confirma que la instalación se realice correctamente comprobando `get-service -displayname *boot*`. El recopilador de eventos de inicio debe estar ejecutándose. Se ejecuta bajo la cuenta de servicio de red y crea un archivo de configuración vacío (Active.xml) en **%SystemDrive%\ProgramData\Microsoft\BootEventCollector\Config**.  
  
También puedes instalar el servicio de recolección de eventos de arranque y el programa de instalación con el Asistente para agregar roles y características en el Administrador de servidores.  
  
## <a name="configuration"></a>Configuración  
Debes configurar dos elementos para recopilar eventos de instalación y de inicio.  
  
-   En los equipos de destino que enviarán los eventos (es decir, los equipos cuya instalación y arranque deseas supervisar), habilita el transporte KDNET/EVENT-NET y habilita el reenvío de eventos.  
  
-   En el equipo del recopilador, especifica desde qué equipos aceptar eventos y dónde guardarlos.  
  
> [!NOTE]  
> No puedes configurar un equipo para enviar los eventos de inicio o arranque a sí mismo. Pero si deseas supervisar dos equipos, puedes configurarlos para enviar los eventos entre sí.  
  
### <a name="configuring-a-target-computer"></a>Configuración de un equipo de destino  
En cada equipo de destino, en primer lugar habilita el transporte KDNET/EVENT-NET, a continuación, habilita el envío de eventos ETW a través del transporte y, a continuación, reinicia el equipo de destino. EVENT-NET es un protocolo de transporte en kernel similar a KDNET (protocolo de depurador de kernel). EVENT-NET solo transmite eventos y no permite el acceso al depurador. Estos dos protocolos son mutuamente excluyentes; solo se puede habilitar uno de ellos a la vez.  
  
Puedes habilitar el transporte de eventos remotamente (con Windows PowerShell) o localmente.  
  
##### <a name="to-enable-event-transport-remotely"></a>Para habilitar el transporte de eventos de forma remota  
  
1.  Si ya has configurado la comunicación remota de Windows PowerShell en el equipo de destino, ve al paso 3. Si no es así, en el equipo de destino, abre un símbolo del sistema y ejecuta el siguiente comando:  
  
    winrm quickconfig  
  
2.  Responde a las preguntas siguientes y, a continuación, reinicia el equipo de destino. Si los equipos de destino no están en el mismo dominio que el equipo del recopilador, es posible que tengas que definirlos como hosts de confianza. Para ello:  
  
3.  En el equipo del recopilador, ejecuta uno de estos comandos:  
  
    -   En un símbolo del sistema de Windows PowerShell: `Set-Item -Force WSMan:\localhost\Client\TrustedHosts "<target1>,<target2>,..."`, seguido por `Set-Item -Force WSMan:\localhost\Client\AllowUnencrypted true` donde \ <target1>, etc. son los nombres o direcciones IP de los equipos de destino.  
  
    -   O en un símbolo del sistema: **winrm set winrm/config/client @{TrustedHosts="\<target1>,\<target2>,...";AllowUnencrypted="true"}}**  
  
        > [!IMPORTANT]  
        > Esto establece la comunicación sin cifrar; por tanto, no lo hagas fuera de un entorno de laboratorio.  
  
4.  Prueba la conexión remota yendo al equipo del recopilador y ejecutando uno de estos comandos de Windows PowerShell:  
  
    Si el equipo de destino se encuentra en el mismo dominio que el equipo del recopilador, ejecuta `New-PSSession -Computer <target> | Remove-PSSession`  
  
    Si el equipo de destino no está en el mismo dominio, ejecuta `New-PSSession -Computer  <target>  -Credential Administrator | Remove-PSSession`, que te pedirá las credenciales.  
  
    Si el comando no devuelve nada, la comunicación remota fue correcta.  
  
5.  En el equipo de destino, abre un símbolo del sistema de Windows PowerShell con privilegios elevados y ejecuta este comando:  
  
    `Enable-SbecBcd -ComputerName <target_name> -CollectorIP <ip> -CollectorPort <port> -Key <a.b.c.d>`  
  
    Aquí <target_name> es el nombre del equipo de destino, \<ip> es la dirección IP del equipo del recopilador. \<port> es el número de puerto donde se ejecutará el recopilador. La clave <a.b.c.d> es una clave de cifrado requerida para la comunicación e incluye cuatro cadenas alfanuméricas separadas por puntos. Esta misma clave se usa en el equipo del recopilador. Si no especificas una clave, el sistema genera una clave aleatoria; la necesitarás para el equipo del recopilador, por lo que deberás anotarla.  
  
6.  Si ya tienes configurado un equipo de recopilador, actualiza el archivo de configuración en el equipo del recopilador con la información del nuevo equipo de destino. Consulta la sección "Configurar el equipo del recopilador" para obtener información detallada.  
  
##### <a name="to-enable-event-transport-locally-on-the-target-computer"></a>Para habilitar el transporte de eventos localmente en el equipo de destino  
  
1.  Inicia un símbolo del sistema con privilegios elevados y, a continuación, ejecuta estos comandos:  
  
    **bcdedit /event yes**  
  
    **bcdedit /eventsettings net hostip:1.2.3.4 port:50000 key:a.b.c.d**  
  
    Aquí "1.2.3.4" es un ejemplo; sustitúyelo por la dirección IP del equipo del recopilador. Sustituye también "50000" por el número de puerto donde se ejecutará el recolector y "a.b.c.d" por la clave de cifrado necesaria para la comunicación. Esta misma clave se usa en el equipo del recopilador. Si no especificas una clave, el sistema genera una clave aleatoria; que necesitarás para el equipo del recopilador, por lo que tendrás que anotarla.  
  
2.  Si ya has configurado un equipo de recopilador, actualiza el archivo de configuración en el equipo del recopilador con la información del nuevo equipo de destino. Consulta la sección "Configuración del equipo del recopilador" para obtener información detallada.  
  
**Ahora que se ha habilitado el transporte de eventos, debes habilitar el sistema para enviar realmente eventos ETW a través de ese transporte.**  
  
##### <a name="to-enable-sending-of-etw-events-through-the-transport-remotely"></a>Para habilitar el envío de eventos ETW a través del transporte de forma remota  
  
1.  En el equipo del recopilador, abre un símbolo del sistema de Windows PowerShell con privilegios elevados.  
  
2.  Ejecuta `Enable-SbecAutologger -ComputerName <target_name>`, donde <target_name> es el nombre del equipo de destino.  
  
Si no puedes configurar la interacción remota de Windows PowerShell, siempre se puedes habilitar el envío de eventos directamente en el equipo de destino.  
  
##### <a name="to-enable-sending-of-etw-events-through-the-transport-locally"></a>Para habilitar el envío de eventos ETW a través del transporte localmente  
  
1.  En el equipo de destino, inicia Regedit.exe y busca esta clave del registro:  
  
    **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\WMI\AutoLogger**. Varias sesiones de registro aparecen como subclaves bajo esta clave. **Plataforma de instalación**, **Registrador del kernel de NT**e **Instalación de Microsoft Windows** son posibles opciones para su uso con Recopilación de eventos de configuración y arranque, pero la opción recomendada es **Sistema de registro de eventos**. Estas claves se detallan en [Configuración e inicio de una sesión del registrador automático](https://msdn.microsoft.com/library/windows/desktop/aa363687(v=vs.85).aspx).  
  
2.  En la clave del Sistema del registro de eventos, cambia el valor de **LogFileMode** de **0x10000180** a **0x10080180**. Para obtener más información acerca de los detalles de estas configuraciones, consulta [Constantes de modo de registro](https://msdn.microsoft.com/library/windows/desktop/aa364080(v=vs.85).aspx).  
  
3.  De forma opcional, también puedes habilitar el reenvío de datos de comprobación de errores para el equipo del recopilador. Para ello, busca el registro de clave HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager y crea la clave **Filtro de impresión de depuración** con un valor de **0 x 1**.  
  
4.  Reinicie el equipo de destino.  
  
### <a name="choosing-the-network-adapter"></a>Elección del adaptador de red  
Si el equipo de destino tiene más de un adaptador de red, el controlador KDNET elegirá el primero admitido que aparezca. Puedes especificar un adaptador de red determinado que se usará para reenviar eventos de instalación con estos pasos:  
  
##### <a name="to-specify-a-network-adapter"></a>Para especificar un adaptador de red  
  
1.  En el equipo de destino, abre el Administrador de dispositivos, expande **Adaptadores de red**, busca el adaptador de red que deseas usar y haz clic en él.  
  
2.  En el menú que se abre, haz clic en **Propiedades**y, a continuación, haz clic en la pestaña **Detalles**. Expande el menú en el campo **Propiedad**, desplázate hasta encontrar **Información de ubicación** (la lista probablemente no esté en orden alfabético) y, a continuación, haz clic en él. El valor será una cadena con el formato **bus PCI X, dispositivo Y, función Z**. Anota X.Y.Z; estos son los parámetros de bus que necesitas para el siguiente comando.  
  
3.  Ejecuta uno de estos comandos:  
  
    Desde el símbolo de Windows PowerShell con privilegios elevados: `Enable-SbecBcd -ComputerName <target_name> -CollectorIP <ip> -CollectorPort <port> -Key <a.b.c.d> -BusParams <X.Y.Z>`  
  
    Desde un símbolo del sistema con privilegios elevados: **bcdedit /eventsettings  net hostip:aaa port:50000 key:bbb busparams:X.Y.Z**  
  
### <a name="validate-target-computer-configuration"></a>Validar la configuración del equipo de destino  
Para comprobar la configuración en el equipo de destino, abre un símbolo del sistema con privilegios elevados y ejecuta **bcdedit/enum**. Cuando finalice, ejecuta **bcdedit /eventsettings**. Puedes comprobar los siguientes valores:  
  
-   Key  
  
-   Debugtype = NET  
  
-   Hostip = \<dirección IP del recopilador>  
  
-   Port = \ <número de puerto especificado para que lo use el recopilador>  
  
-   DHCP = yes  
  
Comprueba además que has habilitado **bcdedit /event**, ya que **/debug** y **/event** son mutuamente excluyentes. Solo puedes ejecutar uno u otro. De forma similar, no puedes mezclar /eventsettings con /debug o /dbgsettings con /event.  
  
Ten en cuenta también que la recopilación de eventos no funciona si se establece en un puerto serie.  
  
### <a name="configuring-the-collector-computer"></a>Configuración del equipo del recopilador  
El servicio de recopilador recibe los eventos y los guarda en archivos ETL. Estos archivos ETL pueden leerse con otras herramientas, como cmdlets del Visor de eventos, Analizador de mensajes Wevtutil y Windows PowerShell.  
  
Dado que el formato ETW no permite especificar el nombre de equipo de destino, los eventos de cada equipo de destino deben guardarse en un archivo independiente. Las herramientas de visualización pueden mostrar un nombre de equipo, pero será el nombre del equipo en el que se ejecuta la herramienta.  
  
Más exactamente, a cada equipo de destino se le asigna un anillo de archivos ETL. Cada nombre de archivo incluye un índice de 000 hasta un valor máximo que configure (hasta 999). Cuando el archivo alcanza el tamaño máximo configurado, pasa los eventos de escritura al archivo siguiente. Después del archivo más alto posible vuelve al índice de archivos 000. De esta forma, los archivos se reciclan automáticamente, limitando el uso de espacio en disco. También puedes establecer directivas de retención externas adicionales para limitar aún más el uso del disco. Por ejemplo, puedes eliminar archivos con una antigüedad de un número determinado de días.  
  
Los archivos ETL recopilados normalmente se guardan en el directorio **c:\ProgramData\Microsoft\BootEventCollector\Etl** (que podría tener subdirectorios adicionales). Puede encontrar el archivo de registro más reciente ordenándolos por la hora de la última modificación. También hay un registro de estado (normalmente en **c:\ProgramData\Microsoft\BootEventCollector\Logs**), que registra cada vez que el recolector de cambia la escritura a un nuevo archivo.  
  
También hay un registro de recopilador, que registra la información sobre el propio recopilador. Puedes conservar este registro con el formato ETW (en el que notifican los eventos para el servicio de registro de Windows; este es el valor predeterminado) o en un archivo (normalmente en **c:\ProgramData\Microsoft\BootEventCollector\Logs**). Usar un archivo puede resultar útil si deseas habilitar los modos detallados que producen una gran cantidad de datos. También puedes establecer el registro para escribir en una salida estándar ejecutando el recopilador desde la línea de comandos.  
  
**Creación del archivo de configuración del recopilador**  
  
Cuando habilites el servicio, se crean tres archivos de configuración XML y se almacenan en **c:\ProgramData\Microsoft\ BootEventCollector\Config**:  
  
-   **Active.XML** Este archivo contiene la configuración activa del servicio del recopilador.  Tras la instalación, este archivo tiene el mismo contenido que Empty.xml. Cuando estableces la configuración del nuevo recopilador, lo guardas en este archivo.  
  
-   **Empty.XML** Este archivo contiene los elementos de configuración mínimos necesarios con sus valores predeterminados definidos. No habilita ninguna colección; sólo permite que el servicio del recopilador se inicie en un modo de inactividad.  
  
-   **Example.xml** Este archivo proporciona ejemplos y explicaciones de los elementos de configuración posibles.  
  
**Elección de un límite de tamaño de archivo**  
  
Una de las decisiones que tendrás que tomar es establecer un límite de tamaño de archivo. El mejor límite de tamaño de archivo depende del volumen de eventos esperado y el espacio en disco disponible. Los archivos de menor tamaño son más convenientes desde el punto de vista de la limpieza de datos antiguos. Sin embargo, cada archivo conlleva la sobrecarga de un encabezado de 64 KB y leer muchos archivos para obtener el historial combinado podría resultar poco práctico. El límite de tamaño de archivo mínimo absoluto es 256 KB. Un límite de tamaño de archivo práctico razonable debe ser más de 1 MB, y 10 MB probablemente es un buen valor típico. Es posible un límite superior razonable si se prevé muchos eventos.  
  
Hay varios detalles que deben tenerse en cuenta con respecto del archivo de configuración:  
  
-   La dirección del equipo de destino. Puedes usar su dirección IPv4, una dirección MAC o un GUID de SMBIOS. Ten estos factores en cuenta al elegir la dirección que usar:  
  
    -   La dirección IPv4 funciona mejor con una asignación estática de las direcciones IP. Sin embargo, las direcciones IP estáticas deben estar disponibles incluso a través de DHCP.  
  
    -   Una dirección MAC o el GUID de SMBIOS es conveniente cuando se conocen de antemano, pero las direcciones IP se asignan de forma dinámica.  
  
    -   El protocolo EVENT-NET no admiten direcciones IPv6.  
  
    -   Es posible especificar varias opciones para identificar el equipo. Por ejemplo, si el hardware físico está a punto de reemplazarse, puedes escribir las direcciones MAC nueva y antigua y cualquiera se aceptará.  
  
-   La clave de cifrado utilizada para la comunicación con el equipo del recopilador  
  
-   El nombre del equipo de destino. Puedes usar la dirección IP, nombre de host o cualquier otro nombre como el nombre del equipo.  
  
-   El nombre del archivo ETL que usar y la configuración de tamaño de anillo para él  
  
##### <a name="to-create-the-configuration-file"></a>Para crear el archivo de configuración  
  
1.  Abre un símbolo del sistema de Windows PowerShell con privilegios elevados y cambia al directorio % SystemDrive%\ProgramData\Microsoft\BootEventCollector\Config.  
  
2.  Escribe `notepad .\newconfig.xml` y presiona INTRO.  
  
3.  Copia este ejemplo de configuración en la ventana del Bloc de notas:  
  
    ```  
    <collector configVersionMajor="1" statuslog="c:\ProgramData\Microsoft\BootEventCollector\Logs\statuslog.xml">  
      <common>  
        <collectorport value="50000"/>  
        <forwarder type="etl">  
          <set name="file" value="c:\ProgramData\Microsoft\BootEventCollector\Etl\{computer}\{computer}_{#3}.etl"/>  
          <set name="size" value="10mb"/>  
          <set name="nfiles" value="10"/>  
          <set name="toxml" value="none"/>  
        </forwarder>  
        <target>  
          <ipv4 value="192.168.1.1"/>  
          <key value="a.b.c.d"/>  
          <computer value="computer1"/>  
        </target>  
        <target>  
          <ipv4 value="192.168.1.2"/>  
          <key value="d1.e2.f3.g4"/>  
          <computer value="computer2"/>  
        </target>  
      </common>  
    </collector>  
    ```  
  
    > [!NOTE]  
    > El nodo raíz es \<collector>. Sus atributos especifican la versión de la sintaxis del archivo de configuración y el nombre del archivo de registro de estado.  
    >   
    > El elemento \<common> agrupa varios destinos que especifican los elementos de configuración comunes para ellos, muy similar al grupo de usuarios que se pueden usar para especificar los permisos comunes para varios usuarios.  
    >   
    > El elemento \<collectorport> define el número de puerto UDP donde escuchará el recopilador de datos entrantes. Este es el mismo puerto tal y como se especificó en el paso de configuración de destino de Bcdedit. El recolector es compatible con un solo puerto y todos los destinos deben conectarse al mismo puerto.  
    >   
    > El elemento \<forwarder> especifica cómo se van recibir los eventos ETW recibidos desde los equipos de destino. No hay un único tipo de reenviador, que los escribe en los archivos ETL. Los parámetros especifican el patrón de nombre de archivo, el límite de tamaño de cada archivo en el anillo y el tamaño del anillo de cada equipo. El ajuste "toxml" especifica que los eventos ETW se escribirán en el formato binario tal y como se recibieron, sin conversión a XML. Consulta la sección "Conversión de eventos XML" para obtener información acerca de cómo decidir si se convierten los eventos a XML o no. El patrón de nombre de archivo contiene las siguientes sustituciones: {computer} para el nombre del equipo y {3 #} para el índice del archivo en el anillo.  
    >   
    > En este archivo de ejemplo, se definen dos equipos de destino con el elemento \<target>. Cada definición especifica la dirección IP con \<ipv4>, pero también podría utilizar la dirección MAC (por ejemplo, <mac value="11:22:33:44:55:66"\/> o <mac value="11-22-33-44-55-66"\/>) o el GUID de SMBIOS (por ejemplo, <guid value="{269076F9-4B77-46E1-B03B-CA5003775B88}"\/>) para identificar el equipo de destino. Tenga en cuenta también la clave de cifrado (igual que se ha especificado o generado con Bcdedit en el equipo de destino) y el nombre del equipo.  
  
4.  Escribe los detalles de cada equipo de destino como un elemento \<target> independiente en el archivo de configuración y, a continuación, guarda Newconfig.xml y cierra el Bloc de notas.  
  
5.  Aplica la nueva configuración con `$result = (Get-Content .\newconfig.xml | Set-SbecActiveConfig); $result`. Debe devolver el resultado con el campo de éxito "true". Si obtienes otro resultado, consulta la sección de solución de problemas de este tema.  
  
Siempre puedes comprobar la configuración activa actual con `(Get-SbecActiveConfig).text`.  
  
Puedes realizar una comprobación de validez en el archivo de configuración con `$result = (Get-Content .\newconfig.xml | Check-SbecConfig); $result`.  
  
Aunque el comando de Windows PowerShell para aplicar automáticamente una nueva configuración actualiza el servicio sin que sea necesario reiniciarlo, siempre puedes reiniciar el servicio con cualquiera de estos comandos:  
  
-   Con Windows PowerShell: `Restart-Service BootEventCollector`  
  
-   En un símbolo del sistema normal: **sc stop BootEventCollector; sc start BootEventCollector**  
  
## <a name="configuring-nano-server-as-a-target-computer"></a>Configuración de Nano Server como un equipo de destino  
La interfaz mínima ofrecida por Nano Server puede a veces dificultar el diagnóstico de problemas con él. Puede configurar tu imagen de Nano Server para participar en Recopilación de eventos de configuración y arranque automáticamente, enviar datos de diagnóstico a un equipo de recopilador sin la intervención ninguna otra intervención por tu parte. Para ello, sigue estos pasos:  
  
#### <a name="to-configure-nano-server-as-a-target-computer"></a>Para configurar Nano Server como un equipo de destino  
  
1.  Crea la imagen de Nano Server básica. Para obtener información detallada, consulta [Introducción a Nano Server](https://technet.microsoft.com/library/mt126167.aspx) .  
  
2.  Configura un equipo del recopilador, como se muestra en la sección "Configuración del equipo del recopilador" de este tema.  
  
3.  Agrega las claves de registro del Registrador automático para habilitar el envío de mensajes de diagnóstico. Para ello, monta el disco duro virtual Nano Server creado en el paso 1, carga el subárbol del registro y, a continuación, agrega ciertas claves del registro. En este ejemplo, la imagen de Nano Server está en C:\NanoServer; la ruta de acceso podría ser diferente, por tanto ajusta los pasos en consecuencia.  
  
    1.  En el equipo del recopilador, copia la carpeta **.. \Windows\System32\WindowsPowerShell\v1.0\Modules\BootEventCollector** y pégala en el directorio **.. \Windows\System32\WindowsPowerShell\v1.0\Modules** en el equipo que se va a usar para modificar el VHD de servidor Nano.  
  
    2.  Inicia una consola de Windows PowerShell con permisos elevados y ejecuta `Import-Module BootEventCollector`.  
  
    3.  Actualiza el registro de VHD Nano Server para habilitar Registradores automáticos. Para ello, ejecuta `Enable-SbecAutoLogger -Path C:\NanoServer\Workloads\IncludingWorkloads.vhd`. Esto agrega una lista básica de la configuración y eventos de arranque más habituales; puedes investigar otros en [Control de sesiones de seguimiento de eventos](https://msdn.microsoft.com/library/windows/desktop/aa363694(v=vs.85).aspx).  
  
4.  Actualiza la configuración de BCD en la imagen de Nano Server para habilitar la marca de eventos y configurar el equipo del recopilador para asegurarse de que los eventos de diagnóstico se envían al servidor correcto. Ten en cuenta la dirección IPv4 del equipo del recopilador, el puerto TCP y la clave de cifrado configurada en el archivo Active.XML del recopilador (se describe en otro sitio de este tema). Usa este comando en una consola de Windows PowerShell con permisos elevados: `Enable-SbecBcd -Path C:\NanoServer\Workloads\IncludingWorkloads.vhd -CollectorIp 192.168.100.1 -CollectorPort 50000 -Key a.b.c.d`  
  
5.  Actualiza el equipo del recopilador para recibir eventos enviados por el equipo de Nano Server agregando el intervalo de direcciones IPv4, la dirección IPv4 específica o la dirección MAC de Nano Server en el archivo Active.XML del equipo del recopilador (consulta la sección "Configuración del equipo del recopilador" de este tema).  
  
## <a name="starting-the-event-collector-service"></a>Inicio del servicio del recopilador de eventos  
Una vez que se guarda un archivo de configuración válido en el equipo del recopilador y se configura un equipo de destino, tan pronto como se reinicie el equipo de destino, se realiza la conexión al selector y se recopilarán los eventos.  
  
El propio registro del servicio del recopilador (que es distinto del programa de los datos de instalación y arranque por el servicio) se pueden encontrar en Microsoft-Windows-BootEvent-Collector/Admin. Para obtener una interfaz gráfica de los eventos, utiliza el Visor de eventos. Crea una nueva vista; expande **Registros de aplicaciones y servicios**, a continuación, expande **Microsoft** y luego **Windows**. Busca **BootEvent-Collector**, expándelo y busca **Admin**.  

-   Con Windows PowerShell: `Get-WinEvent -LogName Microsoft-Windows-BootEvent-Collector/Admin`  
  
-   En un símbolo del sistema normal: **wevtutil qe Microsoft-Windows-BootEvent-Collector/Admin**  
  
## <a name="troubleshooting"></a>Solucionar problemas  
  
### <a name="troubleshooting-installation-of-the-feature"></a>Solución de problemas de instalación de la característica  
  
||Error|Descripción del error|Síntoma|Posible problema|  
|-|---------|---------------------|-----------|---------------------|  
|Dism.exe|87|La opción nombre-característica no se reconoce en este contexto||- Esto puede ocurrir si escribes incorrectamente el nombre de la característica. Comprueba que usas la ortografía correcta e inténtela de nuevo.<br />- Confirma que esta característica está disponible en la versión de sistema operativo que estás utilizando. En Windows PowerShell, ejecuta **dism /online /get-features &#124; ?{$_ -match "boot"}**. Si no se devuelve ninguna coincidencia, probablemente está ejecutando una versión que no es compatible con esta característica.|  
|Dism.exe|0x800f080c|Característica \<name> desconocida.||Igual que arriba|  
  
### <a name="troubleshooting-the-collector"></a>Solución de problemas del recopilador  
  
**Registro:**  
El recopilador registra sus propios eventos como proveedor de ETW en Microsoft-Windows-BootEvent-Collector. Es el primer lugar en el que debes buscar para solucionar problemas con el recopilador. Puedes encontrarlos en el Visor de eventos en Registros de aplicaciones y servicios > Microsoft > Windows > BootEvent-Collector > Admin, o puedes leerlos en una ventana de comandos con uno de estos comandos:  
  
En un símbolo del sistema normal: **wevtutil qe Microsoft-Windows-BootEvent-Collector/Admin**  
  
En un símbolo del sistema de Windows PowerShell: `Get-WinEvent -LogName Microsoft-Windows-BootEvent-Collector/Admin` (puedes anexar `-Oldest` para devolver la lista en orden cronológico con los eventos más antiguos en primer lugar)  
  
Puede ajustar el nivel de detalle en los registros desde "error" hasta "warning", "info" (valor predeterminado), "verbose" y "debug". Niveles más detallados que "info" resultan útiles para diagnosticar problemas con equipos de destino que no se conectan, pero podrían generar una gran cantidad de datos, por lo que debes usarlos con cuidado.  
  
Establece el registro mínimo nivel en el elemento \<collector> del archivo de configuración. Por ejemplo: <collector configVersionMajor="1" minlog\="verbose">.  
  
El nivel "verbose" inserta un registro por cada paquete recibido a medida que se procesa. El nivel "debug" agrega más detalle de procesamiento y vuelca también el contenido de todos los paquetes ETW recibidos.  
  
En el nivel "debug", podría ser útil escribir el registro en un archivo, en lugar de intentar verlo en el sistema de registro habitual. Para ello, agrega un elemento adicional en el elemento \<collector> del archivo de configuración:  
  
<collector configVersionMajor="1" minlog="debug" log\="c:\ProgramData\Microsoft\BootEventCollector\Logs\log.txt">  
      
 **Un enfoque sugerido para la solución de problemas del recopilador:**  
   
 1. En primer lugar, comprueba que el recopilador ha recibido la conexión desde el destino (creará el archivo solo cuando el destino empiece a enviar los mensajes) con   
```  
Get-SbecForwarding  
```  
Si devuelve que hay una conexión desde este destino, el problema podría estar en la configuración del registrador automático. Si no devuelve nada, el problema está en la conexión de KDNET con la que comenzar. Para diagnosticar problemas de conexión de KDNET, intenta comprobar la conexión desde ambos extremos (es decir, desde el recolector y desde el destino).  
  
2. Para ver los diagnósticos extendidos desde el recopilador, agrega esto al elemento \<collector>del archivo de configuración:  
\<collector ... minlog="verbose">  
Esto permitirá mensajes sobre todos los paquetes recibidos.  
3. Comprueba si se reciben todos los paquetes. De forma opcional, es posible que desees escribir el registro en modo detallado directamente en un archivo en lugar de hacerlo a través de ETW. Para ello, agrega lo siguiente al elemento \<collector> del archivo de configuración:  
\<collector ... minlog="verbose" log="c:\ProgramData\Microsoft\BootEventCollector\Logs\log.txt">  
      
4. Comprueba los registros de eventos de todos los mensajes sobre los paquetes recibidos. Comprueba si se reciben todos los paquetes. Si se reciben los paquetes pero de forma incorrecta, comprueba los mensajes de eventos para obtener información detallada.  
5. Desde el lado de destino, KDNET escribe información de diagnóstico en el registro. Busca en   
**HKLM\SYSTEM\CurrentControlSet\Services\kdnet** para los mensajes.  
  KdInitStatus (DWORD) will = 0 en caso de éxito y muestra un código de error en el error  
  KdInitErrorString = explicación del error (también contiene mensajes informativos si no hay error)  
  
6. Ejecuta Ipconfig.exe en el destino y comprueba el nombre de dispositivo que notifica. Si KDNET se carga correctamente, el nombre del dispositivo debe ser similar a "kdnic" en lugar del nombre de la tarjeta del proveedor original.  
7. Comprueba si DHCP se ha configurado para el destino. KDNET absolutamente requiere DHCP.  
8. Confirma que el recopilador está en la misma red que el destino. Si no, comprueba si está configurado correctamente el enrutamiento, especialmente la configuración de la puerta de enlace predeterminada de DHCP.  
  
  
**Estado de conexión**  
  
Puedes comprobar la lista actual de conexiones establecidas e información sobre dónde se están desviando los datos con `Get-SbecForwarding`.  
  
También puedes obtener el historial reciente de cambios de estado en conexiones con `Get-SbecHistory`.  
  
### <a name="troubleshooting-setting-a-new-configuration"></a>Solución de problemas de configuración de una nueva configuración  
Si has aplicado la configuración con el comando de Windows PowerShell `$result = (Get-Content .\newconfig.xml | Set-SbecActiveConfig); $result`, la variable `$result` contendrá información sobre la implementación. Puede consultar esta variable para obtener información diferente fuera de ella:  
  
Obtén información acerca de los errores con `$result.ErrorString`. Si se notifican errores aquí, la nueva configuración no se habrá aplicado y la configuración anterior no se modificará.  
  
Obtén advertencias con `$result.WarningString`.  
  
Obtén información acerca de los detalles de la configuración con `$result.InfoString`.  
  
Puedes obtener el resultado completo con `$result | fl *`.  
Como alternativa, si no deseas guardar el resultado en una variable, puedes utilizar `Get-Content .\newconfig.xml | Set-SbecActiveConfig | fl *`.  
  
### <a name="troubleshooting-target-computers"></a>Solución de problemas de los equipos de destino  
  
||Error|Descripción del error|Síntoma|Posible problema|  
|-|---------|---------------------|-----------|---------------------|  
|Equipo de destino||El destino no se conecta al recopilador||- El equipo de destino no se reinició después de su configuración. Reinicia el equipo de destino.<br />- El equipo de destino tiene una configuración de BCD incorrecta. Comprueba la configuración en la sección "Validar configuración del equipo de destino". Corrige según sea necesario y, a continuación, reinicia el equipo de destino.<br />- El controlador KDNET/EVENT-NET no podía conectarse a un adaptador de red o estaba conectado al adaptador de red incorrecto. En Windows PowerShell, ejecuta `gwmi Win32_NetworkAdapter` y comprueba la salida de uno con el ServiceName **kdnic**. Si se selecciona el adaptador de red incorrecto, vuelve a realizar los pasos descritos en "Para especificar un adaptador de red". Si el adaptador de red no aparece en absoluto, podría ser que el controlador no admita ninguno de los adaptadores de red.<br>**Consulta también** "Un enfoque sugerido para la solución de problemas del recopilador" más arriba, especialmente los pasos del 5 al 8.|  
|Recopilador||Ya no veo eventos después de migrar la VM en la que se hospeda mi recopilador.||Comprueba que no haya cambiado la dirección IP del equipo del recopilador. Si ha cambiado, revisa "Para habilitar el envío de eventos ETW a través del transporte de forma remota".|  
|Recopilador||No se crean los archivos ETL.|`Get-SbecForwarding` muestra que el destino se ha conectado, sin errores, pero no se crean los archivos ETL.|El equipo de destino probablemente no ha enviado ningún dato aún; los archivos ETL solo se crean cuando se reciben datos.|  
|Recopilador||Un evento no se muestra en el archivo ETL.|El equipo de destino ha enviado un evento, pero cuando se lee el archivo ETL con el visor de eventos del analizador de mensajes, el evento no está presente.|- El evento aún podría estar en el búfer. Los eventos no se escriben en el archivo ETL hasta que se recopila un búfer completo de 64 KB o se ha producido un tiempo de espera de 10 a 15 segundos sin eventos nuevos. Espere a que el tiempo de espera caduque o vacíe los búferes con `Save-SbecInstance`.<br />- El manifiesto de eventos no está disponible en el equipo del recopilador o el equipo en el que se ejecuta el Visor de eventos o el analizador de mensajes.  En este caso, es posible que el recolector no pueda procesar el evento (comprueba el registro de recopilador) o el Visor podría no ser capaz de mostrarlo.  Es una práctica recomendada tener todos los manifiestos instalados en el equipo del recopilador e instalar las actualizaciones en el equipo del recopilador antes de instalarlos en los equipos de destino.|