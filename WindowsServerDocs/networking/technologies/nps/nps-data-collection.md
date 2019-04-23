---
title: Recopilación de datos de usuario de servidor de directivas de red
description: ¿Qué información se usa para autenticar a los usuarios por servidor de directivas de redes en Windows Server 2016.
author: MicrosoftGuyJFlo
manager: mtillman
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.date: 05/01/2018
ms.openlocfilehash: 5bddd22c9c2f954435cc6ce37347d18c76ee7de3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888746"
---
# <a name="network-policy-server-user-data-collection"></a>Recopilación de datos de usuario de servidor de directivas de red

Este documento explica cómo encontrar información de usuario recopilada por el servidor de directivas de redes (NPS) en caso de que desea quitarla.

>[!Note]
>Si está interesado en ver o eliminar datos personales, revise las instrucciones de Microsoft en la [asunto solicitudes de datos de Windows para el RGPD](https://docs.microsoft.com/microsoft-365/compliance/gdpr-dsr-windows) sitio. Si desea obtener información general sobre RGPD, consulte el [sección RGPD del portal de confianza del servicio](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

## <a name="information-collected-by-nps"></a>Información recopilada por NPS

- Timestamp
- Marca de tiempo del evento
- Nombre de usuario
- Nombre de usuario completo completa
- Dirección IP del cliente
- Proveedor de cliente
- Nombre descriptivo del cliente
- Tipo de autenticación
- Muchos otros campos relacionados con el protocolo RADIUS

## <a name="gather-data-from-nps"></a>Recopilar datos de NPS

Si los datos de cuentas está habilitados y configurados, se pueden obtener los registros de intentos de autenticación de NPS de un usuario de SQL Server o los archivos de registro según la configuración. 

Si los datos de cuentas está configurados para SQL Server, consulta para todos los registros donde User_Name = `'<username>'`.

Si los datos de cuentas está configurados para un archivo de registro, a continuación, busque el archivo de registro para el `<username>` para buscar todas las entradas de registro.

Entradas de registro de eventos de servicios de acceso y directivas de redes se consideran duplicadas para los datos de cuentas y no es necesario que se recopilen.

Si los datos de cuentas no está habilitados, los registros de intentos de autenticación de NPS de un usuario pueden obtenerse desde el registro de eventos de servicios de acceso y directivas de redes mediante la búsqueda de la `<username>`.
