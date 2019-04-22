---
title: Preparar la migración a MultiPoint Services
description: Describe la información que deben recopilarse antes de migrar a MultiPoint Services en Windows Server 2016
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3060c531-98a2-4957-a02c-be273f25f493
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 9650ebcae7e6207a226617d401d892049405901f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824386"
---
# <a name="prepare-to-migrate-to-multipoint-services-in-windows-server-2016"></a>Preparar la migración a MultiPoint Services en Windows Server 2016

>Se aplica a: Windows Server 2016

Use la siguiente información para recopilar la información que necesita para migrar el servicio de rol Servicios MultiPoint desde un servidor de origen que ejecuta una versión anterior de Windows Server 2016 a un servidor de destino que ejecuta Windows Server 2016 RTM.

Como mínimo, debe ser miembro del grupo Administradores en el servidor de origen y el servidor de destino para instalar, quitar o configurar MultiPoint Services.

>[!NOTE]
> Estos pasos no proporcionan instrucciones para migrar los datos guardan en la carpeta de usuario o las carpetas comparten. Asegúrese de que los usuarios de copia de seguridad de sus datos antes de comenzar la migración.

Usar MultiPoint Manager para recuperar la información necesaria para la migración. Necesitará permisos de administrador de servidor para usar el Administrador de MultiPoint.

Registre la configuración de servidor, el usuario y el entorno MultiPoint en el [hoja de cálculo de recopilación de datos de migración](multipoint-services-migration-worksheet.md). Use los pasos siguientes para recopilar esa información.

## <a name="multipoint-server-settings-for-the-local-server"></a>Configuración de multiPoint Server para el servidor local
1. Start MultiPoint Manager.
2. En el **inicio** , seleccione el servidor local y, a continuación, haga clic en **editar la configuración del servidor.**
3. Anote los valores en la hoja de cálculo de datos.
4. Cierre la ventana de configuración.

## <a name="managed-servers-and-computers"></a>Los equipos y los servidores administrados

Puede encontrar los nombres de los equipos y servidores administrados en el **inicio** ficha en MultiPoint Manager.

## <a name="station-settings"></a>Configuración de la estación
Si la orientación de pantalla o de inicio de sesión automático se configura para la estación, utilice los pasos siguientes para recuperar esa información. En caso contrario, puede omitir este paso.

Para recuperar la configuración de la estación:

1. Vaya a la **estaciones** ficha en MultiPoint Manager.
2. Buscar una estación con "Sí" en el **inicio de sesión automático** columna.
3. Seleccione esa estación y, a continuación, haga clic en **configurar estación**.
4. El usuario que se usa para el inicio de sesión automático del registro.

Para recuperar la configuración de la orientación de pantalla, ver el **configuración de la estación** para cada estación.

## <a name="list-of-users"></a>Lista de usuarios
1. Haga clic en el **usuarios** ficha en MultiPoint Manager.
2. Registro de la **administrador** y **usuario de MultiPoint Dashboard** accoutns.
3. Anote los usuarios estándares.

## <a name="vdi-template-location"></a>Ubicación de la plantilla VDI
 Si previamente habilitó la característica de plantilla VDI, debe registrar la ubicación de la plantilla VDI. Siempre y cuando los servidores de origen y destino están en la misma red, puede importar la plantilla mediante MultiPoint Manager.
 
## <a name="next-step"></a>Paso siguiente
Ahora está listo para [migrar a MultiPoint Services](multipoint-services-migration-steps.md) en la versión RTM de Windows Server 2016.