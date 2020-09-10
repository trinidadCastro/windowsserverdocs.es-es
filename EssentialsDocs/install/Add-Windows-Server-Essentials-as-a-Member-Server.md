---
title: Agregar Windows Server Essentials como servidor miembro
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: d09dd82f-f7d2-47ce-862d-fd9869f2021c
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 3325799dc8fb81ab15f8cb03102b9c6248526b74
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624001"
---
# <a name="add-windows-server-essentials-as-a-member-server"></a>Agregar Windows Server Essentials como servidor miembro

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este tema se aplica a un servidor que ejecuta Windows Server 2012 R2 Standard, Windows Server 2012 R2 Datacenter o Windows Server 2016 con el rol de experiencia con Windows Server Essentials instalado. En el resto de este documento, se hará referencia al rol de Experiencia con Windows Server Essentials como Windows Server Essentials.

> [!NOTE]
>   Windows Server Essentials solo se puede implementar como controlador de dominio. En este documento, Windows Server Essentials no incluye Windows Server Essentials.

 Windows Server Essentials no necesita ser un servidor principal dentro de un dominio de Windows. Puede agregar Windows Server Essentials como un servidor miembro a un entorno de dominio de Active Directory existente y aprovechar las ventajas de las características de protección de datos simple, acceso remoto seguro e integración en la nube. Además, Windows Server Essentials se puede implementar en un entorno de Active Directory existente sin necesidad de ser un controlador de dominio. Esto le permite ampliar el almacenamiento o usar una sucursal para la administración y el almacenamiento local.

 Puede agregar Windows Server Essentials en los siguientes escenarios:

-   Agregue Windows Server Essentials en una sucursal y únalo al controlador de dominio que se encuentra en la oficina central de una ubicación diferente mediante las herramientas nativas. Puede activar las características de BranchCache para el uso del ancho de banda óptimo en este servidor miembro.

-   Agregue Windows Server Essentials como un servidor miembro dentro de una red de Windows Server Essentials para ayudar a ampliar el almacenamiento de la red agregando carpetas de servidor adicionales en el servidor miembro.

-   Agregue Windows Server Essentials como un servidor miembro de la oficina local si el servidor principal que ejecuta Windows Server Essentials se hospeda en Microsoft Azure o hospedado por un proveedor de hospedaje de terceros. Al tener Windows Server Essentials como servidor miembro en el sitio de la oficina local, podrá optimizar el uso del ancho de banda.

## <a name="adding-windows-server-essentials-as-a-member-server"></a>Cómo agregar Windows Server Essentials como servidor miembro
 Para agregar Windows Server Essentials como un servidor miembro a un servidor principal que ejecute Windows Server 2012 R2 o Windows Server Essentials en un entorno de Active Directory existente, debe completar los pasos siguientes:

1.  Unir el servidor que ejecuta Windows Server Essentials a un grupo de trabajo.

2.  Una el servidor que ejecuta Windows Server Essentials al dominio de un servidor principal de Windows Server Essentials.

3.  Configure la experiencia con Windows Server Essentials desde Administrador del servidor.

#### <a name="to-join-windows-server-essentials-to-a-workgroup-or-domain"></a>Para conectar Windows Server Essentials a un dominio o grupo de trabajo

1. Después de completar la instalación de Windows Server Essentials en el segundo servidor, cierre al asistente para configurar Windows Server Essentials.

2. En el cuadro de **búsqueda**, escriba **System Settings** y, en los resultados de búsqueda, haga clic en **Ver la configuración avanzada del sistema**.

3. En **Propiedades del sistema**, haga clic en la pestaña **Nombre de equipo**.

4. En **Nombre de equipo**, en la sección **Dominio**, haga clic en **Cambiar**.

5. En **cambios**en el dominio o el nombre del equipo, en la sección **miembro** , elija si desea unir el servidor que ejecuta Windows Server Essentials a un **grupo de trabajo** o a un **dominio**.

   -   Para agregar el servidor a un grupo de trabajo, escriba **workgroup** y, a continuación, haga clic en **Aceptar**.

   -   Para unir este servidor a un dominio de Active Directory existente, escriba el nombre del dominio y, a continuación, haga clic en **Aceptar**.

6. Reinicie el servidor para aplicar los cambios.

   Después de unir el servidor al dominio del servidor principal, puede seguir configurando Windows Server Essentials ejecutando el Asistente para configurar Windows Server Essentials desde Administrador del servidor.

#### <a name="to-configure-windows-server-essentials-experience-on-a-member-server"></a>Para configurar la Experiencia con Windows Server Essentials en un servidor miembro

1.  (Opcional) Cambiar el nombre del servidor, si fuera necesario.

    > [!IMPORTANT]
    >  No se puede cambiar el nombre del servidor después de haber configurado la experiencia con Windows Server Essentials.

2.  Inicie sesión en el servidor con la cuenta de administrador de dominio.

3.  Abra el Administrador del servidor.

4.  En el área de notificación de marca del **Administrador del servidor**, haga clic en la marca y, a continuación, en **Configurar Windows Server Essentials**.

5.  Configure el servidor como servidor miembro y, a continuación, haga clic en **Siguiente**.

6.  Haga clic en **Configurar** para empezar la configuración. El proceso de configuración tarda aproximadamente 10 minutos en completarse.

7.  En el escritorio, haga clic en el icono de escritorio para iniciar el panel del servidor. En la página principal, complete las tareas **Introducción** que se indican en la pestaña **Instalación**.

## <a name="additional-references"></a>Referencias adicionales


-   [Instalación de Windows Server 2012 Essentials](Install-Windows-Server-Essentials.md)

-   [Instalación de Windows Server 2012 Essentials](../install/Install-Windows-Server-Essentials.md)

