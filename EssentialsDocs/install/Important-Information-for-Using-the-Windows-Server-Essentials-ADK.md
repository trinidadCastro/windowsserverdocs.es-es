---
title: "Información importante para usar el ADK de Windows Server Essentials"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26cb2992-1250-4672-98ee-8b870baa45d5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4dec1fdf01538ca119b991675f932d2d8ec1e097
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="important-information-for-using-the-windows-server-essentials-adk"></a>Información importante para usar el ADK de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Para crear y personalizar una imagen de Windows Server Essentials, usan muchas de las herramientas en el [ADK de Windows 8](https://go.microsoft.com/fwlink/?LinkId=248647), pero existen algunas diferencias importantes entre el ADK de Windows 8 y Windows Server Essentials ADK.  
  
 Debes tener en cuenta las siguientes diferencias importantes:  
  
-   Algunas opciones de configuración se han cambiado en **%windir%\setup\script\SetupComplete.cmd**. Si quieres usar este comando, puedes agregar cmdlines adicionales, pero no quite las líneas existentes.  
  
## <a name="working-with-passwords"></a>Trabajar con las contraseñas  
  
-   La contraseña de administrador se establece en Admin@123 y se habilita el inicio de sesión automático en el Install.wim\unattend.xml. Por lo tanto, no es necesario volver a escribir la contraseña varias veces durante la configuración inicial del servidor. Si tienes un personalizadas unattend.xml en la raíz del medio extraíble, se sobrescribirá esta configuración y tendrás que establecer la contraseña y el inicio de sesión durante el inician...  
  
-   Durante la configuración inicial, se pide al usuario final para crear una nueva cuenta y contraseña. Esta nueva cuenta se convierte en la cuenta de administrador de red para el sistema operativo. A continuación, se deshabilita el inicio de sesión de cuenta y automática de administrador. Puede automatizar este proceso mediante el archivo de cfg.ini de calidad.  
  
-   Consulte la [ADK de Windows 8](https://go.microsoft.com/fwlink/?LinkId=248694) documentación para obtener más información sobre cómo crear un archivo unattend.xml.  
  
## <a name="see-also"></a>Consulta también  

 [Tareas iniciales con el ADK de Windows Server Essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)

 [Tareas iniciales con el ADK de Windows Server Essentials](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Crear y personalizar la imagen](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](../install/Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](../install/Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](../install/Testing-the-Customer-Experience.md)

