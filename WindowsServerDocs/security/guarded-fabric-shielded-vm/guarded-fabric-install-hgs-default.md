---
description: Más información acerca de cómo instalar HGS en un nuevo bosque
title: Instalación de HGS en un nuevo bosque
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 934efb699dd1c9f37219606ebc7f1b748f61eb18
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049733"
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


