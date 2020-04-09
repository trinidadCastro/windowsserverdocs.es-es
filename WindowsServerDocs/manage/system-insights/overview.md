---
title: Introducción a información del sistema
description: System Insights es una nueva característica de análisis predictivo en Windows Server 2019. Las funcionalidades de predicción de System Insights, cada una de las cuales está respaldada por un modelo de aprendizaje automático, analizan de forma local los datos del sistema de Windows Server, como los contadores de rendimiento y los eventos, lo que proporciona una visión general del funcionamiento de los servidores y ayuda a reducir los gastos operativos asociados a la administración de problemas de forma reactiva.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 5/23/2018
ms.openlocfilehash: e2530ff9ecb4bcf69f2f9a3a452f51696b4466d2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815948"
---
# <a name="system-insights-overview"></a>Introducción a información del sistema

>Se aplica a: Windows Server 2019

System Insights es una nueva característica de análisis predictivo en Windows Server 2019. Las funcionalidades de predicción de System Insights, cada una de las cuales está respaldada por un modelo de aprendizaje automático, analizan de forma local los datos del sistema de Windows Server, como los contadores de rendimiento y los eventos, lo que proporciona una visión general del funcionamiento de los servidores y ayuda a reducir los gastos operativos asociados a la administración de problemas de forma reactiva. 

En Windows Server 2019, System Insights se incluye con cuatro funcionalidades predeterminadas centradas en la previsión de la capacidad, lo que predice recursos futuros de proceso, redes y almacenamiento en función de los patrones de uso anteriores. System Insights también se suministra con una [infraestructura extensible](adding-and-developing-capabilities.md), por lo que Microsoft y terceros pueden agregar nuevas capacidades de predicción a la información del sistema sin necesidad de actualizar el sistema operativo. 

Puede administrar información del sistema a través de una extensión intuitiva del [centro de administración de Windows](https://docs.microsoft.com/windows-server/manage/windows-admin-center/overview) o [directamente a través de PowerShell](https://aka.ms/SystemInsightsPowerShell), y System Insights le permite configurar cada una de las funcionalidades predictivas de forma independiente según las necesidades de su implementación. Todos los resultados de predicción se publican en el registro de eventos, lo que permite usar [Azure monitor](https://azure.microsoft.com/services/monitor/) o [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) para agregar fácilmente y ver las predicciones en un grupo de máquinas.

![Extensión de System Insights en el centro de administración de Windows, donde se muestra la capacidad de previsión de la capacidad de CPU con un gráfico que traza la previsión](media/cpu-forecast-2.png)

## <a name="local-functionality"></a>Funcionalidad local
System Insights se ejecuta completamente localmente en Windows Server. Con la nueva funcionalidad introducida en Windows Server 2019, todos los datos se recopilan, conservan y analizan directamente en el equipo, lo que le permite obtener capacidades de análisis predictivo sin ninguna conectividad en la nube.

Los datos del sistema se almacenan en el equipo y los datos se analizan mediante funcionalidades predictivas que no requieren reciclaje en la nube. Con System Insights, puede conservar los datos en la máquina y seguir beneficiándose de las capacidades de análisis predictivo. 

## <a name="get-started"></a>Introducción

<iframe src=https://www.youtube-nocookie.com/embed/AJxQkx5WSaA width=560 height=315 allowfullscreen></iframe>

>[!TIP]
>Vea estos breves vídeos para conocer la información que necesita para empezar a trabajar y administrar con confianza el conocimiento del sistema: [Introducción a System Insights en 10 minutos](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

### <a name="requirements"></a>Requisitos
System Insights está disponible en cualquier instancia de Windows Server 2019. Se ejecuta tanto en el host como en los equipos invitados, en cualquier hipervisor y en cualquier nube.

### <a name="install-system-insights"></a>Instalación del sistema Insights
>[!IMPORTANT]
>System Insights recopila y almacena hasta un año de datos de forma local. Si desea conservar los datos al actualizar el sistema operativo, asegúrese **de usar la actualización en contexto**.

#### <a name="install-the-feature"></a>Instalar la característica
Puede instalar System Insights mediante la extensión del centro de administración de Windows:

![Experiencia del día 0 para la extensión de System Insights.](media/day-0-2.png)

También puede instalar información del sistema directamente a través de Administrador del servidor agregando la característica **System-Insights** o mediante PowerShell:

```PowerShell
Add-WindowsFeature System-Insights -IncludeManagementTools
```

## <a name="provide-feedback"></a>Enviar comentarios
Nos encantaría conocer sus comentarios para ayudarnos a mejorar esta característica. Puede usar los siguientes canales para enviar comentarios:
- **Centro de comentarios**: Use la herramienta centro de comentarios de Windows 10 para archivar un error o comentarios. Al hacerlo, especifique:
    - **Categoría**: servidor 
    - **Subcategoría**: System Insights
- **UserVoice**: envíe solicitudes de características a través de nuestra [Página de uservoice](https://windowsserver.uservoice.com/forums/295071-management-tools). Comparta con sus colegas para votar elementos que sean importantes para usted.
- **Correo electrónico**: Si desea enviar sus comentarios de forma privada al equipo de características, envíe un correo electrónico a system-insights-feed@microsoft.com. Tenga en cuenta que todavía podemos solicitarle que use el centro de opiniones o UserVoice.

## <a name="see-also"></a>Vea también
Para obtener más información acerca de System Insights, utilice los siguientes recursos:

- [Descripción de funcionalidades](understanding-capabilities.md)
- [Administración de funcionalidades](managing-capabilities.md)
- [Funcionalidades de adición y desarrollo](adding-and-developing-capabilities.md)
- [Preguntas más frecuentes sobre System Insights](faq.md)