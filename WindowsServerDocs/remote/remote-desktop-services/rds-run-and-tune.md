---
title: 'RDS: ejecución y optimización'
description: Proporciona información de administración de servicios de escritorio remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 02/08/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79909767-a4c3-4ecf-8d3f-77d37a663153
author: spatnaik
manager: scottman
ms.openlocfilehash: 40f8dbd560da359e8764ed715e7776cc2d230a7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862186"
---
# <a name="run-and-tune-your-remote-desktop-services-environment"></a>Ejecutar y optimizar su entorno de servicios de escritorio remoto

Optimización de la implementación lleva tiempo y requiere la instrumentación y supervisión. Use los procesos siguientes para mejorar la implementación de escritorio remoto, siga ejecutándose y permiten (escalado y reducción horizontal) según sea necesario. 

Es una buena práctica evaluar continuamente las métricas y equilibrar frente a los costos de ejecución.

## <a name="management-and-monitoring"></a>Administración y supervisión

Desproteger [administrar usuarios en la colección de RDS](rds-user-management.md) para obtener información sobre cómo administrar el acceso a recursos remotos y escritorios.

Use **Microsoft Operations Management Suite (OMS)** para supervisar las implementaciones de escritorio remoto para posibles cuellos de botella y administrarlos mediante una de las maneras siguientes: 

- **El administrador del servidor**: Use la herramienta de administración de escritorio remoto que está integrada Windows Server para administrar las implementaciones con hasta 500 simultáneas remoto a los usuarios finales. 
- **PowerShell**: Usar el módulo de PowerShell de escritorio remoto, también integrado en Windows Server, para administrar las implementaciones con hasta 5000 simultáneas remoto a los usuarios finales.

## <a name="scale-bigger-better-faster"></a>Escala: Más grande, mejor, más rápido

Visibilidad de la implementación, puede controlar la escala con más precisión. Agregar o quitar servidores de host de escritorio remoto en función de las necesidades de escala fácilmente. 

Las implementaciones de escritorio remotas que se basan en Azure pueden hacer uso de servicios de Azure, como SQL Azure, para escalar automáticamente a petición.

## <a name="automation-script-for-success"></a>Automatización: Secuencia de comandos para el éxito

Mantener una aplicación de ejecución, con mucho escalada consiste en repetir las operaciones de forma periódica. Use los cmdlets de PowerShell de servicios de escritorio remoto y los proveedores de WMI para desarrollar scripts que se pueden ejecutar en varias implementaciones cuando sea necesario. Ejecutar las reglas del analizador de procedimientos recomendados (BPA) para servicios de escritorio remoto en las implementaciones para optimizar las implementaciones.

## <a name="load-testing-avoid-surprises"></a>Pruebas de carga: Evitar sorpresas

Prueba de carga de la implementación con las pruebas de esfuerzo y simulación de uso reales. ¡Cambiar el tamaño de carga para evitar sorpresas! Asegúrese de que la capacidad de respuesta cumple los requisitos del usuario, y que todo el sistema es resistente. Crear pruebas de carga con las herramientas de simulación, como LoginVSI, que comprueban la capacidad de su implementación para satisfacer las necesidades del usuario. 