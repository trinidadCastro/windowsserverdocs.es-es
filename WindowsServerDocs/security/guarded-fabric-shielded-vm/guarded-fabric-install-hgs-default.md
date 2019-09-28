---
title: Instalación de HGS en un nuevo bosque
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 6dfbe24fb4d9011b48f366d7e5df92fdb80685d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386591"
---
# <a name="install-hgs-in-a-new-forest"></a>Instalación de HGS en un nuevo bosque 

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

## <a name="add-the-hgs-server-role"></a>Agregar el rol de servidor HGS

Ejecute los siguientes comandos en una sesión de PowerShell con privilegios elevados para agregar el rol de servidor HGS e instalar HGS.

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

## <a name="install-hgs"></a>Instalar HGS 

[!INCLUDE [Install HGS by default](../../../includes/install-hgs-default.md)] 

## <a name="next-steps"></a>Pasos siguientes

- Para conocer los pasos siguientes para configurar la atestación basada en TPM, consulte [inicializar el clúster de HGS mediante el modo TPM en un nuevo bosque dedicado (valor predeterminado)](guarded-fabric-initialize-hgs-tpm-mode-default.md).
- Para conocer los pasos siguientes para configurar la atestación de clave de host, consulte [inicializar el clúster de HGS mediante el modo de clave en un nuevo bosque dedicado (valor predeterminado)](guarded-fabric-initialize-hgs-key-mode-default.md).
- Para conocer los pasos siguientes para configurar la atestación basada en el administrador (en desuso en Windows Server 2019), consulte [inicializar el clúster de HGS mediante el modo ad en un nuevo bosque dedicado (valor predeterminado)](guarded-fabric-initialize-hgs-ad-mode-default.md).

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Inicializar HGS](guarded-fabric-initialize-hgs.md)


