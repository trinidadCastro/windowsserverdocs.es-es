---
ms.assetid: ffb9d63c-ba7c-4ad1-b814-6db67f98c943
title: El papel de la canalización de notificaciones
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cd5ca15a9e098f60c6c82980e3148ebcf9ee9932
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869337"
---
# <a name="the-role-of-the-claims-pipeline"></a>El papel de la canalización de notificaciones
La canalización de notificaciones\) en servicios de Federación de Active Directory (AD FS) \(AD FS representa la ruta de acceso que deben seguir las notificaciones a través del servicio de Federación antes de que se puedan emitir. El servicio de Federación administra todo el proceso\-de\-un extremo a otro para el flujo de notificaciones a través de las distintas fases de la canalización de notificaciones, que también incluye el procesamiento de reglas de notificación por parte del motor de reglas de notificación.  
  
Para obtener más información sobre las reglas de notificaciones, consulte [el rol de las reglas de notificaciones](The-Role-of-Claim-Rules.md). Para obtener más información acerca de cómo procesa las reglas el motor de reglas de notificaciones, consulte [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
En la siguiente sección se explica el proceso que supervisa el Servicio de federación con mayor detalle.  
  
## <a name="claims-pipeline-process"></a>Proceso de canalización de notificaciones  
El proceso de canalización de notificaciones\-consta de tres fases de alto nivel. Cada fase de este proceso inicializa el motor de reglas de notificación para procesar las reglas de notificación que son específicas de esa fase. Estas fases incluyen \(en el orden en que se\)producen:  
  
1.  Aceptación de notificaciones entrantes: esta fase de la canalización de notificaciones se usa para extraer las notificaciones entrantes desde el token y eliminar notificaciones no esperadas o que no son de confianza. Una vez extraídas, se ejecutan las reglas de aceptación que componen el conjunto de reglas de transformación de aceptación para un proveedor de reclamaciones de confianza. Estas reglas pueden usarse para pasar o agregar nuevas notificaciones que pueden usarse a continuación en las fases posteriores de la canalización de notificaciones. El resultado de esta fase se usa como entrada para la segunda y tercera fase.  
  
2.  Autorización del solicitante de notificaciones: esta fase es usada por el motor de notificaciones para emitir, permitir o denegar notificaciones en función de si el solicitante del token está autorizado a obtener un token para el usuario de confianza proporcionado o no. Sin embargo, para que esto suceda, se ejecutan las reglas de autorización que forman el conjunto de reglas de autorización de emisión o el conjunto de reglas de autorización de delegación para una relación de confianza para usuario autenticado.  
  
3.  Emisión de notificaciones salientes: esta fase se usa para emitir notificaciones salientes y enviarlas a lo largo de la canalización, donde se empaquetarán en un token de seguridad. Sin embargo, para que esto pueda suceder, se ejecutan las reglas de emisión que componen el conjunto de reglas de transformación de emisión establecida para una relación de confianza para usuario autenticado, que determinarán qué notificaciones se emitirán como notificaciones salientes.  
  
Las tres fases anteriores realizan el procesamiento de reglas de notificaciones, pero usan un conjunto diferente de reglas. Tal y como se ha descrito anteriormente, cada fase tiene un conjunto asociado de reglas basadas en el emisor de las \(notificaciones entrantes las reglas\) de aceptación o el servicio de destino para \(el que se emite la autorización de claimincludes y reglas\)de emisión.  
  
Las notificaciones\-son independientes del token, pero se transmiten a través de la red encapsulada en tokens de seguridad. Las reglas de notificación operan sobre notificaciones independientemente del formato del token de seguridad entrante o saliente.  
  
Las reglas de notificaciones\-contienen la lógica definida por el administrador mediante la cual el motor de notificaciones aceptará las notificaciones entrantes, autorizará notificaciones basadas en la identidad del solicitante y emitirá notificaciones necesarias para un usuario de confianza. En última instancia, es el motor de notificaciones el que determina qué notificaciones entrarán en el token de seguridad que se emitirá después de que se haya introducido la notificación a través de la canalización de notificaciones.  
  
Como se muestra en la siguiente ilustración, la canalización de notificaciones es responsable\-de\-todo el proceso de extremo a extremo de flujo de una notificación a través de las distintas fases de canalización para terminar con una notificación emitida que se enviará a través de una entidad de confianza. La notificación saliente de la ilustración representa la notificación emitida.  
  
![Roles AD FS](media/adfs2_pipeline.gif)  
  
Aunque no se muestra en la ilustración, el motor de notificaciones es el que realiza el procesamiento real de las reglas en cada fase. Para obtener más información, consulte [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  

