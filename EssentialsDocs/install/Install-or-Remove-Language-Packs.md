---
title: Instalar o quitar paquetes de idiomas
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98f13f63-4480-40ba-a7ef-d1d9b7582e5f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4f7657f3ecb3450b5253d1cfe2813c12ecc2718e
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311721"
---
# <a name="install-or-remove-language-packs"></a>Instalar o quitar paquetes de idiomas

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

> [!NOTE]
>  Primero debe crear una imagen de Windows multilingüe tal y como se describe en [Language packs and Deployment](https://technet.microsoft.com/library/hh824829) antes de agregar el paquete de idioma de Windows Server Essentials.  
  
 Los paquetes de idiomas solo están disponibles para la creación de imágenes multilingües. La información de esta sección es específica para instalar o quitar paquetes de idioma en Windows Server Essentials.  
  
> [!NOTE]
>  Si va a ejecutar la configuración inicial desde un equipo cliente que no admite idiomas de Asia Oriental, por ejemplo, ja-jp, y el inglés no está incluido en la imagen multilingüe del servidor, la página web de la configuración inicial mostrará cuadrados. Para que la página web de configuración inicial esté en inglés de forma predeterminada, la imagen multilingüe que el usuario cree debe incluir inglés.  
  
## <a name="adding-language-packs-to-an-image"></a>Agregar paquetes de idiomas a una imagen  
 Los paquetes de idiomas están disponibles en el DVD de personalización OEM. Antes de añadir paquetes de idiomas a una imagen se recomienda copiarlos al equipo del técnico.  
  
 Para instalar los paquetes de idioma, debe utilizar el siguiente comando:  
  
 **DISM. exe/online/Add-Package/PackagePath: C:\\< ruta de acceso completa al directorio del archivo. cab\>\lp.cab**  
  
 Por ejemplo, el siguiente comando muestra cómo agregar el paquete de idioma alemán:  
  
 **DISM. exe/online/Add-Package/PackagePath: C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
> [!IMPORTANT]
>  También debe aplicar paquetes de idioma para Windows Server Essentials para localizar completamente el sistema operativo.  
  
## <a name="removing-language-packs-from-an-image"></a>Eliminación de paquetes de idiomas de una imagen  
 Para quitar un paquete de idioma que no desee incluir en una imagen puede usar el siguiente comando:  
  
 **DISM. exe/online/Remove-Package/PackagePath: C:\\< ruta de acceso completa al directorio del archivo. cab\>\lp.cab**  
  
 Por ejemplo, el siguiente comando muestra cómo quitar el paquete de idioma alemán:  
  
 **DISM. exe/online/Remove-Package/PackagePath: C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
## <a name="see-also"></a>Consulta también  

 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](Testing-the-Customer-Experience.md)

 [Crear y personalizar la imagen](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](../install/Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](../install/Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](../install/Testing-the-Customer-Experience.md)

