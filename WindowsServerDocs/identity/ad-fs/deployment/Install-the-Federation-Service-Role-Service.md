---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: Instalar el servicio de rol Servicio de federación
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6c214d18c7075187ead3122ce5b6bfc3c5c8e4d5
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86963777"
---
# <a name="install-the-federation-service-role-service"></a>Instalar el servicio de rol Servicio de federación

Ahora que ha configurado correctamente un equipo con los certificados y las aplicaciones de requisitos previos, está listo para instalar el servicio de rol Servicio de federación de Servicios de federación de Active Directory (AD FS) \( AD FS \) . Al instalar el Servicio de federación en un equipo, ese equipo se convierte en un servidor de Federación.  
  
> [!NOTE]  
> En el caso del \- diseño SSO de inicio de sesión único Web federado \- \( \) , debe tener al menos un servidor de Federación en la organización del asociado de cuenta y al menos un servidor de Federación en la organización del asociado de recurso. Para obtener más información, consulta [Dónde colocar un servidor de federación](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807127(v=ws.11)).  
  
Puede usar el siguiente procedimiento para instalar el servicio de rol de Servicio de federación de AD FS en un equipo que se convertirá en el primer servidor de Federación o en un equipo que se convertirá en un servidor de Federación para una granja de servidores de Federación existente.  
  
## <a name="prerequisites"></a>Requisitos previos  
\( \) Antes de iniciar este procedimiento, compruebe que ya se ha instalado o importado un certificado SSL con la clave privada en el almacén personal del almacén de certificados local. Si va a utilizar un certificado de firma de tokens \- emitido por una CA de entidad de certificación \( \) , compruebe que \- ya se ha instalado o importado un certificado de firma de tokens con la clave privada en el almacén personal del almacén de certificados local \( antes de \) iniciar este procedimiento. Como alternativa, puede crear un \- certificado de firma de tokens autofirmado \- mediante el Asistente para agregar funciones, como se describe en este procedimiento. Para obtener más información sobre \- los certificados de firma de tokens, consulte [requisitos de certificado para servidores de Federación](../design/certificate-requirements-for-federation-servers.md).  
  
La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \( http: \/ \/ Go.Microsoft.com \/ fwlink \/ ? LinkId \= 83477 \) .   
  
#### <a name="to-install-the-federation-service-role-service"></a>Cómo instalar el servicio de rol de servicio de federación  
  
1.  En la pantalla **Inicio** , escriba**Administrador del servidor**y, a continuación, presione Entrar.  
  
2.  Haga clic en **Administrar** y luego haga clic en **Agregar roles y características** para iniciar el Asistente para agregar roles y características.  
  
3.  En la página **Antes de comenzar** , haga clic en **Siguiente**.  
  
4.  En la página **Seleccionar tipo de instalación** , haga clic en instalación basada en ** \- características o \- en roles**y, a continuación, en **siguiente**.  
  
5.  En la página **Seleccionar servidor de destino**, haz clic en **Seleccionar un servidor del grupo de servidores**, comprueba que el equipo de destino esté destacado y luego haz clic en **Siguiente**.  
  
6.  En la página **Seleccionar roles de servidor**, haga clic en **Servicios de federación de Active Directory** y luego haga clic en Siguiente.  
  
    > [!NOTE]  
    > Si se le pide que instale características adicionales de .NET Framework o del Servicio de activación de procesos de Windows, haga clic en **Agregar características** para instalarlas.  
  
7.  En la página **Seleccionar características**, compruebe que se hayan establecido las características y luego haga clic en **Siguiente**.  
  
8.  En la página ** \( AD FS \) de servicio de Federación de Active Directory** , haga clic en **siguiente**.  
  
9. En la página **Seleccionar servicios de rol**, active la casilla **Servicio de federación** y luego haga clic en **Siguiente**.  
  
10. En la página ** \( IIS \) de rol de servidor Web** , haga clic en **siguiente**.  
  
11. En la página **Seleccionar servicios de rol**, haz clic en **Siguiente**.  
  
12. Después de comprobar la información de la página **Confirmar selecciones de instalación**, active la casilla **Reiniciar automáticamente el servidor de destino en caso necesario** y luego haga clic en **Instalar**.  
  
13. En la página **Progreso de la instalación**, comprueba que todo se instala correctamente y haz clic en **Cerrar**.  
  
