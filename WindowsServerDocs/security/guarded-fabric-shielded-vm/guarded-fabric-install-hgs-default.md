---
title: Instalación de HGS en un nuevo bosque
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8f896b0cea49f9dd26a828a2580b59a78348763a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856608"
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


