---
ms.assetid: ffb9d63c-ba7c-4ad1-b814-6db67f98c943
title: "La función de la canalización de notificaciones"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e433b89ab7f225b589d94d98ce57c76007ef41aa
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: es-ES
---
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-the-claims-pipeline"></a>La función de la canalización de notificaciones
La canalización de notificaciones en los servicios de federación de Active Directory \(AD FS\) representa la ruta de acceso que deben seguir notificaciones a través de servicios de federación antes de que pueden ser emitidos. El servicio de federación administra todo el proceso de end\-to\-end de transmitir notificaciones a través de las distintas fases de la canalización de notificaciones, que también incluye el procesamiento de reglas de notificación mediante el motor de reglas de notificación.  
  
Para obtener más información acerca de las reglas de notificación, consulta [el rol reclamar las reglas de](The-Role-of-Claim-Rules.md). Para obtener más información acerca de cómo el motor de reglas de Reclamación procesa las reglas, consulta [el rol del motor reclamaciones](The-Role-of-the-Claims-Engine.md).  
  
La siguiente sección describe el proceso que supervisa el servicio de federación con más detalle.  
  
## <a name="claims-pipeline-process"></a>Proceso de la canalización de notificaciones  
El proceso de la canalización de reclamaciones consta de tres fases high\ nivel. Cada fase de este proceso inicializa el motor de reglas de notificación para procesar las reglas de notificación que son específicas de esa fase. Estas fases incluyen \ (en el orden en que se puedan occur\):  
  
1.  Aceptar notificaciones entrantes, esta etapa de la canalización de notificaciones se usa para extraer el token de las notificaciones entrantes y eliminar las notificaciones que no se espera o de confianza. Una vez extraídos, las reglas de aceptación que conforman la regla de transformación de aceptación establecer para un reclamaciones que se ejecutan la confianza del proveedor. Estas reglas pueden usarse para pasar o agregar nuevas notificaciones que se pueden usar en las fases posteriores de la canalización de reclamaciones. El resultado de esta fase se usa como entrada para la segunda y tercera fase.  
  
2.  Autorizar el solicitante reclamaciones, esta fase se usa el motor de notificaciones para emitir permitir o denegar reclamaciones en función de si se permite el token solicitante para obtener un token para el usuario de confianza determinado o no. Sin embargo, antes de que esto puede ocurrir establecen las reglas de autorización que conforman una regla de autorización de emisión o la regla de autorización de la delegación para una confianza de terceros de confianza se ejecutó.  
  
3.  Emitir notificaciones salientes, esta fase se usa para emitir notificaciones salientes y enviarlos a lo largo de la canalización de donde se empaquetan en un token de seguridad. Sin embargo, antes de que esto puede ocurrir se ejecutó las reglas de emisión que conforman la regla de transformación de emisión establecida para una confianza de terceros de confianza, que determinan lo que dice que se emitirá como notificaciones salientes.  
  
Todas las tres fases anteriores realizan reclamaciones de procesamiento de reglas, pero usan un conjunto diferente de reglas. Como se describió anteriormente, cada fase tiene asociado un conjunto de reglas basadas en el emisor de las notificaciones entrantes \(the acceptance rules\) o el servicio de destino para el que se está emitiendo la claimincludes \ (rules\ emisión y autorización).  
  
Reclamaciones son token\ independientes, pero se transmiten a través de la red encapsulada en tokens de seguridad. Las reglas de notificación actuar sobre reclamaciones independientemente del formato del token de seguridad entrante o saliente.  
  
Reclamaciones reglas contienen la lógica administrator\ definidos por el que el motor de reclamaciones aceptará las notificaciones entrantes, autorizar reclamaciones en función de la identidad del solicitante y emitir notificaciones que se necesitan por un usuario de confianza. En última instancia, es el motor de notificaciones que determina qué notificaciones entrará en el token de seguridad que se emitirá después de la notificación se ha fluya a través de la canalización de reclamaciones.  
  
Como se muestra en la siguiente ilustración, la canalización de notificaciones es responsable de todo el proceso de end\-to\-end de flujo de una reclamación a través de las distintas fases de canalización para terminar con una notificación emitida que se envían a través de una confianza de terceros de confianza. La notificación en la ilustración saliente representa la reclamación emitida.  
  
![Roles de AD FS](media/adfs2_pipeline.gif)  
  
Aunque no se muestra en la ilustración, es el motor de notificaciones que realiza el procesamiento real de las reglas en cada fase. Para obtener más información, consulta [el rol del motor reclamaciones](The-Role-of-the-Claims-Engine.md).  
  

