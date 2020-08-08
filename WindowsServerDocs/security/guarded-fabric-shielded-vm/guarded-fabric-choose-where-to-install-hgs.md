---
title: Elija si desea instalar HGS en su propio bosque nuevo o en un bosque bastión existente.
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: e1b4c015aa9b4f504d4cdf79bb2f38686588cfdd
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87989534"
---
# <a name="choose-whether-to-install-hgs-in-its-own-dedicated-forest-or-in-an-existing-bastion-forest"></a>Elija si desea instalar HGS en su propio bosque dedicado o en un bosque bastión existente.

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016


El Active Directory bosque para HGS es sensible porque sus administradores tienen acceso a las claves que controlan las máquinas virtuales blindadas.
La instalación predeterminada configurará un nuevo bosque dedicado para HGS y configurará otras dependencias.
Esta opción se recomienda porque el entorno es independiente y se sabe que es seguro cuando se crea.

El único requisito técnico para instalar HGS en un bosque existente es que se agrega al dominio raíz; no se admiten dominios no raíz. Pero también hay requisitos operativos y prácticas recomendadas relacionadas con la seguridad para usar un bosque existente.
Los bosques adecuados se compilan de forma intencionada para atender una función confidencial, como el bosque que usa [privileged Access Management para AD DS](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) o un [bosque de entorno administrativo de seguridad (ESAE) mejorado](../../identity/securing-privileged-access/securing-privileged-access-reference-material.md#esae-administrative-forest-design-approach).
Estos bosques normalmente presentan las siguientes características:

- Tienen pocos administradores (independientes de los administradores de tejido)
- Tienen un número reducido de inicios de sesión
- No son de uso general por naturaleza

Los bosques de uso general, como los bosques de producción, no son adecuados para su uso por parte de HGS.
Los bosques de tejido también son inadecuados porque HGS debe aislarse de los administradores del tejido.

## <a name="next-step"></a>Paso siguiente

Elija la opción de instalación que mejor se adapte a su entorno:

- [Instalación de HGS en su propio bosque dedicado](guarded-fabric-install-hgs-default.md)
- [Instalación de HGS en un bosque bastión existente](guarded-fabric-install-hgs-in-a-bastion-forest.md)