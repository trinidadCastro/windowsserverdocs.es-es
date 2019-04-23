---
title: Hoja de cálculo de planeación para la migración de MultiPoint Services
description: Proporciona las hojas de cálculo de planeación para ayudarle a migrar a MultiPoint Services en Windows Server 2016
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 864405bb-47ed-4c83-97a2-8df4c6e6f96b
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: a9d9b62bced9be90c658b79338c6f4ef07710fc3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880586"
---
# <a name="planning-worksheet-for-multipoint-services-migration"></a>Hoja de cálculo de planeación para la migración de MultiPoint Services

>Se aplica a: Windows Server 2016

Use las siguientes listas y tablas para recopilar a la configuración que se necesita durante la migración de MultiPoint Services.

## <a name="source-server-settings"></a>Configuración del servidor de origen

Puede encontrar la configuración del servidor en el **inicio** ficha en MultiPoint Manager. Coloque una marca de verificación junto a cada valor en uso en el servidor de origen.

- Permitir que una cuenta para tener varias sesiones.
- Permitir que este equipo se administre de forma remota.
- Permitir supervisión de escritorios de este equipo.
- Siempre se inicie en modo de consola.
- No mostrar la notificación de privacidad en el primer inicio de sesión de usuario.
- Asignar una dirección IP única a cada estación.
- Permitir mensajería instantánea entre el panel de MultiPoint y sesiones de usuario en este equipo.
- Permitir la orquestación de administrador y de las sesiones de usuario de MultiPoint Dashboard.
- Permitir estaciones usar la representación de hardware GPU.

## <a name="managed-servers-and-computers"></a>Los equipos y los servidores administrados

Registre los nombres de los equipos y servidores administrados. Puede encontrar esta información en el **inicio** ficha en MultiPoint Manager.

| Computer | Nombre de equipo |
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


## <a name="stations"></a>Estaciones

Registre las estaciones locales y su configuración. Puede encontrar esta información en el **estaciones** ficha en MultiPoint Manager.

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

## <a name="administrators-and-multipoint-dashboard-users"></a>Los administradores y usuarios de MultiPoint Dashboard

Copie los nombres de usuario para los administradores y usuarios de MultiPoint Dashboard. Puede encontrar esta información en el **usuarios** ficha en MultiPoint Manager.

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

Registre la información de la plantilla VDI y los nombres de los escritorios virtuales en su implementación de MultiPoint Services. Puede encontrar esta información en el **escritorios virtuales** ficha en MultiPoint Manager.

**Ubicación de la plantilla VDI**: 

| # | Nombre del escritorio virtual      |
|---|---------------------------|
| 1 |                           |
| 2 |                           |
| 3 |                           |
| 4 |                           |
| 5 |                           |