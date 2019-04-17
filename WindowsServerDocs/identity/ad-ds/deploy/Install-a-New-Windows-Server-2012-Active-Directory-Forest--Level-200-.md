---
ms.assetid: b3d6fb87-c4d4-451c-b3de-a53d2402d295
title: Instalar un bosque nuevo Windows Server 2012 Active Directory (nivel 200)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b8a7502a1b9d27b0f61353f2544182a64d311496
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-new-windows-server-2012-active-directory-forest-level-200"></a>Instalar un bosque nuevo Windows Server 2012 Active Directory (nivel 200)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema explica la característica de promoción nueva de controlador dominio de Active Directory Domain Services de Windows Server 2012 en un nivel de introducción. En Windows Server 2012, AD DS reemplaza la herramienta Dcpromo con un administrador del servidor y sistema de implementación basada en Windows PowerShell.  
  
-   [Simplificar la administración de servicios de dominio de Active Directory](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SimplifiedAdmin)  
  
-   [Información general técnica](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_TechOverview)  
  
-   [Implementación de un bosque con el administrador del servidor](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)  
  
-   [Implementación de un bosque con Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_PSForest)  
  
## <a name="BKMK_SimplifiedAdmin"></a>Simplificar la administración de servicios de dominio de Active Directory  
Windows Server 2012 presenta la próxima generación de Active Directory servicios simplificada administración del dominio y es el más radical dominio volver a una visión desde Windows 2000 Server. Administración simplificada de AD DS toma lecciones aprendidas de 12 años de Active Directory y hace que sea una experiencia más intuitiva administrativa más flexible, ofrece una mayor compatibilidad para los arquitectos y administradores. Esto significa crear nuevas versiones de tecnologías de existente, así como para expandir las funcionalidades de los componentes que se lanzó en Windows Server 2008 R2.  
  
### <a name="what-is-ad-ds-simplified-administration"></a>¿Qué es AD DS administración simplificada?  
Administración simplificada de AD DS es un reimagining de implementación de dominio. Algunas de estas características incluyen:  
  
-   Implementación de rol de AD DS ahora forma parte de la nueva arquitectura de administrador del servidor y permite la instalación remota.  
  
-   El motor de implementación y configuración de AD DS es ahora Windows PowerShell, incluso cuando uses una instalación gráfica.  
  
-   Promoción ahora incluye la comprobación de requisitos previos que valida la preparación de bosque y dominio para el nuevo controlador de dominio, reducir la posibilidad de que promociones erróneas.  
  
-   El bosque de Windows Server 2012 funcional nivel implementar nuevas características y el nivel funcional del dominio se requiere solo para un subconjunto de las nuevas características de Kerberos, lo que libera los administradores de las frecuentes necesita para un entorno de controlador de dominio homogéneo.  
  
### <a name="purpose-and-benefits"></a>Propósito y los beneficios  
Estos cambios pueden aparecer más complejos, no más simples. En rediseñar aunque el proceso de implementación de AD DS, había oportunidad de fusionar muchos de los pasos y procedimientos recomendados en acciones menos y más fáciles. Por ejemplo, esto significa que la configuración gráfica de un nuevo controlador de dominio de réplica es ahora ocho cuadros de diálogo, en lugar de los doce anterior. Crear un nuevo bosque de Active Directory requiere un *solo* comando de Windows PowerShell con solo *una* argumento: el nombre del dominio.  
  
¿Por qué hay énfasis en Windows PowerShell en Windows Server 2012? A medida que evoluciona de computación distribuida, Windows PowerShell permite un solo motor de configuración y mantenimiento de estos dos gráfica, interfaces de línea de comandos. Permite el scripting completo de cualquier componente con la misma nacionalidad de primera clase para el profesional de TI que una API que se concede a los desarrolladores. Como informática basada en la nube se vuelve ubicua, Windows PowerShell finalmente también ofrece la capacidad de administrar un servidor, donde un equipo sin interfaz gráfica tiene las mismas capacidades de administración que uno con un ratón y un monitor de forma remota.  
  
Un administrador de AD DS veterano debe buscar sus conocimientos anterior muy relevante. Un administrador del principio encontrará una curva de aprendizaje mucho menor.  
  
## <a name="BKMK_TechOverview"></a>Información general técnica  
  
### <a name="what-you-should-know-before-you-begin"></a>¿Qué debes saber antes de comenzar  
Este tema supone que estás familiarizado con las versiones anteriores de los servicios de dominio de Active Directory y no proporciona detalles básicos alrededor de su propósito y la funcionalidad. Para obtener más información acerca de AD DS, consulta las páginas de TechNet Portal siguientes:  
  
-   [Servicios de dominio de Active Directory para Windows Server 2008 R2](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
  
-   [Servicios de dominio de Active Directory para Windows Server 2008](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
  
-   [Referencia técnica de Windows Server](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
  
### <a name="functional-descriptions"></a>Descripciones funcionales  
  
#### <a name="ad-ds-role-installation"></a>Instalación de AD DS rol  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_SelectServerRoles.gif)  
  
Instalación de Active Directory los servicios de dominio, usa el administrador del servidor y Windows PowerShell, como todos los demás roles de servidor y características en Windows Server 2012. El programa Dcpromo.exe ya no proporciona opciones de configuración de la interfaz gráfica de usuario.  
  
Usar a un asistente gráfico en el administrador del servidor o el módulo ServerManager para Windows PowerShell en instalaciones locales y remotas. Si ejecuta varias instancias de los asistentes o los cmdlets y destinada a servidores diferentes, puedes implementar AD DS en varios controladores de dominio al mismo tiempo, todo desde una única consola. Aunque estas nuevas características no son compatibles con Windows Server 2008 R2 o sistemas operativos anteriores, también, aún puedes usar la aplicación de Dism.exe introducida en Windows Server 2008 R2 para la instalación del rol local desde la línea de comandos clásica.  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSAddWindowsFeature.png)  
  
#### <a name="ad-ds-role-configuration"></a>Configuración de AD DS rol  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
Configuración de servicios de dominio Directory activa "antes conocido como DCPROMO" es una operación discreta de instalación del rol de un momento. Después de instalar el rol de AD DS, un administrador configura el servidor como un controlador de dominio mediante un asistente independiente en el administrador del servidor o con el módulo de ADDSDeployment Windows PowerShell.  
  
Configuración del rol de AD DS se basa en 12 años de experiencia de campo y ahora configura los controladores de dominio en función de los procedimientos recomendados más recientes de Microsoft. Por ejemplo, el sistema de nombres de dominio y catálogos globales se instalan de forma predeterminada en todos los controladores de dominio.  
  
El Asistente para configuración del administrador del servidor de AD DS combina varios cuadros de diálogo individuales en menos de avisos y ya no oculta la configuración en un modo "avanzado". El proceso de promoción todo está en un cuadro de diálogo de expansión durante la instalación. El asistente y el módulo de PowerShell de Windows ADDSDeployment mostrarán cambios significativos y los problemas de seguridad, con vínculos a información adicional.  
  
La Dcpromo.exe permanece en Windows Server 2012 de línea de comandos instalaciones desatendidas y ya no se ejecuta al Asistente para instalación gráfica. Se recomienda encarecidamente que dejar de usar para las instalaciones desatendidas Dcpromo.exe y reemplazarla por el módulo ADDSDeployment, como el archivo ejecutable desuso ahora no se incluirá en la próxima versión de Windows.  
  
Estas nuevas características no son compatibles con versiones anteriores a Windows Server 2008 R2 o sistemas operativos anteriores.  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSForest.png)  
  
> [!IMPORTANT]  
> Dcpromo.exe ya no contiene a un asistente gráfico y ya no instala archivos binarios de rol o característica. Intenta ejecutar Dcpromo.exe desde el shell de explorador devuelve:  
>   
> "El Asistente para la instalación de los servicios de dominio de Active Directory se traslada en el administrador del servidor. Para obtener más información, consulta https://go.microsoft.com/fwlink/?LinkId=220921."  
>   
> Aún se intenta ejecutar Dcpromo.exe / unattend instala los archivos binarios, como se muestra en los sistemas operativos anteriores, pero advierte:  
>   
> "El dcpromo operación desatendida se reemplaza por el módulo de ADDSDeployment para Windows PowerShell. Para obtener más información, consulta https://go.microsoft.com/fwlink/?LinkId=220924."  
>   
> Windows Server 2012 deja de utilizar dcpromo.exe y no se incluye con las versiones futuras de Windows, ni recibirá aún más mejoras en este sistema operativo. Los administradores deben dejar de utilizar y cambiar a los módulos compatibles de Windows PowerShell si quieren crear controladores de dominio de la línea de comandos.  
  
#### <a name="prerequisite-checking"></a>Comprobación de requisitos previos  
Configuración del controlador de dominio también implementa una fase de comprobación de requisitos previa que da como resultado el bosque y dominio antes de continuar con la promoción del controlador de dominio. Esto incluye la disponibilidad de funciones FSMO, privilegios de usuario, compatibilidad de esquema extendido y otros requisitos. Este nuevo diseño reduce los problemas de dónde se inicia y, a continuación, detiene midway con un error de configuración grave promoción del controlador de dominio. Esto reduce la posibilidad de metadatos del controlador de dominio huérfana en el bosque o un servidor que se cree incorrectamente es un controlador de dominio.  
  
## <a name="BKMK_SMForest"></a>Implementación de un bosque con el administrador del servidor  
Esta sección explica cómo instalar el primer controlador de dominio en un dominio raíz del bosque con el administrador del servidor en un equipo de Windows Server 2012 gráfico.  
  
### <a name="server-manager-ad-ds-role-installation-process"></a>Proceso de instalación del rol de administrador del servidor AD DS  
El siguiente diagrama ilustra el proceso de instalación de la función de los servicios de dominio de Active Directory, ejecutando ServerManager.exe y finalizar justo antes de la promoción del controlador de dominio a partir de.  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment.png)  
  
#### <a name="server-pool-and-add-roles"></a>Servidor de grupo y agregar funciones  
Los equipos de Windows Server 2012 accesibles desde el equipo que ejecuta el administrador del servidor son apto para la agrupación. Una vez agrupado, selecciona los servidores para la instalación remota de AD DS u otras opciones de configuración posibles en el administrador del servidor.  
  
Para agregar servidores, elige una de las siguientes acciones:  
  
-   Haz clic en **agregar otros servidores para administrar** en la ventana bienvenida del panel  
  
-   Haz clic en el **administrar** menú y selecciona **agregar servidores**  
  
-   Haz clic en **todos los servidores** y elige **agregar servidores**  
  
Esto abre el cuadro de diálogo Agregar servidores:  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddServers.png)  
  
Esto te ofrece tres formas de agregar servidores al grupo para el uso o la agrupación:  
  
-   Búsqueda de Active Directory (usa LDAP, requiere que los equipos pertenecen a un dominio, permite que el filtrado del sistema operativo y admite caracteres comodín)  
  
-   Búsqueda DNS (usa alias DNS o la dirección IP a través de difusión ARP o NetBIOS o búsqueda WINS, no permite caracteres comodín de filtrado o soporte técnico del sistema operativo)  
  
-   Importar (usa una lista de servidores separados por rc / del archivo de texto)  
  
Haz clic en **Buscar ahora** para devolver una lista de servidores de ese mismo dominio de Active Directory que el equipo está unido a, haz clic en uno o más nombres de servidor de la lista de servidores. Haz clic en la flecha derecha para agregar los servidores para la **seleccionadas** lista. Usa el **agregar servidores** cuadro de diálogo para agregar servidores seleccionados a los grupos de rol de panel. O haz clic en **administrar**y, a continuación, haz clic en **crear grupo de servidores**, o haz clic en **crear grupo de servidores** en el panel **bienvenida al administrador del servidor** ventana para crear grupos de servidores personalizado.  
  
> [!NOTE]  
> El procedimiento de agregar servidores no valida que un servidor es accesible o en línea. Sin embargo, los servidores inaccesible marca en la vista de facilidad de uso en el administrador del servidor en la próxima actualización  
  
Puedes instalar roles de forma remota en cualquier equipo con Windows Server 2012 agregado equipos del grupo, como se muestra:  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/tADDS_SMI_TR_AddRolesFeatures.png)  
  
No se puede administrar completamente los servidores que ejecutan sistemas operativos anteriores a Windows Server 2012. La **agregar Roles y características** selección ejecuta el módulo de PowerShell de Windows ServerManager **Install-WindowsFeature**.  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddADDSToAnotherServer.png)  
  
También puedes usar el panel de administrador del servidor en un controlador de dominio existente para seleccionar la instalación de servidor remoto de AD DS con el rol ya preseleccionado haciendo clic en el icono del panel de AD DS y seleccionando **agregar AD DS a otro servidor**. Esto está invocando **Install-WindowsFeature servicios de dominio de AD**.  
  
El equipo en que está ejecutando el administrador del servidor sí mismo agrupa automáticamente. Para instalar el rol de AD DS aquí, simplemente haz clic en el **administrar** menú y haz clic en **agregar Roles y características**.  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ManageAddRoles.png)  
  
#### <a name="installation-type"></a>Tipo de instalación  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectInstallationType.png)  
  
La **tipo de instalación** cuadro de diálogo proporciona una opción que no es compatible con los servicios de dominio de Active Directory: la **escenario de servicios de escritorio remoto en función de instalación**. Esta opción solo permite el servicio Escritorio remoto en una carga de trabajo distribuido de varios servidor. Si lo selecciona, no puedes instalar AD DS.  
  
Siempre deje la selección predeterminada en su lugar, al instalar AD DS: **instalación basada en rol o característica**.  
  
#### <a name="server-selection"></a>Selección de servidor  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectDestinationServer.png)  
  
La **selección de servidor** cuadro de diálogo te permite elegir uno de los servidores que se agregó al grupo, siempre que es accesible. El servidor local con el administrador del servidor estará disponible automáticamente.  
  
Además, puedes seleccionar archivos VHD de Hyper-V sin conexión con el sistema operativo de Windows Server 2012 y el administrador del servidor, se agrega la función a ellos directamente a través de mantenimiento del componente. Esto le permite aprovisionar los servidores virtuales con los componentes necesarios antes de configurar más de ellos.  
  
#### <a name="server-roles-and-features"></a>Características y los Roles de servidor  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectServerRoles.png)  
  
Selecciona el **los servicios de dominio de Active Directory** rol si tienes intención de promover un controlador de dominio. Todas las características de administración de Active Directory y servicios necesarios se instalen automáticamente, aunque no aparezca seleccionados en la interfaz de administrador del servidor o evidentemente forman parte de otra función.  
  
El administrador del servidor también presenta un cuadro de diálogo informativa que muestra las características de administración implícitamente instala esta función; Esto equivale a la **- IncludeManagementTools** argumento.  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddFeaturesDialog.gif)  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectFeatures.png)  
  
Adicionales **características** se pueden agregar aquí como deseado.  
  
#### <a name="active-directory-domain-services"></a>Servicios de dominio de Active Directory  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSIntro.png)  
  
La **los servicios de dominio de Active Directory** cuadro de diálogo proporciona información limitada sobre los requisitos y recomendaciones. Principalmente actúa como una confirmación de que has elegido el rol de AD DS "Si no aparece esta pantalla, has seleccionado no AD DS.  
  
#### <a name="confirmation"></a>Confirmación  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Confirmation.png)  
  
La **confirmación** cuadro de diálogo es el punto de control final antes de que comience la instalación del rol. Ofrece una opción para reiniciar el equipo según sea necesario tras la instalación de la función, pero la instalación de AD DS requiere un reinicio.  
  
Haciendo clic en **instalar**, confirmas que estás listo para comenzar la instalación del rol. No se puede cancelar una instalación de la función una vez que comienza.  
  
#### <a name="results"></a>Resultados  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Results.png)  
  
La **resultados** cuadro de diálogo que muestra el progreso actual de la instalación y el estado actual de la instalación. Instalación del rol continúa independientemente de si se cierra el administrador del servidor.  
  
Comprobación de los resultados de instalación todavía es un procedimiento recomendado. Si cierras la **resultados** cuadro de diálogo antes de que finalice la instalación, puedes comprobar los resultados con la marca de la notificación de administrador del servidor. El administrador del servidor también se muestra un mensaje de advertencia para todos los servidores que tienen instalado el rol de AD DS, pero no se ha configurado aún más como controladores de dominio.  
  
**Notificaciones de tareas**  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskNotofications.png)  
  
**Detalles de AD DS**  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSDetails.png)  
  
**Detalles de la tarea**  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskDetails.png)  
  
#### <a name="promote-to-domain-controller"></a>Promocionar al controlador de dominio  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Promote.png)  
  
Al final de la instalación del rol de AD DS, puedes continuar con la configuración mediante el uso de la **promover este servidor a un controlador de dominio** vínculo. Esto es necesario para garantizar al servidor en un controlador de dominio, pero no es necesario para ejecutar al Asistente para configuración inmediatamente. Por ejemplo, solo puedes aprovisionar los servidores con los archivos binarios de AD DS antes de enviarlos a otra sucursal para la configuración más adelante. Adición del rol de AD DS antes del envío, ahorran tiempo cuando llega a su destino. También seguir el procedimiento recomendado de no mantener un controlador de dominio sin conexión para días o semanas. Por último, esto te permite actualizar componentes antes de la promoción del controlador de dominio, lo que permite ahorrar al menos un reinicio posterior.  
  
Seleccionar este vínculo más adelante invoca los cmdlets ADDSDeployment: **instalación addsforest**, **instalación addsdomain**, o **instalación addsdomaincontroller**.  
  
### <a name="uninstallingdisabling"></a>Desinstalar o deshabilitar  
Quitar el rol de AD DS como cualquier otra función, independientemente de si se promueve al servidor para un controlador de dominio. Sin embargo, al quitar el rol de AD DS, requiere un reinicio de finalización.  
  
Eliminación del rol de Active Directory los servicios de dominio es diferente de la instalación, ya que requiere la degradación de controladores de dominio para poder completar. Esto es necesario para impedir que un controlador de dominio tengan sus binarios rol desinstalar sin metadatos correctos Liberador de espacio en el bosque. Para obtener más información, consulta [degradar controladores de dominio y dominios & #40; Nivel 200 & #41; ](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
> [!WARNING]  
> Quita los roles de AD DS con Dism.exe o el módulo DISM de PowerShell de Windows después de promoción para un controlador de dominio no se admite y evitará que el servidor de arranque con normalidad.  
>   
> A diferencia del administrador del servidor o el módulo de implementación de AD DS para Windows PowerShell, DISM es un sistema de mantenimiento nativo que tiene ningún conocimiento inherente de AD DS o su configuración. No uses Dism.exe o el módulo DISM de Windows PowerShell para desinstalar el rol de AD DS, a menos que el servidor ya no es un controlador de dominio.  
  
### <a name="create-an-ad-ds-forest-root-domain-with-server-manager"></a>Crear un dominio de raíz del bosque de AD DS con el administrador del servidor  
El siguiente diagrama ilustra el proceso de configuración de los servicios de dominio de Active Directory, en el caso de que tienes instalado el rol de AD DS e iniciado anteriormente el **Asistente para la configuración a los servicios de Active Directory dominio** con el administrador del servidor.  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_forestdeploy2.png)  
  
#### <a name="deployment-configuration"></a>Configuración de implementación  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddNewForest.png)  
  
El administrador del servidor comienza cada promoción del controlador de dominio con la **configuración de implementación** página. El resto de opciones y los campos obligatorios cambian en esta página y las páginas siguientes, según qué operación de implementación que seleccione.  
  
Para crear un nuevo bosque de Active Directory, haga clic en **agregar un bosque nuevo**. Debes proporcionar un nombre de dominio raíz válido; no puede ser el nombre de etiqueta única (por ejemplo, el nombre debe ser *contoso.com* o similares y no solo *contoso*) y debe usar permitidos requisitos de nombres de dominio DNS.  
  
Para obtener más información sobre los nombres de dominio válido, consulte el artículo [convenciones de nomenclatura en Active Directory para equipos, dominios, sitios y unidades organizativas](https://support.microsoft.com/kb/909264).  
  
> [!WARNING]  
> No crear nuevos bosques de Active Directory con el mismo nombre que un nombre DNS externo. Por ejemplo, si la dirección URL de DNS de Internet es http://contoso.com, debes elegir un nombre diferente para el bosque interno evitar problemas de compatibilidad futura. Ese nombre debe ser único e improbables para el tráfico de la web. Por ejemplo: corp.contoso.com.  
  
Un bosque nuevo no tiene nuevas credenciales de cuenta de administrador del dominio. El proceso de promoción de controlador de dominio usa las credenciales de la cuenta predefinida de administrador desde el primer controlador de dominio que se usó para crear la raíz del bosque. No existe forma (de forma predeterminada) para deshabilitar o bloquear la cuenta predefinida de administrador y puede ser el punto de entrada único en un bosque si las otras cuentas de dominio administrativo son inutilizables. Es fundamental conocer la contraseña antes de implementar un bosque nuevo.  
  
**DomainName** requiere un nombre DNS válido de dominio completo y es necesario.  
  
#### <a name="domain-controller-options"></a>Opciones del controlador de dominio  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DCOptions_Forest.gif)  
  
La **opciones del controlador de dominio** permite configurar el **nivel funcional del bosque** y **nivel funcional del dominio** para el dominio raíz del bosque nuevo. De manera predeterminada, estas opciones de configuración son Windows Server 2012 en un dominio raíz del bosque nuevo. El nivel funcional del bosque de Windows Server 2012 no proporciona ninguna funcionalidad nueva en el nivel funcional del bosque de Windows Server 2008 R2. El nivel funcional de dominio de Windows Server 2012 solo es necesario para implementar la nueva configuración de Kerberos "siempre proporcionar notificaciones" y "Producirá un error en las solicitudes de autenticación unarmored." Un uso principal de niveles funcionales en Windows Server 2012 es restringir la participación en los controladores de dominio para que cumplan los requisitos del sistema operativo mínimo permitido. En otras palabras, puedes especificar Windows Server 2012 dominio funcional nivel solo controladores de dominio que ejecutan Windows Server 2012 pueden hospedar el dominio.  Windows Server 2012 implementa una marca de controlador de dominio nueva denominada **DS_WIN8_REQUIRED** en la **DSGetDcName** función de NetLogon que exclusivamente busca controladores de dominio de Windows Server 2012. Esto permite la flexibilidad de un bosque más homogénea o heterogénea en cuanto a que los sistemas operativos tienen permiso para ejecutarse en controladores de dominio.  
  
Para obtener más información sobre la ubicación del controlador de dominio, revisa [funciones de servicios de directorio](https://msdn.microsoft.com/library/ms675900(VS.85).aspx).  
  
La funcionalidad de controlador de dominio solo configurable es la opción de servidor DNS. Microsoft recomienda que todos los controladores de dominio proporcionan servicios DNS para alta disponibilidad en entornos distribuidos, lo que esta opción está seleccionada de manera predeterminada cuando se instala un controlador de dominio en cualquier modo o dominio. El catálogo Global y leer las opciones de controlador de dominio solo están disponibles cuando se crea un nuevo dominio raíz; el primer controlador de dominio debe ser un catálogo global y no puede ser un controlador de dominio solo lectura (RODC).  
  
Especificado **contraseña de modo de restauración de servicios de directorio** deben cumplir con la directiva de contraseñas aplicada al servidor, que, de manera predeterminada, no requiere una contraseña segura; solo en blanco uno. Siempre elegir una contraseña segura y compleja o preferiblemente, una frase de contraseña.  
  
#### <a name="dns-options-and-dns-delegation-credentials"></a>Opciones de DNS y las credenciales de la delegación de DNS  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestDNSOptions.png)  
  
La **opciones DNS** página te permite configurar la delegación de DNS y proporcionar credenciales administrativas de DNS alternativas.  
  
No puedes configurar opciones de DNS o delegación en el Asistente para la configuración de los servicios de dominio de Active Directory cuando se instala un nuevo Active Directory dominio raíz del bosque donde has seleccionado la **servidor DNS** en la **opciones del controlador de dominio** página. La **delegación DNS crear** opción está disponible cuando se crea una zona DNS de bosque nuevo raíz en una infraestructura de servidor DNS. Esta opción te permite proporcionar credenciales administrativas DNS alternativas que tienen derechos para actualizar la zona DNS.  
  
Para obtener más información acerca de si es necesario crear una delegación de DNS, consulta [acerca de la delegación zona](https://technet.microsoft.com/library/cc771640.aspx).  
  
#### <a name="additional-options"></a>Opciones adicionales  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestAdditionalOptions.png)  
  
La **opciones adicionales** página muestra el nombre NetBIOS del dominio y te permite invalidarlo. De manera predeterminada, el nombre de dominio NetBIOS coincide con la etiqueta de extremo izquierdo del nombre de dominio completo que se proporciona en el **configuración de implementación** página. Por ejemplo, si se proporciona el nombre de dominio completo Corp.contoso.com, el nombre de dominio NetBIOS predeterminado es Corporation.  
  
Si el nombre es de 15 caracteres o menos y no entre en conflicto con otro nombre NetBIOS, no se ha alterado. Si entre en conflicto con otro nombre NetBIOS, un número se anexa al nombre. Si el nombre es más de 15 caracteres, el asistente proporciona una sugerencia única, truncada. En cualquier caso, el Asistente primero valida el nombre no está en uso a través de una búsqueda WINS y NetBIOS difusión.  
  
Para obtener más información sobre los nombres de dominio válido, consulte el artículo [convenciones de nomenclatura en Active Directory para equipos, dominios, sitios y unidades organizativas](https://support.microsoft.com/kb/909264).  
  
#### <a name="paths"></a>Rutas de acceso  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPaths.png)  
  
La **rutas de acceso** página te permite invalidar las ubicaciones predeterminadas de la carpeta de la base de datos de AD DS, los registros de transacciones de base de datos, y compartir la carpeta SYSVOL. Las ubicaciones predeterminadas de siempre están en los subdirectorios de % systemroot % (es decir, C:\Windows).  
  
#### <a name="review-options-and-view-script"></a>Opciones de revisión y el Script de vista  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestReviewOptions.png)  
  
La **opciones de revisión** página te permite validar la configuración y asegúrate de que cumplen los requisitos antes de iniciar la instalación. Esto no es la última oportunidad para detener la instalación al usar el administrador del servidor. Esto es simplemente una opción para confirmar su configuración antes de continuar con la configuración  
  
La **opciones de revisión** también ofrece un opcional de la página en el administrador del servidor **ver Script** botón para crear un archivo de texto de Unicode que contiene la configuración actual de ADDSDeployment como una única secuencia de Windows PowerShell. Esto te permite usar la interfaz gráfica de administrador del servidor como un estudio de implementación de Windows PowerShell. Usar al Asistente para la configuración de los servicios de dominio de Active Directory para configurar las opciones, exportar la configuración y, a continuación, Cancelar al asistente. Este proceso crea una muestra sintácticamente correcta y válida para modificaciones adicionales o su uso directo. Por ejemplo:  
  
```powershell 
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSForest `  
-CreateDNSDelegation `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainMode "Win2012" `  
-DomainName "corp.contoso.com" `  
-DomainNetBIOSName "CORP" `  
-ForestMode "Win2012" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-NoRebootOnCompletion:$false `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> Por lo general, rellena el administrador del servidor en todos los argumentos con los valores de promoción y no se basa en valores predeterminados (como pueden cambiar entre las versiones futuras de Windows o service Pack). La única excepción a esto es la **- safemodeadministratorpassword** argumento (que se omite deliberadamente desde el script). Para forzar a un mensaje de confirmación, omite el valor cuando se ejecuta el cmdlet de manera interactiva.  
  
#### <a name="prerequisites-check"></a>Comprobación de requisitos previos  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPrereqCheck.png)  
  
La **comprobación de requisitos previos** es una nueva característica en la configuración de dominio de AD DS. Esta nueva fase valida que la configuración del servidor es capaz de admitir un nuevo bosque de AD DS.  
  
Al instalar un nuevo dominio raíz, el Asistente de configuración de servicios de Active Directory dominio de Server Manager invoca una serie de pruebas modulares. Estas pruebas avisan con opciones de reparación sugeridas. Puedes ejecutar las pruebas tantas veces como sea necesario. El proceso de controlador de dominio no puede continuar hasta que todos los requisitos previos pruebas pasar.  
  
La **comprobación de requisitos previos** superficies también información relevante, como los cambios de seguridad que afectan a los sistemas operativos anteriores.  
  
Para obtener más información sobre los requisitos previos controles específicos, consulta [comprobación de requisito previo](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
#### <a name="installation"></a>Instalación  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestInstallation.png)  
  
Cuando la **instalación** muestra la página, la configuración del controlador de dominio comienza y no se han detenido o cancela. Operaciones detalladas en esta página muestran y se escriben en registros:  
  
-   %systemroot%\Debug\Dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
> [!NOTE]  
> Puedes ejecutar varios asistentes de configuración de AD DS y de instalación del rol desde la misma consola del administrador del servidor al mismo tiempo.  
  
#### <a name="results"></a>Resultados  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
La **resultados** página muestra el éxito o error de la promoción y cualquier información administrativa importante. El controlador de dominio se reiniciará automáticamente después de 10 segundos.  
  
## <a name="BKMK_PSForest"></a>Implementación de un bosque con Windows PowerShell  
Esta sección explica cómo instalar el primer controlador de dominio en un dominio raíz del bosque mediante Windows PowerShell en un equipo principal de Windows Server 2012.  
  
### <a name="windows-powershell-ad-ds-role-installation-process"></a>Proceso de instalación de Windows PowerShell AD DS rol  
Al implementar unos cmdlets de implementación ServerManager sencillas en los procesos de implementación, te aún más DAS cuenta de que administración simplificada de la visión de AD DS.  
  
En la siguiente imagen ilustra el proceso de instalación de la función de los servicios de dominio de Active Directory, se ejecuta a partir de **PowerShell.exe** y terminando justo antes de la promoción del controlador de dominio.  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment_powershell.png)  
  
|||  
|-|-|  
|ServerManager Cmdlet|Argumentos (**negrita** argumentos son necesarios. *El formato de cursiva* argumentos pueden especificarse mediante Windows PowerShell o el Asistente para la configuración de AD DS.)|  
|Install-WindowsFeature/agregar-WindowsFeature|***-Name***<br /><br />*-Reinicio*<br /><br />*-IncludeAllSubFeature*<br /><br />*-IncludeManagementTools*<br /><br />-Origen<br /><br />*-ComputerName*<br /><br />-Credenciales<br /><br />-LogPath<br /><br />*-Vhd*<br /><br />*-ConfigurationFilePath*|  
  
> [!NOTE]  
> Mientras no es necesario, el argumento **- IncludeManagementTools** es muy recomendable que instalar los binarios de rol de AD DS  
  
El módulo ServerManager expone las partes de instalación, el estado y eliminación de rol del nuevo módulo DISM para Windows PowerShell. Estas capas simplifica las tareas más y reduce la necesidad de uso directo de las potentes (pero peligroso cuando uso incorrecto) módulo DISM.  
  
Usa **comando Get** para exportar el alias y cmdlets en ServerManager.  
  
```powershell  
Get-Command -module ServerManager  
```  
  
Por ejemplo:  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetCommand.png)  
  
Para agregar la función de los servicios de dominio de Active Directory, solo tienes que ejecutar el **Install-WindowsFeature** con el nombre de rol de AD DS como un argumento. Como el administrador del servidor, todos los servicios necesarios implícito en el rol de AD DS se instalen automáticamente.  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services  
```  
  
Si también desea que las herramientas de administración de AD DS instaladas, y esto es muy recomendable -, a continuación, proporcionar la **- IncludeManagementTools** argumento:  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services -IncludeManagementTools  
```  
  
Por ejemplo:  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallWinFeature.png)  
  
Para mostrar todas las características y funciones con su estado de la instalación, usa **Get-WindowsFeature** sin argumentos. Especificar **- ComputerName** argumento para el estado de instalación desde un servidor remoto.  
  
```powershell  
Get-WindowsFeature  
```  
  
Dado que **Get-WindowsFeature** no tiene un filtro mecanismo, debes usar **Where-Object** con una canalización para buscar características específicas. La canalización es un canal utilizado entre varios cmdlets para pasar datos y el cmdlet Where-Object actúa como un filtro. Integrado **$_** variable actúa como el objeto actual que se pasa a través de la canalización con todas las propiedades que puede contener.  
  
```powershell  
Get-WindowsFeature | where-object <options>  
```  
  
Por ejemplo buscar todas las características contenedora "Active Directory" en sus **nombre para mostrar** propiedad, usa:  
  
```powershell  
Get-WindowsFeature | where displayname -like "*active dir*"  
```  
  
Otros ejemplos que se muestra a continuación:  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetWindowsFeature.png)  
  
Para obtener más información acerca de las operaciones de Windows PowerShell más con canalizaciones y Where-Object, consulta [diseño y la canalización de Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).  
  
Ten en cuenta también que Windows PowerShell 3.0 simplificado significativamente los argumentos de línea de comandos necesarios en esta operación de canalización. Windows PowerShell 2.0 habría necesitado:  
  
```powershell  
Get-WindowsFeature | where {$_.displayname - like "*active dir*"}  
```  
  
Mediante el uso de la canalización de Windows PowerShell, puedes crear resultados legibles. Por ejemplo:  
  
```powershell  
Install-WindowsFeature | Format-List  
Install-WindowsFeature | select-object | Format-List  
  
```  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDS.png)  
  
Ten en cuenta como el uso del **Select Object** cmdlet con la **- expandproperty** argumento devuelve datos interesantes:  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSWithTools.png)  
  
> [!NOTE]  
> La **Select Object - expandproperty** argumento ralentiza ligeramente rendimiento general de la instalación.  
  
### <a name="BKMK_PS"></a>Crear un dominio de raíz del bosque de AD DS con Windows PowerShell  
Para instalar un nuevo bosque de Active Directory mediante el módulo ADDSDeployment, usa el cmdlet siguiente:  
  
```powershell  
Install-addsforest  
```  
  
La **instalación AddsForest** cmdlet solo tiene dos fases (comprobación de requisitos previos e instalación). Las dos figuras siguientes muestran la fase de instalación con el argumento mínimo requerido de **- NombreDominio**.  
  
|||  
|-|-|  
|ADDSDeployment Cmdlet|Argumentos (**negrita** argumentos son necesarios. *El formato de cursiva* argumentos pueden especificarse mediante Windows PowerShell o el Asistente para la configuración de AD DS.)|  
|Instalación Addsforest|-Confirmar<br /><br />*-CreateDNSDelegation*<br /><br />*Ruta:*<br /><br />*-DomainMode*<br /><br />***-El nombre de dominio***<br /><br />***El valor de DomainNetBIOSName-***<br /><br />*-DNSDelegationCredential*<br /><br />*-ForestMode*<br /><br />-Force<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-NoDnsOnNetwork<br /><br />-NoRebootOnCompletion<br /><br />*-SafeModeAdministratorPassword*<br /><br />-SkipAutoConfigureDNS<br /><br />-SkipPreChecks<br /><br />*-SYSVOLPath*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> La **- el valor de DomainNetBIOSName** argumento es obligatorio si quieres cambiar el nombre de 15 caracteres generado automáticamente según el prefijo de nombre de dominio DNS o si el nombre es superior a 15 caracteres.  
  
El administrador del servidor equivalente **configuración de implementación** ADDSDeployment cmdlet y los argumentos son:  
  
```powershell  
Install-ADDSForest  
-DomainName <string>  
```  
  
Los argumentos de cmdlet Server Manager dominio controlador opciones ADDSDeployment equivalentes son:  
  
```powershell  
-ForestMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-InstallDNS <{$false | $true}>  
-SafeModeAdministratorPassword <secure string>  
  
```  
  
La **instalación ADDSForest** argumentos que siguen los valores predeterminados mismo como el administrador del servidor, si no se especifica.  
  
La **SafeModeAdministratorPassword** operación del argumento es especial:  
  
-   Si *no especifica* como un argumento, el cmdlet te pedirá que escriba y confirme una contraseña enmascarada. Este es el uso preferido cuando se ejecuta el cmdlet de manera interactiva.  
  
    Por ejemplo crear un nuevo bosque denominado corp.contoso.com y se pide para escribir y Confirmar contraseña enmascarada:  
  
    ```powershell  
    Install-ADDSForest "DomainName corp.contoso.com  
    ```  
  
-   Si se especifica *con un valor*, el valor debe ser una cadena segura. No es el uso preferido cuando se ejecuta el cmdlet de manera interactiva.  
  
Por ejemplo, puedes manualmente pedir contraseña utilizando la **Read-Host** cmdlet para pedir al usuario una cadena segura:  
  
```powershell  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
```  
  
> [!WARNING]  
> Como la opción anterior confirma la contraseña, Ten mucho cuidado: la contraseña no está visible.  
  
También puedes proporcionar una cadena segura como una variable de texto no cifrado convertida, aunque no se recomienda.  
  
```powershell  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
```  
  
Por último, podrías almacenar la contraseña ofuscada en un archivo y, a continuación, volver a utilizarlo más tarde, sin la contraseña de texto no cifrado deja de aparecer. Por ejemplo:  
  
```powershell  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> No se recomienda proporcionar ni almacenar una contraseña de texto activa o desactiva ofuscados. Cualquier persona que ejecutar este comando en un script o buscando por encima del hombro conozca la contraseña DSRM de ese controlador de dominio. Cualquier persona que tenga acceso al archivo puede invertir esa contraseña ofuscada. Con estos datos, pueden iniciar sesión en un controlador de dominio iniciado en DSRM y finalmente suplantar el propio controlador de dominio, eleve sus privilegios para el nivel más alto de un bosque de Active Directory. Un conjunto adicional de pasos mediante **System.Security.Cryptography** para cifrar el archivo de texto datos están recomendable pero fuera del ámbito. El procedimiento recomendado es evitar por completo el almacenamiento de contraseña.  
  
El cmdlet ADDSDeployment ofrece una opción adicional para omitir la configuración automática de configuración del cliente DNS, servidores de reenvío y sugerencias de raíz. No se puede omitir esta opción de configuración cuando se usa el administrador del servidor. Este argumento es importante solo si has instalado el rol de servidor DNS antes de configurar el controlador de dominio:  
  
```powershell  
-SkipAutoConfigureDNS  
```  
  
La **valor de DomainNetBIOSName** operación también es especial:  
  
-   Si la **valor de DomainNetBIOSName** argumento no se especifica con un nombre de dominio de NetBIOS y el nombre de dominio de prefijo de etiqueta única en la **NombreDominio** argumento es de 15 caracteres o menos, promoción continúa con un nombre generado automáticamente.  
  
-   Si la **valor de DomainNetBIOSName** argumento no se especifica con un nombre de dominio de NetBIOS y el nombre de dominio de prefijo de etiqueta única en la **NombreDominio** argumento es de 16 caracteres o más, se produce un error en la promoción.  
  
-   Si la **valor de DomainNetBIOSName** argumento se especifica con un nombre de dominio NetBIOS de 15 caracteres o menos, a continuación, promoción continúa con ese nombre especificado.  
  
-   Si la **valor de DomainNetBIOSName** argumento se especifica con un nombre de dominio NetBIOS de 16 caracteres o más y, luego, se produce un error en la promoción.  
  
El argumento de cmdlet Server Manager adicionales opciones ADDSDeployment equivalente es:  
  
```powershell  
-domainnetbiosname <string>  
```  
  
El administrador del servidor equivalente **rutas de acceso** ADDSDeployment cmdlet argumentos son:  
  
```powershell  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
  
```  
  
Usar opcional **Whatif** argumento con la **instalación ADDSForest** cmdlet para revisar la información de configuración. Esto te permite ver los valores implícitos y explícitos de argumentos de un cmdlet.  
  
Por ejemplo:  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSPaths.png)  
  
No se puede omitir la **comprobar requisitos previos** cuando usa el administrador del servidor, pero puedes omitir el proceso cuando se usa el cmdlet de implementación de AD DS con el argumento siguiente:  
  
```powershell  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft recomienda no omitiendo comprobar los requisitos previos como que puede provocar una promoción del controlador de dominio parcial o daña el bosque de AD DS.  
  
Ten en cuenta cómo hacerlo, al igual que el administrador del servidor, **instalación ADDSForest** te recuerda que promoción reiniciará automáticamente el servidor.  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSReboot.png)  
  
![Instala un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallProgress.png)  
  
Para aceptar automáticamente la consulta de reinicio, usa el **-forzar** o **-confirmar: $false** argumentos con cualquier cmdlet ADDSDeployment Windows PowerShell. Para evitar que el servidor reiniciar automáticamente al final de promoción, usa el **- norebootoncompletion** argumento.  
  
> [!WARNING]  
> No se recomienda reemplazar el reinicio. El controlador de dominio debe reiniciar para que funcione correctamente.  
  
## <a name="see-also"></a>Consulta también  
[Servicios de dominio de Active Directory (Portal de TechNet)](https://technet.microsoft.com/library/cc770946(WS.10).aspx)  
[Servicios de dominio de Active Directory para Windows Server 2008 R2](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
[Servicios de dominio de Active Directory para Windows Server 2008](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
[Referencia técnica de Windows Server (Windows Server 2003)](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
[Centro de administración de Active Directory: Introducción (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd560651(WS.10).aspx)  
[Administración de Active Directory con Windows PowerShell (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd378937(WS.10).aspx)  
[Pregunte al equipo de servicios de directorio (Blog de soporte técnico oficial comerciales de Microsoft)](http://blogs.technet.com/b/askds)  
  

