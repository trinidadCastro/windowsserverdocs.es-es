---
title: Paso 7 probar la conectividad al volver a la red corporativa
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de DirectAccess en un clúster con Windows NLB para Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 5a7252d0-6db8-4a9d-98ee-75082ecd2929
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 479ac48bbb04186f97d25e6744e8630b4dc6a51e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819128"
---
# <a name="step-7-test-connectivity-when-returning-to-the-corpnet"></a>Paso 7 probar la conectividad al volver a la red corporativa

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Muchos de los usuarios se moverán entre ubicaciones remotas y la red corporativa, por lo que es importante que cuando vuelvan a la red corporativa puedan acceder a los recursos sin tener que realizar ningún cambio en la configuración. El acceso remoto hace posible esto porque cuando el cliente de DirectAccess vuelve a la red corporativa, puede establecer una conexión con el servidor de ubicación de red. Una vez que la conexión HTTPS se establece correctamente en el servidor de ubicación de red, el cliente de DirectAccess deshabilita la configuración del cliente de DirectAccess y utiliza una conexión directa a la red corporativa.  
  
### <a name="test-connectivity-on-client1"></a>Probar la conectividad en CLIENT1  
  
1. Apague CLIENT1 y desconecte CLIENT1 de la subred HomeNet o el conmutador virtual y conéctelo a la subred corporativa o al conmutador virtual. Active CLIENT1 e inicie sesión como CORP\User1.  
  
2. Abra una ventana de Windows PowerShell con privilegios elevados, escriba **ipconfig/all**y presione Entrar. La salida indicará que CLIENT1 tiene una dirección IP local y que no hay ningún túnel de IP-HTTPS, 6to4 o Teredo activo.  
  
3. Pruebe la conectividad con el recurso compartido de red en APP2. En la pantalla **Inicio** , escriba<strong>\\\APP2\Files</strong>y, a continuación, presione Entrar. Podrá abrir el archivo en esa carpeta.  
  


