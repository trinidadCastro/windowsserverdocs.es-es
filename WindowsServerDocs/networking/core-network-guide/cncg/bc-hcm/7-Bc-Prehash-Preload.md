---
title: Aplicar previamente hash y precargar contenido en el servidor de caché hospedada (opcional)
description: En esta guía se proporcionan instrucciones sobre cómo implementar BranchCache en modo caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10.
manager: brianlic
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: c9f33a53ac7af8a76d8f6d9b018668ebc8b23771
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950231"
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>Hash y precargar contenido en el servidor de caché hospedada \( opcional\)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar los procedimientos que se describen en esta sección para aplicar el algoritmo hash al contenido de los servidores de contenido, agregar el contenido a los paquetes de datos y, a continuación, cargar previamente el contenido en los servidores de caché hospedada.

Estos procedimientos son opcionales porque no es necesario aplicar previamente hash y precargar contenido en los servidores de caché hospedada.

Si no carga previamente el contenido, los datos se agregan automáticamente a la caché hospedada a medida que los clientes lo descargan a través de la conexión WAN.

>[!IMPORTANT]
>Aunque estos procedimientos son colectivamente opcionales, si decide aplicar previamente hash y precargar contenido en los servidores de caché hospedada, es necesario realizar ambos procedimientos.

- [Crear paquetes de datos del servidor de contenido para web y contenido de archivo &#40;&#41;opcional ](8-Bc-Data-Packages.md)

- [Importar paquetes de datos en el servidor de caché hospedada &#40;&#41;opcional ](9-Bc-Import-Data.md)

Para continuar con esta guía, consulte [crear paquetes de datos del servidor de contenido para web y contenido de archivo &#40;&#41;opcional ](8-Bc-Data-Packages.md).