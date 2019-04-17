---
title: "Recuperación de bosque de AD - agregar el catálogo global"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 3749fd99737f61c55871f613b9feaa21090d3bae
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---adding-the-gc"></a>Recuperación de bosque de AD - agregar el catálogo global 

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Usa el siguiente procedimiento para agregar el catálogo global a un controlador de dominio.  
  
## <a name="to-add-the-global-catalog"></a>Para agregar el catálogo global  
  
1.  Haz clic en **inicio**, elija **todos los programas**, elija **herramientas administrativas**y, a continuación, haz clic en **servicios y sitios de Active Directory**.  
2.  En el árbol de consola, expande el **sitios** contenedor y, a continuación, selecciona el sitio adecuado que contiene el servidor de destino.  
3.  Expande la **servidores** contenedor y, a continuación, expande el objeto de servidor para el controlador de dominio al que quieres agregar el catálogo global.  
4.  Haz clic en **configuración NTDS**y, a continuación, haz clic en **propiedades**.  
5.  Selecciona el **catálogo Global** casilla de verificación.  
![Agregar GC](media/AD-Forest-Recovery-Add-GC/addgc1.png)
  
## <a name="to-add-the-global-catalog-using-repadmin"></a>Para agregar el catálogo global con Repadmin  
  
1.  Abre un símbolo del sistema con privilegios elevados, escribe el siguiente comando y presiona ENTRAR:  
  
    ```  
    repadmin.exe /options DC_NAME +IS_GC  
    ```  
  
 Estos son formas para acelerar el proceso de agregar el catálogo global para el controlador de dominio en el dominio raíz:  
  
-   En teoría, el controlador de dominio en el dominio raíz debe ser un duplicador de los controladores de dominio restaurados en los dominios no raíz. Si es así, confirma que el Comprobador de coherencia de la información (KCC) ha creado el correspondiente **repsFrom** objeto para el origen de controlador de dominio y la partición en la raíz del controlador de dominio. Puedes confirmar ejecutando el **repadmin /showreps /v** comando.  
  
-   Si no hay ningún **repsFrom** objeto creado, crea este objeto para la partición de configuración. De esta forma, el controlador de dominio en el dominio raíz puede determinar qué controladores de dominio en el dominio no raíz se han eliminado. Puedes hacer esto con los siguientes comandos:  
  
    ```  
    repadmin /add ConfigurationNamingContext DestinationDomainController SourceDomainControllerCNAME  
    ```  
  
    ```  
    repadmin /options DSA -Disable_NTDSCONN_XLATE  
    ```  
  
     El formato para el *SourceDomainControllerCNAME* es:  
  
    ```  
  
    sourceDCGuid._msdcs.root domain  
    ```  
  
     Por ejemplo, podría ser el comando de /add repadmin para la partición de la configuración del dominio contoso.com:  
  
    ```  
    repadmin /add cn=configuration,DC=contoso,DC=com DC01 937ef930-7356-43c8-88dc-8baaaa781cf6._msdcs.dDSP17A22.contoso.com  
    ```  
  
-   Si la **repsFrom** objeto está presente, intenta sincronizar el controlador de dominio en el dominio raíz con el controlador de dominio en el dominio no raíz como sigue:  
  
    ```  
    Repadmin /sync DomainNamingContext DestinationDomainController SourceDomainControllerGUID  
    ```  
  
     Donde *DestinationDomainController* es el controlador de dominio en el dominio raíz y *SourceDomainController* es el controlador de dominio restaurado en el dominio no raíz.  
  
-   El servidor DNS del dominio raíz debe tener los registros de recursos de alias (CNAME) para el origen de controlador de dominio. Asegúrate de que la zona DNS principal contiene registros de recursos de la delegación (servidor de nombres (NS) y registros de recursos de host (A)) para los controladores de dominio correctos (los controladores de dominio que se han restaurado desde la copia de seguridad) en la zona secundaria.  
  
-   Asegúrate de que el controlador de dominio en el dominio raíz se pone en contacto con el centro de distribución de claves (KDC) correcto en el dominio no raíz. Para comprobarlo, en el símbolo del sistema, escribe el siguiente comando y, a continuación, presiona ENTRAR:  
  
    ```  
    nltest /dsgetdc:nonroot domain name /KDC /Force  
    ```  
## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)  
