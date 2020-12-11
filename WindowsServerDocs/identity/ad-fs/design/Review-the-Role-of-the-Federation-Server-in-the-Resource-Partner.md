---
description: 'Más información acerca de: revisar el rol del servidor de Federación en el asociado de recurso'
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: Revisar el rol del servidor de federación del asociado de recurso
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: e01c291fe3e8d96cb81cfee9c62dd1abb2f34662
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049393"
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>Revisar el rol del servidor de federación del asociado de recurso

El servidor de Federación de la organización del asociado de recurso intercepta los tokens de seguridad entrantes que envía un servidor de Federación de la cuenta, los valida y los firma y, a continuación, emite sus propios tokens de seguridad que están destinados a la \- aplicación basada en Web.

> [!NOTE]
> Cuando los usuarios federados usan sus exploradores Web para tener acceso a \- aplicaciones basadas en Web, el servidor de Federación de la organización del asociado de recurso crea una nueva cookie de autenticación y la escribe en el explorador. Esta cookie habilita las \- \- capacidades de inicio de sesión único de \( SSO \) para que los usuarios no tengan que volver a iniciar sesión en el servidor de Federación del asociado de cuenta cuando los usuarios intenten tener acceso a diferentes \- aplicaciones basadas en Web en el asociado de recurso.

En el diseño de SSO Web, se debe instalar al menos un servidor de Federación en la red perimetral. En el diseño de SSO Web federado, debe haber al menos un servidor de Federación instalado en la red corporativa de la organización del asociado de cuenta y al menos un servidor de Federación instalado en la red corporativa de la organización del asociado de recurso.

> [!NOTE]
> Para poder configurar un equipo de servidor de Federación en la organización del asociado de recurso, primero debe unir el equipo a cualquier dominio Active Directory en la organización del asociado de recurso. Para obtener más información, consulte [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

