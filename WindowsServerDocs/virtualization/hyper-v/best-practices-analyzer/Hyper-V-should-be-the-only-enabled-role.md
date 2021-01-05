---
title: Hyper-V debe ser el único rol habilitado
description: Obtenga información acerca de qué hacer si los roles distintos de Hyper-V están habilitados en el servidor.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 5a0ed176-048f-40b1-b56c-8391b805fd37
ms.date: 8/16/2016
ms.openlocfilehash: f575f981975253faea30dad610120f3e314a1edf
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97833620"
---
# <a name="hyper-v-should-be-the-only-enabled-role"></a>Hyper-V debe ser el único rol habilitado

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema

*Los roles distintos de Hyper-V están habilitados en este servidor.*

En la mayoría de los casos, no es una buena idea instalar otros roles en un servidor que ejecuta el rol de Hyper-V. Host de virtualización de Escritorio remoto servicio de rol es una excepción, porque forma parte del rol Servicios de Escritorio remoto y requiere que Hyper-V esté instalado en el mismo servidor.

## <a name="impact"></a>Impacto

*El rol de Hyper-V debe ser el único rol habilitado en un servidor.*

Este procedimiento recomendado ayuda a mantener el sistema operativo host libre de roles, características y aplicaciones que no son necesarios para ejecutar Hyper-V. Seguir este procedimiento recomendado y ejecutar Hyper-V en nano Server ayuda a reducir el número de actualizaciones que necesitará, ya que solo nano Server, los componentes del servicio Hyper-V y el hipervisor de Windows estarán sujetos a actualizaciones de software.

## <a name="resolution"></a>Solución

*Use Administrador del servidor para quitar todos los roles excepto Hyper-V.*

Administrador del servidor incluye el Asistente para quitar funciones. Este asistente permite quitar más de un rol a la vez. Antes de quitar roles, el Asistente para quitar roles comprueba las dependencias para reducir el riesgo de quitar el software del que dependen otros roles. Si se encuentran dependencias, el asistente le pedirá que apruebe la eliminación de otras funciones, servicios de rol o software que necesiten los roles instalados.

Para usar Administrador del servidor, debe haber iniciado sesión en el equipo como administrador.

#### <a name="to-remove-a-role"></a>Para quitar un rol

1.  Abra Administrador del servidor mediante accesos directos en el menú **Inicio** , en la barra de tareas de Windows o en herramientas administrativas.
2.   En el área **Resumen de funciones** de la ventana principal de administrador del servidor, haga clic en **quitar funciones**. Siga las instrucciones del Asistente para quitar el rol.





