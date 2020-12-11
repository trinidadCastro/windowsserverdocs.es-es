---
description: 'Más información acerca de: comprobar que el servidor de Federación de Windows Server 2012 R2 está operativo'
ms.assetid: 1115d276-00f6-4c23-9278-eedcc31295d8
title: Comprobar que el servidor de Federación de Windows Server 2012 R2 está operativo
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: dd7585af83b5070122755cdaeffbea3feb4f4f0f
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049043"
---
# <a name="verify-your-windows-server-2012-r2-federation-server-is-operational"></a>Comprobar que el servidor de Federación de Windows Server 2012 R2 está operativo



Puede realizar los siguientes procedimientos para comprobar que un servidor de federación está operativo; es decir, que todos los clientes de la misma red pueden acceder a un nuevo servidor de federación.

Para completar este procedimiento, se requiere como mínimo la pertenencia a **Usuarios**, **Operadores de copia de seguridad**, **Usuarios avanzados**, **Administradores** o equivalente.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).

### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>Procedimiento 1: para comprobar que un servidor de Federación está operativo

1.  Para comprobar que Internet Information Services \( IIS \) está configurado correctamente en el servidor de Federación, inicie sesión en un equipo cliente que se encuentre en el mismo bosque que el servidor de Federación.

2.  Abra una ventana del explorador, en la barra de direcciones, escriba el nombre de host DNS del servidor de Federación y, a continuación, anexe \/ ADFS \/ FS \/ federationserverservice. asmx al nuevo servidor de Federación, por ejemplo:

    **https: \/ \/ FS1.fabrikam.com \/ ADFS \/ FS \/ federationserverservice. asmx**

3.  Presione ENTRAR y complete el siguiente procedimiento en el equipo de servidor de federación. Si ve el mensaje **hay un problema con el certificado de seguridad de este sitio web**, haga clic en **pasar a este sitio web**.

    Se espera que aparezca un documento XML con la descripción del servicio. Si esta página aparece, significa que IIS del servidor de federación está operativo y que está sirviendo correctamente a las páginas.

La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).

### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>Procedimiento 2: para comprobar que un servidor de Federación está operativo

1.  Inicie sesión en el nuevo servidor de federación como administrador.

2.  En la pantalla **Inicio** , escriba **visor de eventos** y, a continuación, presione Entrar.

3.  En el panel de detalles, \- haga doble clic en **registros de aplicaciones y servicios**, haga doble \- clic en **AD FS Eventing** y, a continuación, haga clic en **admin**.

4.  En la columna ID. de **evento** , busque el ID. de evento 100. Si el servidor de Federación está configurado correctamente, verá un nuevo evento, en el registro de aplicaciones de Visor de eventos, con el ID. de evento 100. Este evento comprueba que el servidor de Federación pudo comunicarse correctamente con el Servicio de federación.

## <a name="see-also"></a>Consulte también

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)

[Guía de implementación de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)

[Implementar una granja de servidores de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)



