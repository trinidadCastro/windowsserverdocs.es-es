---
title: Información general de impresión en la nube híbrida de Windows Server
description: La impresión en la nube híbrida permite a los profesionales de ti admitir los requisitos de impresión de BYOD o dispositivos Unidos a un dominio.
ms.topic: conceptual
author: trudyha
ms.author: trudyha
ms.date: 10/16/2017
ms.openlocfilehash: fc76aef0f7fbc9f3c1dd73b94c6510c0ad37034c
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87879414"
---
# <a name="windows-server-hybrid-cloud-print-overview"></a>Información general de impresión en la nube híbrida de Windows Server

**Se aplica a**
-   Windows Server 2016

## <a name="what-is-hybrid-cloud-print"></a>¿Qué es la impresión en la nube híbrida?
**La impresión en nube híbrida** es una nueva característica de Windows Server 2016 disponible a través **de las características a petición**. Permite a los profesionales de ti admitir los requisitos de impresión de las personas que traen sus propios dispositivos, o usar dispositivos Unidos a su Azure Active Directory. Esto incluye dispositivos móviles como Windows Phone, portátiles o tabletas que ejecutan Windows 10 o Windows Mobile. Proporciona compatibilidad con la impresión de cualquier persona que tenga acceso a Internet.

Para los administradores de ti, **la impresión en la nube híbrida** proporciona acceso de usuario seguro a las impresoras locales mediante la autenticación multifactor de Azure para validar el acceso de los usuarios. La funcionalidad de inicio de sesión único (SSO) simplifica la experiencia del usuario. **La impresión en la nube híbrida** se basa en el rol del **servidor de impresión** de Windows, lo que permite a los profesionales de ti una experiencia similar a la administración de impresoras y seguridad de acceso de usuarios.

La impresión en la **nube híbrida** permite que los usuarios de su organización impriman desde los dispositivos que usan para completar su trabajo, incluso cuando están fuera de su escritorio o lugar de trabajo.

**La impresión de nube híbrida** es compatible con Windows 10 Creators Update y Windows 10 S.

## <a name="feature-summary"></a>Resumen de características
**La impresión en la nube híbrida** consta de dos componentes principales del servidor: servicio de **detección** y servicio de **impresión de Windows** .
- Punto de conexión de servicio de **detección** que se ejecuta en un servicio IIS compatible con el estándar del sector de Mopria Alliance para la detección de impresoras en la nube.
- Extremo del servicio de **impresión de Windows** que se ejecuta en un servicio IIS compatible con el protocolo de impresión en Internet estándar del sector (IPP) para garantizar la mayor compatibilidad con el sistema operativo cliente.

## <a name="deployment"></a>Implementación
**La impresión en la nube híbrida** admite un par de opciones de implementación diferentes, en función de dónde requiera la organización la autenticación de los usuarios. Este es el aspecto que podría tener una implementación:

![Diagrama que muestra una representación gráfica de la solución de impresión en la nube híbrida](../media/hybrid-cloud-print/wshcp-deployment-options.png)

*Diagrama de la solución de impresión en la nube híbrida*

En el diagrama se muestra:
- **Impresión en la nube híbrida** mediante Azure Active Directory como proveedor de identidades de usuario.
- El servicio de **impresión de Windows** y los puntos de conexión de servicio de **detección** se registran con Azure Active Directory para permitir que el dispositivo cliente recupere el token de autenticación de usuario necesario para utilizarlo en estos servicios.
- Un servicio MDM, como **Microsoft Intune**, aprovisiona el dispositivo cliente con las directivas necesarias para conectarse Azure Active Directory al servicio de **impresión de Windows** y al servicio de **detección** .

Esta tabla contiene más información sobre los elementos del diagrama.

| Elemento | Descripción |
| ------- | ----------- |
| Azure Active Directory  | Proporciona y controla la funcionalidad de identidad y autorización del usuario |
| Grafo de        | Proporciona y controla la funcionalidad de identidad y autorización del usuario |
| Azure AD Connect  | Sincroniza las credenciales de usuario entre Azure AD y AD local. |
| Servicio MDM (Intune) | Proporciona la funcionalidad de aprovisionamiento de directivas de dispositivo para garantizar que el dispositivo cliente (dispositivo BYOD) cumple las directivas corporativas. |
| Proxy de Azure AD | Proporciona una conexión de larga duración que se establece desde detrás del Firewall a Azure para permitir que el tráfico de aplicaciones configurado específico fluya desde Internet a la red corporativa. |
| Aplicación web de Azure | La base de la solución de impresión en la nube híbrida. Proporciona los puntos de conexión web necesarios para detectar impresoras y enviar contenido de impresión para dispositivos que no están Unidos a un dominio. |
| Dispositivo BYOD/cola de impresión/Impresora del servidor de impresión de Windows | Son tal cual. No hay ningún cambio en la funcionalidad de la implementación. |

Hay dos maneras de instalar **la impresión híbrida en la nube**:
- * * Características a petición, que ven [configurar características a petición en Windows Server](https://docs.microsoft.com/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server) para obtener más información sobre cómo agregar y quitar archivos de roles y características.
- * * Configuración de Windows Server 2016, que los administradores pueden ir a **configuración**  ->  **aplicaciones**  ->  **administrar características opcionales**  ->  **Agregar una característica** y buscar el paquete de características a petición
- Comandos de PowerShell: en una ventana de administrador de PowerShell, ejecute estos comandos:

```PowerShell
    Install-Module -Name PublishCloudPrinter
    Import-Module PublishCloudPrinter
```
