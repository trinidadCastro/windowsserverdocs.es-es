---
title: Crear una imagen personalizada básica
description: Describe cómo usar Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884616"
---
# <a name="create-a-simple-customized-image"></a>Crear una imagen personalizada básica

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puede utilizar el siguiente procedimiento para crear una imagen personalizada sencilla:  
  
#### <a name="to-create-the-image"></a>Para crear la imagen  
  
1.  Tras la instalación del servidor, en la primera página de Configuración inicial, presione Mayús+F10 para iniciar la ventana cmd.  
  
2.  Cree un archivo SkipIC.txt bajo la raíz de la unidad del sistema.  
  
3.  Reinicie el servidor.  
  
4.  Inicie el servidor mediante una unidad flash USB o DVD que contenga el archivo unattend.xml. Para obtener información acerca de cómo crear una unidad flash USB de arranque, consulte [Create a Bootable USB Flash Drive](Create-a-Bootable-USB-Flash-Drive.md).  
  
5.  Agregue la personalización de marca al Panel. Para obtener más información sobre cómo agregar personalización de marca, consulte [Add Branding to the Dashboard, Remote Web Access, and Launchpad](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md).  
  
6.  Cree el archivo OOBE.xml para mostrar información personalizada como el nombre de la empresa, el logotipo y el CLUF. Para obtener más información acerca del archivo OOBE.xml, consulte [Create the Oobe.xml File Including Logo and EULA](Create-the-Oobe.xml-File-Including-Logo-and-EULA.md).  
  
7.  Cambie el nombre del servidor por defecto si no lo define en unattend.xml.  
  
8.  De forma predeterminada, el nombre del servidor será una cadena aleatoria. Cambie el nombre de servidor por otra cadena (como ContosoServer) y, a continuación, informe a su cliente del nuevo nombre del servidor.  
  
9. Preparar la imagen para la implementación, como se describe en [Preparing the Image for Deployment](Preparing-the-Image-for-Deployment.md).  
  
## <a name="see-also"></a>Vea también  
 [Introducción al ADK de Windows Server Essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparar la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](Testing-the-Customer-Experience.md)