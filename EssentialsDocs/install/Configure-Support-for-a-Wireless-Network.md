---
title: Configure la asistencia para un red inalámbrica
description: Obtenga información acerca de cómo configurar el sistema operativo para admitir una red inalámbrica para Windows Server Essentials.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 4d7020d4-fd46-4858-a406-de5c0f21ea06
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 5474d3c0fee3dc390f85fdab498eca492f1b8937
ms.sourcegitcommit: d2224cf55c5d4a653c18908da4becf94fb01819e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2020
ms.locfileid: "97711410"
---
# <a name="configure-support-for-a-wireless-network"></a>Configure la asistencia para un red inalámbrica

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puede configurar el sistema operativo para que sea compatible con redes inalámbricas. El servidor debe cumplir con los siguientes requisitos para que sea compatible con redes inalámbricas:

-   El servidor debe tener instalado un adaptador de red por cable.

-   Es necesario preinstalar el controlador adecuado para el adaptador de red inalámbrica si no es compatible con el sistema operativo.

-   Es necesario poder habilitar y deshabilitar el adaptador de red inalámbrica. Los métodos para ello pueden ser un botón físico en el servidor o una interfaz de usuario personalizada en el Panel. El manual del producto debe indicar los pasos para habilitar y deshabilitar el adaptador de red inalámbrica.

-   Es necesario que se pueda seleccionar una red inalámbrica y conectarse a esta. Para ello debería agregarse una interfaz de usuario personalizada al Panel. El manual del producto debe indicar los pasos para seleccionar y conectarse a una red inalámbrica.

-   Si fuera necesario conectarse a una red inalámbrica ad hoc, deberá configurarse una ampliación de la interfaz de usuario en el Panel. La interfaz de usuario puede ser un botón o un enlace que inicie el Asistente de configuración de red inalámbrica ad hoc en el panel de control de Windows Server 2008 R2.

## <a name="additional-considerations"></a>Consideraciones adicionales
 También es importante tener en cuenta la información que aparece a continuación para configurar una red inalámbrica:

-   El servidor debe estar conectado a la red mediante cable para poder ejecutar la configuración.

-   Es necesario conectar a la red mediante cable un equipo de red en el que se haya ejecutado una recuperación completa.

-   El servidor ha de estar conectado a la red mediante cable para ejecutar la recuperación completa del servidor.

-   Si se crea una red ad hoc en el servidor, el adaptador de red inalámbrica será exclusivo de la red ad hoc, de forma que el usuario deberá siempre conectar el cable de red en el servidor para conectarse a Internet.

> [!NOTE]
>  Para obtener más información acerca de cómo configurar conexiones de red, consulte [Configuración previa del enrutador](Preconfiguring-a-Router.md).

## <a name="see-also"></a>Consulte también
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md) [personalizaciones adicionales](Additional-Customizations.md) [preparar la imagen para probar la implementación de](Preparing-the-Image-for-Deployment.md) [la experiencia del cliente](Testing-the-Customer-Experience.md)