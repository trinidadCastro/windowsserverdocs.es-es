---
title: Los portátiles remotos se desconectan de la red inalámbrica
description: Solucionar un problema por el que el portátil remoto se desconecta de la red inalámbrica.
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: df43a69fa4777a9286cbe27cfa2d241111f7edf6
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265877"
---
# <a name="remote-laptop-disconnects-from-wireless-network"></a>Los portátiles remotos se desconectan de la red inalámbrica

Este problema se puede producir cuando un cliente de Escritorio remoto se conecta a un portátil mediante una red inalámbrica 802.1x. El portátil se desconecta de forma intermitente de la red inalámbrica y no se vuelve a conectar automáticamente.

Se trata de un problema conocido que se produce cuando la configuración de autenticación de la red para la conexión de la red inalámbrica es **Autenticación de usuario**.

Para solucionar este problema, establezca la configuración de autenticación de red en **Autenticación de usuarios o equipos**  o en **Autenticación de equipo** .

 > [!NOTE]  
> Para cambiar la configuración de autenticación de la red en un equipo individual, es posible que tenga que usar el panel de control del Centro de redes y recursos compartidos para crear una nueva conexión inalámbrica con la nueva configuración.

Para obtener una descripción completa de cómo configurar una red inalámbrica mediante GPO, consulte [Configuración de directivas de redes inalámbricas (IEEE 802.11)](../../../networking/core-network-guide/cncg/wireless/e-wireless-access-deployment.md#bkmk_policies).
