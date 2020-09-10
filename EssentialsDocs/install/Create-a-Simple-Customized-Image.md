---
title: Crear una imagen personalizada básica
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 29f9a09f-e4e8-476d-ada1-ab9202a670d7
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: c4b1d0bde98ee162d099976f4ac4db49c9deb773
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623833"
---
# <a name="create-a-simple-customized-image"></a>Crear una imagen personalizada básica

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puede utilizar el siguiente procedimiento para crear una imagen personalizada sencilla:

#### <a name="to-create-the-image"></a>Para crear la imagen

1.  Tras la instalación del servidor, en la primera página de Configuración inicial, presione Mayús+F10 para iniciar la ventana cmd.

2.  Cree un archivo SkipIC.txt bajo la raíz de la unidad del sistema.

3.  Reinicie el servidor.

4.  Inicie el servidor mediante una unidad flash USB o DVD que contenga el archivo unattend.xml. Para obtener información acerca de la creación de la unidad de disco flash USB, consulte [Crear una unidad flash USB de arranque](Create-a-Bootable-USB-Flash-Drive.md).

5.  Agregue la personalización de marca al Panel. Para obtener más información acerca la personalización de marca, consulte [Cómo agregar personalizaciones de marca al Panel, al Acceso web remoto y Launchpad](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md).

6.  Cree el archivo OOBE.xml para mostrar información personalizada como el nombre de la empresa, el logotipo y el CLUF. Para obtener más información acerca del archivo OOBE.xml, consulte [Create the Oobe.xml File Including Logo and EULA](Create-the-Oobe.xml-File-Including-Logo-and-EULA.md).

7.  Cambie el nombre del servidor por defecto si no lo define en unattend.xml.

8.  De forma predeterminada, el nombre del servidor será una cadena aleatoria. Cambie el nombre de servidor por otra cadena (como ContosoServer) y, a continuación, informe a su cliente del nuevo nombre del servidor.

9. Prepare la imagen para su distribución de la forma descrita en [Preparación de la imagen para su distribución](Preparing-the-Image-for-Deployment.md).

## <a name="see-also"></a>Consulte también
 [Introducción con el ADK de Windows Server Essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md) [crear y personalizar la imagen](Creating-and-Customizing-the-Image.md) [personalizaciones adicionales](Additional-Customizations.md) [preparar la imagen para](Preparing-the-Image-for-Deployment.md) [probar la implementación de la experiencia del cliente](Testing-the-Customer-Experience.md)