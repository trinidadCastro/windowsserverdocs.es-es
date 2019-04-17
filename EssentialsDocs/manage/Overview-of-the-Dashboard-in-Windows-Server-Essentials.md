---
title: "Información general del panel en Windows Server Essentials"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f70a79de-9c56-4496-89b5-20a1bff2293e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c2cc603f5e0303ada245956a524151393c538b27
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="overview-of-the-dashboard-in-windows-server-essentials"></a>Información general del panel en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Windows Server Essentials y Windows Server 2012 R2 Standard con el rol de Windows Server Essentials Experience habilitado incluyen un panel administrativo, lo que simplifica las tareas que realizan para administrar la red de Windows Server Essentials y el servidor. Al usar el escritorio de Windows Server Essentials, puedes:  
  
-   Terminar de configurar el servidor  
  
-   Acceso y realizar tareas de administración comunes  
  
-   Ver las alertas de servidor y actuar sobre ellos  
  
-   Configurar y cambiar la configuración del servidor  
  
-   Acceso o buscar temas de ayuda en la web  
  
-   Acceder a recursos de la Comunidad en la web  
  
-   Administrar cuentas de usuario  
  
-   Administrar dispositivos y las copias de seguridad  
  
-   Administrar la configuración de discos duros y carpetas del servidor y acceso  
  
-   Ver y administrar aplicaciones de complementos  
  
-   Integra con Microsoft online services  
  
 Este tema incluye:  
  
-   [Características básicas del panel](#BKMK_Design)  
  
-   [Características de la página principal del panel](#BKMK_Home)  
  
-   [Secciones administrativas del panel](#BKMK_Features)  
  
-   [Acceder al panel de Windows Server Essentials](#BKMK_AccessDb)  
  
-   [Usar el modo seguro](#BKMK_UseSafeMode)  
  
##  <a name="BKMK_Design"></a>Características básicas del panel  
 El escritorio de Windows Server Essentials te ayuda a acceder rápidamente a las características clave de administración y la información de tu servidor. El panel incluye varias secciones. La tabla siguiente describe las secciones.  
  
 
  
|Elemento|Característica de panel|Descripción|  
|----------|-----------------------|-----------------|  
|1|Barra de navegación|Haz clic en una sección en la barra de navegación para acceder a la información y las tareas que están asociadas a esa sección. Cada vez que abras el panel, el **Home** página aparece de manera predeterminada.|  
|2|Pestañas de subsección|Las pestañas subsección proporcionan acceso a un segundo nivel de tareas de administración de Windows Server Essentials.|  
|3|Panel de lista|La vista de lista muestra los objetos que se puede administrar e incluye información básica acerca de cada objeto.|  
|4|Panel de detalles|El panel de detalles muestra información adicional sobre un objeto que hayas seleccionado en la vista de lista.|  
|5|Panel de tareas|El panel de tareas contiene vínculos a herramientas e información que te ayudarán a administrar las propiedades de un objeto específico (por ejemplo, una cuenta de usuario o un equipo) o para la configuración global para la categoría de objeto. El panel de tareas se divide en estas dos secciones:<br /><br /> **Objeto tareas** œ contiene vínculos a herramientas e información que te ayudarán a administración las propiedades de un objeto que hayas seleccionado en la vista de lista (como una cuenta de usuario o un equipo).<br /><br /> **Tareas globales** œ contiene vínculos a herramientas e información que te ayudarán a administración las tareas globales de un área de característica. Tareas globales incluyen tareas para agregar nuevos objetos, Establece la directiva y así sucesivamente.|  
|6|Información y opciones de configuración|Esta sección proporciona un acceso directo a la configuración del servidor y un vínculo de ayuda para obtener información acerca de la página del panel que está viendo.|  
|7|Estado de alertas|El estado de alertas proporciona una indicación visual rápida sobre el estado del servidor. Haz clic en la imagen de alerta para ver las alertas críticas e importantes.<br /><br /> **Nota:** en Windows Server Essentials y Windows Server 2012 R2 Standard con el rol de Windows Server Essentials Experience instalado, el estado de alertas está disponible en la **información y opciones de configuración** pestaña.|  
|8|Barra de estado|La barra de estado, muestra el número de objetos que aparecen en la vista de lista. Aplicaciones Add-in también pueden mostrar otro estado.|  
  
##  <a name="BKMK_Home"></a>Características de la página principal del panel  
 Cuando abras el panel, el **Home** aparecerá la página de forma predeterminada con la **instalación** categoría muestra. La **Home** página del panel de Essentials del servidor de Windows proporciona un acceso rápido a las tareas y la información que te ayudan a personaliza el servidor y configura características clave. La página principal está compuesta por cuatro áreas funcionales que exponen las tareas de configuración e información para las opciones que selecciones. La siguiente tabla describe las características.  
  
|Elemento|Característica|Descripción|  
|----------|-------------|-----------------|  
|1|Barra de navegación|Haz clic en una sección en la barra de navegación para acceder a la información y las tareas que están asociadas a esa sección. Cada vez que abras el panel, el **Home** página muestra de manera predeterminada.|  
|2|Panel de categorías|Este panel muestra las áreas de características que proporcionan acceso rápido a información y herramientas de configuración que te ayudarán a configuración y personalizar el servidor. Haz clic en una categoría para mostrar las tareas y recursos que están asociados a esa categoría.|  
|3|Panel de tareas|Este panel muestra vínculos a las tareas y la información que se aplican a una categoría seleccionada. Haz clic en una tarea o un recurso para mostrar información adicional.|  
|4|Panel de acciones|Este panel proporciona una breve descripción de una característica o la tarea y proporciona vínculos a los que se abren los asistentes de configuración y las páginas de información. Haz clic en un vínculo para realizar ninguna acción adicional.|  
  
##  <a name="BKMK_Features"></a>Secciones administrativas del panel  
 El escritorio de Windows Server Essentials organiza la información de red y las tareas administrativas en áreas funcionales. La página de administración para cada área funcional proporciona información acerca de los objetos asociados a esa área, por ejemplo **usuarios**, o **dispositivos**. La página de administración también incluye tareas que puedes usar para ver o cambiar la configuración, o para ejecutar programas que automatizan las tareas que requieren varios pasos.  
  
 La siguiente tabla describe las secciones administrativas del panel que están disponibles de forma predeterminada tras la instalación. La tabla también enumera las tareas que están disponibles en cada sección.  
  
|Sección|Descripción|  
|-------------|-----------------|  
|Casa|La **Home** página aparece de manera predeterminada cada vez que abra el panel. Incluye tareas e información en las siguientes categorías:<br /><br /> **El programa de instalación** œ completar las tareas en esta categoría para configurar el servidor por primera vez. Para obtener información acerca de estas tareas, consulta [instalar y configurar Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials.md).<br /><br /> **Correo electrónico** œ elige una opción en esta categoría para integrar un servicio de correo electrónico con el servidor.<br /><br /> **Nota:** esta categoría solo está disponible en Windows Server Essentials.<br /><br /> **SERVICIOS** œ elegir una tarea en esta categoría para integrar los servicios en línea de Microsoft con el servidor.<br /><br /> **Nota:** esta categoría solo está disponible en Windows Server Essentials y Windows Server 2012 R2 Standard con el rol de Windows Server Essentials Experience habilitado.<br /><br /> **ADD-INS** œ, haz clic en esta categoría para instalar complementos valiosos para tu negocio.<br /><br /> **ESTADO rápido** œ muestra el estado de alto nivel de servidor. Haz clic en un estado para ver las opciones de configuración e información para esa característica. Si completas todas las tareas en la categoría de configuración, esta categoría aparece en la parte superior del panel de categoría.<br /><br /> **Ayudar a** œ usa el cuadro de búsqueda para buscar ayuda en la Web. Haz clic en un vínculo para visitar el sitio Web para la opción de soporte seleccionado.|  
|Usuarios|Para que los usuarios acceder a los recursos que proporciona Windows Server Essentials, deberás crear cuentas de usuario mediante el panel de Windows Server Essentials. Después de crear cuentas de usuario, puedes administrar las cuentas mediante el uso de las tareas que están disponibles en la **usuarios** página del panel. Las tareas que puedes realizar en esta página incluyen:<br /><br /> -Ver una lista de cuentas de usuario.<br /><br /> -Ver y administrar las propiedades de la cuenta de usuario.<br /><br /> -Activar o desactivar las cuentas de usuario.<br /><br /> -Agregar o quitar cuentas de usuario.<br /><br /> -Asignar cuentas de redes locales a las cuentas de servicios en línea de Microsoft si el servidor está integrado con Office 365.<br /><br /> -Cambiar la contraseña de cuenta de usuario y administrar la directiva de contraseñas.<br /><br /> Para obtener información sobre la administración de cuentas de usuario, consulta [administrar cuentas de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md).|  
|Grupos de usuarios|**Nota:** esta característica solo está disponible en Windows Server Essentials y Windows Server 2012 R2 Standard con el rol de Windows Server Essentials Experience habilitado.<br /><br /> Las tareas que puedes realizar en esta página incluyen:<br /><br /> -Ver una lista de grupos de usuarios.<br /><br /> -Ver y administrar grupos de usuarios.<br /><br /> -Agregar o quitar grupos de usuarios.|  
|Grupos de distribución|**Nota:** esta característica solo está disponible en Windows Server Essentials y Windows Server 2012 R2 Standard con el rol de Windows Server Essentials Experience habilitado. Esta pestaña solo aparece cuando Windows Server Essentials se integra con Office 365.<br /><br /> Las tareas que puedes realizar en esta página incluyen:<br /><br /> -Ver una lista de grupos de distribución.<br /><br /> -Agregar o quitar grupos de distribución.|  
|Dispositivos|Después de conectar equipos a la red de Windows Server Essentials, puedes administrar los equipos de la **dispositivos** página del panel. Las tareas que puedes realizar en esta página incluyen:<br /><br /> -Ver una lista de equipos que están unidos a la red.<br /><br /> -Administrar dispositivos móviles aprovechando la funcionalidad de administración de dispositivos móviles de Office 365.<br /><br /> **Nota:** esta característica solo está disponible en Windows Server Essentials y Windows Server 2012 R2 Standard con el rol de Windows Server Essentials Experience habilitado.<br /><br /> -Ver las propiedades de equipo y alertas de estado para cada equipo.<br /><br /> : Permite configurar y administrar copias de seguridad del equipo.<br /><br /> -Restaurar archivos y carpetas a los equipos.<br /><br /> -Establecer una conexión a Escritorio remoto a un equipo<br /><br /> -Personalizar la configuración de copia de seguridad del equipo y el historial de archivos<br /><br /> Para obtener información sobre la administración de equipos y las copias de seguridad, consulta [administrar dispositivos](Manage-Devices-in-Windows-Server-Essentials.md).|  
|Almacenamiento|Según la versión de Windows Server Essentials cuando se está ejecutando, la **almacenamiento** sección del panel contiene las siguientes secciones de manera predeterminada.<br /><br /> -La **carpetas del servidor** subsección incluye tareas que te ayudarán a ver y administrar las propiedades de carpetas del servidor. La página también incluye tareas para abrir y agregar carpetas de servidor.<br /><br /> -La **unidades de disco duro** página incluye tareas que te ayudarán a la vista y comprobar el estado de las unidades que se adjuntan al servidor.<br /><br /> -En Windows Server Essentials y Windows Server 2012 R2 Standard con el rol de Windows Server Essentials Experience habilitado, el **bibliotecas de SharePoint** página incluye tareas que te ayudarán a administrar las bibliotecas de SharePoint en el servicio de Office 365.<br /><br /> Para obtener información acerca de cómo administrar carpetas del servidor, consulte [administrar carpetas del servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md).<br /><br /> Para obtener información sobre la administración de unidades de disco duro, consulta [administrar almacenamiento del servidor](Manage-Server-Storage-in-Windows-Server-Essentials.md).|  
|Aplicaciones|-La **aplicaciones** sección del panel Windows Server Essentials contiene dos subsecciones de manera predeterminada.<br /><br /> Para obtener información sobre la administración de aplicaciones de complementos, consulta [administrar aplicaciones](Manage-Applications-in-Windows-Server-Essentials.md).<br /><br /> -La **Add-ins** subsección muestra una lista de complementos instalados y proporciona tareas que te permiten quitar un complemento y tener acceso a información adicional acerca de un complemento seleccionado.<br /><br /> -La **Microsoft Pinpoint** subsección muestra una lista de aplicaciones que están disponibles en Microsoft Pinpoint.|  
|Office 365|La **Office 365** ficha muestra solo cuando Windows Server Essentials se integra con Microsoft Office 365. Esta sección contiene información de cuenta de administrador y la suscripción a Office 365.|  
  
> [!NOTE]
>  Si instalas un complemento para el escritorio de Windows Server Essentials, el complemento puede crear secciones administrativas adicionales. Estas secciones pueden aparecer en la barra de navegación, o en una pestaña subsección.  
  
##  <a name="BKMK_AccessDb"></a>Acceder al panel de Windows Server Essentials  
 El panel puede tener acceso mediante el uso de uno de los siguientes métodos. El método que elijas depende de si se tiene acceso el panel desde el servidor, desde un equipo que está conectado a la red de Windows Server Essentials o desde un equipo remoto.  
  
 Para ayudar a mantener una red segura, solo los usuarios con permisos administrativos pueden acceder el escritorio de Windows Server Essentials. Además, no puedes usar la cuenta predefinida de administrador para iniciar sesión en el escritorio de Windows Server Essentials del Launchpad.  
  
###  <a name="BKMK_Server"></a>Acceder al panel desde el servidor  
 Cuando se instalación Windows Server Essentials, el proceso de configuración crea un acceso directo al escritorio en la **inicio** pantalla y también en el escritorio. Si el método abreviado de falta de estas ubicaciones, puedes usar la **búsqueda** panel para encontrar y ejecutar el programa de escritorio.  
  
##### <a name="to-access-the-dashboard-from-the-server"></a>Para acceder al panel desde el servidor  
  
-   Inicia sesión en el servidor como administrador y, a continuación, realiza una de las siguientes acciones:  
  
    -   En la **inicio** de pantalla, haz clic en **panel**.  
  
    -   En el escritorio, haz doble clic en **panel**.  
  
    -   En la **búsqueda** panel, escribe **panel**y, a continuación, haz clic en **panel** en los resultados de búsqueda.  
  
###  <a name="BKMK_Network"></a>Acceder al panel desde un equipo que está conectado a la red  
 En Windows Server Essentials, después de unir un equipo a la red, los administradores pueden usar el Launchpad para acceder al panel del servidor desde el equipo.  
  
> [!WARNING]
>  En Windows Server Essentials, para conectarse al panel desde el equipo cliente, haz clic en el icono de la bandeja y, a continuación, abra el panel en el menú contextual.  
  
##### <a name="to-access-the-dashboard-by-using-the-launchpad"></a>Para acceder al panel mediante el uso del Launchpad  
  
1.  Desde un equipo que está unido a la red, abra el **Launchpad**.  
  
2.  En el menú de Launchpad, haz clic en **panel**.  
  
3.  En el panel **iniciar sesión** página, escribe tus credenciales de administrador de red y, a continuación, presione ENTRAR.  
  
     Abre una conexión remota para el escritorio de Windows Server Essentials.  
  
###  <a name="BKMK_Remote"></a>Acceder al panel desde un equipo remoto  
 Cuando estás trabajando desde un equipo remoto, puede tener acceso el escritorio de Windows Server Essentials mediante el sitio de acceso Web remoto.  
  
##### <a name="to-access-the-dashboard-by-using-remote-web-access"></a>Para acceder al panel mediante el uso de acceso Web remoto  
  
1.  En el equipo remoto, abre un explorador de Internet, como Internet Explorer.  
  
2.  En la barra de direcciones del explorador de Internet, escribe lo siguiente:**https://<DomainName\>/remote**, donde *NombreDominio* es el nombre de dominio externo de su organización (por ejemplo, contoso.com).  
  
3.  Escribe tu nombre de usuario y contraseña para iniciar sesión el sitio de acceso Web remoto.  
  
4.  En la **equipos** sección del acceso Web remoto**Home** página; Busque el servidor y haz clic en **conectar**.  
  
     Abre una conexión remota al panel.  
  
    > [!NOTE]
    >  Si el servidor no aparece en la **equipos** sección de la página principal, haz clic en **conectarse a equipos más**, encontrar su servidor en la lista y, a continuación, haz clic en el servidor para conectarte a ella.  
    >   
    >  Para dar un permiso del usuario para acceder al panel de acceso Web remoto, abre la página de propiedades de la cuenta de usuario y, a continuación, selecciona el **escritorio Server** opción en la **acceso desde cualquier lugar** pestaña.  
  
##  <a name="BKMK_UseSafeMode"></a>Usar el modo seguro  
 La característica de modo seguro de Windows Server Essentials supervisa los complementos que están instalados en el servidor. Si el panel se vuelve inestable o no responde, el modo seguro detectará y aísla los complementos que podrían estar causando el problema y los muestra en la **configuración del modo seguro** la próxima vez que abra el panel de la página. Desde el **configuración del modo seguro** página, puedes deshabilitar y habilitar complementos uno por uno para determinar qué complemento está causando el problema. A continuación, puedes dejar el complemento deshabilitado para nuevas empresas futuras o puedes desinstalar el complemento.  
  
 Puede ver una lista de todos los complementos que están instalados en el servidor en cualquier momento. También puedes abrir el panel en modo seguro, lo que deshabilita automáticamente todos los complementos no son de Microsoft. Windows Server Essentials también permite acceder al panel en modo seguro desde otro equipo en la red.  
  
#### <a name="to-view-a-list-of-installed-add-ins"></a>Para ver una lista de los complementos instalados  
  
-   En el panel, haz clic en **ayuda**y, a continuación, haz clic en **configuración de modo seguro**.  
  
#### <a name="to-open-the-dashboard-in-safe-mode"></a>Para abrir el panel en modo seguro  
  
-   En la **búsqueda** panel, escribe **seguro**y, a continuación, haz clic en **panel (modo seguro)** en los resultados de búsqueda.  
  
#### <a name="to-open-the-dashboard-in-safe-mode-from-another-computer-on-the-network"></a>Para abrir el panel en modo seguro desde otro equipo en la red  
  
1.  Desde un equipo que está conectado a la red, abra el Launchpad de Windows Server Essentials y, a continuación, haz clic en **panel**.  
  
2.  En la página de inicio de sesión del panel, escribe el nombre de usuario y contraseña para una cuenta que tiene permiso para iniciar sesión en el servidor, selecciona el **permitirme seleccionar qué complementos para cargar** y, a continuación, haz clic en la flecha para iniciar sesión.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Administrar aplicaciones](Manage-Applications-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
