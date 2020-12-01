---
title: 'RDS: ejecución y ajuste del entorno de Servicios de Escritorio remoto'
description: Proporciona información de administración de los Servicios de Escritorio remoto.
ms.author: spatnaik
ms.date: 02/08/2017
ms.topic: article
ms.assetid: 79909767-a4c3-4ecf-8d3f-77d37a663153
author: spatnaik
manager: scottman
ms.openlocfilehash: 5a68569943ac540204fd3538719c2a0ef6c4c979
ms.sourcegitcommit: 3181fcb69a368f38e0d66002e8bc6fd9628b1acc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96330437"
---
# <a name="run-and-tune-your-remote-desktop-services-environment"></a>Ejecución y ajuste del entorno de Servicios de Escritorio remoto

La optimización de la implementación lleva tiempo y requiere instrumentación y supervisión. Usa los procesos siguientes para mejorar la implementación de Escritorio remoto, mantenerla en funcionamiento y permitir el escalado horizontal (y la reducción horizontal) según sea necesario.

Es recomendable evaluar continuamente las métricas y el equilibrio en comparación con los costos de funcionamiento.

## <a name="management-and-monitoring"></a>Administración y supervisión

Consulte el artículo [Administración de usuarios de la colección de RDS](rds-user-management.md) para obtener información sobre cómo administrar el acceso a recursos remotos y escritorios.

Usa **Microsoft Operations Management Suite (OMS)** para supervisar las implementaciones de Escritorio remoto y detectar posibles cuellos de botella, y administrarlas mediante una de las siguientes maneras:

- **Administrador del servidor**: Usa la herramienta de administración de Escritorio remoto que está integrada en Windows Server para administrar las implementaciones con hasta 500 usuarios finales remotos simultáneos.
- **PowerShell**: Usa el módulo de PowerShell de Escritorio remoto que está integrado en Windows Server para administrar las implementaciones con hasta 5000 usuarios finales remotos simultáneos.

## <a name="scale-bigger-better-faster"></a>Escalado: Más grande, mejor, más rápido

Con visibilidad sobre la implementación, puedes controlar el escalado con más precisión. Agrega o quita fácilmente servidores host de Escritorio remoto en función de las necesidades del escalado.

Las implementaciones de Escritorio remoto que se crean en Azure pueden utilizar los servicios de Azure, como Azure SQL, para escalar automáticamente a petición.

## <a name="automation-script-for-success"></a>Automatización: Scripts para tener éxito

Mantener una aplicación en ejecución y muy escalada conlleva la repetición de operaciones de manera regular. Usa los cmdlets de PowerShell para los Servicios de Escritorio remoto y los proveedores de WMI para desarrollar scripts que se puedan ejecutar en varias implementaciones cuando sea necesario. Ejecuta las reglas del Analizador de procedimientos recomendados (BPA) para optimizar las implementaciones.

## <a name="load-testing-avoid-surprises"></a>Pruebas de carga: Evita sorpresas

Haz una prueba de carga de la implementación con pruebas de esfuerzo y simulación de uso real. ¡Cambia el tamaño de la carga para evitar sorpresas! Asegúrate de que la capacidad de respuesta cumpla los requisitos del usuario y que todo el sistema sea resistente. Crea pruebas de carga con herramientas de simulación, como LoginVSI, que comprueban la capacidad de la implementación para satisfacer las necesidades de los usuarios.
