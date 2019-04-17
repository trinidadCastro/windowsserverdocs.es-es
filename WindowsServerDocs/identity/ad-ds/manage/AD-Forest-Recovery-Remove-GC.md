---
title: "Recuperación de bosque de AD - quitar el catálogo global"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 60087a62-11e6-4750-a70e-510f35315688
ms.technology: identity-adfs
ms.openlocfilehash: b7da7c03cbae4691e8f902f1be0cb33c0c912248
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---removing-the-global-catalog"></a>Recuperación de bosque de AD - eliminar el catálogo global  

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Usa el siguiente procedimiento para quitar el catálogo global de un controlador de dominio.  
  
 Restauración de un servidor de catálogo global de copia de seguridad podría resultar en el catálogo global que mantiene los datos más recientes para uno de sus réplicas parciales al dominio correspondiente en el que está autorizado para esa réplica parcial. En tal caso, los datos más recientes no se quitará el catálogo global y es posible que incluso replicar a otros servidores del catálogo global. Como resultado, incluso si restaurar un controlador de dominio que era un servidor de catálogo global sin darse cuenta o porque era la copia de seguridad solitaria de confianza, debes quitar el catálogo global pronto después de completar la operación de restauración. Cuando se quita el catálogo global, el equipo quita todas sus réplicas parciales.  
  
## <a name="to-remove-the-global-catalog-using-active-directory-sites-and-services"></a>Para quitar el catálogo global con los servicios y sitios de Active Directory  
 
1.  Abre el administrador del servidor, haz clic en **herramientas** y haz clic en **servicios y sitios de Active Directory**.  
2.  En el árbol de consola, expande el **sitios** contenedor y, a continuación, selecciona el sitio adecuado que contiene el servidor de destino.  
3.  Expande la **servidores** contenedor y, a continuación, expande el *server* objeto para el controlador de dominio desde el que quieres quitar el catálogo global.  
4.  Haz clic en **configuración NTDS**y, a continuación, haz clic en **propiedades**.  
5.  Borrar la **catálogo Global** casilla de verificación.  
![Quitar GC](media/AD-Forest-Recovery-Remove-GC/removegc1.png)
6.  Haz clic en **aplicar**.
  
## <a name="to-remove-the-global-catalog-using-repadmin"></a>Para quitar el catálogo global con Repadmin  
  
1.  Abre un símbolo del sistema con privilegios elevados, escribe el siguiente comando y presiona ENTRAR:  
  
    ```  
    repadmin.exe /options DC_NAME –IS_GC  
    ```  
  
 ## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)
