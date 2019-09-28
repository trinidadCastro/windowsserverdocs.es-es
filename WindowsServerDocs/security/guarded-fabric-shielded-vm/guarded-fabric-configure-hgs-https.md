---
title: Configuración de HGS para comunicaciones https
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: f0cbf6a6dc1970499758a6a48bfaadb95c464ec1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403678"
---
# <a name="configure-hgs-for-https-communications"></a>Configuración de HGS para comunicaciones HTTPS

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

De forma predeterminada, cuando se inicializa el servidor HGS, se configuran los sitios web de IIS para las comunicaciones solo HTTP.
Todo el material confidencial que se transmite hacia y desde HGS se cifra siempre mediante el cifrado de nivel de mensaje, pero si desea un mayor nivel de seguridad, también puede habilitar HTTPS mediante la configuración de HGS con un certificado SSL.

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)] 

