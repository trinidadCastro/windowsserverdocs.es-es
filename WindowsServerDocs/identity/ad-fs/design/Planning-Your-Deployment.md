---
description: Más información acerca de cómo planear la implementación
ms.assetid: bb9b9e18-bf2f-4115-be77-9a165944db41
title: Planear la implementación
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: cf87eb0407ae963d74d90696ed46b90e9e5152b7
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040413"
---
# <a name="planning-your-deployment"></a>Planear la implementación

Al planear \- \( \- la colaboración basada en la Federación entre organizaciones \) con servicios de Federación de Active Directory (AD FS) \( AD FS \) , determine primero si la organización hospedará un recurso Web al que otras organizaciones de Internet tendrán acceso o si proporcionará acceso al recurso Web para los empleados de la organización. Esta determinación influye en el modo en que se implementa AD FS y es fundamental en el planeamiento de la infraestructura de AD FS.

> [!NOTE]
> Asegúrese de que todos los participantes comprenden claramente el papel que desempeña de organización en el acuerdo de federación.

En el [diseño de SSO Web federado](Federated-Web-SSO-Design.md), AD FS usa términos como el *asociado de cuenta* \( también conocido como *proveedor de identidades* en el complemento Administración de AD FS y el asociado de \- \) *recurso* \( también conocido como *usuario de confianza* en el complemento Administración \- de AD FS en \) para ayudar a diferenciar la organización que hospeda las cuentas del \( asociado \) de cuenta de la organización que hospeda los \- recursos basados en Web \( del asociado de recurso \) .

En el [Web SSO Design](Web-SSO-Design.md), la organización actúa en los dos roles de asociado de cuenta y asociado de recurso porque ofrece a sus usuarios acceso a sus aplicaciones.

En los temas siguientes se explican algunos de los conceptos de la organización del asociado AD FS. También contienen vínculos a temas en la guía de implementación de AD FS que contienen información sobre la instalación y configuración de las organizaciones de asociado de cuenta y las organizaciones de asociados de recurso en función de los objetivos de implementación de AD FS.

## <a name="in-this-section"></a>En esta sección

-   [Procedimientos recomendados para planear e implementar AD FS de forma segura](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)

-   [Planificación para la interoperabilidad con AD FS 1.x](Planning-for-Interoperability-with-AD-FS-1.x.md)

-   [Cuándo usar la delegación de identidad](When-to-Use-Identity-Delegation.md)

-   [Implementación de AD FS en la organización del asociado de cuenta](Deploying-AD-FS-in-the-Account-Partner-Organization-2012.md)

-   [Implementación de AD FS en la organización del asociado de recurso](Deploying-AD-FS-in-the-Resource-Partner-Organization-2012.md)

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)


