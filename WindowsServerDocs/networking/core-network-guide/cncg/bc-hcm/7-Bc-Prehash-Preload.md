---
title: Aplicar previamente hash y precargar contenido en el servidor de caché hospedada (opcional)
description: Esta guía proporciona instrucciones sobre cómo implementar BranchCache en modo de caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b60a1f24b8988d6e394df0faf678467021e0c882
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839396"
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>Hacer un hash previo y precargar contenido en el servidor de caché hospedada \(opcional\)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar los procedimientos de esta sección para hacer un hash previo contenido en los servidores de contenido, agregue el contenido a los paquetes de datos y, a continuación, precargar el contenido en los servidores de caché hospedada. 

Estos procedimientos son opcionales porque no es necesario para prehash y precarga de contenido en los servidores de caché hospedada. 

Si no precargar contenido, se agregan automáticamente a la memoria caché hospedada datos como los clientes lo descargan a través de la conexión WAN.

>[!IMPORTANT]
>Aunque estos procedimientos son colectivamente opcionales, si decide prehash y precarga de contenido en los servidores de caché hospedada, realizar ambos procedimientos es necesario.

- [Crear paquetes de datos de servidor de contenido para la Web y el contenido del archivo &#40;opcional&#41;](8-Bc-Data-Packages.md)
  
- [Importar paquetes de datos en el servidor de caché hospedada &#40;opcional&#41;](9-Bc-Import-Data.md)

Para continuar con esta guía, consulte [crear de paquetes de datos de servidor de contenido para la Web y el contenido del archivo &#40;opcional&#41;](8-Bc-Data-Packages.md).