---
description: Más información acerca de cómo inicializar el servicio de protección de host (HGS)
title: Inicializar HGS
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: f2dc8999a851aa3830a0ffb8e17c3eb9909fe2d1
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047263"
---
# <a name="initialize-the-host-guardian-service-hgs"></a>Inicializar el servicio de protección de host (HGS)

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Al inicializar HGS, se especifica el modo que HGS usará para medir el estado de los hosts protegidos. Hay dos opciones mutuamente excluyentes. Para obtener información general sobre qué modo elegir, consulte [Guía de planeamiento de tejido protegido y máquina virtual blindada para proveedores de hospedaje](guarded-fabric-planning-for-hosters.md).

En los temas siguientes se tratan los pasos de implementación para cada modo:

- [Atestación de confianza de TPM (modo TPM)](guarded-fabric-initialize-hgs-tpm-mode.md)
- [Atestación de clave de host (modo de clave)](guarded-fabric-initialize-hgs-key-mode.md)
- [Atestación de administrador de confianza (modo de AD)](guarded-fabric-initialize-hgs-ad-mode.md)

Debe realizar estos pasos en un servidor físico.
