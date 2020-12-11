---
description: 'Más información acerca de: revisar el rol del servidor de Federación en el asociado de cuenta'
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: Revisar el rol del servidor de federación del asociado de cuenta
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: ccde30456f09d2bb86053db457db5758fd03185b
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049403"
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>Revisar el rol del servidor de federación del asociado de cuenta

Un servidor de Federación en Servicios de federación de Active Directory (AD FS) \( AD FS \) funciona como un emisor de tokens de seguridad. Un servidor de Federación genera notificaciones basadas en valores de cuenta que residen en un almacén de atributos locales y los empaqueta en tokens de seguridad para que los usuarios puedan acceder sin problemas a \- las aplicaciones basadas en el explorador Web \- \( mediante el inicio de sesión único en \- \( SSO \) \) que se hospedan en una organización del asociado de recurso.

> [!NOTE]
> Cuando los usuarios acceden a aplicaciones federadas mediante un explorador Web, un servidor de Federación emite automáticamente las cookies a los usuarios para mantener el estado de inicio de sesión de la \- \- aplicación basada en el explorador Web. Estas cookies contienen notificaciones para los usuarios. Las cookies habilitan las capacidades de SSO para que los usuarios no tengan que escribir credenciales cada vez que visiten \- \- aplicaciones basadas en explorador Web diferentes en el asociado de recurso.

En el diseño SSO Web, las organizaciones con una red perimetral que quiera que los usuarios de Internet tengan acceso a las aplicaciones deben instalar un servidor proxy de Federación en la red perimetral. En el diseño de SSO Web federado, debe haber al menos un servidor de Federación instalado en la red corporativa de la organización del asociado de cuenta y al menos un servidor de Federación instalado en la red corporativa de la organización del asociado de recurso.

> [!NOTE]
> Para poder configurar un equipo de servidor de Federación en la organización del asociado de cuenta, primero debe unir el equipo a cualquier dominio del bosque de Active Directory donde se utilizará el servidor de Federación para autenticar a los usuarios de ese bosque. Para obtener más información, consulte [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
