---
title: Inicializar HGS
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 07c40b1da829239dda5210dde0dabe485f659ae0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403583"
---
# <a name="initialize-the-host-guardian-service-hgs"></a>Inicializar el servicio de protección de host (HGS)

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Al inicializar HGS, se especifica el modo que HGS usará para medir el estado de los hosts protegidos. Hay dos opciones mutuamente excluyentes. Para obtener información general sobre qué modo elegir, consulte [Guía de planeamiento de tejido protegido y máquina virtual blindada para proveedores de hospedaje](guarded-fabric-planning-for-hosters.md).

En los temas siguientes se tratan los pasos de implementación para cada modo:

- [Atestación de confianza de TPM (modo TPM)](guarded-fabric-initialize-hgs-tpm-mode.md)
- [Atestación de clave de host (modo de clave)](guarded-fabric-initialize-hgs-key-mode.md)
- [Atestación de administrador de confianza (modo de AD)](guarded-fabric-initialize-hgs-ad-mode.md)

Debe realizar estos pasos en un servidor físico.