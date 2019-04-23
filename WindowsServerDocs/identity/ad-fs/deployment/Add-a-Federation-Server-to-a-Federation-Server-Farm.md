---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: Agregar un servidor de federación a una granja de servidores de federación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d67f4c252ad25a05f11b88771f12fd01d13137d4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880396"
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>Agregar un servidor de federación a una granja de servidores de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Después de instalar el servicio de rol Servicio de federación y configurar los certificados necesarios en un equipo, está listo para configurar el equipo para convertirse en un servidor de federación. Puede realizar el siguiente procedimiento para unir un equipo a una nueva granja de servidores de federación.  
  
Unir un equipo a una granja de servidores con el Asistente para configuración del servidor de federación de AD FS. Cuando use este asistente para unir un equipo a una granja existente, el equipo está configurado con una lectura\-solo copia de la base de datos de configuración de AD FS y debe recibir las actualizaciones desde un servidor de federación principal.  
  
> [!NOTE]  
> Para Federated Web único\-sesión\-en \(SSO\) diseño, debe tener al menos un servidor de federación en la organización del asociado de cuenta y al menos un servidor de federación en la organización del asociado de recurso . Para obtener más información, consulte [Where to Place a Federation Server](https://technet.microsoft.com/library/dd807127.aspx).  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  ¿Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>Para agregar un servidor de federación a una granja de servidores de federación  
  
1.  Hay dos maneras de iniciar al Asistente para configuración del servidor de federación de AD FS. Para iniciar el asistente, realiza una de las acciones siguientes:  
  
    -   Una vez completada la instalación del servicio de rol Servicio de federación, abra el complemento Administración de AD FS\-en y haga clic en el **Asistente para configuración de AD FS Federation Server** vincular en el **Introducción** página o en el **acciones** panel.  
  
    -   En cualquier momento después de que el Asistente para la instalación está completa, abra el Explorador de Windows, navegue hasta la **C:\\Windows\\ADFS** carpeta y double\-haga clic en **FsConfigWizard.exe**.  
  
2.  En la página **Bienvenido**, compruebe que **Agregar servidor de federación a un Servicio de federación existente** está seleccionado y, a continuación, haga clic en **Siguiente**.  
  
3.  Si la base de datos de AD FS que seleccionó ya existe, el **existente de AD FS Configuration Database detectada** aparece la página. Si aparece, haga clic en **Eliminar base de datos** y, a continuación, en **Siguiente**.  
  
    > [!CAUTION]  
    > Seleccione esta opción solo cuando esté seguro de que los datos de esta base de datos de AD FS no son importantes o que no se utiliza en una granja de servidores de federación de producción.  
  
4.  En la página **Especificar Servidor de federación principal y Cuenta de servicio**, en **Nombre del servidor de federación principal**, escriba el nombre del equipo del servidor de federación principal de la granja y, a continuación, haga clic en **Examinar**. En el cuadro de diálogo **Examinar**, ubique la cuenta de dominio que usan como cuenta de servicio los demás servidores de federación de la granja de servidores de federación y, a continuación, haga clic en **Aceptar**. Escriba la contraseña y confírmela y, a continuación, haga clic en **siguiente**:  
  
    > [!NOTE]  
    > Para obtener más información acerca de cómo especificar una cuenta de servicio para una granja de servidores de federación, consulte [configurar manualmente una cuenta de servicio para una granja de servidores de federación](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md). Cada servidor de federación en la granja de servidores de federación debe especificar la misma cuenta de servicio para la granja de servidores sea operativa. Por ejemplo, si la cuenta de servicio que se creó es contoso\\ADFS2SVC, cada equipo configure para el rol de servidor de federación y que participará en la misma granja debe especificar contoso\\ADFS2SVC en este paso el Asistente de configuración de servidor de federación para la granja de servidores sea operativa.  
  
5.  En la página **Listo para aplicar configuración**, comprueba los detalles. Si la configuración parece ser correcta, haga clic en **siguiente** para comenzar a configurar AD FS con esta configuración.  
  
6.  En la página **Resultados de la configuración** , comprueba los resultados. Cuando haya finalizado todos los pasos de configuración, haga clic en **Cerrar**  para salir del asistente.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Cómo configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  

