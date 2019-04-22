---
title: ¿Qué es Server Core 2008?
description: Obtenga información sobre la opción de instalación Server Core en Windows Server 2008
ms.prod: windows-server-threshold
ms.author: helohr
ms.date: 11/01/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: c1ef71dbc589cfdeac63b46d720c4bdd0a44dbaa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815406"
---
#<a name="what-is-server-core-2008"></a>¿Qué es Server Core 2008?
>Se aplica a: Windows Server 2008

>[!NOTE]
>Esta información se aplica a Windows Server 2008. Para obtener información acerca de Server Core en Windows Server, vea [¿qué es la instalación Server Core en Windows Server](https://docs.microsoft.com/windows-server/administration/server-core/what-is-server-core). 

La opción Server Core es una nueva opción de instalación mínima que está disponible al implementar la edición Standard, Enterprise o Datacenter de Windows Server 2008. Server Core ofrece una instalación mínima de Windows Server 2008 que admite la instalación de determinados roles de servidor, como se describe más adelante en este capítulo. Compare esto con la opción de instalación completa de Windows Server 2008, que admite la instalación de todos los roles de servidor disponibles y también otro de Microsoft o aplicaciones de servidor de aplicaciones de terceros, como Microsoft Exchange Server o SAP. 

Antes que avancemos, debe explicar la frase "opción de instalación". Normalmente, al adquirir una copia de Windows Server 2008, adquirir una licencia para usar ciertas ediciones o unidades de (almacenamiento SKU). Tabla 1-1 se enumeran las diferentes ediciones de Windows Server 2008 que están disponibles. La tabla también indica qué opciones de instalación (completo, Server Core o ambos) están disponibles para cada edición.

**Tabla 1-1** las ediciones de Windows Server 2008 y su compatibilidad con las opciones de instalación
| Edición       | Completo          | Server Core  |
| ------------- | :-------------: | :------------: |
| Windows Server 2008 Standard (x86 y x64)       | X | X        |
| Windows Server 2008 Enterprise (x86 y x64)       | X | X        |
| Windows Server 2008 Datacenter (x86 y x64)        | X | X       |
| Windows Web Server 2008 (x86 y x64)       | X | X  |
| Windows Server 2008 For Itanium-based systems       | X |     |
| Windows HPC Server 2008 (sólo x64)       | X |   |
| Windows Server 2008 Standard sin Hyper-V (x86 y x64) | X | X |
| Windows Server 2008 Enterprise sin Hyper-V (x86 y x64)  | X | X |
| Windows Server 2008 Standard sin Hyper-V (x86 y x64) | X | X |

Para entender lo que es una opción de instalación"", supongamos que ha comprado una licencia por volumen que le permite instalar una copia de Windows Server 2008 Enterprise Edition. Cuando inserte el medio de licencias por volumen en un sistema y comenzar el proceso de instalación, una de las pantallas que podrá ver, tal como se muestra en la figura 1-1, presenta una variedad de ediciones y opciones de instalación.

![Seleccionar una opción de instalación Server Core para instalar](../media/what-is-server-core-2008/FIg1-1.png)

**Figura 1-1** seleccionando una opción de instalación Server Core para instalar

En la figura 1-1, la licencia por volumen (o clave de producto, para medios de venta directa) le ofrece dos opciones de instalación, puede elegir entre: la segunda opción (una completa instalación de Windows Server 2008 Enterprise) y la opción quinta (un Server Core instalación de Windows Server 2008 Enterprise), con el último seleccionado en este ejemplo. 

##<a name="full-vs-server-core"></a>Durabilidad total frente a. Server Core 
Desde los primeros días de la plataforma Microsoft Windows, servidores de Windows eran básicamente "todo," servidores que incluyen todos los tipos de características, algunas de las cuales es posible que realmente no utilice en su entorno de red. Por ejemplo, al instalar Windows Server 2003 en un sistema, se instalaron los archivos binarios de enrutamiento y acceso remoto (RRAS) en el servidor incluso si no es necesario para este servicio tenía (aunque todavía tenía que configurar y habilitar RRAS antes de que funcionará). Windows Server 2008 mejora las versiones anteriores mediante la instalación de los archivos binarios necesarios para un rol de servidor sólo si opta por instalar ese rol en especial en el servidor. Sin embargo, la opción de instalación completa de Windows Server 2008 instala todavía muchos servicios y otros componentes que a menudo no son necesarios para un escenario de uso concreto. 

Este es el motivo que Microsoft ha creado una segunda opción de instalación, Server Core, para Windows Server 2008: para eliminar todos los servicios y otras características que no son esenciales para la compatibilidad de ciertos normalmente utiliza roles de servidor. Por ejemplo, un servidor de sistema de nombres de dominio (DNS) realmente no necesita instalado porque no quiere explorar el Web desde un servidor DNS por motivos de seguridad de Windows Internet Explorer. Y un servidor DNS no tiene una interfaz gráfica de usuario (GUI), ya que puede administrar prácticamente todos los aspectos de DNS desde la línea de comandos mediante el comando Dnscmd.exe eficaces, o remotamente mediante Microsoft Management Console (MMC) de DNS, complemento.

Para evitar esto, Microsoft decidió quitar todo, desde Windows Server 2008 que no es absolutamente esencial para ejecutar servicios de red principales, como los servicios de dominio de Active Directory (AD DS), DNS, el protocolo de configuración dinámica de Host (DHCP), archivo e imprimir, y un Algunos roles de servidor. El resultado es la nueva opción de instalación Server Core, que se puede usar para crear un servidor que admite sólo un número limitado de roles y características. 

##<a name="the-server-core-gui"></a>The Server Core GUI
Cuando termine la instalación de Server Core en un sistema y el inicio de sesión por primera vez, tiene un bit de una sorpresa. Figura 1-2 se muestra la interfaz de usuario de Server Core tras el primer inicio de sesión.

![Interfaz de usuario de Server Core](../media/what-is-server-core-2008/Fig1-2.png)

**Figura 1-2** interfaz de usuario de Server Core

¡No hay ningún escritorio! Es decir, no hay ningún shell del explorador de Windows, con su menú Inicio, barra de tareas y las demás características que es posible que esté acostumbrado a ver. Todo lo que es un símbolo del sistema, lo que significa que tendrá que realizar la mayoría del trabajo de la configuración de una instalación Server Core ya sea escribiendo comandos uno a la vez, que es lento, o mediante el uso de scripts y archivos por lotes, lo que pueden ayudar a acelerar y simplificar su configurati en las tareas mediante la automatización de ellos. También puede realizar algunas tareas de configuración inicial con archivos de respuesta al realizar una instalación desatendida de Server Core. 

Para los administradores que son expertos mediante herramientas de línea de comandos como Netsh.exe, Dfscmd.exe y Dnscmd.exe, configurar y administrar una instalación Server Core pueden ser fácil y divertido. Para aquellos que no son expertos, sin embargo, no todo está perdido. Todavía puede usar las herramientas estándar de Windows Server 2008 de MMC para administrar una instalación Server Core. Basta con que se puede usar en un sistema diferente que se ejecuta en una instalación completa de Windows Server 2008 o Windows Vista con Service Pack 1. 

Aprenderá más sobre cómo configurar y administrar una instalación Server Core en los capítulos 3 a 6 de este libro, mientras que los capítulos posteriores ocuparse de cómo administrar roles de servidor específicos y otros componentes. Para obtener más información sobre las diversas herramientas de línea de comandos de Windows y cómo usarlas, hay dos buenos recursos que puede consultar:
* La sección de referencia de comandos del (biblioteca técnica de Windows Server 2008) 
* *Pocket Consultant del Administrador de línea de comandos de Windows* por Stanek (Microsoft Press, 2008) 

Tabla 1-2 se enumeran las principales aplicaciones de interfaz gráfica de usuario, junto con sus archivos ejecutables, que están disponibles en una instalación Server Core.

**Tabla 1-2** aplicaciones de interfaz gráfica de usuario disponibles en una instalación Server Core
| Aplicación de interfaz gráfica de usuario | Archivo ejecutable con la ruta de acceso |
| -------------   | -------------       | 
| Símbolo del sistema | %WINDIR%\System32\Cmd.exe |
| Herramienta de diagnóstico de soporte técnico de Microsoft | %WINDIR%\System32\MSdt.exe |
| Bloc de notas | %WINDIR%\System32\Notepad.exe |
| Editor del registro | %WINDIR%\System32\Regedt32.exe |
| Información del sistema | %WINDIR%\System32\MSinfo32.exe |
| Administrador de tareas | %WINDIR%\System32\Taskmgr.exe |
| Windows Installer | %WINDIR%\System32\MSiexec.exe |

Eso es una breve lista descriptiva. Ahora presentamos una lista de elementos de la interfaz de usuario que no están incluidos en Server Core:
* El shell del explorador de Windows de escritorio (Explorer.exe) y las características de apoyo, como los temas 
* Todas las consolas MMC 
* Todas las utilidades de Panel de Control, con la excepción y lenguaje de configuración Regional (Intl.cpl) y la fecha y hora (Timedate.cpl) 
* Todos los motores de representación de lenguaje de marcado de hipertexto (HTML), incluidos Internet Explorer y la Ayuda HTML 
* Windows Mail 
* Reproductor de Windows Media 
* La mayoría de accesorios como Wordpad, Paint y Calculadora

.NET Framework también no está presente en Server Core, lo que significa que no hay compatibilidad para ejecutar el código administrado en una instalación Server Core. Solo el código nativo, código escrito mediante interfaces de programación de aplicaciones (API) de Windows, puede ejecutarse en Server Core. En resumen, las aplicaciones de GUI que dependan en cualquiera de .NET Framework o en el Explorer.exe shell no se ejecuta en Server Core. 

>[!NOTE]
>Dado que Windows PowerShell requiere .NET Framework, no puede instalar Windows PowerShell en Server Core. Sin embargo, puede administrar una instalación Server Core remotamente mediante Windows PowerShell siempre y cuando utilice solo los comandos de PowerShell WMI.

##<a name="supported-server-roles"></a>Roles de servidor compatibles 
Una instalación Server Core incluye sólo un número limitado de roles de servidor en comparación con una instalación completa de Windows Server 2008. Tabla 1-3 se comparan los roles disponibles para las instalaciones completas y Server Core de Windows Server 2008 Enterprise Edition. 

**Tabla 1-3** comparación de roles de servidor para las instalaciones completas y de Server Core de Windows Server 2008 Enterprise Edition
| Rol del servidor  | Disponible en la instalación completa  | Disponibilidad en Server Core  |
| ------------- | :-------------: | :------------: |
| Active Directory Certificate Services (AD CS)  | X |  |
| Servicios de dominio de Active Directory (AD DS)  | X  | X |
| Servicios de federación de Active Directory (AD FS)  | X  |  |
| Active Directory Lightweight Directory Services (AD LDS)  | X  | X |
| Active Directory Rights Management Services (AD RMS)  | X  |  |
| Servidor de aplicaciones  | X  |  |
| Servidor DHCP  | X  | X |
| Servidor DNS  | X  | X |
| Servidor de fax  | X  |  |
| Servicios de archivos  | X  | X |
| Hyper-V  | X | X |
| Servicios de acceso y directivas de redes  | X  |  |
| Servicios de impresión  | X  | X |
| Servicios de multimedia de transmisión por secuencias  | X  | X |
| Terminal Services  | X  |  |
| Servicios UDDI  | X  |  |
| Servidor web (IIS) | X | X |
| Servicios de implementación de Windows  | X |  |

Aunque los roles disponibles para Server Core generalmente son los mismos independientemente de la arquitectura (x86 o x64) y la edición del producto, hay algunas excepciones:
* El rol de Hyper-V (virtualización) solo está disponible si ha adquirido Windows Server 2008 con el soporte físico del producto Hyper-V (Hyper-V sólo está disponible para x64 versiones). Si no necesita este rol, puede adquirir Windows Server 2008 sin soporte físico del producto Hyper-V en su lugar. 
* El rol Servicios de archivo en Standard Edition está limitado a una raíz de sistema de archivos distribuido (DFS) independiente y no admite la replicación Cross-File (DFS-R). 
* Antes de que puede instalar el rol de servicios multimedia de transmisión por secuencias en Server Core, debe descargar e instalar el Microsoft Update independiente paquete adecuado (archivo .msu) para la arquitectura de su servidor (x86 o x64) desde Microsoft Download Center.
* El rol de servidor Web (IIS) no es compatible con ASP.NET. Esto es porque .NET Framework no se admite en Server Core, lo que limita lo que puede hacer con un servidor Web de Server Core. 

##<a name="supported-optional-features"></a>Características opcionales admitidas
Una instalación Server Core también admite solo un subconjunto limitado de las características disponibles en una instalación completa de Windows Server 2008. Tabla 1-4 se comparan las características disponibles para las instalaciones completas y Server Core de Windows Server 2008 Enterprise Edition.

**Tabla 1-4** comparación de características para las instalaciones completas y de Server Core de Windows Server 2008 Enterprise Edition

| Característica  | Disponible en la instalación completa  | Disponibilidad en Server Core  |
| ------------- | :-------------: | :------------: |
| Características de .NET framework 3.0  | X  |  |
| Cifrado de unidad BitLocker  | X  | X |
| Extensiones de servidor BITS  | X  |  |
| Kit de administración de Connection Manager  | X |  |
| Experiencia de escritorio  | X |  |
| Clústeres de conmutación por error  | X  | X |
| Administración de directivas de grupo  | X  |  |
| Cliente de impresión en Internet  | X  |  |
| Servidor de nombres de almacenamiento de Internet  | X  |  |
| Monitor de puerto de LPR  | X  |  |
| Message Queue Server  | X  |  |
| E/S de múltiples rutas  | X  | X |
| Equilibrio de carga de red  | X  | X |
| Protocolo de resolución de nombres de mismo nivel  | X  |  |
| Windows Audio Video Experience (qWAVE)  | X |  |
| Asistencia remota  | X  |  |
| Compresión diferencial remota | X  |  |
| Herramientas de administración remota del servidor  | X  |  |
| Administrador de almacenamiento extraíble | X  | X |
| Proxy RPC sobre HTTP | X  |  |
| Servicios simples de TCP/IP  | X  |  |
| Servidor SMTP  | X  |  |
| Servicios SMNP  | X  | X  |
| Administrador de almacenamiento para redes SAN  | X  |  |
| Subsistema para aplicaciones UNIX | X | X  |
| Cliente Telnet  | X | X  |
| Servidor Telnet  | X   |  |
| Cliente TFTP  | X   |  |
| Windows Internal Database  | X  |  |
| Windows PowerShell  | X  |  |
| Servicio de activación de producto de Windows  | X   |  |
| Características de copia de seguridad de Windows Server  | X  | X  |
| Administrador de recursos del sistema de Windows  | X  |  |
| Servidor WINS  | X | X |
| Servicio WLAN | X  |  |

De nuevo, hay algunos puntos que debe conocer sobre las características disponibles en Server Core:
* Algunas características pueden requerir hardware especial para funcionar correctamente (o) en Server Core. Estas características incluyen cifrado de unidad BitLocker, agrupación en clústeres de conmutación por error, E/S de múltiples rutas, equilibrio de carga de red y almacenamiento extraíble. 
* Agrupación en clústeres de conmutación por error no está disponible en Standard Edition.

##<a name="server-core-architecture"></a>Arquitectura de Server Core
Al profundizar más en Server Core, veamos brevemente la arquitectura de una instalación Server Core de Windows Server 2008 en comparación con el de una instalación completa. En primer lugar, recuerde que Server Core no es una versión diferente de Windows Server 2008, pero basta con una opción de instalación que se puede seleccionar al instalar Windows Server 2008 en un sistema. Esto implica lo siguiente:
* El kernel en una instalación Server Core es lo mismo que se encuentra en una instalación completa de la misma arquitectura de hardware (x86 o x64) y la edición. 
* Si hay un archivo binario en una instalación Server Core, una instalación completa de la misma arquitectura de hardware (x86 o x64) y la edición tiene la misma versión de ese binario determinado (con dos excepciones que se describe más adelante). 
* Si una configuración determinada (por ejemplo, una excepción de firewall específicas o el tipo de inicio de un servicio en particular) tiene una determinada configuración predeterminada en una instalación Server Core, ese valor se configura exactamente del mismo modo en una instalación completa de la misma arquitectura de hardware (x86 o x64) y edición.

Figura 1-3 se muestra una vista simplificada de la arquitectura de una instalación completa y una instalación Server Core de Windows Server 2008. La línea de puntos indica la arquitectura de Server Core, mientras que todo el diagrama representa la arquitectura de una instalación completa. 

El diagrama muestra la arquitectura modular de Windows Server 2008, con Server Core que se está construyendo tras un subconjunto de las principales características de sistema operativo. Para la misma arquitectura de hardware y la edición, cada archivo está presente en una instalación limpia de Server Core también está presente en una instalación completa, con la excepción de dos archivos especiales (Scregedit.wsf y Oclist.exe), que están presentes solo en Server Core. Estos archivos especiales se incluyeron en Server Core para simplificar la configuración inicial de una instalación Server Core así como la adición o eliminación de funciones y componentes opcionales. 

![Las arquitecturas de las instalaciones de Server Core y completo](../media/what-is-server-core-2008/Fig1-3.png)

**Figura 1-3** las arquitecturas de las instalaciones de Server Core y completo

##<a name="driver-support"></a>Compatibilidad de controladores
Obviamente, se simplifica el diagrama arquitectónico de Server Core, se muestra en la figura 1-3; una cosa que no se muestra es la diferencia en la compatibilidad con controladores de dispositivo entre las instalaciones completas y Server Core. Una instalación completa de Windows Server 2008 contiene miles de controladores para diferentes tipos de dispositivos, lo que le permite instalar productos en una amplia variedad de configuraciones de hardware diferentes. (Sistemas operativos como Windows Vista incluye controladores aún más para admitir dispositivos como cámaras digitales y escáneres que normalmente no se usan con los servidores). 

Si un nuevo dispositivo está conectado a (o instalado en) una instalación completa de Windows Server 2008, el subsistema de Plug and Play (PnP) comprueba primero si existe un controlador en el cuadro del dispositivo. Si se encuentra un controlador en el cuadro compatible, el subsistema de Plug and Play automáticamente instala el controlador y el dispositivo, a continuación, funciona. En una instalación completa de Windows Server 2008, se puede mostrar una notificación de globo emergente, que indica que se ha instalado el controlador y el dispositivo está listo para su uso. 

En una instalación Server Core, el proceso de instalación del controlador es el mismo (el subsistema de Plug and Play está presente en Server Core) con dos salvedades. En primer lugar, Server Core incluye sólo un número mínimo de controladores y solo para los siguientes tipos de dispositivos:
* Un controlador de vídeo estándar de vídeo gráficos VGA (matriz) 
* Controladores de dispositivos de almacenamiento 
* Controladores de adaptadores de red

Tenga en cuenta que para cada una las tres categorías de dispositivos que se muestra aquí, Server Core incluye los mismos controladores predeterminados que se encuentran en una instalación completa correspondiente (para la misma arquitectura de hardware). 

Además, cuando el subsistema de Plug and Play instalará automáticamente el controlador para un dispositivo nuevo, lo hace en modo silencioso, se muestra ninguna notificación de globo emergente. ¿Por qué no? Porque no hay ninguna GUI en Server Core no hay ninguna barra de tareas, por lo que no hay ningún área de notificación en la barra de tareas. 

¿Qué hacer cuando se agrega el rol Servicios de impresión para una instalación Server Core y desea instalar a una impresora? Agregue manualmente el controlador de impresora al servidor, Server Core no tiene controladores de impresión en el equipo.

##<a name="service-footprint"></a>Consumo de servicio
Dado que Server Core es una instalación mínima, tiene una superficie menor de servicio de sistema que una instalación completa correspondiente de la misma arquitectura de hardware y la edición. Por ejemplo, aproximadamente 75 servicios del sistema se instalan de forma predeterminada en una instalación completa de Windows Server 2008, de los cuales aproximadamente 50 están configurados para el inicio automático. Por el contrario, Server Core tiene sólo unos 70 servicios que se instalan de forma predeterminada, y menos de 40 de estas inicio automáticamente. 

1-5 de la tabla se enumeran los servicios que se instalan de forma predeterminada en una instalación Server Core, con el modo de inicio y de cuenta utilizados por cada servicio.

**Tabla 1-5** servicios del sistema instalados de forma predeterminada en Server Core

| Nombre del servicio  | Nombre para mostrar  | Modo de inicio  | Cuenta  |
| ------------- | ------------- | ------------ | ------------ |
| AeLookupSvc  | Experiencia de aplicación  | Automático | LocalSystem |
| AppMgmt  | Administración de aplicaciones  | Manual | LocalSystem |
| BFE | Motor de filtrado de base  | Automático | LocalService |
| BITS | Servicio de transferencia inteligente en segundo plano  | Automático | LocalSystem |
| Browser | Examinador de equipos  | Manual | LocalSystem |
| CertPropSvc | Propagación de certificados  | Manual | LocalSystem |
| COMSysApp  | Aplicación del sistema COM +  | Manual | LocalSystem |
| CryptSvc  | Servicios criptográficos  | Automático | Servicio de red |
| DcomLaunch  | Iniciador de procesos de servidor DCOM  | Automático | LocalSystem |
| DHCP  | Cliente DHCP  | Automático | LocalService |
| Dnscache | Cliente DNS  | Automático | Servicio de red |
| DPS  | Servicio de directivas de diagnóstico  | Automático | LocalService |
| Registro de eventos | Registro de sucesos de Windows  | Automático | LocalService |
| EventSystem  | Sistema de eventos COM +  | Automático | LocalService |
| FCRegSvc  | Servicio de registro de plataforma de canal de fibra de Microsoft  | Manual | LocalService |
| gpsvc  | Cliente de directiva de grupo  | Automático | LocalSystem |
| hidserv | Acceso de dispositivo de interfaz humana  | Manual | LocalSystem |
| hkmsvc  | Administración de certificados y claves de mantenimiento  | Manual | LocalSystem |
| IKEEXT  | Módulos de creación de claves de IPsec para IKE y AuthIP  | Automático | LocalSystem |
| iphlpsvc  | Aplicación auxiliar IP  | Automático | LocalSystem |
| KeyIso | Aislamiento de claves CNG  | Manual | LocalSystem |
| KtmRm  | KTMRM para DTC (Coordinador de transacciones distribuidas)  | Automático | Servicio de red |
| LanmanServer  | Servidor  | Automático | LocalSystem |
| LanmanWorkstation  | Workstatione  | Automático | LocalService |
| lltdsvc  | Asignador de detección de topología de nivel de vínculo  | Manual | LocalService |
| lmhosts  | Aplicación auxiliar de NetBIOS de TCP/IP  | Automático | LocalService |
| MpsSvc  | Firewall de Windows  | Automático | LocalService |
| MSDTC  | Coordinador de transacciones distribuidas  | Automático | Servicio de red |
| MSiSCSI  | Servicio del iniciador iSCSI de Microsoft  | Manual | LocalSystem |
| msiserver  | Windows Installer  | Manual | LocalSystem |
| NAPAgent  | Agente de protección de acceso de red  | Manual | Servicio de red |
| Netlogon  | Netlogon  | Manual | LocalSystem |
| netprofm  | Servicio de lista de red  | Automático | LocalService |
| NlaSvc  | Reconocimiento de ubicación de red  | Automático | Servicio de red |
| nsi  | Servicio Network Store Interface  | Automático | LocalService |
| pla  | Alertas y registros de rendimiento  | Manual | LocalService |
| PlugPlay  | Plug and Play  | Automático | LocalSystem |
| PolicyAgent  | Agente de directiva IPsec  | Automático | Servicio de red |
| ProfSvc  | Servicio de perfil de usuario  | Automático | LocalSystem |
| ProtectedStorage  | Almacenamiento protegido  | Manual | LocalSystem |
| RemoteRegistry  | Registro remoto  | Automático | LocalService |
| RpcSs  | Llamada a procedimiento remoto (RPC)  | Automático | Servicio de red |
| RSoPProv | Conjunto resultante de proveedor de directivas  | Manual | LocalSystem |
| sacsvr  | Ayudante de la consola de administración especial  | Manual | LocalSystem |
| SamSs  | Administrador de cuentas de seguridad  | Automático | LocalSystem |
| SCardSvr | Tarjeta inteligente  | Manual | LocalService |
| Programa | Programador de tareas  | Automático | LocalSystem |
| SCPolicySvc | Directiva de eliminación de tarjeta inteligente  | Manual | LocalSystem |
| seclogon | Inicio de sesión secundario  | Automático | LocalSystem |
| SENS | Servicio de notificación de eventos del sistema  | Automático | LocalSystem |
| SessionEnv | Configuración de Terminal Services  | Manual | LocalSystem |
| slsvc  | Licencias de software | Automático | Servicio de red |
| SNMPTRAP  | Captura de SNMP  | Manual | LocalService |
| swprv  | Proveedor de instantáneas de Software de Microsoft | Manual | LocalSystem |
| TBS | Servicios de Base TPM  | Manual | LocalService |
| TermService  | Terminal Services | Automático | Servicio de red |
| TrustedInstaller | Instalador de módulos de Windows  | Automático | LocalSystem |
| UmRdpService | Redirector de puerto de modo de usuario de Terminal Services  | Manual | LocalSystem |
| vds | Disco virtual  | Manual | LocalSystem |
| VSS | Instantáneas de volumen  | Manual | LocalSystem |
| W32Time | Hora de Windows  | Automático | LocalService |
| WcsPlugInService  | Sistema de colores de Windows  | Manual | LocalService |
| WdiServiceHost  | Host de servicio de diagnóstico  | Manual | LocalService |
| WdiSystemHost  | Host de sistema de diagnóstico  | Manual | LocalSystem |
| Wecsvc | Recopilador de eventos de Windows  | Manual | Servicio de red |
| WinHttpAuto-ProxySvc  | Servicio de detección automática de Proxy Web de WinHTTP  | Automático | LocalService |
| WinMgmt | Instrumental de administración de Windows | Automático | LocalSystem |
| WinRM  | Administración remota de Windows (WS-Management) | Automático | Servicio de red |
| wmiApSrv  | Adaptador de rendimiento de WMI  | Manual | LocalSystem |
| wuauserv | Windows Update | Automático | LocalSystem |