---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: Agregar un servidor de federación a una granja de servidores de federación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 57c59241c36ba4ed657b83e7c691931f57978a7c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408527"
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>Agregar un servidor de federación a una granja de servidores de federación


Después de instalar el servicio de rol de Servicio de federación y configurar los certificados necesarios en un equipo, está listo para configurar el equipo para que se convierta en un servidor de Federación. Puede realizar el siguiente procedimiento para unir un equipo a una nueva granja de servidores de federación.  
  
Une un equipo a una granja de servidores con el Asistente para la configuración del servidor de Federación de AD FS. Cuando se usa este asistente para unir un equipo a una granja existente, el equipo se configura con una copia Read @ no__t-0only de la base de datos de configuración de AD FS y debe recibir actualizaciones de un servidor de Federación principal.  
  
> [!NOTE]  
> En el caso del diseño único Web no__t-0Sign @ no__t-1On \(SSO @ no__t-3, debe tener al menos un servidor de Federación en la organización del asociado de cuenta y al menos un servidor de Federación en la organización del asociado de recurso. Para obtener más información, consulte [Where to Place a Federation Server](https://technet.microsoft.com/library/dd807127.aspx).  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en \( [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>Para agregar un servidor de Federación a una granja de servidores de Federación  
  
1.  Hay dos maneras de iniciar el Asistente para la configuración del servidor de Federación de AD FS. Para iniciar el asistente, realiza una de las acciones siguientes:  
  
    -   Una vez completada la instalación del servicio de rol de Servicio de federación, abra el AD FS Management Snap @ no__t-0in y haga clic en el vínculo del **Asistente para configuración de servidor de Federación de AD FS** en la página **información general** o en el panel **acciones** .  
  
    -   En cualquier momento después de completar el Asistente para la instalación, abra el explorador de Windows, navegue hasta la carpeta **C: \\Windows @ no__t-2ADFS** y haga doble @ no__t-3click **FsConfigWizard. exe**.  
  
2.  En la página **Bienvenido**, compruebe que **Agregar servidor de federación a un Servicio de federación existente** está seleccionado y, a continuación, haga clic en **Siguiente**.  
  
3.  Si el AD FS base de datos que seleccionó ya existe, aparecerá la página **base de datos de configuración de AD FS existente detectada** . Si aparece, haga clic en **Eliminar base de datos** y, a continuación, en **Siguiente**.  
  
    > [!CAUTION]  
    > Seleccione esta opción solo cuando esté seguro de que los datos de esta AD FS base de datos no son importantes o no se usan en una granja de servidores de Federación de producción.  
  
4.  En la página **Especificar Servidor de federación principal y Cuenta de servicio**, en **Nombre del servidor de federación principal**, escriba el nombre del equipo del servidor de federación principal de la granja y, a continuación, haga clic en **Examinar**. En el cuadro de diálogo **Examinar**, ubique la cuenta de dominio que usan como cuenta de servicio los demás servidores de federación de la granja de servidores de federación y, a continuación, haga clic en **Aceptar**. Escriba la contraseña, confírmela y, a continuación, haga clic en **siguiente**:  
  
    > [!NOTE]  
    > Para obtener más información acerca de cómo especificar una cuenta de servicio para una granja de servidores de Federación, consulte [configuración manual de una cuenta de servicio para una granja de](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)servidores de Federación. Cada servidor de Federación de la granja de servidores de Federación debe especificar la misma cuenta de servicio para que la granja esté operativa. Por ejemplo, si la cuenta de servicio que se creó era contoso @ no__t-0ADFS2SVC, cada equipo que configure para el rol de servidor de Federación y que participará en la misma granja debe especificar contoso @ no__t-1ADFS2SVC en este paso del servidor de Federación. Asistente para configuración para que la granja esté operativa.  
  
5.  En la página **Listo para aplicar configuración**, comprueba los detalles. Si la configuración parece ser correcta, haga clic en **siguiente** para empezar a configurar AD FS con esta configuración.  
  
6.  En la página **Resultados de la configuración** , comprueba los resultados. Cuando haya finalizado todos los pasos de configuración, haga clic en **Cerrar**  para salir del asistente.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  

