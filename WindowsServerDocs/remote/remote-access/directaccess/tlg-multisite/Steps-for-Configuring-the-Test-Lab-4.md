---
title: Pasos para configurar el laboratorio de pruebas
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar una implementación de multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7205b4-a822-4038-ab67-ec0a870737f2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a3d01dd8002e28fb127ac6b1b4cea25c58953521
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281396"
---
# <a name="steps-for-configuring-the-test-lab"></a>Pasos para configurar el laboratorio de pruebas

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Los pasos siguientes describen cómo configurar la infraestructura de acceso remoto, los clientes y servidores de acceso remoto y probar la conectividad de DirectAccess desde las subredes de Internet y Homenet.  
  
En esta guía de laboratorio de pruebas se compilará una implementación multisitio de acceso remoto mediante los pasos siguientes:  
  
-   [PASO 1: Completar la configuración Base](assetId:///9eb4a9ba-9118-4ea3-8963-e643ec81c3ed). Complete los pasos descritos en el [Test Lab Guide: Demostrar la instalación de servidor único de DirectAccess con IPv4 e IPv6 mixto](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [PASO 2: Instalar y configurar ENRUTADOR1](assetId:///e4b1a298-d5b0-410e-970b-c5358a9378f9). ENRUTADOR1 proporciona funcionalidad entre las subredes de la red corporativa y 2 Corpnet de reenvío y enrutamiento.  
  
-   [PASO 3: Instalar y configurar CLIENT2](assetId:///6cbee1b5-f6f6-443f-8fa9-31cc5c05a0ee). Client2 es un equipo cliente de Windows 7 que se usa para demostrar la hacia atrás compatibilidad de una implementación de Windows Server 2016, Windows Server 2012 R2 o acceso remoto de Windows Server 2012.  
  
-   [PASO 4: Configurar APP1](assetId:///a0ee655e-c01e-4bf3-a7b3-064e9614f810). Configurar APP1 con ENRUTADOR1 como la puerta de enlace predeterminada y 2-DC1 como el servidor DNS alternativo.  
  
-   [PASO 5: Configurar DC1](assetId:///205ca795-93ce-4e53-aa6b-b44c87f0e14a). Configurar DC1 con grupos de seguridad adicional para los equipos cliente de Windows 7 y un sitio de Active Directory adicional.  
  
-   [PASO 6: Instalar y configurar 2-DC1](assetId:///16752f61-edbf-4ff4-9d7a-e2077b66a127). En una implementación multisitio, tiene dos o más dominios y sitios. 2-DC1 proporciona servicios DNS para el dominio corp2.corp.contoso.com y controlador de dominio.  
  
-   [PASO 7: Instalar y configurar 2-APP1](assetId:///7d04b54e-590a-4d33-9766-415789859f29). 2-APP1 es un servidor web y el archivo en la red Corpnet de 2.  
  
-   [PASO 8: Configurar INET1](assetId:///8ecc0b63-8626-4939-8d26-3d51d051d231). INET1 se simula Internet en esta guía de laboratorio de pruebas. Debe configurar una entrada DNS que se resuelve en la dirección IP pública de 2-EDGE1.  
  
-   [PASO 9: Configurar EDGE1](assetId:///562744dc-30f6-42fa-bd5f-60a013b2179e). Configure el servidor DNS 2 Corpnet y enrutamiento en EDGE1.  
  
-   [PASO 10: Instalar y configurar 2 EDGE1](assetId:///1938c4f3-ca96-475d-9f2e-6bea3b7a4130). Se requieren dos servidores de acceso remoto en una implementación multisitio. 2-EDGE1 proporciona servicios de acceso remoto para el dominio de segundo.  
  
-   [PASO 11: Configurar una implementación multisitio](assetId:///537e4b68-043f-49c9-94d8-15ce8c4b18e2). Después de configurar ambos servidores de acceso remoto, puede configurar la implementación multisitio.  
  
-   [PASO 12: Probar la conectividad de DirectAccess](assetId:///aa293b5d-4b6f-4004-95f3-0ab54804b15c). Probar la conectividad de DirectAccess desde ambos equipos cliente desde la subred de Internet a través de EDGE1 y 2 EDGE1.  
  
-   [PASO 13: Probar la conectividad de DirectAccess desde detrás de un dispositivo NAT](assetId:///41f8195b-00a1-4991-9db8-3703514dbe0c). Probar la conectividad de DirectAccess desde detrás de un dispositivo NAT.  
  
-   [PASO 14: Instantánea de la configuración](assetId:///7b56d5c9-c334-463e-9e29-d652ca110d84). Después de completar el laboratorio de pruebas, tome una instantánea de la implementación multisitio de acceso remoto en funcionamiento para que puede volver a él más adelante para probar los escenarios adicionales.  
  


