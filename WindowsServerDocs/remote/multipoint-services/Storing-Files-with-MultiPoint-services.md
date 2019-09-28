---
title: Almacenamiento de archivos con MultiPoint Services
description: Más información sobre el almacenamiento de archivos en Multipoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c9eb0461-3846-4ddc-97ff-de10f03f30cf
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: bf31f5c582cffb5b38cff8cb15fcfdb4b3f76386
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389298"
---
# <a name="storing-files-with-multipoint-services"></a>Almacenamiento de archivos con MultiPoint Services
Multipoint Services admite el almacenamiento de archivos de usuario de las siguientes maneras:  
  
-   **En la partición del sistema operativo de la unidad de disco duro.** De forma predeterminada, Multipoint Services almacena los archivos de usuario en la unidad de disco duro con el sistema operativo.  
  
-   **En una partición independiente de la unidad de disco duro.** Cuando el sistema Multipoint Services se configura por primera vez, puede *crear particiones* en la unidad de disco duro. Es decir, puede configurar una sección de la unidad para que funcione como si fuera una unidad independiente. De este modo, resulta más fácil restaurar o actualizar el sistema operativo sin que ello afecte a los archivos de usuario. Para obtener más información, vea [crear una partición o una unidad lógica](https://go.microsoft.com/fwlink/?LinkId=182618) en la biblioteca técnica de Windows Server.  
  
-   **En una unidad de disco duro interna o externa adicional.** Puede conectar unidades de disco duro internas o externas adicionales a multipoint Services para guardar y realizar copias de seguridad de los datos.  
  
-   **En una carpeta de red compartida.** Para que los archivos de usuario estén disponibles desde cualquier estación, puede crear una carpeta compartida en la red. Esto requiere otro equipo o servidor además del equipo que ejecuta Multipoint Services. Este es el método recomendado para almacenar archivos si hay un servidor de archivos disponible.  
  
    En el caso de los sistemas pequeños de 2-3 equipos que ejecutan Multipoint Services sin ningún servidor de archivos disponible, uno de los equipos Multipoint Services puede actuar como servidor de archivos para todos los equipos de Multipoint Services. A continuación, debe crear cuentas de usuario para todos los usuarios de Multipoint Services que actúa como servidor de archivos.  
  
