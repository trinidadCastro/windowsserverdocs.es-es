---
title: Hoja de cálculo de planeación para la migración de Multipoint Services
description: Proporciona hojas de cálculo de planeación para ayudarle a migrar a multipoint Services en Windows Server 2016
ms.date: 07/29/2016
ms.topic: article
ms.assetid: 864405bb-47ed-4c83-97a2-8df4c6e6f96b
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 653725776d21a0df0550fb754d207a2de7791491
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946768"
---
# <a name="planning-worksheet-for-multipoint-services-migration"></a>Hoja de cálculo de planeación para la migración de Multipoint Services

>Se aplica a: Windows Server 2016

Use las siguientes listas y tablas para recopilar la configuración que necesita durante la migración de Multipoint Services.

## <a name="source-server-settings"></a>Configuración del servidor de origen

Puede encontrar la configuración del servidor en la pestaña **Inicio** de Multipoint Manager. Coloque una marca de verificación junto a cada opción de configuración en uso en el servidor de origen.

- Permita que una cuenta tenga varias sesiones.
- Permite que este equipo se administre de forma remota.
- Permite la supervisión de los escritorios de este equipo.
- Iniciar siempre en modo de consola.
- No mostrar la notificación de privacidad en el primer inicio de sesión de usuario.
- Asigne una dirección IP única a cada estación.
- Permitir la mensajería instantánea entre el panel multipoint y las sesiones de usuario en este equipo.
- Permitir orquestación de las sesiones de usuario de administrador y Multipoint Dashboard.
- Permita que las estaciones usen la representación de hardware de GPU.

## <a name="managed-servers-and-computers"></a>Equipos y servidores administrados

Registre los nombres de los equipos y servidores administrados. Puede encontrar esta información en la pestaña **Inicio** de Multipoint Manager.

| Computer | Nombre del equipo |
|----------|---------------|
| 1        |               |
| 2        |               |
| 3        |               |
| 4        |               |
| 5        |               |
| 6        |               |
| 7        |               |
| 8        |               |
| 9        |               |
| 10       |               |


## <a name="stations"></a>Cadenas

Grabe las estaciones locales y su configuración. Puede encontrar esta información en la pestaña **estaciones** en Multipoint Manager.

| #  | El nombre de la estación | Cuenta de usuario de inicio de sesión automático | Orientación de la pantalla |
|----|--------------|-------------------------|---------------------|
| 1  |              |                         |                     |
| 2  |              |                         |                     |
| 3  |              |                         |                     |
| 4  |              |                         |                     |
| 5  |              |                         |                     |
| 6  |              |                         |                     |
| 7  |              |                         |                     |
| 8  |              |                         |                     |
| 9  |              |                         |                     |
| 10 |              |                         |                     |

## <a name="administrators-and-multipoint-dashboard-users"></a>Administradores y usuarios de Multipoint Dashboard

Copie los nombres de usuario para los administradores y los usuarios de Multipoint Dashboard. Puede encontrar esta información en la pestaña **usuarios** de Multipoint Manager.

Administradores:

- Nombre de usuario:
- Nombre de usuario:
- Nombre de usuario:
- Nombre de usuario:
- Nombre de usuario:
- Nombre de usuario:

Usuarios del panel:

- Nombre de usuario:
- Nombre de usuario:
- Nombre de usuario:
- Nombre de usuario:
- Nombre de usuario:

## <a name="vdi-template-and-virtual-desktops"></a>Plantilla VDI y escritorios virtuales

Grabe la información de la plantilla de VDI y los nombres de los escritorios virtuales en la implementación de Multipoint Services. Puede encontrar esta información en la pestaña **escritorios virtuales** de Multipoint Manager.

**Ubicación de la plantilla VDI**:

| # | Nombre del escritorio virtual      |
|---|---------------------------|
| 1 |                           |
| 2 |                           |
| 3 |                           |
| 4 |                           |
| 5 |                           |