---
title: Protección del volumen del sistema con protección de disco
description: Proporciona información sobre la protección de disco para Multipoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 18694665-ed65-4d84-8505-f460cf3df907
author: evaseydl
manager: scotman
ms.author: evas
ms.openlocfilehash: 27bcce68cfe5856e987f5ad93460abd12217c26b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389511"
---
# <a name="protecting-the-system-volume-with-disk-protection"></a>Protección del volumen del sistema con protección de disco
Multipoint Services ofrece la opción de borrar instantáneamente cualquier cambio en el volumen del sistema cada vez que se inicia el equipo. Si habilita la característica protección de disco, las modificaciones que se realicen en la unidad, como daños en la configuración o la introducción de malware, se desharán la próxima vez que se reinicie el equipo. Se trata de una característica adecuada para los administradores que quieren asegurarse de que una imagen de software "buena" o "Golden" se cargue cada vez. Las actualizaciones automáticas o la revisión de software se pueden programar, por ejemplo, en el medio de la noche. La consideración de planeación es si desea que los usuarios finales puedan realizar cambios, como la instalación de software, desde Internet. Con esta característica habilitada, si desea que los usuarios puedan almacenar archivos, el recurso compartido de archivos deberá estar fuera del volumen del sistema.  
  
