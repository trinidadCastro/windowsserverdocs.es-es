---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: Instalar el servicio de rol Servicio de federación
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 5aee90e28ae233cf7c96013537d82cc77bc9ba2e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972162"
---
# <a name="install-the-federation-service-role-service"></a>Instalar el servicio de rol Servicio de federación

Ahora que ha configurado correctamente un equipo con los certificados y las aplicaciones que son requisitos previos, está listo para instalar el Servicio de federación servicio de rol de Servicios de federación de Active Directory (AD FS) (AD FS). Al instalar el Servicio de federación en un equipo, ese equipo se convierte en un servidor de Federación.

> [!NOTE]
> En el caso del diseño de inicio de sesión único (SSO) Web federado, debe tener al menos un servidor de Federación en la organización del asociado de cuenta y al menos un servidor de Federación en la organización del asociado de recurso. Para obtener más información, consulta [Dónde colocar un servidor de federación](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807127(v=ws.11)).

Puede usar el siguiente procedimiento para instalar el servicio de rol de Servicio de federación de AD FS en un equipo que se convertirá en el primer servidor de Federación o en un equipo que se convertirá en un servidor de Federación para una granja de servidores de Federación existente.

## <a name="prerequisites"></a>Requisitos previos
Compruebe que ya se ha instalado o importado un certificado SSL con la clave privada en el almacén de certificados local (almacén personal) antes de iniciar este procedimiento. Si va a utilizar un certificado de firma de tokens emitido por una entidad de certificación (CA), compruebe que ya se ha instalado o importado un certificado de firma de tokens con la clave privada en el almacén de certificados local (almacén personal) antes de iniciar este procedimiento. Como alternativa, puede crear un certificado de firma de tokens autofirmado mediante el Asistente para agregar funciones, como se describe en este procedimiento. Para obtener más información acerca de los certificados de firma de tokens, consulte [requisitos de certificado para servidores de Federación](../design/certificate-requirements-for-federation-servers.md).

La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento. Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).

#### <a name="to-install-the-federation-service-role-service"></a>Cómo instalar el servicio de rol de servicio de federación

1. En la pantalla **Inicio** , escriba**Administrador del servidor**y, a continuación, presione Entrar.

2. Haga clic en **Administrar** y luego haga clic en **Agregar roles y características** para iniciar el Asistente para agregar roles y características.

3. En la página **Antes de comenzar** , haga clic en **Siguiente**.

4. En la página **Seleccionar el tipo de instalación**, haz clic en **Instalación basada en características o en roles** y en **Siguiente**.

5. En la página **Seleccionar servidor de destino**, haz clic en **Seleccionar un servidor del grupo de servidores**, comprueba que el equipo de destino esté destacado y luego haz clic en **Siguiente**.

6. En la página **Seleccionar roles de servidor**, haga clic en **Servicios de federación de Active Directory** y luego haga clic en Siguiente.

    > [!NOTE]
    > Si se le pide que instale características adicionales de .NET Framework o del Servicio de activación de procesos de Windows, haga clic en **Agregar características** para instalarlas.

7. En la página **Seleccionar características**, compruebe que se hayan establecido las características y luego haga clic en **Siguiente**.

8. En la página **Servicios de federación de Active Directory (AD FS)**, haz clic en **Siguiente**.

9. En la página **Seleccionar servicios de rol**, active la casilla **Servicio de federación** y luego haga clic en **Siguiente**.

10. En la página **Rol Servidor web (IIS)**, haz clic en **Siguiente**.

11. En la página **Seleccionar servicios de rol**, haz clic en **Siguiente**.

12. Después de comprobar la información de la página **Confirmar selecciones de instalación**, active la casilla **Reiniciar automáticamente el servidor de destino en caso necesario** y luego haga clic en **Instalar**.

13. En la página **Progreso de la instalación**, comprueba que todo se instala correctamente y haz clic en **Cerrar**.