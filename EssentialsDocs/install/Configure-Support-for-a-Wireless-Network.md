---
title: "Configurar la compatibilidad con una red inalámbrica"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d7020d4-fd46-4858-a406-de5c0f21ea06
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c5c98727b81bf37fdb3f90c612270462a51908c8
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="configure-support-for-a-wireless-network"></a>Configurar la compatibilidad con una red inalámbrica

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puedes configurar el sistema operativo para admitir una red inalámbrica. Deben cumplirse los siguientes requisitos para habilitar la compatibilidad con inalámbrica en el servidor:  
  
-   El servidor debe tener un adaptador de red inalámbrica instalado.  
  
-   El controlador correcto del adaptador de red inalámbrica debe estar instalado previamente si el adaptador de red no es compatible con el sistema operativo.  
  
-   La capacidad de habilitar y deshabilitar al adaptador de red inalámbrica debe estar disponible. Para hacer esto puede incluir un botón físico en el servidor o una interfaz de usuario personalizada en el panel. El manual del producto debe proporcionar los pasos para habilitar y deshabilitar al adaptador de red inalámbrica.  
  
-   La capacidad de seleccionar una red inalámbrica y conectarse a ella debe estar disponible. Esto debe hacerse agregando una interfaz de usuario personalizada al panel. El manual del producto debe proporcionar los pasos para seleccionar y conectarse a una red inalámbrica.  
  
-   Si es necesaria la capacidad de admitir una red inalámbrica ad-hoc, debe proporcionarse una interfaz de usuario ampliada en el panel. La interfaz de usuario puede ser un botón o un vínculo que se inicia la configuración de un asistente de red inalámbrica Ad-hoc en el panel de control en Windows Server 2008 R2.  
  
## <a name="additional-considerations"></a>Consideraciones adicionales  
 La siguiente información también debe tenerse en cuenta al configurar la compatibilidad con una red inalámbrica:  
  
-   El servidor debe estar conectado a la red mediante un cable para ejecutar el programa de instalación.  
  
-   Un equipo de red en el que se realice una reconstrucción completa debe estar conectado a la red mediante un cable.  
  
-   El servidor debe estar conectado a la red mediante un cable para realizar una restauración de reconstrucción completa del servidor.  
  
-   Si se crea una red ad-hoc en el servidor, el adaptador de red inalámbrica está dedicado para la red ad-hoc, por lo que el usuario siempre debe conectar el cable de red en el servidor para obtener una conexión a Internet.  
  
> [!NOTE]
>  Para obtener más información acerca de cómo configurar las conexiones de red, consulta [preconfigurar un enrutador](Preconfiguring-a-Router.md).  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)