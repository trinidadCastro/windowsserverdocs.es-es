---
title: Introducción al Launchpad en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 198d16cb-3d07-4706-be89-ad14a5f7dc47
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: c20fd35a1a90e6d635891cd2257913c669273735
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623010"
---
# <a name="overview-of-the-launchpad-in-windows-server-essentials"></a>Introducción al Launchpad en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Launchpad de Windows Server Essentials es una pequeña aplicación que se instala en un equipo la primera vez que se conecta al servidor. Launchpad proporciona a los usuarios autenticados acceso a las características clave de Windows Server Essentials, entre ellas las copias de seguridad del equipo, los archivos y elementos multimedia compartidos y el sitio de Acceso Web remoto. Los usuarios pueden tener acceso a estas características desde equipos unidos a un dominio o desde equipos no unidos a un dominio. Launchpad también proporciona información en tiempo real y notificaciones sobre el estado del equipo. Los administradores pueden usar Launchpad para tener acceso al panel del servidor, aunque el equipo no esté conectado a la red.

 Los fabricantes de equipos originales (OEM) y los proveedores de software independientes (ISV) que desarrollan complementos para Windows Server Essentials pueden usar Launchpad para extender la funcionalidad de los complementos a los equipos de la red.

 Los siguientes sistemas operativos de Windows admiten el uso de Launchpad de Windows Server Essentials:

- **Windows 8**: todas las ediciones.

- **Windows 7**: todas las ediciones.
- **Windows 10**: todas las ediciones.

  Los siguientes sistemas operativos no admiten el uso de Launchpad de Windows Server Essentials:

- **Servidores adicionales**: no se puede ejecutar Launchpad de Windows Server Essentials en los equipos adicionales que ejecutan el sistema operativo Windows Server.

  En este tema:

- [Uso de Launchpad](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Launchpad)

- [Uso de Launchpad con un equipo Mac](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Mac)

##  <a name="use-the-launchpad"></a><a name="BKMK_Launchpad"></a> Uso de Launchpad
 La información y los vínculos siguientes están disponibles en Launchpad de Windows Server Essentials.

### <a name="backup"></a>Copia de seguridad
 Haga clic en **Copia de seguridad** para abrir las **Propiedades de copia de seguridad** del equipo. En la página **Propiedades de copia de seguridad** puede:

- Iniciar o detener una copia de seguridad.

- Ver el estado y los detalles de la copia de seguridad más reciente.

- Especificar la manera en que se administrará la energía del equipo cuando se ejecute la copia de seguridad.

  Para obtener información sobre cómo usar Launchpad para hacer una copia de seguridad del equipo, consulte [Manage Client backup](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md).

### <a name="remote-web-access"></a>Acceso web remoto
 Haga clic en **Acceso Web remoto** para abrir el explorador web en el sitio de Acceso Web remoto. El sitio de Acceso Web remoto le permite conectarse a otros equipos y tener acceso a algunos de los recursos de red desde la oficina o desde cualquier ubicación remota con un equipo habilitado para Internet. Para obtener más información sobre el acceso Web remoto, consulte [administrar el acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md).

### <a name="shared-folders"></a>Carpetas compartidas
 Haga clic en **Carpetas compartidas** para abrir el Explorador de Windows en la ubicación de las carpetas compartidas en el servidor. Para obtener información sobre cómo compartir archivos y carpetas, vea el tema [administrar carpetas de servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md).

### <a name="dashboard"></a>Panel
 Haga clic en  **Panel** para abrir la página **Iniciar sesión** para tener acceso al panel de Windows Server Essentials. Después de iniciar sesión se abre una conexión a Escritorio remoto en el panel del servidor. Para más información sobre el panel, consulte [información general del panel](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md).

> [!NOTE]
>  Para usar esta característica debe tener el acceso o los permisos adecuados para iniciar sesión en el servidor.

### <a name="microsoft-365"></a>Microsoft 365
 El vínculo **Microsoft 365** solo aparece en Launchpad si el usuario tiene una cuenta Microsoft 365. Haga clic en  **Microsoft 365** para tener acceso a vínculos adicionales a los recursos de Microsoft 365. Para obtener más información, vea [Guía de inicio rápido para usar Microsoft 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).

### <a name="computer-health-alerts"></a>Alertas de estado del equipo
 Las alertas que aparecen en Launchpad proporcionan un estado rápido en cuanto al mantenimiento inmediato del equipo. Para ver la información sobre una alerta de estado haga clic en un indicador de alerta para abrir el visor de alertas. Las alertas de estado aparecen en el visor en función del nivel de gravedad. Las alertas más graves aparecen en primer lugar en la lista; las alertas menos graves aparecen a continuación. Para obtener más información acerca de las alertas de estado del equipo, consulte [Manage System Health](Manage-System-Health-in-Windows-Server-Essentials.md).

##  <a name="use-the-launchpad-with-a-mac-computer"></a><a name="BKMK_Mac"></a> Uso de Launchpad con un equipo Mac
 Puede conectar un equipo Mac &reg; que ejecute Mac OS X &reg; 10,5 o posterior a Windows Server Essentials, Windows Server Essentials o windows Server 2012 R2 o descargando e instalando el software del conector. Cuando haya terminado de instalar el software del conector podrá iniciar automáticamente Launchpad en el inicio.

 Launchpad es una pequeña aplicación que proporciona a los usuarios autenticados acceso a las características clave del servidor, entre ellas los archivos y los elementos multimedia compartidos, los complementos y Acceso Web remoto. Launchpad también proporciona información en tiempo real y notificaciones sobre el estado del equipo.

> [!NOTE]
>  Los administradores del servidor no pueden usar Launchpad ni Acceso Web remoto en un equipo Mac para abrir el panel del servidor y administrar el servidor.

### <a name="backup"></a>Copia de seguridad
 Haga clic en **Copia de seguridad** para configurar Time Machine, hacer una copia de seguridad del equipo y cambiar la configuración de Time Machine. Para obtener más información sobre Time Machine, vea la documentación del fabricante del equipo.

### <a name="remote-web-access"></a>Acceso web remoto
 Haga clic en **acceso Web remoto** para abrir el explorador Web en el sitio de acceso Web remoto. El acceso Web remoto le permite tener acceso a los archivos y carpetas compartidos en el servidor desde cualquier ubicación remota con un equipo habilitado para Internet. Puede cargar archivos, reproducir música y vídeos en el Reproductor multimedia basado en web, y ver imágenes y reproducir presentaciones. Para obtener más información, vea [usar acceso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).

### <a name="shared-folders"></a>Carpetas compartidas
 Haga clic en **Carpetas compartidas** para abrir el selector en la ubicación de las carpetas compartidas en el servidor. Para obtener información sobre cómo compartir archivos y carpetas, consulte [usar carpetas compartidas](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md).

### <a name="computer-health-alerts"></a>Alertas de estado del equipo
 Las alertas que aparecen en Launchpad proporcionan un estado rápido sobre el mantenimiento inmediato del equipo. Para ver la información sobre una alerta de estado haga clic en un indicador de alerta para abrir el visor de alertas. Las alertas de estado aparecen en el visor en función del nivel de gravedad. Las alertas más graves aparecen en primer lugar en la lista. Las alertas menos graves aparecen a continuación.

## <a name="additional-references"></a>Referencias adicionales

-   [Conéctese](../use/Get-Connected-in-Windows-Server-Essentials.md)

-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
