---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: Instalar el servicio de rol Servicio de federación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 73c564e1c1117b229f759ca114b18a2d4c8002fa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408383"
---
# <a name="install-the-federation-service-role-service"></a>Instalar el servicio de rol Servicio de federación

Ahora que ha configurado correctamente un equipo con los certificados y las aplicaciones de requisitos previos, está listo para instalar el servicio de rol de Servicio de federación de Servicios de federación de Active Directory (AD FS) \(AD FS\). Al instalar el Servicio de federación en un equipo, ese equipo se convierte en un servidor de Federación.  
  
> [!NOTE]  
> Para el inicio de sesión único Web federado\-\-en \(diseño de\) SSO, debe tener al menos un servidor de Federación en la organización del asociado de cuenta y al menos un servidor de Federación en la organización del asociado de recurso. Para obtener más información, consulte [Where to Place a Federation Server](https://technet.microsoft.com/library/dd807127.aspx).  
  
Puede usar el siguiente procedimiento para instalar el servicio de rol de Servicio de federación de AD FS en un equipo que se convertirá en el primer servidor de Federación o en un equipo que se convertirá en un servidor de Federación para una granja de servidores de Federación existente.  
  
## <a name="prerequisites"></a>Requisitos previos  
Compruebe que ya se ha instalado o importado un certificado SSL con la clave privada en el almacén de certificados local \(\) de almacén personal antes de iniciar este procedimiento. Si va a usar un token\-certificado de firma emitido por una entidad de certificación \(\)CA, compruebe que ya se ha instalado o importado un token\-certificado de firma con la clave privada en el almacén de certificados local \(\) de almacén personal antes de iniciar este procedimiento. Como alternativa, puede crear un certificado de firma de\-de token\-firmado mediante el Asistente para agregar funciones, como se describe en este procedimiento. Para obtener más información sobre los certificados de firma de\-de tokens, consulte [requisitos de certificado para servidores de Federación](https://technet.microsoft.com/library/dd807040.aspx).  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y las pertenencias a grupos en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
#### <a name="to-install-the-federation-service-role-service"></a>Cómo instalar el servicio de rol de servicio de federación  
  
1.  En la pantalla **Inicio** , escriba**Administrador del servidor**y, a continuación, presione Entrar.  
  
2.  Haga clic en **administrar**y, a continuación, haga clic en **Agregar roles y características** para iniciar el Asistente para agregar roles y características.  
  
3.  En la página **Before you begin**, haz clic en **Next**.  
  
4.  En la página **Seleccionar tipo de instalación** , haga clic en **rol\-basado o en instalación basada en\-de características**y, a continuación, haga clic en **siguiente**.  
  
5.  En la página **Seleccionar servidor de destino** , haga clic en **seleccionar un servidor del grupo de servidores**, compruebe que el equipo de destino esté resaltado y, a continuación, haga clic en **siguiente**.  
  
6.  En la página **Seleccionar roles de servidor** , haga clic en **servicios de Federación de Active Directory (AD FS)** y, a continuación, haga clic en siguiente.  
  
    > [!NOTE]  
    > Si se le pide que instale características adicionales de .NET Framework o del servicio de activación de procesos de Windows, haga clic en **Agregar características** para instalarlas.  
  
7.  En la página **seleccionar características** , compruebe que las características están establecidas y, a continuación, haga clic en **siguiente**.  
  
8.  En la página **Active Directory Servicio de federación \(AD FS\)** , haga clic en **siguiente**.  
  
9. En la página **seleccionar servicios de función** , active la casilla **servicio de Federación** y, a continuación, haga clic en **siguiente**.  
  
10. En la página **rol de servidor Web \(IIS\)** , haga clic en **siguiente**.  
  
11. En la página **Seleccionar servicios de rol**, haga clic en **Siguiente**.  
  
12. Después de comprobar la información de la página **Confirmar selecciones de instalación** , selecciona la casilla **Reiniciar automáticamente el servidor de destino en caso necesario** y haz clic en **Instalar**.  
  
13. En la página **Progreso de la instalación** , comprueba que todo se ha instalado correctamente y haz clic en **Cerrar**.  
  

