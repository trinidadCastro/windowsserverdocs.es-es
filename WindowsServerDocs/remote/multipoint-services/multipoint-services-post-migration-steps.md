---
title: 'MultiPoint Services: tareas posteriores a la migración'
description: Obtenga información sobre cómo validar y cierre de la migración a MultiPoint Services
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1497cae0-071e-467d-89b8-a7050815d7de
author: lizap
manager: dongill
ms.openlocfilehash: e3fa3c812355a14289ea4eeff3ab1e7e92e00d97
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863506"
---
# <a name="multipoint-services---post-migration-tasks"></a>MultiPoint Services: tareas posteriores a la migración

>Se aplica a: Windows Server 2016

Después de migrar a MultiPoint Services en Windows Server 2016, use la siguiente información para validar la migración y lleve a cabo los pasos de limpieza.

## <a name="validate-the-migration-by-running-a-pilot-program"></a>Validar la migración mediante la ejecución de un programa piloto

Puede validar la migración de MultiPoint Services mediante la creación de un proyecto piloto en el entorno de producción. Ejecute el proyecto piloto en los servidores antes de poner los servicios del rol migrados en producción para comprobar que la implementación funciona según lo esperado. Considere la posibilidad de limitar el número de conexiones en primer lugar, aumentar lentamente el número de usuarios que acceden a MultiPoint Services.

> [!NOTE] 
> Utilice siempre cuentas de prueba para probar la migración. Use una cuenta con privilegios de administrador y una cuenta para un usuario válido.

## <a name="retire-the-source-server"></a>Retirar el servidor de origen
Después de validar la migración, puede apagar o desconectar el servidor de origen de la red. Si el servidor se ha unido al dominio, puede quitarlo del dominio antes de desconectarlo.

