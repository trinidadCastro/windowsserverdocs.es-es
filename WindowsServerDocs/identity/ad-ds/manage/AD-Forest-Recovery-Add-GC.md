---
title: 'Recuperación del bosque de AD: agregar el GC'
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.openlocfilehash: 00a95269891074f95184c52f5244176f18de7b37
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939945"
---
# <a name="ad-forest-recovery---adding-the-gc"></a>Recuperación del bosque de AD: agregar el GC

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Utilice el procedimiento siguiente para agregar el catálogo global a un controlador de dominio.

## <a name="to-add-the-global-catalog"></a>Para agregar el catálogo global

1. Haga clic en **Inicio**, seleccione **todos los programas**, seleccione **herramientas administrativas**y, a continuación, haga clic en **Active Directory sitios y servicios**.
2. En el árbol de consola, expanda el contenedor **sitios** y, a continuación, seleccione el sitio adecuado que contiene el servidor de destino.
3. Expanda el contenedor **servidores** y, a continuación, expanda el objeto de servidor del controlador de dominio al que desea agregar el catálogo global.
4. Haga clic con el botón secundario en **configuración NTDS**y, a continuación, haga clic en **propiedades**.
5. Active la casilla **catálogo global** .
![Agregar GC](media/AD-Forest-Recovery-Add-GC/addgc1.png)

## <a name="to-add-the-global-catalog-using-repadmin"></a>Para agregar el catálogo global mediante repadmin

- Abra un símbolo del sistema con privilegios elevados, escriba el siguiente comando y presione ENTRAR:

   ```
   repadmin.exe /options DC_NAME +IS_GC
   ```

Las siguientes son formas de acelerar el proceso de agregar el catálogo global al DC en el dominio raíz:

- Idealmente, el DC del dominio raíz debe ser un asociado de replicación de los controladores de dominio restaurados en los dominios que no son de raíz. Si es así, confirme que el comprobador de coherencia de la información (KCC) ha creado el objeto **repsFrom** correspondiente para el DC de origen y la partición en el controlador de dominio raíz. Puede confirmarlo mediante la ejecución del comando **repadmin/showreps/v** .

- Si no se ha creado ningún objeto **repsFrom** , cree este objeto para la partición de configuración. De este modo, el controlador de dominio del dominio raíz puede determinar qué controladores de dominio del dominio no raíz se han eliminado. Puede hacerlo con los siguientes comandos:

   ```
   repadmin /add ConfigurationNamingContext DestinationDomainController SourceDomainControllerCNAME
   ```

   ```
   repadmin /options DSA -Disable_NTDSCONN_XLATE
   ```

   El formato de *SourceDomainControllerCNAME* es:

   ```

   sourceDCGuid._msdcs.root domain
   ```

   Por ejemplo, el comando repadmin/Add para la partición de configuración del dominio contoso.com podría ser:

   ```
   repadmin /add cn=configuration,DC=contoso,DC=com DC01 937ef930-7356-43c8-88dc-8baaaa781cf6._msdcs.dDSP17A22.contoso.com
   ```

- Si el objeto **repsFrom** está presente, intente sincronizar el controlador de dominio en el dominio raíz con el DC en el dominio no raíz como se indica a continuación:

   ```
   Repadmin /sync DomainNamingContext DestinationDomainController SourceDomainControllerGUID
   ```

   Donde *DestinationDomainController* es el DC del dominio raíz y *SOURCEDOMAINCONTROLLER* es el DC restaurado en el dominio no raíz.

- El servidor DNS del dominio raíz debe tener los registros de recursos de alias (CNAME) del DC de origen. Asegúrese de que la zona DNS primaria contiene registros de recursos de delegación (servidor de nombres (NS) y registros de recursos de host (A)) para los controladores de seguridad correctos (los controladores de archivos que se han restaurado desde la copia de seguridad) en la zona secundaria.
- Asegúrese de que el controlador de dominio del dominio raíz se pone en contacto con el Centro de distribución de claves correcto (KDC) del dominio que no es raíz. Para probarlo, en el símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR:

   ```
   nltest /dsgetdc:nonroot domain name /KDC /Force
   ```

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
