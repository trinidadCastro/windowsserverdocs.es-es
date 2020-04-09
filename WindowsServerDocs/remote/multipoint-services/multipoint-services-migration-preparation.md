---
title: Preparar la migración a MultiPoint Services
description: Describe la información que se debe recopilar antes de migrar a multipoint Services en Windows Server 2016
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 3060c531-98a2-4957-a02c-be273f25f493
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 3333570aae34f2c102c36382eeffcb5411b7dd83
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858708"
---
# <a name="prepare-to-migrate-to-multipoint-services-in-windows-server-2016"></a>Preparar la migración a multipoint Services en Windows Server 2016

>Se aplica a: Windows Server 2016

Use la siguiente información para recopilar la información que necesita para migrar el servicio de rol Multipoint Services desde un servidor de origen que ejecuta una versión anterior de Windows Server 2016 a un servidor de destino que ejecuta Windows Server 2016 RTM.

Como mínimo, debe ser miembro del grupo administradores en el servidor de origen y el servidor de destino para instalar, quitar o configurar Multipoint Services.

>[!NOTE]
> Los pasos que se indican a continuación no proporcionan instrucciones para migrar datos guardados en carpetas de usuario o carpetas compartidas. Asegúrese de que los usuarios realicen una copia de seguridad de sus datos antes de comenzar la migración.

Use Multipoint Manager para recuperar la información necesaria para la migración. Necesitará permiso de administrador del servidor para usar Multipoint Manager.

Grabe la configuración de MultiPoint Server, el usuario y el entorno en la [hoja de cálculo de recopilación de datos de migración](multipoint-services-migration-worksheet.md). Use los pasos siguientes para recopilar esa información.

## <a name="multipoint-server-settings-for-the-local-server"></a>Configuración de MultiPoint Server para el servidor local
1. Inicie Multipoint Manager.
2. En la pestaña **Inicio** , seleccione el servidor local y, a continuación, haga clic en **Editar configuración del servidor.**
3. Registre la configuración en la hoja de cálculo de datos.
4. Cierre la ventana de configuración.

## <a name="managed-servers-and-computers"></a>Equipos y servidores administrados

Puede encontrar los nombres de los equipos y servidores administrados en la pestaña **Inicio** de Multipoint Manager.

## <a name="station-settings"></a>Configuración de la estación
Si el inicio de sesión automático o la orientación de la pantalla están configurados para la estación, siga estos pasos para recuperar esa información. De lo contrario, puede omitir este paso.

Para recuperar la configuración de la estación:

1. Vaya a la pestaña **estaciones** en Multipoint Manager.
2. Busque una estación que tenga "sí" en la columna de **Inicio de sesión automático** .
3. Seleccione esa estación y, a continuación, haga clic en **configurar estación**.
4. Registra el usuario que se usa para el inicio de sesión automático.

Para recuperar la configuración de la orientación de la pantalla, vea la configuración de la **estación** de cada estación.

## <a name="list-of-users"></a>Lista de usuarios
1. Haga clic en la pestaña **usuarios** de Multipoint Manager.
2. Grabe el **Administrador** y el **usuario de Multipoint Dashboard** accoutns.
3. Registre los usuarios estándar.

## <a name="vdi-template-location"></a>Ubicación de la plantilla VDI
 Si previamente ha habilitado la característica de plantilla de VDI, grabe la ubicación de la plantilla de VDI. Siempre que los servidores de origen y de destino se encuentren en la misma red, puede importar la plantilla mediante Multipoint Manager.
 
## <a name="next-step"></a>Paso siguiente
Ahora está listo para [migrar a multipoint Services](multipoint-services-migration-steps.md) en la versión RTM de Windows Server 2016.