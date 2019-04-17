---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: "Lista de comprobación: configurar AD FS para consumir reclamaciones de AD FS 1.x"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fe99487d3a770547af36f69722b442d0e2cbb8b1
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>Lista de comprobación: Configurar AD FS para consumir reclamaciones de AD FS 1.x

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-consume-claims-from-ad-fs-1x"></a>Lista de comprobación: Configurar AD FS para consumir reclamaciones de AD FS 1.x  
Esta lista de comprobación incluye las tareas que son necesarias para configurar los servicios de federación de los servicios de federación de Active Directory \(AD FS\) en Windows Server 2012 para consumir notificaciones que se envían por una 1 de AD FS. *x* servicios de federación.  
  
> [!NOTE]  
> Completar las tareas en esta lista de comprobación en orden. Cuando un vínculo de referencia tiene un procedimiento, vuelve a este tema después de completar los pasos de este procedimiento para que se puede continuar con las tareas restantes en esta lista de comprobación.  
  
![consumir notificaciones de AD FS](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de comprobación: configurar AD FS para consumir reclamaciones de AD FS 1.x**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![consumir notificaciones de AD FS](media/icon_checkboxo.gif)|Planear para la interoperabilidad entre AD FS en Windows Server 2012 y versiones anteriores de AD FS y obtener que más información sobre el identificador de nombre de tipo de notificación.|![consumir notificaciones de AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planeación para la interoperabilidad con AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![consumir notificaciones de AD FS](media/icon_checkboxo.gif)|Antes de que pueden interactuar con una versión anterior de AD FS, primero debes crear una confianza de proveedor de notificaciones en el servicio de federación de AD FS. **Nota:** no se puede crear una relación de confianza con una 1 de AD FS. *x* servicios de federación de mediante el uso de metadatos de federación.<br /><br />Cuando configuras la confianza con el procedimiento en el vínculo a la derecha, debes hacer lo siguiente en el Asistente para agregar reclamaciones proveedor de confianza para configurar esta confianza para interoperar con un 1 de AD FS. *x* servicios de federación de:<br /><br />1. en la **Seleccionar origen de datos** , seleccione **introducir datos acerca de los usuarios de confianza de terceros confianza manualmente**.<br />2. en la **Elegir perfil** página, seleccione **perfil de AD FS 1.0 y 1.1**.<br />3. en la **configurar URL** página, en **WS\ federación pasivo URL**, tipo el **dirección URL de extremo de servicios de federación** según se define en el 1 de AD FS. *x* servicios de federación del asociado.<br />4. en la **configurar identificadores** página, debajo **identificador de confianza de proveedor de reclamaciones**, tipo el **federación servicio URI** según se define en el 1 de AD FS. *x* servicios de federación del asociado.|![consumir notificaciones de AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crear un reclamaciones de proveedor de confianza de forma manual](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)|  
|![consumir notificaciones de AD FS](media/icon_checkboxo.gif)|En la confianza del proveedor de notificaciones que creaste anteriormente, debes crear una regla de notificación que te llevará notificaciones entrantes de la AD FS 1.x servicios de federación de paso a través, filtrar y transformarlos en un tipo de notificación de Id. de nombre.<br /><br />Cuando el tipo de notificación de Id. de nombre se ha pasado a través de, filtrado, o transformar, puede usarse como entrada a otra regla o reglas para que se pueda comprender y consumida por el servicio de federación de AD FS en Windows Server 2012.|![consumir notificaciones de AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crear una regla para enviar un AD FS 1.x reclamación Compatible](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![consumir notificaciones de AD FS](media/icon_checkboxo.gif)|Ponte en contacto con el administrador del 1 de AD FS. *x* servicios de federación y el administrador del 1 de AD FS. *x* servicios de federación de configurar una nueva confianza partner de recursos. Además, proporciona el Administrador con el URI de servicios de federación \ (en los servicios de federación properties\), la dirección URL de extremo de servicios de federación y un exportados token\ firma de archivo de certificado \ (con only\ de clave pública). El administrador deberá estos elementos para configurar la relación de confianza.|N\/A|  
  

