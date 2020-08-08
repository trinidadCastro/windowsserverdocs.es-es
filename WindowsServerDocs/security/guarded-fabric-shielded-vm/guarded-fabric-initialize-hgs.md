---
title: Inicializar HGS
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: e36451c90fd543ea49989e51832ab3104d4137c0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939596"
---
# <a name="initialize-the-host-guardian-service-hgs"></a>Inicializar el servicio de protección de host (HGS)

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Al inicializar HGS, se especifica el modo que HGS usará para medir el estado de los hosts protegidos. Hay dos opciones mutuamente excluyentes. Para obtener información general sobre qué modo elegir, consulte [Guía de planeamiento de tejido protegido y máquina virtual blindada para proveedores de hospedaje](guarded-fabric-planning-for-hosters.md).

En los temas siguientes se tratan los pasos de implementación para cada modo:

- [Atestación de confianza de TPM (modo TPM)](guarded-fabric-initialize-hgs-tpm-mode.md)
- [Atestación de clave de host (modo de clave)](guarded-fabric-initialize-hgs-key-mode.md)
- [Atestación de administrador de confianza (modo de AD)](guarded-fabric-initialize-hgs-ad-mode.md)

Debe realizar estos pasos en un servidor físico.