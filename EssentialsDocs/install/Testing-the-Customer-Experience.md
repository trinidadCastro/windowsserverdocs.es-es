---
title: Comprobar la experiencia del cliente
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b1a2040-4cfd-48bf-8d04-3ffde9c26b9b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 223b0e1be3a53e9a7d198dc005fc8725e421db58
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838696"
---
# <a name="testing-the-customer-experience"></a>Comprobar la experiencia del cliente

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Para verificar la experiencia del cliente y comprobar las personalizaciones del asociado, ejecute la Configuración inicial de un equipo de destino. Se recomienda completar la Configuración inicial al menos una vez manualmente para comprobar la experiencia del cliente. Si ha llevado a cabo una personalización de marca conjunta del Panel, debe finalizar la configuración inicial para comprobar la personalización de marca. Si ha complementado al sitio acceso Web remoto, debe tener acceso a http://<servername\> para comprobar la personalización de marca (< servername\> es el nombre del servidor). Puede utilizar la sección de configuración inicial del archivo cfg.ini para automatizar la comprobación de la experiencia del cliente. Para obtener más información acerca de cómo crear esta sección en el archivo cfg.ini, consulte [Cómo crear el archivo Cfg.ini](Create-the-Cfg.ini-File.md).  
  
> [!IMPORTANT]
>  Ejecute el comando Sysprep.exe para preparar la imagen para su distribución antes de comprobar la experiencia de la Configuración inicial. Para obtener más información acerca de cómo ejecutar Sysprep.exe, consulte [Preparación de imagen para su distribución](Preparing-the-Image-for-Deployment.md).  
  
> [!IMPORTANT]
>  Es necesario disponer de una conexión de red para poder comprobar la Configuración inicial. DHCP no se ha configurado o instalado en el servidor, lo que permite la comprobación de red sin interferencias.  
  
 Para comprobar la información de soporte técnico del asociado, en el Panel, haga clic en la flecha abajo que encontrará junto al botón Ayuda.  
  
## <a name="see-also"></a>Vea también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparar la imagen para la implementación](Preparing-the-Image-for-Deployment.md)