---
title: Información importante para el uso del ADK de Windows Server Essentials
description: Obtenga información sobre las diferencias importantes entre el ADK de Windows 8 y el ADK de Windows Server Essentials.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 26cb2992-1250-4672-98ee-8b870baa45d5
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 36cdbc03f6b241026f24c811cb69d5331f71bae7
ms.sourcegitcommit: e00e789dff216dbade861e61365f078b758a5720
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/23/2020
ms.locfileid: "97755061"
---
# <a name="important-information-for-using-the-windows-server-essentials-adk"></a>Información importante para el uso del ADK de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Para crear y personalizar una imagen de Windows Server Essentials, puede usar muchas de las herramientas del ADK de [Windows 8](https://go.microsoft.com/fwlink/?LinkId=248647), pero existen algunas diferencias importantes entre el ADK de Windows 8 y el ADK de Windows Server Essentials.

 Debe tener en cuenta las siguientes diferencias:

-   Se han modificado algunos valores de configuración en **%windir%\setup\script\SetupComplete.cmd**. Si desea utilizar este comando, puede agregar más `cmdlines` , pero no quite las líneas existentes.

## <a name="working-with-passwords"></a>Cómo trabajar con contraseñas

-   La contraseña de administrador está establecida en Admin@123 y el inicio de sesión automático está habilitado en el Install.wim\unattend.xml. Por tanto, no es necesario escribir la contraseña varias veces durante la configuración inicial del servidor. Si tiene un archivo unattend.xml en la raíz del medio extraíble, esta configuración se sobrescribirá, y deberá definir la contraseña e iniciar la sesión durante el proceso de inicio.

-   Durante la Configuración inicial, el usuario final deberá crear una cuenta y contraseña nuevas. La nueva cuenta se convertirá en la cuenta de administrador de red del sistema operativo. En ese momento se deshabilitan la cuenta Administrador y el inicio de sesión automático. Se puede automatizar este proceso utilizando el archivo cfg.ini para realizar las pruebas de control de calidad.

-   Consulte la documentación del [ADK de Windows 8](https://go.microsoft.com/fwlink/?LinkId=248694) para obtener más información sobre cómo crear un archivo unattend.xml.

## <a name="see-also"></a>Consulte también

 [Introducción con el ADK de Windows Server Essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md) [crear y personalizar la imagen](Creating-and-Customizing-the-Image.md) [personalizaciones adicionales](Additional-Customizations.md) [preparar la imagen para](Preparing-the-Image-for-Deployment.md) [probar la implementación de la experiencia del cliente](Testing-the-Customer-Experience.md)

