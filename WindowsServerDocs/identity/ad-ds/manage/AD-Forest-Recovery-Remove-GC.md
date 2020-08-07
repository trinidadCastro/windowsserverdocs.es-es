---
title: 'Recuperación del bosque de AD: quitar el catálogo global'
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 60087a62-11e6-4750-a70e-510f35315688
ms.openlocfilehash: b05415e73faef73831cccbbd9785dd1cf2d1cf9e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969852"
---
# <a name="ad-forest-recovery---removing-the-global-catalog"></a>Recuperación del bosque de AD: quitar el catálogo global

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Utilice el siguiente procedimiento para quitar el catálogo global de un controlador de dominio.

 La restauración de un servidor de catálogo global desde una copia de seguridad puede dar lugar a que el catálogo global contenga datos más recientes para una de sus réplicas parciales que el dominio correspondiente que es autoritativo para esa réplica parcial. En tal caso, los datos más recientes no se quitarán del catálogo global y podrían incluso replicarse en otros servidores de catálogo global. Como resultado, incluso si restauró un controlador de dominio que era un servidor de catálogo global, ya sea accidentalmente o porque se trata de la copia de seguridad de solitarios de confianza, debe quitar el catálogo global poco después de que se complete la operación de restauración. Cuando se quita el catálogo global, el equipo quita todas sus réplicas parciales.

## <a name="to-remove-the-global-catalog-using-active-directory-sites-and-services"></a>Para quitar el catálogo global mediante Active Directory sitios y servicios

1. Abra Administrador del servidor, haga clic en **herramientas** y haga clic en **sitios y servicios de Active Directory**.
2. En el árbol de consola, expanda el contenedor **sitios** y, a continuación, seleccione el sitio adecuado que contiene el servidor de destino.
3. Expanda el contenedor **servidores** y, a continuación, expanda el objeto de *servidor* del controlador de dominio del que desea quitar el catálogo global.
4. Haga clic con el botón secundario en **configuración NTDS**y, a continuación, haga clic en **propiedades**.
5. Desactive la casilla **catálogo global** .
   ![Quitar GC](media/AD-Forest-Recovery-Remove-GC/removegc1.png)
6. Haga clic en **Aplicar**.

## <a name="to-remove-the-global-catalog-using-repadmin"></a>Para quitar el catálogo global mediante repadmin

Abra un símbolo del sistema con privilegios elevados, escriba el siguiente comando y presione ENTRAR:

   ```
   repadmin.exe /options DC_NAME –IS_GC
   ```

## <a name="next-steps"></a>Pasos a seguir

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
