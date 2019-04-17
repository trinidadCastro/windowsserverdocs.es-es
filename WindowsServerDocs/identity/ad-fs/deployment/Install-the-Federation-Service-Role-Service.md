---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: "Instalar el servicio de rol de servicios de federación"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c520cbe22739f2bde263e133c7feb681d824d251
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-federation-service-role-service"></a>Instalar el servicio de rol de servicios de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ahora que has configurado correctamente un equipo con las aplicaciones de requisitos previos y los certificados, estás listo para instalar el servicio de rol de servicios de federación de los servicios de federación de Active Directory \(AD FS\). Cuando se instala el servicio de federación en un equipo, dicho equipo se convierte en un servidor de federación.  
  
> [!NOTE]  
> Para el diseño federados Web Single\-Sign\-On \(SSO\), debes tener al menos un servidor de federación de la organización de partner de la cuenta y al menos un servidor de federación de la organización de partner de recurso. Para obtener más información, consulta [dónde colocar un servidor de federación](https://technet.microsoft.com/library/dd807127.aspx).  
  
Puedes usar el siguiente procedimiento para instalar el servicio de rol de servicios de federación de AD FS en un equipo que se convertirá en el primer servidor de federación o en un equipo que se convertirán en un servidor de federación de un conjunto de servidor de federación existente.  
  
## <a name="prerequisites"></a>Requisitos previos  
Comprueba que un certificado SSL con la clave privada ya se ha instalado o importado en la \(Personal store\) almacén de certificados local antes de iniciar este procedimiento. Si vas a usar un certificado de firma token\ emitido por una entidad de certificación \(CA\), comprobar que un certificado de firma token\ con la clave privada ya se ha instalado o importado en la \(Personal store\) almacén de certificados local antes de iniciar este procedimiento. Como alternativa, puedes crear un certificado de firma self\, firma token\ mediante el Asistente para agregar Roles, como se describe en este procedimiento. Para obtener más información acerca de los certificados de firma de token\, consulta [requisitos de certificado para los servidores de federación](https://technet.microsoft.com/library/dd807040.aspx).  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  ¿Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
#### <a name="to-install-the-federation-service-role-service"></a>Para instalar el servicio de rol de servicios de federación  
  
1.  En la **inicio**, escriba**administrador del servidor**, y, a continuación, presione ENTRAR.  
  
2.  Haz clic en **administrar**y, a continuación, haz clic en **agregar Roles y características** para iniciar el agregar Roles y características de asistente.  
  
3.  En la **antes de comenzar** página, haz clic en **siguiente**.  
  
4.  En la **selecciona el tipo de instalación** página, haz clic en **instalación basado en Feature\ o Role\**y haz clic en **siguiente**.  
  
5.  En la **servidor de destino selecciona** página, haz clic en **seleccionar un servidor desde el grupo de servidores**, comprueba que el equipo de destino se resalta y, a continuación, haz clic en **siguiente**.  
  
6.  En la **seleccionar roles de servidor** página, haz clic en **los servicios de federación de Active Directory**y, a continuación, haz clic en siguiente.  
  
    > [!NOTE]  
    > Si se le pide instalar características adicionales de .NET Framework o servicio de activación de procesos de Windows, haz clic en **agregar características** para su instalación.  
  
7.  En la **Select features** página, comprueba que se establecen las características y, a continuación, haz clic en **siguiente**.  
  
8.  En la **servicios de federación de Active Directory \(AD FS\)** página, haz clic en **siguiente**.  
  
9. En la **seleccione los servicios de rol**, seleccione la **servicios de federación de** y, a continuación, haz clic en **siguiente**.  
  
10. En la **rol de servidor Web \(IIS\)** página, haz clic en **siguiente**.  
  
11. En la **seleccione los servicios de rol** página, haz clic en **siguiente**.  
  
12. Después de comprobar la información en la **Confirmar selecciones de instalación** página, seleccione la **reiniciar automáticamente el servidor de destino en caso necesario** y, a continuación, haz clic en **instalar**.  
  
13. En la **progreso de la instalación** página, comprueba que todo lo que ha instalado correctamente y, a continuación, haz clic en **cerrar**.  
  

