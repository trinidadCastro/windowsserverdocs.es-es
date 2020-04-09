---
title: Configuración de HGS para comunicaciones HTTPS
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: de57a4026a33561760ad36fd78d732352b3aa340
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856848"
---
# <a name="configure-hgs-for-https-communications"></a>Configuración de HGS para comunicaciones HTTPS

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

De forma predeterminada, cuando se inicializa el servidor HGS, se configuran los sitios web de IIS para las comunicaciones solo HTTP.
Todo el material confidencial que se transmite hacia y desde HGS se cifra siempre mediante el cifrado de nivel de mensaje, pero si desea un mayor nivel de seguridad, también puede habilitar HTTPS mediante la configuración de HGS con un certificado SSL.

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)] 

