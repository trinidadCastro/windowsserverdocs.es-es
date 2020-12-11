---
description: Más información acerca de cómo configurar HGS para comunicaciones HTTPS
title: Configuración de HGS para comunicaciones HTTPS
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: c28500d236625410961ef1ee3012001c1d277714
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049893"
---
# <a name="configure-hgs-for-https-communications"></a>Configuración de HGS para comunicaciones HTTPS

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

De forma predeterminada, cuando se inicializa el servidor HGS, se configuran los sitios web de IIS para las comunicaciones solo HTTP.
Todo el material confidencial que se transmite hacia y desde HGS se cifra siempre mediante el cifrado de nivel de mensaje, pero si desea un mayor nivel de seguridad, también puede habilitar HTTPS mediante la configuración de HGS con un certificado SSL.

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)]

