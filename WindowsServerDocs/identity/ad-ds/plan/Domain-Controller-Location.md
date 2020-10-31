---
ms.assetid: cc2834ec-8f66-4209-aba3-402d710cd1bd
title: Ubicación del controlador de dominio
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: d22abba814d9cd4b18294b3c54d550b28c2dfef5
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93068727"
---
# <a name="domain-controller-location"></a>Ubicación del controlador de dominio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los clientes usan el sistema de nombres de dominio (DNS) para buscar controladores de dominio para completar operaciones como el procesamiento de solicitudes de inicio de sesión o la búsqueda de recursos publicados en el directorio. Los controladores de dominio registran una variedad de registros en DNS para ayudar a los clientes y otros equipos a localizarlos. Estos registros se conocen colectivamente como registros de localizador.

Los controladores de dominio también usan DNS para buscar otros controladores de dominio y realizar tareas como la replicación. El proceso por el que los controladores de dominio buscan otros controladores de dominio es el mismo que el proceso por el que los clientes ubican controladores de dominio.



