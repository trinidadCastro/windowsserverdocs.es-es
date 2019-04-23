---
title: Introducción a MultiPoint Services
description: Presenta MultiPoint Services y le permite comenzar a usarlo.
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aca5f0be-f253-46b5-b1e7-0bffa15f3227
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 03dcbc97fac91dfec83b257a8e50bfe49ff3bc14
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884906"
---
# <a name="getting-started-with-multipoint-services"></a>Introducción a MultiPoint Services
El sistema MultiPoint Services permite a los usuarios muchas utilizar varias estaciones que están físicamente conectadas mediante el uso de concentradores de estaciones a un solo equipo. Cada estación normalmente consta de un concentrador de estaciones, mouse, teclado y monitor de vídeo. Cada usuario en una estación de MultiPoint Services experimenta una única sesión informática de Windows que se puede administrar mediante MultiPoint Manager.  
  
Los componentes de un sistema MultiPoint Services incluyen lo siguiente:  
  
-   Software del sistema multiPoint Services, que es compatible con varios monitores, teclados, los dispositivos de mouse y otros dispositivos en el equipo.  
  
-   La aplicación MultiPoint Manager, lo que permite supervisar y realizar acciones en estaciones de MultiPoint Services.  
  
-   Herramientas de administración y mantenimiento.  
  
-   La aplicación de panel de MultiPoint, lo que permite completar las tareas diarias, como la comunicación con otros usuarios.  
  
En este archivo de ayuda se proporciona información sobre cómo administrar estaciones de MultiPoint Services con MultiPoint Manager y el panel de MultiPoint.  
  
## <a name="overview-of-multipoint-manager"></a>Introducción a MultiPoint Manager  
MultiPoint Manager proporciona cuatro pestañas para usar cuando se administran las estaciones de MultiPoint Services. Cada pestaña y las tareas que puede realizar en ellos, se describe con más detalle en cada tema de ayuda.  
  
Las pestañas son los siguientes:  
  
-   **Pestaña Inicio:** Cambiar los modos de realizar tareas administrativas, agregar o quitar servidores MultiPoint, reiniciar o apagar el equipo, habilitar la protección de disco, agregar licencias de acceso de cliente, reasignar las estaciones y obtener ayuda o soporte técnico. Para obtener más información, consulte el [Administrar sistema tareas mediante MultiPoint Manager](Manage-System-Tasks-Using-MultiPoint-Manager.md) tema.  
  
-   **Pestaña estaciones:** Ver usuarios *desktop* estado y *final* o *suspender* las sesiones de usuario. Para obtener más información, consulte el [administrar estaciones de usuario](Manage-User-Stations.md) tema.  
  
-   **Pestaña usuarios:** Crear y administrar *cuentas de usuario estándar* y *cuentas de usuario administrativo*. Para más información, vea el tema [Administrar cuentas de usuario](Manage-User-Accounts.md).  
  
-   **Pestaña de escritorios virtual:** Habilite las funciones de escritorio virtuales. Para obtener más información, consulte el [administrar escritorios virtuales](Manage-Virtual-Desktops.md) tema.  
  
## <a name="multipoint-server-management-and-maintenance"></a>Mantenimiento y administración de multiPoint Server  
Después de configurar el sistema MultiPoint Services, puede usar MultiPoint Manager para administrar MultiPoint Services.  
  
Tipos de acciones que puede realizar mediante MultiPoint Manager incluyen lo siguiente:  
  
-   **Agregar cuentas de usuario:** Usar MultiPoint Manager para crear cuentas de usuario estándar y administrativos.  
  
-   **Editar configuración del servidor:** Puede configurar el sistema MultiPoint Services para iniciar en modo de consola, permitir que una cuenta tener varias sesiones, asigne una dirección IP única a cada estación y otras tareas.  
  
-   **Cambiar a modo de consola:** Puede cambiar el sistema MultiPoint Services a modo de consola con el fin de instalar nuevo software en el sistema MultiPoint Services. Puede especificar que todos los usuarios pueden ejecutar el software o que solo se puede usar el software, dependiendo de la instalación y las opciones del software de licencia.  
  
-   **Solución de problemas:** Si experimenta problemas con MultiPoint Services, vea el [Troubleshooting](Troubleshooting.md) sección para buscar temas que pueden ayudar a solucionar el problema.  
  
## <a name="overview-of-multipoint-dashboard"></a>Información general de MultiPoint Dashboard  
Panel de multiPoint ofrece una experiencia de la cinta de opciones donde puede elegir entre dos fichas para tener acceso a las tareas diarias comunes.  
  
Las pestañas son los siguientes:  
  
-   **Pestaña Inicio:** Bloquear o desbloquear estaciones, establecer web limitar las opciones, proyecto escritorios a otros equipos de escritorio, iniciar o cerrar las aplicaciones, se comunican a través de mensajería instantánea, ayudar a otros usuarios a través del control de escritorio remoto, ajustar las vistas en miniatura de escritorio y habilitar o deshabilitar mensajería instantánea y aplicaciones iniciar automáticamente. Para obtener más información, consulte el [administrar escritorios de usuario utilizando el panel de MultiPoint](Manage-User-Desktops-Using-MultiPoint-Dashboard.md) tema.  
  
-   **Pestaña de sistemas:** Reiniciar, apagar, o la reasignación de todos los sistemas o los seleccionados. Para obtener más información, consulte el [administrar MultiPoint sistemas mediante MultiPoint Dashboard](Manage-MultiPoint-Systems-Using-MultiPoint-Dashboard.md) tema.  
  
## <a name="daily-use-of-your-multipoint-server-system"></a>Uso diario del sistema MultiPoint Server  
Cuando empiece a usar MultiPoint Services diariamente, hay información sobre cómo usar MultiPoint Services que desea compartir con los usuarios en el sistema MultiPoint Services. Esta información incluye lo siguiente:  
  
**Uso compartido de contenido y mantener privados:**  
  
-   Un usuario puede guardar un archivo o documento en una carpeta privada que solamente puede ver que el usuario.  
  
-   Los usuarios también pueden guardar documentos en una carpeta pública que sea accesible para todos los usuarios en el sistema MultiPoint Services.  
  
-   Es importante para los usuarios de MultiPoint Services saber que los usuarios administrativos tienen acceso a todos los archivos y documentos en el sistema, incluso si se almacenan de forma privada en la carpeta personal de un usuario.  
  
Para obtener más información acerca de cómo guardar y administrar el contenido privado y público, consulte el [administrar archivos de usuario](Manage-User-Files.md) tema.  
  
**Información acerca de una sesión de usuario MultiPoint Services:**  
  
-   Cada usuario tiene un nombre de usuario y contraseña y un único escritorio *sesión* en el sistema MultiPoint Services.  
  
-   Un *usuario estándar* no es un *usuario administrativo* en el sistema MultiPoint Services. Los usuarios estándar no se pueden instalar a algunos tipos de software, pero puede guardar archivos y cambiar la configuración de escritorio, excepto para la resolución de pantalla. Todos los cambios realizados por el usuario en el escritorio siguen vigentes cuando vuelve a iniciar sesión.  
  
-   Los usuarios pueden desconectarse de una estación y volver a iniciarla su sesión en una estación diferente sin perder su trabajo. Para más información, vea el tema [Suspender y dejar activa la sesión de usuario](Suspend-and-Leave-User-Session-Active.md).  
  
-   Sesión de un usuario estándar (o todas las sesiones de usuario) pueden ser desconectadas o cerradas sesión el usuario administrativo mediante MultiPoint Manager. Para obtener más información, consulte el [administrar escritorios de usuario](manage-user-desktops-using-multipoint-dashboard.md) tema.  
  
-   Si un usuario olvida su contraseña, puede restablecer la contraseña de la **usuarios** ficha, que usa la funcionalidad de administración de cuentas de usuario de Windows estándar. Para obtener más información, consulte el [actualizar o eliminar una cuenta de usuario](Update-or-Delete-a-User-Account.md) tema.  
  
## <a name="see-also"></a>Vea también  
[Administración del sistema de MultiPoint Server](managing-your-multipoint-services-system.md)  
[Información importante sobre el cumplimiento de licencia de Software](Important-Information-about-Software-License-Compliance.md)  
[Administrar tareas del sistema mediante MultiPoint Manager](Manage-System-Tasks-Using-MultiPoint-Manager.md)  
[Administrar archivos de usuario](Manage-User-Files.md)  
[Administrar escritorios de usuario](manage-user-desktops-using-multipoint-dashboard.md)  
[Suspender y dejar activa la sesión de usuario](Suspend-and-Leave-User-Session-Active.md)  
[Ver el estado de conexión de usuario](View-User-Connection-Status.md)  
[Administrar Hardware de la estación](Manage-Station-Hardware.md)  
[Configurar una estación](Set-Up-a-Station.md)  
[Administrar cuentas de usuario](Manage-User-Accounts.md)  
[Actualizar o eliminar una cuenta de usuario](Update-or-Delete-a-User-Account.md)  
[Administrar escritorios de usuario mediante MultiPoint Dashboard](Manage-User-Desktops-Using-MultiPoint-Dashboard.md)  
[Administrar sistemas MultiPoint mediante MultiPoint Dashboard](Manage-MultiPoint-Systems-Using-MultiPoint-Dashboard.md)  
[Solución de problemas](Troubleshooting.md)    
  
