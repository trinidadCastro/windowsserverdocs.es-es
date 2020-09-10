---
title: Administrar SharePoint Online en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 282f3634-6de6-4691-803c-df6c3c16660d
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 66e5a2c8781e5c9602ba667e15ac789732767be3
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626089"
---
# <a name="manage-sharepoint-online-in-windows-server-essentials"></a>Administrar SharePoint Online en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puede administrar las bibliotecas y los sitios de grupo de SharePoint Online desde el panel, sin iniciar sesión en Microsoft 365, si integra Microsoft 365 con el servidor de Windows Server Essentials. Las bibliotecas de SharePoint Online y los sitios de grupo se obtienen con cualquier Microsoft 365 plan de negocio. [Averigüe cómo integrar Microsoft 365 con el servidor](Manage-Office-365-in-Windows-Server-Essentials.md)

 Como ventaja, los usuarios podrán usar la aplicación My Server 2012 R2 para tener acceso a los archivos de las bibliotecas de SharePoint Online desde cualquier lugar mediante su dispositivo móvil o Windows Phone. [¿Dónde puedo obtener la aplicación My Server?](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)

 ¿Todavía no ha probado SharePoint? [Qué puede hacer ahora](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/get-started-with-sharepoint-2013-HA102772778.aspx)

## <a name="where-on-the-dashboard-will-i-manage-my-libraries-and-team-sites"></a>¿Dónde podré administrar mis bibliotecas y sitios de grupo en el panel?
 Usará la nueva pestaña **SharePoint Online** , que se agrega al área **almacenamiento** del panel al integrar Microsoft 365 con el servidor, para administrar los recursos de SharePoint Online.


## <a name="what-can-i-manage-from-the-dashboard"></a>¿Qué puedo administrar desde el panel?

### <a name="manage-your-online-libraries"></a>Administrar las bibliotecas en línea

- **Agregue una biblioteca.** En la pestaña **Bibliotecas de SharePoint**, use **Agregar una biblioteca**. Podrá llevar a cabo todas las opciones habituales:
  - Elegir un sitio de grupo y el tipo de biblioteca.
  - Decidir si quiere usar el control de versiones.
  - Asignar permisos de acceso.
     **Sugerencia:** Para averiguar qué permisos de sitio de grupo heredará la biblioteca si no se asignan permisos, use **ver los permisos del sitio**.
- **Abra una biblioteca.** Para trabajar con el contenido de la biblioteca, deberá abrirlo en Microsoft 365. Seleccione la biblioteca y haga clic en **Abrir la biblioteca**. Lo que puede hacer con el contenido dependerá de las credenciales que use para iniciar sesión en SharePoint Online.
- **Cambiar los controles de versiones o los permisos de acceso.** Puede usar **Ver las propiedades de la biblioteca** para ver o cambiar los controles de versiones o los permisos de acceso de la biblioteca.
- **Eliminar una biblioteca.** Después de asegurarse de que la biblioteca no almacena nada que necesite después, selecciónela y haga clic en **Eliminar la biblioteca**. **ADVERTENCIA:** Antes de eliminar una biblioteca de SharePoint Online, asegúrese de guardar todos los archivos que desee conservar en otra ubicación. Al eliminar una biblioteca de SharePoint, todo se eliminará de forma permanente. No se podrá recuperar nada de ninguna forma.

### <a name="manage-your-team-sites"></a>Administrar los sitios de grupo

- **Administrar sitios de grupo de SharePoint.** La acción **administrar sitios de grupo** permite iniciar sesión en Microsoft 365 y administrar los sitios de grupo de SharePoint Online. Lo que puede hacer en Microsoft 365 estará determinado por la cuenta en línea con la que inicia sesión. Al cerrar Microsoft 365 y volver al panel, haga clic en **Actualizar** para mostrar los cambios.
- **Ver o cambiar los permisos del sitio de grupo.** Dado que una biblioteca hereda los permisos de su sitio de equipo de forma predeterminada, es útil tener un acceso sencillo al sitio de grupo. Para ver o cambiar los permisos de un sitio de grupo, seleccione el sitio de grupo o cualquiera de sus bibliotecas y haga clic en **ver los permisos del sitio**. **Sugerencia:** ¿Necesita ayuda con los puntos de los permisos de los sitios de grupo de SharePoint? Hay un vínculo muy útil para [Obtener más información](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/introduction-control-user-access-with-permissions-HA102771919.aspx?CTT=5&origin=HA102771924) en los permisos de sitios de grupo.

## <a name="tips"></a>Sugerencias

-   **Haga clic en actualizar para mostrar los últimos cambios realizados en el portal de Microsoft 365.** Deberá actualizar la pantalla después de abrir Microsoft 365 para administrar SharePoint Online. Si mantiene abierto el panel durante mucho tiempo, haga clic en **Actualizar** para asegurarse de que está viendo los cambios más recientes.

-   **Lo que puede hacer en SharePoint Online depende de si está trabajando en el panel o en Microsoft 365.** En el panel, los cambios de SharePoint Online se realizan con la cuenta de administrador para la integración de Microsoft 365. Sin embargo, cuando inicia sesión en Microsoft 365 desde el panel, los permisos de acceso de la cuenta en línea que usa determinan lo que puede hacer.

     Para averiguar la cuenta de administrador para la integración de Microsoft 365, abra la pestaña **Microsoft 365** en el panel.

## <a name="other-things-you-might-want-to-do"></a>Otras acciones que tal vez quiera efectuar

-   [Usar la aplicación My Server para trabajar con las bibliotecas de SharePoint Online desde cualquier lugar](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)

-   [Obtener más información sobre cómo asignar permisos a sitios des grupo de SharePoint](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/introduction-control-user-access-with-permissions-HA102771919.aspx?CTT=5&origin=HA102771924)

-   [Descubrir lo que puede hacer con las características de SharePoint](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/get-started-with-sharepoint-2013-HA102772778.aspx)

-   [Eche un vistazo a los planes de negocio de Microsoft 365 que están disponibles](https://office.microsoft.com/business/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_what-is-office-365-for_Text)

-   [Integrar Microsoft 365 con el servidor](Manage-Office-365-in-Windows-Server-Essentials.md)

-   [Administrar otros servicios en línea de Microsoft desde el panel](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
