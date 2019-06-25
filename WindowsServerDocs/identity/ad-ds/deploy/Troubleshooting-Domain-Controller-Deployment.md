---
ms.assetid: 5ab76733-804d-4f30-bee6-cb672ad5075a
title: Solución de problemas de implementación de controladores de dominio
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 03/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 15ba8b9d17dfcaf5893086c83811d864c019dac6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443125"
---
# <a name="troubleshooting-domain-controller-deployment"></a>Solución de problemas de implementación de controladores de dominio

>Se aplica a: Windows Server 2016

En este tema se describe la metodología para solucionar problemas de configuración e implementación de controladores de dominio.  

## <a name="introduction-to-troubleshooting"></a>Introducción a la solución de problemas

![Solución de problemas](media/Troubleshooting-Domain-Controller-Deployment/adds_deploy_troubleshooting.png)  

## <a name="built-in-logs-for-troubleshooting"></a>Registros integrados para solucionar problemas

Los registros integrados son la herramienta más importante para solucionar problemas de promoción y degradación de controladores de dominio. Todos estos registros están habilitados y configurados para ofrecer el máximo nivel de detalle de forma predeterminada.  

|Fase|Registro|  
|---------|-------|  
|Operaciones de Administrador del servidor o de ADDSDeployment para Windows PowerShell|-   %systemroot%\debug\dcpromoui.log<br /><br />-   %systemroot%\debug\dcpromoui*.log|  
|Instalación/Promoción del controlador de dominio|-   %systemroot%\debug\dcpromo.log<br /><br />-   %systemroot%\debug\dcpromo*.log<br /><br />: Logs\System del Visor de eventos<br /><br />: Logs\Application de Visor de eventos<br /><br />-Event viewer\Applications and services servicios\servicio de directorio<br /><br />-Event viewer\Applications and services servicios\servicio de replicación<br /><br />-Event viewer\Applications y servicios\replicación DFS|  
|Actualización de bosque o de dominio|-   %systemroot%\debug\adprep\\<datetime>\adprep.log<br /><br />-   %systemroot%\debug\adprep\\<datetime>\csv.log<br /><br />-   %systemroot%\debug\adprep\\<datetime>\dspecup.log<br /><br />-   %systemroot%\debug\adprep\\<datetime>\ldif.log*|  
|Motor de implementación de ADDSDeployment para Windows PowerShell de Administrador del servidor|-Event viewer\Applications and services servicios\microsoft\windows\directoryservices-Deployment\Operational|  
|Servicio de actualización de Windows|-   %systemroot%\Logs\CBS\\*<br /><br />-   %systemroot%\servicing\sessions\sessions.xml<br /><br />-   %systemroot%\winsxs\poqexec.log<br /><br />-   %systemroot%\winsxs\pending.xml|  

### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Herramientas y comandos para solucionar problemas de configuración de controladores de dominio

Para solucionar problemas que no se explican con los registros, usa las herramientas siguientes como punto de partida:  

-   Dcdiag.exe  

-   Repadmin.exe  

-   [AutoRuns.exe](https://technet.microsoft.com/sysinternals/bb963902.aspx), Administrador de tareas y MSInfo32.exe  

-   [Network Monitor 3.4](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=4865) (o una herramienta de análisis y captura de red de terceros)  

### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>Metodología general para la solución de problemas de configuración de controladores de dominio  

1.  ¿El error ha sido causado por un problema de sintaxis?  

    1.  ¿Has escrito algo mal o te has olvidado de dar un argumento en ADDSDeployment para Windows PowerShell? Por ejemplo, si usas ADDSDeployment para Windows PowerShell, ¿te has olvidado de agregar el argumento obligatorio **-domainname** con un nombre válido?  

    2.  Examina detenidamente la salida de la consola de Windows PowerShell para comprobar exactamente qué es lo que no se analiza correctamente en la línea de comandos.  

2.  ¿El problema se debe a un error de requisitos previos?  

    1.  Muchos errores que solían aparecer como errores graves de resultados de promoción pueden evitarse con el comprobador de requisitos previos.  

    2.  Examina detenidamente el texto de los errores de requisitos previos, ya que, al tratarse de escenarios controlados, proporcionan los indicios necesarios para solucionar la mayoría de los problemas.  

3.  ¿El error se produce en la promoción y por lo tanto es grave?  

    1.  Examina detenidamente los resultados: muchos errores tienen explicaciones sencillas, como contraseñas incorrectas, resolución de nombres de red o errores graves de conexión de controladores de dominio.  

    2.  Examina los archivos Dcpromoui.log y dcpromo.log, y busca los errores que se muestran en la salida; después, intenta determinar el origen para conocer por qué se produjo el error.  

        1.  Siempre deben compararse con un registro de ejemplo válido  

        2.  Examina los registros de ADPrep y busca errores, pero solo si los resultados indican un problema al ampliar el esquema o al preparar el bosque o dominio.  

        3.  Examina el registro de eventos de DirectoryServices-Deployment y busca errores, pero solo si faltan detalles en Dcpromoui.log o si finaliza arbitrariamente debido a una excepción no controlada en el proceso de configuración.  

    3.  Examina el registro de eventos de Servicios de directorio, Sistema y Aplicación en busca de otros indicadores de un problema de configuración. A menudo, la promoción de controladores de dominio es tan solo un síntoma de otro error de configuración de red que afectaría a todos los sistemas distribuidos.  

    4.  Usa dcdiag.exe y repadmin.exe para validar el estado general del bosque y detectar pequeños errores de configuración que podrían evitar la promoción de controladores de dominio.  

    5.  Usa AutoRuns.exe, Administrador de tareas o MSinfo32.exe para buscar software de terceros que pudiera causar problemas en el equipo.  

        1.  Desinstala el software de terceros (no es suficiente con deshabilitarlo, ya que así no evitas que se carguen los controladores).  

    6.  Instala NetMon 3.4 en el equipo que no pueda completar la promoción, así como en el controlador de dominio asociado de replicación, y analiza el proceso de promoción con capturas de red de doble cara.  

        1.  Compara los resultados con un entorno de trabajo que funcione correctamente para aprender a distinguir entre una promoción correcta y una que produce errores.  

        2.  Llegado este punto, es probable que los errores estén relacionados con objetos del bosque, cambios de seguridad no predeterminados o la red, y este nuevo controlador de dominio sea una víctima de errores de configuración en DNS, firewalls, software de protección de intrusión en host u otros factores externos.  

## <a name="troubleshooting-events-and-error-messages"></a>Solución de problemas de eventos y mensajes de Error

La promoción y la degradación de controladores de dominio siempre devuelven un código al finalizar la operación y, al contrario que la mayoría de programas, no devuelven cero para indicar que se han completado correctamente. Existen varias opciones para ver el código final de la configuración de un controlador de dominio:  

1. Si usas el Administrador del servidor, examina los resultados de la promoción durante los diez segundos anteriores al reinicio automático.  

2. Si usas ADDSDeployment para Windows PowerShell, examina los resultados de la promoción durante los diez segundos anteriores al reinicio automático. Como alternativa, puedes elegir no reiniciar automáticamente cuando se complete la operación. Agrega la canalización **Format-List** para que sea más fácil leer la salida. Por ejemplo:  

   ```  
   Install-addsdomaincontroller <options> -norebootoncompletion:$true | format-list  

   ```  

   Los errores de validación y comprobación de requisitos previos no continuarán después de un reinicio, por lo que serán visibles siempre. Por ejemplo:  

   ![Solución de problemas](media/Troubleshooting-Domain-Controller-Deployment/ADDS_PSPrereqError.png)  

3. En cualquier caso, examina dcpromo.log y dcpromoui.log.  

   > [!NOTE]  
   > Algunos de los errores que se muestran en la lista siguiente no son posibles debido a cambios de configuración del sistema operativo y del controlador de dominio en sistemas operativos posteriores. Los nuevos códigos de ADDSDeployment para Windows PowerShell también evitan determinados errores, pero dcpromo.exe /unattend no lo hace; esta es otra razón de peso para cambiar toda la automatización actual de DCPromo (en desuso) al módulo ADDSDeployment para Windows PowerShell.  

### <a name="promotion-and-demotion-success-codes"></a>Promoción y degradación de códigos de éxito

|Código de error|Explicación|Nota|  
|--------------|---------------|--------|  
|1|Salir, operación correcta|Es necesario reiniciar, esto solo indica que se ha eliminado la marca de reinicio automático|  
|2|Salir, operación correcta, es necesario reiniciar||  
|3|Salir, operación correcta, con un error no grave|Suele aparecer al devolver la advertencia de delegación DNS. Si no configuras la delegación DNS, usa lo siguiente:<br /><br />-creatednsdelegation:$false|  
|4|Salir, operación correcta, con un error no grave, es necesario reiniciar|Suele aparecer al devolver la advertencia de delegación DNS. Si no configuras la delegación DNS, usa lo siguiente:<br /><br />-creatednsdelegation:$false|  

### <a name="promotion-and-demotion-failure-codes"></a>Códigos de error de promoción y degradación

La promoción y la degradación devuelven los siguientes códigos de mensaje de error. Además, es probable que se trate de un mensaje de error extendido; lee siempre todo el error, no solo la parte numérica.  


| Código de error |                                                           Explicación                                                            |                                                                                                                            Solución recomendada                                                                                                                            |
|------------|----------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     11     |                                          La promoción de controladores de dominio ya está ejecutándose                                          |                                                                                 No ejecutes de manera simultánea más de una instancia de promoción de controladores de dominio para el mismo equipo de destino                                                                                  |
|     12     |                                                    El usuario debe ser administrador                                                    |                                                                                        Inicia sesión como miembro del grupo Administradores predefinido y eleva los permisos con UAC                                                                                        |
|     13     |                                               La entidad de certificación está instalada                                               | No se puede degradar este controlador de dominio, ya que también es una entidad de certificación. No elimines la CA sin haber realizado un inventario detallado de su uso; si emite certificados, al eliminar el rol se producirá una interrupción. No ejecutes CA en controladores de dominio |
|     14     |                                                    Ejecución en modo de arranque seguro                                                     |                                                                                                                      Arranca el servidor en modo normal                                                                                                                      |
|     15     |                                            Hay un cambio de rol en curso o es necesario reiniciar                                            |                                                                                             Es necesario reiniciar el servidor (debido a cambios anteriores en la configuración) antes de la promoción                                                                                              |
|     16     |                                                    Ejecución en la plataforma incorrecta                                                     |                                                                                                                       *No es probable que obtenga este error*                                                                                                                       |
|     17     |                                                      No existe ninguna unidad NTFS 5                                                      |                                                                            Este error no puede producirse en Windows Server 2012 ya que, como mínimo, %systemdrive% debe tener formato NTFS                                                                             |
|     18     |                                                    No hay suficiente espacio en windir                                                    |                                                                                                        Libera espacio en el volumen %systemdrive% mediante cleanmgr.exe                                                                                                        |
|     19     |                                                Cambio de nombre pendiente, es necesario reiniciar                                                 |                                                                                                                             Reinicia el servidor                                                                                                                              |
|     20     |                                                 El nombre de equipo usa una sintaxis no válida                                                  |                                                                                                                   Cambia el nombre del equipo por un nombre válido                                                                                                                    |
|     21     |                             Este controlador de dominio contiene roles de FSMO, es un GC o es un servidor DNS                             |                                                                                                      Agregar **- demoteoperationmasterrole** al usar **- forceremoval**.                                                                                                      |
|     22     |                                        TCP/IP no está instalado o no funciona                                         |                                                                                                    Comprueba que TCP/IP esté configurado, vinculado y que funcione correctamente en el equipo                                                                                                     |
|     23     |                                             Es necesario configurar primero el cliente DNS                                              |                                                                                                  Establece un servidor DNS principal al agregar un nuevo controlador de dominio a un dominio                                                                                                  |
|     24     |                                  Las credenciales suministradas no son válidas o carecen de elementos que son obligatorios                                   |                                                                                                               Comprueba que el nombre de usuario y la contraseña sean correctos                                                                                                                |
|     25     |                                 No se pudo encontrar ningún controlador de dominio para el dominio especificado                                  |                                                                                                                Valida la configuración del cliente DNS y las reglas de firewall                                                                                                                |
|     26     |                                        No se pudo leer la lista de dominios en el bosque                                         |                                                                                                      Comprueba que la configuración del cliente DNS, el funcionamiento de LDAP y las reglas de firewall sean correctos                                                                                                      |
|     27     |                                                       Falta el nombre de dominio                                                        |                                                                                                                Especifica un dominio al promocionar o degradar                                                                                                                 |
|     28     |                                                         El nombre de dominio no es válido                                                          |                                                                                                          Elige un nombre de dominio DNS válido que sea distinto al promocionar                                                                                                          |
|     29     |                                                   El dominio primario no existe                                                   |                                                                                             Comprueba el dominio primario especificado al crear un nuevo dominio secundario o dominio de árbol                                                                                             |
|     30     |                                                       El dominio no se encuentra en el bosque                                                       |                                                                                                                      Comprueba el nombre de dominio especificado                                                                                                                       |
|     31     |                                                   El dominio secundario ya existe                                                    |                                                                                                                      Especifica un nombre de dominio distinto                                                                                                                       |
|     32     |                                                     El nombre de dominio NetBIOS es incorrecto                                                      |                                                                                                                    Especifica un nombre de dominio NetBIOS válido                                                                                                                     |
|     33     |                                                 La ruta de acceso a los archivos IFM no es válida                                                 |                                                                                                            Valida la ruta de acceso a la carpeta de Instalar desde el medio                                                                                                             |
|     34     |                                                     La base de datos de IFM está dañada                                                      |                                                          Usa la versión correcta de Instalar desde el medio para el sistema operativo y el rol (la misma versión del sistema operativo y el mismo tipo de controlador de dominio, RODC o RWDC)                                                          |
|     35     |                                                          No se encuentra SYSKEY                                                          |                                                                                             Instalar desde el medio está cifrado y es necesario proporcionar una SYSKEY válida para usarla                                                                                              |
|     37     |                                          La ruta de acceso a la base de datos NTDS o sus registros ya no es válida                                           |                                                                                          Cambia la ruta de acceso a la base de datos y de los registros a un volumen NTFS fijo, no a una unidad asignada o a una ruta de acceso UNC                                                                                           |
|     38     |                                   El volumen no tiene espacio suficiente para la base de datos NTDS o para los registros de la base de datos                                    |                                                                              Libera espacio mediante cleanmgr.exe, agrega más espacio en disco o libera espacio de forma manual (transfiere los datos innecesarios a otra ubicación)                                                                              |
|     39     |                                                    La ruta de acceso a SYSVOL no es válida                                                    |                                                                                            Cambia la ruta de acceso de la carpeta SYSVOL a un volumen NTFS fijo, no a una unidad asignada o a una ruta de acceso UNC                                                                                             |
|     40     |                                                        El nombre de sitio no es válido                                                         |                                                                                                                      Proporciona un nombre de sitio existente                                                                                                                       |
|     41     |                                             Es necesario especificar una contraseña para el modo seguro                                             |                                                                                Especifica una contraseña para la cuenta de DSRM (el campo no puede estar en blanco, independientemente de la configuración de la directiva de contraseñas)                                                                                 |
|     42     |                                    La contraseña de modo seguro no cumple con los criterios (solo promoción)                                    |                                                                                         Proporciona una contraseña para la cuenta de DSRM que cumpla con las reglas configuradas de la directiva de contraseñas                                                                                          |
|     43     |                                      La contraseña de administrador no cumple con los criterios (solo degradación)                                       |                                                                                  Especifica una contraseña para la cuenta de administrador local que cumpla con las reglas configuradas de la directiva de contraseñas                                                                                  |
|     44     |                                           El nombre de bosque especificado no es válido                                           |                                                                                                                Especifica un nombre de dominio DNS raíz de bosque válido                                                                                                                 |
|     45     |                                         Ya existe un bosque con el nombre especificado                                          |                                                                                                               Elige un nombre de dominio DNS raíz de bosque distinto                                                                                                               |
|     46     |                                            El nombre especificado para el árbol no es válido                                            |                                                                                                                    Especifica un nombre de dominio DNS de árbol válido                                                                                                                    |
|     47     |                                          Ya existe un árbol con el nombre especificado                                           |                                                                                                                  Elige un nombre de dominio DNS de árbol distinto                                                                                                                   |
|     48     |                                       El nombre de árbol no se ajusta a la estructura del bosque                                       |                                                                                                                  Elige un nombre de dominio DNS de árbol distinto                                                                                                                   |
|     49     |                                               El dominio especificado no existe                                                |                                                                                                                       Verifica el nombre de dominio especificado                                                                                                                        |
|     50     | Durante la degradación se detectó el último controlador de dominio (aunque no lo era), o bien se especificó el último controlador de dominio (pero no lo es) |        No especifiques el **último controlador de dominio en el dominio** ( **-lastdomaincontrollerindomain**) a menos que sea cierto. Use **- ignorelastdcindomainmismatch** debe reemplazar si esto es realmente el último controlador de dominio y no hay metadatos de controladores de dominio fantasma        |
|     51     |                                          En este controlador de dominio existen particiones de aplicación                                          |                                                                                              Especifica **Quitar particiones de aplicación** ( **-removeapplicationpartitions**)                                                                                               |
|     52     |            No se encuentra el argumento de línea de comandos obligatorio (es decir, es necesario especificar un archivo de respuesta en la línea de comandos)             |                                                                                              *Solo se ven con dcpromo / unattend, que está en desuso. Consulte la documentación anterior*                                                                                              |
|     53     |                               Se produjo un error en la promoción o degradación y es necesario reiniciar el equipo para eliminar los archivos innecesarios                                |                                                                                                                    Examina el error extendido y los registros                                                                                                                     |
|     54     |                                                  Error de promoción o degradación                                                   |                                                                                                                    Examina el error extendido y los registros                                                                                                                     |
|     55     |                                         El usuario canceló la promoción o degradación                                          |                                                                                                                    Examina el error extendido y los registros                                                                                                                     |
|     56     |                      El usuario canceló la promoción o degradación; es necesario reiniciar el equipo para eliminar los archivos innecesarios                       |                                                                                                                    Examina el error extendido y los registros                                                                                                                     |
|     58     |                                       Debe especificarse un nombre de sitio durante la promoción RODC                                        |                                                                                           Es necesario especificar un sitio para un RODC; no detectará automáticamente uno como un RWDC                                                                                           |
|     59     |                        Durante la degradación, este controlador de dominio es el último servidor DNS para una de sus zonas                         |                                                                                    Especifica que este es el **último servidor DNS del dominio** o usa **-ignorelastdnsserverfordomain**                                                                                     |
|     60     |         Para promocionar RODC se necesita un controlador de dominio que ejecute Windows Server 2008 o posterior          |                                                                                             Promociona como mínimo un controlador de dominio que permita la escritura y que ejecute Windows Server 2008 o posterior                                                                                             |
|     61     |        No se puede instalar Active Directory Domain Services con DNS en un dominio existente que no hospede DNS         |                                                                                                                      *No se puede obtener este error*                                                                                                                      |
|     62     |                                         El archivo de respuesta no tiene una sección [DCInstall]                                          |                                                                                             *Solo se ven con dcpromo / unattend, que está en desuso. Consulte la documentación anterior.*                                                                                              |
|     63     |                                       El nivel funcional del bosque es anterior a Windows Server 2003                                       |                                                            Eleva el nivel funcional del bosque como mínimo a Windows Server 2003 nativo. Los sistemas operativos Windows 2000 y Windows NT 4.0 ya no son compatibles                                                             |
|     64     |                                      Error de promoción debido a que no se encontró el archivo binario del componente                                      |                                                                                                                           Instala el rol de AD DS                                                                                                                           |
|     65     |                                    Error de promoción debido a que no se instaló el archivo binario del componente                                     |                                                                                                                           Instala el rol de AD DS                                                                                                                           |
|     66     |                                      Error de promoción debido a que no se encontró el sistema operativo                                      |                                  Examina el error extendido y los registros; el servidor no devuelve la versión del sistema operativo. Puede que sea necesario volver a instalar el equipo, ya que el estado global es muy sospechoso                                   |
|     68     |                                                  El asociado de replicación no es válido                                                  |                                                                             Usa repadmin.exe o la **Get-ADReplication\\**  \* Windows PowerShell para validar el estado del controlador de dominio asociado                                                                              |
|     69     |                                    El puerto requerido ya está en uso por otra aplicación                                     |                                                                                    Usa **netstat.exe -anob** para identificar los procesos que se han asignado de forma incorrecta a los puertos reservados para AD DS                                                                                     |
|     70     |                                          El controlador de dominio raíz del bosque debe ser un GC                                          |                                                                                              *Solo se ven con dcpromo / unattend, que está en desuso. Consulte la documentación anterior*                                                                                              |
|     71     |                                                 El servidor DNS ya está instalado                                                  |                                                                                          No especifiques la instalación de DNS ( **-installDNS**) si el servicio DNS ya está instalado                                                                                           |
|     72     |                                  El equipo está ejecutando Servicios de Escritorio remoto en modo de no administrador                                   |        No se puede promocionar este controlador de dominio, ya que también es un servidor RDS configurado para más de dos usuarios administradores. No elimines RDS sin haber realizado un inventario detallado de su uso; si lo están usando aplicaciones o usuarios finales, al eliminarlo se producirá una interrupción         |
|     73     |                                        El nivel funcional del bosque especificado no es válido.                                         |                                                                                                                  Especifica un nivel funcional de bosque válido                                                                                                                   |
|     74     |                                        El nivel funcional del dominio especificado no es válido.                                         |                                                                                                                  Especifica un nivel funcional de dominio válido                                                                                                                   |
|     75     |                                   No se puede determinar la directiva de replicación de contraseñas predeterminada.                                   |                                                                                                Comprueba que existe la directiva de replicación de contraseñas RODC y que es accesible                                                                                                 |
|     76     |                                 Los grupos de seguridad especificados, tanto replicados como no, no son válidos                                  |                                                                                Comprueba que has escrito el dominio y las cuentas de usuario correctos al especificar una directiva de replicación de contraseñas                                                                                |
|     77     |                                                El argumento especificado no es válido                                                 |                                                                                                                    Examina el error extendido y los registros                                                                                                                     |
|     78     |                                            Error al examinar el bosque de Active Directory                                             |                                                                                                                    Examina el error extendido y los registros                                                                                                                     |
|     79     |                                 RODC no se puede promocionar porque no se ha ejecutado rodcprep                                  |                                                                                               Usa Windows Server 2012 para preparar el bosque o bien usa **adprep.exe /rodcprep**                                                                                                |
|     80     |                                                No se ha ejecutado domainprep                                                 |                                                                                              Usa Windows Server 2012 para preparar el dominio o bien usa **adprep.exe /domainprep**                                                                                               |
|     81     |                                                No se ha ejecutado forestprep                                                 |                                                                                              Usa Windows Server 2012 para preparar el bosque o bien usa **adprep.exe /forestprep**                                                                                               |
|     82     |                                                      Error de coincidencia de esquema de bosque                                                      |                                                                                              Usa Windows Server 2012 para preparar el bosque o bien usa **adprep.exe /forestprep**                                                                                               |
|     83     |                                                         SKU no compatible                                                          |                                                                                                                       *No es probable que obtenga este error*                                                                                                                       |
|     84     |                                           No se encontró ninguna cuenta de controlador de dominio                                           |                                                                                         Comprueba que los controladores de dominio existentes tienen configurado el conjunto de atributos correcto de control de cuentas de usuario.                                                                                         |
|     85     |                                     No se puede seleccionar ninguna cuenta de controlador de dominio para la fase 2                                     |                                                 Se produce si se especifica “Usar cuenta existente” pero no se encuentra ninguna cuenta o se produce un error durante la búsqueda de cuentas. Comprueba que la cuenta preconfigurada RODC que has indicado es la correcta                                                 |
|     86     |                                                  Es necesario ejecutar la promoción de la fase 2                                                   |                                                                       Se produce si se promociona un controlador de dominio adicional pero ya existe una cuenta y no se especificó la opción “Permitir reinstalación”                                                                       |
|     87     |                                      Existe una cuenta de controlador de dominio o un tipo en conflicto                                      |   Cambia el nombre del equipo antes de promocionarlo si no quieres conectarlo a un controlador de dominio no ocupado. Debe asociar a la cuenta de controlador de dominio no ocupado mediante **- useexistingaccount** y el argumento correcto de solo lectura o escritura, dependiendo del tipo de cuenta    |
|     88     |                                             El administrador del servidor especificado no es válido                                              |                                                                           Has especificado una cuenta no válida para la delegación de administración de RODC. Comprueba que la cuenta especificada sea de un usuario o grupo válido                                                                           |
|     89     |                                         El maestro RID del dominio especificado está sin conexión.                                          |                                                                 Usa **netdom.exe query fsmo** para detectar el maestro RID. Haz que esté en línea y accesible al controlador de dominio que vayas a promocionar                                                                  |
|     90     |                                                 El maestro de nomenclatura de dominios está sin conexión.                                                 |                                                            Usa **netdom.exe query fsmo** para detectar el maestro de nomenclatura de dominios. Haz que esté en línea y accesible al controlador de dominio que vayas a promocionar                                                             |
|     91     |                                             No se pudo detectar si el proceso es wow64                                             |                                                                                                  *No se puede obtener este error, el sistema operativo es 64 bits*                                                                                                  |
|     92     |                                                  El proceso wow64 no es compatible                                                  |                                                                                                  *No se puede obtener este error, el sistema operativo es 64 bits*                                                                                                  |
|     93     |                                El servicio de controlador de dominio no está ejecutándose para degradación no forzosa                                |                                                                                                                          Inicie el servicio AD DS                                                                                                                           |
|     94     |                           La contraseña de administrador local no cumple con los requisitos: en blanco o no requerida                           |                                                                                         Proporciona una contraseña que no esté en blanco y comprueba que la directiva de contraseñas local requiera una contraseña                                                                                         |
|     95     |              No se puede degradar el último controlador de dominio de Windows Server 2008 o posterior en el dominio donde existen los RODC              |                                                                             Es necesario degradar todos los RODC antes de degradar los controladores de dominio que permitan la escritura y que ejecuten Windows Server 2008 o posterior                                                                             |
|     96     |                                                 Error al desinstalar los archivos binarios de DS                                                  |                                                                                              *Solo se ven con dcpromo / unattend, que está en desuso. Consulte la documentación anterior*                                                                                              |
|     97     |                      La versión del nivel funcional del bosque es superior a la del sistema operativo del dominio secundario                       |                                                                                           Especifica un dominio secundario funcional que sea igual o superior al nivel funcional del bosque                                                                                            |
|     98     |                                        La instalación o desinstalación del archivo binario del componente está en curso.                                        |                                                                                              *Solo se ven con dcpromo / unattend, que está en desuso. Consulte la documentación anterior*                                                                                              |
|     99     |                              El nivel funcional del bosque es demasiado bajo (el error solo se produce en Windows Server 2012)                              |                                                            Eleva el nivel funcional del bosque como mínimo a Windows Server 2003 nativo. Los sistemas operativos Windows 2000 y Windows NT 4.0 ya no son compatibles                                                             |
|    100     |                              El nivel funcional del dominio es demasiado bajo (el error solo se produce en Windows Server 2012)                              |                                                            Eleva el nivel funcional del dominio como mínimo hasta Windows Server 2003 nativo. Los sistemas operativos Windows 2000 y Windows NT 4.0 ya no son compatibles                                                             |

## <a name="known-issues-and-common-support-scenarios"></a>Problemas conocidos y escenarios comunes de soporte técnico

Los siguientes son problemas habituales que se observan durante el proceso de desarrollo de Windows Server 2012. Todos ellos son problemas debidos al diseño, y tienen una solución válida o una técnica más apropiada para evitar que se produzcan. Muchos de estos comportamientos son idénticos en Windows Server 2008 R2 y sistemas operativos anteriores; sin embargo, al reescribir la implementación de AD DS ha aumentado la frecuencia de los problemas.  

|Problema|Al degradar un controlador de dominio, el DNS se ejecuta sin zonas|  
|---------|-----------------------------------------------------------------|  
|Síntomas|El servidor sigue respondiendo a las consultas DNS, pero no posee información de zona|  
|Solución y notas|Al eliminar el rol de AD DS, también se elimina el rol de servidor DNS o se establece el servicio Servidor DNS como deshabilitado. No te olvides de apuntar el cliente DNS a un servidor distinto. Si usas Windows PowerShell, ejecuta lo siguiente antes de degradar el servidor:<br /><br />Código - dns-windowsfeature desinstalar<br /><br />o bien<br /><br />Código - set-service dns - starttype deshabilitado<br />detener el servicio dns|  

|Problema|Al promocionar un equipo con Windows Server 2012 en un dominio de etiqueta única existente, no se configura updatetopleveldomain=1 ni allowsinglelabeldnsdomain=1|  
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|  
|Síntomas|No se produce el registro dinámico DNS|  
|Solución y notas|Establece estos valores mediante Netlogon y las directivas de grupo de DNS. Microsoft comenzó a bloquear la creación de dominios de etiqueta única en Windows Server 2008; se puede usar ADMT o la herramienta de cambio de nombre de dominio para cambiar a una estructura de dominio DNS aprobada.|  

|Problema|La degradación del último controlador de dominio en un dominio produce errores si existen cuentas RODC creadas anteriormente y sin ocupar|  
|---------|------------------------------------------------------------------------------------------------------------|  
|Síntomas|Error de degradación con el mensaje:<br /><br />**Dcpromo.General.54**<br /><br />Active Directory Domain Services no pudo encontrar otro controlador de dominio de Active Directory para transferir los datos restantes en la partición de directorio CN=Schema,CN=Configuration,DC=corp,DC=contoso,DC=com.<br /><br />“El formato del nombre de dominio especificado no es válido”.|  
|Solución y notas|Elimina el resto de cuentas RODC existentes antes de degradar un dominio mediante **Dsa.msc** o **Ntdsutil.exe metadata cleanup**.|  

|Problema|Al preparar automáticamente el bosque y el dominio no se ejecuta GPPREP|  
|---------|---------------------------------------------------------------|  
|Síntomas|La función de planificación entre dominios para Directiva de grupo, modo de planificación Conjunto resultante de directivas (RSOP), requiere que estén actualizados el sistema de archivos y los permisos de Active Directory para los GP existentes. Sin Gpprep, no se puede usar la planificación de RSOP entre dominios.|  
|Solución y notas|Ejecuta **adprep.exe /gpprep** manualmente para todos los dominios que Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 no prepararon anteriormente. Los administradores solo deben ejecutar GPPrep una vez en el historial de un dominio, no en cada actualización. No se ejecuta con el adprep automático porque, en caso de haber establecido los permisos personalizados correctos, haría que todos los contenidos de SYSVOL volvieran a replicarse en todos los controladores de dominio.|  

|Problema|Error de verificación de Instalar desde el medio al apuntar a una ruta de acceso UNC|  
|---------|------------------------------------------------------------------|  
|Síntomas|Errores devueltos:<br /><br />Código: no se pudo validar la ruta de acceso de medios. Excepción al llamar a "GetDatabaseInfo" con "2" argumentos. La carpeta no es válida.|  
|Solución y notas|Almacena los archivos IFM en un disco local, no en una ruta de acceso UNC remota. Este bloqueo intencional evita la promoción parcial de servidores debido a una interrupción de la red.|  

|Problema|La advertencia delegación DNS se muestra dos veces durante la promoción de controladores de dominio|  
|---------|-------------------------------------------------------------------------|  
|Síntomas|Advertencia devuelta *dos veces* al realizar promociones mediante addsdeployment para Windows PowerShell:<br /><br />Código - "no se puede crear una delegación para este servidor DNS porque la zona principal autoritativa no se encuentra o no se ejecuta Windows DNS server. Si está integrando con una infraestructura DNS existente, debe crear manualmente una delegación a este servidor DNS en la zona primaria para garantizar una resolución de nombres confiable desde fuera del dominio. En caso contrario, se requiere ninguna acción."|  
|Solución y notas|Ignóralo. ADDSDeployment para Windows PowerShell muestra la misma advertencia durante la comprobación de requisitos previos y durante la configuración del controlador de dominio. Si no quieres configurar la delegación DNS, usa el argumento:<br /><br />Código - de - creatednsdelegation: $false<br /><br />*No* omitas las comprobaciones de requisitos previos para suprimir este mensaje|  

|Problema|Al especificar UPN o credenciales que no pertenecen al dominio durante la configuración se producen errores confusos|  
|---------|--------------------------------------------------------------------------------------------|  
|Síntomas|El Administrador del servidor devuelve el error:<br /><br />Código: excepción al llamar a "DNSOption" con argumentos "6"<br /><br />ADDSDeployment para Windows PowerShell devuelve el error:<br /><br />Error de código: comprobación de permisos de usuario. Debe proporcionar el nombre del dominio al que pertenece esta cuenta de usuario.|  
|Solución y notas|Comprueba que las credenciales de dominio que has proporcionado sean válidas (con el formato **dominio\usuario**).|  

|Problema|Si se elimina el rol DirectoryServices-DomainController al usar Dism.exe, el servidor no arrancará|  
|---------|---------------------------------------------------------------------------------------------------|  
|Síntomas|Si se usa Dism.exe para eliminar el rol de AD DS antes de degradar un controlador de dominio correctamente, el servidor ya no arrancará de manera normal y se mostrará el error:<br /><br />Código - estado: 0x000000000<br />Info: Se ha producido un error inesperado.|  
|Solución y notas|Arranca en el Modo de reparación de servicios de directorio con *Mayús + F8*. Vuelve a agregar el rol de AD DS y después fuerza la degradación del controlador de dominio. Como alternativa, restaura el estado del sistema a partir de una copia de seguridad. No uses Dism.exe para la eliminación del rol de AD DS, ya que dicha utilidad no reconoce los controladores de dominio.|  

|Problema|Error al instalar un nuevo bosque si se establece el modo de bosque en Win2012|  
|---------|--------------------------------------------------------------------|  
|Síntomas|Al promocionar mediante ADDSDeployment para Windows PowerShell, se produce el error:<br /><br />Code -  Test.VerifyDcPromoCore.DCPromo.General.74<br /><br />Error de comprobación de requisitos previos para la promoción del controlador de dominio. El nivel funcional del dominio especificado no es válido|  
|Solución y notas|No especifiques un modo funcional de bosque de Win2012 sin especificar *también* un modo funcional de dominio de Win2012. En este ejemplo funcionará sin errores:<br /><br />Código - de - forestmode Win2012 - domainmode Win2012]|  

|||  
|-|-|  
|Problema|No ocurre nada al hacer clic en Verificar en el área de selección de Instalar desde el medio|  
|Síntomas|Si se especifica una ruta de acceso a una carpeta de IFM, al hacer clic en el botón **Verificar** no ocurre nada ni se muestra ningún mensaje.|  
|Solución y notas|El botón **Verificar** solo devuelve errores si existen problemas. En caso contrario, activa el botón **Siguiente** siempre que hayas proporcionado una ruta de acceso de IFM. Haz clic en **Verificar** para continuar en caso de haber seleccionado IFM.|  

|||  
|-|-|  
|Problema|Al realizar una degradación con el Administrador del servidor, no se obtienen mensajes de estado hasta que se completa.|  
|Síntomas|Al usar el Administrador del servidor para quitar el rol de AD DS y degradar un controlador de dominio, no se producen mensajes de estado hasta que la degradación se completa o da error.|  
|Solución y notas|Esta es una limitación del Administrador del servidor. Para recibir mensajes de estado, usa el cmdlet de ADDSDeployment para Windows PowerShell:<br /><br />Código - addsdomaincontroller desinstalar|  

|||  
|-|-|  
|Problema|En Instalar desde el medio, la opción Verificar no detecta el medio RODC proporcionado para el controlador de dominio que permite la escritura, o viceversa.|  
|Síntomas|Al promocionar un nuevo controlador de dominio con IFM y proporcionar el medio incorrecto a un IFM (como un medio RODC para un controlador de dominio que permita la escritura, o bien un medio RWDC para un RODC), el botón Verificar no devuelve ningún error. A continuación, la promoción produce el error siguiente:<br /><br />Código: se produjo un error al intentar configurar este equipo como un controlador de dominio. <br />La promoción de la instalación desde el medio de un controlador de dominio de solo lectura no se puede iniciar porque no se permite la base de datos de origen especificado. Solo las bases de datos desde otros RODC pueden usarse para la promoción de IFM de un RODC.|  
|Solución y notas|Verificar solo valida la integridad general de IFM. Asegúrate de proporcionar siempre el tipo de IFM correcto a un servidor. Reinicia el servidor antes de volver a intentar la promoción con el medio correcto.|  

|||  
|-|-|  
|Problema|Error al promocionar un RODC en una cuenta de equipo existente|  
|Síntomas|Al usar ADDSDeployment para Windows PowerShell para promocionar un nuevo RODC con una cuenta de equipo preconfigurada, se produce el siguiente error:<br /><br />Código - conjunto de parámetros no se puede resolver mediante los parámetros con nombre especificados.    <br />InvalidArgument: ParameterBindingException<br />    + FullyQualifiedErrorId: AmbiguousParameterSet,Microsoft.DirectoryServices.Deployment.PowerShell.Commands.Install|  
|Solución y notas|No proporciones parámetros predefinidos en una cuenta RODC existente. Entre ellos se incluyen los siguientes:<br /><br />Código - de - readonlyreplica<br />-installdns<br />-donotconfigureglobalcatalog<br />-sitename<br />-installdns|  

|||  
|-|-|  
|Problema|No ocurre nada al seleccionar o anular la selección de la opción “Reiniciar automáticamente el servidor de destino en caso necesario”|  
|Síntomas|Si selecciona (o no) la opción del administrador del servidor **reiniciar el servidor de destino automáticamente si es necesario** whendemoting un controlador de dominio mediante la eliminación de roles, el servidor siempre se reinicia, independientemente de elección.|  
|Solución y notas|Este es el comportamiento esperado. El proceso de degradación reinicia el servidor, independientemente de la opción seleccionada.|  

|||  
|-|-|  
|Problema|En el registro Dcpromo.log se muestra “[error] no se pudo establecer la seguridad en archivos de servidor debido al error 2”|  
|Síntomas|La degradación de un controlador de dominio se completa sin problemas; sin embargo, al examinar el registro de dcpromo aparece el siguiente error:<br /><br />Código - [error] configurar la seguridad de archivos del servidor debido al error 2|  
|Solución y notas|Ignora el error, ya que es el comportamiento esperado (de carácter informativo).|  

|||  
|-|-|  
|Problema|La comprobación de requisitos previos de adprep produce el error “No se puede realizar la comprobación de conflicto de esquema de Exchange”|  
|Síntomas|Al intentar promocionar un controlador de dominio de Windows Server 2012 en un bosque existente de Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2, la comprobación de requisitos previos produce el siguiente error:<br /><br />Error de código: comprobación de requisitos previos para la preparación de AD. No se puede realizar la comprobación de conflicto de esquema de Exchange para dominio *<domain name>* (excepción: el servidor RPC no está disponible)<br /><br />En el archivo adprep.log aparece el error:<br /><br />Código - Adprep no pudo recuperar datos desde el servidor *<domain controller>*<br /><br />a través de Instrumental de administración de Windows (WMI).|  
|Solución y notas|El nuevo controlador de dominio no puede acceder a WMI con los protocolos DCOM o RPC contra los controladores de dominio existentes. Se conocen tres causas del error:<br /><br />-Un firewall regla bloquea el acceso a los controladores de dominio existente<br /><br />-Falta de "Inicio de sesión como servicio" la cuenta de servicio de red con privilegios (SeServiceLogonRight) en los controladores de dominio existente<br /><br />-NTLM está deshabilitado en controladores de dominio, mediante las directivas de seguridad descritas en [Introducción a la restricción de la autenticación NTLM](https://technet.microsoft.com/library/dd560653(WS.10).aspx)|  

|||  
|-|-|  
|Problema|Al crear un nuevo bosque de AD DS, siempre se muestra una advertencia sobre DNS|  
|Síntomas|Al crear un nuevo bosque de AD DS y crear la zona DNS en el nuevo controlador de dominio para sí mismo, siempre se muestra el siguiente mensaje de advertencia:<br /><br />Se detectó código - de un error en la configuración de DNS. <br />Ninguno de los servidores DNS usados por este equipo ha respondido dentro del intervalo de tiempo de espera.<br />(código de error 0x000005B4 "ERROR_TIMEOUT")|  
|Solución y notas|Ignóralo. Esta advertencia es intencional en el primer controlador de dominio en el dominio raíz de un bosque nuevo, siempre que apuntes a un servidor y a una zona DNS existentes.|  

|||  
|-|-|  
|Problema|El argumento -whatif de Windows PowerShell devuelve información incorrecta sobre el servidor DNS|  
|Síntomas|Si usas el argumento **-whatif** al configurar un controlador de dominio con la opción **-installdns:$true**, ya sea de forma implícita o explícita, se muestra la siguiente salida:<br /><br />Código: "servidor DNS: No"|  
|Solución y notas|Ignóralo. DNS está instalado y configurado correctamente.|  

|||  
|-|-|  
|Problema|Después de la promoción no se puede iniciar sesión debido al error “Espacio de almacenamiento insuficiente para procesar este comando”|  
|Síntomas|Después de promocionar un nuevo controlador de dominio, si cierras la sesión e intentas volver a iniciarla, se produce el siguiente error:<br /><br />Código: no hay suficiente almacenamiento disponible para procesar este comando|  
|Solución y notas|El controlador de dominio no se reinició después de la promoción, ya sea debido a un error o porque se especificó el argumento **-norebootoncompletion** de ADDSDeployment para Windows PowerShell. Reinicia el controlador de dominio.|  

|||  
|-|-|  
|Problema|el botón Siguiente no está disponible en la página Opciones del controlador de dominio|  
|Síntomas|Incluso después de establecer una contraseña, no está disponible el botón **Siguiente** en la página **Opciones del controlador de dominio** del Administrador del servidor. No aparece ningún sitio en el menú **Nombre de sitio**.|  
|Solución y notas|Tienes varios sitios de AD DS y faltan las subredes por lo menos en uno de ellos; este controlador de dominio futuro pertenece a una de estas subredes. Selecciona de forma manual la subred en el menú desplegable Nombre de sitio. Además, también debes revisar todos los sitios de AD mediante DSSITE.MSC, o bien usar el comando siguiente de Windows PowerShell para identificar todos los sitios en los que faltan subredes:<br /><br />Código - get-adreplicationsite-filtro \* -subredes de la propiedad &#124; where-object {! $_.subnets - eq "\*"} &#124; nombre de formato de tabla|  

|||  
|-|-|  
|Problema|Error de promoción o degradación con el mensaje “No se puede iniciar el servicio”|  
|Síntomas|Al intentar realizar una promoción, degradación o clonación de un controlador de dominio, se produce el error:<br /><br />Código: no se puede iniciar el servicio, ya sea porque está deshabilitado o tiene dispositivos habilitados asociados con él"(0 x 80070422)<br /><br />El error puede ser interactivo, un evento o bien aparecer escrito en un registro como dcpromoui.log o dcpromo.log|  
|Solución y notas|El servicio Servidor de roles de DS (DsRoleSvc) está deshabilitado. De manera predeterminada, este servicio se instala durante la instalación del rol de AD DS y se establece como tipo de inicio manual. No deshabilites este servicio. Establécelo en Manual y permite que las operaciones de roles de DS se inicien y se detengan a petición. Este comportamiento es así por diseño.|  

|||  
|-|-|  
|Problema|El Administrador del servidor sigue advirtiendo de que es necesario promocionar DC|  
|Síntomas|Si promocionas un controlador de dominio con la opción /unattend de dcpromo.exe (en desuso) o actualizas de forma local un controlador de dominio de Windows Server 2008 R2 a Windows Server 2012, el Administrador del servidor seguirá mostrando la tarea de configuración posterior a la implementación **Promover este servidor a controlador de dominio**.|  
|Solución y notas|Haz clic en el vínculo de advertencia posterior a la implementación para que se deje de mostrar el mensaje. Este es el comportamiento esperado (de carácter informativo).|  

|||  
|-|-|  
|Problema|Falta la instalación del rol en el script de implementación del Administrador del servidor|  
|Síntomas|Si promocionas un controlador de dominio con el Administrador del servidor y guardas el script de implementación de Windows PowerShell, no se incluirá el cmdlet de instalación de roles ni sus argumentos (install-windowsfeature -name ad-domain-services -includemanagementtools). No se puede configurar DC sin el rol.|  
|Solución y notas|Agrega de forma manual el cmdlet y los argumentos a los scripts. Este comportamiento es el esperado.|  

|||  
|-|-|  
|Problema|El nombre del script de implementación del Administrador del servidor no es PS1|  
|Síntomas|Si promocionas un controlador de dominio con el Administrador del servidor y guardas el script de implementación de Windows PowerShell, el archivo recibirá un nombre temporal aleatorio y, por lo tanto, no será un archivo PS1.|  
|Solución y notas|Cambia el nombre del archivo de forma manual. Este comportamiento es el esperado.|  

|Problema|Dcpromo /unattend permite niveles funcionales no compatibles|  
|-|-|  
|Síntomas|Si promocionas un controlador de dominio con dcpromo /unattend con el siguiente archivo de respuesta de muestra:<br /><br />Código:<br /><br />[DCInstall]<br />NewDomain=Forest<br /><br />ReplicaOrNewDomain=Domain<br /><br />NewDomainDNSName=corp.contoso.com<br /><br />SafeModeAdminPassword=Safepassword@6<br /><br />DomainNetbiosName=corp<br /><br />DNSOnNetwork = Yes<br /><br />AutoConfigDNS=Yes<br /><br />RebootOnSuccess=NoAndNoPromptEither<br /><br />RebootOnCompletion = No<br /><br />*DomainLevel=0*<br /><br />*ForestLevel=0*<br /><br />La promoción produce los errores siguientes en el archivo dcpromoui.log:<br /><br />Code - dcpromoui EA4.5B8 0089 13:31:50.783       Enter CArgumentsSpec::ValidateArgument DomainLevel<br /><br />Dcpromoui EA4.5B8 008A 13:31:50.783 valor para DomainLevel es 0<br /><br />Dcpromoui EA4.5B8 008B 13:31:50.783 código de salida es 77<br /><br />Dcpromoui EA4.5B8 008C 13:31:50.783 el argumento especificado no es válido.<br /><br />registro de Dcpromoui EA4.5B8 cierre de 13:31:50.783 008D<br /><br />Dcpromoui 0032 EA4.5B8 13:31:50.830 código de salida es 77<br /><br />El nivel 0 es Windows 2000, que no es compatible con Windows Server 2012.|  
|Solución y notas|No uses la opción en desuso dcpromo /unattend (ya que permite especificar opciones no válidas que generan errores). Este comportamiento es el esperado.|  

|Problema|Nunca se completa la promoción "bloqueos" a la creación de objeto de configuración NTDS,|  
|-|-|  
|Síntomas|Si promueve una réplica de DC o RODC, la promoción alcanza "crear objeto de configuración NTDS" y no continúa o se completa. Además, los registros también dejan de actualizarse.|  
|Solución y notas|Este es un problema conocido que se produce al proporcionar credenciales de la cuenta de administrador local predefinida con una contraseña que coincide con la cuenta de administrador de dominio predefinida. Esto causa un problema en el motor de instalación principal que no produce un error, sino que espera indefinidamente (tipo bucle). Esto es normal - aunque no deseados - comportamiento.<br /><br />Para corregir el problema del servidor:<br /><br />1.  Reinícialo.<br /><br />1.  En AD, elimina la cuenta de equipo del miembro de ese servidor (no todavía será una cuenta de controlador de dominio)<br /><br />1.  En dicho servidor, fuerza la separación del dominio<br /><br />1.  En dicho servidor, quita el rol de AD DS.<br /><br />1.  Reiniciar<br /><br />1.  Volver a agregar el AD DS rol y repite la promoción, lo que garantiza que siempre proporcione la ***dominio\admin*** con el formato de las credenciales para la promoción del controlador de dominio y no solo la cuenta de administrador local integrada|  