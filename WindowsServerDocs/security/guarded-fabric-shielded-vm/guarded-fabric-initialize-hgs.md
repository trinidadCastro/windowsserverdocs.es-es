---
title: Inicializar HGS
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 42f76dabbe150f229027909f8b58b843f31ae4e1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856598"
---
# <a name="initialize-the-host-guardian-service-hgs"></a>Inicializar el servicio de protección de host (HGS)

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Al inicializar HGS, se especifica el modo que HGS usará para medir el estado de los hosts protegidos. Hay dos opciones mutuamente excluyentes. Para obtener información general sobre qué modo elegir, consulte [Guía de planeamiento de tejido protegido y máquina virtual blindada para proveedores de hospedaje](guarded-fabric-planning-for-hosters.md).

En los temas siguientes se tratan los pasos de implementación para cada modo:

- [Atestación de confianza de TPM (modo TPM)](guarded-fabric-initialize-hgs-tpm-mode.md)
- [Atestación de clave de host (modo de clave)](guarded-fabric-initialize-hgs-key-mode.md)
- [Atestación de administrador de confianza (modo de AD)](guarded-fabric-initialize-hgs-ad-mode.md)

Debe realizar estos pasos en un servidor físico.