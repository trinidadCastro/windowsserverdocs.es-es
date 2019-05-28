---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: Instalar el servicio de rol Servicio de federación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 80a6cb2bc8e6f0fdb1a777a42f5d245f98ac3dee
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192088"
---
# <a name="install-the-federation-service-role-service"></a>Instalar el servicio de rol Servicio de federación

Ahora que ha configurado correctamente un equipo con las aplicaciones de requisitos previos y los certificados, está listo para instalar el servicio de rol Servicio de federación de Active Directory Federation Services \(AD FS\). Cuando se instala el servicio de federación en un equipo, ese equipo se convierte en un servidor de federación.  
  
> [!NOTE]  
> Para Federated Web único\-sesión\-en \(SSO\) diseño, debe tener al menos un servidor de federación en la organización del asociado de cuenta y al menos un servidor de federación en la organización del asociado de recurso . Para obtener más información, consulte [Where to Place a Federation Server](https://technet.microsoft.com/library/dd807127.aspx).  
  
Puede usar el procedimiento siguiente para instalar el servicio de rol Servicio de federación de AD FS en un equipo que se convertirá en el primer servidor de federación o en un equipo que se convertirá en un servidor de federación para una granja de servidores de federación existente.  
  
## <a name="prerequisites"></a>Requisitos previos  
Compruebe que un certificado SSL con la clave privada ya se ha instalado o se haya importado en el almacén de certificados local \(almacén Personal\) antes de iniciar este procedimiento. Si va a usar un token\-certificado emitido por una entidad de certificación de firma \(CA\), compruebe que un token\-certificado de firma con la clave privada ya se ha instalado o importado en el almacén de certificados local \(almacén Personal\) antes de iniciar este procedimiento. Como alternativa, puede crear un autoservicio\-firmado, token\-firma de certificado mediante el Asistente para agregar funciones, como se describe en este procedimiento. Para obtener más información sobre el token\-certificados de firma, vea [requisitos de certificados para servidores de federación](https://technet.microsoft.com/library/dd807040.aspx).  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  ¿Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
#### <a name="to-install-the-federation-service-role-service"></a>Cómo instalar el servicio de rol de servicio de federación  
  
1.  En el **iniciar** , escriba**administrador del servidor**, y, a continuación, presione ENTRAR.  
  
2.  Haga clic en **administrar**y, a continuación, haga clic en **agregar Roles y características** para iniciar el Asistente de las características y agregar Roles.  
  
3.  En la página **Antes de comenzar** , haga clic en **Siguiente**.  
  
4.  En el **Seleccionar tipo de instalación** página, haga clic en **rol\-características o en\-instalación basada en**y haga clic en **siguiente**.  
  
5.  En el **Seleccionar servidor de destino** página, haga clic en **seleccionar un servidor del grupo de servidores**, compruebe que el equipo de destino está resaltado y, a continuación, haga clic en **siguiente**.  
  
6.  En el **seleccionar roles de servidor** página, haga clic en **Active Directory Federation Services**y, a continuación, haga clic en siguiente.  
  
    > [!NOTE]  
    > Si se le solicite instalar características adicionales de .NET Framework o Windows Process Activation Service, haga clic en **agregar características** para instalarlos.  
  
7.  En el **seleccionar características** , comprueba que se establecen las características y, a continuación, haga clic en **siguiente**.  
  
8.  En el **servicio de federación de Active Directory \(AD FS\)**  página, haga clic en **siguiente**.  
  
9. En el **seleccionar servicios de rol** , seleccione el **servicio de federación** casilla de verificación y, a continuación, haga clic en **siguiente**.  
  
10. En el **rol de servidor Web \(IIS\)**  página, haga clic en **siguiente**.  
  
11. En la página **Seleccionar servicios de rol**, haga clic en **Siguiente**.  
  
12. Después de comprobar la información de la página **Confirmar selecciones de instalación** , selecciona la casilla **Reiniciar automáticamente el servidor de destino en caso necesario** y haz clic en **Instalar**.  
  
13. En la página **Progreso de la instalación** , comprueba que todo se ha instalado correctamente y haz clic en **Cerrar**.  
  

