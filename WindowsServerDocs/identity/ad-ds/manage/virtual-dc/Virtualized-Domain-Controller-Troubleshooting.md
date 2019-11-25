---
ms.assetid: 249ba1be-b0d3-4a77-99af-3699074a2b6e
title: Solucionar problemas de controladores de dominio virtualizados
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 42f9807eb09ff633642a90181b3abfacc00ab76b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409041"
---
# <a name="virtualized-domain-controller-troubleshooting"></a>Solucionar problemas de controladores de dominio virtualizados

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se proporciona la metodología detallada para solucionar problemas de la característica de controlador de dominio virtualizado.  

-   [Solucionar problemas de clonación de controladores de dominio virtualizados](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCCloning)  

-   [Solucionar problemas de restauración segura de controladores de dominio virtualizados](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCSafeRestore)  

## <a name="BKMK_Intro"></a>Aparición  
La mejor manera de mejorar tus habilidades para solucionar problemas es crear un laboratorio de pruebas y examinar rigurosamente los escenarios de trabajo normales. Si encuentras errores, te resultarán más obvios y más fáciles de comprender porque conocerás bien cómo funciona la promoción de controladores de dominio. Esto también te permite desarrollar habilidades de análisis y análisis de red. Esto es aplicable a todas las tecnologías de sistemas distribuidos, no solo a la implementación de controladores de dominio virtualizados.  

Los elementos fundamentales para una solución avanzada de los problemas de configuración de controladores de dominio son:  

1.  Análisis lineal combinado con atención exhaustiva a los detalles.  

2.  Comprender los análisis de captura de red.  

3.  Comprender los registros integrados.  

El primero y el segundo quedan fuera del ámbito de este tema, pero el tercero se puede explicar con más detalle. La solución de problemas de controladores de dominio virtualizados requiere un método lógico y lineal. La clave es enfocar el problema usando los datos proporcionados y recurrir a análisis y herramientas complejos cuando hayas agotado los registros y resultados proporcionados.  

## <a name="BKMK_TshootVDCCloning"></a>Solucionar problemas de clonación de controladores de dominio virtualizados  
En estas secciones se tratan los siguientes temas:  

-   [Herramientas para la solución de problemas](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_Tools)  

-   [Opciones de registro](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_LoggingOptions)  

-   [Metodología general para solucionar problemas de clonación de controladores de dominio](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)  

-   [Server Core y el registro de eventos](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_ServerCoreEvents)  

-   [Solución de problemas específicos](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_SpecificProblems)  

La estrategia para solucionar problemas de clonación de controladores de dominio virtualizados sigue este formato general:  

![solución de problemas de controladores de dominio virtuales](media/Virtualized-Domain-Controller-Troubleshooting/ADDS_VDC_TroublehsootingFlowchart.png)  

### <a name="BKMK_Tools"></a>Herramientas para la solución de problemas  

#### <a name="BKMK_LoggingOptions"></a>Opciones de registro  
Los registros integrados son la herramienta más importante para solucionar problemas de clonación de controladores de dominio. Todos estos registros están habilitados y configurados para ofrecer el máximo nivel de detalle de forma predeterminada.  

|||  
|-|-|  
|**Sesión**|**Inicia**|  
|**Clonación**|-Event Eventos\registros logs\System<br />-Servicio de visor eventos\registros de eventos y servicios de logs\Directory<br />-%SystemRoot%\debug\dcpromo.log|  
|**Ascende**|-%SystemRoot%\debug\dcpromo.log<br />-Servicio de visor eventos\registros de eventos y servicios de logs\Directory<br />-Event Eventos\registros logs\System<br />-Event visor eventos\registros and Services logs\File Replication Service<br />-Visor eventos\registros de eventos y servicios de logs\DFS replicación|  

#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Herramientas y comandos para solucionar problemas de configuración de controladores de dominio  
Para solucionar problemas que no se explican con los registros, usa las herramientas siguientes como punto de partida:  

-   Dcdiag.exe  

-   Repadmin.exe  

-   Monitor de red 3.4  

### <a name="BKMK_GeneralMethodology"></a>Metodología general para solucionar problemas de clonación de controladores de dominio  

1.  ¿La máquina virtual arranca en Modo de reparación de servicios de directorio (DSRM)? Esto indica que es necesario solucionar algún problema. Para iniciar sesión en DSRM, usa la cuenta **.\Administrator** y especifica la contraseña de DSRM.  

    1.  Examina el archivo Dcpromo.log.  

        1.  ¿Los pasos iniciales de la clonación se realizaron correctamente pero se produjo un error en la promoción del controlador de dominio?  

        2.  ¿Los errores indican que hay problemas con el controlador de dominio local o con el entorno de AD DS, por ejemplo, errores devueltos por el emulador de PDC?  

    2.  Examina los registros de eventos de Sistema y Servicios de directorio, así como dccloneconfig.xml y CustomDCCloneAllowList.xml.  

        1.  ¿Una aplicación incompatible necesita estar en la lista de permitidos CustomDCCloneAllowList.xml?  

        2.  ¿La dirección IP o el nombre del equipo que figuran en el archivo dccloneconfig.xml están duplicados o no son válidos?  

        3.  ¿El sitio de Active Directory que figura en el archivo dccloneconfig.xml no es válido?  

        4.  ¿La dirección IP no está establecida en el archivo dccloningconfig.xml y no hay ningún servidor DHCP disponible?  

        5.  ¿El emulador de PDC está conectado y disponible a través del protocolo RPC?  

        6.  ¿El controlador de dominio es miembro del grupo Controladores de dominio clonables? ¿El permiso **Permitir al controlador de dominio crear un clon de sí mismo** está establecido en la raíz del dominio para ese grupo?  

        7.  ¿El archivo Dccloneconfig.xml contiene errores de sintaxis que impiden realizar un análisis correctamente?  

        8.  ¿El hipervisor es compatible?  

        9. ¿Se produjo un error en la promoción del controlador de dominio después de que la clonación comenzara correctamente?  

        10. ¿Se superó el número máximo de nombres de controlador de dominio generados automáticamente (9999)?  

        11. ¿La dirección MAC está duplicada?  

2.  ¿El nombre de host del clon es el mismo que el DC de origen?  

    1.  ¿Hay un archivo Dccloneconfig.xml en una de las ubicaciones permitidas?  

3.  ¿La máquina virtual arranca en modo normal y la clonación se completa, pero el controlador de dominio no funciona correctamente?  

    1.  Comprueba primero si el nombre de host ha cambiado en el clon. Si el nombre de host es diferente, la clonación se ha completado al menos parcialmente.  

    2.  ¿El controlador de dominio tiene una dirección IP duplicada para el controlador de dominio de origen en el archivo dccloneconfig.xml, pero el controlador de dominio de origen estaba desconectado durante la clonación?  

    3.  Si el controlador de dominio se publicita, trata el problema como un problema normal posterior a la promoción que tendrías sin clonación.  

    4.  Si el controlador de dominio no se publicita, busca errores posteriores a la promoción en los registros de eventos de Servicio de directorio, Sistema, Aplicación, Replicación de archivos y Replicación DFS.  

#### <a name="disabling-dsrm-boot"></a>Deshabilitar el arranque en modo DSRM  
Después de arrancar en modo DSRM debido a un error, diagnostica su causa y, si el archivo dcpromo.log no indica que no se pudo reintentar la clonación, corrige la causa y restablece el indicador DSRM. Un clon con error no vuelve al modo normal por sí mismo en el siguiente arranque; debes quitar el indicador de arranque en Modo de restauración de servicios de directorio para volver a intentar la clonación de nuevo. Todos estos pasos requieren la ejecución como administrador con permisos elevados.  

##### <a name="removing-dsrm-with-msconfigexe"></a>Quitar DSRM con Msconfig.exe  
Para desactivar el arranque DSRM usando una GUI, usa la herramienta Configuración del sistema:  

1.  Ejecuta msconfig.exe  

2.  En la pestaña **Arranque**, en **Opciones de arranque**, desactiva **Arranque a prueba de errores** (ya está activada con la opción **Reparar Active Directory** habilitada).  

3.  Haz clic en Aceptar y reinicia cuando se te pida.  

##### <a name="removing-dsrm-with-bcdeditexe"></a>Quitar DSRM con Bcdedit.exe  
Para desactivar el arranque DSRM desde la línea de comandos, usa el editor del almacén de datos de la configuración de arranque:  

1.  Abre un símbolo el sistema CMD y ejecuta:  

    ```  
    Bcdedit.exe /deletevalue safeboot  
    ```  

2.  Reinicia el equipo con:  

    ```  
    Shutdown.exe /t /0 /r  
    ```  

> [!NOTE]  
> Bcdedit.exe también funciona en una consola de Windows PowerShell. En ella, los comandos son:  
>   
> Bcdedit.exe /deletevalue safeboot  
>   
> Restart-computer  

### <a name="BKMK_ServerCoreEvents"></a>Server Core y el registro de eventos  
Los registros de eventos contienen mucha información útil sobre las operaciones de clonación de controladores de dominio virtualizados. De forma predeterminada, la instalación de un equipo de Windows Server 2012 es una instalación Server Core, lo que significa que no hay una interfaz gráfica y, por lo tanto, no hay manera de ejecutar el complemento Visor de eventos local.  

Para revisar los registros de eventos en un servidor que ejecuta una instalación Server Core:  

-   Ejecuta localmente la herramienta Wevtutil.exe  

-   Ejecuta localmente el cmdlet de PowerShell Get-WinEvent  

-   Si ha habilitado las reglas de Firewall avanzado de Windows para los grupos "administración remota de registros de eventos" (o puertos equivalentes) para permitir la comunicación entrante, puede administrar el registro de eventos de forma remota mediante eventvwr. exe, wevtutil. exe o get-WinEvent. Esto se puede hacer en la instalación Server Core mediante NETSH.exe, con la directiva de grupo o con el nuevo cmdlet Set-NetFirewallRule en Windows PowerShell 3.0.  

> [!WARNING]  
> No intentes volver a agregar el shell gráfico al equipo mientras esté en DSRM. La pila de servicio de Windows (CBS) no puede funcionar correctamente en modo seguro o DSRM. Los intentos de agregar características o roles mientras se está en modo DSRM no se completarán y dejarán el equipo en un estado inestable hasta que se arranque normalmente. Como un clon de un controlador de dominio virtualizado en DSRM no puede arrancar normalmente, y no se debe arrancar normalmente en la mayoría de los casos, es imposible agregar el shell gráfico de forma segura. No se permite hacerlo y podría dejar el servidor inservible.  

### <a name="BKMK_SpecificProblems"></a>Solución de problemas específicos  

#### <a name="events"></a>Eventos  
Todos los eventos de clonación de controladores de dominio virtualizados se escriben en el registro de eventos de Servicios de directorio de la máquina virtual del controlador de dominio clonado. Los registros de eventos de Aplicación, Servicio de replicación de archivos y Replicación DFS pueden contener también información útil para solucionar problemas de clonación. Los errores durante la llamada de RPC al emulador de PDC pueden estar disponibles en el registro de eventos del emulador de PDC.  

A continuación se indican los eventos específicos de clonación de Windows Server 2012 en el registro de eventos de Servicios de directorio, con notas y soluciones recomendadas.  

##### <a name="directory-services-event-log"></a>Registro de eventos de Servicios de directorio  

|                          |                                                                                                                                                                                                                                                                                                                                                                                                                            |
|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                                                                                                          **2160**                                                                                                                                                                                                          |
|        **Origen**        |                                                                                                                                                                                      Microsoft-Windows-ActiveDirectory_DomainService                                                                                                                                                                                       |
|       **Gravedad**       |                                                                                                                                                                                                       Informativo                                                                                                                                                                                                        |
|       **Mensaje**        | El *<COMPUTERNAME>* local ha encontrado un archivo de configuración de clonación del controlador de dominio virtual.<br /><br />El archivo de configuración de clonación de controladores de dominio virtuales se encuentra en: %1<br /><br />La existencia del archivo de configuración de clonación de controladores de dominio virtuales indica que el controlador de dominio virtual local es un clon de otro controlador de dominio virtual. El *<COMPUTERNAME>* comenzará a clonarse a sí mismo. |
| **Notas y resolución** |                                                                                                                  Este es un evento de procedimiento correcto y solo es un problema si es imprevisto. Busca el archivo dcclconeconfig.xml en el directorio de trabajo de DSA, %systemroot%\ntds, y en el directorio raíz de todos los discos locales o extraíbles.                                                                                                                  |

|                          |                                                                                                                                                                                          |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                         **2161**                                                                                         |
|        **Origen**        |                                                                     Microsoft-Windows-ActiveDirectory_DomainService                                                                      |
|       **Gravedad**       |                                                                                      Informativo                                                                                       |
|       **Mensaje**        |                         El *<COMPUTERNAME>* local no encontró el archivo de configuración de clonación del controlador de dominio virtual. La máquina local no es un controlador de dominio clonado.                          |
| **Notas y resolución** | Este es un evento de procedimiento correcto y solo es un problema si es imprevisto. Busca el archivo dcclconeconfig.xml en el directorio de trabajo de DSA, %systemroot%\ntds, y en el directorio raíz de todos los discos locales o extraíbles. |

|||  
|-|-|  
|**ID. de evento**|**2162**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|Error de clonación del controlador de dominio virtual.<br /><br />Comprueba los eventos registrados en los registros de eventos del sistema y %systemroot%\debug\dcpromo.log para obtener más información sobre los errores correspondientes al intento de clonación del controlador de dominio virtual.<br /><br />Código de error: %1|  
|**Notas y resolución**|Sigue las instrucciones del mensaje; este error es un CatchAll.|  

|||  
|-|-|  
|**ID. de evento**|**2163**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|Se inició el servicio DsRoleSvc para clonar el controlador de dominio virtual local.|  
|**Notas y resolución**|Este es un evento de procedimiento correcto y solo es un problema si es imprevisto. Busca el archivo dcclconeconfig.xml en el directorio de trabajo de DSA, %systemroot%\ntds, y en el directorio raíz de todos los discos locales o extraíbles.|  

|                          |                                                                                                                                                                                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                             **2164**                                                                                              |
|        **Origen**        |                                                                          Microsoft-Windows-ActiveDirectory_DomainService                                                                          |
|       **Gravedad**       |                                                                                               Error                                                                                               |
|       **Mensaje**        |                                               *<COMPUTERNAME>* no pudo iniciar el servicio DsRoleSvc para clonar el controlador de dominio virtual local.                                                |
| **Notas y resolución** | Examina la configuración del servicio Servidor de roles de DS (DsRoleSvc) y asegúrate de que el tipo de inicio es manual. Comprueba que ningún programa de terceros esté impidiendo el inicio del servicio. |

|                          |                                                                                                                                                                                     |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                      **2165**                                                                                       |
|        **Origen**        |                                                                   Microsoft-Windows-ActiveDirectory_DomainService                                                                   |
|       **Gravedad**       |                                                                                        Error                                                                                        |
|       **Mensaje**        | *<COMPUTERNAME>* no pudo iniciar un subproceso durante la clonación del controlador de dominio virtual local.<br /><br />Código de error:%1<br /><br />Mensaje de error:%2<br /><br />Nombre de subproceso:%3 |
| **Notas y resolución** |                                                                          Ponte en contacto con el soporte técnico de Microsoft.                                                                          |

|                          |                                                                                                                                                             |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                          **2166**                                                                           |
|        **Origen**        |                                                       Microsoft-Windows-ActiveDirectory_DomainService                                                       |
|       **Gravedad**       |                                                                            Error                                                                            |
|       **Mensaje**        | *<COMPUTERNAME>* necesita el servicio RPCSS para iniciar el reinicio en DSRM. Error en la espera de RPCSS para inicializar en un estado de ejecución.<br /><br />Código de error:%1 |
| **Notas y resolución** |                                    Examina el registro de eventos de Sistema y la configuración del servicio Servidor RPC (Rpcss).                                     |

|                          |                                                                                                                                                                            |
|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                  **2167**                                                                                  |
|        **Origen**        |                                                              Microsoft-Windows-ActiveDirectory_DomainService                                                               |
|       **Gravedad**       |                                                                                   Error                                                                                    |
|       **Mensaje**        | *<COMPUTERNAME>* no pudo inicializar el conocimiento del controlador de dominio virtual. Para obtener más información, consulta las entradas anteriores del registro de eventos.<br /><br />Datos adicionales<br /><br />Código de error:%1 |
| **Notas y resolución** |                                                           Sigue las instrucciones del mensaje; este error es un CatchAll.                                                           |

|||  
|-|-|  
|**ID. de evento**|**2168**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|Microsoft-Windows-ActiveDirectory_DomainService<br /><br />El controlador de dominio se está ejecutando en un hipervisor admitido. Se detectó el identificador de generación de VM.<br /><br />Valor actual del identificador de generación de VM: %1|  
|**Notas y resolución**|Este es un evento de procedimiento correcto y solo es un problema si es imprevisto.|  

|||  
|-|-|  
|**ID. de evento**|**2169**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|No se detectó un identificador de generación de VM. El controlador de dominio está hospedado en una máquina física, una versión de nivel inferior de Hyper-V, o un hipervisor que no es compatible con el identificador de generación de VM.<br /><br />Datos adicionales<br /><br />Código de error devuelto al comprobar el identificador de generación de VM:%1|  
|**Notas y resolución**|Este es un evento de procedimiento correcto si el objetivo no es la clonación. De lo contrario, examina el registro de eventos de Sistema y revisa la documentación de soporte técnico del hipervisor.|  

|                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                                                                                                                                                                                                                                                              **2170**                                                                                                                                                                                                                                                                                                                                                              |
|        **Origen**        |                                                                                                                                                                                                                                                                                                                                          Microsoft-Windows-ActiveDirectory_DomainService                                                                                                                                                                                                                                                                                                                                           |
|       **Gravedad**       |                                                                                                                                                                                                                                                                                                                                                              Advertencia                                                                                                                                                                                                                                                                                                                                                               |
|       **Mensaje**        | Se detectó un cambio de id. de generación.<br /><br />Id. de generación almacenado en caché en DS (valor antiguo):%1<br /><br />Id. de generación actualmente en VM (valor nuevo):%2<br /><br />El cambio de id. de generación se produce después de la aplicación de una instantánea de máquina virtual, después de una operación de importación de máquina virtual o después de una operación de migración en vivo. *<COMPUTERNAME>* creará un nuevo identificador de invocación para recuperar el controlador de dominio. Los controladores de dominio virtualizados no deben restaurarse con instantáneas de máquina virtual. El método admitido para restaurar o revertir el contenido de una base de datos de Active Directory Domain Services consiste en restaurar una copia de seguridad del estado del sistema realizada con una aplicación de copia de seguridad compatible con Active Directory Domain Services. |
| **Notas y resolución** |                                                                                                                                                                                                                                                                                                                      Este es un evento de procedimiento correcto si el objetivo es la clonación. De lo contrario, examina el registro de eventos de Sistema.                                                                                                                                                                                                                                                                                                                       |

|||  
|-|-|  
|**ID. de evento**|**2171**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|No se detectó ningún cambio de identificador de generación.<br /><br />Id. de generación almacenado en caché en DS (valor antiguo):%1<br /><br />Id. de generación actualmente en VM (valor nuevo):%2|  
|**Notas y resolución**|Este es un evento de procedimiento correcto si el objetivo no es la clonación, y debería verse cada vez que se reinicia un controlador de dominio. De lo contrario, examina el registro de eventos de Sistema.|  

|||  
|-|-|  
|**ID. de evento**|**2172**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|Se leyó el atributo msDS-GenerationId del objeto de equipo del controlador de dominio.<br /><br />Valor de atributo de msDS-GenerationId:%1|  
|**Notas y resolución**|Este es un evento de procedimiento correcto si el objetivo es la clonación. De lo contrario, examina el registro de eventos de Sistema.|  

|||  
|-|-|  
|**ID. de evento**|**2173**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|No se puede leer el atributo msDS-GenerationId del objeto de equipo del controlador de dominio. Esto puede deberse a un error de transacción de la base de datos o a que no existe el identificador de generación en la base de datos local. El atributo msDS-GenerationId no existe durante el primer reinicio después de ejecutar dcpromo o el controlador de dominio no es un controlador de dominio virtual.<br /><br />Datos adicionales<br /><br />Código de error:%1|  
|**Notas y resolución**|Este es un evento de procedimiento correcto si el objetivo es la clonación y es el primer reinicio de la VM una vez completada la clonación. Se puede pasar por alto en controladores de dominio no virtuales. De lo contrario, examina el registro de eventos de Sistema.|  

|||  
|-|-|  
|**ID. de evento**|**2174**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|El controlador de dominio no es un clon de controlador de dominio virtual ni una instantánea de un controlador de dominio virtual restaurado.|  
|**Notas y resolución**|Este es un evento de procedimiento correcto si el objetivo no es la clonación. De lo contrario, examina el registro de eventos de Sistema.|  

|||  
|-|-|  
|**ID. de evento**|**2175**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|Hay un archivo de configuración de clonación del controlador de dominio virtual en una plataforma no admitida.|  
|**Notas y resolución**|Esto sucede cuando se encuentra un archivo dccloneconfig.xml pero no un id. de generación de máquina virtual, por ejemplo, cuando se encuentra un archivo dccloneconfig.xml en un equipo físico o en un hipervisor que no admite un id. de generación de máquina virtual.|  

|||  
|-|-|  
|**ID. de evento**|**2176**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|Se cambió el nombre del archivo de configuración del clon de controlador de dominio virtual.<br /><br />Datos adicionales<br /><br />Nombre de archivo anterior:%1<br /><br />Nombre de archivo nuevo:%2|  
|**Notas y resolución**|Se espera un cambio de nombre al arrancar una copia de seguridad de una VM de origen, porque el identificador de generación de VM no ha cambiado. Esto impide que el controlador de dominio de origen intente clonarse.|  

|||  
|-|-|  
|**ID. de evento**|**2177**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|Error al cambiar el nombre del archivo de configuración de clonación del controlador de dominio virtual.<br /><br />Datos adicionales<br /><br />Nombre de archivo:%1<br /><br />Código de error:%2 %3|  
|**Notas y resolución**|Se espera un intento de cambio de nombre al arrancar una copia de seguridad de una VM de origen, porque el identificador de generación de VM no ha cambiado. Esto impide que el controlador de dominio de origen intente clonarse. Cambia el nombre del archivo manualmente e investiga los productos de terceros instalados que pudieran impedir el cambio de nombre.|  

|||  
|-|-|  
|**ID. de evento**|**2178**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|Se detectó el archivo de configuración del clon de controlador de dominio virtual, pero no se cambió el identificador de generación de VM. El controlador de dominio local es el controlador de dominio de origen del clon. Cambie el nombre del archivo de configuración del clon.|  
|**Notas y resolución**|Es previsible cuando se arranca una copia de seguridad de una VM de origen, porque el identificador de generación de VM no ha cambiado. Esto impide que el controlador de dominio de origen intente clonarse.|  

|||  
|-|-|  
|**ID. de evento**|**2179**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|El atributo msDS-GenerationId del objeto de equipo del controlador de dominio se estableció en el parámetro siguiente:<br /><br />Atributo GenerationID:%1|  
|**Notas y resolución**|Este es un evento de procedimiento correcto y solo es un problema si es imprevisto.|  

|||  
|-|-|  
|**ID. de evento**|**2180**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Advertencia|  
|**Mensaje**|No se pudo establecer el atributo msDS-GenerationId del objeto de equipo del controlador de dominio.<br /><br />Datos adicionales<br /><br />Código de error:%1|  
|**Notas y resolución**|Examina el registro de eventos de Sistema y Dcpromo.log. Busca el error específico en MS TechNet, MS Knowledgebase y en los blogs de MS para determinar su significado habitual y, después, soluciona los problemas en función de esos resultados.|  

|||  
|-|-|  
|**ID. de evento**|**2182**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|Evento interno: se ha solicitado al servicio de directorio que Clone un DSA remoto:|  
|**Notas y resolución**|Este es un evento de procedimiento correcto y solo es un problema si es imprevisto.|  

|                          |                                                                                                                                                                                                                                                                   |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                             **2183**                                                                                                                              |
|        **Origen**        |                                                                                                          Microsoft-Windows-ActiveDirectory_DomainService                                                                                                          |
|       **Gravedad**       |                                                                                                                           Informativo                                                                                                                           |
|       **Mensaje**        | Evento interno: *<COMPUTERNAME>* completó la solicitud para clonar el agente de sistema de directorio remoto.<br /><br />Nombre de DC original:%3<br /><br />Nombre de DC clonado solicitado:%4<br /><br />Sitio de DC clonado solicitado:%5<br /><br />Datos adicionales<br /><br />Valor del error:%1 %2 |
| **Notas y resolución** |                                                                                                     Este es un evento de procedimiento correcto y solo es un problema si es imprevisto.                                                                                                      |

|                          |                                                                                                                                                                                                                                                                                                  |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                                             **2184**                                                                                                                                             |
|        **Origen**        |                                                                                                                         Microsoft-Windows-ActiveDirectory_DomainService                                                                                                                          |
|       **Gravedad**       |                                                                                                                                              Error                                                                                                                                               |
|       **Mensaje**        | *<COMPUTERNAME>* no pudo crear una cuenta de controlador de dominio para el DC clonado.<br /><br />Nombre de DC original: %1<br /><br />Número permitido de DC clonados:%2<br /><br />Se superó el límite en el número de cuentas de controlador de dominio que se pueden generar mediante la clonación <em><COMPUTERNAME></em>. |
| **Notas y resolución** |              Un único nombre de controlador de dominio de origen solo se puede generar automáticamente 9999 veces si los controladores de dominio no se reducen de nivel, de acuerdo con la convención de nombres. Use el elemento <computername> del código XML para generar un nuevo nombre único o un clon de un controlador de dominio con un nombre diferente.              |

|                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                                                                                                                                                                **2191**                                                                                                                                                                                                                                                                |
|        **Origen**        |                                                                                                                                                                                                                                            Microsoft-Windows-ActiveDirectory_DomainService                                                                                                                                                                                                                                             |
|       **Gravedad**       |                                                                                                                                                                                                                                                             Informativo                                                                                                                                                                                                                                                              |
|       **Mensaje**        | *<COMPUTERNAME>* establecer el siguiente valor del registro para deshabilitar las actualizaciones de DNS.<br /><br />Clave del Registro:%1<br /><br />Valor del Registro: %2<br /><br />Datos del valor del Registro: %3<br /><br />Durante el proceso de clonación, la máquina local podría tener durante un corto período de tiempo el mismo nombre de equipo que la máquina de origen clonada. Los registros A y AAAA de DNS están deshabilitados durante este período para que los clientes no puedan enviar solicitudes a la máquina local que se está clonando. El proceso de clonación habilitará las actualizaciones de DNS de nuevo una vez que finalice la clonación. |
| **Notas y resolución** |                                                                                                                                                                                                                                        Este es un evento de procedimiento correcto y solo es un problema si es imprevisto.                                                                                                                                                                                                                                        |

|                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                                                                                                                                                        **2192**                                                                                                                                                                                                                                                         |
|        **Origen**        |                                                                                                                                                                                                                                     Microsoft-Windows-ActiveDirectory_DomainService                                                                                                                                                                                                                                     |
|       **Gravedad**       |                                                                                                                                                                                                                                                          Error                                                                                                                                                                                                                                                          |
|       **Mensaje**        | *<COMPUTERNAME>* no pudo establecer el siguiente valor del registro para deshabilitar las actualizaciones de DNS.<br /><br />Clave del Registro:%1<br /><br />Valor del Registro: %2<br /><br />Datos del valor del Registro: %3<br /><br />Código de error: % 4<br /><br />Mensaje de error: %5<br /><br />Durante el proceso de clonación, la máquina local podría tener durante un corto período de tiempo el mismo nombre de equipo que la máquina de origen clonada. Los registros A y AAAA de DNS están deshabilitados durante este período para que los clientes no puedan enviar solicitudes a la máquina local que se está clonando. |
| **Notas y resolución** |                                                                                                                                                                                                  Examina los registros de eventos de aplicación y del sistema. Investiga aplicaciones de terceros que pudieran estar bloqueando las actualizaciones del Registro.                                                                                                                                                                                                  |

|                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                                                                                                                        **2193**                                                                                                                                                                                                                         |
|        **Origen**        |                                                                                                                                                                                                     Microsoft-Windows-ActiveDirectory_DomainService                                                                                                                                                                                                     |
|       **Gravedad**       |                                                                                                                                                                                                                      Informativo                                                                                                                                                                                                                      |
|       **Mensaje**        | *<COMPUTERNAME>* establecer el siguiente valor del registro para habilitar las actualizaciones de DNS.<br /><br />Clave del Registro:%1<br /><br />Valor del Registro: %2<br /><br />Datos del valor del Registro: %3<br /><br />Durante el proceso de clonación, la máquina local podría tener durante un corto período de tiempo el mismo nombre de equipo que la máquina de origen clonada. Los registros A y AAAA de DNS están deshabilitados durante este período para que los clientes no puedan enviar solicitudes a la máquina local que se está clonando. |
| **Notas y resolución** |                                                                                                                                                                                                Este es un evento de procedimiento correcto y solo es un problema si es imprevisto.                                                                                                                                                                                                 |

|                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                                                                                                                                                        **2194**                                                                                                                                                                                                                                                        |
|        **Origen**        |                                                                                                                                                                                                                                    Microsoft-Windows-ActiveDirectory_DomainService                                                                                                                                                                                                                                     |
|       **Gravedad**       |                                                                                                                                                                                                                                                         Error                                                                                                                                                                                                                                                          |
|       **Mensaje**        | *<COMPUTERNAME>* no pudo establecer el siguiente valor del registro para habilitar las actualizaciones de DNS.<br /><br />Clave del Registro:%1<br /><br />Valor del Registro: %2<br /><br />Datos del valor del Registro: %3<br /><br />Código de error: % 4<br /><br />Mensaje de error: %5<br /><br />Durante el proceso de clonación, la máquina local podría tener durante un corto período de tiempo el mismo nombre de equipo que la máquina de origen clonada. Los registros A y AAAA de DNS están deshabilitados durante este período para que los clientes no puedan enviar solicitudes a la máquina local que se está clonando. |
| **Notas y resolución** |                                                                                                                                                                                                 Examina los registros de eventos de aplicación y del sistema. Investiga aplicaciones de terceros que pudieran estar bloqueando las actualizaciones del Registro.                                                                                                                                                                                                  |

|||  
|-|-|  
|**ID. de evento**|**2195**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|No se pudo establecer el modo de arranque de DSRM.<br /><br />Código de error:%1<br /><br />Mensaje de error:%2<br /><br />Cuando se produzca un error de clonación del controlador de dominio virtual o cuando aparezca el archivo de configuración de clonación del controlador de dominio virtual en un hipervisor no admitido, la máquina local se reiniciará en DSRM para solucionar los problemas. Error al establecer el arranque de DSRM.|  
|**Notas y resolución**|Examina los registros de eventos de aplicación y del sistema. Investiga aplicaciones de terceros que pudieran estar bloqueando las actualizaciones del Registro.|  

|||  
|-|-|  
|**ID. de evento**|**2196**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|No se pudo establecer el privilegio de apagado.<br /><br />Código de error:%1<br /><br />Mensaje de error:%2<br /><br />Cuando se produzca un error de clonación del controlador de dominio virtual o cuando aparezca el archivo de configuración de clonación del controlador de dominio virtual en un hipervisor no admitido, la máquina local se reiniciará en DSRM para solucionar los problemas. Error al habilitar el privilegio de apagado.|  
|**Notas y resolución**|Examina los registros de eventos de aplicación y del sistema. Investiga aplicaciones de terceros que pudieran estar bloqueando el uso de privilegios.|  

|||  
|-|-|  
|**ID. de evento**|**2197**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|No se pudo iniciar el apagado del sistema.<br /><br />Código de error:%1<br /><br />Mensaje de error:%2<br /><br />Cuando se produzca un error de clonación del controlador de dominio virtual o cuando aparezca el archivo de configuración de clonación del controlador de dominio virtual en un hipervisor no admitido, la máquina local se reiniciará en DSRM para solucionar los problemas. Error al iniciar el apagado del sistema.|  
|**Notas y resolución**|Examina los registros de eventos de aplicación y del sistema. Investiga aplicaciones de terceros que pudieran estar bloqueando el uso de privilegios.|  

|                          |                                                                                                                                                                                   |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                     **2198**                                                                                      |
|        **Origen**        |                                                                  Microsoft-Windows-ActiveDirectory_DomainService                                                                  |
|       **Gravedad**       |                                                                                       Error                                                                                       |
|       **Mensaje**        | *<COMPUTERNAME>* no pudo crear o modificar el siguiente objeto de controlador de dominio clonado.<br /><br />Datos adicionales:<br /><br />Objeto:<br /><br />%1<br /><br />Valor del error: %2<br /><br />%3 |
| **Notas y resolución** |               Busca el error específico en MS TechNet, MS Knowledgebase y en los blogs de MS para determinar su significado habitual y, después, soluciona los problemas en función de esos resultados.               |

|                          |                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                                                                                                                   **2199**                                                                                                                                                                                                                   |
|        **Origen**        |                                                                                                                                                                                               Microsoft-Windows-ActiveDirectory_DomainService                                                                                                                                                                                                |
|       **Gravedad**       |                                                                                                                                                                                                                    Error                                                                                                                                                                                                                     |
|       **Mensaje**        |                                                                                                                     *<COMPUTERNAME>* no pudo crear el siguiente objeto de controlador de dominio clonado porque el objeto ya existe.<br /><br />Datos adicionales:<br /><br />Controlador de dominio de origen:<br /><br />%1<br /><br />Objeto:<br /><br />%2                                                                                                                     |
| **Notas y resolución** | Comprueba que el archivo dccloneconfig.xml no especifique un controlador de dominio existente o no se hayan usado copias del archivo dccloneconfig.xml en varios clones sin editar el nombre. Si aún no se prevé ningún conflicto, determina qué administrador lo promovió y ponte en contacto con él para determinar si el controlador de dominio existente debe disminuirse de nivel, si se deben limpiar los metadatos del controlador de dominio existente o si el clon debe usar un nombre diferente. |

|||  
|-|-|  
|**ID. de evento**|**2203**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|Error en la última clonación del controlador de dominio virtual. Este es el primer reinicio desde entonces, por lo que debería ser un reintento de la clonación. Sin embargo, no existe un archivo de configuración del clon del controlador de dominio virtual ni se ha detectado un cambio de identificador de generación de máquina virtual. Arranque en DSRM.<br /><br />Error en la última clonación del controlador de dominio virtual:%1<br /><br />El archivo de configuración de clonación del controlador de dominio virtual existen:%2<br /><br />Se ha detectado un cambio de identificación de generación de máquina virtual:%3|  
|**Notas y resolución**|Es previsible si ya se había producido un error en la clonación, debido a un archivo dccloneconfig.xml que falta o no es válido.|  

|||  
|-|-|  
|Id. de evento|2210|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Error|  
|Mensaje|Error de <COMPUTERNAME> al crear objetos para el controlador de dominio clonado.<br /><br />Datos adicionales:<br /><br />Id. de clon: %6<br /><br />Nombre del controlador de dominio clonado: %1<br /><br />Bucle de reintentos: %2<br /><br />Valor de excepción: %3<br /><br />Valor del error: %4<br /><br />DSID: %5|  
|Notas y resolución|Revisa los registros de eventos de Sistema y Servicios de directorio, así como dcpromo.log, para obtener más información sobre el motivo del error de la clonación.|  

|||  
|-|-|  
|Id. de evento|2211|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> ha creado objetos para el controlador de dominio clonado.<br /><br />Datos adicionales:<br /><br />Id. de clon: %3<br /><br />Nombre del controlador de dominio clonado: %1<br /><br />Bucle de reintentos: %2|  
|Notas y resolución|Este es un evento de procedimiento correcto y solo es un problema si es imprevisto.|  

|||  
|-|-|  
|Id. de evento|2212|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> comenzó a crear objetos para el controlador de dominio clonado.<br /><br />Datos adicionales:<br /><br />Id. de clon: %1<br /><br />Nombre del clon: %2<br /><br />Sitio del clon: %3<br /><br />RODC del clon: %4|  
|Notas y resolución|Este es un evento de procedimiento correcto y solo es un problema si es imprevisto.|  

|||  
|-|-|  
|Id. de evento|2213|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> creó un nuevo objeto KrbTgt para la clonación del controlador de dominio de solo lectura.<br /><br />Datos adicionales:<br /><br />Id. de clon: %1<br /><br />GUID del nuevo objeto KrbTgt: %2|  
|Notas y resolución|Este es un evento de procedimiento correcto y solo es un problema si es imprevisto.|  

|||  
|-|-|  
|Id. de evento|2214|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> creará un objeto de equipo para el controlador de dominio clonado.<br /><br />Datos adicionales:<br /><br />Id. de clon: %1<br /><br />Controlador de dominio original: %2<br /><br />Controlador de dominio clonado: %3|  
|Notas y resolución|Este es un evento de procedimiento correcto y solo es un problema si es imprevisto.|  

|||  
|-|-|  
|Id. de evento|2215|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> agregará el controlador de dominio clonado al sitio siguiente.<br /><br />Datos adicionales:<br /><br />Id. de clon: %1<br /><br />Sitio: %2|  
|Notas y resolución|Este es un evento de procedimiento correcto y solo es un problema si es imprevisto.|  

|||  
|-|-|  
|Id. de evento|2216|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> creará un contenedor de servidores para el controlador de dominio clonado.<br /><br />Datos adicionales:<br /><br />Id. de clon: %1<br /><br />Contenedor de servidores: %2|  
|Notas y resolución|Este es un evento de procedimiento correcto y solo es un problema si es imprevisto.|  

|||  
|-|-|  
|Id. de evento|2217|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> creará un objeto de servidor para el controlador de dominio clonado.<br /><br />Datos adicionales:<br /><br />Id. de clon: %1<br /><br />Objeto de servidor: %2|  
|Notas y resolución|Este es un evento de procedimiento correcto y solo es un problema si es imprevisto.|  

|||  
|-|-|  
|Id. de evento|2218|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> creará un objeto de configuración NTDS para el controlador de dominio clonado.<br /><br />Datos adicionales:<br /><br />Id. de clon: %1<br /><br />Objeto: %2|  
|Notas y resolución|Este es un evento de procedimiento correcto y solo es un problema si es imprevisto.|  

|||  
|-|-|  
|Id. de evento|2219|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> creará objetos de conexión para el controlador de dominio de solo lectura clonado.<br /><br />Datos adicionales:<br /><br />Id. de clon: %1|  
|Notas y resolución|Este es un evento de procedimiento correcto y solo es un problema si es imprevisto.|  

|||  
|-|-|  
|Id. de evento|2220|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> creará objetos SYSVOL para el controlador de dominio de solo lectura clonado.<br /><br />Datos adicionales:<br /><br />Id. de clon: %1|  
|Notas y resolución|Este es un evento de procedimiento correcto y solo es un problema si es imprevisto.|  

|||  
|-|-|  
|Id. de evento|2221|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Error|  
|Mensaje|Error de <COMPUTERNAME> al generar una contraseña aleatoria para el controlador de dominio clonado.<br /><br />Datos adicionales:<br /><br />Id. de clon: %1<br /><br />Nombre del controlador de dominio clonado: %2<br /><br />Error: %3 %4|  
|Notas y resolución|Examina el registro de eventos de Sistema para obtener más información sobre el motivo por el que no se pudo crear la contraseña de la cuenta de la máquina.|  

|||  
|-|-|  
|Id. de evento|2222|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Error|  
|Mensaje|Error de <COMPUTERNAME> al establecer la contraseña para el controlador de dominio clonado.<br /><br />Datos adicionales:<br /><br />Id. de clon: %1<br /><br />Nombre del controlador de dominio clonado: %2<br /><br />Error: %3 %4|  
|Notas y resolución|Examina el registro de eventos de Sistema para obtener más información sobre el motivo por el que no se pudo establecer la contraseña de la cuenta de la máquina.|  

|||  
|-|-|  
|Id. de evento|2223|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> establece correctamente la contraseña de la cuenta de equipo para el controlador de dominio clonado.<br /><br />Datos adicionales:<br /><br />Id. de clon: %1<br /><br />Nombre del controlador de dominio clonado: %2<br /><br />Número total de reintentos: %3|  
|Notas y resolución|Este es un evento de procedimiento correcto y solo es un problema si es imprevisto.|  

|||  
|-|-|  
|Id. de evento|2224|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Error|  
|Mensaje|Error de clonación del controlador de dominio virtual. En el equipo clonado existen las siguientes %1 cuentas de servicio administradas:<br /><br />%2<br /><br />Para que la clonación se realice correctamente, deben quitarse todas las cuentas de servicio administradas. Para ello, usa el cmdlet de PowerShell Remove-ADComputerServiceAccount.|  
|Notas y resolución|Es previsible cuando se usan MSA independientes (no MSA de grupo). *No* sigas el consejo del evento de quitar la cuenta; está escrito incorrectamente. Use Uninstall-AdServiceAccount- [https://technet.microsoft.com/library/hh852310](https://technet.microsoft.com/library/hh852310).<br /><br />MSA independientes: se lanzaron por primera vez en Windows Server 2008 R2 y se reemplazaron en Windows Server 2012 con MSA de grupo (gMSA). Las GMSA admiten la clonación.|  

|||  
|-|-|  
|Id. de evento|2225|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|Los secretos en caché de la siguiente entidad de seguridad se han quitado correctamente del controlador de dominio local:<br /><br />%1<br /><br />Después de clonar un controlador de dominio de solo lectura, los secretos previamente guardados en caché en el controlador de dominio de solo lectura origen de la clonación se quitarán del controlador de dominio clonado.|  
|Notas y resolución|Este es un evento de procedimiento correcto y solo es un problema si es imprevisto.|  

|||  
|-|-|  
|Id. de evento|2226|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Error|  
|Mensaje|Error al quitar los secretos en caché de la siguiente entidad de seguridad del controlador de dominio local:<br /><br />%1<br /><br />Error: %2 (%3)<br /><br />Después de clonar un controlador de dominio de solo lectura, los secretos previamente guardados en caché del controlador de dominio de solo lectura origen de la clonación deben eliminarse del clon para reducir el riesgo de que un atacante pueda obtener esas credenciales del clon robado o comprometido. Si la entidad de seguridad es una cuenta con muchos privilegios y debería estar protegida frente a estas amenazas, use la operación de rootDSE rODCPurgeAccount para borrar manualmente sus secretos en el controlador de dominio local.|  
|Notas y resolución|Examina los registros de eventos de Sistema y Servicios de directorio para obtener más información.|  

|||  
|-|-|  
|Id. de evento|2227|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Error|  
|Mensaje|Se produjo una excepción al intentar quitar los secretos en caché del controlador de dominio local.<br /><br />Datos adicionales:<br /><br />Valor de excepción: %1<br /><br />Valor del error: %2<br /><br />DSID: %3<br /><br />Después de clonar un controlador de dominio de solo lectura, los secretos previamente guardados en caché del controlador de dominio de solo lectura origen de la clonación deben eliminarse del clon para reducir el riesgo de que un atacante pueda obtener esas credenciales del clon robado o comprometido. Si alguna de las entidades de seguridad es una cuenta con muchos privilegios y debería estar protegida frente a estas amenazas, usa la operación de rootDSE rODCPurgeAccount para borrar manualmente sus secretos en el controlador de dominio local.|  
|Notas y resolución|Examina los registros de eventos de Sistema y Servicios de directorio para obtener más información.|  

|||  
|-|-|  
|Id. de evento|2228|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Error|  
|Mensaje|El id. de generación de la máquina virtual en la base de datos de Active Directory de este controlador de dominio difiere del valor actual de esta máquina virtual. Sin embargo, no se pudo encontrar un archivo de configuración del clon del controlador de dominio virtual (DCCloneConfig.xml), por lo que no se intentó la clonación del controlador de dominio. Si tu intención era realizar una operación de este tipo, asegúrate de que se proporcione un DCCloneConfig.xml en cualquiera de las ubicaciones admitidas. Asimismo, la dirección IP de este controlador de dominio está en conflicto con la dirección IP de otro controlador de dominio. Para evitar que haya interrupciones en el servicio, el controlador de dominio se ha configurado para arrancar en DSRM.<br /><br />Datos adicionales:<br /><br />Dirección IP duplicada: %1|  
|Notas y resolución|Este mecanismo de protección detiene los controladores de dominio duplicados cuando es posible (no lo hará cuando se usa DHCP, por ejemplo). Agrega un archivo DcCloneConfig.xml válido, quita el indicador DSRM y vuelve a intentar la clonación.|  

|||  
|-|-|  
|Id. de evento|29218|  
|Origen|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravedad|Error|  
|Mensaje|Error de clonación del controlador de dominio virtual. La operación de clonación no se pudo completar y el controlador de dominio clonado se reinició en modo de restauración de servicios de directorio (DSRM).<br /><br />Comprueba los eventos anteriormente registrados y %systemroot%\debug\dcpromo.log para obtener más información sobre los errores correspondientes al intento de clonación del controlador de dominio virtual y si esta imagen clonada puede volver a usarse o no.<br /><br />Si una o más entradas de registro indican que el proceso de clonación no se puede recuperar, la imagen debe destruirse de forma segura. Si los registros indican que el proceso de clonación puede volver a intentarse, soluciona los errores, borra la bandera de inicio de DSRM y reinicia de forma normal. Tras el reinicio, volverá a intentarse la operación de clonación.|  
|Notas y resolución|Revisa los registros de eventos de Sistema y Servicios de directorio, así como dcpromo.log, para obtener más información sobre el motivo del error de la clonación.|  

|||  
|-|-|  
|Id. de evento|29219|  
|Origen|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravedad|Informativo|  
|Mensaje|La clonación del controlador de dominio virtual se realizó correctamente.|  
|Notas y resolución|Este es un evento de procedimiento correcto y solo es un problema si es imprevisto.|  

|||  
|-|-|  
|Id. de evento|29248|  
|Origen|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravedad|Error|  
|Mensaje|La clonación del controlador de dominio virtual no pudo obtener la notificación de Winlogon. El código de error devuelto es %1 (%2).<br /><br />Para obtener más información acerca del error, busca en %systemroot%\debug\dcpromo.log errores correspondientes al intento de clonación del controlador de dominio virtual.|  
|Notas y resolución|Ponte en contacto con el soporte técnico de Microsoft.|  

|||  
|-|-|  
|Id. de evento|29249|  
|Origen|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravedad|Error|  
|Mensaje|La clonación de controlador de dominio virtual no pudo analizar el archivo de configuración de controlador de dominio virtual.<br /><br />El código HRESULT devuelto es %1.<br /><br />El archivo de configuración es:%2<br /><br />Soluciona los errores en el archivo de configuración y vuelve a intentar la operación de clonación.<br /><br />Para obtener más información sobre este error, consulta %systemroot%\debug\dcpromo.log.|  
|Notas y resolución|Comprueba si el archivo dclconeconfig.xml contiene errores de sintaxis usando un editor XML y el archivo de esquema DCCloneConfigSchema.xsd.|  

|||  
|-|-|  
|Id. de evento|29250|  
|Origen|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravedad|Error|  
|Mensaje|Error de clonación del controlador de dominio virtual. Hay software o tareas actualmente habilitados en el controlador de dominio virtual clonado que no están presentes en la lista de aplicaciones permitidas para la clonación del controlador de dominio virtual.<br /><br />A continuación se indican las entradas que faltan:<br /><br />%2<br /><br />%1 (si hay) se usó como la lista de inclusión definida.<br /><br />No se puede completar la operación de clonación si hay aplicaciones instaladas que no pueden clonarse.<br /><br />Ejecuta el cmdlet Get-ADDCCloningExcludedApplicationList de Active Directory PowerShell para comprobar cuáles son las aplicaciones que están instaladas en el equipo clonado, pero que no se incluyen en la lista de aplicaciones permitidas, y agrégalas a dicha lista si son compatibles con la clonación del controlador de dominio virtual. Si alguna de estas aplicaciones no es compatible con la clonación del controlador de dominio virtual, desinstálala antes de volver a intentar la operación de clonación.<br /><br />El proceso de clonación del controlador de dominio virtual busca el archivo con la lista de aplicaciones permitidas, CustomDCCloneAllowList.xml, en el siguiente orden; se utiliza el primer archivo que se encuentra y se omiten los demás:<br /><br />1. el nombre del valor del registro: HKey_Local_Machine \System\CurrentControlSet\Services\NTDS\Parameters\AllowListFolder<br /><br />2. el mismo directorio donde reside la carpeta del directorio de trabajo de DSA<br /><br />3. %windir%\NTDS<br /><br />4. medios extraíbles de lectura/escritura en orden de letra de unidad en la raíz de la unidad|  
|Notas y resolución|Sigue las instrucciones del mensaje.|  

|                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       Id. de evento       |                                                                                                                                                                                                                                                                                                         29251                                                                                                                                                                                                                                                                                                          |
|        Origen        |                                                                                                                                                                                                                                                                                   Microsoft-Windows-DirectoryServices-DSROLE-Server                                                                                                                                                                                                                                                                                    |
|       Gravedad       |                                                                                                                                                                                                                                                                                                         Error                                                                                                                                                                                                                                                                                                          |
|       Mensaje        | La clonación del controlador de dominio virtual no pudo restablecer las direcciones IP de la máquina clonada.<br /><br />El código de error devuelto es %1 (%2).<br /><br />Puede que este error se deba a un error de configuración en las secciones de configuración de red en el archivo de configuración del controlador de dominio virtual.<br /><br />Consulta %systemroot%\debug\dcpromo.log para obtener más información acerca de los errores correspondientes al restablecimiento de direcciones IP durante los intentos de clonación de controladores de dominio virtual.<br /><br />Puede encontrar más información sobre cómo restablecer las direcciones IP de las máquinas en la máquina clonada en https://go.microsoft.com/fwlink/?LinkId=208030 |
| Notas y resolución |                                                                                                                                                                                                                                                  Comprueba que información de las direcciones IP establecida en dccloneconfig.xml sea válida y no duplique la máquina de origen original.                                                                                                                                                                                                                                                   |

|                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       Id. de evento       |                                                                                                                                                                                                                                                                                   29253                                                                                                                                                                                                                                                                                   |
|        Origen        |                                                                                                                                                                                                                                                             Microsoft-Windows-DirectoryServices-DSROLE-Server                                                                                                                                                                                                                                                             |
|       Gravedad       |                                                                                                                                                                                                                                                                                   Error                                                                                                                                                                                                                                                                                   |
|       Mensaje        | Error de clonación del controlador de dominio virtual. El controlador de dominio clonado no pudo encontrar el maestro de operaciones del controlador de dominio principal (PDC) en el dominio principal del equipo clonado de la máquina clonada.<br /><br />El código de error devuelto es %1 (%2).<br /><br />Comprueba que el controlador de dominio principal en el dominio principal de la máquina clonada esté asignado a un controlador de dominio activo, esté en línea y sea operativo. Comprueba que la máquina clonada tenga conectividad LDAP/RPC al controlador de dominio principal en los puertos y protocolos necesarios. |
| Notas y resolución |                                                                                                                                      Comprueba que se haya establecido la información sobre DNS e IP del controlador de dominio clonado. Use DCDiag. exe/test: locatorcheck para validar si el PDCE está en línea, use Nltest. exe/Server: *<PDCE>* /dclist: *<domain>* a un RPC válido, obtener una captura de red del PDCE mientras se produce un error en la clonación y analizar el tráfico.                                                                                                                                       |

|                      |                                                                                                                                                                                                                                                                                                                                                                    |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       Id. de evento       |                                                                                                                                                                               29254                                                                                                                                                                                |
|        Origen        |                                                                                                                                                         Microsoft-Windows-DirectoryServices-DSROLE-Server                                                                                                                                                          |
|       Gravedad       |                                                                                                                                                                               Error                                                                                                                                                                                |
|       Mensaje        | La clonación del controlador de dominio virtual no pudo enlazar con el controlador de dominio principal %1.<br /><br />El código de error devuelto es %2 (%3).<br /><br />Comprueba que el controlador de dominio principal %1 esté en línea y sea operativo. Comprueba que la máquina clonada tenga conectividad LDAP/RPC al controlador de dominio principal en los puertos y protocolos necesarios. |
| Notas y resolución |                                   Comprueba que se haya establecido la información sobre DNS e IP del controlador de dominio clonado. Use DCDiag. exe/test: locatorcheck para validar si el PDCE está en línea, use Nltest. exe/Server: *<PDCE>* /dclist: *<domain>* a un RPC válido, obtener una captura de red del PDCE mientras se produce un error en la clonación y analizar el tráfico.                                   |

|||  
|-|-|  
|Id. de evento|29255|  
|Origen|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravedad|Error|  
|Mensaje|Error de clonación del controlador de dominio virtual.<br /><br />Un intento de crear objetos en el controlador de dominio principal %1 necesario para clonar la imagen devolvió el error %2 (%3).<br /><br />Comprueba que el controlador de dominio clonado tiene privilegios para clonarse a sí mismo. Comprueba los eventos relacionados en el registro de eventos de Servicios de directorio en el controlador de dominio principal %1.|  
|Notas y resolución|Busca el error específico en MS TechNet, MS Knowledgebase y en los blogs de MS para determinar su significado habitual y, después, soluciona los problemas en función de esos resultados.|  

|||  
|-|-|  
|Id. de evento|29256|  
|Origen|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravedad|Error|  
|Mensaje|Error al intentar establecer la marca de arranque en el Modo de restauración de servicios de directorio %1.<br /><br />Consulta %systemroot%\debug\dcpromo.log para obtener más información sobre los errores.|  
|Notas y resolución|Examina el registro de Servicios de directorio y dcpromo.log para obtener más información. Examina los registros de eventos de aplicación y del sistema. Investiga aplicaciones de terceros que pudieran estar bloqueando el uso de privilegios.|  

|||  
|-|-|  
|Id. de evento|29257|  
|Origen|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravedad|Error|  
|Mensaje|Se ha realizado la clonación del controlador de dominio virtual. Error al intentar reiniciar el equipo. Código de error %1.<br /><br />Reinicia el equipo para finalizar la operación de clonación.|  
|Notas y resolución|Examina los registros de eventos de aplicación y del sistema. Investiga aplicaciones de terceros que pudieran estar bloqueando el uso de privilegios.|  

|||  
|-|-|  
|Id. de evento|29264|  
|Origen|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravedad|Error|  
|Mensaje|Error al intentar borrar la marca de arranque en el Modo de restauración de servicios de directorio. Código de error: %1.<br /><br />Consulta %systemroot%\debug\dcpromo.log para obtener más información sobre los errores.|  
|Notas y resolución|Examina el registro de Servicios de directorio y dcpromo.log para obtener más información. Examina los registros de eventos de aplicación y del sistema. Investiga aplicaciones de terceros que pudieran estar bloqueando el uso de privilegios.|  

|||  
|-|-|  
|Id. de evento|29265|  
|Origen|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravedad|Informativo|  
|Mensaje|La clonación del controlador de dominio virtual se realizó correctamente. Se ha cambiado el nombre del archivo de configuración de clonación del controlador de dominio virtual %1 a %2.|  
|Notas y resolución|N/D. Este es un evento de procedimiento correcto.|  

|||  
|-|-|  
|Id. de evento|29266|  
|Origen|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravedad|Error|  
|Mensaje|La clonación del controlador de dominio virtual se realizó correctamente. Error al intentar cambiar el nombre del archivo de configuración de clonación del controlador de dominio virtual %1. Código de error: %2 (%3).|  
|Notas y resolución|Cambia manualmente el nombre del archivo dccloneconfig.xml.|  

|||  
|-|-|  
|Id. de evento|29267|  
|Origen|Microsoft-Windows-DirectoryServices-DSROLE-Server|  
|Gravedad|Error|  
|Mensaje|Error del controlador de dominio virtual al comprobar la lista de aplicaciones con permiso para clonar el controlador de dominio virtual.<br /><br />El código de error devuelto es %1 (%2).<br /><br />La causa podría ser un error de sintaxis en el archivo de lista de permitidos para clonar (el archivo que se está comprobando actualmente es: %3). Para obtener más información sobre este error, consulta %systemroot%\debug\dcpromo.log.|  
|Notas y resolución|Sigue las instrucciones del evento.|  

##### <a name="error-messages"></a>Mensajes de error  
No hay errores interactivos directos para la clonación incorrecta de controladores de dominio virtualizados; toda la información sobre la clonación se registra en los registros de Sistema y de Servicios de directorio, y la promoción de controladores de dominio se registra en dcpromo.log. Sin embargo, si el servidor arranca en el modo de restauración de DS, investiga inmediatamente porque se ha producido un error de promoción o de clonación.  

El registro dcpromo.log es el primer lugar donde se comprueban los errores de clonación. Según el error indicado, puede ser necesario revisar después los registros de Sistema y de Servicios de directorio para continuar el diagnóstico.  

#### <a name="known-issues-and-support-scenarios"></a>Problemas conocidos y escenarios de soporte  
Los siguientes son problemas habituales que se observan durante el proceso de desarrollo de Windows Server 2012. Todos ellos son problemas debidos al diseño, y tienen una solución válida o una técnica más apropiada para evitar que se produzcan. Algunos podrían resolverse en versiones posteriores de Windows Server 2012.  

|||  
|-|-|  
|**Problema**|**Error de clonación, DSRM**|  
|**Síntomas**|El clon arranca en el modo de restauración de servicios de directorio.|  
|**Resolución y notas**|Valida todos los pasos seguidos en las secciones Implementar un controlador de dominio virtualizado y [Metodología general para solucionar problemas de clonación de controladores de dominio](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)<br /><br />Descrito en KB 2742844.|  

|||  
|-|-|  
|**Problema**|**Concesiones de IP adicionales al usar DHCP para clonar**|  
|**Síntomas**|Después de clonar correctamente un controlador de dominio y de usar DHCP, el primer arranque del clon toma una concesión de DHCP. Después, cuando se cambia el nombre del servidor y se reinicia como un controlador de dominio, toma una segunda concesión de DHCP. La primera dirección IP no se libera y termina con una concesión "fantasma".|  
|**Resolución y notas**|Elimina manualmente la concesión de la dirección no utilizada en DHCP o deja que expire normalmente. Descrito en KB 2742836.|  

|||  
|-|-|  
|**Problema**|**No se puede realizar la clonación en DSRM después de un retraso muy largo**|  
|**Síntomas**|La clonación parece pausarse en "Clonación del controlador de dominio completada al X%" de 8 a 15 minutos. Después, se produce un error de clonación y arranca en DSRM.|  
|**Resolución y notas**|El equipo clonado no puede obtener una dirección IP dinámica de DHCP o SLAAC, o está usando una dirección IP duplicada, o no encuentra el PDC. Los múltiples reintentos que realiza la clonación provocan el retraso. Resuelve el problema de red para permitir la clonación.<br /><br />Descrito en KB 2742844.|  

|||  
|-|-|  
|**Problema**|**La clonación no vuelve a crear todos los nombres principales de servicio**|  
|**Síntomas**|Si un conjunto de nombres de entidad de seguridad de servicio (SPN) en *tres partes* incluye un nombre NetBIOS con un puerto y otro nombre NetBIOS idéntico salvo que sin puerto, la entrada sin puerto no se vuelve a crear con el nuevo nombre de equipo. Por ejemplo:<br /><br />customspn/DC1:200/app1 INVALID USE OF SYMBOLS *se vuelve a crear con el nuevo nombre de equipo*<br /><br />customspn/DC1/app1 INVALID USE OF SYMBOLS *no se vuelve a crear con el nuevo nombre de equipo*<br /><br />Los nombres completos se vuelven a crear así como los SPN que no tienen tres partes, independientemente de los puertos. Por ejemplo, estos se vuelven a crear correctamente en el clon:<br /><br />customspn/DC1:202 INVALID USE OF SYMBOLS *se vuelve a crear*<br /><br />customspn/DC1 INVALID USE OF SYMBOLS *se vuelve a crear*<br /><br />customspn/DC1.corp.contoso.com:202 INVALID USE OF SYMBOLS *este es el nombre que se ha vuelto a crear*<br /><br />customspn/DC1.corp.contoso.com INVALID USE OF SYMBOLS *se vuelve a crear*|  
|**Resolución y notas**|Esta es una limitación del proceso de cambio de nombre de los controladores de dominio en Windows, no solo en la clonación. La lógica de cambio de nombre no trata los SPN con tres partes en ningún escenario. La mayoría de los servicios incluidos en Windows no se ven afectados, porque vuelven a crear los SPN que faltan según sea necesario. Otras aplicaciones pueden necesitar que se especifique manualmente el SPN para solucionar el problema.<br /><br />Descrito en KB 2742874.|  

|||  
|-|-|  
|**Problema**|**Se produce un error en la clonación, arranca en DSRM, errores de red generales**|  
|**Síntomas**|El clon arranca en el modo de reparación de servicios de directorio. Hay errores de red generales.|  
|Solución y notas|Asegúrate de que el controlador de dominio de origen no haya asignado una dirección MAC estática duplicada al nuevo clon; para ver si una máquina virtual usa direcciones MAC estáticas, ejecuta este comando en el host del hipervisor tanto para la máquina virtual de origen como para la clonada:<br /><br />Get-VM-VMName *Test-VM* &#124; Get-VMNetworkAdapter &#124; FL *<br /><br />Cambia la dirección MAC por una dirección estática única o cambia para usar direcciones MAC dinámicas.<br /><br />Descrito en KB 2742844|  

|                          |                                                                                                                                                                                                                                                                                                                                                                 |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        **Problema**         |                                                                                                                                               **Se produce un error en la clonación, arranca en DSRM como un duplicado del DC de origen**                                                                                                                                                |
|       **Síntomas**       |                                     Un nuevo clon arranca sin clonación. El archivo dccloneconfig.xml no cambia de nombre y el servidor se inicia en el modo de restauración de DS. El registro de eventos de Servicios de directorio muestra el error 2164.<br /><br />*<COMPUTERNAME>* no pudo iniciar el servicio DsRoleSvc para clonar el controlador de dominio virtual local.                                      |
| **Resolución y notas** | Examina la configuración del servicio Servidor de roles de DS (DsRoleSvc) y asegúrate de que el tipo de inicio es Manual. Comprueba que ningún programa de terceros esté impidiendo el inicio del servicio.<br /><br />Para obtener más información sobre cómo reclamar este controlador de dominio secundario y, al mismo tiempo, asegurarte de que se producen la replicación de salida de las actualizaciones, consulta el artículo 2742970 de Microsoft KB. |

|||  
|-|-|  
|**Problema**|**No se puede realizar la clonación, arranca en DSRM, error 8610**|  
|**Síntomas**|El clon arranca en el modo de restauración de servicios de directorio. Dcpromo.log muestra el error 8610 (que es ERROR_DS_ROLE_NOT_VERIFIED 8610 o 0x21A2)|  
|**Resolución y notas**|Se producirá si el PDC se puede detectar pero no ha realizado una replicación suficiente para poder asumir el rol. Por ejemplo, si la clonación empieza y otro administrador mueve el rol FSMO del PDCE a un nuevo controlador de dominio.<br /><br />Descrito en KB 2742916.|  

|||  
|-|-|  
|**Problema**|**Se produce un error en la clonación, arranca en DSRM, errores de red generales**|  
|**Síntomas**|El clon arranca en el modo de restauración de servicios de directorio. Hay errores de red generales.|  
|**Resolución y notas**|Asegúrate de que el controlador de dominio de origen no haya asignado una dirección MAC estática duplicada al nuevo clon; para ver si una máquina virtual usa direcciones MAC estáticas, ejecuta este comando en el host de Hyper-V tanto para la máquina virtual de origen como para la clonada:<br /><br />Get-VM-VMName *Test-VM* &#124; Get-VMNetworkAdapter &#124; FL *<br /><br />Cambia la dirección MAC por una dirección estática única o cambia para usar direcciones MAC dinámicas.<br /><br />Descrito en KB 2742844.|  

|||  
|-|-|  
|**Problema**|**Se produce un error en la clonación, arranca en DSRM**|  
|**Síntomas**|El clon arranca en el modo de reparación de servicios de directorio.|  
|**Resolución y notas**|Asegúrate de que dccloneconfig.xml contiene la definición del esquema (consulta sampledccloneconfig.xml, línea 2):<br /><br />**< D3C: archivo dccloneconfig xmlns: D3C = "URI. com: schemas: archivo dccloneconfig" >**<br /><br />Descrito en KB 2742844|  

|||  
|-|-|  
|Problema|**No hay servidores de inicio de sesión disponibles registro de errores en DSRM**|  
|**Síntomas**|El clon arranca en el modo de reparación de servicios de directorio. Intentas iniciar sesión y recibes el error:<br /><br />**Actualmente no hay ningún servidor de inicio de sesión disponible para atender la solicitud de inicio de sesión**|  
|**Resolución y notas**|Asegúrate de iniciar sesión con la cuenta de administrador de DSRM y no con la cuenta de dominio. Usa la flecha izquierda y escribe un nombre de usuario de:<br /><br />**.\Administrator**<br /><br />Descrito en KB 2742908.|  

|||  
|-|-|  
|**Problema**|**El origen de clonación produce un error en DSRM.**|  
|**Síntomas**|Durante la clonación, se produce el error 8437 "Create clone DC objects on PDC failed" (Error al crear objetos de controlador de dominio en el PDC) (0x20f5)|  
|**Resolución y notas**|Se estableció un nombre de equipo duplicado en DCCloneConfig.xml como controlador de dominio de origen o un controlador de dominio existente. El nombre del equipo también tiene que tener el formato de nombre de equipo NetBIOS (15 caracteres o menos, no un nombre completo).<br /><br />Corrige el archivo dccloneconfig.xml estableciendo un nombre válido, único.<br /><br />Descrito en KB 2742959.|  

|||  
|-|-|  
|**Problema**|**New-addccloneconfigfile error "el índice estaba fuera del intervalo"**|  
|**Síntomas**|Al ejecutar el cmdlet new-addccloneconfigfile, recibes el error:<br /><br />El índice estaba fuera del intervalo. No debe ser negativo y con un tamaño inferior al de la colección.|  
|**Resolución y notas**|Debes ejecutar el cmdlet en una consola de Windows PowerShell con privilegios elevados de administrador. Este error está causado por la no pertenencia al grupo de administradores local del equipo.<br /><br />Descrito en KB 2742927.|  

|||  
|-|-|  
|**Problema**|**Error de clonación, DC duplicado**|  
|**Síntomas**|El clon arranca sin clonar, duplica un controlador de dominio de origen existente|  
|**Resolución y notas**|El equipo se copió y se inició, pero no contiene ningún archivo DcCloneConfig.xml en ninguna de las ubicaciones admitidas, y no tenía ninguna dirección IP duplicada con el controlador de dominio de origen. El controlador de dominio debe quitarse correctamente para evitar la pérdida de datos.<br /><br />Descrito en KB 2742970.|  

|||  
|-|-|  
|**Problema**|**New-ADDCCloneConfigFile produce un error con el servidor no es un error operativo cuando comprueba si el controlador de dominio de origen es miembro del grupo controladores de dominio clonables si no hay un GC disponible.**|  
|**Síntomas**|Al ejecutar New-ADDCCloneConfigFile para crear un archivo dccloneconfig.xml, recibes el error:<br /><br />Código: el servidor no está operativo|  
|**Resolución y notas**|Comprueba la conectividad con un GC desde el servidor donde ejecutas New-ADDCCloneConfigFile y comprueba que la pertenencia del controlador de dominio de origen al grupo de controladores de dominio clonable se ha replicado en ese GC.<br /><br />Ejecuta el siguiente comando como un medio de limpiar la memoria caché del servicio de ubicación del controlador de dominio en los casos en los que un GC o un controlador de dominio se haya desconectado recientemente:<br /><br />Code-Nltest/dsgetdc:/GC/FORCE|  

### <a name="advanced-troubleshooting"></a>Solución avanzada de problemas  
El objetivo de este módulo es enseñar la solución avanzada de problemas usando registros de *trabajo* como muestras, con algunas explicaciones de lo que ocurrió. Si comprendes cómo funciona correctamente un controlador de dominio virtualizado, los errores de tu entorno te resultarán más obvios. Estos registros se presentan por origen, con los eventos *previstos* (aunque sean advertencias y errores) relativos a un controlador de dominio dentro de cada registro, en orden ascendente.  

#### <a name="cloning-a-domain-controller"></a>Clonar un controlador de dominio  
En este ejemplo, el controlador de dominio clonado usa DHCP para obtener una dirección IP, replica SYSVOL usando FRS o DFSR (consulta el registro correspondiente cuando sea necesario), es un catálogo global y usa un archivo dccloneconfig.xml en blanco.  

##### <a name="directory-services-event-log"></a>Registro de eventos de Servicios de directorio  
El registro de Servicios de directorio contiene la mayoría de la información operativa basada en eventos relativa a la clonación. El hipervisor cambia el id. de generación de VM y el servicio NTDS lo anota; después, invalida el grupo de RID y cambia el id. de invocación. Se establece el nuevo id. de generación de VM y el servidor replica los datos de Active Directory de entrada. El servicio DFSR se detiene y se elimina su base de datos, que hospeda SYSVOL, lo que obliga a una sincronización de entrada no autoritativa. Se ajusta la marca de límite superior de USN.  


|              |                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|--------------|-------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ID. de evento** |          **Origen**           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  **Mensaje**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|   **2160**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                                                                         Servicios de dominio de Active Directory local encontró un archivo de configuración de clonación de controladores de dominio virtuales.<br /><br />El archivo de configuración de clonación de controladores de dominio virtuales se encuentra en:<br /><br />*<path>* \DCCloneConfig.XML<br /><br />La existencia del archivo de configuración de clonación de controladores de dominio virtuales indica que el controlador de dominio virtual local es un clon de otro controlador de dominio virtual. Active Directory Domain Services empezará a clonarse a sí mismo.                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|   **2191**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                             Active Directory Domain Services establecieron el valor del Registro siguiente para deshabilitar las actualizaciones de DNS.<br /><br />Clave del Registro:<br /><br />SYSTEM\CurrentControlSet\Services\Netlogon\Parameters<br /><br />Valor del Registro:<br /><br />UseDynamicDns<br /><br />Datos del valor del Registro:<br /><br />0<br /><br />Durante el proceso de clonación, la máquina local podría tener durante un corto período de tiempo el mismo nombre de equipo que la máquina de origen clonada. Los registros A y AAAA de DNS están deshabilitados durante este período para que los clientes no puedan enviar solicitudes a la máquina local que se está clonando. El proceso de clonación habilitará las actualizaciones de DNS de nuevo una vez que finalice la clonación.                                                                                                                                                                                                                                                                                                                                                                                              |
|   **2191**   | ActiveDirectory_DomainService | Active Directory Domain Services establecieron el valor del Registro siguiente para deshabilitar las actualizaciones de DNS.<br /><br />Clave del Registro:<br /><br />SYSTEM\CurrentControlSet\Services\Dnscache\Parameters<br /><br />Valor del Registro:<br /><br />RegistrationEnabled<br /><br />Datos del valor del Registro:<br /><br />0<br /><br />Durante el proceso de clonación, la máquina local podría tener durante un corto período de tiempo el mismo nombre de equipo que la máquina de origen clonada. Los registros A y AAAA de DNS están deshabilitados durante este período para que los clientes no puedan enviar solicitudes a la máquina local que se está clonando. El proceso de clonación habilitará las actualizaciones de DNS de nuevo una vez que finalice la clonación.<br /><br />"Información 2/7/2012 3:12:49 PM Microsoft-Windows-ActiveDirectory_DomainService 2191 configuración interna" Active Directory Domain Services establezca el siguiente valor del registro para deshabilitar las actualizaciones de DNS.<br /><br />Clave del Registro:<br /><br />SYSTEM\CurrentControlSet\Services\Tcpip\Parameters<br /><br />Valor del Registro:<br /><br />DisableDynamicUpdate<br /><br />Datos del valor del Registro:<br /><br />1<br /><br />Durante el proceso de clonación, la máquina local podría tener durante un corto período de tiempo el mismo nombre de equipo que la máquina de origen clonada. Los registros A y AAAA de DNS están deshabilitados durante este período para que los clientes no puedan enviar solicitudes a la máquina local que se está clonando. El proceso de clonación habilitará las actualizaciones de DNS de nuevo una vez que finalice la clonación. |
|   **2172**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              Se leyó el atributo msDS-GenerationId del objeto de equipo del controlador de dominio.<br /><br />Valor de atributo msDS-GenerationId:<br /><br />*<Number>*                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|   **2170**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                           Se detectó un cambio de id. de generación.<br /><br />Id. de generación almacenado en caché en DS (valor antiguo):<br /><br />*<Number>*<br /><br />Id. de generación actualmente en VM (valor nuevo):<br /><br />*<Number>*<br /><br />El cambio de id. de generación se produce después de la aplicación de una instantánea de máquina virtual, después de una operación de importación de máquina virtual o después de una operación de migración en vivo. Servicios de dominio de Active Directory creará un nuevo id. de invocación para recuperar el controlador de dominio. Los controladores de dominio virtualizados no deben restaurarse con instantáneas de máquina virtual. El método admitido para restaurar o revertir el contenido de una base de datos de Active Directory Domain Services consiste en restaurar una copia de seguridad del estado del sistema realizada con una aplicación de copia de seguridad compatible con Active Directory Domain Services.                                                                                                                                                                                                                                                                                                                           |
|   **1109**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                           El atributo invocationID de este servidor de directorio ha cambiado. El número de secuencia de actualización mayor en el momento de crear la copia de seguridad es el siguiente:<br /><br />Atributo invocationID (valor anterior):<br /><br />*<GUID>*<br /><br />Atributo invocationID (valor nuevo):<br /><br />*<GUID>*<br /><br />Número de secuencias actualizadas:<br /><br />*<Number>*<br /><br />El atributo invocationID cambia cuando un servidor de directorio se restaura desde medios de copia de seguridad, se configura para hospedar una partición de directorio de aplicaciones de escritura, se reanuda después de aplicar una instantánea de máquina virtual, después de una operación de importación de máquina virtual o después de una operación de migración en vivo. Los controladores de dominio virtualizados no deben restaurarse con instantáneas de máquina virtual. El método admitido para restaurar o revertir el contenido de una base de datos de Active Directory Domain Services consiste en restaurar una copia de seguridad del estado del sistema realizada con una aplicación de copia de seguridad compatible con Active Directory Domain Services.                                                                                                                                                                                                                            |
|   **1000**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          Inicio de Active Directory Domain Services de Microsoft completado.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|   **1394**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              Se solucionaron todos los problemas que impedían la actualización de la base de datos de Active Directory Domain Services. Las nuevas actualizaciones de la base de datos de Active Directory Domain Services se están realizando correctamente. Se reinició el servicio de Net Logon.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|   **2163**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  Se inició el servicio DsRoleSvc para clonar el controlador de dominio virtual local.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|   **326**    |           NTDS ISAM           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                NTDS (536) NTDSa: el motor de base de datos adjuntó una base de datos (1, C:\Windows\NTDS\ntds.dit). (Tiempo=0 segundos)<br /><br />Secuencia temporal interna: [1] 0,000, [2] 0,000, [3] 0,000, [4] 0,000, [5] 0,000, [6] 0,016, [7] 0,000, [8] 0,000, [9] 0,000, [10] 0,000, [11] 0,000, [12] 0,000.<br /><br />Caché guardada: 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|   **103**    |           NTDS ISAM           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  NTDS (536) NTDSa: el motor de base de datos detuvo la instancia (0).<br /><br />Cierre con errores: 0<br /><br />Secuencia temporal interna: [1] 0,000, [2] 0,000, [3] 0,000, [4] 0,000, [5] 0,032, [6] 0,000, [7] 0,000, [8] 0,000, [9] 0,031, [10] 0,000, [11] 0,000, [12] 0,000, [13] 0,000, [14] 0,000, [15] 0,000.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|   **102**    |           NTDS ISAM           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             NTDS (536) NTDSa: el motor de base de datos (6.02.8225.0000) está iniciando una nueva instancia (0).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|   **105**    |           NTDS ISAM           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               NTDS (536) NTDSa: el motor de base de datos inició una nueva instancia (0). (Tiempo=0 segundos)<br /><br />Secuencia temporal interna: [1] 0,016, [2] 0,000, [3] 0,015, [4] 0,078, [5] 0,000, [6] 0,000, [7] 0,000, [8] 0,000, [9] 0,046, [10] 0,000, [11] 0,000.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|   **1004**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          Active Directory Domain Services se cerró correctamente.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|   **102**    |           NTDS ISAM           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             NTDS (536) NTDSa: el motor de base de datos (6.02.8225.0000) está iniciando una nueva instancia (0).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|   **326**    |           NTDS ISAM           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                NTDS (536) NTDSa: el motor de base de datos adjuntó una base de datos (1, C:\Windows\NTDS\ntds.dit). (Tiempo=0 segundos)<br /><br />Secuencia temporal interna: [1] 0,000, [2] 0,015, [3] 0,016, [4] 0,000, [5] 0,031, [6] 0,000, [7] 0,000, [8] 0,000, [9] 0,000, [10] 0,000, [11] 0,000, [12] 0,000.<br /><br />Caché guardada: 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|   **105**    |           NTDS ISAM           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               NTDS (536) NTDSa: el motor de base de datos inició una nueva instancia (0). (Tiempo=1 segundos)<br /><br />Secuencia temporal interna: [1] 0,031, [2] 0,000, [3] 0,000, [4] 0,391, [5] 0,000, [6] 0,000, [7] 0,000, [8] 0,000, [9] 0,031, [10] 0,000, [11] 0,000.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|   **1109**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                           El atributo invocationID de este servidor de directorio ha cambiado. El número de secuencia de actualización mayor en el momento de crear la copia de seguridad es el siguiente:<br /><br />Atributo invocationID (valor anterior):<br /><br />*<GUID>*<br /><br />Atributo invocationID (valor nuevo):<br /><br />*<GUID>*<br /><br />Número de secuencias actualizadas:<br /><br />*<Number>*<br /><br />El atributo invocationID cambia cuando un servidor de directorio se restaura desde medios de copia de seguridad, se configura para hospedar una partición de directorio de aplicaciones de escritura, se reanuda después de aplicar una instantánea de máquina virtual, después de una operación de importación de máquina virtual o después de una operación de migración en vivo. Los controladores de dominio virtualizados no deben restaurarse con instantáneas de máquina virtual. El método admitido para restaurar o revertir el contenido de una base de datos de Active Directory Domain Services consiste en restaurar una copia de seguridad del estado del sistema realizada con una aplicación de copia de seguridad compatible con Active Directory Domain Services.                                                                                                                                                                                                                            |
|   **1168**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                Error interno: se ha producido un error de Active Directory Domain Services.<br /><br />Datos adicionales<br /><br />Valor del error (decimal):<br /><br />2<br /><br />Valor del error (hexadecimal):<br /><br />2<br /><br />Id. interno:<br /><br />7011658                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|   **1110**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                           La promoción de este controlador de dominio a catálogo global se retrasará en el intervalo indicado.<br /><br />Intervalo (minutos):<br /><br />5<br /><br />Este retraso es necesario para que las particiones de directorio requeridas puedan prepararse antes de que se anuncie el catálogo global. En el Registro, puedes especificar el número de segundos que el agente de sistema de directorio esperará antes de ascender el controlador de dominio local a catálogo global. Para obtener más información acerca del valor del Registro correspondiente al anuncio de retardo del catálogo global, consulta la Guía de sistemas distribuidos del Kit de recursos.                                                                                                                                                                                                                                                                                                                                                                                                                            |
|   **103**    |           NTDS ISAM           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  NTDS (536) NTDSa: el motor de base de datos detuvo la instancia (0).<br /><br />Cierre con errores: 0<br /><br />Secuencia temporal interna: [1] 0,000, [2] 0,000, [3] 0,000, [4] 0,000, [5] 0,047, [6] 0,000, [7] 0,000, [8] 0,000, [9] 0,016, [10] 0,000, [11] 0,000, [12] 0,000, [13] 0,000, [14] 0,000, [15] 0,000.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|   **1004**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          Active Directory Domain Services se cerró correctamente.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|   **1539**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  Active Directory Domain Services no pudieron deshabilitar la memoria caché de escritura en disco por software en el siguiente disco duro.<br /><br />Disco duro:<br /><br />c:<br /><br />Pueden perderse datos si se producen errores del sistema.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|   **2179**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  El atributo msDS-GenerationId del objeto de equipo del controlador de dominio se estableció en el parámetro siguiente:<br /><br />Atributo GenerationID:<br /><br />*<Number>*                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|   **2173**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      No se puede leer el atributo msDS-GenerationId del objeto de equipo del controlador de dominio. Esto puede deberse a un error de transacción de la base de datos o a que no existe el identificador de generación en la base de datos local. El atributo msDS-GenerationId no existe durante el primer reinicio después de ejecutar dcpromo o el controlador de dominio no es un controlador de dominio virtual.<br /><br />Datos adicionales<br /><br />Código de error:<br /><br />6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|   **1000**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                Inicio de Microsoft Active Directory Domain Services completado, versión 6.2.8225.0.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|   **1394**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             Se solucionaron todos los problemas que impedían la actualización de la base de datos de Active Directory Domain Services. Las nuevas actualizaciones de la base de datos de Active Directory Domain Services se están realizando correctamente. Se reinició el servicio de Net Logon.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|   **1128**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      1128, Comprobador de coherencia de la información, "Se creó una conexión de replicación desde el siguiente servicio de directorio de origen al servicio de directorio local.<br /><br />Servicio de directorio de origen:<br /><br />CN = configuración NTDS, *<Domain Controller DN>*<br /><br />Servicio de directorio local:<br /><br />CN = configuración NTDS, *<Domain Controller DN>*<br /><br />Datos adicionales<br /><br />Código de motivo:<br /><br />0x2<br /><br />ID interno de punto de creación:<br /><br />f0a025d                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|   **1999**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                           El servicio de directorio de origen ha optimizado el número de secuencia de actualización (USN) presentado por el servicio de directorio de destino. Los servicios de directorio de destino y de origen tienen un asociado de replicación en común. El servicio de directorio de destino está actualizado con el asociado de replicación común y el servicio de directorio de origen se instaló utilizando una copia de seguridad de este asociado.<br /><br />Id. de servicio de directorio de destino:<br /><br />*<GUID> (<FQDN>)*<br /><br />Id. de servicio de directorio común:<br /><br />*<GUID>*<br /><br />USN de la propiedad común:<br /><br />*<Number>*<br /><br />Como resultado, el vector de actualización del servicio de directorio de destino se ha configurado de la siguiente forma.<br /><br />USN del objeto anterior:<br /><br />0<br /><br />USN de la propiedad anterior:<br /><br />0<br /><br />GUID de la base de datos:<br /><br />*<GUID>*<br /><br />USN de objeto:<br /><br />*<Number>*<br /><br />USN de la propiedad:<br /><br />*<Number>*                                                                                                                                                                                                                                           |

##### <a name="system-event-log"></a>Registro de eventos de Sistema  
Las siguientes indicaciones sobre las operaciones de clonación están en el registro de eventos de Sistema. Cuando el hipervisor indica al equipo invitado que fue clonado o restaurado a partir de una instantánea, el controlador de dominio invalida inmediatamente su grupo de RID para evitar duplicar las entidades de seguridad más adelante. A medida que continúa la clonación, aparecen varios mensajes y operaciones previstos, la mayoría relativos al inicio y detención de servicios, y algunos errores causados por este motivo. Una vez completada, el registro de eventos de Sistema anota que la clonación se realizó correctamente.  

||||  
|-|-|-|  
|**ID. de evento**|**Origen**|**Mensaje**|  
|**16654**|Directory-Services-SAM|Se invalidó un grupo de identificadores de cuenta (RID). Esto puede suceder en los siguientes casos previstos:<br /><br />1. un controlador de dominio se restaura a partir de una copia de seguridad.<br /><br />2. un controlador de dominio que se ejecuta en una máquina virtual se restaura desde una instantánea.<br /><br />3. un administrador invalidó manualmente el grupo|  
|**7036**|administrador de control de servicios|El servicio Active Directory Domain Services entró en estado de ejecución.|  
|**7036**|administrador de control de servicios|El servicio Centro de distribución de claves Kerberos entró en estado de ejecución.|  
|**3096**|Netlogon|No se puede encontrar el controlador de dominio principal para este dominio.|  
|**7036**|administrador de control de servicios|El servicio Administrador de cuentas de servicio entró en estado de ejecución.|  
|**7036**|administrador de control de servicios|El servicio Servidor entró en estado de ejecución.|  
|**7036**|administrador de control de servicios|El servicio Netlogon entró en estado de ejecución.|  
|**7036**|administrador de control de servicios|El servicio Servicios web de Active Directory entró en estado de ejecución.|  
|**7036**|administrador de control de servicios|El servicio Replicación DFS entró en estado de ejecución.|  
|**7036**|administrador de control de servicios|El servicio Servicio de replicación de archivos entró en estado de ejecución.|  
|**14533**|Microsoft-Windows-DfsSvc|DFS terminó de generar todos los espacios de nombres.|  
|**14531**|Microsoft-Windows-DfsSvc|El servidor DFS terminó de inicializarse.|  
|**7036**|administrador de control de servicios|El servicio Espacio de nombres DFS entró en estado de ejecución.|  
|**7023**|administrador de control de servicios|El servicio Mensajería entre sitios se cerró con el siguiente error:<br /><br />El servidor especificado no puede ejecutar la operación solicitada.|  
|**7036**|administrador de control de servicios|El servicio Mensajería entre sitios entró en estado de detención.|  
|**5806**|Netlogon|Las actualizaciones dinámicas se han deshabilitado manualmente en este controlador de dominio.<br /><br />ACCIÓN DEL USUARIO<br /><br />Vuelve a configurar este controlador de dominio para utilizar actualizaciones dinámicas de DNS o agrega manualmente los registros DNS desde el archivo '%SystemRoot%\System32\Config\Netlogon.dns' a la base de datos DNS.|  
|**16651**|Directory-Services-SAM|Error al solicitar un nuevo grupo de identificadores de cuenta. Se seguirá intentando hasta que la solicitud se realice correctamente. Error:<br /><br />Error en la operación FSMO solicitada. No se puede poner en contacto con el titular de FSMO actual.|  
|**7036**|administrador de control de servicios|El servicio Servidor DNS entró en estado de ejecución.|  
|**7036**|administrador de control de servicios|El servicio Servidor de roles de DNS entró en estado de ejecución.|  
|**7036**|administrador de control de servicios|El servicio Netlogon entró en estado de detención.|  
|**7036**|administrador de control de servicios|El servicio Servicio de replicación de archivos entró en estado de detención.|  
|**7036**|administrador de control de servicios|El servicio Centro de distribución de claves Kerberos entró en estado de detención.|  
|**7036**|administrador de control de servicios|El servicio Servidor DNS entró en estado de detención.|  
|**7036**|administrador de control de servicios|El servicio Servicios de dominio de Active Directory entró en estado de detención.|  
|**7036**|administrador de control de servicios|El servicio Netlogon entró en estado de ejecución.|  
|**7040**|administrador de control de servicios|El tipo de inicio del servicio Active Directory Domain Services se cambió de inicio automático a deshabilitado.|  
|**7036**|administrador de control de servicios|El servicio Netlogon entró en estado de detención.|  
|**7036**|administrador de control de servicios|El servicio Servicio de replicación de archivos entró en estado de ejecución.|  
|**29219**|DirectoryServices-DSROLE-Server|La clonación del controlador de dominio virtual se realizó correctamente.|  
|**29223**|DirectoryServices-DSROLE-Server|Este servidor es ahora un controlador de dominio.|  
|**29265**|DirectoryServices-DSROLE-Server|La clonación del controlador de dominio virtual se realizó correctamente. Se ha cambiado el nombre del archivo de configuración de clonación del controlador de dominio virtual C:\Windows\NTDS\DCCloneConfig.xml a C:\Windows\NTDS\DCCloneConfig.20120207-151533.xml.|  
|**1074**|User32|El proceso C:\Windows\system32\lsass.exe (DC2) ha iniciado el reinicio del equipo DC2 en nombre de usuario NT AUTHORITY\SYSTEM por el siguiente motivo: sistema operativo: reconfiguración (planeada)<br /><br />Código de motivo: 0x80020004<br /><br />Tipo de apagado: reiniciar<br /><br />Comentario: "|  

##### <a name="dcpromolog"></a>DCPROMO.LOG  
El registro Dcpromo.log contiene la parte real de la promoción de la clonación que el registro de eventos de Servicios de directorio no describe. Como el registro no proporciona el mismo nivel de explicación que las entradas del registro de eventos, esta sección del módulo contiene anotaciones adicionales.  

En el proceso de promoción, la clonación comienza, se limpia la configuración actual del controlador de dominio y se vuelve a promocionar usando la base de datos de AD existente (de forma muy similar a una promoción de IFM); después, el controlador de dominio replica los deltas de cambio de entrada de AD y SYSVOL, y la clonación finaliza.  

> [!NOTE]  
> En este módulo, el registro ha sido modificado y se ha quitado la columna de fecha para facilitar su lectura.  

> [!NOTE]  
> Para obtener más información sobre dcpromo.log, consulta Conocimiento y solución de problemas de la administración simplificada de AD DS en Windows Server 2012.  
>   
> [https://go.microsoft.com/fwlink/p/?LinkId=237244](https://go.microsoft.com/fwlink/p/?LinkId=237244)  

-   Inicia la promoción basada en clon.  

-   Establece el indicador del modo de restauración de servicios de directorio para que el servidor no arranque la copia de seguridad normalmente y no provoque conflictos de nombres o del servicio de directorio.  

-   Actualiza el registro de eventos de Servicios de directorio.  

```  
15:14:01 [INFO] vDC Cloneing: Setting Boot into DSRM flag succeeded.  
15:14:01 [WARNING] Cannot get user Token for Format Message: 1725l  
15:14:01 [INFO] vDC Cloning: Created vDCCloningUpdate event.  
15:14:01 [INFO] vDC Cloning: Created vDCCloningComplete event.  
```  

-   Detén el servicio NetLogon para que el controlador de dominio no se publicite.  

```  
15:14:01 [INFO] Stopping service NETLOGON  
15:14:01 [INFO] ControlService(STOP) on NETLOGON returned 1(gle=0)  
15:14:01 [INFO] DsRolepWaitForService: waiting for NETLOGON to enter one of 7 states  
15:14:01 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:02 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=1  
15:14:02 [INFO] DsRolepWaitForService: exiting because NETLOGON entered STOPPED state  
15:14:02 [INFO] DsRolepWaitForService(for any end state) on NETLOGON service returned 0  
15:14:02 [INFO] ControlService(STOP) on NETLOGON returned 0(gle=1062)  
15:14:02 [INFO] Exiting service-stop loop after service NETLOGON entered STOPPED state  
15:14:02 [INFO] StopService on NETLOGON returned 0  
15:14:02 [INFO] Configuring service NETLOGON to 1 returned 0  
15:14:02 [INFO] Updating service status to 4  
15:14:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  

-   Examina el archivo dccloneconfig.xml y busca personalizaciones especificadas por el administrador.  

-   En este caso de muestra, es un archivo en blanco por lo que toda la configuración se genera automáticamente y se requiere direccionamiento IP automático de la red.  

```  
15:14:02 [INFO] vDC Cloning: Clone config file C:\Windows\NTDS\DCCloneConfig.xml is considered to be a blank file (containing 0 bytes)  
15:14:02 [INFO] vDC Cloning: Parsing clone config file C:\Windows\NTDS\DCCloneConfig.xml returned HRESULT 0x0  
```  

-   Comprueba que no haya instalados servicios o programas que no formen parte de DefaultDCCloneAllowList.xml o CustomDCCloneAllowList.xml  

```  
15:14:02 [INFO] vDC Cloning: Checking allowed list:  
15:14:03 [INFO] vDC Cloning: Completed checking allowed list:  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  

-   Habilita DHCP en los adaptadores de red, porque el administrador no especificó la información de IP.  

```  
15:14:03 [INFO] vDC Cloning: Enable DHCP:  
15:14:03 [INFO] WMI Instance: Win32_NetworkAdapterConfiguration.Index=12  
15:14:03 [INFO] Method: EnableDHCP  
15:14:03 [INFO] HRESULT code: 0x0 (0)  
15:14:03 [INFO] Return Value: 0x0 (0)  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  

-   Busca el emulador de PDC.  

-   Establece el sitio del clon (en este caso se genera automáticamente).  

-   Establece el nombre del clon (en este caso se genera automáticamente).  

```  
15:14:03 [INFO] vDC Cloning: Found PDC. Name: DC1.root.fabrikam.com  
15:14:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:04 [INFO] vDC Cloning: Winlogon UI Notification #1: Domain Controller cloning is at 5% completion...  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #2: Domain Controller cloning is at 10% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Site of the cloned DC: Default-First-Site-Name  
```  

-   Crea el nuevo objeto de equipo clonado.  

-   Cambia el nombre del clon para que coincida con el nuevo nombre.  

```  
15:14:05 [INFO] vDC Cloning: Clone DC objects are created on PDC.  
15:14:05 [INFO] Name of the cloned DC: DC2-CL0001  
15:14:05 [INFO] DsRolepSetRegStringValue on System\CurrentControlSet\Services\NTDS\Parameters\CloneMachineName to DC2-CL0001 returned 0  
15:14:05 [INFO] vDC Cloning: Save CloneMachineName in registry: 0x0 (0)  
```  

-   Proporciona la configuración de la promoción de acuerdo con el archivo dccloneconfig.xml anterior o con las reglas de generación automática.  

```  
15:14:05 [INFO] vDC Cloning: Promotion parameters setting:  
15:14:05 [INFO] DNS Domain Name: root.fabrikam.com  
15:14:05 [INFO] Replica Partner: \\DC1.root.fabrikam.com  
15:14:05 [INFO] Site Name: Default-First-Site-Name  
15:14:05 [INFO] DS Database Path: C:\Windows\NTDS  
15:14:05 [INFO] DS Log Path: C:\Windows\NTDS  
15:14:05 [INFO] SysVol Root Path: C:\Windows\SYSVOL  
15:14:05 [INFO] Account: root.fabrikam.com\DC2-CL0001$  
15:14:05 [INFO] Options: DSROLE_DC_CLONING (0x800400)  
```  

-   Inicia la promoción.  

```  
15:14:05 [INFO] Promote DC as a clone  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #3: Domain Controller cloning is at 15% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #4: Domain Controller cloning is at 16% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Validate supplied paths  
15:14:05 [INFO] Validating path C:\Windows\NTDS.  
15:14:05 [INFO] Path is a directory  
15:14:05 [INFO] Path is on a fixed disk drive.  
15:14:05 [INFO] Validating path C:\Windows\NTDS.  
15:14:05 [INFO] Path is a directory  
15:14:05 [INFO] Path is on a fixed disk drive.  
15:14:05 [INFO] Validating path C:\Windows\SYSVOL.  
15:14:05 [INFO] Path is on a fixed disk drive.  
15:14:05 [INFO] Path is on an NTFS volume  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #5: Domain Controller cloning is at 17% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Start the worker task  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #6: Domain Controller cloning is at 20% completion...  
15:14:05 [INFO] Request for promotion returning 0  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #7: Domain Controller cloning is at 21% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  

-   Detén y configura todos los servicios relacionados con AD DS (NTDS, NTFRS/DFSR, KDC, DNS).  

> [!NOTE]  
> En este escenario, es previsible que el servicio DNS tarde mucho tiempo, porque usa zonas integradas de AD que ya no estaban disponibles incluso antes de que se detuviera el servicio NTDS. Consulta los eventos DNS que se describen más adelante en esta sección del módulo.  

```  
15:14:15 [INFO] Stopping service NTDS  
15:14:15 [INFO] Stopping service NtFrs  
15:14:15 [INFO] ControlService(STOP) on NtFrs returned 1(gle=0)  
15:14:15 [INFO] DsRolepWaitForService: waiting for NtFrs to enter one of 7 states  
15:14:15 [INFO] DsRolepWaitForService: QueryServiceStatus on NtFrs returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:16 [INFO] DsRolepWaitForService: QueryServiceStatus on NtFrs returned 1 (gle=0), SvcStatus.dwCS=1  
15:14:16 [INFO] DsRolepWaitForService: exiting because NtFrs entered STOPPED state  
15:14:16 [INFO] DsRolepWaitForService(for any end state) on NtFrs service returned 0  
15:14:16 [INFO] ControlService(STOP) on NtFrs returned 0(gle=1062)  
15:14:16 [INFO] Exiting service-stop loop after service NtFrs entered STOPPED state  
15:14:16 [INFO] StopService on NtFrs returned 0  
15:14:16 [INFO] Configuring service NtFrs to 1 returned 0  
15:14:16 [INFO] Stopping service Kdc  
15:14:16 [INFO] ControlService(STOP) on Kdc returned 1(gle=0)  
15:14:16 [INFO] DsRolepWaitForService: waiting for Kdc to enter one of 7 states  
15:14:16 [INFO] DsRolepWaitForService: QueryServiceStatus on Kdc returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:17 [INFO] DsRolepWaitForService: QueryServiceStatus on Kdc returned 1 (gle=0), SvcStatus.dwCS=1  
15:14:17 [INFO] DsRolepWaitForService: exiting because Kdc entered STOPPED state  
15:14:17 [INFO] DsRolepWaitForService(for any end state) on Kdc service returned 0  
15:14:17 [INFO] ControlService(STOP) on Kdc returned 0(gle=1062)  
15:14:17 [INFO] Exiting service-stop loop after service Kdc entered STOPPED state  
15:14:17 [INFO] StopService on Kdc returned 0  
15:14:17 [INFO] Configuring service Kdc to 1 returned 0  
15:14:17 [INFO] Stopping service DNS  
15:14:17 [INFO] ControlService(STOP) on DNS returned 1(gle=0)  
15:14:17 [INFO] DsRolepWaitForService: waiting for DNS to enter one of 7 states  
15:14:17 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:18 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:19 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:20 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:21 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:22 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:23 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:24 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:25 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:26 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:27 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:28 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:29 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:30 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:31 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:32 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:33 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:34 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:35 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:36 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:37 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:38 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:39 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:40 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:41 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:42 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:43 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:44 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:45 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:46 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:47 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:48 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:49 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:50 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:51 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:52 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:53 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:54 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:55 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:56 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:57 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:58 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:14:59 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3  
15:15:00 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=1  
15:15:00 [INFO] DsRolepWaitForService: exiting because DNS entered STOPPED state  
15:15:00 [INFO] DsRolepWaitForService(for any end state) on DNS service returned 0  
15:15:00 [INFO] ControlService(STOP) on DNS returned 0(gle=1062)  
15:15:00 [INFO] Exiting service-stop loop after service DNS entered STOPPED state  
15:15:00 [INFO] StopService on DNS returned 0  
15:15:00 [INFO] Configuring service DNS to 1 returned 0  
15:15:00 [INFO] ControlService(STOP) on NTDS returned 1(gle=1062)  
15:15:00 [INFO] DsRolepWaitForService: waiting for NTDS to enter one of 7 states  
15:15:00 [INFO] DsRolepWaitForService: QueryServiceStatus on NTDS returned 1 (gle=0), SvcStatus.dwCS=3  
15:15:01 [INFO] DsRolepWaitForService: QueryServiceStatus on NTDS returned 1 (gle=0), SvcStatus.dwCS=1  
15:15:01 [INFO] DsRolepWaitForService: exiting because NTDS entered STOPPED state  
15:15:01 [INFO] DsRolepWaitForService(for any end state) on NTDS service returned 0  
15:15:01 [INFO] ControlService(STOP) on NTDS returned 0(gle=1062)  
15:15:01 [INFO] Exiting service-stop loop after service NTDS entered STOPPED state  
15:15:01 [INFO] StopService on NTDS returned 0  
15:15:01 [INFO] Configuring service NTDS to 1 returned 0  
15:15:01 [INFO] Configuring service NTDS  
15:15:01 [INFO] Configuring service NTDS to 64 returned 0  
15:15:01 [INFO] vDC Cloning: Winlogon UI Notification #8: Domain Controller cloning is at 22% completion...  
15:15:01 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:01 [INFO] vDC Cloning: Winlogon UI Notification #9: Domain Controller cloning is at 25% completion...  
15:15:01 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  

-   Aplica una sincronización temporal NT5DS (NTP) con otro controlador de dominio (normalmente el PDCE).  

```  
15:15:02 [INFO] Forcing time sync  
```  

-   Ponte en contacto con un controlador de dominio que contenga la cuenta del controlador de dominio de origen del clon.  

-   Limpia los vales de Kerberos existentes.  

```  
15:15:02 [INFO] Searching for a domain controller for the domain root.fabrikam.com that contains the account DC2$  
15:15:02 [INFO] Located domain controller DC1.root.fabrikam.com for domain root.fabrikam.com  
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #10: Domain Controller cloning is at 26% completion...  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:02 [INFO] Directing kerberos authentication to DC1.root.fabrikam.com returns 0  
15:15:02 [INFO] DsRolepFlushKerberosTicketCache() successfully flushed the Kerberos ticket cache  
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #11: Domain Controller cloning is at 27% completion...  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:02 [INFO] Using site Default-First-Site-Name for server \\DC1.root.fabrikam.com  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  

-   Detén el servicio NetLogon y establece su tipo de inicio.  

```  
15:15:02 [INFO] Stopping service NETLOGON  
15:15:02 [INFO] Stopping service NETLOGON  
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #12: Domain Controller cloning is at 29% completion...  
15:15:02 [INFO] ControlService(STOP) on NETLOGON returned 1(gle=0)  
15:15:02 [INFO] DsRolepWaitForService: waiting for NETLOGON to enter one of 7 states  
15:15:02 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=3  
15:15:03 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=1  
15:15:03 [INFO] DsRolepWaitForService: exiting because NETLOGON entered STOPPED state  
15:15:03 [INFO] DsRolepWaitForService(for any end state) on NETLOGON service returned 0  
15:15:03 [INFO] ControlService(STOP) on NETLOGON returned 0(gle=1062)  
15:15:03 [INFO] Exiting service-stop loop after service NETLOGON entered STOPPED state  
15:15:03 [INFO] StopService on NETLOGON returned 0  
15:15:03 [INFO] Configuring service NETLOGON to 1 returned 0  
15:15:03 [INFO] Stopped NETLOGON  
15:15:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:03 [INFO] vDC Cloning: Winlogon UI Notification #13: Domain Controller cloning is at 30% completion...  
```  

-   Configura los servicios DFSR/NTFRS para que se ejecuten automáticamente.  

-   Elimina sus archivos de base de datos existentes para aplicar una sincronización no autoritativa de SYSVOL la próxima vez que se inicie el servicio.  

```  
15:15:03 [INFO] Configuring service DFSR  
15:15:03 [INFO] Configuring service DFSR to 256 returned 0  
15:15:03 [INFO] Configuring service NTFRS  
15:15:03 [INFO] Configuring service NTFRS to 256 returned 0  
15:15:03 [INFO] Removing DFSR Database files for SysVol  
15:15:03 [INFO] Removing FRS Database files in C:\Windows\ntfrs\jet  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edb.log  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbres00001.jrs  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbres00002.jrs  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbtmp.log  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\ntfrs.jdb  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\sys\edb.chk  
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\temp\tmp.edb  
15:15:04 [INFO] Created system volume path  
15:15:04 [INFO] Configuring service DFSR  
15:15:04 [INFO] Configuring service DFSR to 128 returned 0  
15:15:04 [INFO] Configuring service NTFRS  
15:15:04 [INFO] Configuring service NTFRS to 128 returned 0  
15:15:04 [INFO] vDC Cloning: Winlogon UI Notification #14: Domain Controller cloning is at 40% completion...  
15:15:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  

-   Inicia el proceso de promoción usando el archivo de base de datos NTDS existente.  

-   Ponte en contacto con el maestro RID.  

> [!NOTE]  
> El servicio AD DS no está realmente instalado aquí; esta es una instrumentación heredada del registro.  

```  
15:15:04 [INFO] Installing the Directory Service  
15:15:04 [INFO] Calling NtdsInstall for root.fabrikam.com  
15:15:04 [INFO] Starting Active Directory Domain Services installation  
15:15:04 [INFO] Validating user supplied options  
15:15:04 [INFO] Determining a site in which to install  
15:15:04 [INFO] Examining an existing forest...  
15:15:04 [INFO] Starting a replication cycle between DC1.root.fabrikam.com and the RID operations master (2008r2-01.root.fabrikam.com), so that the new replica will be able to create users, groups, and computer objects...  
15:15:04 [INFO] Configuring the local computer to host Active Directory Domain Services  
15:15:04 [INFO] EVENTLOG (Warning): NTDS General / Service Control : 1539  
Active Directory Domain Services could not disable the software-based disk write cache on the following hard disk.  
Hard disk:  
c:  
Data might be lost during system failures.  
15:15:10 [INFO] EVENTLOG (Informational): NTDS General / Internal Processing : 2041  
Duplicate event log entries were suppressed.  
See the previous event log entry for details. An entry is considered a duplicate if  
the event code and all of its insertion parameters are identical. The time period for  
this run of duplicates is from the time of the previous event to the time of this event.  
Event Code:  
80000603  
Number of duplicate entries:   
2  
15:15:10 [INFO] EVENTLOG (Informational): NTDS General / Internal Configuration : 2121  
This Active Directory Domain Services server is disabling the Recycle Bin. Deleted objects may not be undeleted at this time.  
```  

-   Cambia el id. de invocación que existía en la base de datos de equipos de origen.  

-   Crea un nuevo objeto Configuración NTDS para este clon.  

-   Replica los deltas de objeto de AD del controlador de dominio del asociado.  

> [!NOTE]  
> Aunque todos los objetos se muestran como replicados, se trata solo de los metadatos necesarios para agregar las actualizaciones. Todos los objetos que no han cambiado en la base de datos de NTDS clonada ya existen y no es necesario volver a replicarlos, igual que cuando se usa la promoción basada en IFM.  

```  
15:15:10 [INFO] EVENTLOG (Informational): NTDS Replication / Replication : 1109  
The invocationID attribute for this directory server has been changed. The highest update sequence number at the time the backup was created is as follows:  
InvocationID attribute (old value):  
24e7b22f-4706-402d-9b4f-f2690f730b40  
InvocationID attribute (new value):  
f74cefb2-89c2-442c-b1ba-3234b0ed62f8  
Update sequence number:  
20520  
The invocationID is changed when a directory server is restored from backup media, is configured to host a writeable application directory partition, has been resumed after a virtual machine snapshot has been applied, after a virtual machine import operation, or after a live migration operation. Virtualized domain controllers should not be restored using virtual machine snapshots. The supported method to restore or rollback the content of an Active Directory Domain Services database is to restore a system state backup made with an Active Directory Domain Services-aware backup application.  
15:15:10 [INFO] EVENTLOG (Error): NTDS General / Internal Processing : 1168  
Internal error: An Active Directory Domain Services error has occurred.  
Additional Data  
Error value (decimal):  
2  
Error value (hexadecimal):  
2  
Internal ID:  
7011658  
15:15:11 [INFO] Creating the NTDS Settings object for this Active Directory Domain Controller on the remote AD DC DC1.root.fabrikam.com...  
15:15:11 [INFO] Replicating the schema directory partition  
15:15:11 [INFO] Replicated the schema container.  
15:15:12 [INFO] Active Directory Domain Services updated the schema cache.  
15:15:12 [INFO] Replicating the configuration directory partition  
15:15:12 [INFO] Replicating data CN=Configuration,DC=root,DC=fabrikam,DC=com: Received 2612 out of approximately 2612 objects and 94 out of approximately 94 distinguished name (DN) values...  
15:15:12 [INFO] Replicated the configuration container.  
15:15:13 [INFO] Replicating critical domain information...  
15:15:13 [INFO] Replicating data DC=root,DC=fabrikam,DC=com: Received 109 out of approximately 109 objects and 35 out of approximately 35 distinguished name (DN) values...  
15:15:13 [INFO] Replicated the critical objects in the domain container.  
```  

-   Rellena las particiones de GC según sea necesario con las actualizaciones que faltan.  

-   Completa la parte de AD DS crítica de la promoción.  

```  
15:15:13 [INFO] EVENTLOG (Informational): NTDS General / Global Catalog : 1110  
Promotion of this domain controller to a global catalog will be delayed for the following interval.  
Interval (minutes):  
5  
This delay is necessary so that the required directory partitions can be prepared before the global catalog is advertised. In the registry, you can specify the number of seconds that the directory system agent will wait before promoting the local domain controller to a global catalog. For more information about the Global Catalog Delay Advertisement registry value, see the Resource Kit Distributed Systems Guide.  
15:15:14 [INFO] EVENTLOG (Informational): NTDS General / Service Control : 1000  
Microsoft Active Directory Domain Services startup complete, version 6.2.8225.0   
15:15:15 [INFO] Creating new domain users, groups, and computer objects  
15:15:16 [INFO] Completing Active Directory Domain Services installation  
15:15:16 [INFO] NtdsInstall for root.fabrikam.com returned 0  
15:15:16 [INFO] DsRolepInstallDs returned 0  
15:15:16 [INFO] Installed Directory Service  
```  

-   Completa la replicación de entrada de SYSVOL.  

```  
15:15:16 [INFO] vDC Cloning: Winlogon UI Notification #15: Domain Controller cloning is at 60% completion...  
15:15:16 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] Completed system volume replication  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #16: Domain Controller cloning is at 70% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] SetProductType to 2 [LanmanNT] returned 0  
15:15:18 [INFO] Set the product type  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #17: Domain Controller cloning is at 71% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #18: Domain Controller cloning is at 72% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] Set the system volume path for NETLOGON  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #19: Domain Controller cloning is at 73% completion...  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] Replicating non critical information  
15:15:18 [INFO] User specified to not replicate non-critical data  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #20: Domain Controller cloning is at 80% completion...  
15:15:18 [INFO] Stopped the DS  
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #21: Domain Controller cloning is at 90% completion...  
15:15:18 [INFO] Configuring service NTDS  
15:15:18 [INFO] Configuring service NTDS to 16 returned 0  
```  

-   Habilita el registro de DNS de cliente.  

```  
15:15:18 [INFO] vDC Cloning: Set DisableDynamicUpdate reg value to 0 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set UseDynamicDns reg value to 1 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set RegistrationEnabled reg value to 1 to enable dynamic update records registration.  
```  

-   Ejecuta los módulos SYSPREP especificados por el elemento <SysprepInformation> de DefaultDCCloneAllowList.xml.  

```  
15:15:18 [INFO] vDC Cloning: Running sysprep providers.  
15:15:32 [INFO] vDC Cloning: Completed running sysprep providers.  
```  

-   La promoción de la clonación se ha completado.  

-   Quita el indicador de arranque DSRM para que el servidor arranque normalmente la próxima vez.  

-   Cambia el nombre de dccloneconfig.xml para que no se vuelva a leer en el próximo arranque.  

-   Reinicia el equipo.  

```  
15:15:32 [INFO] The attempted domain controller operation has completed  
15:15:32 [INFO] Updating service status to 4  
15:15:32 [INFO] DsRolepSetOperationDone returned 0  
15:15:32 [INFO] vDC Cloning: Set vDCCloningComplete event.  
15:15:32 [INFO] vDC Cloneing: Clearing Boot into DSRM flag succeeded.  
15:15:32 [INFO] vDC Cloning: Winlogon UI Notification #22: Cloning Domain Controller succeeded. Now rebooting...  
15:15:33 [INFO] vDC Cloning: Renamed vDC clone configuration file.  
15:15:33 [INFO] vDC Cloning: The old name is: C:\Windows\NTDS\DCCloneConfig.xml  
15:15:33 [INFO] vDC Cloning: The new name is: C:\Windows\NTDS\DCCloneConfig.20120207-151533.xml  
15:15:34 [INFO] vDC Cloning: Release Ipv4 on interface 'Wired Ethernet Connection 2', result=0.  
15:15:34 [INFO] vDC Cloning: Release Ipv6 on interface 'Wired Ethernet Connection 2', result=0.  
15:15:34 [INFO] Rebooting machine  
```  

##### <a name="active-directory-web-services-event-log"></a>Registro de eventos de Servicios web de Active Directory  
Durante la clonación, la base de datos NTDS.DIT suele estar sin conexión durante largos períodos. El servicio ADWS registra al menos un evento para esto. Una vez completa la clonación, el servicio ADWS se inicia, anota que aún no hay un certificado de equipo válido (podría haberlo o no según si tu entorno implementa un PKI de Microsoft con inscripción automática o no) y, después, inicia la instancia del nuevo controlador de dominio.  

||||  
|-|-|-|  
|**ID. de evento**|**Origen**|**Mensaje**|  
|**1202**|Eventos de instancias de ADWS|Este equipo hospeda ahora la instancia de directorio especificada, pero Servicios web de Active Directory no pudo atenderla. Servicios web de Active Directory reintentará esta operación periódicamente.<br /><br />Instancia de directorio: NTDS<br /><br />Puerto LDAP de instancia de directorio: 389<br /><br />Puerto SSL de instancia de directorio: 636|  
|**1000**|Eventos de instancias de ADWS|Se está iniciando Servicios web de Active Directory|  
|**1008**|Eventos de instancias de ADWS|Servicios web de Active Directory redujo correctamente sus privilegios de seguridad|  
|**1100**|Eventos de instancias de ADWS|Los valores especificados en la sección <appsettings> del archivo de configuración de Servicios web de Active Directory se cargó sin errores.|  
|**1400**|Eventos de instancias de ADWS|Servicios web de Active Directory no encontró ningún certificado de servidor con el nombre de certificado especificado. Se requiere un certificado para usar conexiones SSL/TLS. Para usar conexiones SSL/TLS, comprueba que haya un certificado de autenticación de servidor válido de una entidad de certificación (CA) de confianza instalado en el equipo.<br /><br />Nombre del certificado: *<Server FQDN>*|  
|**1100**|Eventos de instancias de ADWS|Los valores especificados en la sección <appsettings> del archivo de configuración de Servicios web de Active Directory se cargó sin errores.|  
|**1200**|Eventos de instancias de ADWS|Servicios web de Active Directory está atendiendo a la instancia de directorio especificada.<br /><br />Instancia de directorio: NTDS<br /><br />Puerto LDAP de instancia de directorio: 389<br /><br />Puerto SSL de instancia de directorio: 636|  

##### <a name="dns-server-event-log"></a>Registro de eventos del servidor DNS  
El servicio DNS experimentará breves interrupciones previstas durante la clonación, porque el servicio DNS sigue ejecutándose mientras la base de datos de AD DS está sin conexión. Esto sucede si se usa DSN integrado en Active Directory, pero no si se usa DSN principal o secundario estándar. Estos errores se registran varias veces. Una vez completa la clonación, DNS vuelve a conectarse normalmente.  

||||  
|-|-|-|  
|**ID. de evento**|**Origen**|**Mensaje**|  
|**4013**|DNS-Server-Service|El servidor DNS está esperando a que Active Directory Domain Services (AD DS) señalen que se ha completado la sincronización inicial del directorio. El servicio del servidor DNS no puede iniciarse hasta que se haya completado la sincronización inicial porque es posible que todavía no se hayan replicado en este controlador de dominio algunos datos de DNS críticos. Si los eventos del registro de eventos de AD DS indican que hay un problema con la resolución de nombres DNS, puede agregar la dirección IP de otro servidor DNS para este dominio a la lista de servidores DNS en las propiedades de protocolo de Internet de este equipo. Este evento se registrará cada dos minutos hasta que AD DS haya señalado que la sincronización inicial se ha completado correctamente.|  
|**4015**|DNS-Server-Service|El servidor DNS encontró un error crítico en Active Directory. Comprueba que Active Directory esté funcionado correctamente. La información de depuración de error extendida (puede estar vacía) es """". Los datos del evento contienen el error.|  
|**4000**|DNS-Server-Service|El servidor DNS no pudo abrir Active Directory.  Este servidor DNS está configurado para obtener y usar información del directorio para esta zona y no puede cargarla sin él.  Comprueba que Active Directory esté funcionando correctamente y vuelve a cargar la zona. Los datos del evento son el código de error.|  
|**4013**|DNS-Server-Service|El servidor DNS está esperando a que Active Directory Domain Services (AD DS) señalen que se ha completado la sincronización inicial del directorio. El servicio del servidor DNS no puede iniciarse hasta que se haya completado la sincronización inicial porque es posible que todavía no se hayan replicado en este controlador de dominio algunos datos de DNS críticos. Si los eventos del registro de eventos de AD DS indican que hay un problema con la resolución de nombres DNS, puede agregar la dirección IP de otro servidor DNS para este dominio a la lista de servidores DNS en las propiedades de protocolo de Internet de este equipo. Este evento se registrará cada dos minutos hasta que AD DS haya señalado que la sincronización inicial se ha completado correctamente.|  
|**2**|DNS-Server-Service|El servidor DNS se ha iniciado.|  
|**4**|DNS-Server-Service|El servidor DNS completó la carga de zonas en segundo plano. Todas las zonas están disponibles para actualizaciones de DNS y transferencias de zona según lo permitido por su configuración de zona individual.|  

##### <a name="file-replication-service-event-log"></a>Registro de eventos del Servicio de replicación de archivos  
El Servicio de replicación de archivos realiza una sincronización no autoritativa de un asociado durante la clonación. Para ello, la clonación elimina los archivos de base de datos de NTFRS y deja el contenido de SYSVOL intacto, para usarlo como datos previos a la inicialización. Se esperan dos intentos de sincronización.  


|              |            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|--------------|------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ID. de evento** | **Origen** |                                                                                                                                                                                                                                                                                                                                                                                                                  **Mensaje**                                                                                                                                                                                                                                                                                                                                                                                                                  |
|  **13562**   |   NtFrs    |                                                                                                                                                                                                                                                                            El siguiente resumen muestra las advertencias y errores que el Servicio de replicación de archivos encuentra mientras sondea DC2.root.fabrikam.com del controlador de dominio para buscar información sobre la configuración del conjunto de réplicas de FRS.<br /><br />No se pudo enlazar con un controlador de dominio. Se volverá a intentar en el próximo ciclo de sondeo.                                                                                                                                                                                                                                                                            |
|  **13502**   |   NtFrs    |                                                                                                                                                                                                                                                                                                                                                                                                   El Servicio de replicación de archivos se está deteniendo.                                                                                                                                                                                                                                                                                                                                                                                                   |
|  **13565**   |   NtFrs    |                                                                         El Servicio de réplica de archivos está inicializando el volumen del sistema con datos de otro controlador de dominio. El equipo DC2 no se puede convertir en un controlador de dominio hasta que este proceso se haya completado. El volumen del sistema será entonces compartido como SYSVOL.<br /><br />Para comprobar el recurso compartido SYSVOL, en el símbolo del sistema, escribe:<br /><br />net share<br /><br />Cuando el Servicio de réplica de archivos complete el proceso de inicialización, aparecerá el recurso compartido SYSVOL.<br /><br />La inicialización del volumen de sistema puede tardar un poco. El tiempo depende de la cantidad de datos en el volumen de sistema, la disponibilidad de otros controladores de dominio, y el intervalo de replicación entre controladores de dominio.                                                                         |
|  **13501**   |   NtFrs    |                                                                                                                                                                                                                                                                                                                                                                                                   El Servicio de replicación de archivos se está iniciando.                                                                                                                                                                                                                                                                                                                                                                                                    |
|  **13502**   |   NtFrs    |                                                                                                                                                                                                                                                                                                                                                                                                   El Servicio de replicación de archivos se está deteniendo.                                                                                                                                                                                                                                                                                                                                                                                                   |
|  **13503**   |   NtFrs    |                                                                                                                                                                                                                                                                                                                                                                                                   El Servicio de replicación de archivos se ha detenido.                                                                                                                                                                                                                                                                                                                                                                                                   |
|  **13565**   |   NtFrs    |                                                                         El Servicio de réplica de archivos está inicializando el volumen del sistema con datos de otro controlador de dominio. El equipo DC2 no se puede convertir en un controlador de dominio hasta que este proceso se haya completado. El volumen del sistema será entonces compartido como SYSVOL.<br /><br />Para comprobar el recurso compartido SYSVOL, en el símbolo del sistema, escribe:<br /><br />net share<br /><br />Cuando el Servicio de réplica de archivos complete el proceso de inicialización, aparecerá el recurso compartido SYSVOL.<br /><br />La inicialización del volumen de sistema puede tardar un poco. El tiempo depende de la cantidad de datos en el volumen de sistema, la disponibilidad de otros controladores de dominio, y el intervalo de replicación entre controladores de dominio.                                                                         |
|  **13501**   |   NtFrs    |                                                                                                                                                                                                                                                                                                                                                                                                   El Servicio de replicación de archivos se está iniciando.                                                                                                                                                                                                                                                                                                                                                                                                   |
|  **13553**   |   NtFrs    |                                                                                                                                                                          El servicio de replicación de archivos agregó correctamente este equipo al conjunto de replicas siguientes:<br /><br />"DOMAIN SYSTEM VOLUME (SYSVOL SHARE)"<br /><br />La información relacionada con este evento se muestra a continuación:<br /><br />El nombre DNS del equipo es *<Domain Controller FQDN>*<br /><br />El nombre del miembro del conjunto de réplicas es *<Domain Controller>*<br /><br />La ruta de acceso raíz del conjunto de réplicas es *<path>*<br /><br />La ruta del directorio de ensayo de réplica es *<path>*<br /><br />La ruta de acceso del directorio de trabajo de réplica es *<path>*                                                                                                                                                                           |
|  **13520**   |   NtFrs    |                El servicio de replicación de archivos desplazó los archivos preexistentes en <path>a *<path>* \ NtFrs_PreExisting___See_EventLog.<br /><br />El servicio de replicación de archivos puede eliminar los archivos de *<path>* \ NtFrs_PreExisting___See_EventLog en cualquier momento. Los archivos se pueden guardar de la eliminación copiándolos fuera de *<path>* \ NtFrs_PreExisting___See_EventLog. Si los copias en c:\windows\sysvol\domain, pueden aparecer conflictos con los nombres de archivos que ya existen en otro replicador.<br /><br />En algunos casos, el servicio de replicación de archivos puede copiar un archivo de *<path>* \ NtFrs_PreExisting___See_EventLog en *<path>* en lugar de replicar el archivo desde otro asociado de replicación.<br /><br />Se puede recuperar el espacio en cualquier momento eliminando los archivos de *<path>* \ NtFrs_PreExisting___See_EventLog ".                 |
|  **13508**   |   NtFrs    | el servicio de replicación de archivos tiene problemas para habilitar la replicación desde *\\\\<Domain Controller FQDN>* a *<Domain Controller>* para *<path>* mediante el<br /><br />Nombre DNS *\\\\<Domain Controller FQDN>* . FRS continuará reintentando.<br /><br />A continuación observarás algunas de las razones por las que aparecerá esta advertencia.<br /><br />[1] FRS no puede resolver correctamente el nombre DNS *\\\\<Domain Controller FQDN>* desde este equipo.<br /><br />[2] FRS no se está ejecutando en *\\\\<Domain Controller FQDN>* .<br /><br />[3] La información de topología de esta replicación en Active Directory aún no ha sido replicada a todos los controladores de dominio.<br /><br />Este mensaje de registro de eventos aparecerá una sola vez para cada conexión. Una vez que se haya resuelto el problema volverás a ver otro mensaje de registro de eventos que indica que la conexión se ha establecido. |
|  **13509**   |   NtFrs    |                                                                                                                                                                                                                                                                                                                                            El servicio de replicación de archivos ha habilitado la replicación desde *\\\\<Domain Controller FQDN>* a *<Domain Controller>* para *<Path>* después de reintentos repetidos.                                                                                                                                                                                                                                                                                                                                             |
|  **13516**   |   NtFrs    |                                                                                                                                                                                                                                               El servicio de replicación de archivos ya no impide que el equipo *<Domain Controller>* se convierta en un controlador de dominio. Se ha inicializado correctamente el volumen de sistema y se ha notificado al servicio de Net Logon de que dicho volumen está preparado para compartirse como SYSVOL.<br /><br />Escribe "net share" para comprobar el recurso compartido SYSVOL.                                                                                                                                                                                                                                               |

##### <a name="dfs-replication-event-log"></a>Registro de eventos de Replicación DFS  
Los servicios DFSR realizan una sincronización no autoritativa de un asociado durante la clonación. Para ello, la clonación elimina los archivos de base de datos de DFSR y deja el contenido de SYSVOL intacto, para usarlo como datos previos a la inicialización. Se esperan dos intentos de sincronización.  


|              |            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|--------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ID. de evento** | **Origen** |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           **Mensaje**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|   **1004**   |    DFSR    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            El servicio de replicación DFS se ha iniciado.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|   **1314**   |    DFSR    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  El servicio de replicación DFS configuró correctamente los archivos de registro de depuración.<br /><br />Más información:<br /><br />Ruta de acceso del archivo de registro de depuración: C:\Windows\debug                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|   **6102**   |    DFSR    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            El servicio de replicación DFS registró correctamente el proveedor WMI                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|   **1206**   |    DFSR    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 El servicio de replicación DFS pudo establecer contacto con el controlador de dominio DC2.corp.contoso.com para tener acceso a la información de configuración.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|   **1210**   |    DFSR    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    El servicio de replicación DFS estableció una escucha RPC para las solicitudes de replicación entrantes.<br /><br />Más información:<br /><br />Puerto: 0 "                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|   **4614**   |    DFSR    | El servicio de replicación DFS inicializó SYSVOL en la ruta local C:\Windows\SYSVOL\domain y está esperando realizar la replicación inicial. La carpeta replicada permanecerá en el estado inicial de sincronización hasta que se replique con el asociado. Si el servidor se estaba ascendiendo a un controlador de dominio, el controlador de dominio no se anunciará ni funcionará como tal hasta que este problema se resuelva. Esto puede suceder si el asociado especificado también está en el estado inicial de sincronización, o si se detectaron infracciones de uso compartido en este servidor o en el asociado de sincronización. Si este evento se presentó durante la migración de SYSVOL del servicio Replicación de archivos (FRS) a la Replicación DFS, los cambios no se replicarán a menos que este problema se resuelva. Esto puede provocar que la carpeta SYSVOL en este servidor deje de estar sincronizada con los demás controladores de dominio.<br /><br />Más información:<br /><br />Nombre de la carpeta replicada: SYSVOL share<br /><br />IDENTIFICADOR de la carpeta replicada: *<GUID>*<br /><br />Nombre del grupo de replicación: volumen del sistema de dominio<br /><br />IDENTIFICADOR de grupo de replicación: *<GUID>*<br /><br />IDENTIFICADOR de miembro: *<GUID>*<br /><br />Solo lectura: 0 |
|   **4604**   |    DFSR    |                                                                                                                                                                                                                                                                      El servicio Replicación DFS inicializó correctamente la carpeta replicada SYSVOL en la ruta local C:\Windows\SYSVOL\domain. Este miembro completó la sincronización inicial de SYSVOL con el asociado dc1.corp.contoso.com.  Para comprobar la presencia de una carpeta SYSVOL, abre una ventana del símbolo del sistema y escribe ""net share"".<br /><br />Más información:<br /><br />Nombre de la carpeta replicada: SYSVOL share<br /><br />IDENTIFICADOR de la carpeta replicada: *<GUID>*<br /><br />Nombre del grupo de replicación: volumen del sistema de dominio<br /><br />IDENTIFICADOR de grupo de replicación: *<GUID>*<br /><br />IDENTIFICADOR de miembro: *<GUID>*<br /><br />Asociado de sincronización: *<domain controller FQDN>*                                                                                                                                                                                                                                                                       |

## <a name="BKMK_TshootVDCSafeRestore"></a>Solucionar problemas de restauración segura de controladores de dominio virtualizados  

### <a name="tools-for-troubleshooting"></a>Herramientas para la solución de problemas  

#### <a name="logging-options"></a>Opciones de registro  
Los registros integrados son la herramienta más importante para solucionar problemas de restauración segura de instantáneas. Todos estos registros están habilitados y configurados para ofrecer el máximo nivel de detalle de forma predeterminada.  

|||  
|-|-|  
|**Sesión**|**Inicia**|  
|**Creación de instantáneas**|-Event visor eventos\registros and Services logs\Microsoft\Windows\Hyper-V-Worker|  
|**Restauración de instantáneas**|-Servicio de visor eventos\registros de eventos y servicios de logs\Directory<br />-Event Eventos\registros logs\System<br />-Event Eventos\registros \ aplicación<br />-Event visor eventos\registros and Services logs\File Replication Service<br />-Visor eventos\registros de eventos y servicios de logs\DFS replicación<br />-Event visor eventos\registros and Services logs\DNS<br />-Event visor eventos\registros and Services logs\Microsoft\Windows\Hyper-V-Worker|  

#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Herramientas y comandos para solucionar problemas de configuración de controladores de dominio  
Para solucionar problemas que no se explican con los registros, usa las herramientas siguientes como punto de partida:  

-   Dcdiag.exe  

-   Repadmin.exe  

-   Monitor de red 3.4  

#### <a name="BKMK_TshhotSafeRestore"></a>Metodología general para solucionar problemas de restauración segura de controladores de dominio  

1.  ¿La restauración segura de instantáneas era previsible pero tiene problemas?  

    1.  Examina el registro de eventos de servicios de directorio.  

        1.  ¿Hay errores de restauración de instantáneas?  

        2.  ¿Hay errores de replicación de AD?  

    2.  Examina el registro de eventos del sistema.  

        1.  ¿Hay errores de comunicación?  

        2.  ¿Hay errores de AD?  

2.  ¿La restauración segura de instantáneas no era previsible?  

    1.  Examina los registros de auditoría del hipervisor para determinar quién o qué causó la reversión.  

    2.  Ponte en contacto con todos los administradores del hipervisor y pregúntales quién revirtió la máquina virtual sin notificarlo.  

3.  ¿El servidor está implementando protección de la reversión de USN y no se está restaurando de forma segura?  

    1.  Busca en el registro de eventos de servicios de directorio si hay un hipervisor o servicios de integración no compatibles.  

    2.  Examina el sistema operativo y comprueba que se está ejecutando Windows Server 2012.  

### <a name="BKMK_TshootSpecificSafeRestore"></a>Solución de problemas específicos  

#### <a name="events"></a>Eventos  
Todos los eventos de restauración segura de instantáneas de controladores de dominio virtualizados se escriben en el registro de eventos de Servicios de directorio de la máquina virtual del controlador de dominio restaurado. Los registros de eventos de Aplicación, Sistema, Servicio de replicación de archivos y Replicación DFS pueden contener también información útil para solucionar problemas de clonación.  

Los siguientes son eventos específicos de la restauración segura de Windows Server 2012 en el registro de eventos de Servicios de directorio.  


|                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                                                                                                                                                                                                                                                              **2170**                                                                                                                                                                                                                                                                                                                                                              |
|        **Origen**        |                                                                                                                                                                                                                                                                                                                                          Microsoft-Windows-ActiveDirectory_DomainService                                                                                                                                                                                                                                                                                                                                           |
|       **Gravedad**       |                                                                                                                                                                                                                                                                                                                                                              Advertencia                                                                                                                                                                                                                                                                                                                                                               |
|       **Mensaje**        | Se detectó un cambio de id. de generación.<br /><br />Id. de generación almacenado en caché en DS (valor antiguo):%1<br /><br />Id. de generación actualmente en VM (valor nuevo):%2<br /><br />El cambio de id. de generación se produce después de la aplicación de una instantánea de máquina virtual, después de una operación de importación de máquina virtual o después de una operación de migración en vivo. *<COMPUTERNAME>* creará un nuevo identificador de invocación para recuperar el controlador de dominio. Los controladores de dominio virtualizados no deben restaurarse con instantáneas de máquina virtual. El método admitido para restaurar o revertir el contenido de una base de datos de Active Directory Domain Services consiste en restaurar una copia de seguridad del estado del sistema realizada con una aplicación de copia de seguridad compatible con Active Directory Domain Services. |
| **Notas y resolución** |                                                                                                                                                                                                                                                                                            Este es un evento de procedimiento correcto si la instantánea era previsible. Si no lo es, examina el registro de eventos de Hyper-V-Worker o ponte en contacto con el administrador del hipervisor.                                                                                                                                                                                                                                                                                             |

|||  
|-|-|  
|**ID. de evento**|**2174**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|El controlador de dominio no es un clon de controlador de dominio virtual ni una instantánea de un controlador de dominio virtual restaurado.|  
|**Notas y resolución**|Evento previsible cuando se inician controladores de dominio físicos o controladores de dominio virtualizados no restaurados a partir de la instantánea.|  

|||  
|-|-|  
|**ID. de evento**|**2181**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|La transacción se anuló debido a que una máquina virtual se revirtió a un estado anterior.  Esto se produce después de la aplicación de una instantánea de máquina virtual, después de una operación de importación de máquina virtual o después de una operación de migración en vivo.|  
|**Notas y resolución**|Es previsible cuando se restaura una instantánea. Las transacciones rastrean los cambios de identificadores de generación de máquina virtual.|  

|                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                                                                                                                                                                                                                                                                                **2185**                                                                                                                                                                                                                                                                                                                                                                                 |
|        **Origen**        |                                                                                                                                                                                                                                                                                                                                                             Microsoft-Windows-ActiveDirectory_DomainService                                                                                                                                                                                                                                                                                                                                                             |
|       **Gravedad**       |                                                                                                                                                                                                                                                                                                                                                                              Informativo                                                                                                                                                                                                                                                                                                                                                                              |
|       **Mensaje**        |                                                                                      *<COMPUTERNAME>* detuvo el servicio FRS o DFSR usado para replicar la carpeta Sysvol.<br /><br />Nombre del servicio:%1<br /><br />Active Directory detectó que la máquina virtual que hospeda el controlador de dominio se revirtió a un estado anterior. *<COMPUTERNAME>* debe inicializar una restauración no autoritativa en la réplica de SYSVOL local. Para ello, se debe detener el servicio de replicación FRS o DFSR que se usa para replicar la carpeta SYSVOL y, a continuación, iniciarlo con los valores y las claves del Registro adecuados para desencadenar la restauración. Se registrará el evento 2187 cuando el servicio FRS o DFSR se reinicie.                                                                                       |
| **Notas y resolución** |                                                                                                                                                                                                                                                                                                                           Es previsible cuando se restaura una instantánea. Todos los datos de SYSVOL de este controlador de dominio se reemplazan con una copia de un controlador de dominio del asociado.                                                                                                                                                                                                                                                                                                                           |
|       **ID. de evento**       |                                                                                                                                                                                                                                                                                                                                                                                  2186                                                                                                                                                                                                                                                                                                                                                                                   |
|        **Origen**        |                                                                                                                                                                                                                                                                                                                                                             Microsoft-Windows-ActiveDirectory_DomainService                                                                                                                                                                                                                                                                                                                                                             |
|       **Gravedad**       |                                                                                                                                                                                                                                                                                                                                                                                  Error                                                                                                                                                                                                                                                                                                                                                                                  |
|       **Mensaje**        | *<COMPUTERNAME>* no pudo detener el servicio FRS o DFSR usado para replicar la carpeta Sysvol.<br /><br />Nombre del servicio:%1<br /><br />Código de error: %2<br /><br />Mensaje de error: %3<br /><br />Active Directory detectó que la máquina virtual que hospeda el controlador de dominio se revirtió a un estado anterior. *<COMPUTERNAME>* debe inicializar una restauración no autoritativa en la réplica de SYSVOL local. Para ello, se debe detener el servicio de replicación FRS o DFSR que se usa para replicar la carpeta SYSVOL y, a continuación, iniciarlo con los valores y las claves del Registro adecuados para desencadenar la restauración. *<COMPUTERNAME>* no pudo detener el servicio en ejecución actual y no puede completar la restauración no autoritativa. Ejecuta una restauración no autoritativa manualmente. |
| **Notas y resolución** |                                                                                                                                                                                                                                                                                                                                                  Examina los registros de eventos de Sistema, FRS y DFSR para obtener más información.                                                                                                                                                                                                                                                                                                                                                   |

|                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                                                                                                                                                           **2187**                                                                                                                                                                                                                                                           |
|        **Origen**        |                                                                                                                                                                                                                                       Microsoft-Windows-ActiveDirectory_DomainService                                                                                                                                                                                                                                        |
|       **Gravedad**       |                                                                                                                                                                                                                                                        Informativo                                                                                                                                                                                                                                                         |
|       **Mensaje**        | *<COMPUTERNAME>* inició el servicio FRS o DFSR usado para replicar la carpeta Sysvol.<br /><br />Nombre del servicio:%1<br /><br />Active Directory detectó que la máquina virtual que hospeda el controlador de dominio se revirtió a un estado anterior. *<COMPUTERNAME>* necesario para inicializar una restauración no autoritativa en la réplica de SYSVOL local. Para ello, se tuvo que detener el servicio FRS o DFSR que se usa para replicar la carpeta SYSVOL y, a continuación, iniciarlo con los valores y las claves del Registro adecuados para desencadenar la restauración. |
| **Notas y resolución** |                                                                                                                                                                                                     Es previsible cuando se restaura una instantánea. Todos los datos de SYSVOL de este controlador de dominio se reemplazan con una copia de un controlador de dominio del asociado.                                                                                                                                                                                                      |

|                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                                                                                                                                                                                                                                                                                                 **2188**                                                                                                                                                                                                                                                                                                                                                                                                  |
|        **Origen**        |                                                                                                                                                                                                                                                                                                                                                                              Microsoft-Windows-ActiveDirectory_DomainService                                                                                                                                                                                                                                                                                                                                                                              |
|       **Gravedad**       |                                                                                                                                                                                                                                                                                                                                                                                                   Error                                                                                                                                                                                                                                                                                                                                                                                                   |
|       **Mensaje**        | *<COMPUTERNAME>* no pudo iniciar el servicio FRS o DFSR usado para replicar la carpeta Sysvol.<br /><br />Nombre del servicio:%1<br /><br />Código de error: %2<br /><br />Mensaje de error: %3<br /><br />Active Directory detectó que la máquina virtual que hospeda el controlador de dominio se revirtió a un estado anterior. *<COMPUTERNAME>* necesita inicializar una restauración no autoritativa en la réplica de SYSVOL local. Para DFSR, se debe detener el servicio DFSR, eliminar las bases de datos de DFSR y reiniciar el servicio. Una vez reiniciado, DFSR volverá a generar las bases de datos e iniciará la sincronización inicial. *<COMPUTERNAME>* no pudo iniciar el servicio FRS o DFSR usado para replicar la carpeta Sysvol y no puede completar la restauración no autoritativa. Ejecuta una restauración no autoritativa manualmente y reinicia el servicio. |
| **Notas y resolución** |                                                                                                                                                                                                                                                                                                                                                                   Examina los registros de eventos de Sistema, FRS y DFSR para obtener más información.                                                                                                                                                                                                                                                                                                                                                                    |

|                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                                                                                                                                                                                                         **2189**                                                                                                                                                                                                                                                                                                          |
|        **Origen**        |                                                                                                                                                                                                                                                                                      Microsoft-Windows-ActiveDirectory_DomainService                                                                                                                                                                                                                                                                                      |
|       **Gravedad**       |                                                                                                                                                                                                                                                                                                       Informativo                                                                                                                                                                                                                                                                                                       |
|       **Mensaje**        | *<COMPUTERNAME>* establecer los siguientes valores del registro para inicializar la réplica SYSVOL durante una restauración no autoritativa:<br /><br />Clave del Registro:%1<br /><br />Valor del Registro: %2<br /><br />Datos del valor del Registro: %3<br /><br />Active Directory detectó que la máquina virtual que hospeda el controlador de dominio se revirtió a un estado anterior. *<COMPUTERNAME>* necesita inicializar una restauración no autoritativa en la réplica de SYSVOL local. Para ello, hay que detener el servicio FRS o DFSR usado para replicar la carpeta SYSVOL e iniciarlo con las claves y los valores del Registro adecuados para desencadenar la restauración. |
| **Notas y resolución** |                                                                                                                                                                                                                                                    Es previsible cuando se restaura una instantánea. Todos los datos de SYSVOL de este controlador de dominio se reemplazan con una copia de un controlador de dominio del asociado.                                                                                                                                                                                                                                                    |

|                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                                                                                                                                                                                                                                                                                                                              **2190**                                                                                                                                                                                                                                                                                                                                                                                                                              |
|        **Origen**        |                                                                                                                                                                                                                                                                                                                                                                                                          Microsoft-Windows-ActiveDirectory_DomainService                                                                                                                                                                                                                                                                                                                                                                                                           |
|       **Gravedad**       |                                                                                                                                                                                                                                                                                                                                                                                                                               Error                                                                                                                                                                                                                                                                                                                                                                                                                                |
|       **Mensaje**        | *<COMPUTERNAME>* no pudo establecer los siguientes valores del registro para inicializar la réplica de SYSVOL durante una restauración no autoritativa:<br /><br />Clave del Registro:%1<br /><br />Valor del Registro: %2<br /><br />Datos del valor del Registro: %3<br /><br />Código de error: % 4<br /><br />Mensaje de error: %5<br /><br />Active Directory detectó que la máquina virtual que hospeda el rol de controlador de dominio se revirtió a un estado anterior. *<COMPUTERNAME>* necesita inicializar una restauración no autoritativa en la réplica de SYSVOL local. Para ello, hay que detener el servicio FRS o DFSR usado para replicar la carpeta SYSVOL e iniciarlo con las claves y los valores del Registro adecuados para desencadenar la restauración. *<COMPUTERNAME>* no pudo establecer los valores del registro anteriores y no puede completar la restauración no autoritativa. Ejecuta una restauración no autoritativa manualmente. |
| **Notas y resolución** |                                                                                                                                                                                                                                                                                                                                                                       Examina los registros de eventos de aplicación y del sistema. Investiga aplicaciones de terceros que pudieran estar bloqueando las actualizaciones del Registro.                                                                                                                                                                                                                                                                                                                                                                       |

|                          |                                                                                                                                                                                                                                                                    |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                              **2200**                                                                                                                              |
|        **Origen**        |                                                                                                          Microsoft-Windows-ActiveDirectory_DomainService                                                                                                           |
|       **Gravedad**       |                                                                                                                           Informativo                                                                                                                            |
|       **Mensaje**        | Active Directory detectó que la máquina virtual que hospeda el controlador de dominio se revirtió a un estado anterior. *<COMPUTERNAME>* inicializa la replicación para actualizar el controlador de dominio. Se registrará el evento 2201 cuando la replicación finalice. |
| **Notas y resolución** |                                                                                         Es previsible cuando se restaura una instantánea. Marca el comienzo de una replicación de AD de entrada.                                                                                         |

|                          |                                                                                                                                                                                                         |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                **2201**                                                                                                 |
|        **Origen**        |                                                                             Microsoft-Windows-ActiveDirectory_DomainService                                                                             |
|       **Gravedad**       |                                                                                              Informativo                                                                                              |
|       **Mensaje**        | Active Directory detectó que la máquina virtual que hospeda el controlador de dominio se revirtió a un estado anterior. *<COMPUTERNAME>* ha finalizado la replicación para actualizar el controlador de dominio. |
| **Notas y resolución** |                                                              Es previsible cuando se restaura una instantánea. Marca el final de una replicación de AD de entrada.                                                               |

|                          |                                                                                                                                                                                                                                                                             |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                                  **2202**                                                                                                                                   |
|        **Origen**        |                                                                                                               Microsoft-Windows-ActiveDirectory_DomainService                                                                                                               |
|       **Gravedad**       |                                                                                                                                    Error                                                                                                                                    |
|       **Mensaje**        | Active Directory detectó que la máquina virtual que hospeda el controlador de dominio se revirtió a un estado anterior. *<COMPUTERNAME>* produjo un error en la replicación para actualizar el controlador de dominio. El controlador de dominio se actualizará después de la próxima replicación periódica. |
| **Notas y resolución** |                                                                        Examina el registro de eventos de Servicios de directorio y Sistema. Usa repadmin.exe para intentar forzar la replicación y anota los posibles errores.                                                                         |

|                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                                                                                                                                                                                                                                                                                                                   **2204**                                                                                                                                                                                                                                                                                                                                                                                                                    |
|        **Origen**        |                                                                                                                                                                                                                                                                                                                                                                                                Microsoft-Windows-ActiveDirectory_DomainService                                                                                                                                                                                                                                                                                                                                                                                                |
|       **Gravedad**       |                                                                                                                                                                                                                                                                                                                                                                                                                 Informativo                                                                                                                                                                                                                                                                                                                                                                                                                 |
|       **Mensaje**        | *<COMPUTERNAME>* ha detectado un cambio de identificador de generación de la máquina virtual. El cambio significa que el controlador de dominio virtual se revirtió a un estado anterior. *<COMPUTERNAME>* realizará las siguientes operaciones para proteger el controlador de dominio revertido contra una posible divergencia de datos y para proteger la creación de entidades de seguridad con SID duplicados:<br /><br />Crear un id. de invocación<br /><br />Invalidar el grupo RID actual<br /><br />La propiedad de los roles FSMO se validará en la próxima replicación de entrada. Durante este intervalo, si el controlador de dominio tuvo un rol FSMO, dicho rol no estará disponible.<br /><br />Inicia la operación de restauración del servicio de replicación de SYSVOL.<br /><br />Inicie la replicación para actualizar el controlador de dominio revertido al estado más actual.<br /><br />Solicite un nuevo grupo RID. |
| **Notas y resolución** |                                                                                                                                                                                                                                                                                                                                                    Es previsible cuando se restaura una instantánea. Esto explica todas las operaciones de restablecimiento que se producen como parte del proceso de restauración segura.                                                                                                                                                                                                                                                                                                                                                    |

|                          |                                                                                                                                                             |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                          **2205**                                                                           |
|        **Origen**        |                                                       Microsoft-Windows-ActiveDirectory_DomainService                                                       |
|       **Gravedad**       |                                                                        Informativo                                                                        |
|       **Mensaje**        |                        *<COMPUTERNAME>* grupo RID actual invalidado después de que el controlador de dominio virtual se revirtió al estado anterior.                        |
| **Notas y resolución** | Es previsible cuando se restaura una instantánea. El grupo de RID local debe destruirse porque el controlador de dominio ha viajado en el tiempo y puede que ya se hayan emitido. |

|                          |                                                                                                                                                                                                         |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                **2206**                                                                                                 |
|        **Origen**        |                                                                             Microsoft-Windows-ActiveDirectory_DomainService                                                                             |
|       **Gravedad**       |                                                                                                  ERROR                                                                                                  |
|       **Mensaje**        | *<COMPUTERNAME>* no pudo invalidar el grupo RID actual después de que el controlador de dominio virtual se revirtió al estado anterior.<br /><br />Datos adicionales:<br /><br />Código de error: %1<br /><br />Valor del error: %2 |
| **Notas y resolución** |                     Examina el registro de eventos de Servicios de directorio y Sistema. Comprueba que el maestro RID está conectado y es accesible desde este servidor usando Dcdiag.exe /test:ridmanager                      |

|                          |                                                                                                                                                                                         |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                        **2207**                                                                                         |
|        **Origen**        |                                                                     Microsoft-Windows-ActiveDirectory_DomainService                                                                     |
|       **Gravedad**       |                                                                                          ERROR                                                                                          |
|       **Mensaje**        | *<COMPUTERNAME>* no se pudo restaurar después de que el controlador de dominio virtual se revirtió al estado anterior. Se necesitó reiniciar en DSRM. Comprueba los eventos anteriores para obtener más información. |
| **Notas y resolución** |                                                                  Examina el registro de eventos de Servicios de directorio y Sistema.                                                                  |

|                          |                                                                                                                                                                                                                                                                                                                                 |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                                                            **2208**                                                                                                                                                             |
|        **Origen**        |                                                                                                                                         Microsoft-Windows-ActiveDirectory_DomainService                                                                                                                                         |
|       **Gravedad**       |                                                                                                                                                          Informativo                                                                                                                                                          |
|       **Mensaje**        |                                                                                                            *<COMPUTERNAME>* eliminar las bases de datos de DFSR para inicializar la réplica de SYSVOL durante una restauración no autoritativa.                                                                                                             |
| **Notas y resolución** | Es previsible cuando se restaura una instantánea. Esto garantiza que DFSR realiza una sincronización no autoritativa desde un controlador de dominio asociado. Ten en cuenta que todas las demás carpetas replicadas de DFSR que estén en el mismo volumen que SYSVOL también se sincronizarán de forma autoritativa (no se recomienda que los controladores de dominio hospeden conjuntos de DFSR personalizados en el mismo volumen que SYSVOL). |

|                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **ID. de evento**       |                                                                                                                                                                                                                                                                         **2209**                                                                                                                                                                                                                                                                         |
|        **Origen**        |                                                                                                                                                                                                                                                     Microsoft-Windows-ActiveDirectory_DomainService                                                                                                                                                                                                                                                      |
|       **Gravedad**       |                                                                                                                                                                                                                                                                          Error                                                                                                                                                                                                                                                                           |
|       **Mensaje**        | *<COMPUTERNAME>* no pudo eliminar las bases de datos de DFSR.<br /><br />Datos adicionales:<br /><br />Código de error: %1<br /><br />Valor del error: %2<br /><br />Active Directory detectó que la máquina virtual que hospeda el controlador de dominio se revirtió a un estado anterior. *<COMPUTERNAME>* necesita inicializar una restauración no autoritativa en la réplica de SYSVOL local. Para DFSR, se debe detener el servicio DFSR, eliminar las bases de datos de DFSR y reiniciar el servicio. Una vez reiniciado, DFSR volverá a generar las bases de datos e iniciará la sincronización inicial. |
| **Notas y resolución** |                                                                                                                                                                                                                                                               Examina el registro de eventos de DFSR.                                                                                                                                                                                                                                                                |

#### <a name="error-messages"></a>Mensajes de error  
No hay errores interactivos directos en caso de error de la restauración segura de instantáneas de controladores de dominio virtualizados en los registros de eventos de Servicios de directorio. Los errores críticos de replicación o de publicación de servidores se manifiestan como síntomas en otros lugares.  

#### <a name="known-issues-and-support-scenarios"></a>Problemas conocidos y escenarios de soporte  
La [metodología general para solucionar problemas de restauración segura de controladores de dominio](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootSpecificSafeRestore) suele ser adecuada para solucionar la mayoría de los problemas.  

|||  
|-|-|  
|**Problema**|**No se pueden crear nuevas entidades de seguridad en el controlador de dominio restaurado seguro recientemente**|  
|**Síntomas**|Después de restaurar una instantánea, se producirá un error al intentar crear una entidad de seguridad nueva (usuario, equipo, grupo) en el controlador de dominio con:<br /><br />Error 0x2010<br /><br />El servicio de directorios no puede asignar un identificador relativo.|  
|**Resolución y notas**|Este problema se debe a que el conocimiento que el equipo restaurado tiene del rol FSMO de maestro RID está obsoleto. Si el rol se movió a este o a otro controlador de dominio después de tomar una instantánea y de restaurarla después, el controlador de dominio restaurado no tendrá conocimiento del maestro RID hasta que finalice la replicación.<br /><br />Para resolver el problema, deja que termine la replicación de AD de entrada en el controlador de dominio restaurado. Si sigue sin funcionar, comprueba que todos los controladores de dominio tengan el mismo conocimiento correcto de qué controlador de dominio hospeda el maestro RID.|  

|||  
|-|-|  
|**Problema**|**Los controladores de dominio restaurados no comparten SYSVOL, anuncian**|  
|**Síntomas**|Después de restaurar una instantánea, uno o varios controladores de dominio no se publicitan, no comparten sysvol y no tienen contenido de SYSVOL actualizado.|  
|**Resolución y notas**|Los asociados precedentes en la cadena del controlador de dominio no tienen una réplica de SYSVOL en funcionamiento que se esté replicando correctamente con DFSR o FRS. Este problema no está relacionado con la restauración segura sino que probablemente se está manifestando como un problema de restauración segura, porque el cliente no conocía el otro problema de replicación que afecta a los controladores de dominio no restaurados.|  

### <a name="advanced-troubleshooting"></a>Solución avanzada de problemas  
El objetivo de este módulo es enseñar la solución avanzada de problemas usando registros de *trabajo* como muestras, con algunas explicaciones de lo que ocurrió. Si comprendes cómo funciona correctamente un controlador de dominio virtualizado, los errores de tu entorno te resultarán más obvios. Estos registros se presentan por origen, con los eventos *previstos* relativos a un controlador de dominio dentro de cada registro, en orden ascendente.  

#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-dfsr"></a>Restaurar un controlador de dominio que replica SYSVOL mediante DFSR  

##### <a name="directory-services-event-log"></a>Registro de eventos de Servicios de directorio  
El registro de Servicios de directorio contiene la mayoría de la información operativa relativa a la restauración segura. El hipervisor cambia el id. de generación de VM y el servicio NTDS lo anota; después, invalida el grupo de RID y cambia el id. de invocación. Se establece el nuevo id. de generación de VM y los servidores replican los datos de AD de entrada. El servicio DFSR se detiene y se elimina su base de datos, que hospeda SYSVOL, lo que obliga a una sincronización de entrada no autoritativa. Se ajusta la marca de límite superior de USN.  


|              |                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|--------------|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ID. de evento** |          **Origen**           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           **Mensaje**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|   **2170**   | ActiveDirectory_DomainService |                                                                                                                                                                   Se detectó un cambio de id. de generación.<br /><br />Id. de generación almacenado en caché en DS (valor antiguo):<br /><br />*<number>*<br /><br />Id. de generación actualmente en VM (valor nuevo):<br /><br />*<number>*<br /><br />El cambio de id. de generación se produce después de la aplicación de una instantánea de máquina virtual, después de una operación de importación de máquina virtual o después de una operación de migración en vivo. Servicios de dominio de Active Directory creará un nuevo id. de invocación para recuperar el controlador de dominio. Los controladores de dominio virtualizados no deben restaurarse con instantáneas de máquina virtual. El método admitido para restaurar o revertir el contenido de una base de datos de Servicios de dominio de Active Directory consiste en restaurar una copia de seguridad del estado del sistema realizada con una aplicación de copia de seguridad compatible con Servicios de dominio de Active Directory.                                                                                                                                                                   |
|   **2181**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                                           La transacción se anuló debido a que una máquina virtual se revirtió a un estado anterior.  Esto se produce después de la aplicación de una instantánea de máquina virtual, después de una operación de importación de máquina virtual o después de una operación de migración en vivo.                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|   **2204**   | ActiveDirectory_DomainService |                                                                                                                          Los Servicios de dominio de Active Directory detectaron un cambio de id. de generación de la máquina virtual. El cambio significa que el controlador de dominio virtual se revirtió a un estado anterior. Active Directory Domain Services realizarán las siguientes operaciones para proteger el controlador de dominio revertido contra una posible divergencia de datos y para proteger la creación de entidades de seguridad con SID duplicados:<br /><br />Crear un id. de invocación<br /><br />Invalidar el grupo RID actual<br /><br />La propiedad de los roles FSMO se validará en la próxima replicación de entrada. Durante este intervalo, si el controlador de dominio tuvo un rol FSMO, dicho rol no estará disponible.<br /><br />Inicia la operación de restauración del servicio de replicación de SYSVOL.<br /><br />Inicia la replicación para actualizar el controlador de dominio revertido al estado más actual.<br /><br />Solicite un nuevo grupo RID.                                                                                                                          |
|   **2181**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                                           La transacción se anuló debido a que una máquina virtual se revirtió a un estado anterior.  Esto se produce después de la aplicación de una instantánea de máquina virtual, después de una operación de importación de máquina virtual o después de una operación de migración en vivo.                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|   **1109**   | ActiveDirectory_DomainService |                                                                   El atributo invocationID de este servidor de directorio ha cambiado. El número de secuencia de actualización mayor en el momento de crear la copia de seguridad es el siguiente:<br /><br />Atributo invocationID (valor anterior):<br /><br />*<GUID>*<br /><br />Atributo invocationID (valor nuevo):<br /><br />*<GUID>*<br /><br />Número de secuencias actualizadas:<br /><br />*<number>*<br /><br />El atributo invocationID cambia cuando un servidor de directorio se restaura desde medios de copia de seguridad, se configura para hospedar una partición de directorio de aplicaciones de escritura, se reanuda después de aplicar una instantánea de máquina virtual, después de una operación de importación de máquina virtual o después de una operación de migración en vivo. Los controladores de dominio virtualizados no deben restaurarse con instantáneas de máquina virtual. El método admitido para restaurar o revertir el contenido de una base de datos de Servicios de dominio de Active Directory consiste en restaurar una copia de seguridad del estado del sistema realizada con una aplicación de copia de seguridad compatible con Servicios de dominio de Active Directory.                                                                    |
|   **2179**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                                                                          El atributo msDS-GenerationId del objeto de equipo del controlador de dominio se estableció en el parámetro siguiente:<br /><br />Atributo GenerationID:<br /><br />*<number>*                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|   **2200**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                       Active Directory detectó que la máquina virtual que hospeda el controlador de dominio se revirtió a un estado anterior. Active Directory Domain Services inicializan una replicación para actualizar el controlador de dominio. Se registrará el evento 2201 cuando la replicación finalice.                                                                                                                                                                                                                                                                                                                                                                                                                        |
|   **2201**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                                                                                                                                                                                     Active Directory detectó que la máquina virtual que hospeda el controlador de dominio se revirtió a un estado anterior. Active Directory Domain Services finalizaron la replicación para actualizar el controlador de dominio.                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|   **2185**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                           Active Directory Domain Services detuvieron el servicio FRS o DFSR usado para replicar la carpeta SYSVOL.<br /><br />Nombre de servicio:<br /><br />DFSR<br /><br />Active Directory detectó que la máquina virtual que hospeda el controlador de dominio se revirtió a un estado anterior. Active Directory Domain Services deben inicializar una restauración no autoritativa en la réplica de SYSVOL local. Para ello, se debe detener el servicio de replicación FRS o DFSR que se usa para replicar la carpeta SYSVOL y, a continuación, iniciarlo con los valores y las claves del Registro adecuados para desencadenar la restauración. Se registrará el evento 2187 cuando el servicio FRS o DFSR se reinicie.                                                                                                                                                                                                                                           |
|   **2208**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                                Los Servicios de dominio de Active Directory eliminaron las bases de datos de DFSR para inicializar la réplica de SYSVOL durante una restauración no autoritativa.<br /><br />Active Directory detectó que la máquina virtual que hospeda el controlador de dominio se revirtió a un estado anterior. Active Directory Domain Services deben inicializar una restauración no autoritativa en la réplica de SYSVOL local. Para DFSR, se debe detener el servicio DFSR, eliminar las bases de datos de DFSR y reiniciar el servicio. Al reiniciar DFSR, se volverán a generar las bases de datos e iniciará la sincronización inicial. "                                                                                                                                                                                                                                                                                 |
|   **2187**   | ActiveDirectory_DomainService |                                                                                                                                                                                                                                                                          Active Directory Domain Services iniciaron el servicio FRS o DFSR usado para replicar la carpeta SYSVOL.<br /><br />Nombre de servicio:<br /><br />DFSR<br /><br />Active Directory detectó que la máquina virtual que hospeda el controlador de dominio se revirtió a un estado anterior. Los Servicios de dominio de Active Directory tuvieron que inicializar una restauración no autoritativa en la réplica de SYSVOL local. Para ello, se tuvo que detener el servicio FRS o DFSR que se usa para replicar la carpeta SYSVOL y, a continuación, iniciarlo con los valores y las claves del Registro adecuados para desencadenar la restauración. "                                                                                                                                                                                                                                                                           |
|   **1587**   | ActiveDirectory_DomainService | Este servicio de directorio se ha restaurado o se ha configurado para hospedar una partición de aplicación. Como resultado, su identidad de replicación ha cambiado. Un asociado ha solicitado cambios de replicación usando nuestra identidad antigua. Se ha ajustado el número de secuencia de inicio.<br /><br />El servicio de directorio de destino que corresponde al siguiente GUID del objeto ha solicitado cambios, empezando por un USN que precede al USN donde se restauró el servicio de directorio local desde el medio de copia de seguridad.<br /><br />GUID del objeto:<br /><br />*<GUID> (<FQDN of partner domain controller>)*<br /><br />USN en el momento de la restauración:<br /><br />*<number>*<br /><br />Como resultado, el vector de actualización del servicio de directorio de destino se ha configurado de la siguiente forma.<br /><br />GUID de la base de datos anterior:<br /><br />*<GUID>*<br /><br />USN del objeto anterior:<br /><br />*<number>*<br /><br />USN de la propiedad anterior:<br /><br />*<number>*<br /><br />GUID de la nueva base de datos:<br /><br />*<GUID>*<br /><br />USN del nuevo objeto:<br /><br />*<number>*<br /><br />USN de nueva propiedad:<br /><br />*<number>* |

##### <a name="system-event-log"></a>Registro de eventos de Sistema  
El registro de eventos de Sistema anota la hora del equipo en que se conecta de nuevo una máquina virtual y se sincroniza con la hora del host. El grupo RID se invalida y se reinician los servicios DFSR o FRS.  


|              |                         |                                                                                                                                                                                                                                                                                                                                                                                                                         |
|--------------|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ID. de evento** |       **Origen**        |                                                                                                                                                                                                       **Mensaje**                                                                                                                                                                                                       |
|    **1**     |     Kernel-General      |                                                                                                                                   La hora del sistema ha cambiado a *?<now>* en *< > de fecha y hora*de la instantánea.<br /><br />Cambiar motivo: una aplicación o un componente del sistema cambió la hora.                                                                                                                                   |
|  **16654**   | Directory-Services-SAM  | Se invalidó un grupo de identificadores de cuenta (RID). Esto puede suceder en los siguientes casos previstos:<br /><br />1. un controlador de dominio se restaura a partir de una copia de seguridad.<br /><br />2. un controlador de dominio que se ejecuta en una máquina virtual se restaura desde una instantánea.<br /><br />3. un administrador invalidó el grupo manualmente.<br /><br />Consulte <https://go.microsoft.com/fwlink/?LinkId=226247> para obtener más información. |
|   **7036**   | administrador de control de servicios |                                                                                                                                                                                 El servicio Replicación DFS entró en estado de detención.                                                                                                                                                                                  |
|   **7036**   | administrador de control de servicios |                                                                                                                                                                                 El servicio Replicación DFS entró en estado de ejecución.                                                                                                                                                                                  |

##### <a name="application-event-log"></a>Registro de eventos de Aplicación  
El registro de eventos de Aplicación anota la detención e inicio de la base de datos de DFSR.  


|              |            |                                                                                                                                                                                                                                                                                                                                                                                                  |
|--------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ID. de evento** | **Origen** |                                                                                                                                                                                           **Mensaje**                                                                                                                                                                                            |
|   **103**    |   ESENT    |        DFSR (1360) \\\\.\c: \System Volume Information\DFSR\database<em>_<GUID></em>\dfsr.dB: el motor de base de datos detuvo la instancia (0).<br /><br />Cierre con errores: 0<br /><br />Secuencia temporal interna: [1] 0,000, [2] 0,000, [3] 0,000, [4] 0,000, [5] 0,141, [6] 0,000, [7] 0,000, [8] 0,000, [9] 0,000, [10] 0,000, [11] 0,016, [12] 0,000, [13] 0.000, [14] 0.000, [15] 0.000.         |
|   **102**    |   ESENT    |                                                                                                                    DFSR (532) \\\\.\c: \System Volume Information\DFSR\database<em>_<GUID></em>\dfsr.dB: el motor de base de datos (6.02.8189.0000) está iniciando una nueva instancia (0).                                                                                                                    |
|   **105**    |   ESENT    |                                      DFSR (532) \\\\.\c: \System Volume Information\DFSR\database<em>_<GUID></em>\dfsr.dB: el motor de base de datos inició una nueva instancia (0). (Tiempo=0 segundos)<br /><br />Secuencia temporal interna: [1] 0,000, [2] 0,000, [3] 0,000, [4] 0,000, [5] 0,000, [6] 0,000, [7] 0,000, [8] 0,000, [9] 0,031, [10] 0,000, [11] 0,000.                                      |
|              |            | DFSR (532) \\\\.\c: \System Volume Information\DFSR\database<em> _<GUID></em>\dfsr.dB: el motor de base de datos creó una nueva base de datos (1, \\\\.\c: \System Volume Information\DFSR\database<em>_ <GUID></em>\dfsr.dB). (Tiempo=0 segundos)<br /><br />Secuencia temporal interna: [1] 0,000, [2] 0,000, [3] 0,016, [4] 0,062, [5] 0,000, [6] 0,016, [7] 0,000, [8] 0,000, [9] 0,015, [10] 0,000, [11] 0,000. |

##### <a name="dfs-replication-event-log"></a>Registro de eventos de Replicación DFS  
El servicio DFSR se detiene y se elimina la base de datos que hospeda SYSVOL, lo que obliga a una sincronización de entrada no autoritativa.  


|              |            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|--------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ID. de evento** | **Origen** |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           **Mensaje**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|   **1006**   |    DFSR    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            El servicio de replicación DFS se está deteniendo.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|   **1008**   |    DFSR    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            El servicio de replicación DFS se ha detenido.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|   **1002**   |    DFSR    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            El servicio de replicación DFS se está iniciando.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|   **1004**   |    DFSR    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            El servicio de replicación DFS se ha iniciado.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|   **1314**   |    DFSR    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  El servicio de replicación DFS configuró correctamente los archivos de registro de depuración.<br /><br />Más información:<br /><br />Ruta de acceso del archivo de registro de depuración: C:\Windows\debug                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|   **6102**   |    DFSR    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            El servicio de replicación DFS registró correctamente el proveedor WMI.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|   **1206**   |    DFSR    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              El servicio de Replicación DFS se ha conectado correctamente *<domain controller FQDN>* de controlador de dominio para obtener acceso a la información de configuración.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|   **1210**   |    DFSR    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    El servicio de replicación DFS estableció una escucha RPC para las solicitudes de replicación entrantes.<br /><br />Más información:<br /><br />Puerto: 0                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|   **4614**   |    DFSR    | El servicio de replicación DFS inicializó SYSVOL en la ruta local C:\Windows\SYSVOL\domain y está esperando realizar la replicación inicial. La carpeta replicada permanecerá en el estado inicial de sincronización hasta que se replique con el asociado. Si el servidor se estaba ascendiendo a un controlador de dominio, el controlador de dominio no se anunciará ni funcionará como tal hasta que este problema se resuelva. Esto puede suceder si el asociado especificado también está en el estado inicial de sincronización, o si se detectaron infracciones de uso compartido en este servidor o en el asociado de sincronización. Si este evento se presentó durante la migración de SYSVOL del servicio Replicación de archivos (FRS) a la Replicación DFS, los cambios no se replicarán a menos que este problema se resuelva. Esto puede provocar que la carpeta SYSVOL en este servidor deje de estar sincronizada con los demás controladores de dominio.<br /><br />Más información:<br /><br />Nombre de la carpeta replicada: SYSVOL share<br /><br />IDENTIFICADOR de la carpeta replicada: *<GUID>*<br /><br />Nombre del grupo de replicación: volumen del sistema de dominio<br /><br />IDENTIFICADOR de grupo de replicación: *<GUID>*<br /><br />IDENTIFICADOR de miembro: *<GUID>*<br /><br />Solo lectura: 0 |
|   **4604**   |    DFSR    |                                                                                                                                                                                                                                                                   El servicio Replicación DFS inicializó correctamente la carpeta replicada SYSVOL en la ruta local C:\Windows\SYSVOL\domain. Este miembro completó la sincronización inicial de SYSVOL con el asociado dc1.corp.contoso.com.  Para comprobar la presencia de una carpeta SYSVOL, abre una ventana del símbolo del sistema y escribe "net share".<br /><br />Más información:<br /><br />Nombre de la carpeta replicada: SYSVOL share<br /><br />IDENTIFICADOR de la carpeta replicada: *<GUID>*<br /><br />Nombre del grupo de replicación: volumen del sistema de dominio<br /><br />IDENTIFICADOR de grupo de replicación: *<GUID>*<br /><br />IDENTIFICADOR de miembro: *<GUID>*<br /><br />Asociado de sincronización: *<partner domain controller FQDN>*                                                                                                                                                                                                                                                                    |

#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-frs"></a>Restaurar un controlador de dominio que replica SYSVOL mediante FSR  
En este caso, se usa el registro de eventos de Replicación de archivos en lugar del registro de eventos de DFSR. El registro de eventos de Aplicación también escribe diferentes eventos relativos a FRS. Por lo demás, los mensajes de los registros de eventos de Servicios de directorio y de Sistema por lo general son los mismos, y en el mismo orden que se han descrito.  

##### <a name="file-replication-service-event-log"></a>Registro de eventos del Servicio de replicación de archivos  
El servicio FRS se ha detenido y reiniciado con un valor D2 BURFLAGS para sincronizar SYSVOL de forma no autoritativa.  


|              |            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|--------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ID. de evento** | **Origen** |                                                                                                                                                                                                                                                                                                                                                                                           **Mensaje**                                                                                                                                                                                                                                                                                                                                                                                            |
|  **13502**   |   NTFRS    |                                                                                                                                                                                                                                                                                                                                                                            El Servicio de replicación de archivos se está deteniendo.                                                                                                                                                                                                                                                                                                                                                                             |
|  **13503**   |   NTFRS    |                                                                                                                                                                                                                                                                                                                                                                            El Servicio de replicación de archivos se ha detenido.                                                                                                                                                                                                                                                                                                                                                                             |
|  **13501**   |   NTFRS    |                                                                                                                                                                                                                                                                                                                                                                             El Servicio de replicación de archivos se está iniciando.                                                                                                                                                                                                                                                                                                                                                                             |
|  **13512**   |   NTFRS    |                                                                                                                                                                                                                                                            El Servicio de replicación de archivos ha detectado una caché de escritura en disco habilitada en la unidad que contiene el directorio c:\windows\ntfrs\jet en el equipo DC4. Es posible que el Servicio de replicación de archivos no se recupere cuando se interrumpa la conexión a la unidad o se pierdan actualizaciones críticas.                                                                                                                                                                                                                                                            |
|  **13565**   |   NTFRS    |                                                  El Servicio de réplica de archivos está inicializando el volumen del sistema con datos de otro controlador de dominio. El equipo DC4 no se puede convertir en un controlador de dominio hasta que este proceso se haya completado. El volumen del sistema será entonces compartido como SYSVOL.<br /><br />Para comprobar el recurso compartido SYSVOL, en el símbolo del sistema, escribe:<br /><br />net share<br /><br />Cuando el Servicio de réplica de archivos complete el proceso de inicialización, aparecerá el recurso compartido SYSVOL.<br /><br />La inicialización del volumen de sistema puede tardar un poco. El tiempo depende de la cantidad de datos en el volumen de sistema, la disponibilidad de otros controladores de dominio, y el intervalo de replicación entre controladores de dominio.                                                  |
|  **13520**   |   NTFRS    | El servicio de replicación de archivos desplazó los archivos preexistentes en *<path>* a *<path>* \ NtFrs_PreExisting___See_EventLog.<br /><br />El servicio de replicación de archivos puede eliminar los archivos de *<path>* \ NtFrs_PreExisting___See_EventLog en cualquier momento. Los archivos se pueden guardar de la eliminación copiándolos fuera de *<path>* \ NtFrs_PreExisting___See_EventLog. La copia de los archivos en *<path>* puede provocar conflictos de nombres si los archivos ya existen en otro asociado de replicación.<br /><br />En algunos casos, el servicio de replicación de archivos puede copiar un archivo de *<path>* \ NtFrs_PreExisting___See_EventLog en *<path>* en lugar de replicar el archivo desde otro asociado de replicación.<br /><br />Se puede recuperar el espacio en cualquier momento eliminando los archivos de *<path>* \ NtFrs_PreExisting___See_EventLog. |
|  **13553**   |   NTFRS    |                                                                                                                                            El servicio de replicación de archivos agregó correctamente este equipo al conjunto de replicas siguientes:<br /><br />"DOMAIN SYSTEM VOLUME (SYSVOL SHARE)"<br /><br />La información relacionada con este evento se muestra a continuación:<br /><br />El nombre DNS del equipo es " *<domain controller FQDN>* "<br /><br />El nombre del miembro del conjunto de réplicas es " *<domain controller name>* "<br /><br />La ruta de la raíz del conjunto de réplicas es " *<path>* "<br /><br />La ruta del directorio de ensayo de réplicas es " *<path>* "<br /><br />La ruta del directorio de trabajo de réplicas es " *<path>* "                                                                                                                                             |
|  **13554**   |   NTFRS    |                                                                                                                                                                                                                     El servicio de replicación de archivos agregó correctamente las conexiones mostradas a continuación en el conjunto de la réplica:<br /><br />"DOMAIN SYSTEM VOLUME (SYSVOL SHARE)"<br /><br />Entrante desde " *<partner domain controller FQDN>* "<br /><br />Saliente a " *<partner domain controller FQDN>* "<br /><br />Puede aparecer más información en mensajes posteriores del registro de eventos.                                                                                                                                                                                                                     |
|  **13516**   |   NTFRS    |                                                                                                                                                                                                                                  El servicio de replicación de archivos ya no impide que el equipo DC4 sea un controlador de dominio. Se ha inicializado correctamente el volumen de sistema y se ha notificado al servicio de Net Logon de que dicho volumen está preparado para compartirse como SYSVOL.<br /><br />Escribe "net share" para comprobar el recurso compartido SYSVOL.                                                                                                                                                                                                                                  |

##### <a name="application-event-log"></a>Registro de eventos de Aplicación  
La base de datos de FRS se detiene y se inicia, y se limpia debido a la operación D2 BURFLAGS.  

||||  
|-|-|-|  
|**ID. de evento**|**Origen**|**Mensaje**|  
|**327**|ESENT|ntfrs (1424) El motor de la base de datos separó una base de datos (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Tiempo=0 segundos)<br /><br />Secuencia temporal interna: [1] 0,000, [2] 0,015, [3] 0,000, [4] 0,000, [5] 0,000, [6] 0,516, [7] 0,000, [8] 0,000, [9] 0,000, [10] 0,000, [11] 0,063, [12] 0,000.<br /><br />Caché reactivada: 0|  
|**103**|ESENT|ntfrs (1424) El motor de base de datos detuvo la instancia (0).<br /><br />Cierre con errores: 0<br /><br />Secuencia temporal interna: [1] 0,000, [2] 0,000, [3] 0,000, [4] 0,000, [5] 0,000, [6] 0,000, [7] 0,000, [8] 0,000, [9] 0,031, [10] 0,000, [11] 0,016, [12] 0,000, [13] 0,000, [14] 0,047, [15] 0,000.|  
|**102**|ESENT|ntfrs (3000) El motor de base de datos (6.02.8189.0000) está iniciando una nueva instancia (0).|  
|**105**|ESENT|ntfrs (3000) El motor de base de datos inició una nueva instancia (0). (Tiempo=0 segundos)<br /><br />Secuencia temporal interna: [1] 0,000, [2] 0,000, [3] 0,000, [4] 0,000, [5] 0,000, [6] 0,000, [7] 0,000, [8] 0,000, [9] 0,062, [10] 0,000, [11] 0,141.|  
|**103**|ESENT|ntfrs (3000) El motor de base de datos detuvo la instancia (0).<br /><br />Cierre con errores: 0<br /><br />Secuencia temporal interna: [1] 0,000, [2] 0,000, [3] 0,000, [4] 0,000, [5] 0,000, [6] 0,000, [7] 0,000, [8] 0,000, [9] 0,000, [10] 0,000, [11] 0,000, [12] 0,000, [13] 0,015, [14] 0,000, [15] 0,000.|  
|**102**|ESENT|ntfrs (3000) El motor de base de datos (6.02.8189.0000) está iniciando una nueva instancia (0).|  
|**105**|ESENT|ntfrs (3000) El motor de base de datos inició una nueva instancia (0). (Tiempo=0 segundos)<br /><br />Secuencia temporal interna: [1] 0,000, [2] 0,000, [3] 0,000, [4] 0,000, [5] 0,000, [6] 0,000, [7] 0,000, [8] 0,000, [9] 0,078, [10] 0,000, [11] 0,109.|  
|**325**|ESENT|ntfrs (3000) El motor de base de datos creó una nueva base de datos (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Tiempo=0 segundos)<br /><br />Secuencia temporal interna: [1] 0,000, [2] 0,000, [3] 0,016, [4] 0,016, [5] 0,000, [6] 0,015, [7] 0,000, [8] 0,000, [9] 0,078, [10] 0,016, [11] 0,000.|  
|**103**|ESENT|ntfrs (3000) El motor de base de datos detuvo la instancia (0).<br /><br />Cierre con errores: 0<br /><br />Secuencia temporal interna: [1] 0,000, [2] 0,000, [3] 0,000, [4] 0,000, [5] 0,078, [6] 0,000, [7] 0,000, [8] 0,000, [9] 0,125, [10] 0,016, [11] 0,000, [12] 0,000, [13] 0,000, [14] 0,000, [15] 0,000.|  
|**102**|ESENT|ntfrs (3000) El motor de base de datos (6.02.8189.0000) está iniciando una nueva instancia (0).|  
|**105**|ESENT|ntfrs (3000) El motor de base de datos inició una nueva instancia (0). (Tiempo=0 segundos)<br /><br />Secuencia temporal interna: [1] 0,016, [2] 0,000, [3] 0,000, [4] 0,094, [5] 0,000, [6] 0,000, [7] 0,000, [8] 0,000, [9] 0,032, [10] 0,000, [11] 0,000.|  
|**326**|ESENT|ntfrs (3000) El motor de la base de datos adjuntó una base de datos (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Tiempo=0 segundos)<br /><br />Secuencia temporal interna: [1] 0,000, [2] 0,015, [3] 0,000, [4] 0,000, [5] 0,016, [6] 0,015, [7] 0,000, [8] 0,000, [9] 0,000, [10] 0,000, [11] 0,000, [12] 0,000.<br /><br />Caché guardada: 1|  



