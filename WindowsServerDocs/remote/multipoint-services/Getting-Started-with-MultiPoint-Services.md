---
title: Introducción con Multipoint Services
description: Presenta Multipoint Services y le ayuda a empezar a usarlo.
ms.topic: article
ms.assetid: aca5f0be-f253-46b5-b1e7-0bffa15f3227
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 2469bbb35ce0e68801b28e221c05c285b4f1c464
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996336"
---
# <a name="getting-started-with-multipoint-services"></a>Introducción con Multipoint Services
El sistema Multipoint Services permite a muchos usuarios usar varias estaciones que están conectadas físicamente mediante concentradores de estaciones a un solo equipo. Cada estación consta normalmente de un concentrador de estaciones, un mouse, un teclado y un monitor de vídeo. Cada usuario de una estación de Multipoint Services experimenta una sesión de computación de Windows única que puede administrar mediante Multipoint Manager.

Entre los componentes de un sistema Multipoint Services se incluyen los siguientes:

-   Software del sistema Multipoint Services, que admite varios monitores, teclados, dispositivos de mouse y otros dispositivos del equipo.

-   La aplicación Multipoint Manager, que le permite supervisar y realizar acciones en las estaciones de Multipoint Services.

-   Herramientas de mantenimiento y administración.

-   La aplicación Multipoint Dashboard, que permite completar las tareas diarias, como la comunicación con otros usuarios.

En este archivo de ayuda se proporciona información sobre cómo administrar las estaciones de Multipoint Services con Multipoint Manager y Multipoint Dashboard.

## <a name="overview-of-multipoint-manager"></a>Información general de Multipoint Manager
Multipoint Manager proporciona cuatro pestañas para usar al administrar las estaciones de Multipoint Services. Cada pestaña y las tareas que puede realizar en ellas se describen con más detalle en cada tema de ayuda.

Las pestañas son las siguientes:

-   **Pestaña Inicio:** Cambiar los modos para realizar tareas administrativas, agregar o quitar servidores Multipoint, reiniciar o apagar el equipo, habilitar la protección de disco, agregar licencias de acceso de cliente, reasignar estaciones y obtener ayuda o soporte técnico. Para obtener más información, consulte el tema [administrar tareas del sistema mediante Multipoint Manager](Manage-System-Tasks-Using-MultiPoint-Manager.md) .

-   **Pestaña estaciones:** Ver el estado del *escritorio* de los usuarios y *Finalizar* o *suspender* las sesiones de usuario. Para obtener más información, consulte el tema [administrar estaciones de usuario](Manage-User-Stations.md) .

-   **Pestaña usuarios:** Crear y administrar *cuentas de usuario estándar* y *cuentas de usuario administrativas*. Para más información, vea el tema [Administrar cuentas de usuario](Manage-User-Accounts.md).

-   **Pestaña escritorios virtuales:** Habilitar roles de escritorio virtual. Para obtener más información, consulte el tema [administrar escritorios virtuales](Manage-Virtual-Desktops.md) .

## <a name="multipoint-server-management-and-maintenance"></a>Administración y mantenimiento de MultiPoint Server
Una vez configurado el sistema Multipoint Services, puede usar Multipoint Manager para administrar Multipoint Services.

Entre los tipos de acciones que puede realizar con Multipoint Manager se incluyen los siguientes:

-   **Agregar cuentas de usuario:** Use Multipoint Manager para crear cuentas de usuario estándar y administrativas.

-   **Editar la configuración del servidor:** Puede configurar el sistema Multipoint Services para que se inicie en el modo de consola, permitir que una cuenta tenga varias sesiones, asignar una dirección IP única a cada estación y otras tareas.

-   **Cambiar al modo de consola:** Puede cambiar el sistema Multipoint Services al modo de consola para instalar software nuevo en el sistema Multipoint Services. Puede especificar que todos los usuarios puedan ejecutar el software o que solo pueda usar el software, en función de las opciones de instalación y licencia del software.

-   **Solución de problemas:** Si tiene problemas con Multipoint Services, consulte la sección de [solución de problemas](Troubleshooting.md) para buscar temas que puedan ayudarle a solucionar el problema.

## <a name="overview-of-multipoint-dashboard"></a>Información general sobre el panel de Multipoint
Multipoint Dashboard incluye una experiencia de cinta en la que puede elegir entre dos pestañas para tener acceso a las tareas diarias habituales.

Las pestañas son las siguientes:

-   **Pestaña Inicio:** Bloquee o desbloquee las estaciones, establezca opciones de limitación Web, proyectos de escritorio en otros equipos de escritorio, inicie o cierre aplicaciones, comunique a través de mensajería instantánea, ayude a otros usuarios a través de control de escritorio remoto, ajuste las vistas de miniatura del escritorio y habilite o deshabilite la mensajería instantánea y el inicio automático de aplicaciones. Para obtener más información, consulte el tema [administrar escritorios de usuario mediante Multipoint Dashboard](Manage-User-Desktops-Using-MultiPoint-Dashboard.md) .

-   **Pestaña sistemas:** Reiniciar, apagar o reasignar todos los sistemas seleccionados o todos ellos. Para más información, consulte el tema [Administración de sistemas Multipoint mediante Multipoint Dashboard](Manage-MultiPoint-Systems-Using-MultiPoint-Dashboard.md) .

## <a name="daily-use-of-your-multipoint-server-system"></a>Uso diario del sistema MultiPoint Server
Cuando empiece a usar Multipoint Services cada día, hay información sobre cómo usar Multipoint Services que podría querer compartir con los usuarios del sistema Multipoint Services. Esta información incluye lo siguiente:

**Compartir contenido y mantener el contenido privado:**

-   Un usuario puede guardar un archivo o un documento en una carpeta privada que solo pueda ver el usuario.

-   Los usuarios también pueden guardar documentos en una carpeta pública a la que puedan acceder todos los usuarios del sistema Multipoint Services.

-   Es importante que los usuarios de Multipoint Services sepan que los usuarios administrativos tienen acceso a todos los archivos y documentos del sistema, incluso si se almacenan de forma privada en la carpeta personal de un usuario.

Para obtener más información sobre cómo guardar y administrar contenido público y privado, vea el tema [administrar archivos de usuario](Manage-User-Files.md) .

**Información sobre la sesión de Multipoint Services de un usuario:**

-   Cada usuario tiene un nombre de usuario y una contraseña, y una *sesión* de escritorio única en el sistema Multipoint Services.

-   Un *usuario estándar* no es un *usuario administrativo* en el sistema Multipoint Services. Los usuarios estándar no pueden instalar algunos tipos de software, pero pueden guardar archivos y cambiar la configuración del escritorio, excepto la resolución de pantalla. Todos los cambios realizados por el usuario en el escritorio siguen vigentes cuando vuelve a iniciar sesión.

-   Los usuarios pueden desconectarse de una estación y volver a iniciarla en su sesión en otra estación sin perder su trabajo. Para más información, vea el tema [Suspender y dejar activa la sesión de usuario](Suspend-and-Leave-User-Session-Active.md).

-   El usuario administrativo puede desconectar o cerrar la sesión de un usuario estándar (o todas las sesiones de usuario) a través de Multipoint Manager. Para obtener más información, consulte el tema [administrar escritorios de usuario](manage-user-desktops-using-multipoint-dashboard.md) .

-   Si un usuario olvida una contraseña, puede restablecer la contraseña en la pestaña **usuarios** , que usa la funcionalidad de administración de cuentas de usuario estándar de Windows. Para obtener más información, vea el tema [actualizar o eliminar una cuenta de usuario](Update-or-Delete-a-User-Account.md) .

## <a name="see-also"></a>Consulte también
[Administración del sistema](managing-your-multipoint-services-system.md) 
 MultiPoint Server [Información importante sobre el cumplimiento](./multipoint-software-license-compliance.md) 
 de la licencia de software [Administrar tareas del sistema mediante Multipoint Manager](Manage-System-Tasks-Using-MultiPoint-Manager.md) 
 [Administrar archivos](Manage-User-Files.md) 
 de usuario [Administrar escritorios](manage-user-desktops-using-multipoint-dashboard.md) 
 de usuario [Suspender y dejar activa](Suspend-and-Leave-User-Session-Active.md) 
 la sesión de usuario [Ver el estado](View-User-Connection-Status.md) 
 de conexión del usuario [Administrar el hardware](Manage-Station-Hardware.md) 
 de la estación [Configuración de una estación](Set-Up-a-Station.md) 
 [Administrar cuentas](Manage-User-Accounts.md) 
 de usuario [Actualizar o eliminar una cuenta](Update-or-Delete-a-User-Account.md) 
 de usuario [Administrar escritorios de usuario mediante Multipoint Dashboard](Manage-User-Desktops-Using-MultiPoint-Dashboard.md) 
 [Administración de sistemas Multipoint mediante Multipoint Dashboard](Manage-MultiPoint-Systems-Using-MultiPoint-Dashboard.md) 
 [Solución de problemas](Troubleshooting.md)