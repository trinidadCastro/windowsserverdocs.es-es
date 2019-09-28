---
title: Elija si desea instalar HGS en su propio bosque nuevo o en un bosque bastión existente.
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 28c7eceefa4747a35d1b989df4a2c5e43a8d6a42
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386798"
---
# <a name="choose-whether-to-install-hgs-in-its-own-dedicated-forest-or-in-an-existing-bastion-forest"></a>Elija si desea instalar HGS en su propio bosque dedicado o en un bosque bastión existente.

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016


El Active Directory bosque para HGS es sensible porque sus administradores tienen acceso a las claves que controlan las máquinas virtuales blindadas. La instalación predeterminada configurará un nuevo bosque dedicado para HGS y configurará otras dependencias. Esta opción se recomienda porque el entorno es independiente y se sabe que es seguro cuando se crea. 

El único requisito técnico para instalar HGS en un bosque existente es que se agrega al dominio raíz; no se admiten dominios no raíz. Pero también hay requisitos operativos y prácticas recomendadas relacionadas con la seguridad para usar un bosque existente. Los bosques adecuados se compilan de forma intencionada para atender una función confidencial, como el bosque que usa [privileged Access Management para AD DS](https://docs.microsoft.com/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) o un [bosque de entorno administrativo de seguridad (ESAE) mejorado](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access-reference-material#ESAE_BM). Estos bosques normalmente presentan las siguientes características:

- Tienen pocos administradores (independientes de los administradores de tejido)
- Tienen un número reducido de inicios de sesión
- No son de uso general por naturaleza 

Los bosques de uso general, como los bosques de producción, no son adecuados para su uso por parte de HGS. Los bosques de tejido también son inadecuados porque HGS debe aislarse de los administradores del tejido.

## <a name="next-step"></a>Paso siguiente

Elija la opción de instalación que mejor se adapte a su entorno:

- [Instalación de HGS en su propio bosque dedicado](guarded-fabric-install-hgs-default.md)
- [Instalación de HGS en un bosque bastión existente](guarded-fabric-install-hgs-in-a-bastion-forest.md)


