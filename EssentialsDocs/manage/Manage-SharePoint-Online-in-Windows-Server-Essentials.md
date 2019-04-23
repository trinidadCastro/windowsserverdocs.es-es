---
title: Administrar SharePoint Online en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 282f3634-6de6-4691-803c-df6c3c16660d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b9b12c138e6166684b4b9e87b794444febd3c247
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830386"
---
# <a name="manage-sharepoint-online-in-windows-server-essentials"></a>Administrar SharePoint Online en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puede administrar las bibliotecas de SharePoint Online y sitios de equipo desde el panel, sin iniciar sesión en Office 365, si integra Office 365 con su servidor Windows Server Essentials. Obtenga las bibliotecas de SharePoint Online y sitios de grupo con cualquier plan de negocio de Office 365. [Averigüe cómo integrar Office 365 con su servidor](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
 Como ventaja adicional, los usuarios podrán usar la aplicación My Server 2012 R2 para tener acceso a archivos en las bibliotecas de SharePoint Online desde cualquier lugar mediante un dispositivo móvil o teléfono de Windows. [¿Dónde puedo obtener la aplicación My Server?](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)  
  
 ¿Todavía no ha probado SharePoint? [¿Qué puede hacer ahora](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/get-started-with-sharepoint-2013-HA102772778.aspx)  
  
## <a name="where-on-the-dashboard-will-i-manage-my-libraries-and-team-sites"></a>¿Dónde podré administrar mis bibliotecas y sitios de grupo en el panel?  
 Se usará la nueva **SharePoint Online** ficha, que se agrega a la **almacenamiento** área del panel al integrar Office 365 con el servidor, para administrar recursos de SharePoint Online.  

  
## <a name="what-can-i-manage-from-the-dashboard"></a>¿Qué puedo administrar desde el panel?  
  
### <a name="manage-your-online-libraries"></a>Administrar las bibliotecas en línea  
   
|-|-|  
| Agregar una biblioteca | En el **las bibliotecas de SharePoint** , utilice **agregar una biblioteca**. Podrá llevar a cabo todas las opciones habituales:<br /><br /> -Elegir un sitio de grupo y el tipo de biblioteca.<br />-Decida si desea utilizar el control de versiones.<br />-Asignar permisos de acceso.<br /><br /> **Sugerencia:** Para averiguar qué permisos de sitio de equipo heredará su biblioteca si no asigna permisos, use **ver los permisos del sitio**. |  
| Abra una biblioteca | Para trabajar con el contenido de la biblioteca, deberá abrirlo en Office 365. Seleccione la biblioteca y haga clic en **Abrir la biblioteca**. ¿Qué puede hacer con el contenido dependerá de las credenciales que usó para iniciar sesión en SharePoint Online. |  
| Cambiar los controles de versiones o los permisos de acceso | Puede usar **ver las propiedades de la biblioteca** para ver o cambiar los controles de versiones o los permisos para la biblioteca de acceso. |  
| Eliminar una biblioteca | **Advertencia:** Antes de eliminar una biblioteca de SharePoint Online asegúrese de guardar todos los archivos que quiera conservar en otra ubicación. Cuando se elimina una biblioteca de SharePoint, todo lo que se elimina permanentemente. No se podrá recuperar nada de ninguna forma.<br /><br /> Una vez que haya comprobado para asegurarse de que la biblioteca no almacena nada que necesite más adelante, seleccione la biblioteca y haga clic en **eliminar la biblioteca**. |  
  
### <a name="manage-your-team-sites"></a>Administrar los sitios de grupo  
 
|-|-|  
| Administrar sitios de grupo de SharePoint | El **administrar sitios de grupo** acción le permite iniciar sesión en Office 365 y administrar los sitios de grupo SharePoint Online. Lo que puede hacer en Office 365 se determinará por el iniciar sesión con la cuenta en línea.<br /><br /> Cuando cierre Office 365 y volver al panel, haga clic en **actualizar** para mostrar los cambios. | Ver o cambiar los permisos de sitio del equipo | Puesto que una biblioteca hereda los permisos de su sitio de equipo de forma predeterminada, es útil tener fácil acceso al sitio del equipo. Para ver o cambiar los permisos de un sitio de grupo, seleccione el sitio de grupo o cualquiera de sus bibliotecas y haga clic en **ver los permisos del sitio**.<br /><br /> **Sugerencia:** ¿Necesita ayuda con las puntas finas de los permisos de sitios de grupo de SharePoint? Hay un vínculo muy útil para [Obtener más información](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/introduction-control-user-access-with-permissions-HA102771919.aspx?CTT=5&origin=HA102771924) en los permisos de sitios de grupo.  
  
## <a name="tips"></a>Sugerencias  
  
-   **Haga clic en Actualizar para mostrar los últimos cambios realizados en el portal de Office 365.** Deberá actualizar la pantalla después de abrir Office 365 para administrar SharePoint Online. Si mantiene abierto el panel durante mucho tiempo, haga clic en **Actualizar** para asegurarse de que está viendo los cambios más recientes.  
  
-   **Lo que puede hacer en SharePoint Online depende de si está trabajando en el panel o en Office 365.** En el panel, se realizan cambios de SharePoint Online con la cuenta de administrador para la integración de Office 365. Pero cuando inicia sesión Office 365 desde el panel, los permisos de acceso para la cuenta en línea que utilice determinan lo que puede hacer.  
  
     Para buscar la cuenta de administrador para la integración de Office 365, abra el **Office 365** pestaña del panel.  
  
## <a name="other-things-you-might-want-to-do"></a>Otras acciones que tal vez quiera efectuar  
  
-   [Usar la aplicación My Server para trabajar con las bibliotecas de SharePoint Online desde cualquier lugar](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)  
  
-   [Más información sobre cómo asignar permisos para los sitios de grupo de SharePoint](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/introduction-control-user-access-with-permissions-HA102771919.aspx?CTT=5&origin=HA102771924)  
  
-   [Descubra lo que puede hacer con las características de SharePoint](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/get-started-with-sharepoint-2013-HA102772778.aspx)  
  
-   [Eche un vistazo a los planes de negocio de Office 365 que están disponibles](https://office.microsoft.com/business/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_what-is-office-365-for_Text)  
  
-   [Integrar Office 365 con su servidor](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
-   [Administrar otros servicios en línea de Microsoft desde el panel](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
