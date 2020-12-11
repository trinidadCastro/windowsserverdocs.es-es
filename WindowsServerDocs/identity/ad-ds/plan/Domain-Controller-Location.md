---
description: 'Más información acerca de: Ubicación del controlador de dominio'
ms.assetid: cc2834ec-8f66-4209-aba3-402d710cd1bd
title: Ubicación del controlador de dominio
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 6885f9cabc9f9f60939e41858f04537cb1bcfcbe
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97045123"
---
# <a name="domain-controller-location"></a>Ubicación del controlador de dominio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los clientes usan el sistema de nombres de dominio (DNS) para buscar controladores de dominio para completar operaciones como el procesamiento de solicitudes de inicio de sesión o la búsqueda de recursos publicados en el directorio. Los controladores de dominio registran una variedad de registros en DNS para ayudar a los clientes y otros equipos a localizarlos. Estos registros se conocen colectivamente como registros de localizador.

Los controladores de dominio también usan DNS para buscar otros controladores de dominio y realizar tareas como la replicación. El proceso por el que los controladores de dominio buscan otros controladores de dominio es el mismo que el proceso por el que los clientes ubican controladores de dominio.



