---
title: Aplicar previamente hash y precargar contenido en el servidor de caché hospedada (opcional)
description: En esta guía se proporcionan instrucciones sobre cómo implementar BranchCache en modo caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fe206576278b09e4a360c7bb27f5ff076af97be7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356257"
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>Hash y precargar contenido en el servidor de caché hospedada \(Optional @ no__t-1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar los procedimientos que se describen en esta sección para aplicar el algoritmo hash al contenido de los servidores de contenido, agregar el contenido a los paquetes de datos y, a continuación, cargar previamente el contenido en los servidores de caché hospedada. 

Estos procedimientos son opcionales porque no es necesario aplicar previamente hash y precargar contenido en los servidores de caché hospedada. 

Si no carga previamente el contenido, los datos se agregan automáticamente a la caché hospedada a medida que los clientes lo descargan a través de la conexión WAN.

>[!IMPORTANT]
>Aunque estos procedimientos son colectivamente opcionales, si decide aplicar previamente hash y precargar contenido en los servidores de caché hospedada, es necesario realizar ambos procedimientos.

- [Crear paquetes de datos de servidor de contenido para Web &#40;y contenido de archivo opcional&#41;](8-Bc-Data-Packages.md)
  
- [Importar paquetes de datos en el servidor &#40;de caché hospedada opcional&#41;](9-Bc-Import-Data.md)

Para continuar con esta guía, consulte [crear paquetes de datos del servidor de contenido para Web &#40;y&#41;contenido de archivo opcional](8-Bc-Data-Packages.md).