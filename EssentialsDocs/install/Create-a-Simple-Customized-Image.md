---
title: Crear una sencilla imagen personalizada
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 29f9a09f-e4e8-476d-ada1-ab9202a670d7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e18ff5ded94127449072d28d00b98e17dbe63c3a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="create-a-simple-customized-image"></a>Crear una sencilla imagen personalizada

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puedes usar el siguiente procedimiento para crear una imagen personalizada simple:  
  
#### <a name="to-create-the-image"></a>Para crear la imagen  
  
1.  Después de la instalación del servidor, en la primera página de configuración inicial, presiona MAYÚS + F10 para iniciar la ventana cmd.  
  
2.  Crear un archivo de SkipIC.txt en la raíz de la unidad del sistema.  
  
3.  Reiniciar el servidor.  
  
4.  Inicia el servidor mediante una unidad flash USB o DVD, que incluye el archivo unattend.xml. Para obtener información sobre cómo crear una unidad flash USB de arranque, vea [crear una unidad Flash de USB de arranque](Create-a-Bootable-USB-Flash-Drive.md).  
  
5.  Agregar personalización de marca de logotipo al panel. Para obtener más información sobre cómo agregar personalización de marca, consulta [agregar personalización de marca en el panel, acceso Web remoto y del Launchpad](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md).  
  
6.  Crear el archivo OOBE.xml para mostrar información personalizada, como el nombre de la compañía, logotipo y términos de licencia. Para obtener más información sobre el archivo OOBE.xml, consulta [crear el CLUF y el logotipo como de archivo Oobe.xml](Create-the-Oobe.xml-File-Including-Logo-and-EULA.md).  
  
7.  Cambiar el nombre del servidor de forma predeterminada si no se define en unattend.xml.  
  
8.  De manera predeterminada, el nombre del servidor será una cadena aleatoria. Cambiar el nombre del servidor en otra cadena (por ejemplo, ContosoServer) y, a continuación, informar a sus clientes sobre el nuevo nombre de servidor.  
  
9. Preparar la imagen para la implementación, como se describe en [preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md).  
  
## <a name="see-also"></a>Consulta también  
 [Tareas iniciales con el ADK de Windows Server Essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)