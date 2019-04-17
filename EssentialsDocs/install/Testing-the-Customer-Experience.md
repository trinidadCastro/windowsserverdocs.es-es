---
title: Prueba la experiencia del cliente
description: "Describe cómo usar Windows Server Essentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="testing-the-customer-experience"></a>Prueba la experiencia del cliente

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Para comprobar la experiencia del cliente y comprobar las personalizaciones de partner, ejecuta la configuración inicial de un equipo de destino. Se recomienda que se complete la configuración inicial al menos una vez manualmente para recorrer la experiencia del cliente. Si la marca del panel es compartida debe completar la configuración inicial para comprobar la personalización de marca. Si la marca del sitio acceso Web remoto es compartida, debe tener acceso a http://<servername\> para comprobar la personalización de marca (< servername\ > es el nombre del servidor). Puedes usar la sección de la configuración inicial del archivo cfg.ini para automatizar las pruebas de la experiencia del cliente. Para obtener más información sobre la creación de esta sección en el archivo cfg.ini consulta [crear el archivo Cfg.ini](Create-the-Cfg.ini-File.md).  
  
> [!IMPORTANT]
>  Debes ejecutar el comando Sysprep.exe para preparar la imagen para implementación antes de probar la experiencia de configuración inicial. Para obtener más información acerca de cómo ejecutar Sysprep.exe, consulta [preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md).  
  
> [!IMPORTANT]
>  Se requiere una conexión de red para probar la configuración inicial. DHCP no está configurado ni instalado en el servidor, lo que permite pruebas de red sin interferencias.  
  
 Para comprobar la información de soporte técnico del asociado en el panel, haz clic en la flecha abajo situada junto al botón Ayuda.  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)