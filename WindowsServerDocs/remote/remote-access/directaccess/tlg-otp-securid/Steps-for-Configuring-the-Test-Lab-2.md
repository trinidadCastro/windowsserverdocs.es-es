---
title: Pasos para configurar el laboratorio de pruebas
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar DirectAccess con autenticación OTP y RSA SecurID para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a40183c-afd1-43ca-b306-05745640a37d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0607506f2b6dd49284e6b377fb87da4f731eb94d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283079"
---
# <a name="steps-for-configuring-the-test-lab"></a>Pasos para configurar el laboratorio de pruebas

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Los pasos siguientes describen cómo configurar la infraestructura de acceso remoto, el servidor de acceso remoto y el cliente y probar la conectividad de DirectAccess desde las subredes Homenet e Internet.  
  
En esta guía de laboratorio de pruebas se compilará un acceso remoto con el entorno de OTP realizando los pasos siguientes:  
  
-   [PASO 1: Completar la configuración de DirectAccess](assetId:///4dbf877f-02fb-439b-907a-f5b3f1d8afa6). Complete los pasos descritos en el [Test Lab Guide: Demostrar la instalación de servidor único de DirectAccess con IPv4 e IPv6 mixto](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [PASO 2: Configurar APP1](assetId:///c1bb590f-91d4-4ed5-bceb-b0e36eabd4ff). Configurar APP1 con plantillas de certificado OTP para su uso por EDGE1.  
  
-   [PASO 3: Configurar DC1](assetId:///904a6edc-a771-45ed-9630-a34a680bb522). Compruebe el que nombre Principal de usuario definidos en DC1.  
  
-   [PASO 7: Instalar y configurar RSA](assetId:///baa4c28c-add7-42e2-8afd-ccc7a559406a). Instalar y configurar el servidor RSA, RADIUS y OTP y configurar EDGE1 para OTP.  
  
-   [PASO 11: Comprobar el estado OTP en EDGE1](assetId:///3b397a4a-8478-47f2-a932-9e8e048c14ba). Asegúrese de que el estado de OTP es correcto en el servidor de acceso remoto.  
  
-   [PASO 8: Probar la conectividad de DirectAccess desde la subred Homenet](assetId:///ba1652a6-0692-4add-91ca-34a84956ba14). Probar la funcionalidad de OTP de DirectAccess desde detrás de un dispositivo NAT.  
  
-   [PASO 10: Probar la conectividad de DirectAccess desde Internet](assetId:///321149eb-5f23-4a0b-b8fb-1244540126e9). Probar la conectividad de cliente de DirectAccess desde Internet.  
  
-   [PASO 12: Instantánea de la configuración](assetId:///8a51ed3c-9c32-402f-85d1-617ce46845b4). Después de completar el laboratorio de pruebas, tome una instantánea del espacio de trabajo DirectAccess con configuración de OTP para que puede volver a él más adelante para probar los escenarios adicionales.  
  


