---
title: Instalar o quitar paquetes de idiomas
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 98f13f63-4480-40ba-a7ef-d1d9b7582e5f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9999b78b1b0a4b1823162158b95d175f9c159091
ms.sourcegitcommit: 04637054de2bfbac66b9c78bad7bf3e7bae5ffb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87837934"
---
# <a name="install-or-remove-language-packs"></a>Instalar o quitar paquetes de idiomas

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

> [!NOTE]
>  Primero debe crear una imagen de Windows multilingüe tal y como se describe en [Language packs and Deployment](/previous-versions/windows/it-pro/windows-8.1-and-8/hh824829(v=win.10)) antes de agregar el paquete de idioma de Windows Server Essentials.

 Los paquetes de idiomas solo están disponibles para la creación de imágenes multilingües. La información de esta sección es específica para instalar o quitar paquetes de idioma en Windows Server Essentials.

> [!NOTE]
>  Si va a ejecutar la configuración inicial desde un equipo cliente que no admite idiomas de Asia Oriental, por ejemplo, ja-jp, y el inglés no está incluido en la imagen multilingüe del servidor, la página web de la configuración inicial mostrará cuadrados. Para que la página web de configuración inicial esté en inglés de forma predeterminada, la imagen multilingüe que el usuario cree debe incluir inglés.

## <a name="adding-language-packs-to-an-image"></a>Agregar paquetes de idiomas a una imagen
 Los paquetes de idiomas están disponibles en el DVD de personalización OEM. Antes de añadir paquetes de idiomas a una imagen se recomienda copiarlos al equipo del técnico.

 Para instalar los paquetes de idioma, debe utilizar el siguiente comando:

 **dism.exe/online/Add-Package/PackagePath: C: \\<ruta de acceso completa al directorio del archivo. cab \>\lp.cab**

 Por ejemplo, el siguiente comando muestra cómo agregar el paquete de idioma alemán:

 **dism.exe /online /Add-Package /PackagePath:C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**

> [!IMPORTANT]
>  También debe aplicar paquetes de idioma para Windows Server Essentials para localizar completamente el sistema operativo.

## <a name="removing-language-packs-from-an-image"></a>Eliminación de paquetes de idiomas de una imagen
 Para quitar un paquete de idioma que no desee incluir en una imagen puede usar el siguiente comando:

 **dism.exe/online/Remove-Package/PackagePath: C: \\<ruta de acceso completa al directorio del archivo. cab \>\lp.cab**

 Por ejemplo, el siguiente comando muestra cómo quitar el paquete de idioma alemán:

 **dism.exe /online /Remove-Package /PackagePath:C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**

## <a name="see-also"></a>Consulte también

 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md) [personalizaciones adicionales](Additional-Customizations.md) [preparar la imagen para probar la implementación de](Preparing-the-Image-for-Deployment.md) [la experiencia del cliente](Testing-the-Customer-Experience.md)