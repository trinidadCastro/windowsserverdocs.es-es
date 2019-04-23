---
title: Configuración de HGS para comunicaciones Https
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 83529a5bdb4547b9881bb307a8a4cd526552d02c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829006"
---
# <a name="configure-hgs-for-https-communications"></a>Configuración de HGS para comunicaciones HTTPS

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

De forma predeterminada, al inicializar el servidor HGS configurará los sitios web IIS para las comunicaciones de sólo HTTP.
Todo el material confidencial que se transmiten a y desde HGS siempre se cifran mediante cifrado de nivel de mensaje, sin embargo, si desea un mayor nivel de seguridad también puede habilitar HTTPS mediante la configuración de HGS con un certificado SSL.

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)] 

