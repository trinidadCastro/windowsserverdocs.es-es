---
title: Prehash y precargar contenido en el servidor de memoria caché hospedada (opcional)
description: Esta guía brinda instrucciones sobre cómo implementar BranchCache en modo de memoria caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0e7ffaac4e427222d5539195ecef91768f61c4a3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>Prehash y precargar contenido en hospedado servidor \(Optional\) en caché

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puedes usar los procedimientos descritos en esta sección prehash contenido en los servidores de contenido, agrega el contenido para paquetes de datos y, a continuación, cargar el contenido en los servidores de la memoria caché hospedada. 

Estos procedimientos son opcionales, ya que no es necesarios para el contenido prehash y precarga en los servidores de la memoria caché hospedada. 

Si no precargar contenido, se agregan automáticamente a la memoria caché hospedada datos como los clientes descargan con la conexión WAN.

>[!IMPORTANT]
>Si bien estos procedimientos son opcionales colectivamente, si decides contenido prehash y precarga en los servidores de la memoria caché hospedada, realizar dos procedimientos es necesario.

- [Crear paquetes de datos del servidor de contenido Web y contenido de los archivos & #40; opcional & #41;](8-Bc-Data-Packages.md)
  
- [Importar paquetes de datos en el servidor de la memoria caché hospedada & #40; opcional & #41;](9-Bc-Import-Data.md)

Para continuar con esta guía, consulte [crear contenido servidor datos paquetes para la Web y contenido de archivo & #40; opcional & #41;](8-Bc-Data-Packages.md).