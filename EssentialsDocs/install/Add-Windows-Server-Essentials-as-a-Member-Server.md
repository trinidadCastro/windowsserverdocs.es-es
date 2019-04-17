---
title: Agregar Windows Server Essentials como un servidor miembro
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d09dd82f-f7d2-47ce-862d-fd9869f2021c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8fb73f8186d3984c9e93f7a6e39cb72a54db1e58
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="add-windows-server-essentials-as-a-member-server"></a>Agregar Windows Server Essentials como un servidor miembro

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

En este tema se aplica a un servidor que ejecuta Windows Server 2012 R2 Standard, Windows Server 2012 R2 Datacenter o Windows Server 2016 con el rol de Windows Server Essentials Experience instalado. En el resto de este documento, el rol de Windows Server Essentials Experience se se conocen como Windows Server Essentials.  
  
> [!NOTE]
>   Solo se puede implementar Windows Server Essentials como controlador de dominio. En este documento, Windows Server Essentials no incluye Windows Server Essentials.  
  
 Windows Server Essentials no tiene que ser un servidor dentro de un dominio de Windows principal. Puedes agregar Windows Server Essentials como un servidor miembro a un entorno de dominio de Active Directory y sacar provecho de la protección de datos sencillos, acceso remoto seguro y las características de integración de nube que proporciona. Además, Windows Server Essentials se puede implementar en un entorno de Active Directory existente sin tener que un controlador de dominio. Esto te permite ampliar el almacenamiento o utilizar una sucursal para la administración y el almacenamiento local.  
  
 Puedes agregar Windows Server Essentials en los siguientes escenarios:  
  
-   Agregar Windows Server Essentials en una sucursal y unirse a él al controlador de dominio que se encuentra en la oficina principal en una ubicación distinta con las herramientas nativas. Puedes activar las características de BranchCache para el uso de ancho de banda óptimo en este servidor miembro.  
  
-   Agregar Windows Server Essentials como un servidor miembro dentro de una red de Windows Server Essentials para ayudar a ampliar el almacenamiento en la red agregando carpetas de servidor adicionales en el servidor miembro.  
  
-   Agregar Windows Server Essentials como un servidor miembro en la oficina local si el servidor principal que se ejecuta Windows Server Essentials se hospeda en Microsoft Azure u hospedado por un host de terceros. Tener Windows Server Essentials como un servidor miembro en tu sitio de office local te ayuda a optimizar el uso de ancho de banda.  
  
## <a name="adding-windows-server-essentials-as-a-member-server"></a>Agregar Windows Server Essentials como un servidor miembro  
 Para agregar Windows Server Essentials como un servidor miembro a un servidor principal que se ejecute Windows Server 2012 R2 o Windows Server Essentials en un entorno de Active Directory existente, debes realizar los siguientes pasos:  
  
1.  Unir el servidor que ejecuta Windows Server Essentials a un grupo de trabajo.  
  
2.  Unir el servidor que ejecuta Windows Server Essentials en el dominio de un servidor de Windows Server Essentials principal.  
  
3.  Configurar Windows Server Essentials Experience desde el administrador del servidor.  
  
#### <a name="to-join-windows-server-essentials-to-a-workgroup-or-domain"></a>Unir Windows Server Essentials a un dominio o grupo de trabajo  
  
1.  Después de completar la instalación de Windows Server Essentials en el segundo servidor, cierra al Asistente de configurar Windows Server Essentials.  
  
2.  En la **búsqueda**, escriba **configuración del sistema**y en los resultados de búsqueda, haz clic en **ver configuración avanzada del sistema**.  
  
3.  En **propiedades del sistema**, haz clic en el **nombre de equipo** pestaña.  
  
4.  En **nombre de equipo**, en la **dominio** sección, haz clic en **cambio**.  
  
5.  En **cambios de dominio o el nombre de equipo**, en la **miembro** sección, elige si quieres unirte al servidor que ejecuta Windows Server Essentials para un **grupo de trabajo** o a un **dominio**.  
  
    -   Para agregar el servidor a un grupo de trabajo, escriba **grupo de trabajo**y, a continuación, haz clic en **Aceptar**.  
  
    -   Para unirte a este servidor a un dominio de Active Directory existente, escribe el nombre del dominio y, a continuación, haz clic en **Aceptar**.  
  
6.  Reiniciar el servidor para aplicar los cambios.  
  
 Después de que el servidor se ha unido al dominio s servidor principal, puedes seguir configurar Windows Server Essentials ejecutando el asistente Configurar Windows Server Essentials para desde el administrador del servidor.  
  
#### <a name="to-configure-windows-server-essentials-experience-on-a-member-server"></a>Para configurar Windows Server Essentials Experience en un servidor miembro  
  
1.  (Opcional) Cambiar el nombre del servidor, si es necesario.  
  
    > [!IMPORTANT]
    >  Después de haber configurado Windows Server Essentials Experience, no puedes cambiar el nombre del servidor.  
  
2.  Iniciar sesión en el servidor mediante el uso de tu cuenta de administrador de dominio.  
  
3.  Abre el administrador del servidor.  
  
4.  En el área de notificación de marca en **administrador del servidor**, haz clic en el marcador y, a continuación, haz clic en **configurar Windows Server Essentials**.  
  
5.  La opción de configurar el servidor como un servidor miembro y, a continuación, haz clic en **siguiente**.  
  
6.  Haz clic en **configurar** para comenzar la configuración. El proceso de configuración tarda aproximadamente 10 minutos en completarse.  
  
7.  En el escritorio, haz clic en el icono de escritorio para iniciar el panel del servidor. En la página principal, completar el **Introducción** tareas que se enumeran en la **instalación** pestaña.  
  
## <a name="see-also"></a>Consulta también  
  

-   [Instalar Windows Server Essentials](Install-Windows-Server-Essentials.md)

-   [Instalar Windows Server Essentials](../install/Install-Windows-Server-Essentials.md)

