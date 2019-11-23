---
title: Introducción al registro de inventario de software
description: Describe cómo instalar y empezar a usar el registro de inventario de software
ms.custom: na
ms.prod: windows-server
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed51c13c-7cbf-4144-a675-011fd29379d4
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a38c984b2d81fc4db980a969ef0312109950b867
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382999"
---
# <a name="get-started-with-software-inventory-logging"></a>Introducción al registro de inventario de software

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

 El registro de inventario de software recopila datos de inventario de software de Microsoft por servidor. Antes de usar el registro de inventario de software con Windows Server 2012 R2, asegúrese de que Windows Update [kb 3000850](https://support.microsoft.com/kb/3000850) y [KB 3060681](https://support.microsoft.com/kb/3060681) están instalados en cada sistema en el que se va a realizar el inventario. No se requiere ningún Windows Update para Windows Server 2016. Además, si desea usar la capacidad de SIL para reenviar datos a un servidor de agregación, asegúrese de que tiene certificados SSL válidos para la red.

## <a name="BKMK_OVER"></a>Descripción de la característica
El registro de inventario de software de Windows Server es una característica dotada de un sencillo conjunto de cmdlets de PowerShell que ayuda a los administradores de servidores a recuperar una lista del software de Microsoft instalado en sus servidores. También proporciona la capacidad de recopilar y reenviar estos datos periódicamente a través de la red a un servidor web de destino, mediante el protocolo HTTPS, para la agregación. También se usan comandos de PowerShell para administrar la característica, en particular para recopilar datos cada hora y reenviarlos.

> [!NOTE]
> Se puede configurar por separado un servidor de agregación que ejecute un servicio web. Obtenga más información sobre [Agregador de Registro de inventario de software](software-inventory-logging-aggregator.md).

> [!IMPORTANT]
> Ninguno de los datos recopilados por el registro de inventario de software se envía a Microsoft como parte de la funcionalidad de la característica.

## <a name="BKMK_APP"></a>Aplicaciones prácticas
El registro de inventario de software está pensado para reducir los costes operativos derivados de la recuperación de información precisa sobre el software de Microsoft implementado de forma local en un servidor y, más particularmente, en numerosos servidores en un entorno de TI (suponiendo que esté implementado y habilitado en dicho entorno). Al poder reenviar estos datos a un servidor de agregación (que un administrador de TI deberá configurar por separado), los datos se recopilan en una misma ubicación mediante un proceso automático y uniforme. Aunque esto también se puede conseguir con una consulta directa a las interfaces, el registro de inventario de software, mediante una arquitectura de reenvío (a través de la red) que se inicia en cada servidor, puede superar los retos de detección de equipos típicos en muchos escenarios relativos al inventario y a la administración de activos de software. SSL se usa para proteger los datos reenviados a través de HTTPS a un servidor de agregación de un administrador. Tener los datos en una única ubicación (en un servidor) simplifica su análisis y manipulación, y la elaboración de informes sobre ellos. Es importante destacar que estos datos no se envían a Microsoft como parte de la funcionalidad de la característica. Los datos y la funcionalidad de registro de inventario de software están destinados al uso exclusivo del propietario y los administradores con licencia del software de servidor.

El registro de inventario de software puede ayudar a los administradores del servidor a realizar las siguientes tareas:

-   Recuperar información relativa al inventario de software y del servidor de Windows Server de forma remota y a petición.

-   Agregar información de inventario de software y servidor para una amplia variedad de escenarios de administración de activos de software mediante la habilitación de la característica registro de inventario de software de cada servidor y la elección de un URI de destino de servidor Web y la huella digital de certificado para la autenticación.

## <a name="see-also"></a>Consulta también
[Agregador de Registro de inventario de software](https://technet.microsoft.com/library/mt572043.aspx)<br>
[Administrar el Registro de inventario de software](manage-software-inventory-logging.md)<br>
[Cmdlets de registro de inventario de software en Windows PowerShell](https://technet.microsoft.com/library/dn283390.aspx)<br>
[Microsoft Assessment and Planning Toolkit](https://www.microsoft.com/download/en/details.aspx?id=7826)
[Herramienta de administración de activación por volumen](http://blogs.technet.com/b/volume-licensing/)

