---
title: Funcionalidades de adición y desarrollo
description: System Insights le permite agregar nuevas capacidades de predicción a información del sistema, sin necesidad de ninguna actualización del sistema operativo. Esto permite a los desarrolladores, incluidos Microsoft y terceros, crear y ofrecer nuevas capacidades de versión mediados para abordar los escenarios que le interesan. Las nuevas funcionalidades pueden especificar datos personalizados para recopilar y analizar, y también se integran con los planes de administración de System Insights existentes.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 891edb108bc2c298c70b29c596fb8d0ca0e06b7e
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475192"
---
# <a name="adding-and-developing-new-capabilities"></a>Adición y desarrollo de nuevas capacidades

>Se aplica a: Windows Server 2019

System Insights le permite agregar nuevas capacidades de predicción a información del sistema, sin necesidad de ninguna actualización del sistema operativo. Esto permite a los desarrolladores, incluidos Microsoft y terceros, crear y ofrecer nuevas capacidades de versión mediados para abordar los escenarios que le interesan.

Cualquier nueva funcionalidad puede integrarse con la infraestructura de System Insights existente y ampliarla:

- Las nuevas capacidades pueden **especificar cualquier contador de rendimiento o evento del sistema**, que se recopilará, se conservará localmente y se devolverá a la capacidad de análisis cuando se invoque la funcionalidad.
- Las nuevas capacidades pueden **aprovechar el centro de administración de Windows existente y los planos de administración de PowerShell**. No solo se podrán detectar nuevas funcionalidades en información del sistema, sino que también se beneficiarán de las programaciones personalizadas y las acciones de corrección.

## <a name="manage-new-capabilities"></a>Administrar nuevas funcionalidades
- [Obtenga información sobre](add-remove-update-capabilities.md) cómo agregar, quitar y actualizar funcionalidades con PowerShell.

## <a name="develop-a-capability"></a>Desarrollar una funcionalidad
Use los siguientes recursos para ayudarle a empezar a escribir sus propias funcionalidades personalizadas:
- [Obtenga información](data-sources.md) sobre los orígenes de datos que puede recopilar.
- [Descargue](https://www.nuget.org/packages/Microsoft.WindowsServer.SystemInsights/) el paquete NuGet de System Insights, que contiene las clases e interfaces que necesita para escribir una funcionalidad.
- [Visite](https://aka.ms/systeminsights-api) la documentación de la API para obtener información sobre las clases e interfaces de System Insights.
- [Use](https://aka.ms/systeminsights-samplecapability) la funcionalidad de ejemplo de System Insights para ayudarle a empezar. Esto muestra cómo registrar una funcionalidad, especificar los orígenes de datos que se van a recopilar y empezar a analizar los datos del sistema.

>[!NOTE]
>Esta es la funcionalidad de versión preliminar. Está sujeto a cambios, a medida que agregamos nueva funcionalidad e incorporamos Comentarios.

## <a name="additional-references"></a>Referencias adicionales
Para obtener más información acerca de System Insights, utilice los siguientes recursos:

- [Introducción a información del sistema](overview.md)
- [Descripción de funcionalidades](understanding-capabilities.md)
- [Administración de funcionalidades](managing-capabilities.md)
- [Funcionalidades de adición, eliminación y actualización](add-remove-update-capabilities.md)
- [Preguntas más frecuentes sobre System Insights](faq.md)