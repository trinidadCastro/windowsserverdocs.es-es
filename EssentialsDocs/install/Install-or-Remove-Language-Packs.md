---
title: Instalar o quitar paquetes de idioma
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98f13f63-4480-40ba-a7ef-d1d9b7582e5f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: a41b491bbe4b4a8ee7f9743dc85e5bdaffb08496
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="install-or-remove-language-packs"></a>Instalar o quitar paquetes de idioma

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

> [!NOTE]
>  Primero debes crear una imagen de Windows multilingüe como se describe en la [paquetes de idioma e implementación](https://technet.microsoft.com/library/hh824829) antes de agregar el paquete de idioma de Windows Server Essentials.  
  
 Paquetes de idioma solo están disponibles para la creación de imágenes multilingües. La información de esta sección es específica para instalar o quitar paquetes de idioma en Windows Server Essentials.  
  
> [!NOTE]
>  Si vas a ejecutar la configuración inicial (CI) desde un equipo cliente que no es compatible con los idiomas de Asia oriental, como ja-jp, e inglés no está incluido en la imagen multilingüe, en el servidor, la página Web de IC mostrará cuadrados. Para la página Web de IC con un valor predeterminado para el inglés, la imagen multilingüe que crees debe incluir el inglés.  
  
## <a name="adding-language-packs-to-an-image"></a>Agregar paquetes de idioma a una imagen  
 Paquetes de idioma están disponibles en el DVD de personalización de OEM. Se recomienda que Copies los paquetes de idioma para el equipo del técnico antes de agregar los paquetes de idioma a la imagen.  
  
 Debes usar el siguiente comando para instalar paquetes de idioma:  
  
 **Dism.exe//online /Add-Package /PackagePath: C:\\ < ruta de acceso completa cab directory\ archivo > \lp.cab**  
  
 Por ejemplo, el siguiente comando muestra cómo agregar un paquete de idioma alemán:  
  
 **Dism.exe//online /Add-Package /PackagePath: C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
> [!IMPORTANT]
>  También debes aplicar paquetes de idioma para Windows Server Essentials localizar completamente el sistema operativo.  
  
## <a name="removing-language-packs-from-an-image"></a>Eliminación de paquetes de idioma de una imagen  
 Puedes usar el siguiente comando para quitar un paquete de idioma ya no quieres incluir en una imagen:  
  
 **Dism.exe//online /Remove-Package /PackagePath: C:\\ < ruta de acceso completa cab directory\ archivo > \lp.cab**  
  
 Por ejemplo, el siguiente comando muestra cómo quitar un paquete de idioma alemán:  
  
 **Dism.exe//online /Remove-Package /PackagePath: C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
## <a name="see-also"></a>Consulta también  

 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)

 [Crear y personalizar la imagen](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](../install/Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](../install/Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](../install/Testing-the-Customer-Experience.md)

