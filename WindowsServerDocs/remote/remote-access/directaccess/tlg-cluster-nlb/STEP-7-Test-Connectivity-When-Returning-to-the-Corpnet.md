---
title: PASO 7 probar la conectividad al volver a la red corporativa
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar DirectAccess en un clúster con NLB de Windows para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a7252d0-6db8-4a9d-98ee-75082ecd2929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c21b890523ca61b8f0821692178d050bc11efd79
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890816"
---
# <a name="step-7-test-connectivity-when-returning-to-the-corpnet"></a>PASO 7 probar la conectividad al volver a la red corporativa

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Muchos de los usuarios se moverán entre ubicaciones remotas y la red corporativa, por lo que es importante que al regresar a la red corporativa que son capaces de obtener acceso a recursos sin tener que realizar ninguna configuración cambia. Acceso remoto lo hace posible porque cuando el cliente de DirectAccess vuelve a la red corporativa, puede establecer una conexión al servidor de ubicación de red. Una vez que la conexión HTTPS se establece correctamente al servidor de ubicación de red, el cliente de DirectAccess deshabilita la configuración de cliente de DirectAccess y usa una conexión directa a la red corporativa.  
  
### <a name="test-connectivity-on-client1"></a>Probar la conectividad en CLIENT1  
  
1.  Apague CLIENT1 y, a continuación, desconecta CLIENT1 del conmutador virtual o subred Homenet y conectarlo a la subred de la red corporativa o un conmutador virtual. Activar CLIENT1 e iniciar sesión como corp\usuario1.  
  
2.  Abra una ventana de Windows PowerShell con privilegios elevados, escriba **ipconfig/all**, y presione ENTRAR. La salida indicará que CLIENT1 tiene una dirección IP local y que no hay ningún activo 6to4, Teredo o IP-HTTPS túnel.  
  
3.  Probar la conectividad con el recurso compartido de red en APP2. En el **iniciar** , escriba**\\\APP2\Files**, y, a continuación, presione ENTRAR. Podrá abrir el archivo en esa carpeta.  
  


