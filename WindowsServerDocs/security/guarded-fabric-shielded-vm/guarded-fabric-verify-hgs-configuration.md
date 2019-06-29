---
title: Verify the HGS configuration (Comprobar la configuración HGS)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 8f01df37-f18e-4386-ae73-ecf84feaa9df
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8098edd1eea475cea1face5541459b262364a07b
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469550"
---
# <a name="verify-the-hgs-configuration"></a>Verify the HGS configuration (Comprobar la configuración HGS)

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016


A continuación, se debe validar que todo funciona según lo previsto. Para ello, ejecute el siguiente comando en una consola de Windows PowerShell con privilegios elevados:

```powershell
Get-HgsTrace -RunDiagnostics
```

Dado que la configuración de HGS todavía no contiene información acerca de los hosts que se incluirán en el tejido protegido, los diagnósticos indicará que no hay hosts podrán dar fe correctamente todavía. Pasar por alto este resultado y revise el resto de información proporcionada por los diagnósticos.

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

Ejecute los diagnósticos en cada nodo del clúster de HGS.

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Implementar hosts protegidos](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)

