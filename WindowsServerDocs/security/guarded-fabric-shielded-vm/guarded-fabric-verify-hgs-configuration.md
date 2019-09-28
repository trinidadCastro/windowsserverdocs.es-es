---
title: Verify the HGS configuration (Comprobar la configuración HGS)
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 8f01df37-f18e-4386-ae73-ecf84feaa9df
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: d219b7aa9ca1e17df3281fd756106a6f07864116
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386349"
---
# <a name="verify-the-hgs-configuration"></a>Verify the HGS configuration (Comprobar la configuración HGS)

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016


A continuación, es necesario validar que todo funciona según lo previsto. Para ello, ejecute el siguiente comando en una consola de Windows PowerShell con privilegios elevados:

```powershell
Get-HgsTrace -RunDiagnostics
```

Dado que la configuración de HGS todavía no contiene información sobre los hosts que se incluirán en el tejido protegido, el diagnóstico indicará que todavía no se puede atestar correctamente. Omita este resultado y revise la información adicional proporcionada por el diagnóstico.

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

Ejecute el diagnóstico en cada nodo del clúster de HGS.

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Implementar hosts protegidos](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)

