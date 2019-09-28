---
title: Pasos para configurar el laboratorio de pruebas
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de DirectAccess con autenticación OTP y RSA SecurID para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a40183c-afd1-43ca-b306-05745640a37d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ef5ce37983b8565fab8287eeaae7423be0c269f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404725"
---
# <a name="steps-for-configuring-the-test-lab"></a>Pasos para configurar el laboratorio de pruebas

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En los pasos siguientes se describe cómo configurar la infraestructura de acceso remoto, configurar el cliente y el servidor de acceso remoto, y probar la conectividad de DirectAccess desde las subredes HomeNet y Internet.  
  
En esta guía del laboratorio de pruebas, creará un acceso remoto con el entorno OTP mediante los pasos siguientes:  
  
-   [PASO 1: Complete la configuración de DirectAccess @ no__t-0. Complete todos los pasos de la guía de laboratorio @no__t 0Test: Mostrar la configuración de servidor único de DirectAccess con IPv4 e IPv6 no__t-0.  
  
-   [PASO 2: Configure APP1 @ no__t-0. Configure APP1 con plantillas de certificado de OTP para su uso por parte de EDGE1.  
  
-   [PASO 3: Configure DC1 @ no__t-0. Compruebe el nombre principal de usuario definido en DC1.  
  
-   [PASO 7: Instale y configure RSA @ no__t-0. Instale y configure RSA, un servidor RADIUS y OTP y configure EDGE1 para OTP.  
  
-   [PASO 11: Compruebe el estado de OTP en EDGE1 @ no__t-0. Asegúrese de que el estado de OTP sea correcto en el servidor de acceso remoto.  
  
-   [PASO 8: Pruebe la conectividad de DirectAccess desde la subred HomeNet @ no__t-0. Pruebe la funcionalidad OTP de DirectAccess desde detrás de un dispositivo NAT.  
  
-   [PASO 10: Probar la conectividad de DirectAccess desde Internet @ no__t-0. Probar la Conectividad del cliente de DirectAccess desde Internet.  
  
-   [PASO 12: Una instantánea de la configuración @ no__t-0. Después de completar el laboratorio de pruebas, tome una instantánea de la configuración de DirectAccess en funcionamiento con OTP para que pueda volver a ella más adelante para probar escenarios adicionales.  
  


