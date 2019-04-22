---
title: Recuperación de bosques de AD - quitar el catálogo global
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 60087a62-11e6-4750-a70e-510f35315688
ms.technology: identity-adds
ms.openlocfilehash: d730ce65fc179aee6a98f7cfc1a5b693bfcd6c93
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817076"
---
# <a name="ad-forest-recovery---removing-the-global-catalog"></a>Recuperación de bosques de AD - quitar el catálogo global  

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Utilice el procedimiento siguiente para quitar el catálogo global de un controlador de dominio. 
  
 Restaurar un servidor de catálogo global de copia de seguridad podría dar lugar a que el catálogo global que contiene los datos más recientes para uno de sus réplicas parciales que el dominio correspondiente que está autorizado para esa réplica parcial. En tal caso, los datos más recientes no se quitará el catálogo global y puede que incluso se repliquen a otros servidores de catálogo global. Como resultado, incluso si restaura un controlador de dominio que era un servidor de catálogo global, ya sea por accidente o porque ese era la copia de seguridad Solitario de confianza, debe quitar el catálogo global, poco después de completada la operación de restauración. Cuando se quita el catálogo global, el equipo quita todas sus réplicas parciales. 
  
## <a name="to-remove-the-global-catalog-using-active-directory-sites-and-services"></a>Para quitar el catálogo global con servicios y sitios de Active Directory  
 
1. Abra el administrador del servidor, haga clic en **herramientas** y haga clic en **servicios y sitios de Active Directory**. 
2. En el árbol de consola, expanda el **sitios** contenedor y, a continuación, seleccione el sitio adecuado que contiene el servidor de destino. 
3. Expanda el **servidores** contenedor y, a continuación, expanda el *server* objeto para el controlador de dominio del que desea quitar el catálogo global. 
4. Haga clic en **configuración NTDS**y, a continuación, haga clic en **propiedades**. 
5. Desactive el **catálogo Global** casilla de verificación. 
   ![Remove GC](media/AD-Forest-Recovery-Remove-GC/removegc1.png)
6. Haga clic en **Aplicar**.
  
## <a name="to-remove-the-global-catalog-using-repadmin"></a>Para quitar el catálogo global con Repadmin  
  
Abra un símbolo del sistema con privilegios elevados, escriba el siguiente comando y presione ENTRAR:  

   ```
   repadmin.exe /options DC_NAME –IS_GC  
   ```  

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación de bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
