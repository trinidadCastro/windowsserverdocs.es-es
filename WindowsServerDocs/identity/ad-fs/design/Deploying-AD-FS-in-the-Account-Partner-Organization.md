---
description: Más información acerca de la implementación de AD FS heredadas en la organización del asociado de cuenta
ms.assetid: 8c3536b7-d091-4ee6-ad04-24713f070862
title: Implementación de AD FS heredadas en la organización del asociado de cuenta
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 4310fd72068516403b8090389de86d885d672956
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044733"
---
# <a name="deploying-legacy-ad-fs-in-the-account-partner-organization"></a>Implementación de AD FS heredadas en la organización del asociado de cuenta

Un asociado de cuenta en Servicios de federación de Active Directory (AD FS) \( AD FS \) representa la organización en la relación de confianza de Federación que almacena físicamente las cuentas de usuario en un almacén de atributos compatible. Para obtener más información sobre qué almacenes de atributos se admiten, vea [el rol de almacenes de atributos](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).

El servidor de Federación de la organización del asociado de cuenta autentica a los usuarios locales y crea tokens de seguridad usados por el asociado de recurso para tomar decisiones de autorización. Los usuarios de confianza como los sitios web y los servicios Web pueden registrarse fácilmente con el servidor de Federación y consumir tokens emitidos para la autenticación y el control de acceso.

En escenarios en los que es necesario proporcionar a los usuarios acceso a varias aplicaciones o servicios federados, cuando cada aplicación o servicio está hospedado en una organización diferente, puede configurar el servidor de Federación del asociado de cuenta para que pueda implementar varios usuarios de confianza.

Para obtener más información acerca de la instalación y configuración de una organización de asociado de cuenta, consulte [Checklist: Configuring the Account Partner Organization](../../ad-fs/deployment/Checklist--Configuring-the-Account-Partner-Organization.md).

## <a name="in-this-section"></a>En esta sección

-   [Revisar el rol del servidor de federación en el asociado de cuenta](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)

-   [Revisar el rol del servidor proxy de federación en el asociado de cuenta](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)

-   [Preparar los equipos cliente en el asociado de cuenta](Prepare-Client-Computers-in-the-Account-Partner.md)

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
