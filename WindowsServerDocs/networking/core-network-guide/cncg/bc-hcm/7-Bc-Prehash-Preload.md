---
title: Aplicar previamente hash y precargar contenido en el servidor de caché hospedada (opcional)
description: En esta guía se proporcionan instrucciones sobre cómo implementar BranchCache en modo caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10.
manager: brianlic
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 45d84103f3b41b08beb207bee570b469a3caf3dc
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964231"
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>Hash y precargar contenido en el servidor de caché hospedada \( opcional\)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar los procedimientos que se describen en esta sección para aplicar el algoritmo hash al contenido de los servidores de contenido, agregar el contenido a los paquetes de datos y, a continuación, cargar previamente el contenido en los servidores de caché hospedada.

Estos procedimientos son opcionales porque no es necesario aplicar previamente hash y precargar contenido en los servidores de caché hospedada.

Si no carga previamente el contenido, los datos se agregan automáticamente a la caché hospedada a medida que los clientes lo descargan a través de la conexión WAN.

>[!IMPORTANT]
>Aunque estos procedimientos son colectivamente opcionales, si decide aplicar previamente hash y precargar contenido en los servidores de caché hospedada, es necesario realizar ambos procedimientos.

- [Crear paquetes de datos del servidor de contenido para web y contenido de archivo &#40;&#41;opcional](8-Bc-Data-Packages.md)

- [Importar paquetes de datos en el servidor de caché hospedada &#40;&#41;opcional](9-Bc-Import-Data.md)

Para continuar con esta guía, consulte [crear paquetes de datos del servidor de contenido para web y contenido de archivo &#40;&#41;opcional ](8-Bc-Data-Packages.md).