---
title: Instalar Add-Ins
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e62e4f07-c2ba-4c5e-b30c-bdc287cd654e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d00cb6886e812ee2b780ad79e1fba44442e279ad
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="install-add-ins"></a>Instalar Add-Ins

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puedes incluir complementos en todos los equipos cliente o servidor instalando antes de crear una imagen. Estos complementos se incluirá automáticamente en todos los equipos replicados con esa imagen. Puedes instalar un complemento ejecutando el archivo .wssx, o puedes instalar archivos de complemento individuales siguiendo las instrucciones en el [documentación del SDK](https://go.microsoft.com/fwlink/?LinkID=248648)para cada tipo de complemento. Si instalas mediante un archivo .wssx, el complemento podrá desinstalarse con el administrador Add-In. Si instalas los archivos individuales, el complemento no se administra desde el Administrador de Add-In.  
  
> [!NOTE]
>  Cualquier software que se instale o descargue en el servidor (incluidas las pestañas del panel y notificaciones de estado) deberían tener una interfaz localizada que coincida con el idioma del sistema operativo que se instala en el servidor.  
  
#### <a name="to-install-an-add-in"></a>Para instalar un complemento  
  
1.  (Opcional) Si vas a instalar el complemento mediante un archivo .wssx, completa los siguientes pasos:  
  
    1.  Guarda el archivo de < AddinName\ > .wssx en el equipo de referencia.  
  
    2.  Haz doble clic en el archivo .wssx para abrir al Asistente de instalación Add-in.  
  
    3.  Sigue las instrucciones del Asistente para completar la instalación.  
  
2.  (Opcional) Instala los archivos de complemento individuales en las ubicaciones apropiadas, según se define en el SDK para cada tipo de complemento.  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)