---
title: Recopilación de datos de usuario del servidor de directivas de redes
description: Información que se usa para ayudar a los usuarios a autenticar a los usuarios mediante el servidor de directivas de redes en Windows Server 2016.
author: MicrosoftGuyJFlo
manager: mtillman
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.topic: article
ms.date: 05/01/2018
ms.openlocfilehash: a47cb5791b6e157b434d80535d10b107159db48f
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87994596"
---
# <a name="network-policy-server-user-data-collection"></a>Recopilación de datos de usuario del servidor de directivas de redes

En este documento se explica cómo buscar información de usuario recopilada por el servidor de directivas de redes (NPS) en el evento que desea quitar.

>[!Note]
>Si le interesa ver o eliminar datos personales, consulte la guía de Microsoft en las solicitudes de los [asuntos de datos de Windows para el sitio de RGPD](/microsoft-365/compliance/gdpr-dsr-windows) . Si está buscando información general sobre RGPD, consulte la [sección RGPD del portal de confianza de servicios](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

## <a name="information-collected-by-nps"></a>Información recopilada por NPS

- Timestamp
- Marca de tiempo del evento
- Nombre de usuario
- Nombre de usuario completo
- Dirección IP de cliente
- Proveedor del cliente
- Nombre descriptivo del cliente
- Tipo de autenticación
- Muchos otros campos relacionados con el protocolo RADIUS

## <a name="gather-data-from-nps"></a>Recopilación de datos de NPS

Si los datos de cuentas están habilitados y configurados, los registros de los intentos de autenticación de NPS de un usuario se pueden obtener de SQL Server o de los archivos de registro en función de la configuración.

Si los datos de cuentas están configurados para SQL Server, consulte todos los registros donde User_Name = `'<username>'` .

Si se configuran los datos de cuentas para un archivo de registro, busque en el archivo de registro para `<username>` Buscar todas las entradas de registro.

Las entradas del registro de eventos de servicios de acceso y directivas de redes se consideran duplicadas en los datos de cuentas y no es necesario recopilarlas.

Si los datos de cuentas no están habilitados, los registros de los intentos de autenticación de NPS de un usuario se pueden obtener del registro de eventos de servicios de acceso y directivas de redes mediante la búsqueda de `<username>` .