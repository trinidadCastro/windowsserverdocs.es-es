---
ms.assetid: ad61c586-ba8a-4534-8824-b45994d60c6b
title: Comprobar que un servidor de federación está operativo
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: d33c4d6b7c674e8a1029f47eb4cfffdfc2d61f7e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969732"
---
# <a name="verify-that-a-federation-server-is-operational"></a>Comprobar que un servidor de federación está operativo


Puede realizar los siguientes procedimientos para comprobar que un servidor de federación está operativo; es decir, que todos los clientes de la misma red pueden acceder a un nuevo servidor de federación.

Para completar este procedimiento, se requiere como mínimo la pertenencia a **Usuarios**, **Operadores de copia de seguridad**, **Usuarios avanzados**, **Administradores** o equivalente.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).

### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>Procedimiento 1: para comprobar que un servidor de Federación está operativo

1.  Para comprobar que Internet Information Services \( IIS \) está configurado correctamente en el servidor de Federación, inicie sesión en un equipo cliente que se encuentre en el mismo bosque que el servidor de Federación.

2.  Abra una ventana del explorador, en la barra de direcciones, escriba el nombre de host DNS del servidor de Federación y, a continuación, anexe/ADFS/FS/federationserverservice.asmx para el nuevo servidor de Federación, por ejemplo:

    **https://fs1.fabrikam.com/adfs/fs/federationserverservice.asmx**

3.  Presione ENTRAR y complete el siguiente procedimiento en el equipo de servidor de federación. Si ve el mensaje **hay un problema con el certificado de seguridad de este sitio web**, haga clic en **pasar a este sitio web**.

    Se espera que aparezca un documento XML con la descripción del servicio. Si esta página aparece, significa que IIS del servidor de federación está operativo y que está sirviendo correctamente a las páginas.

La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).

### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>Procedimiento 2: para comprobar que un servidor de Federación está operativo

1.  Inicie sesión en el nuevo servidor de federación como administrador.

2.  En el menú **Inicio**, escriba **Visor de eventos** y presione ENTRAR.

3.  En el panel de detalles, \- haga doble clic en **registros de aplicaciones y servicios**, haga doble \- clic en **AD FS Eventing**y, a continuación, haga clic en **admin**.

4.  En la columna ID. de **evento** , busque el ID. de evento 100. Si el servidor de Federación está configurado correctamente, verá un nuevo evento, en el registro de aplicaciones de Visor de eventos, con el ID. de evento 100. Este evento comprueba que el servidor de Federación pudo comunicarse correctamente con el Servicio de federación.

## <a name="additional-references"></a>Referencias adicionales
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)


