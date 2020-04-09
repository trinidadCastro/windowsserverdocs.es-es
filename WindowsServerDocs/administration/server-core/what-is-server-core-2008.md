---
title: ¿Qué es Server Core 2008?
description: Obtener información acerca de la opción de instalación Server Core en Windows Server 2008
ms.prod: windows-server
ms.author: helohr
ms.date: 11/01/2017
ms.topic: article
author: heidilohr
ms.openlocfilehash: 13167dad848ac31827c42045360e45c76718207a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851598"
---
# <a name="what-is-server-core-2008"></a>¿Qué es Server Core 2008?
>Se aplica a: Windows Server 2008

>[!NOTE]
>Esta información se aplica a Windows Server 2008. Para obtener información acerca de Server Core en Windows Server, consulte [¿Qué es la instalación Server Core en Windows Server](https://docs.microsoft.com/windows-server/administration/server-core/what-is-server-core)?. 

La opción Server Core es una nueva opción de instalación mínima que está disponible cuando se implementa la edición Standard, Enterprise o Datacenter de Windows Server 2008. Server Core proporciona una instalación mínima de Windows Server 2008 que admite la instalación de solo determinados roles de servidor, como se describe más adelante en este capítulo. Compare esto con la opción de instalación completa de Windows Server 2008, que admite la instalación de todos los roles de servidor disponibles y otras aplicaciones de servidor de Microsoft o de terceros, como Microsoft Exchange Server o SAP. 

Antes de continuar, debe explicarse la frase "opción de instalación". Normalmente, cuando se adquiere una copia de Windows Server 2008, se adquiere una licencia para usar determinadas ediciones o unidades de retención de existencias (SKU). En la tabla 1-1 se enumeran las distintas ediciones de Windows Server 2008 que están disponibles. La tabla también indica qué opciones de instalación (Full, Server Core o ambos) están disponibles para cada edición.

**Tabla 1-1** Ediciones de Windows Server 2008 y su compatibilidad con las opciones de instalación

| Edición       | Full          | Server Core  |
| ------------- | :-------------: | :------------: |
| Windows Server 2008 Standard (x86 y x64)       | X | X        |
| Windows Server 2008 Enterprise (x86 y x64)       | X | X        |
| Windows Server 2008 Datacenter (x86 y x64)        | X | X       |
| Windows Web Server 2008 (x86 y x64)       | X | X  |
| Windows Server 2008 para sistemas basados en Itanium       | X |     |
| Windows HPC Server 2008 (solo x64)       | X |   |
| Windows Server 2008 Standard sin Hyper-V (x86 y x64) | X | X |
| Windows Server 2008 Enterprise sin Hyper-V (x86 y x64)  | X | X |
| Windows Server 2008 Standard sin Hyper-V (x86 y x64) | X | X |

Para comprender qué es una "opción de instalación", supongamos que ha adquirido una licencia por volumen que le permite instalar una copia de Windows Server 2008 Enterprise Edition. Cuando inserte los medios con licencias por volumen en un sistema e inicie el proceso de instalación, una de las pantallas que verá, como se muestra en la figura 1-1, le presentará una serie de opciones de instalación y ediciones.

![Selección de una opción de instalación de Server Core para instalar](../media/what-is-server-core-2008/FIg1-1.png)

**Figura 1-1** Selección de una opción de instalación de Server Core para instalar

En la figura 1-1, su licencia por volumen (o clave de producto, para medios comerciales) ofrece dos opciones de instalación entre las que puede elegir: la segunda opción (una instalación completa de Windows Server 2008 Enterprise) y la quinta opción (una instalación Server Core de Windows Server 2008 Enterprise) con la última seleccionada en este ejemplo. 

## <a name="full-vs-server-core"></a>Completo frente a Server Core 
Desde los primeros días de la plataforma de Microsoft Windows, los servidores de Windows eran esencialmente "todos" servidores que incluían todo tipo de características, algunas de las cuales es posible que nunca use en su entorno de red. Por ejemplo, al instalar Windows Server 2003 en un sistema, los archivos binarios para el servicio de enrutamiento y acceso remoto (RRAS) se instalaron en el servidor aunque no tuviera necesidad de este servicio (aunque todavía tenía que configurar y habilitar RRAS antes de funcionar). Windows Server 2008 mejora las versiones anteriores mediante la instalación de los archivos binarios necesarios para un rol de servidor solo si decide instalar ese rol concreto en el servidor. Sin embargo, la opción de instalación completa de Windows Server 2008 todavía instala muchos servicios y otros componentes que no suelen ser necesarios para un escenario de uso determinado. 

Este es el motivo por el que Microsoft creó una segunda opción de instalación (Server Core) para Windows Server 2008: para eliminar los servicios y otras características que no son esenciales para la compatibilidad de determinados roles de servidor que se usan habitualmente. Por ejemplo, un servidor de sistema de nombres de dominio (DNS) realmente no necesita tener instalado Windows Internet Explorer porque no desearía explorar la web desde un servidor DNS por motivos de seguridad. Además, un servidor DNS no necesita una interfaz gráfica de usuario (GUI), ya que puede administrar prácticamente todos los aspectos de DNS desde la línea de comandos mediante el poderoso comando dnscmd. exe o de forma remota con el complemento Microsoft Management Console (MMC) de DNS.

Para evitar esto, Microsoft decidió quitar todo de Windows Server 2008 que no era absolutamente esencial para ejecutar servicios de red principales como Active Directory Domain Services (AD DS), DNS, protocolo de configuración dinámica de host (DHCP), archivo e impresión y algunos otros roles de servidor. El resultado es la nueva opción de instalación de Server Core, que se puede usar para crear un servidor que solo admite un número limitado de roles y características. 

## <a name="the-server-core-gui"></a>GUI de Server Core
Al terminar de instalar Server Core en un sistema e iniciar sesión por primera vez, tiene un poco de sorpresa. En la figura 1-2 se muestra la interfaz de usuario de Server Core después del primer inicio de sesión.

![Interfaz de usuario de Server Core](../media/what-is-server-core-2008/Fig1-2.png)

**Figura 1-2** Interfaz de usuario de Server Core

No hay ningún escritorio. Es decir, no hay ningún Shell del explorador de Windows, con el menú Inicio, la barra de tareas y las demás características que se pueden usar para ver. Todo lo que tiene es un símbolo del sistema, lo que significa que tiene que realizar la mayor parte del trabajo de configuración de una instalación de Server Core escribiendo comandos de uno en uno, lo que es lento o mediante scripts y archivos por lotes, que pueden ayudarle a acelerar y simplificar las tareas de configuración mediante su automatización. También puede realizar algunas tareas de configuración iniciales mediante archivos de respuesta cuando realice una instalación desatendida de Server Core. 

Para los administradores que son expertos en el uso de herramientas de línea de comandos como netsh. exe, dfscmd. exe y dnscmd. exe, la configuración y administración de una instalación de Server Core puede ser fácil, incluso divertida. No obstante, para aquellos que no sean expertos, no se perderán todos. Todavía puede usar las herramientas de MMC estándar de Windows Server 2008 para administrar una instalación de Server Core. Solo tiene que usarlas en un sistema diferente que ejecute una instalación completa de Windows Server 2008 o Windows Vista con Service Pack 1. 

Obtendrá más información sobre la configuración y administración de una instalación de Server Core en los capítulos 3 a 6 de este libro, mientras que los capítulos posteriores tratan sobre cómo administrar roles de servidor específicos y otros componentes. Para obtener más información acerca de las diversas herramientas de línea de comandos de Windows y cómo usarlas, hay dos buenos recursos para consultar:
* La sección de referencia de comandos de la biblioteca técnica de Windows Server 2008 () 
* *Pocket Consultant del administrador de línea de comandos de Windows* de William R. Stanek (Microsoft Press, 2008) 

En la tabla 1-2 se enumeran las aplicaciones de GUI principales, junto con sus ejecutables, que están disponibles en una instalación Server Core.

**Tabla 1-2** Aplicaciones GUI disponibles en una instalación Server Core

| Aplicación GUI | Ejecutable con ruta de acceso |
| -------------   | -------------       | 
| Símbolo del sistema | %WINDIR%\System32\Cmd.exe |
| Herramienta de diagnóstico de Soporte técnico de Microsoft | %WINDIR%\System32\MSdt.exe |
| Bloc de notas | %WINDIR%\System32\Notepad.exe |
| Editor del registro | %WINDIR%\System32\Regedt32.exe |
| Información del sistema | %WINDIR%\System32\MSinfo32.exe |
| Administrador de tareas | %WINDIR%\System32\Taskmgr.exe |
| Windows Installer | %WINDIR%\System32\MSiexec.exe |

Esta es una lista bastante corta. Ahora se muestra una lista de elementos de la interfaz de usuario que no están incluidos en Server Core:
* El shell de escritorio de Windows Explorer (Explorer. exe) y las características auxiliares, como los temas 
* Todas las consolas de MMC 
* Todas las utilidades del panel de control, con la excepción de configuración regional y de idioma (Intl. cpl) y fecha y hora (TimeDate. cpl) 
* Todos los motores de representación de Lenguaje de marcado de hipertexto (HTML), incluida la ayuda de Internet Explorer y HTML 
* Windows Mail 
* Reproductor de Windows Media 
* La mayoría de los accesorios, como Paint, Calculator y WordPad

El .NET Framework tampoco está presente en Server Core, lo que significa que no se admite la ejecución de código administrado en una instalación Server Core. Solo el código nativo, que se escribe mediante interfaces de programación de aplicaciones (API) de Windows, se puede ejecutar en Server Core. En Resumen, las aplicaciones GUI que dependen de los .NET Framework o en el shell de Explorer. exe no se ejecutan en Server Core. 

>[!NOTE]
>Dado que Windows PowerShell requiere el .NET Framework, no puede instalar Windows PowerShell en Server Core. Sin embargo, puede administrar una instalación de Server Core de forma remota con Windows PowerShell, siempre y cuando use solo comandos WMI de PowerShell.

## <a name="supported-server-roles"></a>Roles de servidor admitidos 
Una instalación Server Core solo incluye un número limitado de roles de servidor en comparación con una instalación completa de Windows Server 2008. En la tabla 1-3 se comparan los roles disponibles para las instalaciones completas y Server Core de Windows Server 2008 Enterprise Edition. 

**Tabla 1-3** Comparación de los roles de servidor para las instalaciones completas y Server Core de Windows Server 2008 Enterprise Edition

| Rol del servidor  | Disponible en la instalación completa  | Disponible en Server Core  |
| ------------- | :-------------: | :------------: |
| Active Directory Certificate Services (AD CS)  | X |  |
| Servicios de dominio de Active Directory (AD DS)  | X  | X |
| Servicios de federación de Active Directory (AD FS)  | X  |  |
| Servicios de directorio ligero de Active Directory (AD LDS)  | X  | X |
| Active Directory Rights Management Services (AD RMS)  | X  |  |
| Servidor de aplicaciones  | X  |  |
| Servidor DHCP  | X  | X |
| DNS Server  | X  | X |
| Servidor de fax  | X  |  |
| Servicios de archivo  | X  | X |
| Hyper-V  | X | X |
| Servicios de acceso y directivas de redes  | X  |  |
| Servicios de impresión  | X  | X |
| Servicios multimedia de streaming  | X  | X |
| Terminal Services  | X  |  |
| Servicios UDDI  | X  |  |
| Servidor web (IIS) | X | X |
| Servicios de implementación de Windows  | X |  |

Aunque los roles disponibles para Server Core suelen ser los mismos independientemente de la arquitectura (x86 o x64) y la edición del producto, hay algunas excepciones:
* El rol Hyper-V (virtualización) solo está disponible si ha adquirido Windows Server 2008 con los medios del producto Hyper-V (Hyper-V solo está disponible para las versiones x64). Si no necesita este rol, puede comprar Windows Server 2008 sin el producto de Hyper-V en su lugar. 
* El rol servicios de archivo de Standard Edition está limitado a una raíz de Sistema de archivos distribuido independiente (DFS) y no admite la replicación entre archivos (DFS-R). 
* Antes de poder instalar el rol de Streaming Media Services en Server Core, debe descargar e instalar el paquete independiente de Microsoft Update correspondiente (archivo. MSU) para la arquitectura del servidor (x86 o x64) desde el centro de descarga de Microsoft.
* El rol servidor Web (IIS) no es compatible con ASP.NET. Esto se debe a que el .NET Framework no se admite en Server Core, lo que limita lo que puede hacer con un servidor Web Server Core. 

## <a name="supported-optional-features"></a>Características opcionales admitidas
Una instalación Server Core también admite solo un subconjunto limitado de las características disponibles en una instalación completa de Windows Server 2008. En la tabla 1-4 se comparan las características disponibles para las instalaciones completas y Server Core de Windows Server 2008 Enterprise Edition.

**Tabla 1-4** Comparación de las características de las instalaciones completas y Server Core de Windows Server 2008 Enterprise Edition

| Característica  | Disponible en la instalación completa  | Disponible en Server Core  |
| ------------- | :-------------: | :------------: |
| .NET Framework características 3,0  | X  |  |
| Cifrado de unidad BitLocker  | X  | X |
| Extensiones de servidor BITS  | X  |  |
| Kit de administración de Connection Manager  | X |  |
| Experiencia de escritorio  | X |  |
| Clústeres de conmutación por error  | X  | X |
| Administración de directivas de grupo  | X  |  |
| Cliente de impresión en Internet  | X  |  |
| Servidor de nombres de almacenamiento de Internet  | X  |  |
| Monitor de puerto de LPR  | X  |  |
| Message Queuing  | X  |  |
| E/S de múltiples rutas  | X  | X |
| Equilibrio de carga de red  | X  | X |
| Protocolo de resolución de nombres de mismo nivel  | X  |  |
| Experiencia de calidad de audio y vídeo de Windows  | X |  |
| Asistencia remota  | X  |  |
| Compresión diferencial remota | X  |  |
| Herramientas de administración remota del servidor  | X  |  |
| Administrador de almacenamiento extraíble | X  | X |
| P roxy RPC sobre HTTP | X  |  |
| Servicios simples de TCP/IP  | X  |  |
| SMTP Server  | X  |  |
| Servicios de SMNP  | X  | X  |
| Administrador de almacenamiento para redes SAN  | X  |  |
| Subsystem for UNIX-based Applications | X | X  |
| Cliente Telnet  | X | X  |
| Servidor Telnet  | X   |  |
| Cliente TFTP  | X   |  |
| Windows Internal Database  | X  |  |
| Windows PowerShell  | X  |  |
| Servicio de activación de productos de Windows  | X   |  |
| Copias de seguridad de Windows Server características  | X  | X  |
| Administrador de recursos del sistema de Windows  | X  |  |
| Servidor WINS  | X | X |
| Servicio WLAN | X  |  |

De nuevo, hay algunos puntos que debe conocer en relación con las características disponibles en Server Core:
* Algunas características pueden requerir hardware especial para funcionar correctamente (o en todo) en Server Core. Estas características incluyen Cifrado de unidad BitLocker, clústeres de conmutación por error, e/s de múltiples rutas, equilibrio de carga de red y almacenamiento extraíble. 
* Los clústeres de conmutación por error no están disponibles en la edición Standard.

## <a name="server-core-architecture"></a>Arquitectura de Server Core
Con el fin de profundizar en Server Core, echemos un vistazo a la arquitectura de una instalación Server Core de Windows Server 2008 comparándola con la de una instalación completa. En primer lugar, recuerde que Server Core no es una versión diferente de Windows Server 2008, sino simplemente una opción de instalación que puede seleccionar al instalar Windows Server 2008 en un sistema. Esto implica lo siguiente:
* El kernel de una instalación Server Core es el mismo que se encuentra en una instalación completa de la misma arquitectura de hardware (x86 o x64) y Edition. 
* Si hay un archivo binario en una instalación Server Core, una instalación completa de la misma arquitectura de hardware (x86 o x64) y edición tiene la misma versión que el binario en particular (con dos excepciones que se describen más adelante). 
* Si una configuración determinada (por ejemplo, una excepción de Firewall específica o el tipo de inicio de un servicio determinado) tiene una configuración predeterminada determinada en una instalación Server Core, esa configuración se configura exactamente del mismo modo en una instalación completa de la misma arquitectura de hardware (x86 o x64) y Edition.

En la figura 1-3 se muestra una vista simplificada de la arquitectura de una instalación completa y una instalación Server Core de Windows Server 2008. La línea de puntos indica la arquitectura de Server Core, mientras que el diagrama completo representa la arquitectura de una instalación completa. 

El diagrama ilustra la arquitectura modular de Windows Server 2008, con Server Core que se construye en un subconjunto de las características principales del sistema operativo. Para la misma arquitectura y edición de hardware, cada archivo presente en una instalación limpia de Server Core también está presente en una instalación completa, con la excepción de dos archivos especiales (Scregedit. wsf y OCList. exe), que solo están presentes en Server Core. Estos archivos especiales se incluyeron en Server Core para simplificar la configuración inicial de una instalación Server Core y la adición o eliminación de roles y componentes opcionales. 

![Arquitecturas de las instalaciones Server Core y Full](../media/what-is-server-core-2008/Fig1-3.png)

**Figura 1-3** Arquitecturas de las instalaciones Server Core y Full

## <a name="driver-support"></a>Compatibilidad con controladores
El diagrama arquitectónico de Server Core que se muestra en la figura 1-3 es obviamente simplificado; lo único que no se muestra es la diferencia en la compatibilidad de los controladores de dispositivos entre las instalaciones Server Core y las instalaciones completas. Una instalación completa de Windows Server 2008 contiene miles de Controladores integrados para distintos tipos de dispositivos, lo que le permite instalar productos en una amplia variedad de configuraciones de hardware diferentes. (Los sistemas operativos cliente como Windows Vista incluyen todavía más controladores para admitir dispositivos como cámaras digitales y escáneres que normalmente no se usan con servidores). 

Si un nuevo dispositivo está conectado a una instalación completa de Windows Server 2008 (o está instalado en él), el subsistema Plug and Play (PnP) comprueba primero si hay un controlador incluido para el dispositivo. Si se encuentra un controlador incluido compatible, el subsistema PnP instala automáticamente el controlador y, a continuación, funciona. En una instalación completa de Windows Server 2008, se puede mostrar una notificación emergente de globo, que indica que el controlador se ha instalado y el dispositivo está listo para su uso. 

En una instalación Server Core, el proceso de instalación del controlador es el mismo (el subsistema PnP está presente en Server Core) con dos calificaciones. En primer lugar, Server Core incluye solo un número mínimo de controladores incluidos y solo para los siguientes tipos de dispositivos:
* Un controlador de vídeo estándar de matriz de gráficos de vídeo (VGA) 
* Controladores para dispositivos de almacenamiento 
* Controladores para adaptadores de red

Tenga en cuenta que para cada una de las tres categorías de dispositivos que se muestran aquí, Server Core incluye los mismos controladores incluidos en una instalación completa correspondiente (para la misma arquitectura de hardware). 

Además, cuando el subsistema PnP instala automáticamente un controlador para un dispositivo nuevo, lo hace de forma silenciosa; no se muestra ninguna notificación emergente de globo. ¿A qué se debe? Dado que no hay ninguna GUI en Server Core, no hay ninguna barra de tareas, por lo que no hay ningún área de notificación en la barra de tareas. 

¿Qué se puede hacer cuando se agrega la función servicios de impresión a una instalación Server Core y se desea instalar una impresora? El controlador de impresora se agrega manualmente al servidor: Server Core no tiene controladores de impresión incluidos.

## <a name="service-footprint"></a>Superficie del servicio
Como Server Core es una instalación mínima, tiene una superficie de servicio del sistema más pequeña que la instalación completa correspondiente de la misma arquitectura y edición de hardware. Por ejemplo, los servicios del sistema 75 se instalan de forma predeterminada en una instalación completa de Windows Server 2008, de la cual aproximadamente 50 están configurados para el inicio automático. Por el contrario, Server Core solo tiene unos 70 servicios instalados de forma predeterminada, y menos de 40 de estos se inician automáticamente. 

En la tabla 1-5 se enumeran los servicios que se instalan de forma predeterminada en una instalación Server Core, con el modo de inicio de y la cuenta que usa cada servicio.

**Tabla 1-5** Servicios del sistema instalados de forma predeterminada en Server Core

| Nombre del servicio  | Nombre para mostrar  | Modo de inicio  | Cuenta  |
| ------------- | ------------- | ------------ | ------------ |
| AeLookupSvc  | Experiencia de la aplicación  | Automático | LocalSystem |
| AppMgmt  | Administración de aplicaciones  | Manual | LocalSystem |
| BFE | Motor de filtrado de base  | Automático | LocalService |
| BITS | Servicio de transferencia inteligente en segundo plano  | Automático | LocalSystem |
| Explorador. | Explorador de equipos  | Manual | LocalSystem |
| CertPropSvc | Propagación de certificados  | Manual | LocalSystem |
| COMSysApp  | Aplicación de sistema COM+  | Manual | LocalSystem |
| CryptSvc  | Servicios criptográficos  | Automático | Servicio de red |
| DcomLaunch  | Iniciador de procesos de servidor DCOM  | Automático | LocalSystem |
| Dhcp  | Cliente DHCP  | Automático | LocalService |
| Dnscache | Cliente DNS  | Automático | Servicio de red |
| DPS  | Servicio de directivas de diagnóstico  | Automático | LocalService |
| EventLog | Registro de sucesos de Windows  | Automático | LocalService |
| EventSystem  | Sistema de eventos COM+  | Automático | LocalService |
| FCRegSvc  | Servicio de registro de plataforma de Microsoft Canal de fibra  | Manual | LocalService |
| gpsvc  | Cliente de directiva de grupos  | Automático | LocalSystem |
| hidserv | Acceso HID  | Manual | LocalSystem |
| hkmsvc  | Administración de certificados y claves de mantenimiento  | Manual | LocalSystem |
| IKEEXT  | Módulos de creación de claves de IPsec para IKE y AuthIP  | Automático | LocalSystem |
| iphlpsvc  | Asistente IP  | Automático | LocalSystem |
| KeyIso | Aislamiento de claves CNG  | Manual | LocalSystem |
| KtmRm  | KTMRM para DTC (Coordinador de transacciones distribuidas)  | Automático | Servicio de red |
| LanmanServer  | Server  | Automático | LocalSystem |
| LanmanWorkstation  | Estación de trabajo  | Automático | LocalService |
| lltdsvc  | Asignador de detección de topologías de nivel de vínculo  | Manual | LocalService |
| lmhosts  | Asistente NetBIOS sobre TCP/IP  | Automático | LocalService |
| MpsSvc  | Firewall de Windows  | Automático | LocalService |
| MSDTC  | Coordinador de transacciones distribuidas  | Automático | Servicio de red |
| MSiSCSI  | Servicio del iniciador iSCSI de Microsoft  | Manual | LocalSystem |
| msiserver  | Windows Installer  | Manual | LocalSystem |
| napagent  | Agente de protección de acceso a la red  | Manual | Servicio de red |
| Netlogon  | Netlogon  | Manual | LocalSystem |
| netprofm  | Servicio de lista de redes  | Automático | LocalService |
| NlaSvc  | Reconocimiento de ubicación de red  | Automático | Servicio de red |
| nsi  | Servicio Interfaz de almacenamiento en red  | Automático | LocalService |
| pla  | Registros y alertas de rendimiento  | Manual | LocalService |
| PlugPlay  | Plug and Play  | Automático | LocalSystem |
| PolicyAgent  | Agente de directiva IPsec  | Automático | Servicio de red |
| ProfSvc  | Servicio de perfiles de usuario  | Automático | LocalSystem |
| ProtectedStorage  | Almacenamiento protegido  | Manual | LocalSystem |
| RemoteRegistry  | Registro remoto  | Automático | LocalService |
| RpcSs  | Llamada a procedimiento remoto (RPC)  | Automático | Servicio de red |
| RSoPProv | Conjunto resultante de proveedor de directivas  | Manual | LocalSystem |
| sacsvr  | Aplicación auxiliar especial de la consola de administración  | Manual | LocalSystem |
| SamSs  | Administrador de cuentas de seguridad  | Automático | LocalSystem |
| SCardSvr | Tarjeta inteligente  | Manual | LocalService |
| Programa | Programador de tareas  | Automático | LocalSystem |
| SCPolicySvc | Directiva de extracción de tarjetas inteligentes  | Manual | LocalSystem |
| seclogon | Inicio de sesión secundario  | Automático | LocalSystem |
| SENS | Servicio de notificación de eventos del sistema  | Automático | LocalSystem |
| SessionEnv | Configuración de Terminal Services  | Manual | LocalSystem |
| slsvc  | Licencias de software | Automático | Servicio de red |
| SNMPTRAP  | Captura de SNMP  | Manual | LocalService |
| swprv  | Proveedor de instantáneas de software de Microsoft | Manual | LocalSystem |
| TB | Servicios base de TPM  | Manual | LocalService |
| TermService  | Terminal Services | Automático | Servicio de red |
| TrustedInstaller | Instalador de módulos de Windows  | Automático | LocalSystem |
| UmRdpService | Redirector de puerto en modo de Terminal Services  | Manual | LocalSystem |
| vds | Disco virtual  | Manual | LocalSystem |
| VSS | Instantánea de volumen  | Manual | LocalSystem |
| W32Time | Hora de Windows  | Automático | LocalService |
| WcsPlugInService  | Sistema de colores de Windows  | Manual | LocalService |
| WdiServiceHost  | Host de servicio de diagnóstico  | Manual | LocalService |
| WdiSystemHost  | Host de sistema de diagnóstico  | Manual | LocalSystem |
| Wecsvc | Recopilador de eventos de Windows  | Manual | Servicio de red |
| WinHttpAuto-ProxySvc  | Servicio de detección automática del proxy web WinHTTP  | Automático | LocalService |
| Winmgmt | Instrumental de administración de Windows | Automático | LocalSystem |
| WinRM  | Administración remota de Windows (WS-Management) | Automático | Servicio de red |
| wmiApSrv  | Adaptador de rendimiento de WMI  | Manual | LocalSystem |
| wuauserv | Windows Update | Automático | LocalSystem |