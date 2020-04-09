---
title: Pasos para configurar el laboratorio de pruebas
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de DirectAccess con autenticación OTP y RSA SecurID para Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 0a40183c-afd1-43ca-b306-05745640a37d
ms.author: lizross
author: eross-msft
ms.openlocfilehash: fc40bf8f1b4bcb3d28b23abd981575a6438f871e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814388"
---
# <a name="steps-for-configuring-the-test-lab"></a>Pasos para configurar el laboratorio de pruebas

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En los pasos siguientes se describe cómo configurar la infraestructura de acceso remoto, configurar el cliente y el servidor de acceso remoto, y probar la conectividad de DirectAccess desde las subredes HomeNet y Internet.  
  
En esta guía del laboratorio de pruebas, creará un acceso remoto con el entorno OTP mediante los pasos siguientes:  
  
-   [Paso 1: completar la configuración de DirectAccess](assetId:///4dbf877f-02fb-439b-907a-f5b3f1d8afa6). Complete todos los pasos de la [Guía del laboratorio de pruebas: demostración de la configuración de servidor único de DirectAccess con IPv4 e IPv6 mixtos](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [Paso 2: configurar app1](assetId:///c1bb590f-91d4-4ed5-bceb-b0e36eabd4ff). Configure APP1 con plantillas de certificado de OTP para su uso por parte de EDGE1.  
  
-   [Paso 3: configurar DC1](assetId:///904a6edc-a771-45ed-9630-a34a680bb522). Compruebe el nombre principal de usuario definido en DC1.  
  
-   [Paso 7: instalar y configurar RSA](assetId:///baa4c28c-add7-42e2-8afd-ccc7a559406a). Instale y configure RSA, un servidor RADIUS y OTP y configure EDGE1 para OTP.  
  
-   [Paso 11: comprobar el estado de OTP en EDGE1](assetId:///3b397a4a-8478-47f2-a932-9e8e048c14ba). Asegúrese de que el estado de OTP sea correcto en el servidor de acceso remoto.  
  
-   [Paso 8: probar la conectividad de DirectAccess desde la subred HomeNet](assetId:///ba1652a6-0692-4add-91ca-34a84956ba14). Pruebe la funcionalidad OTP de DirectAccess desde detrás de un dispositivo NAT.  
  
-   [Paso 10: probar la conectividad de DirectAccess desde Internet](assetId:///321149eb-5f23-4a0b-b8fb-1244540126e9). Probar la Conectividad del cliente de DirectAccess desde Internet.  
  
-   [Paso 12: instantánea de la configuración](assetId:///8a51ed3c-9c32-402f-85d1-617ce46845b4). Después de completar el laboratorio de pruebas, tome una instantánea de la configuración de DirectAccess en funcionamiento con OTP para que pueda volver a ella más adelante para probar escenarios adicionales.  
  


