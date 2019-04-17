---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: "Planeación de la ubicación del controlador de dominio de raíz de bosque"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 374ce31c61c666302e2b00a8f365c289cb8799f8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="planning-forest-root-domain-controller-placement"></a>Planeación de la ubicación del controlador de dominio de raíz de bosque

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controladores de dominio raíz de bosque son necesarios para crear las rutas de acceso de confianza para los clientes que necesiten acceder a recursos en dominios distintos sus propias. Coloca los controladores de dominio raíz de bosque en ubicaciones de concentrador y en las ubicaciones que hospedan los centros de datos. Si los usuarios de una ubicación determinada se necesitan acceder a recursos de otros dominios en la misma ubicación y la disponibilidad de red entre el centro de datos y la ubicación del usuario no es confiable, puedes agregar un controlador de dominio raíz de bosque en la ubicación o crear un acceso directo de confianza entre los dos dominios. Es más rentable para crear un acceso directo de confianza entre dominios a menos que haya otras razones para colocar un controlador de dominio raíz de bosque en esa ubicación.  
  
Ayudar a optimizar las solicitudes de autenticación de usuarios situados en cualquiera de los dominios de confianzas de acceso directo. Para obtener más información acerca de método abreviado confianzas entre dominios, consulta cuándo crear un acceso directo de confianza ([https://go.microsoft.com/fwlink/?LinkId=107061](https://go.microsoft.com/fwlink/?LinkId=107061)).  
  
Para que una hoja de cálculo que le ayudarán a documentar la ubicación del controlador de dominio raíz bosque, consulta el trabajo Aid para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), descargar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, y Abre (DSSTOPO_4.doc) "Ubicación del controlador de dominio".  
  
Tendrás que hacer referencia a esta información cuando se crea el dominio raíz. Para obtener más información acerca de la implementación de dominio raíz del bosque, consulta [implementar Windows Server 2008 dominio raíz del bosque](https://technet.microsoft.com/library/cc731174.aspx).  
  


