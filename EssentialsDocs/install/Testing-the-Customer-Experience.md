---
title: Comprobar la experiencia del cliente
description: Obtenga información acerca de cómo ejecutar a través de la configuración inicial de un equipo de destino para comprobar la experiencia del cliente y comprobar las personalizaciones de los asociados.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 1b1a2040-4cfd-48bf-8d04-3ffde9c26b9b
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: afe9ec09266eb57c25b0dea158a434da200573b5
ms.sourcegitcommit: e00e789dff216dbade861e61365f078b758a5720
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/23/2020
ms.locfileid: "97755351"
---
# <a name="testing-the-customer-experience"></a>Comprobar la experiencia del cliente

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Para verificar la experiencia del cliente y comprobar las personalizaciones del asociado, ejecute la Configuración inicial de un equipo de destino. Se recomienda completar la Configuración inicial al menos una vez manualmente para comprobar la experiencia del cliente. Si ha llevado a cabo una personalización de marca conjunta del Panel, debe finalizar la configuración inicial para comprobar la personalización de marca. Si su personalización de marca el sitio de acceso Web remoto, debe tener acceso a http://<ServerName \> para comprobar la personalización de marca (<ServerName \> es el nombre del servidor). Puede utilizar la sección de configuración inicial del archivo cfg.ini para automatizar la comprobación de la experiencia del cliente. Para obtener más información acerca de cómo crear esta sección en el archivo cfg.ini, consulte [Cómo crear el archivo Cfg.ini](Create-the-Cfg.ini-File.md).

> [!IMPORTANT]
>  Ejecute el comando Sysprep.exe para preparar la imagen para su distribución antes de comprobar la experiencia de la Configuración inicial. Para obtener más información acerca de cómo ejecutar Sysprep.exe, consulte [Preparación de imagen para su distribución](Preparing-the-Image-for-Deployment.md).

> [!IMPORTANT]
>  Es necesario disponer de una conexión de red para poder comprobar la Configuración inicial. DHCP no se ha configurado o instalado en el servidor, lo que permite la comprobación de red sin interferencias.

 Para comprobar la información de soporte técnico del asociado, en el Panel, haga clic en la flecha abajo que encontrará junto al botón Ayuda.

## <a name="see-also"></a>Consulte también
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md) [personalizaciones adicionales](Additional-Customizations.md) [preparar la imagen para la implementación](Preparing-the-Image-for-Deployment.md)