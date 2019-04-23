---
ms.assetid: cc2834ec-8f66-4209-aba3-402d710cd1bd
title: Ubicación del controlador de dominio
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6b66b224278f15b6abeecbef8fe0778a98159bb7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872626"
---
# <a name="domain-controller-location"></a>Ubicación del controlador de dominio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los clientes usan el sistema de nombres de dominio (DNS) para buscar controladores de dominio para operaciones completas como procesamiento de solicitudes de inicio de sesión o buscar en el directorio para los recursos publicados. Controladores de dominio registran una variedad de registros en DNS para ayudar a los clientes y buscarlos en otros equipos. Estos registros se conocen colectivamente como los registros de localizador.  
  
Los controladores de dominio también usan DNS para localizar a otros controladores de dominio y para realizar tareas como la replicación. El proceso por el que los controladores de dominio localizar a otros controladores de dominio es el mismo que el proceso por el que los clientes buscan controladores de dominio.  
  


