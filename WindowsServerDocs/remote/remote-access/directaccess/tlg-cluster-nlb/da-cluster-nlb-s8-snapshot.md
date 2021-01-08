---
title: 'PASO 8: instantánea del clúster de DirectAccess: configuración de NLB'
description: Obtenga información sobre cómo crear y guardar una instantánea del laboratorio de prueba de la configuración de NLB del clúster de DirectAccess.
manager: brianlic
ms.topic: article
ms.assetid: 915ef7dd-169d-4d58-9174-438d8ffa3584
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: c9eb0664d878c0050aae238cd0ab3fbc7a716d89
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040235"
---
# <a name="step-8-snapshot-the-directaccess-cluster-nlb-configuration"></a>PASO 8: instantánea del clúster de DirectAccess: configuración de NLB

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Esto completa el laboratorio de pruebas de DirectAccess. Para guardar esta configuración a fin de poder volver rápidamente a un DirectAccess en funcionamiento con la configuración de clúster NLB desde la que puede probar otras guías del laboratorio de pruebas modulares de DirectAccess, extensiones de la guía del laboratorio de pruebas o para su propio aprendizaje y experimentación, haga lo siguiente:

1.  En todos los equipos físicos o las máquinas virtuales del laboratorio de prueba, cierre todas las ventanas y, a continuación, realice un cierre estable.

2.  Si el laboratorio se basa en máquinas virtuales, guarde una instantánea de cada máquina virtual y asigne un nombre al clúster de DirectAccess de las instantáneas y al NLB. Si el laboratorio usa equipos físicos, cree imágenes de disco para guardar la configuración del laboratorio de pruebas de DirectAccess.
