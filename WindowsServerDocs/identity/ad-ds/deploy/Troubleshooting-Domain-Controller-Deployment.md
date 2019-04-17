---
ms.assetid: 5ab76733-804d-4f30-bee6-cb672ad5075a
title: "Solución de problemas de implementación del controlador de dominio"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 254209b0da6a7bc0cd5f3880e14a7e5b16822cdd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshooting-domain-controller-deployment"></a>Solución de problemas de implementación del controlador de dominio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema cubre metodología detallada de solución de problemas de implementación y configuración del controlador de dominio.  
  
-   [Introducción a la solución de problemas](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md#BKMK_Intro)  
  
-   [Opciones de solución de problemas](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md#BKMK_Options)  
  
## <a name="BKMK_Intro"></a>Introducción a la solución de problemas  
![Solución de problemas](media/Troubleshooting-Domain-Controller-Deployment/adds_deploy_troubleshooting.png)  
  
## <a name="BKMK_Options"></a>Opciones de solución de problemas  
  
### <a name="logging-options"></a>Opciones de registro  
Los registros integrados son el instrumento más importante para solucionar problemas con la promoción del controlador de dominio y la degradación. Todos estos registros se habilitado y configurados para el máximo nivel de detalle de manera predeterminada.  
  
|Fase|Registro|  
|---------|-------|  
|Operaciones de administrador del servidor o ADDSDeployment Windows PowerShell|-%systemroot%\debug\dcpromoui.log<br /><br />-%systemroot%\debug\dcpromoui*.log|  
|Instalación y promoción del controlador de dominio|-%systemroot%\debug\dcpromo.log<br /><br />-%systemroot%\debug\dcpromo*.log<br /><br />-Evento viewer\Windows logs\System<br /><br />-Evento viewer\Windows logs\Application<br /><br />-Evento viewer\Applications y servicios logs\Directory servicio<br /><br />-Evento viewer\Applications y servicios logs\File servicio de replicación<br /><br />-Evento viewer\Applications y servicios logs\DFS replicación|  
|Actualización de dominio o bosque|-%systemroot%\debug\adprep\\<datetime>\adprep.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\csv.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\dspecup.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\ldif.log*|  
|Motor de implementación de Server Manager ADDSDeployment Windows PowerShell|-Evento viewer\Applications y servicios logs\Microsoft\Windows\DirectoryServices-Deployment\Operational|  
|Mantenimiento de Windows|-%systemroot%\Logs\CBS\\*<br /><br />-%systemroot%\servicing\sessions\sessions.xml<br /><br />-%systemroot%\winsxs\poqexec.log<br /><br />-%systemroot%\winsxs\pending.xml|  
  
### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Herramientas y los comandos para solucionar problemas de configuración del controlador de dominio  
Para solucionar problemas que no se explica por los registros, usa las siguientes herramientas como punto de partida:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   [AutoRuns.exe](https://technet.microsoft.com/sysinternals/bb963902.aspx), Administrador de tareas y MSInfo32.exe  
  
-   [3.4 del Monitor de red](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=4865) (o una herramienta de captura y análisis de la red de terceros)  
  
### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>Metodología general para solucionar problemas de configuración del controlador de dominio  
  
1.  ¿Un problema de la sintaxis sencilla provocar el error?  
  
    1.  ¿Mal u olvida de proporcionar un argumento ADDSDeployment Windows PowerShell? Por ejemplo, si usas ADDSDeployment Windows PowerShell, ¿ha olvidado agregar argumento requerido **- NombreDominio** con un nombre válido?  
  
    2.  Examina el resultado de la consola de Windows PowerShell con cuidado para ver exactamente por qué se error al analizar la línea de comandos proporcionados.  
  
2.  ¿Es el error un requisito previo error?  
  
    1.  Muchos errores que aparecían como resultados de la promoción grave ahora pueden evitarse con la comprobación de requisitos previos.  
  
    2.  Examinar el texto de los errores de requisitos previos con cuidado, proporcionan las directrices necesarias para resolver la mayoría de los problemas, como son los escenarios controlados.  
  
3.  ¿Es el error en la promoción y, por tanto, fatal?  
  
    1.  Examinar los resultados con cuidado: muchos errores tienen explicaciones simple como contraseñas incorrectas, la resolución de nombres de red o controladores de dominio sin conexión crítica.  
  
    2.  Examina la Dcpromoui.log y dcpromo.log para los errores que se muestra en la salida, a continuación, trabaja en orden inverso desde ellas para ver las indicaciones de la causa del error.  
  
        1.  Siempre se compara con un registro de la muestra de funcionar  
  
        2.  Examine los registros de ADPrep errores solo si los resultados indican un problema extender el esquema o de preparar el dominio o bosque.  
  
        3.  Examina el registro de eventos de implementación de DirectoryServices errores solo si el Dcpromoui.log carece de detalles o finaliza arbitrariamente debido a una excepción no controlada en el proceso de configuración.  
  
    3.  Examina los registros de eventos de servicios de directorio, de sistema y aplicación para otros indicadores de un problema de configuración. En ocasiones, la promoción de controlador de dominio es simplemente un síntoma de otros errores de red que pudieran afectar a distribuido todos los sistemas de configuración.  
  
    4.  Usar dcdiag.exe y repadmin.exe para validar el estado general de bosque e indicar incorrectas sutiles que pueden impedir la promoción del controlador de dominio.  
  
    5.  Usar AutoRuns.exe, Administrador de tareas o MSinfo32.exe para examinar el equipo para el software de terceros que pueda interferir.  
  
        1.  Quitar el software de terceros (no simplemente deshabilite el software; que evite la carga de controladores).  
  
    6.  3.4 NetMon se instala en el equipo que se produce un error para promocionar así como el controlador de dominio de partner de replicación y analizar el proceso de promoción con capturas de red a doble cara.  
  
        1.  Compara esto del entorno de laboratorio de trabajo para comprender qué aspecto tiene una promoción en buen estada y dónde se producen errores.  
  
        2.  En este punto, los errores están probable que con los objetos de bosque, cambios de seguridad no predeterminada o la red, y este nuevo controlador de dominio es víctima de errores de configuración de DNS, firewalls, software de protección de intrusiones de host u otros factores externos.  
  
### <a name="troubleshooting-specific-problems"></a>Solucionar problemas específicos  
  
#### <a name="events-and-error-messages"></a>Eventos y mensajes de Error  
Promoción del controlador de dominio y la degradación siempre devuelve un código al final de la operación y, a diferencia de la mayoría de los programas, hacer cero no devuelto para el éxito. Para ver el código al final de una configuración del controlador de dominio, tienes varias opciones:  
  
1.  Al usar el administrador del servidor, examine los resultados de la promoción en los diez segundos antes del reinicio automático.  
  
2.  Cuando uses ADDSDeployment Windows PowerShell, examinar los resultados de la promoción en los diez segundos antes del reinicio automático. Como alternativa, se elige no se reinicie automáticamente en la finalización. Debes agregar el **lista formato** canalización para facilitar la lectura de la salida. Por ejemplo:  
  
    ```  
    Install-addsdomaincontroller <options> -norebootoncompletion:$true | format-list  
  
    ```  
  
    Errores de validación de requisitos previo y comprobación no continúe con un reinicio, por lo que son visibles en todos los casos. Por ejemplo:  
  
  ![Solución de problemas](media/Troubleshooting-Domain-Controller-Deployment/ADDS_PSPrereqError.png)  
  
3.  En cualquier escenario, examina la dcpromo.log y dcpromoui.log.  
  
    > [!NOTE]  
    > Algunos de los errores que se indican a continuación, ya no son posibles debido a cambios de configuración de controlador de dominio y de sistema operativo en sistemas operativos posteriores. Los nuevos códigos ADDSDeployment Windows PowerShell también bloquea algunos errores, pero la dcpromo.exe / unattend no; Este es otro motivo atractivo para cambiar a todos de la automatización actual desde el desuso DCPromo ADDSDeployment Windows PowerShell.  
  
Promoción y la degradación códigos de retorno el éxito siguiente mensaje.  
  
|Código de error|Explicación|Ten en cuenta|  
|--------------|---------------|--------|  
|1|Salida de aciertos|Todavía debe reiniciar el equipo, este solo las notas que el reinicio automático marca se ha eliminado|  
|2|Salida de éxito, necesita reiniciar||  
|3|Salida de éxito, con un error no crítico|Suele aparecer cuando se devuelve la advertencia de delegación DNS. Si no configurar la delegación de DNS, usa:<br /><br />-creatednsdelegation: $false|  
|4|Salida de éxito, con un error no crítico, necesita reiniciar|Suele aparecer cuando se devuelve la advertencia de delegación DNS. Si no configurar la delegación de DNS, usa:<br /><br />-creatednsdelegation: $false|  
  
Promoción y la degradación códigos de retorno el error siguiente mensaje. También hay probabilidades de ser un mensaje de error extendida; Leer siempre el error todo con cuidado, no solo en la parte numérica.  
  
|Código de error|Explicación|Resolución sugerida|  
|--------------|---------------|------------------------|  
|11|Promoción del controlador de dominio ya se está ejecutando|No se ejecutan a una instancia de la promoción del controlador de dominio al mismo tiempo para el mismo equipo de destino|  
|12|Usuario debe ser administrador|Inicio de sesión como miembro del grupo Administradores y garantizar que elevar con UAC integrado|  
|13|Se instala la entidad de certificación|No se puede degradar este controlador de dominio, como también es una entidad de certificación. No extraiga la CA antes de inventario cuidadosamente su uso - si emite certificados, la función se quita una interrupción. No se recomienda ejecutar entidades emisoras de certificados en los controladores de dominio|  
|14|Ejecuta en modo de arranque seguro|El servidor en el modo normal de arranque|  
|15|Cambio de función está en curso o se necesita reiniciar|Debes reiniciar el servidor (debido a cambios en la configuración anterior) antes de promoción|  
|16|Cuando se ejecuta en una plataforma incorrecta|*No es probable obtener este error*|  
|17|No hay unidades NTFS 5 existe|Este error no es posible en Windows Server 2012, lo que requiere al menos % systemdrive % estar formateada con NTFS|  
|18|No hay espacio suficiente en windir|Liberar espacio en el volumen de % systemdrive % con cleanmgr.exe|  
|19|Nombre cambiar pendiente, se necesita reiniciar|Reiniciar el servidor|  
|20|Nombre del equipo es la sintaxis no válida|Cambiar el nombre del equipo con un nombre válido|  
|21|Este controlador de dominio contiene funciones FSMO, es un catálogo global o es un servidor DNS|Agregar **- demoteoperationmasterrole** al usar **- forceremoval**.|  
|22|TCP/IP debe instalarse o no funciona|Comprueba el equipo tiene TCP/IP configurado, enlazada y funciona correctamente|  
|23|Cliente DNS debe configurarse primero|Configurar un servidor DNS principal cuando se agrega un nuevo controlador de dominio a un dominio|  
|24|Credenciales proporcionadas son elementos necesarios falta o no válidos|Comprueba tu nombre de usuario y contraseña es correcta|  
|25|No se pudo encontrar el controlador de dominio para el dominio especificado|Validar la configuración del cliente DNS, las reglas del firewall|  
|26|No se pudo leer la lista de dominios del bosque|Validar la configuración del cliente DNS, funcionalidad LDAP, las reglas del firewall|  
|27|Falta el nombre de dominio|Especificar un dominio al promover o degradar|  
|28|Nombre de dominio incorrecto|Elige un nombre de dominio DNS distintos, válido cuando promociones|  
|29|Dominio principal no existe|Compruebe el dominio principal especificado al crear un nuevo dominio secundario o dominio del árbol|  
|30|Dominio en el bosque|Comprueba el nombre de dominio siempre|  
|31|Dominio secundario ya existe|Especificar un nombre de dominio diferente|  
|32|Nombre de dominio NetBIOS incorrecto|Especificar un nombre de dominio NetBIOS válido|  
|33|Ruta de acceso a los archivos IFM no es válido|Validar la ruta de acceso a la carpeta de instalación de medios|  
|34|La base de datos IFM es incorrecta|Usar los instalar desde medios correctos de este sistema operativo y el rol (misma versión del sistema operativo, el mismo tipo de controlador de dominio - RODC frente a RWDC)|  
|35|Falta SYSKEY|La instalación desde medios está cifrada y debes proporcionar un SYSKEY válido para usarla|  
|37|Ruta de acceso de la base de datos NTDS o con sus registros no es válido|Cambiar la ruta de acceso de base de datos y los registros en un volumen NTFS fijo, no una unidad asignada o ruta de acceso UNC|  
|38|Volumen no tiene suficiente espacio para la base de datos NTDS o registros|Liberar espacio usando cleanmgr.exe, agregar más espacio en disco, borrar manualmente espacio moviendo datos innecesarios en otro lugar|  
|39|Ruta de acceso para SYSVOL no es válido|Cambiar la ruta de acceso de la carpeta SYSVOL a un volumen NTFS fijo, no una unidad asignada o ruta de acceso UNC|  
|40|Nombre de sitio no válido|Proporcionar un nombre de sitio que existe|  
|41|Es necesario especificar una contraseña para el modo de prueba de errores|Proporcionar una contraseña para la cuenta DSRM, no puede ser en blanco sin importar cómo se configura la directiva de contraseñas|  
|42|Contraseña de modo de prueba de errores no cumple los criterios (solo promoción)|Proporcionar una contraseña de la cuenta DSRM que cumpla con las reglas configurado de la directiva de contraseña|  
|43|Contraseña de administrador no cumplan con criterios (solo degradación)|Proporcionar una contraseña para la cuenta de administrador local que cumpla con la directiva de contraseñas configurado reglas|  
|44|El nombre para el bosque especificado no es válido|Especificar un nombre de dominio DNS de bosque válido raíz|  
|45|Un bosque con el nombre especificado ya existe|Elige un nombre de dominio DNS de otro bosque raíz|  
|46|El nombre para el árbol especificado no es válido|Especificar un nombre de dominio DNS de árbol válido|  
|47|Un árbol con el nombre especificado ya existe|Elige un nombre de dominio DNS de árbol diferente|  
|48|El nombre del árbol no cabe en la estructura del bosque|Elige un nombre de dominio DNS de árbol diferente|  
|49|El dominio especificado no existe|Comprueba el nombre de dominio con tipo|  
|50|Durante volver atrás, se ha detectado último controlador de dominio, aunque no lo está, o se especificó el último controlador de dominio, pero no es|No se especifica **último controlador de dominio en el dominio** (**- lastdomaincontrollerindomain**) a menos que sea true. Usa **- ignorelastdcindomainmismatch** invalidar si esto es realmente el último controlador de dominio y no hay metadatos del controlador de dominio fantasma|  
|51|Existen particiones de aplicación en este controlador de dominio|Especificar a **quitar particiones de la aplicación** (**- removeapplicationpartitions**)|  
|52|Argumento de línea de comandos falta necesaria (es decir, un archivo de respuesta debe especificarse en la línea de comandos)|*Solo se ven con dcpromo / unattend, que está en desuso. Consulta la documentación anterior*|  
|53|Promoción o degradación error, el equipo debe reiniciarse para limpiar|Examina los registros y error extendido|  
|54|La promoción o degradación de error|Examina los registros y error extendido|  
|55|El usuario ha cancelado la degradación de promoción|Examina los registros y error extendido|  
|56|El usuario ha cancelado la promoción o degradación, el equipo debe reiniciarse para limpiar|Examina los registros y error extendido|  
|58|Debe especificar un nombre de sitio durante la promoción RODC|Debes especificar un sitio para un RODC, no detectará automáticamente una como un RWDC|  
|59|Durante el disminuir, este controlador de dominio es el último servidor DNS para una de sus zonas|Especifica que este es el **último servidor DNS del dominio** o usar **- ignorelastdnsserverfordomain**|  
|60|Un controlador de dominio que ejecutan Windows Server 2008 o posterior debe estar presente en el dominio para fomentar RODC|Promocionar uno al menos Windows Server 2008 o posterior modelo de controlador de dominio grabable|  
|61|No puedes instalar los servicios de dominio de Active Directory con DNS en un dominio existente que aún no hospeda DNS|*No es posible obtener este error*|  
|62|Archivo de respuesta no tiene una sección [DCInstall]|*Solo se ven con dcpromo / unattend, que está en desuso. Consulta la documentación más antigua.*|  
|63|Nivel funcional del bosque está por debajo de servidor de windows 2003|Aumentar el nivel funcional del bosque al menos Windows Server 2003 nativo. Windows 2000 y Windows NT 4.0 dejan de sistemas operativos compatibles|  
|64|Error porque no se pudo realizar la detección de componente binario de promoción|Instalar el rol de AD DS|  
|65|Error porque no se pudo realizar la instalación del componente binario de promoción|Instalar el rol de AD DS|  
|66|No se pudo realizar la promoción porque no se pudo realizar la detección de sistema operativo|Examinar el error extendido y registros; el servidor no puede devolver su versión del sistema operativo. Es probable que el equipo, deberás volver a instalarse, como su estado general es altamente sospechosa|  
|68|Duplicador no es válido|Usar repadmin.exe o **obtener-ADReplication\ *** Windows PowerShell para validar el estado de controlador de dominio de partners|  
|69|Puerto necesario ya está en uso por otra aplicación|Usa **netstat.exe - anob** para buscar procesos que se asignan incorrectamente a los puertos de AD DS reservados|  
|70|El controlador de dominio raíz de bosque debe ser un catálogo global|*Solo se ven con dcpromo / unattend, que está en desuso. Consulta la documentación anterior*|  
|71|Ya está instalado el servidor DNS|No se especifica para instalar DNS (**- installDNS**) si el servicio DNS ya está instalado|  
|72|Equipo ejecuta Servicios de escritorio remoto en el modo no administrador|No puede promover este controlador de dominio, como también es un servidor RDS configurado más de dos usuarios de administrador. No extraiga RDS antes de su uso - inventario detenidamente si se usa para aplicaciones o los usuarios finales, eliminación provocará una interrupción|  
|73|El nivel funcional del bosque especificado no es válido.|Especificar un nivel funcional del bosque válido|  
|74|El nivel funcional del dominio especificado no es válido.|Especificar un nivel funcional del dominio válido|  
|75|No se puede determinar la directiva de replicación de contraseñas predeterminada.|Validar que la directiva de replicación de contraseñas RODC existe y que sea accesible|  
|76|Grupos de seguridad replicados o no replicados especificado no son válidos|Validar que has escrito en cuentas de dominio y usuario válidas al especificar una directiva de replicación de contraseña|  
|77|El argumento especificado no es válido|Examina los registros y error extendido|  
|78|Error al examinar el bosque de Active Directory|Examina los registros y error extendido|  
|79|No se puede promover RODC porque no se ha realizado rodcprep|Usa Windows Server 2012 para preparar el bosque o usar **adprep.exe /rodcprep**|  
|80|No se ha ejecutado DomainPrep|Usa Windows Server 2012 para preparar el dominio o usar **adprep.exe /domainprep**|  
|81|No se ha ejecutado ForestPrep|Usa Windows Server 2012 para preparar el bosque o usar **adprep.exe /forestprep**|  
|82|Coincidencia de esquema del bosque|Usa Windows Server 2012 para preparar el bosque o usar **adprep.exe /forestprep**|  
|83|SKU no compatible|*No es probable obtener este error*|  
|84|No se puede detectar una cuenta de controlador de dominio|Valida que los controladores de dominio existentes establecer atributo de control de cuentas de usuario correcta.|  
|85|No se puede seleccionar una cuenta de controlador de dominio para la fase 2|Devuelve si especificas "Utilizar la cuenta existente" pero cualquiera no se encontró la cuenta o hay un error durante la búsqueda de la cuenta. Asegúrate de que proporcionaste en la cuenta RODC provisionalmente correcta|  
|86|Para ejecutar la promoción de la fase 2|Devuelve si promover un controlador de dominio pero existe una cuenta existente y no se especificó "Permitir reinstalar"|  
|87|Existe una cuenta de controlador de dominio de tipo en conflicto|Cambia el nombre del equipo antes de promoción, si no intenta asociarse a un controlador de dominio sin ocupar. Debes adjuntar la cuenta de controlador de dominio sin ocupar mediante **- useexistingaccount** y el argumento de solo lectura o escritura correcto, según el tipo de cuenta|  
|88|El administrador del servidor especificado no es válido|Especifica una cuenta válida para la delegación de administración RODC. Comprueba que la cuenta especificada es un usuario o grupo válido|  
|89|QUITAR principal para el dominio especificado es sin conexión.|Usa **netdom.exe consulta fsmo** para detectar el maestro RID. Poner en línea y que sea accesible para el controlador de dominio que promueve|  
|90|Patrón de nomenclatura de dominio es sin conexión.|Usa **netdom.exe consulta fsmo** para detectar el maestro de nombres de dominio. Poner en línea y que sea accesible para el controlador de dominio que promueve|  
|91|No se puede detectar si el proceso es wow64|*No es posible obtener este error, el sistema operativo es 64 bits*|  
|92|No se admite el proceso de WOW64|*No es posible obtener este error, el sistema operativo es 64 bits*|  
|93|No se está ejecutando el servicio de controlador de dominio para degradación no forzosa|Iniciar el servicio de AD DS|  
|94|Contraseña de administrador local no cumple el requisito: en blanco o no es necesaria|Proporcionar una contraseña en blanco y asegúrate de que la directiva de contraseñas local requiere una contraseña|  
|95|No se puede degradar el último Windows Server 2008 o posterior controlador de dominio en el dominio donde existen RODC dinámicos|Primero debe degradar todos los RODCs para poder degradar todos los Windows Server 2008 o posterior controladores de dominio grabable|  
|96|No se puede desinstalar los binarios de DS|*Solo se ven con dcpromo / unattend, que está en desuso. Consulta la documentación anterior*|  
|97|Bosque funcional versión de nivel más alto que el sistema operativo de dominio de secundarios|Proporcionar un dominio secundario funcional de la misma o mayor que el nivel funcional del bosque|  
|98|Componente binario instalar o desinstalar está en curso.|*Solo se ven con dcpromo / unattend, que está en desuso. Consulta la documentación anterior*|  
|99|Es demasiado bajo nivel funcional del bosque (error es Windows Server 2012 solo)|Aumentar el nivel funcional del bosque al menos Windows Server 2003 nativo. Windows 2000 y Windows NT 4.0 dejan de sistemas operativos compatibles|  
|100|Nivel funcional del dominio es demasiado bajo (error es Windows Server 2012 solo)|Aumentar el nivel funcional de dominio al menos Windows Server 2003 nativo. Windows 2000 y Windows NT 4.0 dejan de sistemas operativos compatibles|  
  
#### <a name="knownlikely-issues-and-support-scenarios"></a>Problemas conocidos o probable y escenarios de soporte técnico  
Estos son los problemas comunes que se detectaron durante el proceso de desarrollo de Windows Server 2012. Todos estos problemas son "por diseño" y tengan una solución alternativa válida o técnica más adecuada para evitarlos en primer lugar. Muchos de estos comportamientos son idénticos en Windows Server 2008 R2 y sistemas operativos más antiguos, pero la reescritura de implementación de AD DS lleva la sensibilidad cada vez mayor a problemas.  
  
|Problema|Degradar un controlador de dominio deja DNS que se ejecuta con ningún zonas|  
|---------|-----------------------------------------------------------------|  
|Síntomas|Servidor aún responde a las solicitudes DNS, pero no tiene ninguna información de la zona|  
|Resolución y notas|Al quitar el rol de AD DS, también quitar el rol de servidor DNS o establecer el servicio de servidor DNS en deshabilitado. Recuerda que tienes que apuntar al cliente DNS a otro servidor que él mismo. Si usas Windows PowerShell, ejecuta lo siguiente después de degradar al servidor:<br /><br />Código - windowsfeature desinstalar dns<br /><br />o<br /><br />Código: conjunto de servicio dns - starttype deshabilitado<br />Detener servicio dns|  
  
|Problema|Promocionar un Windows Server 2012 en un dominio de etiqueta única existente no configura updatetopleveldomain = 1 o allowsinglelabeldnsdomain = 1|  
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|  
|Síntomas|No se produce el registro de registro dinámica de DNS|  
|Resolución y notas|Establecer estos valores en las directivas de grupo Netlogon y DNS. Microsoft empezó a bloquear la creación de dominio de etiqueta única en Windows Server 2008; Puedes usar ADMT o la herramienta de cambiar el nombre de dominio para cambiar a una estructura de dominio DNS aprobada.|  
  
|Problema|Se produce un error en la degradación del último controlador de dominio en un dominio si hay cuentas RODC creadas previamente, libres|  
|---------|------------------------------------------------------------------------------------------------------------|  
|Síntomas|Se produce un error en la degradación con mensaje:<br /><br />**Dcpromo.General.54**<br /><br />Los servicios de dominio de Active Directory no pudo encontrar otro controlador de dominio de directorio activo para transferir los datos restantes en la partición de directorio CN = esquema, CN = Configuración, DC = corp, DC = contoso, DC = com.<br /><br />"El formato de nombre de dominio especificado no es válido."|  
|Resolución y notas|Quitar cualquier restante creado previamente cuentas RODC antes de la degradación de un dominio, con **Dsa.msc** o **limpieza de los metadatos Ntdsutil.exe**.|  
  
|Problema|Preparación de bosque y dominio automatizada no ejecuta GPPREP|  
|---------|---------------------------------------------------------------|  
|Síntomas|Funcionalidad de planificación de dominios para la directiva de grupo, modo de planeamiento de conjunto de resultante de directivas (RSOP), requiere permisos de Active Directory y sistema de archivos actualizado para GP existente. Sin Gpprep, no puedes usar RSOP planeación entre dominios.|  
|Resolución y notas|Ejecutar **adprep.exe /gpprep** manualmente para todos los dominios que no se han preparado anteriormente para Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2. Los administradores deben ejecutar GPPrep una sola vez en el historial de un dominio, no con cada actualización. No se ejecuta por adprep automática porque si ya has configurado los permisos adecuados personalizados, esto haría que todo el contenido SYSVOL volver a replicar en todos los controladores de dominio.|  
  
|Problema|Se produce un error en la instalación desde un medio verificar cuando apuntando a una ruta de acceso UNC|  
|---------|------------------------------------------------------------------|  
|Síntomas|Mensaje de error:<br /><br />El código: no se pudo validar la ruta del archivo multimedia. Excepción al llamar a "GetDatabaseInfo" argumentos "2". La carpeta no es válida.|  
|Resolución y notas|Debes almacenar archivos IFM en un disco local, no una ruta de acceso UNC remoto. Este bloque de forma intencionada impide que la promoción de servidor parcial debido a una interrupción de la red.|  
  
|Problema|Advertencia de delegación de DNS que se muestran dos veces durante la promoción del controlador de dominio|  
|---------|-------------------------------------------------------------------------|  
|Síntomas|Advertencia devuelve *dos veces* cuando promociones mediante ADDSDeployment Windows PowerShell:<br /><br />Código - "no se puede crear una delegación de este servidor DNS porque no se encuentra la zona principal autorizado o no se ejecuta DNS de Windows server. Si va a integrar con una infraestructura de DNS, debes crear manualmente una delegación de este servidor DNS en la zona principal para garantizar la resolución de nombres confiable desde fuera del dominio. De lo contrario, se requiere ninguna acción."|  
|Resolución y notas|Pasar por alto. ADDSDeployment Windows PowerShell muestra la advertencia primero durante la comprobación de requisitos previos, a continuación, vuelve a durante la configuración del controlador de dominio. Si no desea configurar la delegación de DNS, usa el argumento:<br /><br />Código - - creatednsdelegation: $false<br /><br />Hacer *no* omitir la comprobación de requisitos previa para suprimir este mensaje|  
  
|Problema|Especifica el UPN o las credenciales no del dominio durante la configuración de devuelve engañosos errores|  
|---------|--------------------------------------------------------------------------------------------|  
|Síntomas|El administrador del servidor devuelve error:<br /><br />Código - excepción llamando a "DNSOption" argumentos "6"<br /><br />ADDSDeployment Windows PowerShell devuelve error:<br /><br />Error de código - verificación de permisos de usuario. Debe proporcionar el nombre del dominio al que pertenece esta cuenta de usuario.|  
|Resolución y notas|Asegúrate de que estás proporcionando credenciales de dominio válidas en forma de **dominio\nombre de usuario**.|  
  
|Problema|Quitar la función DirectoryServices-DomainController con Dism.exe conduce a un servidor que no puede arrancar|  
|---------|---------------------------------------------------------------------------------------------------|  
|Síntomas|Si usa Dism.exe para quitar el rol de AD DS antes de devolver un controlador de dominio correctamente, el servidor ya no se arranca normalmente y muestra el error:<br /><br />Código - estado: 0 x 000000000<br />Información: Se ha producido un error inesperado.|  
|Resolución y notas|Arrancar en modo de reparación de servicios de directorio usando *MAYÚS+F8*. Agrega el rol de AD DS atrás y, a continuación, forzar la degradación el controlador de dominio. Como alternativa, restaurar el estado del sistema de copia de seguridad. No uses Dism.exe para su eliminación del rol de AD DS; la utilidad no tiene conocimiento de los controladores de dominio.|  
  
|Problema|No se puede instalar un bosque nuevo al establecer forestmode en Win2012|  
|---------|--------------------------------------------------------------------|  
|Síntomas|Promoción mediante Windows PowerShell de ADDSDeployment devuelve error:<br /><br />Código - Test.VerifyDcPromoCore.DCPromo.General.74<br /><br />Error de comprobación de requisitos previos para la promoción de controlador de dominio. El nivel funcional del dominio especificado no es válido|  
|Resolución y notas|No especificas un modo funcional del bosque de Win2012 sin *también* especificar un modo de Win2012 funcional del dominio. Este es un ejemplo que funcione sin errores:<br /><br />Código - de - forestmode Win2012 - domainmode Win2012]|  
  
|||  
|-|-|  
|Problema|Al hacer clic en comprobar en la instalación desde el área de selección de elementos multimedia aparece para no hacer nada|  
|Síntomas|Cuando especifica una ruta de acceso a una carpeta IFM, haz clic en el **comprobar** botón nunca devuelve un mensaje o parece que hacer nada.|  
|Resolución y notas|La **comprobar** botón devuelve solo los errores si hay problemas. De lo contrario, facilita la **siguiente** botón seleccionable si has proporcionado una ruta de acceso IFM. Debe hacer clic **comprobar** continuar si has seleccionado IFM.|  
  
|||  
|-|-|  
|Problema|Degradar con el administrador del servidor no proporciona comentarios hasta que finalice.|  
|Síntomas|Al usar el administrador del servidor para quitar el rol de AD DS y degradar un controlador de dominio, no hay ningún comentario en curso dada hasta que finalice la degradación o con errores.|  
|Resolución y notas|Esta es una limitación del administrador del servidor. Comentarios, usa el cmdlet de ADDSDeployment Windows PowerShell:<br /><br />Código - addsdomaincontroller desinstalar|  
  
|||  
|-|-|  
|Problema|Instalar desde medios comprobar no detecta que el contenido multimedia RODC proporcionada para el controlador de dominio grabable, ni viceversa.|  
|Síntomas|Al promover un nuevo controlador de dominio usando IFM y proporcionar medios incorrectas para IFM, como un medio de RODC para un controlador de dominio grabable o RWDC multimedia para un RODC - el botón Comprobar no devuelve un error. Más adelante, promoción se produce error:<br /><br />Código - se ha producido un error al intentar configurar este equipo como un controlador de dominio. <br />La promoción de la instalación desde el medio de un controlador de dominio de solo lectura no se puede iniciar porque no se permite la base de datos de origen especificado. Solo las bases de datos de otros RODC pueden usarse para su promoción IFM de un RODC.|  
|Resolución y notas|Comprueba solo valida la integridad general de IFM. No incluyas tipo incorrecto IFM a un servidor. Reiniciar el servidor antes de intentar promoción con los medios correctos.|  
  
|||  
|-|-|  
|Problema|Promocionar un RODC en una cuenta de equipo creado previamente se produce un error|  
|Síntomas|Al usar ADDSDeployment Windows PowerShell para promocionar un nuevo RODC con una cuenta de equipo provisional, recibe el error:<br /><br />Código - el conjunto de parámetros no se puede resolver con los parámetros con nombre especificado.    <br />InvalidArgument: ParameterBindingException<br />    + FullyQualifiedErrorId: AmbiguousParameterSet,Microsoft.DirectoryServices.Deployment.PowerShell.Commands.Install|  
|Resolución y notas|No proporciones parámetros ya definido ya de una cuenta RODC creada previamente. Estos incluyen:<br /><br />Código - - readonlyreplica<br />-installdns<br />-donotconfigureglobalcatalog<br />-el nombre de sitio<br />-installdns|  
  
|||  
|-|-|  
|Problema|Anular la selección/seleccionar "Reiniciar cada servidor de destino automáticamente si es necesario" no hace nada|  
|Síntomas|Si seleccionar (o no) la opción de administrador del servidor **reiniciar automáticamente cada servidor de destino en caso necesario** whendemoting un controlador de dominio mediante la eliminación de rol, el servidor se reinicia, independientemente de elección.|  
|Resolución y notas|Esto es intencionado. El proceso de degradación reinicia el servidor independientemente de esta configuración.|  
  
|||  
|-|-|  
|Problema|Dcpromo.log muestra "[error] configuración de seguridad en los archivos del servidor que no se pudo con 2"|  
|Síntomas|Degradación de un controlador de dominio que se complete sin problemas, pero el examen del registro dcpromo de error de muestra:<br /><br />Error de código: [error] configurar la seguridad de archivos del servidor con 2|  
|Resolución y notas|Pasar por alto, error es cosméticos y esperado.|  
  
|||  
|-|-|  
|Problema|Se produce un error de comprobación de requisitos previos adprep con el error "No se pudo realizar la comprobación de conflicto de esquema de Exchange"|  
|Síntomas|Al intentar promover un controlador de dominio de Windows Server 2012 en una existente de Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 bosque, se produce un error en la comprobación de requisitos previos con error:<br /><br />Error de código - verificación de requisitos previos para preparar el anuncio. No se puede realizar la comprobación de conflicto de esquema de Exchange para el dominio * <domain name> * (excepción: el servidor RPC no está disponible)<br /><br />La adprep.log muestra el error:<br /><br />Código - Adprep no pudo recuperar datos desde el servidor*<domain controller>*<br /><br />a través de Instrumental de administración de Windows (WMI).|  
|Resolución y notas|El nuevo controlador de dominio no puede acceder a WMI a través de protocolos DCOM/RPC con los controladores de dominio existente. Hasta la fecha, ha habido tres causas:<br /><br />-Un firewall regla bloquea el acceso a los controladores de dominio existente<br /><br />-La cuenta de servicio de red es falta de "Inicio de sesión como servicio" (SeServiceLogonRight) privilegios en los controladores de dominio existente<br /><br />-NTLM está deshabilitada en los controladores de dominio, mediante las directivas de seguridad que se describe en [introducir la restricción de autenticación NTLM](https://technet.microsoft.com/library/dd560653(WS.10).aspx)|  
  
|||  
|-|-|  
|Problema|Crear un nuevo bosque de AD DS siempre muestra la advertencia de DNS|  
|Síntomas|Al crear un nuevo bosque de AD DS y la zona DNS en el nuevo controlador de dominio por sí mismos, siempre que reciben el mensaje de advertencia:<br /><br />Se detectó el código: un error en la configuración de DNS. <br />Ninguno de los servidores DNS usados en este equipo ha respondido en el intervalo de tiempo de espera.<br />(código de error 0x000005B4 "ERROR_TIMEOUT")|  
|Resolución y notas|Pasar por alto. Esta advertencia es intencionada en el primer controlador de dominio en el dominio raíz de un bosque nuevo, en caso de pensada para apuntar a un servidor DNS y la zona.|  
  
|||  
|-|-|  
|Problema|Windows PowerShell - whatif argumento devuelve información incorrecta del servidor DNS|  
|Síntomas|Si usas el **- whatif** argumento al configurar un controlador de dominio con implícito o explícito **- installdns: $true**, la salida resultante se muestra:<br /><br />Código - "servidor DNS: No"|  
|Resolución y notas|Pasar por alto. DNS está instalado y configurado correctamente.|  
  
|||  
|-|-|  
|Problema|Después de la promoción, inicio de sesión se produce un error con "no hay suficiente almacenamiento está disponible para procesar este comando"|  
|Síntomas|Después de promocionar un nuevo controlador de dominio y, a continuación, cierra la sesión e intentar iniciar una sesión interactiva, recibe el error:<br /><br />Código: no hay suficiente almacenamiento está disponible para procesar este comando|  
|Resolución y notas|El controlador de dominio no se reinicia después de la promoción, ya sea debido a un error o porque se especificó el argumento ADDSDeployment Windows PowerShell **- norebootoncompletion**. Reinicia el controlador de dominio.|  
  
|||  
|-|-|  
|Problema|El botón siguiente no está disponible en la página Opciones de controlador de dominio|  
|Síntomas|Incluso si has configurado una contraseña, la **siguiente** botón en la **opciones del controlador de dominio** página en el administrador del servidor no está disponible. No hay ningún sitio aparece en la **nombre del sitio** menú.|  
|Resolución y notas|Tiene varios sitios de AD DS y al menos una falta subredes; Este controlador de dominio futuras pertenece a una de las subredes. Debes seleccionar manualmente la subred en el menú desplegable de nombre de sitio. También debes revisar todos los sitios de AD mediante DSSITE. MSC o usa el siguiente comando de Windows PowerShell para buscar todos los sitios que falta subredes:<br /><br />Código - adreplicationsite de get-filtro *-subredes propiedad & #124; WHERE-object {! $_.subnets - EC "\ *"} & #124; nombre de la tabla de formato|  
  
|||  
|-|-|  
|Problema|Promoción o degradación se produce un error con el mensaje "no se puede iniciar el servicio"|  
|Síntomas|Si intentas promoción, la degradación o la clonación de un controlador de dominio recibe el error:<br /><br />Código: no se puede iniciar el servicio, ya sea porque está deshabilitado o si se tiene dispositivos habilitados asociados"(0 x 80070422)<br /><br />Este error puede ser interactivo, un evento, o al escribirlos en un registro como dcpromoui.log o dcpromo.log|  
|Resolución y notas|El servicio de rol servidor de DS (DsRoleSvc) está deshabilitado. De manera predeterminada, este servicio se instala durante la instalación del rol de AD DS y establece como un tipo de inicio Manual. No deshabilite este servicio. Vuelve a establecerlo Manual y permitir que las operaciones de rol de DS para iniciar y detener la a petición. Este comportamiento es por diseño.|  
  
|||  
|-|-|  
|Problema|El administrador del servidor aún advierte que necesitas promover el controlador de dominio|  
|Síntomas|Si promover un controlador de dominio mediante el desuso dcpromo.exe / unattend o actualizar un controlador de dominio de Windows Server 2008 R2 existente en su lugar a Windows Server 2012, el administrador del servidor sigue mostrando la tarea de configuración posterior a la implementación **promover este servidor a un controlador de dominio**.|  
|Resolución y notas|Haz clic en el vínculo de advertencia posterior a la implementación y el mensaje desaparece para siempre. Este comportamiento es cosméticos y esperada.|  
  
|||  
|-|-|  
|Problema|Script de implementación de administrador del servidor que falta la instalación del rol|  
|Síntomas|Si promover un controlador de dominio mediante el administrador del servidor y guarda el script de implementación de Windows PowerShell, esta información no incluye los argumentos y el cmdlet de instalación del rol (install-windowsfeature-el nombre de servicios de dominio de ad - includemanagementtools). Sin el rol, no se puede configurar el controlador de dominio.|  
|Resolución y notas|Agregar manualmente ese cmdlet y argumentos para las secuencias de comandos. Este comportamiento es normal y por diseño.|  
  
|||  
|-|-|  
|Problema|Script de implementación de administrador del servidor no se denomina PS1|  
|Síntomas|Si promover un controlador de dominio mediante el administrador del servidor y guarda el script de implementación de Windows PowerShell, el archivo se llama con un nombre aleatorio temporal y no como un archivo PS1.|  
|Resolución y notas|Manualmente el nombre del archivo. Este comportamiento es normal y por diseño.|  
  
|Problema|Dcpromo / unattend permite niveles funcionales no compatibles|  
|-|-|  
|Síntomas|Si promoción un controlador de dominio mediante dcpromo / unattend con el siguiente archivo de respuesta de muestra:<br /><br />Código:<br /><br />[DCInstall]<br />NewDomain = bosque<br /><br />ReplicaOrNewDomain = dominio<br /><br />NewDomainDNSName=corp.contoso.com<br /><br />SafeModeAdminPassword =Safepassword@6<br /><br />Valor de DomainNetbiosName = corp<br /><br />DNSOnNetwork es "yes"<br /><br />AutoConfigDNS es "yes"<br /><br />RebootOnSuccess = NoAndNoPromptEither<br /><br />RebootOnCompletion = No<br /><br />*DomainLevel = 0*<br /><br />*ForestLevel = 0*<br /><br />Se produce un error en la promoción con los siguientes errores en la dcpromoui.log:<br /><br />Código - dcpromoui EA4.5B8 0089 13:31:50.783 ENTRAR CArgumentsSpec::ValidateArgument DomainLevel<br /><br />Dcpromoui EA4.5B8 008A 13:31:50.783 valor para DomainLevel es 0<br /><br />Dcpromoui EA4.5B8 008B 13:31:50.783 código de salida es 77<br /><br />Dcpromoui EA4.5B8 008C 13:31:50.783 el argumento especificado no es válido.<br /><br />registro de cierre de 13:31:50.783 D 008 EA4.5B8 Dcpromoui<br /><br />Dcpromoui 0032 EA4.5B8 13:31:50.830 código de salida es 77<br /><br />Nivel de 0 es Windows 2000, que no se admite en Windows Server 2012.|  
|Resolución y notas|No utilice el desuso dcpromo / unattend y comprender que permite especificar la configuración no válida que errores más adelante. Este comportamiento es normal y por diseño.|  

|Problema|Promoción "se bloquea" al crear el objeto de configuración NTDS, nunca se completa|  
|-|-|  
|Síntomas|Si promocionar una réplica DC o RODC, la promoción y llega a "crear el objeto de configuración de NTDS" nunca ganancias o completa. Los registros de detener la actualización también.|  
|Resolución y notas|Este es un problema conocido causado por proporcionar credenciales de la cuenta predefinida de administrador local con una contraseña coincidente a la cuenta de administrador de dominio integrada. Esto hace que un error hacia abajo en el motor de configuración principal que hace no error, sino en espera de manera indefinida (cuasi bucle). Esto se espera - aunque no deseables - comportamiento.<br /><br />Para corregir el servidor:<br /><br />1. reinicia.<br /><br />1. en AD, eliminar cuenta de equipo de ese servidor miembro (no todavía será una cuenta de DC)<br /><br />1. en ese servidor, forzosamente retirar, desde el dominio<br /><br />1. en ese servidor, quita el rol de AD DS.<br /><br />1. reinicio de<br /><br />1. vuelve a Agregar promoción de reintento y el rol de AD DS, garantizar que siempre proporciones el ***domain\admin*** con el formato de credenciales para la promoción de controlador de dominio y no solo la cuenta predefinida de administrador local|  
