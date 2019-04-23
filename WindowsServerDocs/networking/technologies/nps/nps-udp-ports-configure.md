---
title: Configurar la información del puerto UDP de NPS
description: Puede utilizar este tema para configurar los puertos que usa el servidor de directivas de redes (NPS) para la autenticación de servicio de autenticación remota telefónica de usuario (RADIUS) y el tráfico de cuentas en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 44c20092180c47e97f1505271203f4491606bbcf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851976"
---
# <a name="configure-nps-udp-port-information"></a>Configurar la información del puerto UDP de NPS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar el procedimiento siguiente para configurar los puertos que usa el servidor de directivas de redes (NPS) para Remote Authentication Dial-in User Service \(RADIUS\) autenticación y el tráfico de cuentas.

De forma predeterminada, NPS escucha el tráfico RADIUS en los puertos 1812, 1813, 1645 y 1646 para el protocolo de Internet versión 6 \(IPv6\) e IPv4 para todos los adaptadores de red instalados.

>[!NOTE]
>Si desinstala IPv4 o IPv6 en un adaptador de red, NPS no supervisará el tráfico RADIUS para el protocolo desinstalado.

Los valores de puerto de 1812 para autenticación y 1813 para cuentas son puertos estándar RADIUS definidos por la Internet Engineering Task Force \(IETF\) en RFC 2865 y 2866. Sin embargo, de forma predeterminada, muchos de los servidores de acceso usan puertos 1645 para las solicitudes de autenticación y 1646 para las solicitudes de cuentas. Con independencia de qué números de puerto que decida utilizar, asegúrese de que NPS y el servidor de acceso están configurados para usar los mismos.

>[IMPORTANTE] Si no usa los números de puerto RADIUS predeterminados, debe configurar las excepciones del firewall para el equipo local permitir el tráfico RADIUS en los nuevos puertos. Para obtener más información, consulte [configurar Firewalls para el tráfico RADIUS](nps-firewalls-configure.md).

Para completar este procedimiento, se requiere como mínimo la pertenencia a **Admins. del dominio** o equivalente.

## <a name="to-configure-nps-udp-port-information"></a>Para configurar la información del puerto UDP de NPS 

1. Abra la consola NPS.
2. Haga clic en **servidor de directivas de red**y, a continuación, haga clic en **propiedades**.
3. Haga clic en el **puertos** ficha y, a continuación, examine la configuración de puertos. Si la autenticación RADIUS y los puertos UDP de contabilidad de RADIUS son distintos de los valores predeterminados proporcionados (1812 y 1645 para autenticación y 1813 y 1646 para cuentas), escriba la configuración del puerto en **autenticación** y  **Contabilidad**.
4. Para utilizar varias configuraciones de puerto para la autenticación o las solicitudes de cuentas, separe los números de puerto con comas.

Para obtener más información sobre la administración de NPS, consulte [administrar un servidor de directivas de redes](nps-manage-top.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
