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
ms.openlocfilehash: 43b2a90eaa987e96b716e10259f75ffaf9f136a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832576"
---
# <a name="verify-the-hgs-configuration"></a>Verify the HGS configuration (Comprobar la configuración HGS)

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016


A continuación, se debe validar que todo funciona según lo previsto. Para ello, ejecute el siguiente comando en una consola de Windows PowerShell con privilegios elevados:

```powershell
Get-HgsTrace -RunDiagnostics
```

Dado que la configuración de HGS todavía no contiene información acerca de los hosts que se incluirán en el tejido protegido, los diagnósticos indicará que no hay hosts podrán dar fe correctamente todavía. Pasar por alto este resultado y revise el resto de información proporcionada por los diagnósticos.

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

<!-- When a link is available for an updated troubleshooting guide, add a sentence like the following and create a link to the troubleshooting guide:
If failures did occur, please review the remediation steps provided or see the Troubleshooting Guide.
-->

Ejecute los diagnósticos en cada nodo del clúster de HGS.

## <a name="next-step"></a>Paso siguiente

>[!div class="nextstepaction"]
[Implementar hosts protegidos](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)

