---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: Agregar un servidor de federación a una granja de servidores de federación
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ea826f8984937c5d32a830131f042a8519528268
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815058"
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>Agregar un servidor de federación a una granja de servidores de federación


Después de instalar el servicio de rol de Servicio de federación y configurar los certificados necesarios en un equipo, está listo para configurar el equipo para que se convierta en un servidor de Federación. Puede realizar el siguiente procedimiento para unir un equipo a una nueva granja de servidores de federación.  
  
Un equipo se une a una granja de servidores con el Asistente para la configuración del servidor de federación de AD FS. Cuando se usa este asistente para unir un equipo a una granja de servidores existente, el equipo se configura con una copia de solo lectura\-de la base de datos de configuración de AD FS y debe recibir actualizaciones de un servidor de Federación principal.  
  
> [!NOTE]  
> Para el inicio de sesión único Web federado\-\-en \(diseño de\) SSO, debe tener al menos un servidor de Federación en la organización del asociado de cuenta y al menos un servidor de Federación en la organización del asociado de recurso. Para obtener más información, consulta [Dónde colocar un servidor de federación](https://technet.microsoft.com/library/dd807127.aspx).  
  
La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas adecuadas y las pertenencias a grupos en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>Para agregar un servidor de Federación a una granja de servidores de Federación  
  
1.  Hay dos maneras de iniciar el Asistente para la configuración del servidor de federación de AD FS. Para iniciar el asistente, siga uno de estos procedimientos:  
  
    -   Una vez finalizada la instalación del servicio de rol de Servicio de federación, abra el\-de administración de AD FS en y haga clic en el vínculo **AD FS del Asistente para configuración de servidor de Federación** en la página **información general** o en el panel **acciones** .  
  
    -   En cualquier momento después de completar el Asistente para la instalación, abra el explorador de Windows, vaya a la carpeta **C:\\Windows\\ADFS** y haga doble\-haga clic en **FsConfigWizard. exe**.  
  
2.  En la página **Bienvenida**, compruebe que la opción **Agregar un servidor de federación a un servicio de federación existente** esté seleccionada y luego haga clic en **Siguiente**.  
  
3.  Si el AD FS base de datos que seleccionó ya existe, aparecerá la página **base de datos de configuración de AD FS existente detectada** . Si ocurre eso, haga clic en **Eliminar base de datos** y luego en **Siguiente**.  
  
    > [!CAUTION]  
    > Seleccione esta opción solo cuando esté seguro de que los datos en esta base de datos de AD FS no son importantes o no se usan en una granja de servidores de federación de producción.  
  
4.  En la página **Especificar el servidor de federación principal y la cuenta de servicio**, en **Nombre del servidor de federación principal**, escriba el nombre del equipo del servidor de federación principal en la granja y luego haga clic en **Examinar**. En el cuadro de diálogo **Examinar**, busque la cuenta de dominio que usarán como cuenta de servicio los demás servidores de federación en la granja de servidores de federación existente y luego haga clic en **Aceptar**. Escriba la contraseña, confírmela y luego haga clic en **Siguiente**.  
  
    > [!NOTE]  
    > Para obtener más información acerca de cómo especificar una cuenta de servicio para una granja de servidores de Federación, consulte [configuración manual de una cuenta de servicio para una granja de](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)servidores de Federación. Cada servidor de Federación de la granja de servidores de Federación debe especificar la misma cuenta de servicio para que la granja esté operativa. Por ejemplo, si la cuenta de servicio que se creó era contoso\\ADFS2SVC, cada equipo que configure para el rol de servidor de Federación y que participará en la misma granja debe especificar contoso\\ADFS2SVC en este paso en el Asistente para la configuración del servidor de Federación para que la granja esté operativa.  
  
5.  En la página **Listo para aplicar configuración**, revise los detalles. Si la configuración parece ser correcta, haga clic en **Siguiente** para comenzar a configurar AD FS con estos valores.  
  
6.  En la página **Resultados de configuración**, revise los resultados. Cuando haya finalizado todos los pasos de configuración, haga clic en **Cerrar**  para salir del asistente.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configuración de un servidor de Federación](Checklist--Setting-Up-a-Federation-Server.md)  
  

