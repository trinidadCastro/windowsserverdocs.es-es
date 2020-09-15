---
title: 'Multipoint Services: tareas posteriores a la migración'
description: Obtenga información sobre cómo validar y cerrar la migración a multipoint Services
ms.date: 07/29/2016
ms.topic: article
ms.assetid: 1497cae0-071e-467d-89b8-a7050815d7de
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 4cfd658c8d5ed6109bd18c7ebb06ce6fcf355661
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077582"
---
# <a name="multipoint-services---post-migration-tasks"></a>Multipoint Services: tareas posteriores a la migración

>Se aplica a: Windows Server 2016

Después de migrar a multipoint Services en Windows Server 2016, use la siguiente información para validar la migración y realizar los pasos de limpieza.

## <a name="validate-the-migration-by-running-a-pilot-program"></a>Validar la migración mediante la ejecución de un programa piloto

Puede validar la migración de Multipoint Services mediante la creación de un proyecto piloto en el entorno de producción. Ejecute el proyecto piloto en los servidores antes de poner los servicios de rol migrados en producción para comprobar que la implementación funciona como se espera. Considere la posibilidad de limitar el número de conexiones al principio, lo que aumenta lentamente el número de usuarios que acceden a multipoint Services.

> [!NOTE]
> Use siempre las cuentas de prueba para probar la migración. Use una cuenta con privilegios de administrador y una cuenta para un usuario válido.

## <a name="retire-the-source-server"></a>Retirar el servidor de origen
Después de validar la migración, puede apagar o desconectar el servidor de origen de la red. Si el servidor está unido a un dominio, quítelo del dominio antes de desconectarlo.

