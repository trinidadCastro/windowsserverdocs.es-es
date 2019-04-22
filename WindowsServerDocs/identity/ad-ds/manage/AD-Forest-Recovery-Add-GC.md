---
title: Recuperación de bosques de AD - agregar el catálogo global
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 156a4a64d9c3bb8261bd603b72ae11b81ff1d152
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825636"
---
# <a name="ad-forest-recovery---adding-the-gc"></a>Recuperación de bosques de AD - agregar el catálogo global

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Use el procedimiento siguiente para agregar el catálogo global a un controlador de dominio.  
  
## <a name="to-add-the-global-catalog"></a>Para agregar el catálogo global  
  
1. Haga clic en **iniciar**, apunte a **todos los programas**, apunte a **herramientas administrativas**y, a continuación, haga clic en **servicios y sitios de Active Directory**.  
2. En el árbol de consola, expanda el **sitios** contenedor y, a continuación, seleccione el sitio adecuado que contiene el servidor de destino.  
3. Expanda el **servidores** contenedor y, a continuación, expanda el objeto de servidor para el controlador de dominio al que desea agregar el catálogo global.  
4. Haga clic en **configuración NTDS**y, a continuación, haga clic en **propiedades**.  
5. Seleccione el **catálogo Global** casilla de verificación.  
![Agregar GC](media/AD-Forest-Recovery-Add-GC/addgc1.png)

## <a name="to-add-the-global-catalog-using-repadmin"></a>Para agregar el catálogo global con Repadmin  

- Abra un símbolo del sistema con privilegios elevados, escriba el siguiente comando y presione ENTRAR:  

   ```  
   repadmin.exe /options DC_NAME +IS_GC  
   ```  

Los siguientes son formas de acelerar el proceso de agregar el catálogo global para el controlador de dominio en el dominio raíz:  

- Idealmente, el controlador de dominio en el dominio raíz debe ser un asociado de replicación de los controladores de dominio restaurados en los dominios que no es raíz. Si es así, confirme que el Comprobador de coherencia de la información (KCC) ha creado la correspondiente **repsFrom** objeto para el DC de origen y la partición en la raíz del controlador de dominio. Puede confirmar esto si ejecutamos el **repadmin /showreps /v** comando. 

- Si no hay ningún **repsFrom** creado, cree este objeto para la partición de configuración. De este modo, el controlador de dominio en el dominio raíz puede determinar qué controladores de dominio en el dominio que no es raíz que se haya eliminado. Puede hacerlo con los siguientes comandos:  

   ```
   repadmin /add ConfigurationNamingContext DestinationDomainController SourceDomainControllerCNAME  
   ```

   ```
   repadmin /options DSA -Disable_NTDSCONN_XLATE  
   ```

   El formato para el *SourceDomainControllerCNAME* es:  

   ```
  
   sourceDCGuid._msdcs.root domain  
   ```

   Por ejemplo, el repadmin / agregar comandos para la partición de configuración del dominio contoso.com podría ser:  

   ```
   repadmin /add cn=configuration,DC=contoso,DC=com DC01 937ef930-7356-43c8-88dc-8baaaa781cf6._msdcs.dDSP17A22.contoso.com  
   ```

- Si el **repsFrom** objeto está presente, intente sincronizar el controlador de dominio en el dominio raíz con el controlador de dominio en el dominio que no es raíz como sigue:  

   ```
   Repadmin /sync DomainNamingContext DestinationDomainController SourceDomainControllerGUID  
   ```

   Donde *DestinationDomainController* es el controlador de dominio en el dominio raíz y *SourceDomainController* es el controlador de dominio restaurado en el dominio que no es raíz. 

- El servidor DNS del dominio raíz debe tener los registros de recursos de alias (CNAME) para el controlador de dominio de origen. Asegúrese de que la zona DNS primaria contiene registros de recursos de la delegación (servidor de nombres (NS) y registros de recursos de host (A)) para los controladores de dominio correctos (los controladores de dominio que se han restaurado desde la copia de seguridad) en la zona secundaria. 
- Asegúrese de que el controlador de dominio en el dominio raíz es ponerse en contacto con el centro de distribución de claves (KDC) correcto en el dominio que no es raíz. Para probar esto, en el símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR:  

   ```
   nltest /dsgetdc:nonroot domain name /KDC /Force  
   ```

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación de bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)  
