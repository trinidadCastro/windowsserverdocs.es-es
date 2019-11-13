---
title: Pasos para configurar el laboratorio de pruebas
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de una implementación multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7205b4-a822-4038-ab67-ec0a870737f2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dd8b8864dff98e51bf55aad9307523df4a0c30bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404701"
---
# <a name="steps-for-configuring-the-test-lab"></a>Pasos para configurar el laboratorio de pruebas

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En los pasos siguientes se describe cómo configurar la infraestructura de acceso remoto, configurar los clientes y servidores de acceso remoto y probar la conectividad de DirectAccess desde las subredes Internet y HomeNet.  
  
En esta guía del laboratorio de pruebas, creará una implementación de acceso remoto multisitio realizando los pasos siguientes:  
  
-   [Paso 1: completar la configuración de base](assetId:///9eb4a9ba-9118-4ea3-8963-e643ec81c3ed). Complete todos los pasos de la [Guía del laboratorio de pruebas: demostración de la configuración de servidor único de DirectAccess con IPv4 e IPv6 mixtos](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [Paso 2: instalar y configurar ENRUTADOR1](assetId:///e4b1a298-d5b0-410e-970b-c5358a9378f9). ENRUTADOR1 proporciona funcionalidad de enrutamiento y reenvío entre las subredes corporativas y 2-CorpNet.  
  
-   [Paso 3: instalar y configurar cliente2](assetId:///6cbee1b5-f6f6-443f-8fa9-31cc5c05a0ee). Cliente2 es un equipo cliente de Windows 7 que se usa para mostrar la compatibilidad con versiones anteriores de una implementación de acceso remoto de Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
-   [Paso 4: configurar app1](assetId:///a0ee655e-c01e-4bf3-a7b3-064e9614f810). Configure APP1 con ENRUTADOR1 como la puerta de enlace predeterminada y 2-DC1 como servidor DNS alternativo.  
  
-   [Paso 5: configurar DC1](assetId:///205ca795-93ce-4e53-aa6b-b44c87f0e14a). Configure DC1 con un sitio de Active Directory adicional y grupos de seguridad adicionales para equipos cliente de Windows 7.  
  
-   [Paso 6: instalar y configurar 2-DC1](assetId:///16752f61-edbf-4ff4-9d7a-e2077b66a127). En una implementación multisitio, tiene dos o más dominios y sitios. 2-DC1 proporciona controladores de dominio y servicios DNS para el dominio corp2.corp.contoso.com.  
  
-   [Paso 7: instalar y configurar 2-app1](assetId:///7d04b54e-590a-4d33-9766-415789859f29). 2-APP1 es un servidor Web y de archivos en la red 2-CorpNet.  
  
-   [Paso 8: configurar INET1](assetId:///8ecc0b63-8626-4939-8d26-3d51d051d231). INET1 simula Internet en esta guía del laboratorio de pruebas. Debe configurar una entrada DNS que se resuelva en la dirección IP pública de 2-EDGE1.  
  
-   [Paso 9: configurar EDGE1](assetId:///562744dc-30f6-42fa-bd5f-60a013b2179e). Configure el servidor DNS de 2-CorpNet y el enrutamiento en EDGE1.  
  
-   [Paso 10: instalación y configuración de 2-EDGE1](assetId:///1938c4f3-ca96-475d-9f2e-6bea3b7a4130). Se requieren dos servidores de acceso remoto en una implementación multisitio. 2-EDGE1 proporciona servicios de acceso remoto para el segundo dominio.  
  
-   [Paso 11: configurar la implementación multisitio](assetId:///537e4b68-043f-49c9-94d8-15ce8c4b18e2). Después de configurar ambos servidores de acceso remoto, puede configurar la implementación multisitio.  
  
-   [Paso 12: probar la conectividad de DirectAccess](assetId:///aa293b5d-4b6f-4004-95f3-0ab54804b15c). Pruebe la conectividad de DirectAccess desde ambos equipos cliente desde la subred de Internet a través de EDGE1 y 2-EDGE1.  
  
-   [Paso 13: probar la conectividad de DirectAccess desde detrás de un dispositivo NAT](assetId:///41f8195b-00a1-4991-9db8-3703514dbe0c). Pruebe la conectividad de DirectAccess desde detrás de un dispositivo NAT.  
  
-   [Paso 14: instantánea de la configuración](assetId:///7b56d5c9-c334-463e-9e29-d652ca110d84). Después de completar el laboratorio de pruebas, tome una instantánea de la implementación multisitio de acceso remoto en funcionamiento para poder volver a ella más adelante y probar escenarios adicionales.  
  


