---
title: Inicializar HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: c689a1d5f69731db0cb85a884f5af2ee0230e7be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865456"
---
# <a name="initialize-the-host-guardian-service-hgs"></a>Inicializar el servicio de protección de Host (HGS)

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Cuando se inicializa HGS, especifique el modo de HGS se usará para medir el estado de los hosts protegidos. Hay dos opciones mutuamente excluyentes. Para obtener información general acerca de qué modo para elegir, consulte [tejido protegido blindadas VM Guía de planeamiento y los proveedores de hospedaje](guarded-fabric-planning-for-hosters.md).

Los temas siguientes describen los pasos de implementación para cada modo:

- [Atestación de confianza de TPM (modo TPM)](guarded-fabric-initialize-hgs-tpm-mode.md)
- [Atestación de clave de host (modo de clave)](guarded-fabric-initialize-hgs-key-mode.md)
- [Atestación de administrador de confianza (modo de AD)](guarded-fabric-initialize-hgs-ad-mode.md)

Debe realizar estos pasos en un servidor físico.