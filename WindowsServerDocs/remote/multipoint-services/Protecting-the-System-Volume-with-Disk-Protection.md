---
title: Protección del volumen del sistema con protección de disco
description: Proporciona información sobre la protección de disco de MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 18694665-ed65-4d84-8505-f460cf3df907
author: evaseydl
manager: scotman
ms.author: evas
ms.openlocfilehash: a663bb27a7480066c997844d68b33774f9032e92
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855216"
---
# <a name="protecting-the-system-volume-with-disk-protection"></a>Protección del volumen del sistema con protección de disco
MultiPoint Services ofrece la opción de borrar al instante los cambios realizados en el volumen del sistema cada vez que se inicia el equipo. Si habilita la característica de protección de disco, las modificaciones en la unidad, como daños en la configuración o la introducción de tipos de malware, se deshacen la próxima vez que se reinicie el equipo. Esta es una característica conveniente para los administradores que deseen asegurarse de que una image de software conocido de "buenas" o "golden" se carga cada vez. Las actualizaciones automáticas o las revisiones de software se pueden programar, por ejemplo, durante la noche. La consideración de diseño es si desea que los usuarios finales puedan realizar cambios, como la instalación de software, desde Internet. Con esta característica habilitada, si desea que los usuarios para poder almacenar los archivos, el recurso compartido de archivos debe estar fuera del volumen del sistema.  
  
