---
ms.assetid: 249ba1be-b0d3-4a77-99af-3699074a2b6e
title: "Virtualizados dominio solución de problemas de controlador"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 63f96e7a3035bc25f7a7a349acb6bf26f62a2a3f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-troubleshooting"></a>Virtualizados dominio solución de problemas de controlador

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema proporciona metodología detallada sobre solución de problemas de la función de controlador de dominio virtualizada.  
  
-   [Solución de problemas virtualizados clonación del controlador de dominio](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCCloning)  
  
-   [Solución de problemas de restauración segura de controlador de dominio virtualizada](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCSafeRestore)  
  
## <a name="BKMK_Intro"></a>Introducción  
La forma más importante para mejorar tus conocimientos sobre la solución de problemas es un laboratorio de pruebas de compilación y rigurosamente examinar normal, trabajar escenarios. Si se producen errores, son más evidentes y fáciles de comprender, ya que, a continuación, tienes una sólida base de cómo funciona la promoción de controlador de dominio. Esto también te permite crear tus habilidades de análisis de análisis y la red. Esto se aplica a todas las tecnologías de sistemas distribuidos, implementación del controlador de dominio no solo virtualizada.  
  
Los elementos fundamentales para la solución avanzada de problemas de configuración del controlador de dominio son:  
  
1.  Análisis lineal combinan con atención a los detalles.  
  
2.  Descripción de los análisis de captura de red  
  
3.  Descripción de los registros integrados  
  
El primer y segundo están fuera del ámbito de este tema, pero el tercero puede explican en detalle. Solución de problemas del controlador de dominio virtualizada requiere un método lógico y lineal. La clave es abordar el problema con los datos proporcionados y solo recurrir a herramientas complejas y análisis si ha agotado el registro y la salida proporcionada.  
  
## <a name="BKMK_TshootVDCCloning"></a>Solución de problemas virtualizados clonación del controlador de dominio  
Esto secciones carcasas:  
  
-   [Herramientas de solución de problemas](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_Tools)  
  
-   [Opciones de registro](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_LoggingOptions)  
  
-   [Metodología general para solucionar problemas de dominio clonación del controlador](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)  
  
-   [Server Core y el registro de eventos](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_ServerCoreEvents)  
  
-   [Solucionar problemas específicos](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_SpecificProblems)  
  
La estrategia de solución de problemas de clonación del controlador de dominio virtualizada tiene este formato general:  
  
![solución de problemas de dc virtual](media/Virtualized-Domain-Controller-Troubleshooting/ADDS_VDC_TroublehsootingFlowchart.png)  
  
### <a name="BKMK_Tools"></a>Herramientas de solución de problemas  
  
#### <a name="BKMK_LoggingOptions"></a>Opciones de registro  
Los registros integrados son la herramienta más importante para solucionar problemas con la clonación del controlador de dominio. Todos estos registros se habilitado y configurados para el máximo nivel de detalle, de manera predeterminada.  
  
|||  
|-|-|  
|**Operación**|**Registro**|  
|**Clonación**|-Evento viewer\Windows logs\System<br />-Evento viewer\Applications y servicios logs\Directory servicio<br />-%systemroot%\debug\dcpromo.log|  
|**Promoción**|-%systemroot%\debug\dcpromo.log<br />-Evento viewer\Applications y servicios logs\Directory servicio<br />-Evento viewer\Windows logs\System<br />-Evento viewer\Applications y servicios logs\File servicio de replicación<br />-Evento viewer\Applications y servicios logs\DFS replicación|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Herramientas y los comandos para solucionar problemas de configuración del controlador de dominio  
Para solucionar problemas que no se explica por los registros, usa las siguientes herramientas como punto de partida:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   3.4 del Monitor de red  
  
### <a name="BKMK_GeneralMethodology"></a>Metodología general para solucionar problemas de dominio clonación del controlador  
  
1.  ¿Se está iniciando la máquina virtual en el modo de reparación de DS (DSRM)? Esto indica que es necesario solucionar el problema. Para iniciar sesión en DSRM, usar **. \Administrator** cuenta y especifica la contraseña DSRM.  
  
    1.  Examina la Dcpromo.log.  
  
        1.  ¿Pasos clonación iniciales se realizan correctamente, pero se producirá un error en la promoción del controlador de dominio?  
  
        2.  ¿Errores indican problemas con el controlador de dominio local o con el entorno de AD DS, como los errores se devuelven desde el emulador PDC?  
  
    2.  Examina los registros de eventos del sistema y los servicios de directorio y los dccloneconfig.xml y CustomDCCloneAllowList.xml  
  
        1.  ¿Una aplicación incompatible necesita estar en el CustomDCCloneAllowList.xml lista de permitidos?  
  
        2.  ¿Es el nombre de equipo o dirección IP duplicada o no válido en el dccloneconfig.xml?  
  
        3.  ¿No es válido en el dccloneconfig.xml el sitio de Active Directory?  
  
        4.  ¿La dirección IP no se establece en el dccloningconfig.xml y no hay ningún servidor DHCP?  
  
        5.  ¿Es el emulador PDC en línea y disponible a través del protocolo RPC?  
  
        6.  ¿Es el controlador de dominio miembro del grupo de controladores de dominio clonación? ¿Es el permiso **permitir que un controlador de dominio crear una copia de sí mismo** establecer en la raíz del dominio para ese grupo?  
  
        7.  ¿Contiene el archivo de Dccloneconfig.xml errores de sintaxis que impiden que el análisis correcto?  
  
        8.  ¿Se admite el hipervisor?  
  
        9. ¿Hicimos errores de promoción de controlador de dominio después clonación inició correctamente?  
  
        10. ¿Fue el número máximo de nombres de controlador de dominio genera automáticamente (9999) superado?  
  
        11. ¿Se duplica la dirección MAC?  
  
2.  ¿Es el nombre de host de la copia el mismo que el origen de controlador de dominio?  
  
    1.  ¿Hay un archivo Dccloneconfig.xml en una de las ubicaciones permitidas?  
  
3.  Es la máquina virtual arrancar en modo normal y clonación completado, pero el controlador de dominio no funciona correctamente?  
  
    1.  Primero comprueba si se cambia el nombre de host en la copia. Si el nombre de host es diferente, al menos parcialmente ha completado la clonación.  
  
    2.  ¿El controlador de dominio tiene una dirección IP duplicada del controlador de dominio de origen de la dccloneconfig.xml, pero el controlador de dominio de origen estaba sin conexión durante la clonación?  
  
    3.  Si el controlador de dominio se anuncia, considera el problema como cualquier problema de promoción posterior a la normal que tendría sin clonación.  
  
    4.  Si no se anuncia el controlador de dominio, examina los registros de eventos de servicio de directorio, el sistema, la aplicación, el archivo replicación y replicación DFS errores posteriores a la promoción.  
  
#### <a name="disabling-dsrm-boot"></a>Deshabilitar el arranque DSRM  
Una vez que se arranca en DSRM debido a cualquier error, diagnosticar la causa del error y si dcpromo.log no indica que no se puede reintentar clonación, solucionar la causa del error y restablecer el indicador DSRM. Un error clone no vuelve al modo normal por sí solo en el siguiente reinicio; Debes quitar la marca de arranque del modo de restauración de DS con el fin de probar clonación de nuevo. Todos estos pasos requieren ejecutar como administrador con privilegios elevados.  
  
##### <a name="removing-dsrm-with-msconfigexe"></a>Quitar DSRM con Msconfig.exe  
Para activar el arranque DSRM desactivar con una interfaz gráfica de usuario, usa la herramienta de configuración del sistema:  
  
1.  Ejecutar msconfig.exe  
  
2.  En la **arranque** ficha **opciones de arranque**, anule la selección de **arranque seguro** (ya está seleccionada con la opción **reparación de Active Directory** habilitado)  
  
3.  Haz clic en Aceptar y reiniciar cuando se te solicite  
  
##### <a name="removing-dsrm-with-bcdeditexe"></a>Quitar DSRM con Bcdedit.exe  
Para desactivar el arranque DSRM desde la línea de comandos, usa el Editor de almacén de datos de configuración de arranque:  
  
1.  Abre un CMD símbolo del sistema y de ejecución:  
  
    ```  
    Bcdedit.exe /deletevalue safeboot  
    ```  
  
2.  Reinicie el equipo con:  
  
    ```  
    Shutdown.exe /t /0 /r  
    ```  
  
> [!NOTE]  
> Bcdedit.exe también funciona en una consola de Windows PowerShell. Los comandos son:  
>   
> Bcdedit.exe/DeleteValue safeboot  
>   
> Reiniciar el equipo  
  
### <a name="BKMK_ServerCoreEvents"></a>Server Core y el registro de eventos  
Los registros de eventos contienen gran parte de la información útil acerca de las operaciones de clonación de controlador de dominio virtualizada. De manera predeterminada, la instalación en un equipo de Windows Server 2012 es una instalación Server Core, lo que significa que no hay ninguna interfaz gráfica y por lo tanto, no hay manera de ejecutar el complemento Visor de eventos local.  
  
Para revisar los registros de eventos en un servidor que ejecuta una instalación Server Core:  
  
-   Ejecutar la herramienta Wevtutil.exe localmente  
  
-   Ejecutar el cmdlet de PowerShell Get WinEvent localmente  
  
-   Si has habilitado las reglas de Firewall de Windows avanzada para los grupos de "Administración remota de registro de eventos" (o equivalentes puertos) permitir la comunicación entrante, puedes administrar el registro de eventos con remotamente Eventvwr.exe, wevtutil.exe o Get Winevent. Esto puede hacerse en instalación Server Core con NETSH.exe, directiva de grupo o el cmdlet Set-NetFirewallRule nuevo en Windows PowerShell 3.0.  
  
> [!WARNING]  
> No intentes volver a agregar el shell de gráficos al equipo mientras está en DSRM. Mantenimiento de pila (CB) de Windows no funciona correctamente en modo seguro o DSRM. Intenta agregar características o las funciones en DSRM no se completa y deja el equipo en un estado inestable hasta que se arranca normalmente. Dado que un clone de controlador de dominio virtualizada en DSRM no puede iniciarse normalmente y no debe iniciarse normalmente en la mayoría de los casos, es imposible agregar el shell gráfico de forma segura. Si lo haces, no es compatible y pueden dejar con un servidor inutilizable.  
  
### <a name="BKMK_SpecificProblems"></a>Solucionar problemas específicos  
  
#### <a name="events"></a>Eventos  
Todos los eventos de clonación de controlador de dominio virtualizada se escriben en el registro de eventos de servicios de directorio del controlador de dominio clone máquina virtual. Los registros de eventos de la aplicación, el servicio de replicación de archivos y la replicación DFS también puede contener información útil de solución de problemas para clonar erróneos. Errores durante la llamada RPC para el emulador PDC pueden estar disponibles en el registro de eventos en el emulador PDC.  
  
A continuación, indicamos los eventos de clonación específicos de Windows Server 2012 en el registro de eventos de servicios de directorio, con las notas y las soluciones sugeridas errores.  
  
##### <a name="directory-services-event-log"></a>Registro de eventos de servicios de directorio  
  
|||  
|-|-|  
|**Identificador de evento**|**2160**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|Local * <COMPUTERNAME> * ha encontrado un controlador de dominio virtual clonación el archivo de configuración.<br /><br />El controlador de dominio virtual clonación el archivo de configuración se encuentra en: %1<br /><br />La existencia del archivo de configuración de clonación de controlador de dominio virtual indica que el controlador de dominio virtual local es un clone otro virtual del controlador de dominio. La * <COMPUTERNAME> * empezarán a clonar sí.|  
|**Notas y resolución**|Este es un evento de éxito y solo un problema si inesperado. Examinar el directorio de trabajo de DSA, % systemroot%\ntds y raíz de los discos extraíbles o locales para el archivo dcclconeconfig.xml.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2161**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|Local * <COMPUTERNAME> * no se ha encontrado el controlador de dominio virtual clonación el archivo de configuración. El equipo local no es un controlador de dominio clonado.|  
|**Notas y resolución**|Este es un evento de éxito y solo un problema si inesperado. Examinar el directorio de trabajo de DSA, % systemroot%\ntds y raíz de los discos extraíbles o locales para el archivo dcclconeconfig.xml.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2162**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|No se pudo clonación del controlador de dominio virtual.<br /><br />Ponte en contacto eventos registrados en los registros de eventos de sistema y %systemroot%\debug\dcpromo.log para obtener más información sobre los errores que corresponden al controlador de dominio virtual clonación intento.<br /><br />Código de error: %1|  
|**Notas y resolución**|Siga las instrucciones de mensaje, este error es un general que engloba todos.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2163**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|Se inició el servicio de DsRoleSvc clonar el controlador de dominio virtual local.|  
|**Notas y resolución**|Este es un evento de éxito y solo un problema si inesperado. Examinar el directorio de trabajo de DSA, % systemroot%\ntds y raíz de los discos extraíbles o locales para el archivo dcclconeconfig.xml.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2164**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|*<COMPUTERNAME>*no se pudo iniciar el servicio de DsRoleSvc para clonar el controlador de dominio virtual local.|  
|**Notas y resolución**|Examinar la configuración del servicio para el servicio de rol servidor de DS (DsRoleSvc) y asegúrate de que su tipo de inicio se establece de forma manual. Valida que no hay ningún programa de terceros impide el inicio de este servicio.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2165**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|*<COMPUTERNAME>*Error al iniciar un subproceso durante la clonación del controlador de dominio virtual local.<br /><br />Código de error: % 1<br /><br />Mensaje de error: % 2<br /><br />Nombre: % 3 de subproceso|  
|**Notas y resolución**|Ponte en contacto con soporte técnico de Microsoft|  
  
|||  
|-|-|  
|**Identificador de evento**|**2166**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|*<COMPUTERNAME>*debe iniciar reiniciar en DSRM servicio RPCSS. Esperando RPCSS inicializar en un estado de ejecución que no se pudo realizar.<br /><br />Código de error: % 1|  
|**Notas y resolución**|Examina el registro de eventos del sistema y la configuración del servicio para el servicio de servidor RPC (Rpcss)|  
  
|||  
|-|-|  
|**Identificador de evento**|**2167**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|*<COMPUTERNAME>*no se pudo inicializar el conocimiento del controlador de dominio virtual. Consulta la entrada de registro de eventos anterior para obtener más información.<br /><br />Datos adicionales<br /><br />Código de error: % 1|  
|**Notas y resolución**|Siga las instrucciones de mensaje, este error es un general que engloba todos.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2168**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|Microsoft-Windows-ActiveDirectory_DomainService<br /><br />El controlador de dominio se ejecuta en un hipervisor compatible. Se detecta el Id. de generación de máquina virtual.<br /><br />Valor actual del Id. de generación de máquina virtual: %1|  
|**Notas y resolución**|Este es un evento de éxito y solo un problema si inesperado.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2169**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|No hay ningún identificador de generación de VM detectado. El controlador de dominio se hospeda en una máquina física, una versión de nivel inferior de Hyper-V o un hipervisor que no es compatible con el identificador de generación de máquina virtual.<br /><br />Datos adicionales<br /><br />Código de error devuelto tras la comprobación de generación de VM Id.:%1|  
|**Notas y resolución**|Esto es un evento de éxito si no se pretende clonar. De lo contrario, examina el registro de eventos del sistema y revisar la documentación de soporte técnico de hipervisor.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2170**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Advertencia|  
|**Mensaje**|Se ha detectado un cambio de identificador de generación.<br /><br />Id. de generación que se almacenan en caché en DS (valor antiguo): %1<br /><br />Identificador de generación actualmente en la máquina virtual (nuevo valor): %2<br /><br />Después de la aplicación de una instantánea de la máquina virtual, después de una operación de importación de máquina virtual o una operación de migración en vivo, se produce el cambio de identificador de generación. *<COMPUTERNAME>*se crea un nuevo identificador de invocación para recuperar el controlador de dominio. Controladores de dominio virtualizados no deben restaurarse mediante instantáneas de máquina virtual. El método admitido para restaurar o reversión el contenido de una base de datos de los servicios de dominio de Active Directory es restaurar una copia de seguridad realizada con una aplicación de copia de seguridad compatible con los servicios de dominio de Active Directory.|  
|**Notas y resolución**|Esto es un evento de éxito si quiere clonar. De lo contrario, se examina el registro de eventos del sistema.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2171**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|No se detectó ningún cambio de identificador de generación.<br /><br />Id. de generación que se almacenan en caché en DS (valor antiguo): %1<br /><br />Identificador de generación actualmente en la máquina virtual (nuevo valor): %2|  
|**Notas y resolución**|Este es un evento de éxito si no se pretende clonar y debería verse en cada reinicio de un controlador de dominio virtualizada. De lo contrario, se examina el registro de eventos del sistema.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2172**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|Leer el atributo msDS GenerationId del controlador de dominio objeto de equipo.<br /><br />msDS GenerationId valor de atributo: % 1|  
|**Notas y resolución**|Esto es un evento de éxito si quiere clonar. De lo contrario, se examina el registro de eventos del sistema.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2173**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|Error al leer el atributo msDS GenerationId del controlador de dominio objeto de equipo. Esto puede deberse a errores de la transacción de base de datos o el identificador de generación no existe en la base de datos local. No existe el GenerationId msDS durante el primer reinicio después dcpromo o el controlador de dominio no es un controlador de dominio virtual.<br /><br />Datos adicionales<br /><br />Código de error: % 1|  
|**Notas y resolución**|Este es un evento de éxito si quiere clonar y es el primer reinicio VM una clonación completada. También puede omitirse en controladores de dominio no virtual. De lo contrario, se examina el registro de eventos del sistema.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2174**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|El controlador de dominio no es un clone de controlador de dominio virtual ni una instantánea del controlador de dominio virtual restaurados.|  
|**Notas y resolución**|Esto es un evento de éxito si no se pretende clonar. De lo contrario, se examina el registro de eventos del sistema.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2175**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|Archivo de configuración de clone de controlador de dominio virtual se presenta en una plataforma no compatible.|  
|**Notas y resolución**|Esto ocurre cuando se encuentra un dccloneconfig.xml y un identificador de generación de máquina virtual no se pudo encontrar, por ejemplo, cuando se encuentra un archivo dccloneconfig.xml en un equipo físico o en un hipervisor que no admite la generación de VM-Id.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2176**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|Cambiar el nombre de archivo de configuración de clone de controlador de dominio virtual.<br /><br />Datos adicionales<br /><br />Nombre de archivo antiguo: % 1<br /><br />Nombre de archivo nuevo: % 2|  
|**Notas y resolución**|Cambiar el nombre que habrá al arrancar un máquina virtual de origen una copia de seguridad, no ha cambiado el Id. de generación de máquina virtual. Esto impide que intente clonar el controlador de dominio de origen.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2177**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|No se pudo cambiar el nombre de archivo de configuración de clone de controlador de dominio virtual.<br /><br />Datos adicionales<br /><br />Nombre del archivo: % 1<br /><br />Código de error: % 2 %3|  
|**Notas y resolución**|Cambia el nombre intento esperada al arrancar un máquina virtual de origen una copia de seguridad, debido a que no ha cambiado el Id. de generación de máquina virtual. Esto impide que intente clonar el controlador de dominio de origen. El nombre del archivo e investigar instalado los productos de terceros que pueden impedir el cambio de nombre de archivo manualmente.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2178**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|Detecta el archivo de configuración de clone de controlador de dominio virtual, pero no ha cambiado el Id. de generación de máquina virtual. El controlador de dominio local es el origen de clone DC. Cambia el nombre del archivo de configuración clone.|  
|**Notas y resolución**|Espera al arrancar un máquina virtual de origen una copia de seguridad, ya que no ha cambiado el Id. de generación de máquina virtual. Esto impide que intente clonar el controlador de dominio de origen.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2179**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|Se ha establecido el atributo msDS GenerationId del controlador de dominio objeto de equipo en el parámetro siguiente:<br /><br />GenerationID atributo: % 1|  
|**Notas y resolución**|Este es un evento de éxito y solo un problema si inesperado.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2180**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Advertencia|  
|**Mensaje**|No se pudo establecer el atributo msDS GenerationId del controlador de dominio objeto de equipo.<br /><br />Datos adicionales<br /><br />Código de error: % 1|  
|**Notas y resolución**|Examina el registro de eventos del sistema y Dcpromo.log. Búsqueda en Microsoft TechNet, Knowledgebase MS y MS blogs para determinar su significado habitual y, a continuación, solucionar el error específico que se basa en los resultados.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2182**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|Evento interno: el servicio de directorio ha sido formulado clonar un DSA remoto:|  
|**Notas y resolución**|Este es un evento de éxito y solo un problema si inesperado.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2183**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|Evento interno: * <COMPUTERNAME> * completar la solicitud para clonar el agente de sistema de directorio remoto.<br /><br />Nombre de DC original: % 3<br /><br />Solicitar clone nombre del controlador de dominio: % 4<br /><br />Solicitar clone DC sitio: % 5<br /><br />Datos adicionales<br /><br />Valor de error: % 1 %2|  
|**Notas y resolución**|Este es un evento de éxito y solo un problema si inesperado.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2184**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|*<COMPUTERNAME>*Error al crear una cuenta de controlador de dominio para el controlador de dominio clonado.<br /><br />Nombre de DC original: % 1<br /><br />Número permitido de clonados DC:% 2<br /><br />El límite del número de cuentas de controlador de dominio que se pueden generar duplicando * <COMPUTERNAME> *superó.|  
|**Notas y resolución**|Nombre de un controlador de dominio de origen solo puede generar solo automáticamente 9999 veces si los controladores de dominio no se degradan, en función de la convención de nomenclatura. Usa el <computername> elemento en el XML para generar un nuevo nombre único o clone desde un controlador de dominio con un nombre distinto.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2191**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|*<COMPUTERNAME>*establece el valor del registro siguiente para desactivar las actualizaciones DNS.<br /><br />Clave del registro: % 1<br /><br />Valor del registro: %2<br /><br />Información del valor del registro: %3<br /><br />Durante el proceso de clonación, el equipo local puede tener el mismo nombre de equipo que el equipo de origen clone durante un rato. DNS A y registro de registro AAAA están deshabilitadas durante este período para que los clientes no pueden enviar solicitudes a la máquina local está sometiendo a clonación. El proceso de clonación te permitirá actualizaciones DNS nuevamente después de completa la clonación.|  
|**Notas y resolución**|Este es un evento de éxito y solo un problema si inesperado.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2192**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|*<COMPUTERNAME>*no se pudo establecer el siguiente valor del registro para deshabilitar las actualizaciones DNS.<br /><br />Clave del registro: % 1<br /><br />Valor del registro: %2<br /><br />Información del valor del registro: %3<br /><br />Código de error: % 4<br /><br />Mensaje de error: % 5<br /><br />Durante el proceso de clonación, el equipo local puede tener el mismo nombre de equipo que el equipo de origen clone durante un rato. DNS A y registro de registro AAAA están deshabilitadas durante este período para que los clientes no pueden enviar solicitudes a la máquina local está sometiendo a clonación.|  
|**Notas y resolución**|Examina los registros de eventos de aplicación y del sistema. Investigar la aplicación de terceros que puede estar bloqueando las actualizaciones del registro.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2193**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|*<COMPUTERNAME>*establecer el siguiente valor del registro para habilitar actualizaciones DNS.<br /><br />Clave del registro: % 1<br /><br />Valor del registro: %2<br /><br />Información del valor del registro: %3<br /><br />Durante el proceso de clonación, el equipo local puede tener el mismo nombre de equipo que el equipo de origen clone durante un rato. DNS A y registro de registro AAAA están deshabilitadas durante este período para que los clientes no pueden enviar solicitudes a la máquina local está sometiendo a clonación.|  
|**Notas y resolución**|Este es un evento de éxito y solo un problema si inesperado.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2194**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|*<COMPUTERNAME>*no se pudo establecer el siguiente valor del registro para habilitar actualizaciones DNS.<br /><br />Clave del registro: % 1<br /><br />Valor del registro: %2<br /><br />Información del valor del registro: %3<br /><br />Código de error: % 4<br /><br />Mensaje de error: % 5<br /><br />Durante el proceso de clonación, el equipo local puede tener el mismo nombre de equipo que el equipo de origen clone durante un rato. DNS A y registro de registro AAAA están deshabilitadas durante este período para que los clientes no pueden enviar solicitudes a la máquina local está sometiendo a clonación.|  
|**Notas y resolución**|Examina los registros de eventos de aplicación y del sistema. Investigar la aplicación de terceros que puede estar bloqueando las actualizaciones del registro.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2195**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|No se pudo establecer arranque DSRM.<br /><br />Código de error: % 1<br /><br />Mensaje de error: % 2<br /><br />Cuando la clonación del controlador de dominio virtual no se pudo o archivo de configuración de clone de controlador de dominio virtual aparece en un hipervisor no compatible, se reiniciará el equipo local en DSRM para solucionar problemas. Error de arranque DSRM de configuración.|  
|**Notas y resolución**|Examina los registros de eventos de aplicación y del sistema. Investigar la aplicación de terceros que puede estar bloqueando las actualizaciones del registro.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2196**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|Error al habilitar el privilegio de apagado.<br /><br />Código de error: % 1<br /><br />Mensaje de error: % 2<br /><br />Cuando la clonación del controlador de dominio virtual no se pudo o archivo de configuración de clone de controlador de dominio virtual aparece en un hipervisor no compatible, se reiniciará el equipo local en DSRM para solucionar problemas. No se pudo habilitar privilegios de apagado.|  
|**Notas y resolución**|Examina los registros de eventos de aplicación y del sistema. Investigar la aplicación de terceros que puede bloquear el uso de privilegios.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2197**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|No se pudo inician el apagado del sistema.<br /><br />Código de error: % 1<br /><br />Mensaje de error: % 2<br /><br />Cuando la clonación del controlador de dominio virtual no se pudo o archivo de configuración de clone de controlador de dominio virtual aparece en un hipervisor no compatible, se reiniciará el equipo local en DSRM para solucionar problemas. No se pudo iniciar apagado del sistema.|  
|**Notas y resolución**|Examina los registros de eventos de aplicación y del sistema. Investigar la aplicación de terceros que puede bloquear el uso de privilegios.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2198**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|*<COMPUTERNAME>*Error al crear o modificar el siguiente objeto DC clonado.<br /><br />Datos adicionales:<br /><br />Objeto:<br /><br />%1<br /><br />Valor de error: %2<br /><br />%3|  
|**Notas y resolución**|Búsqueda en Microsoft TechNet, Knowledgebase MS y MS blogs para determinar su significado habitual y, a continuación, solucionar el error específico que se basa en los resultados.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2199**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|*<COMPUTERNAME>*Error al crear el siguiente objeto DC clonado porque el objeto ya existe.<br /><br />Datos adicionales:<br /><br />Origen de controlador de dominio:<br /><br />%1<br /><br />Objeto:<br /><br />%2|  
|**Notas y resolución**|Validar la dccloneconfig.xml no especificó un controlador de dominio o que se haya usado copias de la dccloneconfig.xml en varios clones sin modificar el nombre. Si la colisión es aún inesperada, determinar qué administrador promueve Ponte en contacto con ellos para hablar de si el controlador de dominio existente debe se degrada, limpiar los metadatos de controlador de dominio existentes, o si la copia debe usar un nombre diferente.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2203**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|Error en el último controlador de dominio virtual clonación. Este es el primer reinicio desde, a continuación, por lo tanto, esta debe ser un volver a prueba de la clonación. Sin embargo, existe ningún archivo de configuración de dominio virtual controlador clone ni se detecta el cambio de identificador de generación de máquina virtual. Arranque en DSRM.<br /><br />Clonación del controlador de dominio virtual último error: %1<br /><br />Existe el archivo de configuración de clone de controlador de dominio virtual: %2<br /><br />Se detecta el cambio de identificador de generación de máquina virtual: %3|  
|**Notas y resolución**|Se esperaba si clonación error anteriormente, dccloneconfig.xml falta o no válido|  
  
|||  
|-|-|  
|Identificador de evento|2210|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Error|  
|Mensaje|<COMPUTERNAME> Error al crear objetos clone controlador de dominio.<br /><br />Datos adicionales:<br /><br />Clonar Id: %6<br /><br />Nombre del controlador de dominio Clone: %1<br /><br />Reintentar bucle: %2<br /><br />El valor de excepción: %3<br /><br />Valor de error: %4<br /><br />DSID: %5|  
|Notas y resolución|Revisar los registros de eventos del sistema y los servicios de directorio y dcpromo.log para obtener más información sobre el motivo del error de clonación.|  
  
|||  
|-|-|  
|Identificador de evento|2211|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> ha creado objetos clone controlador de dominio.<br /><br />Datos adicionales:<br /><br />Clonar Id: %3<br /><br />Nombre del controlador de dominio Clone: %1<br /><br />Reintentar bucle: %2|  
|Notas y resolución|Este es un evento de éxito y solo un problema si inesperado.|  
  
|||  
|-|-|  
|Identificador de evento|2212|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> empezar a crear objetos para el controlador de dominio clone.<br /><br />Datos adicionales:<br /><br />Clonar Id: %1<br /><br />Nombre de Clone: %2<br /><br />Sitio Clone: %3<br /><br />Clonar RODC: %4|  
|Notas y resolución|Este es un evento de éxito y solo un problema si inesperado.|  
  
|||  
|-|-|  
|Identificador de evento|2213|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> crea un nuevo objeto de KrbTgt para clonación del controlador de dominio de solo lectura.<br /><br />Datos adicionales:<br /><br />Clonar Id: %1<br /><br />Nuevo Guid de objeto KrbTgt: %2|  
|Notas y resolución|Este es un evento de éxito y solo un problema si inesperado.|  
  
|||  
|-|-|  
|Identificador de evento|2214|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> se crea un objeto de equipo para el controlador de dominio clone.<br /><br />Datos adicionales:<br /><br />Clonar Id: %1<br /><br />Controlador de dominio original: %2<br /><br />Controlador de dominio Clone: %3|  
|Notas y resolución|Este es un evento de éxito y solo un problema si inesperado.|  
  
|||  
|-|-|  
|Identificador de evento|2215|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> se agrega el controlador de dominio clone en el siguiente sitio.<br /><br />Datos adicionales:<br /><br />Clonar Id: %1<br /><br />Sitio: %2|  
|Notas y resolución|Este es un evento de éxito y solo un problema si inesperado.|  
  
|||  
|-|-|  
|Identificador de evento|2216|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> se crea un contenedor de servidores para el controlador de dominio clone.<br /><br />Datos adicionales:<br /><br />Clonar Id: %1<br /><br />Contenedor de servidores: %2|  
|Notas y resolución|Este es un evento de éxito y solo un problema si inesperado.|  
  
|||  
|-|-|  
|Identificador de evento|2217|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> se crea un objeto de servidor para el controlador de dominio clone.<br /><br />Datos adicionales:<br /><br />Clonar Id: %1<br /><br />Objeto de servidor: %2|  
|Notas y resolución|Este es un evento de éxito y solo un problema si inesperado.|  
  
|||  
|-|-|  
|Identificador de evento|2218|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> se crea un objeto de configuración NTDS para el controlador de dominio clone.<br /><br />Datos adicionales:<br /><br />Clonar Id: %1<br /><br />Objeto: %2|  
|Notas y resolución|Este es un evento de éxito y solo un problema si inesperado.|  
  
|||  
|-|-|  
|Identificador de evento|2219|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> crear objetos de conexión para el controlador de dominio de solo lectura clone.<br /><br />Datos adicionales:<br /><br />Clonar Id: %1|  
|Notas y resolución|Este es un evento de éxito y solo un problema si inesperado.|  
  
|||  
|-|-|  
|Identificador de evento|2220|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> crear objetos SYSVOL para el controlador de dominio de solo lectura clone.<br /><br />Datos adicionales:<br /><br />Clonar Id: %1|  
|Notas y resolución|Este es un evento de éxito y solo un problema si inesperado.|  
  
|||  
|-|-|  
|Identificador de evento|2221|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Error|  
|Mensaje|<COMPUTERNAME> Error al generar una contraseña aleatoria para el controlador de dominio clonados.<br /><br />Datos adicionales:<br /><br />Clonar Id: %1<br /><br />Nombre del controlador de dominio Clone: %2<br /><br />Error: %3 %4|  
|Notas y resolución|Examina el registro de eventos del sistema para obtener más información sobre por qué no se pudo crear la contraseña de cuenta de máquina.|  
  
|||  
|-|-|  
|Identificador de evento|2222|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Error|  
|Mensaje|<COMPUTERNAME> no se pudo establecer contraseña para el controlador de dominio clonados.<br /><br />Datos adicionales:<br /><br />Clonar Id: %1<br /><br />Nombre del controlador de dominio Clone: %2<br /><br />Error: %3 %4|  
|Notas y resolución|Examina el registro de eventos del sistema para obtener más información sobre por qué no se pudo establecer la contraseña de cuenta de máquina.|  
  
|||  
|-|-|  
|Identificador de evento|2223|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|<COMPUTERNAME> establecer correctamente la contraseña de cuenta de equipo para el controlador de dominio clonados.<br /><br />Datos adicionales:<br /><br />Clonar Id: %1<br /><br />Nombre del controlador de dominio Clone: %2<br /><br />Total del número de reintentos: %3|  
|Notas y resolución|Este es un evento de éxito y solo un problema si inesperado.|  
  
|||  
|-|-|  
|Identificador de evento|2224|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Error|  
|Mensaje|No se pudo clonación del controlador de dominio virtual. El siguiente %1 cuentas de servicio administradas existe en el equipo clonado:<br /><br />%2<br /><br />Para clonar para tener éxito, se debe quitar todas las cuentas de servicio administradas. Esto puede realizarse mediante el cmdlet de PowerShell de quitar ADComputerServiceAccount.|  
|Notas y resolución|Espera al usar MSA independiente (no group MSA). Hacer *no* sigue el Consejo de evento para quitar la cuenta, está escrito incorrectamente. Usar Uninstall-AdServiceAccount - [https://technet.microsoft.com/library/hh852310](https://technet.microsoft.com/library/hh852310).<br /><br />Se reemplazaron MSA independiente - publicó por primera vez en Windows Server 2008 R2 - en Windows Server 2012 con MSA de grupo (gMSA). GMSAs admite la clonación.|  
  
|||  
|-|-|  
|Identificador de evento|2225|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Informativo|  
|Mensaje|Los secretos almacenado en caché de la entidad de seguridad siguiente se han quitado correctamente desde el controlador de dominio local:<br /><br />%1<br /><br />Después de la clonación un controlador de dominio de solo lectura, se quitará secretos que anteriormente se almacena en caché en el controlador de dominio de solo lectura de origen clonación del controlador de dominio clonados.|  
|Notas y resolución|Este es un evento de éxito y solo un problema si inesperado.|  
  
|||  
|-|-|  
|Identificador de evento|2226|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Error|  
|Mensaje|Error al quitar secretos almacenado en caché de la entidad de seguridad siguiente controlador de dominio local:<br /><br />%1<br /><br />Error: %2 (%3)<br /><br />Después de la clonación un controlador de dominio de solo lectura, secretos que anteriormente se almacena en caché en la necesidad de controlador de dominio de solo lectura de origen clonación retirarlas en la copia con el fin de reducir el riesgo de que un atacante puede obtener las credenciales de clone en peligro o robado. Si la entidad de seguridad es una cuenta con privilegios elevados y debe estar protegido contra esto, usa rootDSE operación rODCPurgeAccount borrar manualmente sus secretos en el controlador de dominio local.|  
|Notas y resolución|Examina los registros de eventos del sistema y los servicios de directorio para obtener más información.|  
  
|||  
|-|-|  
|Identificador de evento|2227|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Error|  
|Mensaje|Se genera una excepción al intentar eliminar información confidencial almacenada en caché de controlador de dominio local.<br /><br />Datos adicionales:<br /><br />El valor de excepción: %1<br /><br />Valor de error: %2<br /><br />DSID: %3<br /><br />Después de la clonación un controlador de dominio de solo lectura, secretos que anteriormente se almacena en caché en la necesidad de controlador de dominio de solo lectura de origen clonación retirarlas en la copia con el fin de reducir el riesgo de que un atacante puede obtener las credenciales de clone en peligro o robado. Si cualquiera de estas entidades de seguridad es una cuenta con privilegios elevados y deben estar protegidos contra esto, usa rootDSE operación rODCPurgeAccount borrar manualmente sus secretos en el controlador de dominio local.|  
|Notas y resolución|Examina los registros de eventos del sistema y los servicios de directorio para obtener más información.|  
  
|||  
|-|-|  
|Identificador de evento|2228|  
|Origen|Microsoft-Windows-ActiveDirectory_DomainService|  
|Gravedad|Error|  
|Mensaje|El identificador de generación de máquina Virtual en la base de datos de Active Directory de este controlador de dominio es diferente del valor actual de esta máquina virtual. Sin embargo, un archivo de configuración clone de controlador de dominio virtual (DCCloneConfig.xml) no se pudo encontrar por lo que no se intentó clonación del controlador de dominio. Si un controlador de dominio clonación operación concibió, asegúrese de que se proporciona un DCCloneConfig.xml en cualquiera de las ubicaciones compatibles. Además, la dirección IP de este controlador de dominio entra en conflicto con la dirección IP del otro controlador de dominio. Para garantizar que se producen sin interrupciones en el servicio, el controlador de dominio que se ha configurado para arrancar en DSRM.<br /><br />Datos adicionales:<br /><br />La dirección IP duplicada: %1|  
|Notas y resolución|Este mecanismo de protección deja de controladores de dominio duplicados cuando sea posible (lo hará no cuando se usa DHCP, por ejemplo). Agregar un archivo DcCloneConfig.xml válido, quita la marca DSRM y volver a intentar clonación|  
  
|||  
|-|-|  
|Identificador de evento|29218|  
|Origen|Microsoft Windows DirectoryServices DSROLE Server|  
|Gravedad|Error|  
|Mensaje|No se pudo clonación del controlador de dominio virtual. No se pudo completar la operación de clonación y el controlador de dominio clonados se reinicie en modo de restauración de servicios de directorio (DSRM).<br /><br />Por favor, verificación registrado con anterioridad a eventos y %systemroot%\debug\dcpromo.log para obtener más información sobre los errores que corresponden al controlador de dominio virtual clonación intento y si no se puede reutilizar esta imagen clone.<br /><br />Si una o varias entradas de registro indican que no se puede reintentar el proceso de clonación, deberá destruirse la imagen de forma segura. De lo contrario puede corregir los errores, borra la marca de arranque DSRM y reinicie normalmente. al reiniciar, se volverá a la operación de clonación.|  
|Notas y resolución|Revisar los registros de eventos del sistema y los servicios de directorio y dcpromo.log para obtener más información sobre el motivo del error de clonación.|  
  
|||  
|-|-|  
|Identificador de evento|29219|  
|Origen|Microsoft Windows DirectoryServices DSROLE Server|  
|Gravedad|Informativo|  
|Mensaje|Clonación del controlador de dominio virtual se realizó correctamente.|  
|Notas y resolución|Este es un evento de éxito y solo un problema si inesperado.|  
  
|||  
|-|-|  
|Identificador de evento|29248|  
|Origen|Microsoft Windows DirectoryServices DSROLE Server|  
|Gravedad|Error|  
|Mensaje|Clonación del controlador de dominio virtual no se pudo obtener notificaciones de Winlogon. El código de error devuelto es %1 (2 %).<br /><br />Para obtener más información sobre este error, revisa %systemroot%\debug\dcpromo.log para los errores que corresponden al controlador de dominio virtual clonación intento.|  
|Notas y resolución|Ponte en contacto con soporte técnico de Microsoft|  
  
|||  
|-|-|  
|Identificador de evento|29249|  
|Origen|Microsoft Windows DirectoryServices DSROLE Server|  
|Gravedad|Error|  
|Mensaje|Clonación del controlador de dominio virtual no se pudo analizar el archivo de configuración del controlador de dominio virtual.<br /><br />El código HRESULT devuelto es %1.<br /><br />El archivo de configuración es: %2<br /><br />Por favor, corregir los errores en el archivo de configuración y vuelve a intentar la operación de clonación.<br /><br />Para obtener más información sobre este error, consulta % systemroot%\debug\dcpromo.log.|  
|Notas y resolución|Examina el archivo dclconeconfig.xml errores de sintaxis con un editor de XML y el archivo de esquema DCCloneConfigSchema.xsd.|  
  
|||  
|-|-|  
|Identificador de evento|29250|  
|Origen|Microsoft Windows DirectoryServices DSROLE Server|  
|Gravedad|Error|  
|Mensaje|No se pudo clonación del controlador de dominio virtual. Hay software o servicios está habilitados en el controlador de dominio virtual clonados no está presente en la lista de aplicaciones permitidas para clonar de controlador de dominio virtual.<br /><br />A continuación es las entradas que faltan:<br /><br />%2<br /><br />%1 (si existe) se usó como la lista de inclusión definido.<br /><br />No se puede completar la operación de clonación si hay no clonación aplicaciones instaladas.<br /><br />Ejecute directorio PowerShell Cmdlet Get-ADDCCloningExcludedApplicationList del activo para comprobar las aplicaciones que están instaladas en el equipo clonado, pero no se incluyen en la lista de permitidos y agregarlas a la lista de permitidos, si son compatibles con la clonación del controlador de dominio virtual. Si cualquiera de estas aplicaciones no son compatible con la clonación del controlador de dominio virtual, desinstalarlos antes de volver a intentar la operación de clonación.<br /><br />El controlador de dominio virtual clonación proceso busca el archivo de lista del aplicación permitida, CustomDCCloneAllowList.xml, basado en el siguiente orden de búsqueda; se usa el primer archivo que se encuentra y se omiten todas las demás:<br /><br />1. el nombre del valor del registro: HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters\AllowListFolder<br /><br />2. el mismo directorio donde se encuentra la carpeta del directorio de trabajo de DSA<br /><br />3. %windir%\NTDS<br /><br />4. extraíbles de lectura y escritura multimedia en el orden de la letra de unidad en la raíz de la unidad|  
|Notas y resolución|Sigue las instrucciones del mensaje|  
  
|||  
|-|-|  
|Identificador de evento|29251|  
|Origen|Microsoft Windows DirectoryServices DSROLE Server|  
|Gravedad|Error|  
|Mensaje|Clonación del controlador de dominio virtual no se pudo restablecer las direcciones IP de la máquina clone.<br /><br />El código de error devuelto es %1 (2 %).<br /><br />Este error puede deberse a errores en las secciones de la configuración de red en el archivo de configuración del controlador de dominio virtual.<br /><br />Consulte %systemroot%\debug\dcpromo.log para obtener más información sobre los errores que corresponden a las direcciones IP restablecer durante el controlador de dominio virtual clonación intentos.<br /><br />Detalles sobre cómo restablecer direcciones IP en el equipo clonado pueden encontrarse en https://go.microsoft.com/fwlink/?LinkId=208030|  
|Notas y resolución|Compruebe la información en la dccloneconfig.xml IP es válida y no duplica el equipo de origen original.|  
  
|||  
|-|-|  
|Identificador de evento|29253|  
|Origen|Microsoft Windows DirectoryServices DSROLE Server|  
|Gravedad|Error|  
|Mensaje|No se pudo clonación del controlador de dominio virtual. El controlador de dominio clone no pudo encontrar al maestro de operaciones de dominio principal (PDC) del controlador en el clonados dominio del equipo principal de la máquina clonado.<br /><br />El código de error devuelto es %1 (2 %).<br /><br />Compruebe que el controlador de dominio principal en el dominio principal de la máquina clonado se asigna a un controlador de dominio dinámicos, está en línea y está en funcionamiento. Comprueba que la máquina clonada tiene conectividad LDAP/RPC al controlador de dominio principal sobre los protocolos y puertos necesarios.|  
|Notas y resolución|Valida la dirección IP de controlador de dominio clonados y se establece la información de DNS. Usar Dcdiag.exe /test:locatorcheck para validar si la PDCE está en línea, usa/Server Nltest.exe:* <PDCE> * /DCLIST:* <domain> * RPC válido, obtener una captura de red desde el PDCE durante la clonación se produce un error y analizar el tráfico.|  
  
|||  
|-|-|  
|Identificador de evento|29254|  
|Origen|Microsoft Windows DirectoryServices DSROLE Server|  
|Gravedad|Error|  
|Mensaje|Clonación del controlador de dominio virtual no se pudo enlazar con el controlador de dominio principal %1.<br /><br />El código de error devuelto es %2 (%3).<br /><br />Compruebe que el controlador de dominio principal %1 está en línea y está en funcionamiento. Comprueba que la máquina clonada tiene conectividad LDAP/RPC al controlador de dominio principal sobre los protocolos y puertos necesarios.|  
|Notas y resolución|Valida la dirección IP de controlador de dominio clonados y se establece la información de DNS. Usar Dcdiag.exe /test:locatorcheck para validar si la PDCE está en línea, usa/Server Nltest.exe:* <PDCE> * /DCLIST:* <domain> * RPC válido, obtener una captura de red desde el PDCE durante la clonación se produce un error y analizar el tráfico.|  
  
|||  
|-|-|  
|Identificador de evento|29255|  
|Origen|Microsoft Windows DirectoryServices DSROLE Server|  
|Gravedad|Error|  
|Mensaje|No se pudo clonación del controlador de dominio virtual.<br /><br />Un intento de crear objetos en el controlador de dominio principal %1 necesario para la imagen que se clonan devolvió el error %2 (%3).<br /><br />Compruebe que el controlador de dominio clonados tiene privilegios clonar sí. Busca eventos relacionados en el registro de eventos de servicio de directorio en el controlador de dominio principal %1.|  
|Notas y resolución|Búsqueda en Microsoft TechNet, Knowledgebase MS y MS blogs para determinar su significado habitual y, a continuación, solucionar el error específico que se basa en los resultados.|  
  
|||  
|-|-|  
|Identificador de evento|29256|  
|Origen|Microsoft Windows DirectoryServices DSROLE Server|  
|Gravedad|Error|  
|Mensaje|Error al intentar establecer el arranque en la marca de modo de restauración de servicios de directorio con código de error %1.<br /><br />Consulte %systemroot%\debug\dcpromo.log para obtener más información sobre los errores.|  
|Notas y resolución|Examina el registro de servicios de directorio y dcpromo.log para obtener más información. Examina los registros de eventos de aplicación y del sistema. Investigar la aplicación de terceros que puede bloquear el uso de privilegios.|  
  
|||  
|-|-|  
|Identificador de evento|29257|  
|Origen|Microsoft Windows DirectoryServices DSROLE Server|  
|Gravedad|Error|  
|Mensaje|Se ha realizado la clonación del controlador de dominio virtual. Error al intentar reiniciar el equipo con el código de error %1.<br /><br />Reinicie el equipo para finalizar la operación de clonación.|  
|Notas y resolución|Examina los registros de eventos de aplicación y del sistema. Investigar la aplicación de terceros que puede bloquear el uso de privilegios.|  
  
|||  
|-|-|  
|Identificador de evento|29264|  
|Origen|Microsoft Windows DirectoryServices DSROLE Server|  
|Gravedad|Error|  
|Mensaje|Error al intentar borrar el arranque en la marca de modo de restauración de servicios de directorio con código de error %1.<br /><br />Consulte %systemroot%\debug\dcpromo.log para obtener más información sobre los errores.|  
|Notas y resolución|Examina el registro de servicios de directorio y dcpromo.log para obtener más información. Examina los registros de eventos de aplicación y del sistema. Investigar la aplicación de terceros que puede bloquear el uso de privilegios.|  
  
|||  
|-|-|  
|Identificador de evento|29265|  
|Origen|Microsoft Windows DirectoryServices DSROLE Server|  
|Gravedad|Informativo|  
|Mensaje|Clonación del controlador de dominio virtual se realizó correctamente. El archivo de configuración clonación %1 de dominio virtual controlador ha cambiado de nombre a %2.|  
|Notas y resolución|N/D, este es un evento de éxito.|  
  
|||  
|-|-|  
|Identificador de evento|29266|  
|Origen|Microsoft Windows DirectoryServices DSROLE Server|  
|Gravedad|Error|  
|Mensaje|Clonación del controlador de dominio virtual se realizó correctamente. Error al intentar cambiar el nombre de archivo de configuración clonación dominio virtual controlador %1 con código de error %2 (%3).|  
|Notas y resolución|Cambiar el nombre de archivo dccloneconfig.xml manualmente.|  
  
|||  
|-|-|  
|Identificador de evento|29267|  
|Origen|Microsoft Windows DirectoryServices DSROLE Server|  
|Gravedad|Error|  
|Mensaje|Clonación del controlador de dominio virtual no se pudo comprobar que la clonación del controlador de dominio virtual permite la lista de aplicaciones.<br /><br />El código de error devuelto es %1 (2 %).<br /><br />Este error puede estar causado por una sintaxis de error en la copia de permitir que el archivo de lista (el archivo desprotegido actualmente está: %3). Para obtener más información sobre este error, consulta % systemroot%\debug\dcpromo.log.|  
|Notas y resolución|Sigue las instrucciones del evento|  
  
##### <a name="error-messages"></a>Mensajes de error  
No existen errores interactivos directos para clonar de controlador de dominio virtualizada con errores; clonación toda la información que se registra en el sistema y la sesión de servicios de directorio y la promoción del controlador de dominio que se inicie sesión dcpromo.log. Sin embargo, si el servidor se arranca en modo de restauración de DS, investigar inmediatamente, como promoción o clonación errónea.  
  
Dcpromo.log es el primer lugar para comprobar si clonación error. Según el error aparece, puede ser necesario posteriormente revisar registros de sistema de otros diagnósticos y servicios de directorio.  
  
#### <a name="known-issues-and-support-scenarios"></a>Problemas conocidos y escenarios de soporte técnico  
Estos son los problemas comunes que se detectaron durante el proceso de desarrollo de Windows Server 2012. Todos estos problemas son "por diseño" y tengan una solución alternativa válida o técnica más adecuada para evitarlos en primer lugar. Algunas pueden haber resuelto en versiones posteriores de Windows Server 2012.  
  
|||  
|-|-|  
|**Problema**|**Se produce un error, DSRM la clonación**|  
|**Síntomas**|Clonar arranca en modo de restauración de servicios de directorio|  
|**Resolución y notas**|Validar todos los pasos seguidos de sección de la implementación de controlador de dominio virtualizados de secciones y [metodología General para la solución de problemas clonación del controlador de dominio](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)<br /><br />Se describe en KB 2742844.|  
  
|||  
|-|-|  
|**Problema**|**Concesiones IP adicionales al usar DHCP para clonar**|  
|**Síntomas**|Después de clonación correctamente un controlador de dominio y con DHCP, el primer arranque del duplicado toma una DHCP. A continuación, cuando el servidor se cambió de nombre y reinicie como un controlador de dominio, toma una segunda DHCP. La primera dirección IP no se libera y terminan con un permiso "fantasma"|  
|**Resolución y notas**|Eliminar la concesión de dirección no utilizados en DHCP o permitir a que expire normalmente manualmente. Se describe en KB 2742836.|  
  
|||  
|-|-|  
|**Problema**|**Clonación se produce un error en DSRM tras una demora mucho**|  
|**Síntomas**|Clonación aparece en pausa en "clonación del controlador de dominio es x % de completarse" para entre 8 y 15 minutos. Después, la clonación se produce un error y se inicia en DSRM.|  
|**Resolución y notas**|El equipo clonado no puede obtener una dirección IP dinámica de DHCP o SLAAC, o está usando una dirección IP duplicada o no puede encontrar el PDC. Varios reintentos realizadas clonación provocar el retraso. Resolver el problema de red para permitir la clonación.<br /><br />Se describe en KB 2742844.|  
  
|||  
|-|-|  
|**Problema**|**Clonación no volver a crear todos los nombres de entidad de seguridad de servicio**|  
|**Síntomas**|Si un conjunto de *tres partes* nombres principales de servicio (SPN) que se incluye un nombre NetBIOS con un puerto y un nombre NetBIOS idéntico sin un puerto, la entrada no puerto no se vuelve a crear con el nuevo nombre de equipo. Por ejemplo:<br /><br />customspn / DC1:200 / usar app1 válida de símbolos *Esto se vuelve a crear con el nuevo nombre de equipo*<br /><br />DC1/customspn/app1 uso no válido de símbolos *esto no se vuelve a crear con el nuevo nombre de equipo*<br /><br />Los nombres completos se vuelven a crear y se vuelven a crear el SPN sin tres partes, independientemente de puertos. Por ejemplo, estos se vuelve a crear correctamente en la copia de:<br /><br />customspn usa DC1:202 válida de símbolos *Esto se vuelve a crear*<br /><br />customspn/DC1 símbolos no válido de uso *Esto se vuelve a crear*<br /><br />customspn/DC1.corp.contoso.com:202 no válido de uso de símbolos *se vuelve a crear este nombre*<br /><br />USO de símbolos no válido de customspn/DC1.corp.contoso.com *Esto se vuelve a crear*|  
|**Resolución y notas**|Esta es una limitación del proceso de cambio de nombre del controlador de dominio en Windows, no solo en clonación. La lógica de cambio de nombre en cualquier escenario no administra el SPN de tres partes. Más incluye servicios de Windows no se ven afectados por esto, como volver a cualquier faltantes SPN según sea necesario. Otras aplicaciones pueden requerir introducir manualmente el SPN para resolver el problema.<br /><br />Se describe en KB 2742874.|  
  
|||  
|-|-|  
|**Problema**|**Clonación se produce un error, arranca en DSRM, errores de red generales**|  
|**Síntomas**|Clonar arranca en modo de reparación de servicios de directorio. Hay general de errores de red.|  
|Resolución y notas|Asegúrate de que la nueva copia no tiene una dirección MAC estática duplicada asignada desde el controlador de dominio de origen; Puedes ver si una máquina virtual usa direcciones MAC estáticas al ejecutar este comando en el host de hipervisor para el origen y el clone máquinas virtuales:<br /><br />VM de Get - VMName *vm de prueba* & #124; Get-VMNetworkAdapter & #124; Florida *<br /><br />Cambiar la dirección MAC a una dirección única estática o cambiar para usar direcciones MAC dinámicas.<br /><br />Se describe en KB 2742844|  
  
|||  
|-|-|  
|**Problema**|**Clonación se produce un error, arranca en DSRM como un duplicado del origen de controlador de dominio**|  
|**Síntomas**|Inicia una nueva copia sin clonación. No se cambia la dccloneconfig.xml y el servidor se inicia en modo de restauración de DS. Error 2164 se muestra el registro de eventos de servicios de directorio<br /><br />*<COMPUTERNAME>*no se pudo iniciar el servicio de DsRoleSvc para clonar el controlador de dominio virtual local.|  
|**Resolución y notas**|Examinar la configuración del servicio para el servicio de rol servidor de DS (DsRoleSvc) y asegúrate de que su tipo de inicio se establece de forma manual. Valida que no hay ningún programa de terceros impide el inicio de este servicio.<br /><br />Para obtener más información sobre cómo recuperar este controlador de dominio secundario al mismo tiempo que las actualizaciones se replican saliente, consulta el artículo de Microsoft KB 2742970.|  
  
|||  
|-|-|  
|**Problema**|**Clonación se produce un error, arranca en DSRM, error 8610**|  
|**Síntomas**|Clonar arranca en modo de restauración de servicios de directorio. . Log Dcpromo muestra el error 8610 (que es ERROR_DS_ROLE_NOT_VERIFIED 8610 o 0x21A2)|  
|**Resolución y notas**|Se producirá si el PDC puede ser reconocible, pero no ha realizado la replicación suficiente para permitir que asumir la función. Por ejemplo, si se inicia la clonación y otro administrador mueve a la función FSMO PDCE a un nuevo controlador de dominio.<br /><br />Se describe en KB 2742916.|  
  
|||  
|-|-|  
|**Problema**|**Clonación se produce un error, arranca en DSRM, errores de red generales**|  
|**Síntomas**|Clonar arranca en modo de restauración de servicios de directorio. Hay general de errores de red.|  
|**Resolución y notas**|Asegúrate de que la nueva copia no tiene una dirección MAC estática duplicada asignada desde el controlador de dominio de origen; Puedes ver si una máquina virtual usa direcciones MAC estáticas al ejecutar este comando en el host de Hyper-V para el origen y el clone máquinas virtuales:<br /><br />VM de Get - VMName *vm de prueba* & #124; Get-VMNetworkAdapter & #124; Florida *<br /><br />Cambiar la dirección MAC a una dirección única estática o cambiar para usar direcciones MAC dinámicas.<br /><br />Se describe en KB 2742844.|  
  
|||  
|-|-|  
|**Problema**|**Se produce un error de clonación, arranca en DSRM**|  
|**Síntomas**|Clonar arranca en modo de reparación de servicios de directorio|  
|**Resolución y notas**|Asegúrese de que la dccloneconfig.xml contiene la definición de esquema (consulta sampledccloneconfig.xml, línea 2):<br /><br />**< d3c:DCCloneConfig xmlns:d3c="uri:microsoft.com:schemas:DCCloneConfig" >**<br /><br />Se describe en KB 2742844|  
  
|||  
|-|-|  
|Problema|**Ningún servidor es el registro de errores disponible en DSRM**|  
|**Síntomas**|Clonar arranca en modo de reparación de servicios de directorio. Intenta iniciar sesión y recibe el error:<br /><br />**Actualmente no hay ningún servidor está disponibles para la solicitud de inicio de sesión**|  
|**Resolución y notas**|Asegúrate de que iniciar sesión con la cuenta de administrador DSRM y no la cuenta de dominio. Usa la flecha izquierda y escribe el nombre de usuario:<br /><br />**. \administrator**<br /><br />Se describe en KB 2742908|  
  
|||  
|-|-|  
|**Problema**|**Se produce un error en el origen de clonación DSRM, error**|  
|**Síntomas**|Durante la clonación, se produce un error 8437 "crear clone DC objetos en PDC error" (0x20f5)|  
|**Resolución y notas**|Nombre de equipo duplicado se estableció en DCCloneConfig.xml como el origen de controlador de dominio o un controlador de dominio existente. El nombre del equipo también debe estar en el formato de nombre de equipo NetBIOS (15 caracteres o menos, no un FQDN).<br /><br />Corregir el archivo dccloneconfig.xml estableciendo un nombre único válido.<br /><br />Se describe en KB 2742959|  
  
|||  
|-|-|  
|**Problema**|**Error de nuevo addccloneconfigfile "el índice está fuera del intervalo"**|  
|**Síntomas**|Cuando se ejecuta el cmdlet de nuevo addccloneconfigfile, recibe el error:<br /><br />Índice está fuera del intervalo. Debe ser no negativo y menor que el tamaño de la colección.|  
|**Resolución y notas**|Debes ejecutar el cmdlet en una consola de Windows PowerShell administrador privilegios elevados. Este error se deba a la falta de pertenencia a grupos de administrador local en el equipo.<br /><br />Se describe en KB 2742927|  
  
|||  
|-|-|  
|**Problema**|**Clonación se produce un error, DC duplicado**|  
|**Síntomas**|Clonar arranca sin clonación duplicados existente DC de origen|  
|**Resolución y notas**|El equipo se ha copiado inicia pero no contiene un archivo de DcCloneConfig.xml en cualquiera de las ubicaciones admitidas y no tenía una dirección IP duplicada con el controlador de dominio de origen. El controlador de dominio debe quitarse correctamente con el fin de evitar la pérdida de datos.<br /><br />Se describe en KB 2742970|  
  
|||  
|-|-|  
|**Problema**|**Se produce un error en la nueva ADDCCloneConfigFile con el servidor no está operativa error cuando comprueba si el controlador de dominio de origen es un miembro del grupo de controladores de dominio clonación si un catálogo global no está disponible.**|  
|**Síntomas**|Cuando se ejecuta ADDCCloneConfigFile de nuevo para crear un archivo dccloneconfig.xml, recibe el error:<br /><br />Código: el servidor no está operativo|  
|**Resolución y notas**|Comprobar la conectividad a un catálogo global del servidor donde ejecutar ADDCCloneConfigFile de nuevo y comprueba que la pertenencia del controlador de dominio de origen en el grupo de controladores de dominio clonación se replica en ese GC.<br /><br />Ejecuta el siguiente comando como un medio de vaciado de caché de ubicador de DC para los casos donde un GC o un controlador de dominio puede haber quedado sin conexión recientemente:<br /><br />Código - nltest/dsgetdc: /GC/Force|  
  
### <a name="advanced-troubleshooting"></a>Solución avanzada de problemas  
Este módulo intenta enseñar la solución avanzada de problemas mediante el uso de *trabajar* registros como muestras, con una explicación de lo que ha ocurrido. Si conoces el aspecto de una operación de controlador de dominio virtualizada correcta, errores sean obvias en su entorno. Estos registros se presentan por su origen, con el orden ascendente de *espera* eventos (incluso cuando se encuentran errores y advertencias) relacionados con un controlador de dominio clonados dentro de cada registro.  
  
#### <a name="cloning-a-domain-controller"></a>Clonación un controlador de dominio  
En este ejemplo, el controlador de dominio clone usa DHCP para obtener una dirección IP, replica SYSVOL con FRS o DFSR (consulta el registro adecuado según sea necesario), es un catálogo global y usa un archivo de dccloneconfig.xml en blanco.  
  
##### <a name="directory-services-event-log"></a>Registro de eventos de servicios de directorio  
El registro de servicios de directorio contiene la mayoría de basado en eventos clonación información operativa. El hipervisor cambia el identificador de generación de la máquina virtual y notas el servicio NTDS, invalida el conjunto de RID y cambia el identificador de invocación. Se establece el nuevo ID de generación de la máquina virtual y el servidor replica los datos de entrada de Active Directory. Se detiene el servicio DFSR y se elimina en su base de datos que hospeda SYSVOL, si la fuerzas una sincronización no autorizado entrante. Se ajusta el límite máximo de USN.  
  
||||  
|-|-|-|  
|**Identificador de evento**|**Origen**|**Mensaje**|  
|**2160**|ActiveDirectory_DomainService|Los servicios de dominio de Active Directory local ha encontrado un archivo de configuración de clonación de controlador de dominio virtual.<br /><br />El archivo de configuración de clonación de controlador de dominio virtual se encuentra en:<br /><br />*<path>*\DCCloneConfig.Xml<br /><br />La existencia del archivo de configuración de clonación de controlador de dominio virtual indica que el controlador de dominio virtual local es un clone otro virtual del controlador de dominio. Los servicios de dominio de Active Directory se iniciará clonar sí.|  
|**2191**|ActiveDirectory_DomainService|Los servicios de dominio de Active Directory establece el siguiente valor del registro para deshabilitar las actualizaciones DNS.<br /><br />Clave del registro:<br /><br />SYSTEM\CurrentControlSet\Services\Netlogon\Parameters<br /><br />Valor del registro:<br /><br />UseDynamicDns<br /><br />Datos del valor del registro:<br /><br />0<br /><br />Durante el proceso de clonación, el equipo local puede tener el mismo nombre de equipo que el equipo de origen clone durante un rato. DNS A y registro de registro AAAA están deshabilitadas durante este período para que los clientes no pueden enviar solicitudes a la máquina local está sometiendo a clonación. El proceso de clonación te permitirá actualizaciones DNS nuevamente después de completa la clonación.|  
|**2191**|ActiveDirectory_DomainService|Los servicios de dominio de Active Directory establece el siguiente valor del registro para deshabilitar las actualizaciones DNS.<br /><br />Clave del registro:<br /><br />SYSTEM\CurrentControlSet\Services\Dnscache\Parameters<br /><br />Valor del registro:<br /><br />RegistrationEnabled<br /><br />Datos del valor del registro:<br /><br />0<br /><br />Durante el proceso de clonación, el equipo local puede tener el mismo nombre de equipo que el equipo de origen clone durante un rato. DNS A y registro de registro AAAA están deshabilitadas durante este período para que los clientes no pueden enviar solicitudes a la máquina local está sometiendo a clonación. El proceso de clonación te permitirá actualizaciones DNS nuevamente después de completa la clonación.<br /><br />"Información 2/7/2012 3:12:49 configuración interna de PM Microsoft-Windows-ActiveDirectory_DomainService 2191" los servicios de dominio de Active Directory establece el siguiente valor del registro para deshabilitar las actualizaciones DNS.<br /><br />Clave del registro:<br /><br />SYSTEM\CurrentControlSet\Services\Tcpip\Parameters<br /><br />Valor del registro:<br /><br />DisableDynamicUpdate<br /><br />Datos del valor del registro:<br /><br />1<br /><br />Durante el proceso de clonación, el equipo local puede tener el mismo nombre de equipo que el equipo de origen clone durante un rato. DNS A y registro de registro AAAA están deshabilitadas durante este período para que los clientes no pueden enviar solicitudes a la máquina local está sometiendo a clonación. El proceso de clonación te permitirá actualizaciones DNS nuevamente después de completa la clonación.|  
|**2172**|ActiveDirectory_DomainService|Leer el atributo msDS GenerationId del controlador de dominio objeto de equipo.<br /><br />valor del atributo msDS GenerationId:<br /><br />*<Number>*|  
|**2170**|ActiveDirectory_DomainService|Se ha detectado un cambio de identificador de generación.<br /><br />Id. de generación que se almacenan en caché en DS (valor antiguo):<br /><br />*<Number>*<br /><br />Id. de generación actualmente en la máquina virtual (nuevo valor):<br /><br />*<Number>*<br /><br />Después de la aplicación de una instantánea de la máquina virtual, después de una operación de importación de máquina virtual o una operación de migración en vivo, se produce el cambio de identificador de generación. Los servicios de dominio de Active Directory creará un nuevo identificador de invocación para recuperar el controlador de dominio. Controladores de dominio virtualizados no deben restaurarse mediante instantáneas de máquina virtual. El método admitido para restaurar o reversión el contenido de una base de datos de los servicios de dominio de Active Directory es restaurar una copia de seguridad realizada con una aplicación de copia de seguridad compatible con los servicios de dominio de Active Directory.|  
|**1109**|ActiveDirectory_DomainService|Se cambió el atributo de Id. de invocación de este servidor de directorio. El mayor número de secuencia de actualización en el momento en que se creó la copia de seguridad es la siguiente:<br /><br />Atributo de Id. de invocación (valor antiguo):<br /><br />*<GUID>*<br /><br />Atributo de Id. de invocación (nuevo valor):<br /><br />*<GUID>*<br /><br />Número de secuencia de actualización:<br /><br />*<Number>*<br /><br />Ha cambiado el Id. de invocación cuando un servidor de directorio se restaura desde un medio de copia de seguridad, está configurado para hospedar una partición de directorio de escritura de aplicación, se ha reanudado después de aplicar una instantánea de la máquina virtual, después de una operación de importación de máquina virtual o una operación de migración en vivo. Controladores de dominio virtualizados no deben restaurarse mediante instantáneas de máquina virtual. El método admitido para restaurar o reversión el contenido de una base de datos de los servicios de dominio de Active Directory es restaurar una copia de seguridad realizada con una aplicación compatible con servicios de dominio de Active Directory de copia de seguridad.|  
|**1000**|ActiveDirectory_DomainService|Inicio de Microsoft Active Directory Domain Services completo.|  
|**1394**|ActiveDirectory_DomainService|Se ha eliminado todos los problemas de evitar las actualizaciones a la base de datos de los servicios de dominio de Active Directory. Nuevas actualizaciones a la base de datos de los servicios de dominio de Active Directory se realizan correctamente. Reinicie el servicio de Net Logon|  
|**2163**|ActiveDirectory_DomainService|Se inició el servicio de DsRoleSvc clonar el controlador de dominio virtual local.|  
|**326**|NTDS ISAM|NTDSA NTDS (536): El motor de base de datos adjunta una base de datos (1, C:\Windows\NTDS\ntds.dit). (Tiempo = 0 segundos)<br /><br />Secuencia de intervalo de tiempo interno: 0,000 [1], 0,000 [2], [3] 0,000, 0,000 [4], 0,000 [5], [6] 0.016, 0,000 [7], [8] 0,000, 0,000 [9], 0,000 [10], 0,000 [11], 0,000 [12].<br /><br />Guardar en caché: 1|  
|**103**|NTDS ISAM|NTDSA NTDS (536): En el motor de base de datos se detuvo la instancia (0).<br /><br />Cierre incorrecto: 0<br /><br />Secuencia de intervalo de tiempo interno: 0,000 [1], 0,000 [2], 0,000 [3], 0,000 [4], 0.032 [5], 0,000 [6], 0,000 [7], [8] 0,000, 0.031 [9], 0,000 [10], 0,000 [11], [12] 0,000, 0,000 [13], [14] 0,000, 0,000 [15].|  
|**102**|NTDS ISAM|NTDSA NTDS (536): El motor de base de datos (6.02.8225.0000) está iniciando una nueva instancia (0).|  
|**105**|NTDS ISAM|NTDSA NTDS (536): El motor de base de datos inició una nueva instancia (0). (Tiempo = 0 segundos)<br /><br />Secuencia de intervalo de tiempo interno: 0.016 [1], 0,000 [2], [3] 0,015, 0.078 [4], 0,000 [5], 0,000 [6], [7] 0,000, 0,000 [8], [9] 0.046, 0,000 [10], 0,000 [11].|  
|**1004**|ActiveDirectory_DomainService|Los servicios de dominio de Active Directory se cerró correctamente.|  
|**102**|NTDS ISAM|NTDSA NTDS (536): El motor de base de datos (6.02.8225.0000) está iniciando una nueva instancia (0).|  
|**326**|NTDS ISAM|NTDSA NTDS (536): El motor de base de datos adjunta una base de datos (1, C:\Windows\NTDS\ntds.dit). (Tiempo = 0 segundos)<br /><br />Secuencia de intervalo de tiempo interno: 0,000 [1], 0,015 [2], [3] 0.016, 0,000 [4], [5] 0.031, 0,000 [6], 0,000 [7], [8] 0,000, 0,000 [9], 0,000 [10], 0,000 [11], 0,000 [12].<br /><br />Guardar en caché: 1|  
|**105**|NTDS ISAM|NTDSA NTDS (536): El motor de base de datos inició una nueva instancia (0). (Tiempo = 1 segundos)<br /><br />Secuencia de intervalo de tiempo interno: 0.031 [1], 0,000 [2], [3] 0,000, 0.391 [4], 0,000 [5], 0,000 [6], [7] 0,000, 0,000 [8], [9] 0.031, 0,000 [10], 0,000 [11].|  
|**1109**|ActiveDirectory_DomainService|Se cambió el atributo de Id. de invocación de este servidor de directorio. El mayor número de secuencia de actualización en el momento en que se creó la copia de seguridad es la siguiente:<br /><br />Atributo de Id. de invocación (valor antiguo):<br /><br />*<GUID>*<br /><br />Atributo de Id. de invocación (nuevo valor):<br /><br />*<GUID>*<br /><br />Número de secuencia de actualización:<br /><br />*<Number>*<br /><br />Ha cambiado el Id. de invocación cuando un servidor de directorio se restaura desde un medio de copia de seguridad, está configurado para hospedar una partición de directorio de escritura de aplicación, se ha reanudado después de aplicar una instantánea de la máquina virtual, después de una operación de importación de máquina virtual o una operación de migración en vivo. Controladores de dominio virtualizados no deben restaurarse mediante instantáneas de máquina virtual. El método admitido para restaurar o reversión el contenido de una base de datos de los servicios de dominio de Active Directory es restaurar una copia de seguridad realizada con una aplicación compatible con servicios de dominio de Active Directory de copia de seguridad.|  
|**1168**|ActiveDirectory_DomainService|Error interno: se ha producido el error An Active Directory Domain Services.<br /><br />Datos adicionales<br /><br />Valor de error (decimal):<br /><br />2<br /><br />Valor de error (hexadecimal):<br /><br />2<br /><br />Id. interno:<br /><br />7011658|  
|**1110**|ActiveDirectory_DomainService|Se retrasará la promoción de este controlador de dominio en un catálogo global para el siguiente intervalo.<br /><br />Intervalo (minutos):<br /><br />5<br /><br />Este retraso es necesario para que se puedan preparar las particiones de directorios necesaria antes de que se anuncia el catálogo global. En el registro, puedes especificar el número de segundos que esperará el agente de directorio del sistema para promover el controlador de dominio local a un catálogo global. Para obtener más información sobre el valor del registro de anuncio de retraso de catálogo Global, consulta a guía distribuyen sistemas del Kit de recursos|  
|**103**|NTDS ISAM|NTDSA NTDS (536): En el motor de base de datos se detuvo la instancia (0).<br /><br />Cierre incorrecto: 0<br /><br />Secuencia de intervalo de tiempo interno: 0,000 [1], 0,000 [2], 0,000 [3], 0,000 [4], 0.047 [5], 0,000 [6], 0,000 [7], [8] 0,000, 0.016 [9], 0,000 [10], 0,000 [11], [12] 0,000, 0,000 [13], [14] 0,000, 0,000 [15].|  
|**1004**|ActiveDirectory_DomainService|Los servicios de dominio de Active Directory se cerró correctamente.|  
|**1539**|ActiveDirectory_DomainService|Los servicios de dominio de Active Directory no se puede deshabilitar la caché de escritura del disco duro basadas en el software en el disco duro siguiente.<br /><br />Disco duro:<br /><br />c:<br /><br />Datos es posible que se pierdan durante errores del sistema|  
|**2179**|ActiveDirectory_DomainService|Se ha establecido el atributo msDS GenerationId del controlador de dominio objeto de equipo en el parámetro siguiente:<br /><br />Atributo GenerationID:<br /><br />*<Number>*|  
|**2173**|ActiveDirectory_DomainService|Error al leer el atributo msDS GenerationId del controlador de dominio objeto de equipo. Esto puede deberse a errores de la transacción de base de datos o el identificador de generación no existe en la base de datos local. No existe el GenerationId msDS durante el primer reinicio después dcpromo o el controlador de dominio no es un controlador de dominio virtual.<br /><br />Datos adicionales<br /><br />Código de error:<br /><br />6|  
|**1000**|ActiveDirectory_DomainService|Inicio de Microsoft Active Directory Domain Services completado, versión 6.2.8225.0|  
|**1394**|ActiveDirectory_DomainService|Se ha eliminado todos los problemas de evitar las actualizaciones a la base de datos de los servicios de dominio de Active Directory. Nuevas actualizaciones a la base de datos de los servicios de dominio de Active Directory se realizan correctamente. Se ha reiniciado el servicio de Net Logon.|  
|**1128**|ActiveDirectory_DomainService|Comprobador de coherencia 1128 "una conexión de replicación fue creada desde el servicio de directorio de origen siguientes al servicio de directorio local.<br /><br />Servicio de directorio de origen:<br /><br />CN = NTDS Settings,*<Domain Controller DN>*<br /><br />Servicio de directorio local:<br /><br />CN = NTDS Settings,*<Domain Controller DN>*<br /><br />Datos adicionales<br /><br />Código de razón:<br /><br />0 x 2<br /><br />Crear puntos ID interna:<br /><br />f0a025d|  
|**1999**|ActiveDirectory_DomainService|El servicio de directorio de origen optimizado el número de secuencia de actualización (USN) presentado por el servicio de directorio de destino. Los servicios de directorio de origen y de destino tienen un duplicador comunes. El servicio de directorio de destino está actualizado con el asociado de replicación comunes y el servicio de directorio de origen se instaló con una copia de seguridad de este asociado.<br /><br />Identificador de servicio de directorio de destino:<br /><br />*<GUID> (<FQDN>)*<br /><br />Identificador de servicio de directorio comunes:<br /><br />*<GUID>*<br /><br />Propiedad común USN:<br /><br />*<Number>*<br /><br />Como resultado, el vector de actualización del servicio de directorio de destino se ha configurado con la siguiente configuración.<br /><br />Objeto anterior USN:<br /><br />0<br /><br />Propiedad anterior USN:<br /><br />0<br /><br />GUID de la base de datos:<br /><br />*<GUID>*<br /><br />Objeto USN:<br /><br />*<Number>*<br /><br />Propiedad USN:<br /><br />*<Number>*|  
  
##### <a name="system-event-log"></a>Registro de eventos del sistema  
Las siguientes indicaciones de operaciones de clonación se encuentran en el registro de eventos del sistema. Como el hipervisor indica al equipo de invitado que se clonan o restaurar a partir de una instantánea, el controlador de dominio invalida inmediatamente su grupo RID para evitar la duplicación de entidades de seguridad más adelante. Como clonación ganancias, aparecen varias operaciones esperadas y mensajes, principalmente alrededor de iniciar y detener los servicios y algunos espera errores causados por este. Cuando se completa las notas de registro de eventos del sistema generales clonación éxito.  
  
||||  
|-|-|-|  
|**Identificador de evento**|**Origen**|**Mensaje**|  
|**16654**|Servicios de directorio de SAM|Se ha invalidado un conjunto de identificadores de cuenta (RID). Esto puede ocurrir en los siguientes casos esperados:<br /><br />1. un controlador de dominio se restaura desde la copia de seguridad.<br /><br />2. un controlador de dominio que se ejecuta en una máquina virtual se restaura desde la instantánea.<br /><br />3. un administrador ha invalidado manualmente el grupo|  
|**7036**|Administrador de Control de servicio|El servicio de Active Directory Domain Services entró en estado de ejecución.|  
|**7036**|Administrador de Control de servicio|El servicio del centro de distribución de claves Kerberos entró en estado de ejecución.|  
|**3096**|Netlogon|No se pudo encontrar el controlador de dominio principal para este dominio.|  
|**7036**|Administrador de Control de servicio|El servicio de administrador de cuentas de seguridad entró en estado de ejecución.|  
|**7036**|Administrador de Control de servicio|El servicio de servidor entró en estado de ejecución.|  
|**7036**|Administrador de Control de servicio|El servicio de Netlogon entró en estado de ejecución.|  
|**7036**|Administrador de Control de servicio|El servicio de servicios de Active Directory Web entró en estado de ejecución.|  
|**7036**|Administrador de Control de servicio|El servicio de replicación DFS entró en estado de ejecución.|  
|**7036**|Administrador de Control de servicio|El servicio de servicio de replicación entró en estado de ejecución.|  
|**14533**|Microsoft-Windows-DfsSvc|DFS ha terminado de crear todos los espacios de nombres.|  
|**14531**|Microsoft-Windows-DfsSvc|Servidor DFS ha terminado de inicialización.|  
|**7036**|Administrador de Control de servicio|El servicio de DFS Namespace entró en estado de ejecución.|  
|**7023**|Administrador de Control de servicio|El servicio de mensajería entre sitios terminó con el siguiente error:<br /><br />El servidor especificado no puede realizar la operación solicitada.|  
|**7036**|Administrador de Control de servicio|El servicio de mensajería entre sitios entró en estado de parada.|  
|**5806**|Netlogon|Se han deshabilitado las actualizaciones dinámicas manualmente en este controlador de dominio.<br /><br />ACCIÓN DEL USUARIO<br /><br />Volver a configurar este controlador de dominio para utilizar las actualizaciones dinámicas o agregar manualmente los registros DNS desde el archivo '% SystemRoot%\System32\Config\Netlogon.dns' a la base de datos DNS."|  
|**16651**|Servicios de directorio de SAM|No se pudo realizar la solicitud de un nuevo grupo de identificadores de cuenta. Se volverá a la operación hasta que la solicitud se realiza correctamente. El error es<br /><br />No se pudo realizar la operación FSMO solicitada. No se pudo contactar con el titular de FSMO actual.|  
|**7036**|Administrador de Control de servicio|El servicio de servidor DNS entró en estado de ejecución.|  
|**7036**|Administrador de Control de servicio|El servicio de rol servidor de DS entró en estado de ejecución.|  
|**7036**|Administrador de Control de servicio|El servicio de Netlogon entró en estado de parada.|  
|**7036**|Administrador de Control de servicio|El servicio de servicio de replicación entró en estado de parada.|  
|**7036**|Administrador de Control de servicio|El servicio del centro de distribución de claves Kerberos entró en estado de parada.|  
|**7036**|Administrador de Control de servicio|El servicio de servidor DNS entró en estado de parada.|  
|**7036**|Administrador de Control de servicio|El servicio de Active Directory Domain Services entró en estado de parada.|  
|**7036**|Administrador de Control de servicio|El servicio de Netlogon entró en estado de ejecución.|  
|**7040**|Administrador de Control de servicio|El tipo de inicio del servicio de los servicios de dominio de Active Directory se cambió de inicio automático en deshabilitado.|  
|**7036**|Administrador de Control de servicio|El servicio de Netlogon entró en estado de parada.|  
|**7036**|Administrador de Control de servicio|El servicio de servicio de replicación entró en estado de ejecución.|  
|**29219**|Servidor de DSROLE DirectoryServices|Clonación del controlador de dominio virtual se realizó correctamente.|  
|**29223**|Servidor de DSROLE DirectoryServices|Este servidor ahora es un controlador de dominio.|  
|**29265**|Servidor de DSROLE DirectoryServices|Clonación del controlador de dominio virtual se realizó correctamente. El controlador de dominio virtual clonación C:\Windows\NTDS\DCCloneConfig.xml del archivo de configuración ha cambiado de nombre a C:\Windows\NTDS\DCCloneConfig.20120207-151533.xml.|  
|**1074**|User32|El proceso C:\Windows\system32\lsass.exe (DC2) ha iniciado el reinicio del equipo DC2 en nombre de usuario NT AUTHORITY\SYSTEM por el motivo siguiente: sistema operativo: reconfiguración (planeada)<br /><br />Código de razón: 0 x 80020004<br /><br />Tipo de apagado: reiniciar<br /><br />Comentario: "|  
  
##### <a name="dcpromolog"></a>DCPROMO. REGISTRO  
Dcpromo.log contiene la parte de promoción de clonación que no se describe el registro de eventos de servicios de directorio. Dado que el registro no se proporciona el nivel de explicación que aportar las entradas de registro de eventos, esta sección del módulo contiene anotación adicional.  
  
El medio del proceso de promoción que comienza la clonación, el controlador de dominio se se depuran de su configuración actual y promueve volver a usar la base de datos de anuncios existente (muy similar a una promoción IFM) y, luego, el controlador de dominio replica deltas de cambio de entrada de anuncio y SYSVOL y clonación se complete.  
  
> [!NOTE]  
> El registro se ha modificado en este módulo para mejorar la legibilidad, mediante la eliminación de la columna de fecha.  
  
> [!NOTE]  
> Para obtener una explicación más de dcpromo.log consulta la comprender y solucionar problemas AD DS simplificada administración en Windows Server 2012.  
>   
> [https://go.microsoft.com/fwlink/p/?LinkId=237244](https://go.microsoft.com/fwlink/p/?LinkId=237244)  
  
-   Inicio basadas en clones promoción  
  
-   Establece la marca de modo de restauración de servicios de directorio para que el servidor no se inicia una copia de seguridad normalmente como clone original y causa de nomenclatura o las colisiones de servicio de directorio  
  
-   Actualizar el registro de eventos de servicios de directorio  
  
```  
15:14:01 [INFO] vDC Cloneing: Setting Boot into DSRM flag succeeded.  
15:14:01 [WARNING] Cannot get user Token for Format Message: 1725l  
15:14:01 [INFO] vDC Cloning: Created vDCCloningUpdate event.  
15:14:01 [INFO] vDC Cloning: Created vDCCloningComplete event.  
```  
  
-   Detener el servicio de Net Logon para que el controlador de dominio no anuncia  
  
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
  
-   Examina el archivo dccloneconfig.xml para personalizaciones especificada por el administrador.  
  
-   En este caso, muestra es un archivo en blanco, por lo que se genera automáticamente toda la configuración y el direccionamiento IP automático es necesario de la red  
  
```  
15:14:02 [INFO] vDC Cloning: Clone config file C:\Windows\NTDS\DCCloneConfig.xml is considered to be a blank file (containing 0 bytes)  
15:14:02 [INFO] vDC Cloning: Parsing clone config file C:\Windows\NTDS\DCCloneConfig.xml returned HRESULT 0x0  
```  
  
-   Validar que no hay ningún servicio ni programas instalados que no forman parte del DefaultDCCloneAllowList.xml o CustomDCCloneAllowList.xml  
  
```  
15:14:02 [INFO] vDC Cloning: Checking allowed list:  
15:14:03 [INFO] vDC Cloning: Completed checking allowed list:  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Habilitar DHCP en los adaptadores de red, ya que la información de IP no se ha especificado por el administrador  
  
```  
15:14:03 [INFO] vDC Cloning: Enable DHCP:  
15:14:03 [INFO] WMI Instance: Win32_NetworkAdapterConfiguration.Index=12  
15:14:03 [INFO] Method: EnableDHCP  
15:14:03 [INFO] HRESULT code: 0x0 (0)  
15:14:03 [INFO] Return Value: 0x0 (0)  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   Busque el emulador PDC  
  
-   Establecer el sitio de la copia (generado automáticamente en este caso)  
  
-   Establecer el nombre de la copia (generado automáticamente en este caso)  
  
```  
15:14:03 [INFO] vDC Cloning: Found PDC. Name: DC1.root.fabrikam.com  
15:14:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:04 [INFO] vDC Cloning: Winlogon UI Notification #1: Domain Controller cloning is at 5% completion...  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #2: Domain Controller cloning is at 10% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Site of the cloned DC: Default-First-Site-Name  
```  
  
-   Crear el nuevo objeto de equipo clone  
  
-   Cambiar el nombre de la copia para que coincida con el nuevo nombre  
  
```  
15:14:05 [INFO] vDC Cloning: Clone DC objects are created on PDC.  
15:14:05 [INFO] Name of the cloned DC: DC2-CL0001  
15:14:05 [INFO] DsRolepSetRegStringValue on System\CurrentControlSet\Services\NTDS\Parameters\CloneMachineName to DC2-CL0001 returned 0  
15:14:05 [INFO] vDC Cloning: Save CloneMachineName in registry: 0x0 (0)  
```  
  
-   Proporciona la configuración de promoción, en función de dccloneconfig.xml anterior o las reglas de generación automática  
  
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
  
-   Promoción de inicio  
  
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
  
-   Detener y configurar todos los servicios relacionados con el DS AD (NTDS, NTFRS/DFSR, KDC, DNS)  
  
> [!NOTE]  
> Se espera que el servicio DNS tarda mucho tiempo el apagado en este escenario, como lo usa zonas integradas de anuncios que ya no estaban disponibles incluso antes que el servicio NTDS detenido - ver los eventos DNS que se describe más adelante en esta sección del módulo.  
  
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
  
-   Forzar la sincronización de hora NT5DS (NTP) con otro controlador de dominio (normalmente la PDCE)  
  
```  
15:15:02 [INFO] Forcing time sync  
```  
  
-   Ponte en contacto con un controlador de dominio que contiene la cuenta de controlador de dominio de origen de la copia  
  
-   Vaciar los vales Kerberos existentes  
  
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
  
-   Detener el servicio de Net Logon y establecer su tipo de inicio  
  
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
  
-   Configurar los servicios DFSR/NTFRS para ejecutarse automáticamente  
  
-   Eliminar sus archivos de base de datos existente para forzar la sincronización no autorizado de SYSVOL cuando el servicio se inicia a continuación  
  
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
  
-   Iniciar el proceso de promoción con el archivo de base de datos NTDS existente  
  
-   Ponte en contacto con el maestro RID  
  
> [!NOTE]  
> El servicio de AD DS no se instala realmente aquí, lo cual es heredado instrumentación en el registro  
  
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
  
-   Cambia el identificador de invocación existente que existían en la base de datos de equipos de origen  
  
-   Crea un nuevo objeto de configuración NTDS para este clone  
  
-   Replicar en delta del objeto de AD desde el controlador de dominio de partners  
  
> [!NOTE]  
> Aunque todos los objetos se enumeran como replicar, esto es solo metadatos necesarios para incluir las actualizaciones. Todos los objetos sin cambios en la base de datos NTDS clonado ya existen y no requieren la replicación de nuevo, igual que con basado en IFM promoción.  
  
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
  
-   Rellenar las particiones GC según sea necesario con las actualizaciones que faltan  
  
-   Completar la parte crítica de AD DS de la promoción  
  
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
  
-   Completar la replicación de entrada de SYSVOL  
  
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
  
-   Habilitar el registro DNS de cliente  
  
```  
15:15:18 [INFO] vDC Cloning: Set DisableDynamicUpdate reg value to 0 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set UseDynamicDns reg value to 1 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set RegistrationEnabled reg value to 1 to enable dynamic update records registration.  
```  
  
-   Ejecutar los módulos SYSPREP especificados por el DefaultDCCloneAllowList.xml <SysprepInformation> elemento.  
  
```  
15:15:18 [INFO] vDC Cloning: Running sysprep providers.  
15:15:32 [INFO] vDC Cloning: Completed running sysprep providers.  
```  
  
-   Promoción de clonación es completa  
  
-   Quita la marca de arranque DSRM por lo que normalmente la próxima vez que inicia el servidor  
  
-   Cambiar el nombre de la dccloneconfig.xml para que no se leen nuevo en el siguiente arranque  
  
-   Reinicia el equipo  
  
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
  
##### <a name="active-directory-web-services-event-log"></a>Registro de eventos de servicios Web de Active Directory  
Mientras clonación se está produciendo, NTDS. Base de datos DIT suele sin conexión durante períodos prolongados. El servicio ADWS registra al menos un evento para esto. Una vez completada la clonación, se inicia el servicio ADWS, notas que hay aún no es un certificado de equipo válido (es posible o pero no puede ser, según el entorno de implementación de una PKI de Microsoft con la inscripción automática o no) y, a continuación, inicia la instancia del nuevo controlador de dominio.  
  
||||  
|-|-|-|  
|**Identificador de evento**|**Origen**|**Mensaje**|  
|**1202**|Eventos de instancia ADWS|Este equipo ahora hospeda la instancia de directorio especificado, pero no pudo mantenimiento servicios Web de Active Directory. Servicios Web de Active Directory intentará esta operación periódicamente.<br /><br />Instancia de directorio: NTDS<br /><br />La instancia de directorio puerto LDAP: 389<br /><br />La instancia de directorio puerto SSL: 636|  
|**1000**|Eventos de instancia ADWS|A partir de los servicios de Web de Active Directory|  
|**1008**|Eventos de instancia ADWS|Servicios Web de Active Directory se reducirá correctamente sus privilegios de seguridad|  
|**1100**|Eventos de instancia ADWS|Los valores especificados en la <appsettings> sección del archivo de configuración para los servicios de Active Directory Web se han cargado sin errores.|  
|**1400**|Eventos de instancia ADWS|Eventos de certificado ADWS "los servicios de Web de Active Directory no pudo encontrar un certificado de servidor con el nombre del certificado especificado. Un certificado es necesaria para usar conexiones SSL/TLS. Para usar conexiones SSL/TLS, comprobar que un certificado de autenticación de servidor válido de una entidad de confianza de certificación (CA) está instalado en el equipo.<br /><br />Nombre del certificado:*<Server FQDN>*|  
|**1100**|Eventos de instancia ADWS|Los valores especificados en la <appsettings> sección del archivo de configuración para los servicios de Active Directory Web se han cargado sin errores.|  
|**1200**|Eventos de instancia ADWS|Servicios de Active Directory Web ahora realiza el mantenimiento de la instancia de directorio especificado.<br /><br />Instancia de directorio: NTDS<br /><br />La instancia de directorio puerto LDAP: 389<br /><br />La instancia de directorio puerto SSL: 636|  
  
##### <a name="dns-server-event-log"></a>Registro de eventos de servidor DNS  
El servicio DNS experimentará breves interrupciones esperadas mientras se realiza la clonación, como el servicio DNS sigue en ejecución mientras la base de datos de AD DS está sin conexión. Esto ocurre si utiliza DNS integrado de Active Directory, pero no si usando estándar principal o secundario DNS. Estos errores iniciar sesión varias veces. Una vez completada la clonación, DNS vuelva normalmente a estar conectado.  
  
||||  
|-|-|-|  
|**Identificador de evento**|**Origen**|**Mensaje**|  
|**4013**|Servicio de servidor DNS|El servidor DNS está esperando a los servicios de dominio de Active Directory (AD DS) indicar que se ha completado la sincronización inicial del directorio. No se puede iniciar el servicio del servidor DNS hasta que se complete la sincronización inicial porque los datos de DNS críticas es posible que no todavía se replicar en este controlador de dominio. Si los eventos en el registro de eventos de AD DS indican que hay un problema con la resolución de nombres DNS, considera la posibilidad de agregar la dirección IP del otro servidor DNS para este dominio a la lista de servidores DNS en las propiedades de protocolo de Internet de este equipo. Este evento se registrará cada dos minutos hasta que se ha señalado AD DS que la sincronización inicial se ha completado correctamente.|  
|**4015**|Servicio de servidor DNS|El servidor DNS ha encontrado un error crítico de Active Directory. Compruebe que Active Directory está funcionando correctamente. La información de depuración de error extendida (que puede estar vacía) es "" ". Los datos del evento contienen el error.|  
|**4000**|Servicio de servidor DNS|El servidor DNS no pudo abrir Active Directory.  Este servidor DNS está configurado para obtener y usar la información del directorio de la zona y no puede cargar la zona sin ella.  Compruebe que Active Directory está funcionando correctamente y vuelve a cargar la zona. Los datos del evento están el código de error.|  
|**4013**|Servicio de servidor DNS|El servidor DNS está esperando a los servicios de dominio de Active Directory (AD DS) indicar que se ha completado la sincronización inicial del directorio. No se puede iniciar el servicio del servidor DNS hasta que se complete la sincronización inicial porque los datos de DNS críticas es posible que no todavía se replicar en este controlador de dominio. Si los eventos en el registro de eventos de AD DS indican que hay un problema con la resolución de nombres DNS, considera la posibilidad de agregar la dirección IP del otro servidor DNS para este dominio a la lista de servidores DNS en las propiedades de protocolo de Internet de este equipo. Este evento se registrará cada dos minutos hasta que se ha señalado AD DS que la sincronización inicial se ha completado correctamente.|  
|**2**|Servicio de servidor DNS|Se ha iniciado el servidor DNS.|  
|**4**|Servicio de servidor DNS|El servidor DNS ha terminado la carga en segundo plano de zonas. Todas las zonas ahora están disponibles para las actualizaciones de DNS y transferencias de zona, según lo permita la configuración de zona individual.|  
  
##### <a name="file-replication-service-event-log"></a>Registro de eventos de servicio de replicación de archivos  
El servicio de replicación de archivo se sincroniza no autorizada de un socio durante clonación. Clonación para ello, eliminar los archivos de base de datos NTFRS y dejando que el contenido de SYSVOL permanecen intactos, para usarlo como datos previamente inicializados. Se espera que los dos intentos para sincronizar.  
  
||||  
|-|-|-|  
|**Identificador de evento**|**Origen**|**Mensaje**|  
|**13562**|NtFrs|La siguiente es el resumen de las advertencias y errores encontrados por el servicio de replicación de archivos mientras sondeo la DC2.root.fabrikam.com del controlador de dominio para réplica FRS establece la información de configuración.<br /><br />No se puede enlazar a un controlador de dominio. Se vuelva a intentarlo más próximo ciclo de sondeo|  
|**13502**|NtFrs|Se detiene el servicio de replicación de archivos.|  
|**13565**|NtFrs|Servicio de replicación de archivo se está iniciando el volumen del sistema con los datos de otro controlador de dominio. DC2 del equipo no puede convertirse en un controlador de dominio hasta que este proceso se complete. A continuación, se compartirá el volumen del sistema como SYSVOL.<br /><br />Para comprobar si el recurso compartido SYSVOL, en el símbolo del sistema, escribe:<br /><br />recurso compartido de red<br /><br />Cuando el servicio de replicación de archivos se completa el proceso de inicialización, aparecerá el recurso compartido SYSVOL.<br /><br />La inicialización del volumen del sistema puede tardar algún tiempo. El tiempo depende de la cantidad de datos en el volumen del sistema, la disponibilidad de otros controladores de dominio y el intervalo de replicación entre controladores de dominio.|  
|**13501**|NtFrs|Se inicia el servicio de replicación de archivos|  
|**13502**|NtFrs|Se detiene el servicio de replicación de archivos.|  
|**13503**|NtFrs|El servicio de replicación de archivo se ha detenido.|  
|**13565**|NtFrs|Servicio de replicación de archivo se está iniciando el volumen del sistema con los datos de otro controlador de dominio. DC2 del equipo no puede convertirse en un controlador de dominio hasta que este proceso se complete. A continuación, se compartirá el volumen del sistema como SYSVOL.<br /><br />Para comprobar si el recurso compartido SYSVOL, en el símbolo del sistema, escribe:<br /><br />recurso compartido de red<br /><br />Cuando el servicio de replicación de archivos se completa el proceso de inicialización, aparecerá el recurso compartido SYSVOL.<br /><br />La inicialización del volumen del sistema puede tardar algún tiempo. El tiempo depende de la cantidad de datos en el volumen del sistema, la disponibilidad de otros controladores de dominio y el intervalo de replicación entre controladores de dominio.|  
|**13501**|NtFrs|El servicio de replicación de archivo se está iniciando.|  
|**13553**|NtFrs|El servicio de replicación de archivo, este equipo se agregó correctamente al siguiente conjunto de réplica:<br /><br />"DOMINIO VOLUMEN DEL SISTEMA (SYSVOL SHARE)"<br /><br />Información relacionada con este evento se muestra a continuación:<br /><br />Es el nombre de equipo DNS*<Domain Controller FQDN>*<br /><br />Nombre de miembro del conjunto de réplica*<Domain Controller>*<br /><br />Es la ruta de acceso raíz de réplica conjunto*<path>*<br /><br />Es la ruta de acceso de directorio provisional réplica*<path>*<br /><br />Es la ruta de acceso del directorio de trabajo de réplica*<path>*|  
|**13520**|NtFrs|El servicio de replicación de archivo mueve los archivos existentes en <path>a * <path> *\NtFrs_PreExisting___See_EventLog.<br /><br />El servicio de replicación de archivos puede eliminar los archivos de * <path> *\NtFrs_PreExisting___See_EventLog en cualquier momento. Archivos se pueden guardar de la eliminación copiándolos fuera de * <path> *\NtFrs_PreExisting___See_EventLog. Copiar los archivos en c:\windows\sysvol\domain puede causar conflictos de nombres si los archivos ya existen en otro asociado de replicación.<br /><br />En algunos casos, el servicio de replicación de archivos puede copiar un archivo de * <path> *\NtFrs_PreExisting___See_EventLog en * <path> * en lugar de duplicar el archivo de otro asociado de replicación.<br /><br />Se pueda recuperar espacio en cualquier momento mediante la eliminación de los archivos en * <path> *\NtFrs_PreExisting___See_EventLog. "|  
|**13508**|NtFrs|que el servicio de replicación de archivo tiene problemas para habilitar la replicación del * \\\\ <Domain Controller FQDN> * a * <Domain Controller> * para * <path> * mediante la<br /><br />DNS name *\\\\<Domain Controller FQDN>*. FRS seguirá intentándolo.<br /><br />Siguientes son algunas de las razones que verá esta advertencia.<br /><br />[1] FRS correctamente no puede resolver el nombre DNS * \\\\ <Domain Controller FQDN> * desde este equipo.<br /><br />[2] FRS no se está ejecutando * \\\\ <Domain Controller FQDN> *.<br /><br />[3] la información de la topología de los servicios de dominio de Active Directory para esta réplica no se replica aún en todos los controladores de dominio.<br /><br />Este mensaje de registro de eventos aparecerá una vez por cada conexión, después de que se resuelva el problema se verá otro mensaje de registro de eventos que indica que se haya establecido la conexión.|  
|**13509**|NtFrs|El servicio de replicación de archivos ha habilitado la replicación de * \\\\ <Domain Controller FQDN> * a * <Domain Controller> * para * <Path> * después de varios intentos repetidos.|  
|**13516**|NtFrs|El servicio de replicación de archivos ya no está impidiendo que el equipo * <Domain Controller> * se convierta en un controlador de dominio. El volumen del sistema se ha inicializado correctamente y ha notificado el servicio Netlogon que el volumen del sistema está preparado para compartido como SYSVOL.<br /><br />Tipo de "recurso compartido de red" para comprobar si el recurso compartido SYSVOL. "|  
  
##### <a name="dfs-replication-event-log"></a>Registro de eventos de replicación de DFS  
Los servicios DFSR sincroniza no autorizada de un socio durante clonación. Clonación para ello, eliminar los archivos de base de datos DFSR y dejando que el contenido de SYSVOL permanecen intactos, para usarlo como datos previamente inicializados. Se espera que los dos intentos para sincronizar.  
  
||||  
|-|-|-|  
|**Identificador de evento**|**Origen**|**Mensaje**|  
|**1004**|DFSR|Se ha iniciado el servicio de replicación de DFS.|  
|**1314**|DFSR|El servicio de replicación de DFS había configurado correctamente los archivos de registro de depuración.<br /><br />Información adicional:<br /><br />Ruta de acceso de archivo de registro de depuración: C:\Windows\debug|  
|**6102**|DFSR|El servicio de replicación de DFS registró correctamente el proveedor WMI|  
|**1206**|DFSR|El servicio de replicación de DFS correctamente contactar con el controlador de dominio DC2.corp.contoso.com para acceder a información de configuración.|  
|**1210**|DFSR|El servicio de replicación de DFS configurado correctamente una escucha de RPC para las solicitudes entrantes de replicación.<br /><br />Información adicional:<br /><br />Puerto: de 0"|  
|**4614**|DFSR|El servicio de replicación de DFS inicializa SYSVOL en la ruta de acceso local C:\Windows\SYSVOL\domain y se va a realizar la replicación inicial. La carpeta replicada permanecerá en el estado de sincronización inicial hasta que se replique con su partner. Si el servidor estaba promocionando a un controlador de dominio, el controlador de dominio no anunciar y funcionan como un controlador de dominio hasta que se resuelva este problema. Esto puede ocurrir si el partner especificado también está en el estado de la sincronización inicial, o si se encuentran infracciones de uso compartidas de este servidor o el asociado de sincronización. Si este evento se produjo durante la migración de SYSVOL desde el servicio de replicación de archivos (FRS) para la replicación de DFS, no se replicarán los cambios hasta que se resuelva este problema. Esto puede causar la carpeta SYSVOL en este servidor sea sincronizados con otros controladores de dominio.<br /><br />Información adicional:<br /><br />Nombre de carpeta duplicadas: Recurso compartido SYSVOL<br /><br />Identificador de carpeta duplicadas:*<GUID>*<br /><br />Nombre del grupo de replicación: Volumen del sistema de dominio<br /><br />Id. del grupo de replicación:*<GUID>*<br /><br />Id. de miembros:*<GUID>*<br /><br />Solo lectura: 0|  
|**4604**|DFSR|La carpeta SYSVOL replicado en la ruta de acceso local C:\Windows\SYSVOL\domain inicializó correctamente con el servicio de replicación de DFS. Este miembro ha completado la sincronización inicial de SYSVOL con dc1.corp.contoso.com del partner. Para comprobar la presencia del recurso compartido SYSVOL, abre una ventana del símbolo del sistema y, a continuación, escribe "" net share"".<br /><br />Información adicional:<br /><br />Nombre de carpeta duplicadas: Recurso compartido SYSVOL<br /><br />Identificador de carpeta duplicadas:*<GUID>*<br /><br />Nombre del grupo de replicación: Volumen del sistema de dominio<br /><br />Id. del grupo de replicación:*<GUID>*<br /><br />Id. de miembros:*<GUID>*<br /><br />Partner de sincronización:*<domain controller FQDN>*|  
  
## <a name="BKMK_TshootVDCSafeRestore"></a>Solución de problemas de restauración segura de controlador de dominio virtualizada  
  
### <a name="tools-for-troubleshooting"></a>Herramientas de solución de problemas  
  
#### <a name="logging-options"></a>Opciones de registro  
Los registros integrados son la herramienta más importante para solucionar problemas con la restauración de instantánea seguro del controlador de dominio. Todos estos registros se habilitado y configurados para el máximo nivel de detalle, de manera predeterminada.  
  
|||  
|-|-|  
|**Operación**|**Registro**|  
|**Creación de instantáneas**|-Evento viewer\Applications y servicios logs\Microsoft\Windows\Hyper-V-trabajo|  
|**Restauración de instantánea**|-Evento viewer\Applications y servicios logs\Directory servicio<br />-Evento viewer\Windows logs\System<br />-Evento viewer\Windows logs\Application<br />-Evento viewer\Applications y servicios logs\File servicio de replicación<br />-Evento viewer\Applications y servicios logs\DFS replicación<br />: Logs\DNS viewer\Applications y servicios de evento<br />-Evento viewer\Applications y servicios logs\Microsoft\Windows\Hyper-V-trabajo|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Herramientas y los comandos para solucionar problemas de configuración del controlador de dominio  
Para solucionar problemas que no se explica por los registros, usa las siguientes herramientas como punto de partida:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   3.4 del Monitor de red  
  
#### <a name="BKMK_TshhotSafeRestore"></a>Metodología general para solucionar problemas de restauración segura del controlador de dominio  
  
1.  ¿Se espera la restauración de una instantánea seguro, pero experimenta problemas?  
  
    1.  Examina el registro de eventos de servicios de directorio  
  
        1.  ¿Hay errores de restauración de instantánea?  
  
        2.  ¿Hay errores de replicación de AD?  
  
    2.  Examina el registro de eventos del sistema  
  
        1.  ¿Hay errores de comunicación?  
  
        2.  ¿Hay errores de anuncios?  
  
2.  ¿Es la restauración de una instantánea seguro inesperados?  
  
    1.  Examina los registros de auditoría de hipervisor para determinar quién o lo que causa una operación de deshacer  
  
    2.  Ponte en contacto con todos los administradores del hipervisor y consultar ellos sobre quién revierten la máquina virtual sin notificación  
  
3.  ¿Es la protección de reversión de USN implementación de servidor de seguridad y restauración no de forma segura?  
  
    1.  Examina el registro de eventos de servicios de directorio para una no admitido hipervisor o integración de servicios  
  
    2.  Examinar el sistema operativo y validar que ejecute Windows Server 2012?  
  
### <a name="BKMK_TshootSpecificSafeRestore"></a>Solucionar problemas específicos  
  
#### <a name="events"></a>Eventos  
Todos virtualizan restauración de instantánea seguro de controlador de dominio eventos escriben en el registro de eventos de servicios de directorio del controlador de dominio restaurado máquina virtual. Los registros de eventos de la aplicación, sistema, el servicio de replicación de archivos y la replicación DFS también puede contener información de solución de problemas útil para la restauración error.  
  
A continuación son los eventos específicos de la restauración de Windows Server 2012 seguros en el registro de eventos de servicios de directorio.  
  
|||  
|-|-|  
|**Identificador de evento**|**2170**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Advertencia|  
|**Mensaje**|Se ha detectado un cambio de identificador de generación.<br /><br />Id. de generación que se almacenan en caché en DS (valor antiguo): %1<br /><br />Identificador de generación actualmente en la máquina virtual (nuevo valor): %2<br /><br />Después de la aplicación de una instantánea de la máquina virtual, después de una operación de importación de máquina virtual o una operación de migración en vivo, se produce el cambio de identificador de generación. *<COMPUTERNAME>*se crea un nuevo identificador de invocación para recuperar el controlador de dominio. Controladores de dominio virtualizados no deben restaurarse mediante instantáneas de máquina virtual. El método admitido para restaurar o reversión el contenido de una base de datos de los servicios de dominio de Active Directory es restaurar una copia de seguridad realizada con una aplicación de copia de seguridad compatible con los servicios de dominio de Active Directory.|  
|**Notas y resolución**|Esto es un evento de éxito si se esperaba la instantánea. Si no es así, examina el registro de eventos de Hyper-V-trabajo o ponte en contacto con el Administrador de hipervisor.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2174**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|El controlador de dominio no es un clone de controlador de dominio virtual ni una instantánea del controlador de dominio virtual restaurados.|  
|**Notas y resolución**|Evento esperado al iniciar controladores de dominio físico o controladores de dominio virtualizados no se restauran desde instantánea|  
  
|||  
|-|-|  
|**Identificador de evento**|**2181**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|La transacción se anuló debido a la máquina virtual que se ha vuelto a un estado anterior.  Esto ocurre después de la aplicación de una instantánea de la máquina virtual, después de una operación de importación de máquina virtual o después de una operación de migración en vivo.|  
|**Notas y resolución**|Espera al restaurar una instantánea. Realizar el seguimiento de las transacciones el cambio de Id. de generación de máquina virtual|  
  
|||  
|-|-|  
|**Identificador de evento**|**2185**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|*<COMPUTERNAME>*detiene el servicio de FRS o DFSR utilizado para replicar la carpeta SYSVOL.<br /><br />Nombre de servicio: % 1<br /><br />Active Directory detecta que la máquina virtual que hospeda el controlador de dominio se ha vuelto a un estado anterior. *<COMPUTERNAME>*debe inicializar una restauración no autorizados en la réplica SYSVOL local. Esto se realiza mediante la detención del servicio FRS o DFSR se utiliza para replicar la carpeta SYSVOL y a partir de las claves de registro adecuadas y valores para desencadenar la restauración. Cuando se reinicie el servicio FRS o DFSR, se registrará eventos 2187.|  
|**Notas y resolución**|Espera al restaurar una instantánea. Todos los datos SYSVOL en este controlador de dominio se reemplaza con copia de un partner del controlador de dominio.|  
|**Identificador de evento**|2186|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|*<COMPUTERNAME>*Error al detener el servicio de FRS o DFSR utilizado para replicar la carpeta SYSVOL.<br /><br />Nombre de servicio: % 1<br /><br />Código de error: % 2<br /><br />Mensaje de error: % 3<br /><br />Active Directory detecta que la máquina virtual que hospeda el controlador de dominio se ha vuelto a un estado anterior. *<COMPUTERNAME>*debe inicializar una restauración no autorizados en la réplica SYSVOL local. Esto se hace detener el servicio de replicación FRS o DFSR usado para replicar la carpeta SYSVOL y, a continuación, volver a iniciarlo con las claves de registro adecuadas y valores para desencadenar la restauración. *<COMPUTERNAME>*no se pudo detener el servicio en ejecución actual y no puede completar la restauración no autorizado. Realice una restauración no autorizado manualmente.|  
|**Notas y resolución**|Examina los registros de eventos del sistema, FRS y DFSR para obtener más información.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2187**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|*<COMPUTERNAME>*iniciar el servicio de FRS o DFSR utilizado para replicar la carpeta SYSVOL.<br /><br />Nombre de servicio: % 1<br /><br />Active Directory detecta que la máquina virtual que hospeda el controlador de dominio se ha vuelto a un estado anterior. *<COMPUTERNAME>*necesario para inicializar una restauración no autorizados en la réplica SYSVOL local. Esto se realiza al detener el servicio FRS o DFSR se utiliza para replicar la carpeta SYSVOL y a partir de las claves de registro adecuadas y valores para desencadenar la restauración.|  
|**Notas y resolución**|Espera al restaurar una instantánea. Todos los datos SYSVOL en este controlador de dominio se reemplaza con copia de un partner del controlador de dominio.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2188**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|*<COMPUTERNAME>*Error al iniciar el servicio de FRS o DFSR utilizado para replicar la carpeta SYSVOL.<br /><br />Nombre de servicio: % 1<br /><br />Código de error: % 2<br /><br />Mensaje de error: % 3<br /><br />Active Directory detecta que la máquina virtual que hospeda el controlador de dominio se ha vuelto a un estado anterior. *<COMPUTERNAME>*debe inicializar una restauración no autorizados en la réplica SYSVOL local. Para ello, detener el servicio FRS o DFSR se utiliza para replicar SYSVOL e iniciar con claves de registro apropiadas y valores para desencadenar la restauración. *<COMPUTERNAME>*no se pudo iniciar el servicio de FRS o DFSR utilizado para replicar la carpeta SYSVOL y no puede completar la restauración no autorizado. Por favor, realizar una restauración no autorizado manualmente y reiniciar el servicio.|  
|**Notas y resolución**|Examina los registros de eventos del sistema, FRS y DFSR para obtener más información.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2189**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|*<COMPUTERNAME>*establecer los siguientes valores del registro para inicializar réplica SYSVOL durante la restauración no autorizado:<br /><br />Clave del registro: % 1<br /><br />Valor del registro: %2<br /><br />Información del valor del registro: %3<br /><br />Active Directory detecta que la máquina virtual que hospeda el controlador de dominio se ha vuelto a un estado anterior. *<COMPUTERNAME>*debe inicializar una restauración no autorizados en la réplica SYSVOL local. Para ello, usa el servicio FRS o DFSR para replicar la carpeta SYSVOL de detener e iniciar con las claves de registro adecuadas y valores para desencadenar la restauración.|  
|**Notas y resolución**|Espera al restaurar una instantánea. Todos los datos SYSVOL en este controlador de dominio se reemplaza con copia de un partner del controlador de dominio.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2190**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|*<COMPUTERNAME>*no se pudo establecer los siguientes valores del registro para inicializar la réplica SYSVOL durante la restauración no autorizado:<br /><br />Clave del registro: % 1<br /><br />Valor del registro: %2<br /><br />Información del valor del registro: %3<br /><br />Código de error: % 4<br /><br />Mensaje de error: % 5<br /><br />Active Directory detecta que la máquina virtual que hospeda la función de controlador de dominio se ha vuelto a un estado anterior. *<COMPUTERNAME>*debe inicializar una restauración no autorizados en la réplica SYSVOL local. Para ello, usa el servicio FRS o DFSR para replicar la carpeta SYSVOL de detener e iniciar con las claves de registro adecuadas y valores para desencadenar la restauración. *<COMPUTERNAME>*no se pudo establecer los valores del registro anterior y no puede completar la restauración no autorizado. Realice una restauración no autorizado manualmente.|  
|**Notas y resolución**|Examina los registros de eventos de aplicación y del sistema. Investiga las aplicaciones de terceros que pueden estar bloqueando las actualizaciones del registro.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2200**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|Active Directory detecta que la máquina virtual que hospeda el controlador de dominio se ha vuelto a un estado anterior. *<COMPUTERNAME>*Inicializa la replicación para que aparezca el controlador de dominio actual. Cuando finalice la replicación, se registrará eventos 2201.|  
|**Notas y resolución**|Espera al restaurar una instantánea. Marca el comienzo de replicación de AD entrante.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2201**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|Active Directory detecta que la máquina virtual que hospeda el controlador de dominio se ha vuelto a un estado anterior. *<COMPUTERNAME>*ha finalizado la replicación para que aparezca el controlador de dominio actual.|  
|**Notas y resolución**|Espera al restaurar una instantánea. Marca el final de replicación de AD entrante.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2202**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|Active Directory detecta que la máquina virtual que hospeda el controlador de dominio se ha vuelto a un estado anterior. *<COMPUTERNAME>*Error replicación para que aparezca el controlador de dominio actualizado. Después de la siguiente replicación periódica, se actualizará el controlador de dominio.|  
|**Notas y resolución**|Examina los registros de eventos del sistema y los servicios de directorio. Usar repadmin.exe intente forzar la replicación y ten en cuenta los posibles errores.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2204**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|*<COMPUTERNAME>*ha detectado un cambio de identificador de generación de máquina virtual. El cambio significa que el controlador de dominio virtual ha recuperado a un estado anterior. *<COMPUTERNAME>*realizará las siguientes operaciones para proteger el controlador de dominio revertir contra divergencia datos posibles y para proteger la creación de entidades de seguridad con el SID duplicado de:<br /><br />Crear un nuevo identificador de invocación<br /><br />Invalidar el grupo RID actual<br /><br />Propiedad de las funciones FSMO se validará en siguiente replicación entrante. Durante esta ventana si el controlador de dominio mantiene una función FSMO, esa función no estará disponible.<br /><br />Iniciar la operación de restauración de servicio de replicación de SYSVOL.<br /><br />Iniciar la replicación para que aparezca el controlador de dominio revertir al estado más reciente.<br /><br />Solicitar un nuevo conjunto de RID.|  
|**Notas y resolución**|Espera al restaurar una instantánea. Esto se explica que las distintas restablecer las operaciones que surja como parte del proceso de restauración seguro.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2205**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|*<COMPUTERNAME>*invalida el grupo RID actual después de controlador de dominio virtual se revierte a un estado anterior.|  
|**Notas y resolución**|Espera al restaurar una instantánea. El conjunto de RID local debe destruirse como el controlador de dominio tiene tiempo recorrida y pueden ya han sido emitidos.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2206**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|ERROR|  
|**Mensaje**|*<COMPUTERNAME>*Error al invalidar el grupo RID actual después de controlador de dominio virtual se revierte a un estado anterior.<br /><br />Datos adicionales:<br /><br />Código de error: %1<br /><br />Valor de error: %2|  
|**Notas y resolución**|Examina los registros de eventos del sistema y los servicios de directorio. Validar que el maestro LIBRARSE está en línea se puede acceder desde este servidor mediante Dcdiag.exe /test:ridmanager|  
  
|||  
|-|-|  
|**Identificador de evento**|**2207**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|ERROR|  
|**Mensaje**|*<COMPUTERNAME>*se ha podido restaurar después de controlador de dominio virtual se revierte a un estado anterior. Se solicitó un reinicio DSRM. Ponte en contacto eventos anteriores para obtener más información.|  
|**Notas y resolución**|Examina los registros de eventos del sistema y los servicios de directorio.|  
  
|||  
|-|-|  
|**Identificador de evento**|**2208**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Informativo|  
|**Mensaje**|*<COMPUTERNAME>*eliminar bases de datos DFSR para inicializar réplica SYSVOL durante la restauración no autorizado.|  
|**Notas y resolución**|Espera al restaurar una instantánea. Esto garantiza que DFSR no autorizada sincroniza SYSVOL desde un controlador de dominio asociado. Ten en cuenta que cualquier otro DFSR carpetas replicadas en el mismo volumen que SYSVOL se sincronizarán también no autorizada (controladores no se recomiendan host personalizado que DFSR se establece en el mismo volumen que SYSVOL del dominio).|  
  
|||  
|-|-|  
|**Identificador de evento**|**2209**|  
|**Origen**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**Gravedad**|Error|  
|**Mensaje**|*<COMPUTERNAME>*no se pudo eliminar DFSR bases de datos.<br /><br />Datos adicionales:<br /><br />Código de error: %1<br /><br />Valor de error: %2<br /><br />Active Directory detecta que la máquina virtual que hospeda el controlador de dominio se ha vuelto a un estado anterior. *<COMPUTERNAME>*debe inicializar una restauración no autorizados en la réplica SYSVOL local. DFSR, esto se hace mediante detener el servicio DFSR, eliminar DFSR bases de datos y volver a iniciar el servicio. Al reiniciar DFSR se vuelva a generar las bases de datos y empieza a la sincronización inicial.|  
|**Notas y resolución**|Examina el registro de eventos DFSR.|  
  
#### <a name="error-messages"></a>Mensajes de error  
No existen errores interactivos directas para que no se pudo realizar la restauración de instantánea seguro de controlador de dominio virtualizada; información de clonación todos los registros en los registros de eventos de servicios de directorio. Naturalmente, cualquier errores graves de publicidad de replicación o servidor manifiestan como síntomas en otro lugar.  
  
#### <a name="known-issues-and-support-scenarios"></a>Problemas conocidos y escenarios de soporte técnico  
La [metodología General para la solución de problemas de dominio controlador seguro restaurar](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootSpecificSafeRestore) suelen ser adecuadas solucionar la mayoría de los problemas.  
  
|||  
|-|-|  
|**Problema**|**No se puede crear a nuevos principales de seguridad en el controlador de dominio restaurado recientemente seguro**|  
|**Síntomas**|Después de restaurar una instantánea, intenta crear una nueva entidad de seguridad de (usuario, equipo, el grupo) en dicho error de controlador de dominio con:<br /><br />Error 0x2010<br /><br />El servicio de directorio no pudo asignar un identificador relativo.|  
|**Resolución y notas**|Este problema está causado por conocimiento obsoletos restaurado del equipo de la función eliminar maestro FSMO. Si la función se mueve al controlador de dominio de este u otro después de tomar una instantánea y luego se restaura, el controlador de dominio restaurado no tendrán conocimiento del patrón RID hasta que se haya completado la replicación inicial.<br /><br />Para resolver el problema, permitir la replicación de AD completar entrante para el controlador de dominio restaurados. Si sigue sin funcionar, validar que todos los controladores de dominio tienen el mismo conocimiento correcto de los cuales DC aloja el maestro de eliminar.|  
  
|||  
|-|-|  
|**Problema**|**Controladores de dominio restaurado no compartir SYSVOL, anunciar**|  
|**Síntomas**|No anuncies después de restaurar una instantánea, uno o varios controladores de dominio, no comparten sysvol y no tienen contenido SYSVOL al día|  
|**Resolución y notas**|Dirección ascendentes asociados de los controladores de dominio no tienen una réplica SYSVOL de trabajo que se replique correctamente con DFSR o FRS. Este problema está relacionado con la restauración segura, pero es probable que manifestarse como un problema de restauración segura, ya que el cliente estaba al tanto del otro problema de replicación que esto afecte a los controladores de dominio no van a restaurar|  
  
### <a name="advanced-troubleshooting"></a>Solución avanzada de problemas  
Este módulo intenta enseñar la solución avanzada de problemas mediante el uso de *trabajar* registros como muestras, con una explicación de lo que ha ocurrido. Si conoces el aspecto de una operación de controlador de dominio virtualizada correcta, errores sean obvias en su entorno. Estos registros se presentan por su origen, con el orden ascendente de *espera* eventos relacionados con un controlador de dominio clonados dentro de cada registro.  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-dfsr"></a>Restauración de un controlador de dominio que se replica SYSVOL mediante DFSR  
  
##### <a name="directory-services-event-log"></a>Registro de eventos de servicios de directorio  
El registro de servicios de directorio contiene la mayoría de obtener información operativa restauración segura. El hipervisor cambia el identificador de generación de la máquina virtual y notas el servicio NTDS, invalida el conjunto de RID y cambia el identificador de invocación. El nuevo ID de generación de VM es conjunto y la entrada de datos de anuncios se replica servidores. Se detiene el servicio DFSR y se elimina en su base de datos que hospeda SYSVOL, si la fuerzas una sincronización no autorizado entrante. Se ajusta el límite máximo de USN.  
  
||||  
|-|-|-|  
|**Identificador de evento**|**Origen**|**Mensaje**|  
|**2170**|ActiveDirectory_DomainService|Se ha detectado un cambio de identificador de generación.<br /><br />Id. de generación que se almacenan en caché en DS (valor antiguo):<br /><br />*<number>*<br /><br />Id. de generación actualmente en la máquina virtual (nuevo valor):<br /><br />*<number>*<br /><br />Después de la aplicación de una instantánea de la máquina virtual, después de una operación de importación de máquina virtual o una operación de migración en vivo, se produce el cambio de identificador de generación. Los servicios de dominio de Active Directory creará un nuevo identificador de invocación para recuperar el controlador de dominio. Controladores de dominio virtualizados no deben restaurarse mediante instantáneas de máquina virtual. El método admitido para restaurar o reversión el contenido de una base de datos de los servicios de dominio de Active Directory es restaurar una copia de seguridad realizada con una aplicación de copia de seguridad de los servicios de dominio de Active Directory compatible".|  
|**2181**|ActiveDirectory_DomainService|La transacción se anuló debido a la máquina virtual que se ha vuelto a un estado anterior.  Esto ocurre después de la aplicación de una instantánea de la máquina virtual, después de una operación de importación de máquina virtual o después de una operación de migración en vivo.|  
|**2204**|ActiveDirectory_DomainService|Los servicios de dominio de Active Directory ha detectado un cambio de identificador de generación de máquina virtual. El cambio significa que el controlador de dominio virtual ha recuperado a un estado anterior. Los servicios de dominio de Active Directory realizará las siguientes operaciones para proteger el controlador de dominio revertir contra divergencia datos posibles y para proteger la creación de entidades de seguridad con el SID duplicado de:<br /><br />Crear un nuevo identificador de invocación<br /><br />Invalidar el grupo RID actual<br /><br />Propiedad de las funciones FSMO se validará en siguiente replicación entrante. Durante esta ventana si el controlador de dominio mantiene una función FSMO, esa función no estará disponible.<br /><br />Iniciar la operación de restauración de servicio de replicación de SYSVOL.<br /><br />Iniciar la replicación para que aparezca el controlador de dominio revertir al estado más reciente.<br /><br />Solicitar un nuevo grupo RID".|  
|**2181**|ActiveDirectory_DomainService|La transacción se anuló debido a la máquina virtual que se ha vuelto a un estado anterior.  Esto ocurre después de la aplicación de una instantánea de la máquina virtual, después de una operación de importación de máquina virtual o después de una operación de migración en vivo.|  
|**1109**|ActiveDirectory_DomainService|Se cambió el atributo de Id. de invocación de este servidor de directorio. El mayor número de secuencia de actualización en el momento en que se creó la copia de seguridad es la siguiente:<br /><br />Atributo de Id. de invocación (valor antiguo):<br /><br />*<GUID>*<br /><br />Atributo de Id. de invocación (nuevo valor):<br /><br />*<GUID>*<br /><br />Número de secuencia de actualización:<br /><br />*<number>*<br /><br />Ha cambiado el Id. de invocación cuando un servidor de directorio se restaura desde un medio de copia de seguridad, está configurado para hospedar una partición de directorio de escritura de aplicación, se ha reanudado después de aplicar una instantánea de la máquina virtual, después de una operación de importación de máquina virtual o una operación de migración en vivo. Controladores de dominio virtualizados no deben restaurarse mediante instantáneas de máquina virtual. El método admitido para restaurar o reversión el contenido de una base de datos de los servicios de dominio de Active Directory es restaurar una copia de seguridad realizada con una aplicación compatible con servicios de dominio de Active Directory de copia de seguridad."|  
|**2179**|ActiveDirectory_DomainService|Se ha establecido el atributo msDS GenerationId del controlador de dominio objeto de equipo en el parámetro siguiente:<br /><br />Atributo GenerationID:<br /><br />*<number>*|  
|**2200**|ActiveDirectory_DomainService|Active Directory detecta que la máquina virtual que hospeda el controlador de dominio se ha vuelto a un estado anterior. Los servicios de dominio de Active Directory inicializa replicación para que aparezca el controlador de dominio actual. Cuando finalice la replicación, se registrará eventos 2201.|  
|**2201**|ActiveDirectory_DomainService|Active Directory detecta que la máquina virtual que hospeda el controlador de dominio se ha vuelto a un estado anterior. Los servicios de dominio de Active Directory ha terminado de replicación para que aparezca el controlador de dominio actual.|  
|**2185**|ActiveDirectory_DomainService|Los servicios de dominio de Active Directory detiene el servicio de FRS o DFSR utilizado para replicar la carpeta SYSVOL.<br /><br />Nombre de servicio:<br /><br />DFSR<br /><br />Active Directory detecta que la máquina virtual que hospeda el controlador de dominio se ha vuelto a un estado anterior. Los servicios de dominio de Active Directory debe inicializar una restauración no autorizados en la réplica SYSVOL local. Esto se realiza mediante la detención del servicio FRS o DFSR se utiliza para replicar la carpeta SYSVOL y a partir de las claves de registro adecuadas y valores para desencadenar la restauración. Evento 2187 se registrará cuando se reinicie el servicio FRS o DFSR."|  
|**2208**|ActiveDirectory_DomainService|Los servicios de dominio de Active Directory, se eliminan las bases de datos DFSR para inicializar la réplica SYSVOL durante la restauración no autorizado.<br /><br />Active Directory detecta que la máquina virtual que hospeda el controlador de dominio se ha vuelto a un estado anterior. Los servicios de dominio de Active Directory debe inicializar una restauración no autorizados en la réplica SYSVOL local. DFSR, esto se hace mediante detener el servicio DFSR, eliminar DFSR bases de datos y volver a iniciar el servicio. Al reiniciar DFSR se vuelva a generar las bases de datos y empieza a la sincronización inicial. "|  
|**2187**|ActiveDirectory_DomainService|Los servicios de dominio de Active Directory inicia el servicio de FRS o DFSR utilizado para replicar la carpeta SYSVOL.<br /><br />Nombre de servicio:<br /><br />DFSR<br /><br />Active Directory detecta que la máquina virtual que hospeda el controlador de dominio se ha vuelto a un estado anterior. Los servicios de dominio de Active Directory necesario para inicializar una restauración no autorizados en la réplica SYSVOL local. Esto se realiza al detener el servicio FRS o DFSR se utiliza para replicar la carpeta SYSVOL y a partir de las claves de registro adecuadas y valores para desencadenar la restauración. "|  
|**1587**|ActiveDirectory_DomainService|Este servicio de directorio se ha restaurado o se ha configurado para hospedar una partición de directorio de la aplicación. Como resultado, ha cambiado su identidad de replicación. Un asociado ha solicitado cambios de replicación mediante nuestra antigua identidad. Se ajustó el número de secuencia de inicio.<br /><br />El servicio de directorio de destino correspondiente al objeto siguiente que GUID ha solicitado cambios empezando por un USN que precede a la USN a la que se ha restaurado el servicio de directorio local desde un medio de copia de seguridad.<br /><br />GUID de objeto:<br /><br />*<GUID> (<FQDN of partner domain controller>)*<br /><br />USN en el momento de restauración:<br /><br />*<number>*<br /><br />Como resultado, el vector de actualización del servicio de directorio de destino se ha configurado con la siguiente configuración.<br /><br />Base de datos anterior GUID:<br /><br />*<GUID>*<br /><br />Objeto anterior USN:<br /><br />*<number>*<br /><br />Propiedad anterior USN:<br /><br />*<number>*<br /><br />Nueva base de datos GUID:<br /><br />*<GUID>*<br /><br />Nuevo objeto USN:<br /><br />*<number>*<br /><br />Nueva propiedad USN:<br /><br />*<number>*|  
  
##### <a name="system-event-log"></a>Registro de eventos del sistema  
El registro de eventos de sistema notas el tiempo de máquina que se produce cuando llevar una máquinas virtuales sin conexión en línea y sincronizar con el tiempo del host. El conjunto de RID invalida y los servicios DFSR o FRS se reinician.  
  
||||  
|-|-|-|  
|**Identificador de evento**|**Origen**|**Mensaje**|  
|**1**|Kernel General|¿La hora del sistema haya cambiado a *?<now>* desde *< fecha y hora de instantáneas >*.<br /><br />Motivo del cambio: Una aplicación o un componente de sistema cambia a la vez.|  
|**16654**|Servicios de directorio de SAM|Se ha invalidado un conjunto de identificadores de cuenta (RID). Esto puede ocurrir en los siguientes casos esperados:<br /><br />1. un controlador de dominio se restaura desde la copia de seguridad.<br /><br />2. un controlador de dominio que se ejecuta en una máquina virtual se restaura desde la instantánea.<br /><br />3. un administrador ha invalidado manualmente el grupo.<br /><br />Consulta https://go.microsoft.com/fwlink/?LinkId=226247 para obtener más información.|  
|**7036**|Administrador de Control de servicio|El servicio de replicación DFS entró en estado de parada.|  
|**7036**|Administrador de Control de servicio|El servicio de replicación DFS entró en estado de ejecución.|  
  
##### <a name="application-event-log"></a>Registro de eventos de la aplicación  
El registro de eventos de la aplicación notas de la base de datos DFSR detener e iniciar.  
  
||||  
|-|-|-|  
|**Identificador de evento**|**Origen**|**Mensaje**|  
|**103**|ESSENT|DFSRs (1360) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db: el motor de base de datos ha detenido la instancia (0).<br /><br />Cierre incorrecto: 0<br /><br />Secuencia de intervalo de tiempo interno: 0,000 [1], 0,000 [2], 0,000 [3], 0,000 [4], 0.141 [5], 0,000 [6], 0,000 [7], [8] 0,000, 0,000 [9], 0,000 [10], 0.016 [11], [12] 0,000, 0,000 [13], [14] 0,000, 0,000 [15].|  
|**102**|ESSENT|DFSRs (532) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db: el motor de base de datos (6.02.8189.0000) está iniciando una nueva instancia (0).|  
|**105**|ESSENT|DFSRs (532) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db: el motor de base de datos inició una nueva instancia (0). (Tiempo = 0 segundos)<br /><br />Secuencia de intervalo de tiempo interno: 0,000 [1], 0,000 [2], [3] 0,000, 0,000 [4], 0,000 [5], 0,000 [6], [7] 0,000, 0,000 [8], [9] 0.031, 0,000 [10], 0,000 [11].|  
|||DFSRs (532) \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db: el motor de base de datos crea una nueva base de datos (1, \\\.\C:\System Volume Information\DFSR\database*_<GUID>*\dfsr.db). (Tiempo = 0 segundos)<br /><br />Secuencia de intervalo de tiempo interno: 0,000 [1], 0,000 [2], [3] 0.016, 0.062 [4], 0,000 [5], [6] 0.016, 0,000 [7], [8] 0,000, 0,015 [9], 0,000 [10], 0,000 [11].|  
  
##### <a name="dfs-replication-event-log"></a>Registro de eventos de replicación de DFS  
El servicio DFSR se detiene y se elimina la base de datos que contiene SYSVOL, forzar la sincronización no autorizado entrante.  
  
||||  
|-|-|-|  
|**Identificador de evento**|**Origen**|**Mensaje**|  
|**1006**|DFSR|Se detiene el servicio de replicación de DFS.|  
|**1008**|DFSR|El servicio de replicación DFS ha detenido.|  
|**1002**|DFSR|Se está iniciando el servicio de replicación de DFS.|  
|**1004**|DFSR|Se ha iniciado el servicio de replicación de DFS.|  
|**1314**|DFSR|El servicio de replicación de DFS había configurado correctamente los archivos de registro de depuración.<br /><br />Información adicional:<br /><br />Ruta de acceso de archivo de registro de depuración: C:\Windows\debug|  
|**6102**|DFSR|El servicio de replicación de DFS registró correctamente el proveedor WMI.|  
|**1206**|DFSR|El controlador de dominio correctamente contactado del servicio de replicación DFS * <domain controller FQDN> * acceso a la información de configuración.|  
|**1210**|DFSR|El servicio de replicación de DFS configurado correctamente una escucha de RPC para las solicitudes entrantes de replicación.<br /><br />Información adicional:<br /><br />Puerto: 0|  
|**4614**|DFSR|El servicio de replicación de DFS inicializa SYSVOL en la ruta de acceso local C:\Windows\SYSVOL\domain y se va a realizar la replicación inicial. La carpeta replicada permanecerá en el estado de sincronización inicial hasta que se replique con su partner. Si el servidor estaba promocionando a un controlador de dominio, el controlador de dominio no anunciar y funcionan como un controlador de dominio hasta que se resuelva este problema. Esto puede ocurrir si el partner especificado también está en el estado de la sincronización inicial, o si se encuentran infracciones de uso compartidas de este servidor o el asociado de sincronización. Si este evento se produjo durante la migración de SYSVOL desde el servicio de replicación de archivos (FRS) para la replicación de DFS, no se replicarán los cambios hasta que se resuelva este problema. Esto puede causar la carpeta SYSVOL en este servidor sea sincronizados con otros controladores de dominio.<br /><br />Información adicional:<br /><br />Nombre de carpeta duplicadas: Recurso compartido SYSVOL<br /><br />Identificador de carpeta duplicadas:*<GUID>*<br /><br />Nombre del grupo de replicación: Volumen del sistema de dominio<br /><br />Id. del grupo de replicación:*<GUID>*<br /><br />Id. de miembros:*<GUID>*<br /><br />Solo lectura: 0|  
|**4604**|DFSR|La carpeta SYSVOL replicado en la ruta de acceso local C:\Windows\SYSVOL\domain inicializó correctamente con el servicio de replicación de DFS. Este miembro ha completado la sincronización inicial de SYSVOL con dc1.corp.contoso.com del partner. Para comprobar la presencia del recurso compartido SYSVOL, abre una ventana del símbolo del sistema y, a continuación, escribe "recurso compartido de red".<br /><br />Información adicional:<br /><br />Nombre de carpeta duplicadas: Recurso compartido SYSVOL<br /><br />Identificador de carpeta duplicadas:*<GUID>*<br /><br />Nombre del grupo de replicación: Volumen del sistema de dominio<br /><br />Id. del grupo de replicación:*<GUID>*<br /><br />Id. de miembros:*<GUID>*<br /><br />Partner de sincronización:*<partner domain controller FQDN>*|  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-frs"></a>Restauración de un controlador de dominio que se replica SYSVOL mediante FRS  
El registro de eventos de replicación de archivo se usa en lugar del registro de eventos DFSR en este caso. El registro de eventos de la aplicación también escribe eventos relacionados con la FRS diferentes. De lo contrario, los servicios de directorio y registro de eventos del sistema de mensajes por lo general, son los mismos y en el mismo orden tal y como se describen.  
  
##### <a name="file-replication-service-event-log"></a>Registro de eventos de servicio de replicación de archivos  
El servicio FRS se detiene y se reinicia con un valor D2 BURFLAGS no autorizada sincronizar SYSVOL.  
  
||||  
|-|-|-|  
|**Identificador de evento**|**Origen**|**Mensaje**|  
|**13502**|NTFRS|Se detiene el servicio de replicación de archivos.|  
|**13503**|NTFRS|El servicio de replicación de archivo se ha detenido.|  
|**13501**|NTFRS|Se inicia el servicio de replicación de archivos|  
|**13512**|NTFRS|El servicio de replicación de archivos ha detectado una caché de escritura del disco habilitada en la unidad que contiene la c:\windows\ntfrs\jet directorio en el equipo DC4. No es posible que recupere el servicio de replicación de archivos cuando se interrumpe la alimentación de la unidad y se pierdan actualizaciones críticas.|  
|**13565**|NTFRS|Servicio de replicación de archivo se está iniciando el volumen del sistema con los datos de otro controlador de dominio. Equipo DC4 no puede convertirse en un controlador de dominio hasta que este proceso se complete. A continuación, se compartirá el volumen del sistema como SYSVOL.<br /><br />Para comprobar si el recurso compartido SYSVOL, en el símbolo del sistema, escribe:<br /><br />recurso compartido de red<br /><br />Cuando el servicio de replicación de archivos se completa el proceso de inicialización, aparecerá el recurso compartido SYSVOL.<br /><br />La inicialización del volumen del sistema puede tardar algún tiempo. El tiempo depende de la cantidad de datos en el volumen del sistema, la disponibilidad de otros controladores de dominio y el intervalo de replicación entre controladores de dominio."|  
|**13520**|NTFRS|El servicio de replicación de archivo mueve los archivos existentes en * <path> * a * <path> *\NtFrs_PreExisting___See_EventLog.<br /><br />El servicio de replicación de archivos puede eliminar los archivos de * <path> *\NtFrs_PreExisting___See_EventLog en cualquier momento. Archivos se pueden guardar de la eliminación copiándolos fuera de * <path> *\NtFrs_PreExisting___See_EventLog. Copiar los archivos en * <path> * puede causar conflictos de nombres si los archivos ya existen en otro asociado de replicación.<br /><br />En algunos casos, el servicio de replicación de archivos puede copiar un archivo de * <path> *\NtFrs_PreExisting___See_EventLog en * <path> * en lugar de duplicar el archivo de otro asociado de replicación.<br /><br />Se pueda recuperar espacio en cualquier momento mediante la eliminación de los archivos en * <path> *\NtFrs_PreExisting___See_EventLog.|  
|**13553**|NTFRS|El servicio de replicación de archivo, este equipo se agregó correctamente al siguiente conjunto de réplica:<br /><br />"DOMINIO VOLUMEN DEL SISTEMA (SYSVOL SHARE)"<br /><br />Información relacionada con este evento se muestra a continuación:<br /><br />Nombre del equipo DNS es "*<domain controller FQDN>*"<br /><br />Nombre de miembro del conjunto de réplica es "*<domain controller name>*"<br /><br />Ruta de acceso de réplica conjunto raíz es "*<path>*"<br /><br />Ruta de acceso del directorio de la copia intermedia de réplica es "* <path> * "<br /><br />Es la ruta de acceso del directorio de trabajo de réplica "*<path>*"|  
|**13554**|NTFRS|El servicio de replicación de archivo agregado correctamente las conexiones que se muestra a continuación para el conjunto de réplica:<br /><br />"DOMINIO VOLUMEN DEL SISTEMA (SYSVOL SHARE)"<br /><br />Entrada de "*<partner domain controller FQDN>*"<br /><br />Salida a "*<partner domain controller FQDN>*"<br /><br />Puede aparecer más información en los mensajes de registro de eventos posteriores.|  
|**13516**|NTFRS|El servicio de replicación de archivos ya no impide que el equipo DC4 se convierta en un controlador de dominio. El volumen del sistema se ha inicializado correctamente y ha notificado el servicio Netlogon que el volumen del sistema está preparado para compartido como SYSVOL.<br /><br />Tipo de "recurso compartido de red" para comprobar si el recurso compartido SYSVOL.|  
  
##### <a name="application-event-log"></a>Registro de eventos de la aplicación  
La base de datos FRS se detiene y se inicia y es purgar debido a la operación D2 BURFLAGS.  
  
||||  
|-|-|-|  
|**Identificador de evento**|**Origen**|**Mensaje**|  
|**327**|ESSENT|NTFRS (1424) el motor de base de datos separa una base de datos (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Tiempo = 0 segundos)<br /><br />Secuencia de intervalo de tiempo interno: 0,000 [1], 0,015 [2], [3] 0,000, 0,000 [4], 0,000 [5], [6] 0.516, 0,000 [7], [8] 0,000, 0,000 [9], 0,000 [10], 0.063 [11], 0,000 [12].<br /><br />Caché a revivir: 0|  
|**103**|ESSENT|NTFRS (1424) el motor de base de datos ha detenido la instancia (0).<br /><br />Cierre incorrecto: 0<br /><br />Secuencia de intervalo de tiempo interno: 0,000 [1], 0,000 [2], [3] 0,000, 0,000 [4], 0,000 [5], 0,000 [6], 0,000 [7], [8] 0,000, 0.031 [9], 0,000 [10], 0.016 [11], 0,000 [12], 0,000 [13], [14] 0.047, 0,000 [15].|  
|**102**|ESSENT|NTFRS (3000) el motor de base de datos (6.02.8189.0000) está iniciando una nueva instancia (0).|  
|**105**|ESSENT|NTFRS (3000) el motor de base de datos inició una nueva instancia (0). (Tiempo = 0 segundos)<br /><br />Secuencia de intervalo de tiempo interno: 0,000 [1], 0,000 [2], [3] 0,000, 0,000 [4], 0,000 [5], 0,000 [6], [7] 0,000, 0,000 [8], [9] 0.062, 0,000 [10], 0.141 [11].|  
|**103**|ESSENT|NTFRS (3000) el motor de base de datos ha detenido la instancia (0).<br /><br />Cierre incorrecto: 0<br /><br />Secuencia de intervalo de tiempo interno: 0,000 [1], 0,000 [2], 0,000 [3], 0,000 [4], 0,000 [5], 0,000 [6], 0,000 [7], [8] 0,000, 0,000 [9], 0,000 [10], 0,000 [11], [12] 0,000, 0,015 [13], [14] 0,000, 0,000 [15].|  
|**102**|ESSENT|NTFRS (3000) el motor de base de datos (6.02.8189.0000) está iniciando una nueva instancia (0).|  
|**105**|ESSENT|NTFRS (3000) el motor de base de datos inició una nueva instancia (0). (Tiempo = 0 segundos)<br /><br />Secuencia de intervalo de tiempo interno: 0,000 [1], 0,000 [2], [3] 0,000, 0,000 [4], 0,000 [5], 0,000 [6], [7] 0,000, 0,000 [8], [9] 0.078, 0,000 [10], 0.109 [11].|  
|**325**|ESSENT|NTFRS (3000) en el motor de base de datos se crea una nueva base de datos (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Tiempo = 0 segundos)<br /><br />Secuencia de intervalo de tiempo interno: 0,000 [1], 0,000 [2], [3] 0.016, 0.016 [4], 0,000 [5], [6] 0,015, 0,000 [7], [8] 0,000, 0.078 [9], 0.016 [10], 0,000 [11].|  
|**103**|ESSENT|NTFRS (3000) el motor de base de datos ha detenido la instancia (0).<br /><br />Cierre incorrecto: 0<br /><br />Secuencia de intervalo de tiempo interno: 0,000 [1], 0,000 [2], 0,000 [3], 0,000 [4], 0.078 [5], 0,000 [6], 0,000 [7], [8] 0,000, 0,125 [9], 0.016 [10], 0,000 [11], [12] 0,000, 0,000 [13], [14] 0,000, 0,000 [15].|  
|**102**|ESSENT|NTFRS (3000) el motor de base de datos (6.02.8189.0000) está iniciando una nueva instancia (0).|  
|**105**|ESSENT|NTFRS (3000) el motor de base de datos inició una nueva instancia (0). (Tiempo = 0 segundos)<br /><br />Secuencia de intervalo de tiempo interno: 0.016 [1], 0,000 [2], [3] 0,000, 0.094 [4], 0,000 [5], 0,000 [6], [7] 0,000, 0,000 [8], [9] 0.032, 0,000 [10], 0,000 [11].|  
|**326**|ESSENT|NTFRS (3000) el motor de base de datos adjunta una base de datos (1, c:\windows\ntfrs\jet\ntfrs.jdb). (Tiempo = 0 segundos)<br /><br />Secuencia de intervalo de tiempo interno: 0,000 [1], 0,015 [2], [3] 0,000, 0,000 [4], 0.016 [5], 0,015 [6], 0,000 [7], [8] 0,000, 0,000 [9], 0,000 [10], 0,000 [11], [12] 0,000.<br /><br />Guardar en caché: 1|  
  


