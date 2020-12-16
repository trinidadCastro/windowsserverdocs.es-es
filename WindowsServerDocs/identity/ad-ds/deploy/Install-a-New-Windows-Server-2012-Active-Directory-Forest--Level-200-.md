---
description: Más información acerca de cómo instalar un nuevo bosque de Active Directory de Windows Server 2012 (nivel 200)
ms.assetid: b3d6fb87-c4d4-451c-b3de-a53d2402d295
title: Instalar un nuevo bosque de Active Directory de Windows Server 2012 (nivel 200)
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 96e9ee67b60c6c2e2125e8c518a70bdde7a9a863
ms.sourcegitcommit: 6fbe337587050300e90340f9aa3e899ff5ce1028
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/16/2020
ms.locfileid: "97599798"
---
# <a name="install-a-new-windows-server-2012-active-directory-forest-level-200"></a>Instalar un nuevo bosque de Active Directory de Windows Server 2012 (nivel 200)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema es una introducción a la nueva característica de promoción de controladores de dominio de los Servicios de dominio de Active Directory (AD DS) en Windows Server 2012. En Windows Server 2012, AD DS reemplaza a la herramienta Dcpromo por un sistema de implementación basado en Windows PowerShell y el Administrador del servidor.

-   [Administración simplificada de los Servicios de dominio de Active Directory](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SimplifiedAdmin)

-   [Información técnica](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_TechOverview)

-   [Implementación de un bosque con el Administrador del servidor](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)

-   [Implementación de un bosque con Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_PSForest)

## <a name="active-directory-domain-services-simplified-administration"></a><a name="BKMK_SimplifiedAdmin"></a>Administración simplificada de los Servicios de dominio de Active Directory
Windows Server 2012 presenta la nueva generación de Administración simplificada de Servicios de dominio de Active Directory, el nuevo rediseño de dominio más radical desde Windows 2000 Server. La Administración simplificada de AD DS se ha inspirado en lo aprendido a lo largo de doce años de Active Directory para crear una experiencia de administración más compatible, flexible e intuitiva para arquitectos y administradores. Esto ha conllevado la creación de nuevas versiones de las tecnologías existentes y la ampliación de las capacidades de los componentes incluidos en Windows Server 2008 R2.

### <a name="what-is-ad-ds-simplified-administration"></a>¿Qué es la Administración simplificada de AD DS?
La Administración simplificada de AD DS supone un nuevo concepto de implementación de dominio. Estas son algunas de las características:

-   Ahora, la implementación del rol de AD DS forma parte de la nueva arquitectura del Administrador del servidor y permite la instalación remota.

-   Ahora, el motor de implementación y configuración de AD DS es Windows PowerShell, aunque se use una configuración gráfica.

-   Ahora, la promoción incluye la comprobación de requisitos previos, que valida la preparación de los bosques y los dominios para el nuevo controlador de dominio. Así, es menos probable que se produzcan errores en las promociones.

-   El nivel funcional del bosque de Windows Server 2012 no implementa las características nuevas, y el nivel funcional del dominio solo es necesario para un subconjunto de las nuevas características de Kerberos. De este modo, los administradores ya no necesitan tan a menudo un entorno de controlador de dominio homogéneo.

### <a name="purpose-and-benefits"></a>Objetivo y ventajas
Estos cambios pueden parecer más complejos y no más sencillos. Sin embargo, al rediseñar el proceso de implementación de AD DS, tuvimos la oportunidad de fusionar muchos pasos y procedimientos recomendados en menos acciones más fáciles. Por ejemplo, la configuración gráfica de un nuevo controlador de dominio de réplica tiene ahora ocho diálogos en lugar de los doce anteriores. Para crear un nuevo bosque de Active Directory, hace falta *un solo* comando de Windows PowerShell con solo *un* argumento: el nombre del dominio.

¿Por qué se insiste tanto en Windows PowerShell en Windows Server 2012? Mientras la computación distribuida evoluciona, Windows PowerShell permite usar un solo motor para la configuración y el mantenimiento de las interfaces gráficas y de la línea de comandos. Permite el scripting completo de cualquier componente y proporciona la misma funcionalidad de primera clase a los profesionales de TI que las API a los desarrolladores. Y, por último, a medida que la computación en la nube abarca más y más ámbitos, Windows PowerShell ofrece también la posibilidad de administrar de forma remota un servidor. En este caso, un ordenador sin interfaz gráfica tiene la misma funcionalidad de administración que uno con monitor y mouse.

Los administradores de AD DS con experiencia comprobarán que sus conocimientos anteriores les resultan muy útiles. Y los administradores principiantes disfrutarán de una curva de aprendizaje mucho menos pronunciada.

## <a name="technical-overview"></a><a name="BKMK_TechOverview"></a>Información técnica

### <a name="what-you-should-know-before-you-begin"></a>Qué debes saber antes de empezar
En este tema, se da por hecho que estás familiarizado con las versiones anteriores de los Servicios de dominio de Active Directory. No se proporcionan detalles básicos sobre su finalidad ni su funcionalidad. Para más información sobre AD DS, consulta las páginas del portal de TechNet vinculadas a continuación:

-   [Servicios de dominio de Active Directory para Windows Server 2008 R2](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd378801(v=ws.10))

-   [Servicios de dominio de Active Directory para Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd378891(v=ws.10))

-   [Referencia técnica de Windows Server](/previous-versions/windows/it-pro/windows-server-2003/cc739127(v=ws.10))

### <a name="functional-descriptions"></a>Descripciones funcionales

#### <a name="ad-ds-role-installation"></a>Instalación del rol de AD DS
![Captura de pantalla que muestra la página roles de servidor en el Asistente para agregar roles y características.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_SelectServerRoles.gif)

La instalación de los Servicios de dominio de Active Directory emplea el Administrador del servidor y Windows PowerShell, como todos los demás roles del servidor y características de Windows Server 2012. El programa Dcpromo.exe ya no ofrece opciones de configuración de la GUI.

Tanto en las instalaciones locales como en las remotas, se usa un asistente gráfico en el Administrador del servidor o el módulo ServerManager para Windows PowerShell. Ejecutando varias instancias de esos asistentes o cmdlets y usando como destino diferentes servidores, puedes implementar AD DS en varios controladores de dominio al mismo tiempo, todo ello desde una sola consola. Estas nuevas características no son compatibles con las versiones anteriores de Windows Server 2008 R2 ni con los sistemas operativos anteriores. Sin embargo, puedes usar la aplicación Dism.exe, presentada en Windows Server 2008 R2, para instalar roles de forma local desde la línea de comandos clásica.

![Captura de pantalla que muestra una ventana de terminal de Windows PowerShell.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSAddWindowsFeature.png)

#### <a name="ad-ds-role-configuration"></a>Configuración del rol de AD DS
![Captura de pantalla que muestra la página Configuración de implementación del Asistente para configuración de Active Directory Domain Services.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DeploymentConfiguration_Forest.gif)

Active Directory Domain Services configuración "anteriormente conocida como DCPROMO" es ahora una operación discreta de la instalación del rol. Después de instalar el rol de AD DS, un administrador configura el servidor como controlador de dominio con un asistente independiente dentro del Administrador del servidor o con el módulo ADDSDeployment de Windows PowerShell.

La configuración del rol de AD DS se basa en doce años de experiencia en el campo y, ahora, configura los controladores de dominio de acuerdo con los procedimientos recomendados más recientes de Microsoft. Por ejemplo, en todos los controladores de dominio se instalan de forma predeterminada el Sistema de nombres de dominio y los catálogos globales.

El Asistente para configuración de Administrador del servidor AD DS combina muchos cuadros de diálogo individuales en menos mensajes y deja de ocultar la configuración en modo "avanzado". Durante la instalación, todo el proceso de promoción se desarrolla en un solo cuadro de diálogo que va expandiéndose. El asistente y el módulo ADDSDeployment de Windows PowerShell te muestran los cambios destacables y los riesgos de seguridad, con vínculos a más información.

Dcpromo.exe se mantiene en Windows Server 2012 solamente para las instalaciones desatendidas desde la línea de comandos y ya no ejecuta el Asistente para instalación gráfico. Te recomendamos encarecidamente que dejes de usar Dcpromo.exe para las instalaciones desatendidas y que lo reemplaces por el módulo ADDSDeployment: el ejecutable se considera ahora desusado y no se incluirá en la próxima versión de Windows.

Estas nuevas características no son compatibles con las versiones anteriores de Windows Server 2008 R2 ni con los sistemas operativos anteriores.

![Captura de pantalla que muestra una ventana de terminal de Windows PowerShell durante una instalación.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSForest.png)

> [!IMPORTANT]
> Dcpromo.exe ya no contiene un asistente gráfico y ya no instala los archivos binarios de los roles ni las características. Si tratas de ejecutar Dcpromo.exe desde el shell del Explorador, devolverá:
>
> "El Asistente para instalación de Active Directory Domain Services se reubica en Administrador del servidor. Para más información, consulte: <https://go.microsoft.com/fwlink/?LinkId=220921>.
>
> Si tratas de ejecutar Dcpromo.exe /unattend, se instalarán los archivos binarios, como en los sistemas operativos anteriores, pero aparecerá la siguiente advertencia:
>
> "La operación desatendida de DCPROMO se reemplaza por el módulo ADDSDeployment para Windows PowerShell. Para más información, consulte: <https://go.microsoft.com/fwlink/?LinkId=220924>.
>
> En Windows Server 2012 se deja de usar dcpromo.exe, que no se incluirá en las futuras versiones de Windows ni recibirá más mejoras en este sistema operativo. Los administradores deben dejar de utilizarlo y, si quieren crear controladores de dominio desde la línea de comandos, deberán usar en su lugar los módulos de Windows PowerShell admitidos.

#### <a name="prerequisite-checking"></a>Comprobación de requisitos previos
La configuración de los controladores de dominio también implementa una fase de comprobación de requisitos previos en la que se evalúan el bosque y el dominio antes de continuar con la promoción de los controladores de dominio. Incluye la disponibilidad del rol FSMO, los privilegios de los usuarios, la compatibilidad del esquema extendido y otros requisitos. Este nuevo diseño mitiga los problemas que se producen cuando la promoción de los controladores de dominio se inicia y, luego, se detiene en mitad del proceso con un error de configuración irrecuperable. Disminuye la probabilidad de que haya metadatos de controladores de dominio huérfanos en el bosque o servidores que crean por error que son controladores de dominio.

## <a name="deploying-a-forest-with-server-manager"></a><a name="BKMK_SMForest"></a>Implementación de un bosque con el Administrador del servidor
En esta sección, se explica cómo instalar el primer controlador de dominio en el dominio raíz de un bosque con el Administrador del servidor en un equipo con Windows Server 2012 gráfico.

### <a name="server-manager-ad-ds-role-installation-process"></a>Proceso de instalación del rol de AD DS del Administrador del servidor
El siguiente diagrama representa el proceso de instalación del rol de los Servicios de dominio de Active Directory. Empieza cuando ejecutas ServerManager.exe y finaliza justo antes de la promoción del controlador de dominio.

![Diagrama que muestra el proceso de instalación de roles de Active Directory Domain Services, empezando por la ejecución de ServerManager.exe y la finalización justo antes de la promoción del controlador de dominio.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment.png)

#### <a name="server-pool-and-add-roles"></a>Grupo de servidores y adición de roles
Se pueden agrupar todos los equipos con Windows Server 2012 accesibles desde el equipo que ejecuta el Administrador del servidor. Una vez agrupados, se seleccionan los servidores para la instalación remota de AD DS o cualquier otra opción de configuración que ofrezca el Administrador del servidor.

Para agregar servidores, elige una de estas opciones:

-   Haz clic en **Agregar otros servidores para administrar** en la ventana de bienvenida del panel.

-   Haz clic en el menú **Administrar** y elige **Agregar servidores**.

-   Haz clic con el botón secundario en **Todos los servidores** y elige **Agregar servidores**.

Aparecerá el diálogo Agregar servidores:

![Captura de pantalla que muestra la Active Directory pestaña en el cuadro de diálogo Agregar servidores.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddServers.png)

Ofrece tres maneras de agregar servidores al grupo para utilizarlos o agruparlos:

-   Búsqueda de Active Directory (utiliza LDAP, requiere que los equipos pertenezcan a un dominio, permite filtrar sistemas operativos y admite caracteres comodín).

-   Búsqueda de DNS (utiliza el alias DNS o la dirección IP por difusión ARP o NetBIOS o búsqueda WINS, y no permite filtrar sistemas operativos ni admite caracteres comodín).

-   Importación (usa una lista de servidores en archivo de texto separados por CR/LF)

Haz clic en **Buscar ahora** para que el sistema devuelva una lista de servidores del mismo dominio de Active Directory al que se unió el equipo. En la lista de servidores, haz clic en el nombre de uno o varios servidores. Haz clic en la flecha derecha para agregar los servidores a la lista **Seleccionados**. Utiliza el diálogo **Agregar servidores** para agregar los servidores seleccionados a los grupos de roles del panel. O haz clic en **Administrar** y, luego, en **Crear grupo de servidores**. También puedes hacer clic en **Crear grupo de servidores** en la ventana **Administrador del servidor** del panel para crear grupos de servidores personalizados.

> [!NOTE]
> El procedimiento Agregar servidores no valida que un servidor se encuentre en línea o sea accesible. Sin embargo, los servidores inaccesibles se marcan en la vista Estado del Administrador del servidor al actualizarla la siguiente vez.

Puedes instalar roles de forma remota en los equipos con Windows Server 2012 que estén agregados al grupo, así:

![Captura de pantalla que muestra cómo se pueden instalar roles de forma remota en los equipos con Windows Server 2012 agregados al grupo.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/tADDS_SMI_TR_AddRolesFeatures.png)

Los servidores con sistemas operativos anteriores a Windows Server 2012 no se pueden administrar totalmente. La selección **Agregar roles y características** ejecuta el módulo de Windows PowerShell de ServerManager **Install-WindowsFeature**.

![Captura de pantalla que muestra la opción de menú Agregar AD DS a otro servidor.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddADDSToAnotherServer.png)

También puedes usar el panel del Administrador del servidor en un controlador de dominio existente para seleccionar la instalación de AD DS del servidor remoto con el rol ya preseleccionado. Para ello, haz clic con el botón secundario en la ventana del panel de AD DS y elige **Agregar AD DS a otro servidor**. Esto invocará **Install-WindowsFeature AD-Domain-Services**.

El equipo en el que ejecutas el Administrador del servidor se incluye en el grupo automáticamente. Para instalar aquí el rol de AD DS, solo tienes que hacer clic en el menú **Administrar** y hacer clic en **Agregar roles y características**.

![Captura de pantalla que muestra cómo obtener acceso a la opción de menú Agregar roles y características.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ManageAddRoles.png)

#### <a name="installation-type"></a>Tipo de instalación
![Captura de pantalla que muestra la página tipo de instalación del Asistente para agregar roles y características.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectInstallationType.png)

El diálogo **Tipo de instalación** ofrece una opción que no admite Active Directory Domain Services: la **instalación basada en escenario de los Servicios de Escritorio remoto**. Esa opción solamente permite el Servicio de Escritorio remoto en una carga de trabajo distribuida con varios servidores. Si la activas, AD DS no se podrá instalar.

Al instalar AD DS, deja siempre la selección predeterminada: **Instalación basada en roles o basada en características**.

#### <a name="server-selection"></a>Selección de servidor
![Captura de pantalla que muestra la página selección de servidor en el Asistente para quitar roles y características.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectDestinationServer.png)

El diálogo **Selección de servidor** te permite elegir uno de los servidores agregados anteriormente al grupo, siempre que esté accesible. El servidor local que ejecuta el Administrador del servidor está disponible de forma automática.

Además, puedes seleccionar archivos de VHD de Hyper-V sin conexión con el sistema operativo Windows Server 2012 y el Administrador del servidor les agregará el rol directamente con el mantenimiento de componentes. Así, puedes aprovisionar los servidores virtuales con los componentes necesarios antes de seguir configurándolos.

#### <a name="server-roles-and-features"></a>Características y roles del servidor
![Captura de pantalla que muestra la página roles de servidor en el Asistente para agregar roles y características.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectServerRoles.png)

Selecciona el rol **Active Directory Domain Services** si quieres promover un controlador de dominio. Todos los servicios necesarios y las características de administración de Active Directory se instalarán de manera automática, aunque aparentemente formen parte de otro rol o no aparezcan seleccionados en la interfaz del Administrador del servidor.

El Administrador del servidor presenta también un diálogo informativo que muestra qué características de administración instala este rol de forma implícita. Equivale al argumento **-IncludeManagementTools**.

![Captura de pantalla que muestra las características de administración que este rol instala implícitamente; es equivalente al argumento-IncludeManagementTools.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddFeaturesDialog.gif)

![Captura de pantalla que muestra la página características del Asistente para agregar roles y características.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectFeatures.png)

Aquí puedes agregar las **Características** adicionales que quieras.

#### <a name="active-directory-domain-services"></a>Active Directory Domain Services
![Captura de pantalla que muestra la página AD DS del Asistente para quitar roles y características.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSIntro.png)

El diálogo **Active Directory Domain Services** proporciona cierta información sobre los requisitos y los procedimientos recomendados. Actúa principalmente como confirmación de que eligió el rol AD DS ". Si no aparece esta pantalla, no seleccionó AD DS.

#### <a name="confirmation"></a>Confirmación
![Captura de pantalla que muestra la página confirmación en el Asistente para agregar roles y características.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Confirmation.png)

El diálogo **Confirmación** es el último punto de control antes de que se inicie la instalación del rol. Ofrece la opción de reiniciar el equipo si es necesario después de instalar el rol, pero la instalación de AD DS no requiere reiniciarlo.

Al hacer clic en **Instalar**, confirmas que está todo listo para empezar la instalación del rol. Una vez que haya comenzado la instalación del rol, no se podrá cancelar.

#### <a name="results"></a>Results
![Captura de pantalla que muestra la página resultados en el Asistente para agregar roles y características.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Results.png)

El diálogo **Resultados** muestra el progreso actual de la instalación y el estado de instalación actual. La instalación del rol continuará sin importar si está cerrado el Administrador del servidor.

Comprobar los resultados de la instalación sigue siendo un procedimiento recomendado. Si cierras el diálogo **Resultados** antes de que finalice la instalación, puedes consultar los resultados con la marca de notificación del Administrador del servidor. El Administrador del servidor muestra, además, un mensaje de advertencia si hay servidores que tienen instalado el rol de AD DS pero no siguieron configurándose como controladores de dominio.

**Task Notifications**

![Captura de pantalla que muestra una notificación de tarea.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskNotofications.png)

**AD DS Details**

![Captura de pantalla que muestra dónde ver AD DS detalles.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSDetails.png)

**Detalles de la tarea**

![Captura de pantalla que muestra dónde ver los detalles de la tarea.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskDetails.png)

#### <a name="promote-to-domain-controller"></a>Promoción a controlador de dominio
![Captura de pantalla que muestra el vínculo promover este servidor a controlador de dominio.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Promote.png)

Al finalizar la instalación del rol de AD DS, puedes continuar con la configuración con el vínculo **Promover este servidor a controlador de dominio**. Para convertir el servidor en un controlador de dominio, es necesario hacer esto, pero no hace falta ejecutar el Asistente para configuración inmediatamente. Por ejemplo, puede que solo quieras aprovisionar los servidores con los archivos binarios de AD DS antes de enviarlos a otra sucursal para configurarlos más adelante. Si agregas el rol de AD DS antes del envío, ahorrarás tiempo cuando llegue a su destino. Además, estarás siguiendo el procedimiento recomendado que indica que no se debe mantener un controlador de dominio sin conexión durante días o semanas. Por último, esto te permite actualizar los componentes antes de la promoción del controlador de dominio y te ahorra, al menos, un reinicio posterior.

Al seleccionar este vínculo más tarde, se invocan los cmdlets de ADDSDeployment: **install-addsforest**, **install-addsdomain** o **install-addsdomaincontroller**.

### <a name="uninstallingdisabling"></a>Desinstalación o deshabilitación
El rol de AD DS se quita como cualquier otro rol: da igual si promoviste el servidor a controlador de dominio. Sin embargo, para quitar el rol de AD DS, se tiene que reiniciar el sistema al finalizar.

La eliminación del rol de los Servicios de dominio de Active Directory es distinta de la instalación: para que finalice, hay que disminuir el nivel del controlador de dominio. Es necesario hacer esto para que los archivos binarios del rol de un controlador de dominio no se desinstalen sin que los metadatos del bosque se limpien adecuadamente. Para obtener más información, vea [degradar controladores de dominio y dominios &#40;nivel 200&#41;](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md).

> [!WARNING]
> No se admite la eliminación de roles de AD DS con Dism.exe ni con el módulo DISM de Windows PowerShell después de la promoción a controlador de dominio. Si se hace, el servidor no podrá arrancar con normalidad.
>
> A diferencia del Administrador del servidor o el módulo de implementación de AD DS para Windows PowerShell, DISM es un sistema de mantenimiento nativo sin conocimiento inherente de AD DS ni de su configuración. No utilices Dism.exe ni el módulo DISM de Windows PowerShell para desinstalar el rol de AD DS, salvo que el servidor ya no sea un controlador de dominio.

### <a name="create-an-ad-ds-forest-root-domain-with-server-manager"></a>Creación de un dominio raíz del bosque de AD DS con el Administrador del servidor
El siguiente diagrama representa el proceso de configuración de Active Directory Domain Services, en el caso de que instalaras anteriormente el rol de AD DS e iniciaras el **Asistente para configuración de Active Directory Domain Services** con el Administrador del servidor.

![Diagrama que ilustra el proceso de configuración de Active Directory Domain Services, en el caso de que haya instalado previamente el rol de AD DS e iniciado el Asistente para configuración de Active Directory Domain Services con Administrador del servidor. ](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_forestdeploy2.png)

#### <a name="deployment-configuration"></a>Configuración de la implementación
![Captura de pantalla que muestra la configuración de implementación.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddNewForest.png)

El Administrador del servidor comienza la promoción de cada controlador de dominio con la página **Configuración de implementación**. Las opciones restantes y los campos requeridos cambian en esta página y en las páginas siguientes, según qué operación de implementación se seleccione.

Para crear un bosque de Active Directory, haz clic en **Agregar un nuevo bosque**. Aquí debes indicar un nombre de dominio raíz válido. El nombre no puede tener una sola etiqueta (por ejemplo, debe ser *contoso.com* u otro similar, y no solamente *contoso*) y debe seguir los requisitos de nombres de dominio DNS permitidos.

Para más información sobre los nombres de dominio válidos, consulte el artículo de Knowledge Base [Convenciones de nomenclatura de Active Directory para equipos, sitios, dominios y unidades organizativas](https://support.microsoft.com/kb/909264).

> [!WARNING]
> No crees bosques de Active Directory con el mismo nombre que un nombre DNS externo. Por ejemplo, si la dirección URL de DNS de Internet es http://contoso.com , debe elegir un nombre diferente para el bosque interno para evitar problemas de compatibilidad en el futuro. El nombre tiene que ser único y debe ser poco probable que se use para el tráfico de la Web. Por ejemplo: corp.contoso.com.

Los bosques nuevos no necesitan nuevas credenciales para la cuenta de administrador del dominio. El proceso de promoción del controlador de dominio emplea las credenciales de la cuenta de administrador integrada del primer controlador de dominio que se usó para crear la raíz del bosque. No hay ninguna forma (predeterminada) de deshabilitar o bloquear la cuenta de administrador integrada, y puede ser el único punto de entrada a un bosque si las demás cuentas administrativas del dominio no se pueden usar. Antes de implementar un bosque nuevo, es fundamental conocer la contraseña.

**DomainName** requiere un nombre DNS de dominio completo y es obligatorio.

#### <a name="domain-controller-options"></a>Opciones del controlador de dominio
![Captura de pantalla que muestra las opciones del controlador de dominio en el Asistente para configuración de Active Directory Domain Services.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DCOptions_Forest.gif)

Las **Opciones del controlador de dominio** te permiten configurar el **nivel funcional del bosque** y el **nivel funcional del dominio** del nuevo dominio raíz del bosque. De forma predeterminada, esta configuración es Windows Server 2012 en un nuevo dominio raíz del bosque. El nivel funcional del bosque de Windows Server 2012 no proporciona ninguna funcionalidad nueva a través del nivel funcional del bosque de Windows Server 2008 R2. El nivel funcional de dominio de Windows Server 2012 solo es necesario para implementar la nueva configuración de Kerberos "proporcionar notificaciones siempre" y "error en las solicitudes de autenticación sin protección". Un uso principal para los niveles funcionales en Windows Server 2012 es restringir la participación en el dominio a los controladores de dominio que cumplan los requisitos mínimos de sistema operativo permitidos. En otras palabras, puede especificar el nivel funcional de dominio de Windows Server 2012 solo los controladores de dominio que ejecutan Windows Server 2012 pueden hospedar el dominio.  Windows Server 2012 implementa una nueva marca de controlador de dominio denominada **DS_WIN8_REQUIRED** en la función **DSGetDcName** de Netlogon que ubica exclusivamente los controladores de dominio de Windows Server 2012. Así, tienes la flexibilidad de elegir un bosque más o menos homogéneo en cuestión de los sistemas operativos que se pueden ejecutar en los controladores de dominio.

Para más información sobre la ubicación del controlador de dominio, revise [funciones del servicio de directorio](/windows/win32/ad/directory-service-functions).

La única funcionalidad del controlador de dominio que se puede configurar es la opción del servidor DNS. Microsoft recomienda que todos los controladores de dominio proporcionen servicios DNS para alta disponibilidad en entornos distribuidos. Por eso, esta opción está activada de forma predeterminada al instalar un controlador de dominio en cualquier modo o dominio. Al crear un dominio raíz del bosque, no están disponibles las opciones de catálogo global y controlador de dominio de solo lectura. El primer controlador de dominio debe ser un catálogo global y no puede ser un controlador de dominio de solo lectura (RODC).

La **Contraseña del modo de restauración de servicios de directorio** que se especifique debe cumplir la directiva de contraseñas aplicada al servidor. De forma predeterminada, no requiere una contraseña segura: solo una que no esté en blanco. Siempre elija una contraseña segura y compleja o, preferentemente, una frase de contraseña.

#### <a name="dns-options-and-dns-delegation-credentials"></a>Opciones de DNS y credenciales de delegación DNS
![Captura de pantalla que muestra las opciones de DNS en el Asistente para configuración de Active Directory Domain Services.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestDNSOptions.png)

La página **Opciones de DNS** te permite configurar la delegación DNS y proporcionar credenciales administrativas de DNS alternativas.

Si activaste el **Servidor DNS** en la página **Opciones del controlador de dominio**, al instalar un nuevo dominio raíz del bosque de Active Directory, no podrás configurar las opciones de DNS ni la delegación en el Asistente para configuración de Active Directory Domain Services. La opción **Crear delegación DNS** está disponible al crear una nueva zona DNS raíz del bosque en una infraestructura de servidor DNS existente. Esta opción te permite proporcionar credenciales administrativas de DNS alternativas que tengan derecho a actualizar la zona DNS.

Para obtener más información sobre si es necesario crear una delegación DNS, vea [Descripción de la delegación de zonas](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771640(v=ws.11)).

#### <a name="additional-options"></a>Additional Options
![Captura de pantalla que muestra la Página opciones adicionales del Asistente para configuración de Active Directory Domain Services.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestAdditionalOptions.png)

La página **Opciones adicionales** muestra el nombre NetBIOS del dominio y te permite reemplazarlo. De forma predeterminada, el nombre del dominio NetBIOS coincide con la etiqueta del extremo izquierdo del nombre de dominio completo indicada en la página **Configuración de implementación**. Por ejemplo, si indicaste como nombre de dominio completo corp.contoso.com, el nombre del dominio NetBIOS predeterminado será CORP.

Si el nombre tiene 15 caracteres o menos y no entra en conflicto con otro nombre NetBIOS, no se modificará. Si entra en conflicto con otro nombre NetBIOS, se anexará un número al nombre. Si el nombre tiene más de 15 caracteres, el asistente proporcionará una sugerencia truncada que sea única. En cualquiera de estos casos, el asistente validará primero si el nombre no se está usando ya con una búsqueda WINS y difusión NetBIOS.

Para más información sobre los nombres de dominio válidos, consulte el artículo de Knowledge Base [Convenciones de nomenclatura de Active Directory para equipos, sitios, dominios y unidades organizativas](https://support.microsoft.com/kb/909264).

#### <a name="paths"></a>Rutas de acceso
![Captura de pantalla que muestra la página rutas de acceso en el Asistente para configuración de Active Directory Domain Services.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPaths.png)

La página **Rutas de acceso** permite reemplazar las ubicaciones de carpeta predeterminadas de la base de datos AD DS, los registros de transacciones de la base de datos y el recurso compartido SYSVOL. Las ubicaciones predeterminadas siempre están en subdirectorios de %systemroot% (es decir, C:\Windows).

#### <a name="review-options-and-view-script"></a>Revisión de opciones y visualización del script
![Captura de pantalla que muestra la página Opciones de revisión en el Asistente para configuración de Active Directory Domain Services.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestReviewOptions.png)

La página **Revisar opciones** te permite validar la configuración y comprobar que coincide con lo que necesitas antes de iniciar la instalación. No es la última oportunidad para detener la instalación con el Administrador del servidor. Es, simplemente, una opción para confirmar la configuración antes de continuar.

La página **Revisar opciones** del Administrador del servidor también ofrece un botón **Ver script** opcional para crear un archivo de texto Unicode que contiene la configuración ADDSDeployment actual como un script Windows PowerShell único. Esto le permite usar la interfaz gráfica del Administrador del servidor como un estudio de implementación de Windows PowerShell. Use el Asistente para configuración de Servicios de dominio de Active Directory para configurar las opciones, exportar la configuración y, a continuación, cancele el asistente. Este proceso crea un ejemplo válido y sintácticamente correcto para realizar más modificaciones o la utilización directa. Por ejemplo:

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
> Normalmente, el Administrador del servidor rellena todos los argumentos con valores al realizar la promoción y no se basa en los valores predeterminados (dado que pueden cambiar entre las futuras versiones de Windows o los Service Pack). La única excepción de esta regla es el argumento **-safemodeadministratorpassword** (que se omite del script deliberadamente). Para forzar la aparición de un aviso de confirmación, omite el valor al ejecutar el cmdlet de forma interactiva.

#### <a name="prerequisites-check"></a>Prerequisites Check
![Captura de pantalla que muestra la página de comprobación de requisitos previos en el Asistente para configuración de Active Directory Domain Services.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPrereqCheck.png)

La **Comprobación de requisitos previos** es una característica nueva en la configuración de dominios de AD DS. Esta nueva fase valida la capacidad de la configuración del servidor para admitir un nuevo bosque de AD DS.

Al instalar un nuevo dominio raíz del bosque, el Asistente para configuración de Servicios de dominio de Active Directory del Administrador del servidor invoca una serie de pruebas modulares. Las pruebas te envían alertas con opciones de reparación sugeridas. Puedes ejecutar las pruebas tantas veces como sea necesario. El proceso del controlador de dominio no puede continuar hasta que se superen todas las pruebas de requisitos previos.

La **Comprobación de requisitos previos** también expone información relevante, como los cambios de seguridad que afectan a los sistemas operativos anteriores.

Para más información sobre las comprobaciones de requisitos previos específicas, consulta [Comprobación de requisitos previos](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).

#### <a name="installation"></a>Instalación
![Captura de pantalla que muestra la página de instalación del Asistente para configuración de Active Directory Domain Services.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestInstallation.png)

Cuando se muestra la página **Instalación**, la configuración del controlador de dominio comienza y no se puede detener ni cancelar. Las operaciones detalladas se muestran en esta página y se escriben en los registros:

-   %systemroot%\debug\dcpromo.log

-   %systemroot%\debug\dcpromoui.log

> [!NOTE]
> Puedes ejecutar simultáneamente varios Asistentes para configuración de AD DS y de instalación de roles desde la misma consola del Administrador del servidor.

#### <a name="results"></a>Results
![Captura de pantalla que muestra la página resultados en la que puede ver si la promoción se realizó correctamente o no.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestSignOff.png)

La página **Resultados** indica si la promoción se realizó correctamente o si se produjo algún error, junto con toda la información administrativa importante que corresponda. El controlador de dominio se reiniciará automáticamente 10 segundos después.

## <a name="deploying-a-forest-with-windows-powershell"></a><a name="BKMK_PSForest"></a>Implementación de un bosque con Windows PowerShell
En esta sección, se explica cómo instalar el primer controlador de dominio en el dominio raíz de un bosque con Windows PowerShell en un equipo con Windows Server 2012 Core.

### <a name="windows-powershell-ad-ds-role-installation-process"></a>Proceso de instalación del rol de AD DS de Windows PowerShell
Al implementar unos pocos cmdlets de implementación de ServerManager muy sencillos en tus procesos de implementación, entenderás mejor la perspectiva que ofrece la Administración simplificada de AD DS.

La siguiente ilustración representa el proceso de instalación del rol de Active Directory Domain Services. Empieza cuando ejecutas **PowerShell.exe** y finaliza justo antes de la promoción del controlador de dominio.

![Diagrama que muestra el proceso de instalación de roles de Active Directory Domain Services, empezando por ejecutar PowerShell.exe y finalizar justo antes de la promoción del controlador de dominio.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment_powershell.png)

| Cmdlet de ServerManager | Argumentos (los argumentos en **negrita** son obligatorios. Los argumentos en *cursiva* se pueden especificar con Windows PowerShell o con el Asistente para configuración de AD DS). |
|--|--|
| Install-WindowsFeature/Add-WindowsFeature | ***-Nombre** _<p>_ -Restart *<p>* -IncludeAllSubFeature *<p>* -IncludeManagementTools *<p> <p> -source*-COMPUTERNAME *<p> -Credential <p> - <p> LogPath*-VHD *<p>* -ConfigurationFilePath * |

> [!NOTE]
> El argumento **-IncludeManagementTools** no es obligatorio, pero te recomendamos encarecidamente que lo uses al instalar los archivos binarios del rol de AD DS.

El módulo ServerManager expone las partes de instalación, estado y eliminación de roles del nuevo módulo DISM para Windows PowerShell. Con estas capas, se simplifica la mayor cantidad posible de tareas y ya no es tan necesario utilizar directamente el módulo DISM, que ofrece muchas posibilidades, pero es peligroso si se usa de forma incorrecta.

Usa **Get-Command** para exportar los alias y cmdlets de ServerManager.

```powershell
Get-Command -module ServerManager
```

Por ejemplo:

![Captura de pantalla de una ventana de terminal que muestra dónde encontrar el cmdlet Install-WindowsFeature.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetCommand.png)

Para agregar el rol de Active Directory Domain Services, simplemente, ejecuta **Install-WindowsFeature** con el nombre del rol de AD DS como argumento. Igual que en el Administrador del servidor, todos los servicios necesarios que sean implícitos al rol de AD DS se instalarán automáticamente.

```powershell
Install-WindowsFeature -name AD-Domain-Services
```

Si quieres que se instalen también las herramientas de administración de AD DS (muy recomendado), indica el argumento **-IncludeManagementTools**:

```powershell
Install-WindowsFeature -name AD-Domain-Services -IncludeManagementTools
```

Por ejemplo:

![Captura de pantalla de una ventana de terminal que muestra dónde se debe proporcionar el argumento-IncludeManagementTools.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallWinFeature.png)

Para ver una lista de todos los roles y las características con su estado de instalación, utiliza **Get-WindowsFeature** sin argumentos. Especifica el argumento **-ComputerName** para consultar el estado de instalación desde un servidor remoto.

```powershell
Get-WindowsFeature
```

Como **Get-WindowsFeature** no tiene ningún mecanismo de filtrado, para buscar características concretas, tendrás que usar **Where-Object** con una canalización. La canalización es un canal que utilizan varios cmdlets para pasar datos, y el cmdlet Where-Object actúa como filtro. La variable integrada **$_** funciona como el objeto que está pasando en este momento por la canalización junto con las propiedades que contenga.

```powershell
Get-WindowsFeature | where-object <options>
```

Por ejemplo, para buscar todas las características que contengan “Active Dir” en la propiedad **Nombre para mostrar**, usa:

```powershell
Get-WindowsFeature | where displayname -like "*active dir*"
```

A continuación se incluyen más ejemplos:

![Instalar un bosque nuevo](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetWindowsFeature.png)

Para más información sobre las operaciones de Windows PowerShell con canalizaciones y Where-Object, consulte [Canalizar y la canalización de Windows PowerShell](/previous-versions/windows/it-pro/windows-powershell-1.0/ee176927(v=technet.10)).

Windows PowerShell 3.0 simplificó considerablemente los argumentos de la línea de comandos necesarios para esta operación de canalización. En Windows PowerShell 2.0, tendrías que hacer lo siguiente:

```powershell
Get-WindowsFeature | where {$_.displayname - like "*active dir*"}
```

Con la canalización de Windows PowerShell, puedes crear resultados legibles. Por ejemplo:

```powershell
Install-WindowsFeature | Format-List
Install-WindowsFeature | select-object | Format-List

```

![Captura de pantalla de una ventana de terminal que muestra cómo se pueden crear resultados legibles.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDS.png)

Observa cómo, al utilizar el cmdlet **Select-Object** con el argumento **-expandproperty**, se devuelven datos interesantes:

![Captura de pantalla de una ventana de terminal que muestra cómo usar el cmdlet Select-Object con el argumento-expandproperty devuelve datos interesantes.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSWithTools.png)

> [!NOTE]
> El argumento **Select-Object -expandproperty** ralentiza un poco el rendimiento general de la instalación.

### <a name="create-an-ad-ds-forest-root-domain-with-windows-powershell"></a><a name="BKMK_PS"></a>Creación de un dominio raíz del bosque de AD DS con Windows PowerShell
Para instalar un nuevo bosque de Active Directory con el módulo ADDSDeployment, utiliza este cmdlet:

```powershell
Install-addsforest
```

El cmdlet **Install-AddsForest** solo tiene dos fases: comprobación de requisitos previos e instalación. En las dos ilustraciones siguientes, se muestra la fase de instalación con el argumento mínimo obligatorio de **-domainname**.

| Cmdlet de ADDSDeployment | Argumentos (los argumentos en **negrita** son obligatorios. Los argumentos en *cursiva* se pueden especificar con Windows PowerShell o con el Asistente para configuración de AD DS). |
|--|--|
| install-addsforest | -Confirm<p>*-CreateDNSDelegation*<p>*-DatabasePath*<p>*-DomainMode*<p>***-NombreDeDominio** _<p>_ *_-DomainNetBIOSName_* _<p>_ -DNSDelegationCredential *<p>* -ForestMode *<p> -Force <p>*-InstallDNS *<p>* -LogPath *<p> -NoDnsOnNetwork <p> -NoRebootOnCompletion <p>*-SafeModeAdministratorPassword *<p> -SkipAutoConfigureDNS <p> -SkipPreChecks <p>*-SYSVOLPath *<p>* -Whatif * |

> [!NOTE]
> El argumento **-DomainNetBIOSName** es obligatorio si quieres cambiar el nombre de 15 caracteres que se genera automáticamente de acuerdo con el prefijo de nombre de dominio DNS o si el nombre tiene más de 15 caracteres.

El cmdlet y los argumentos de ADDSDeployment equivalentes a la **Configuración de implementación** del Administrador del servidor son:

```powershell
Install-ADDSForest
-DomainName <string>
```

Los argumentos del cmdlet de ADDSDeployment equivalentes a las Opciones del controlador de dominio del Administrador del servidor son:

```powershell
-ForestMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>
-InstallDNS <{$false | $true}>
-SafeModeAdministratorPassword <secure string>
```

Los argumentos de **Install-ADDSForest**, si no se especifican, siguen los mismos valores predeterminados que el Administrador del servidor.

La operación del argumento **SafeModeAdministratorPassword** es especial:

-   Si *no se especifica* como un argumento, el cmdlet le pide que escriba y confirme una contraseña enmascarada. Este es el uso preferido cuando se ejecuta el cmdlet en forma interactiva.

    Por ejemplo, para crear un bosque llamado corp.contoso.com y que te pidan que escribas y confirmes una contraseña enmascarada:

    ```powershell
    Install-ADDSForest "DomainName corp.contoso.com
    ```

-   Si se especifica *con un valor*, este debe ser una cadena segura. Este no es el uso preferido cuando se ejecuta el cmdlet en forma interactiva.

Por ejemplo, puedes solicitar una contraseña de forma manual con el cmdlet **Read-Host** para pedirle al usuario que escriba una cadena segura:

```powershell
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)
```

> [!WARNING]
> Como la opción anterior no confirma la contraseña, tenga mucho cuidado: la contraseña no es visible.

También puedes proporcionar una cadena segura como una variable no cifrada convertida, pero te recomendamos encarecidamente que no lo hagas.

```powershell
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)
```

Por último, puedes almacenar la contraseña cifrada en un archivo y reutilizarla más adelante, sin que la contraseña aparezca descifrada en ningún momento. Por ejemplo:

```powershell
$file = "c:\pw.txt"
$pw = read-host -prompt "Password:" -assecurestring
$pw | ConvertFrom-SecureString | Set-Content $file

-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)

```

> [!WARNING]
> No se recomienda proporcionar ni almacenar contraseñas no cifradas. Cualquier persona que ejecute este comando en un script o que mire sobre su hombro sabrá la contraseña de DSRM de ese controlador de dominio. Cualquier persona que tenga acceso al archivo podría invertir la contraseña cifrada. Si conocen la contraseña, pueden iniciar sesión en un controlador de dominio iniciado en DSRM y llegar a suplantar el controlador de dominio. Elevarían sus privilegios al nivel máximo en el bosque de Active Directory. Te recomendamos que uses un conjunto adicional de pasos con **System.Security.Cryptography** para cifrar los datos del archivo de texto, pero ese tema no se trata aquí. El procedimiento recomendado es no almacenar la contraseña en absoluto.

El cmdlet de ADDSDeployment ofrece una opción adicional para omitir la configuración automática de las opciones, los reenviadores y las sugerencias de raíz del cliente DNS. Al usar el Administrador del servidor, no se puede omitir esta opción de configuración. Este argumento solo es relevante si instalaste el rol del servidor DNS antes de configurar el controlador de dominio:

```powershell
-SkipAutoConfigureDNS
```

La operación **DomainNetBIOSName** también es especial:

-   Si el argumento **DomainNetBIOSName** no se especifica con un nombre de dominio NetBIOS y el nombre de dominio del prefijo de una sola etiqueta del argumento **DomainName** tiene 15 caracteres o menos, la promoción continuará con un nombre generado automáticamente.

-   Si el argumento **DomainNetBIOSName** no se especifica con un nombre de dominio NetBIOS y el nombre de dominio del prefijo de una sola etiqueta del argumento **DomainName** tiene 16 caracteres o más, la promoción no se realizará correctamente.

-   Si el argumento **DomainNetBIOSName** se especifica con un nombre de dominio NetBIOS de 15 caracteres o menos, la promoción continuará con el nombre especificado.

-   Si el argumento **DomainNetBIOSName** se especifica con un nombre de dominio NetBIOS de 16 caracteres o más, la promoción no se realizará correctamente.

El argumento del cmdlet de ADDSDeployment equivalente a las Opciones adicionales del Administrador del servidor es:

```powershell
-domainnetbiosname <string>
```

Los argumentos del cmdlet de ADDSDeployment equivalentes a las **Rutas** del Administrador del servidor son:

```powershell
-databasepath <string>
-logpath <string>
-sysvolpath <string>

```

Utiliza el argumento opcional **Whatif** con el cmdlet **Install-ADDSForest** para revisar la información de configuración. Te permitirá ver los valores explícitos e implícitos de los argumentos de un cmdlet.

Por ejemplo:

![Captura de pantalla de una ventana de terminal que muestra cómo usar el argumento opcional Whatif con el cmdlet Install-ADDSForest para revisar la información de configuración.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSPaths.png)

No puedes omitir la **Comprobación de requisitos previos** al usar el Administrador del servidor, pero sí al utilizar el cmdlet de implementación de AD DS con el siguiente argumento:

```powershell
-skipprechecks
```

> [!WARNING]
> Microsoft no recomienda omitir la comprobación de requisitos previos: omitirla puede hacer que los controladores de dominio se promuevan parcialmente o puede provocar daños en el bosque de AD DS.

Observa que **Install-ADDSForest**, como el Administrador del servidor, te recuerda que la promoción reiniciará el servidor automáticamente.

![Captura de pantalla de una ventana de terminal que muestra Install-ADDSForest recordando que la promoción reiniciará automáticamente el servidor.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSReboot.png)

![Captura de pantalla de una ventana de terminal que muestra el progreso del proceso de reinicio.](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallProgress.png)

Para aceptar el aviso de reinicio de forma automática, utiliza los argumentos **-force** o **-confirm:$false** con cualquier cmdlet de ADDSDeployment de Windows PowerShell. Para impedir que el servidor se reinicie automáticamente al finalizar la promoción, utiliza el argumento **-norebootoncompletion**.

> [!WARNING]
> No se recomienda invalidar el reinicio. El controlador de dominio debe reiniciarse para funcionar correctamente.

## <a name="see-also"></a>Consulte también
[Active Directory Domain Services (portal de TechNet)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770946(v=ws.10)) 
 [Active Directory Domain Services para Windows Server 2008 R2](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd378801(v=ws.10)) 
 [Active Directory Domain Services para Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd378891(v=ws.10)) 
 [Referencia técnica de Windows Server (Windows server 2003)](/previous-versions/windows/it-pro/windows-server-2003/cc739127(v=ws.10)) 
 [Centro de administración de Active Directory: Introducción (Windows Server 2008 R2)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560651(v=ws.10)) 
 [Administración de Active Directory con Windows PowerShell (Windows Server 2008 R2)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd378937(v=ws.10)) 
 [Pregunte al equipo de servicios de directorio (blog oficial de soporte técnico comercial de Microsoft)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd378937(v=ws.10))
