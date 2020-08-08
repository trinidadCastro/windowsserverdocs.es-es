---
title: Configuración de HGS para comunicaciones HTTPS
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 4e708b61bd629b5b784926338b1aee122e2ecbee
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87966162"
---
# <a name="configure-hgs-for-https-communications"></a>Configuración de HGS para comunicaciones HTTPS

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

De forma predeterminada, cuando se inicializa el servidor HGS, se configuran los sitios web de IIS para las comunicaciones solo HTTP.
Todo el material confidencial que se transmite hacia y desde HGS se cifra siempre mediante el cifrado de nivel de mensaje, pero si desea un mayor nivel de seguridad, también puede habilitar HTTPS mediante la configuración de HGS con un certificado SSL.

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)]

