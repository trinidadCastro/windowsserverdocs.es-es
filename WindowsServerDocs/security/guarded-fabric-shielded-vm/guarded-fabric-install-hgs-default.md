---
title: Instalar HGS en un bosque nuevo
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 7f04d9964f1a19e79335e50a1882f0326f15ddc1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860036"
---
# <a name="install-hgs-in-a-new-forest"></a>Instalar HGS en un bosque nuevo 

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

## <a name="add-the-hgs-server-role"></a>Agregar el rol de servidor HGS

Ejecute los siguientes comandos en una sesión de PowerShell con privilegios elevados para agregar el rol de servidor HGS e instalar HGS.

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

## <a name="install-hgs"></a>Instalar HGS 

[!INCLUDE [Install HGS by default](../../../includes/install-hgs-default.md)] 

## <a name="next-steps"></a>Pasos siguientes

- Para que obtener los pasos siguientes configurar la atestación de TPM, consulte [inicializar utilizando el modo TPM en un bosque nuevo dedicado (valor predeterminado) el clúster HGS](guarded-fabric-initialize-hgs-tpm-mode-default.md).
- Para que obtener los pasos siguientes configurar la atestación de clave de host, consulte [inicializar mediante el modo de clave en un bosque nuevo dedicado (valor predeterminado) el clúster de HGS](guarded-fabric-initialize-hgs-key-mode-default.md).
- Durante los próximos pasos para configurar la atestación basada en Administrador (en desuso en Windows Server 2019), consulte [inicializar el clúster HGS utilizando el modo de AD en un bosque nuevo dedicado (valor predeterminado)](guarded-fabric-initialize-hgs-ad-mode-default.md).

## <a name="next-step"></a>Paso siguiente

>[!div class="nextstepaction"]
[Inicializar HGS](guarded-fabric-initialize-hgs.md)


