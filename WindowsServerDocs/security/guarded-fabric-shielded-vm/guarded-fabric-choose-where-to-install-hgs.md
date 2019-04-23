---
title: Elija si desea instalar HGS en su propio bosque nuevo o en un bosque bastión existente
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 4e02cd37391e629c9b947095fe32626bd15726ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827506"
---
# <a name="choose-whether-to-install-hgs-in-its-own-dedicated-forest-or-in-an-existing-bastion-forest"></a>Elija si desea instalar HGS en su propio bosque dedicado o en un bosque bastión existente

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016


El bosque de Active Directory para HGS es confidencial porque sus los administradores tienen acceso a las claves que control de máquinas virtuales blindadas. La instalación predeterminada se configura un nuevo bosque dedicada para HGS y configurar otras dependencias. Esta opción se recomienda porque el entorno es independiente y sabe que es seguro cuando se crea. 

El requisito técnico solo para la instalación de HGS en un bosque existente es que se agregarán al dominio raíz; no se admiten los dominios que no es raíz. Pero también son los requisitos operativos y recomendaciones relacionadas con la seguridad para el uso de un bosque existente. Bosques adecuados deliberadamente se compilan para dar servicio a una función confidencial, como el bosque usando [Privileged Access Management para AD DS](https://docs.microsoft.com/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) o un [bosque mejorada seguridad administrativa entorno (ESAE)](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access-reference-material#ESAE_BM). Normalmente, estos bosques presentan las siguientes características:

- Tienen pocos administradores (independientes de los administradores del tejido)
- Tienen un número reducido de inicios de sesión
- No sean de uso generales por naturaleza 

Bosques de uso general como bosques de producción no son adecuadas para su uso por el HGS. Los bosques de fabric también son no es adecuados porque HGS deba estar aislado de los administradores del tejido.

## <a name="next-step"></a>Paso siguiente

Elija la opción de instalación que mejor se adapte a su entorno:

- [Instalar HGS en su propio bosque dedicado](guarded-fabric-install-hgs-default.md)
- [Instalar HGS en un bosque bastión existente](guarded-fabric-install-hgs-in-a-bastion-forest.md)


