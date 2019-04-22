---
title: Información general de implementación de acceso inalámbrico
description: Este tema forma parte de la Guía de redes de Windows Server 2016 "Implementar mediante 802.1X basado en contraseña X Authenticated Wireless Access"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 29ae0f54-f045-465a-a08e-5867979345f2
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 6658c4750ba2f71b24acd4f7da02029da63179bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818926"
---
# <a name="wireless-access-deployment-overview"></a>Información general de implementación de acceso inalámbrico

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

La siguiente ilustración muestra los componentes que son necesarios para implementar 802.1X autentican el acceso inalámbrico con PEAP\-MS\-CHAP v2.  

![Información general de la infraestructura de implementación de 802.1X](../../../media/8021X-Deploy-Overview/8021X-Deploy-Overview.jpg)

## <a name="wireless-access-deployment-components"></a>Componentes de la implementación de acceso inalámbrico
La siguiente infraestructura es necesaria para esta implementación de acceso inalámbrico:

### <a name="8021x-capable-wireless-access-points"></a>802.1X\-compatibles con puntos de acceso inalámbrico
Después de los servicios de infraestructura de red requiere compatibilidad con la red de área local inalámbrica en su lugar, puede comenzar el proceso de diseño para la ubicación de la AP inalámbricos. El proceso de diseño de implementación de AP inalámbrico implica estos pasos:

- Identificar las áreas de cobertura para usuarios inalámbricos. Mientras que identifica las áreas de cobertura, asegúrese de identificar si desea proporcionar servicios inalámbricos fuera del edificio y si es así, determinar específicamente que son esas áreas externas.

- Determine cuántos puntos de acceso inalámbricos para implementar para garantizar una cobertura suficiente.

- Determinar dónde colocar puntos de acceso inalámbricos.

- Seleccione las frecuencias de canal para puntos de acceso inalámbricos.

### <a name="active-directory-domain-services"></a>Active Directory Domain Services
Los elementos siguientes de AD DS son necesarios para la implementación de acceso inalámbrico.

#### <a name="users-and-computers"></a>Los usuarios y equipos

Usar los usuarios de Active Directory y ajustar los equipos\-en para crear y administrar cuentas de usuario y para crear un grupo de seguridad inalámbrica que incluye cada miembro del dominio al que desea conceder acceso inalámbrico.

#### <a name="wireless-network-ieee-80211-policies"></a>Red inalámbrica \(IEEE 802.11\) directivas

Puede usar la red inalámbrica \(IEEE 802.11\) extensión de directivas de administración de directivas de grupo para configurar directivas que se aplican a los equipos inalámbricos cuando intenten tener acceso a la red.

En grupo Directiva de Editor de administración, cuando secundario\-haga clic en **red inalámbrica \(IEEE 802.11\) directivas**, tiene las dos opciones siguientes para el tipo de directiva inalámbrica que cree.

- **Crear una nueva directiva de red inalámbrica para Windows Vista y versiones posteriores**

- **Crear una nueva directiva de Windows XP**

>[!TIP]
>Al configurar una nueva directiva de red inalámbrica, tiene la opción para cambiar el nombre y la descripción de la directiva. Si cambia el nombre de la directiva, el cambio se refleja en el **detalles** panel del Editor de administración de directiva de grupo y en la barra de título del cuadro de diálogo de directiva de red inalámbrica. Independientemente de cómo cambiar el nombre las directivas, la nueva directiva inalámbrica de XP siempre se mostrarán en el Editor de administración del directiva de grupo con el **tipo** mostrar **XP**. Otras directivas se muestran con el **tipo** mostrando **Vista y versiones posteriores**.  

La directiva de red inalámbrica de Vista de Windows y versiones posteriores permite configurar, priorizar y administrar varios perfiles inalámbricos. Un perfil inalámbrico es una colección de conectividad y la configuración de seguridad que se usa para conectarse a una red inalámbrica específica. Cuando se actualiza la directiva de grupo en los equipos cliente inalámbricos, los perfiles creados en la directiva de red inalámbrica se agregan automáticamente a la configuración en los equipos cliente inalámbrico al que se aplica la directiva de red inalámbrica.

##### <a name="allowing-connections-to-multiple-wireless-networks"></a>Permitir conexiones a varias redes inalámbricas

Si tiene clientes inalámbricos que se mueven entre las ubicaciones físicas en su organización, como entre una oficina central y una sucursal, puede conectar los equipos a más de una red inalámbrica. En esta situación, puede configurar un perfil inalámbrico que contiene la configuración de conectividad y seguridad específica para cada red.

Por ejemplo, suponga que su empresa tiene una red inalámbrica de la oficina corporativa central, con un identificador \(SSID\) WlanCorp.

La sucursal tiene también una red inalámbrica a la que desea conectarse. La sucursal tiene el SSID configurado como WlanBranch.

En este escenario, puede configurar un perfil para cada red y los equipos u otros dispositivos que se usan ambas la oficina corporativa y sucursal puede conectarse a cualquiera de las redes inalámbricas cuando están físicamente en el intervalo del área de cobertura de la red.

##### <a name="mixed-mode-wireless-networks"></a>Mixto\-redes inalámbricas de modo

Como alternativa, se supone que la red tiene una mezcla de los equipos inalámbricos y los dispositivos compatibles con los estándares de seguridad diferente. Quizás algunos equipos más antiguos tienen adaptadores inalámbricos que sólo se pueden utilizar WPA\-Enterprise, mientras que los dispositivos más nuevos pueden usar el más seguro WPA2\-estándar de la empresa.

Puede crear dos perfiles diferentes que utilizan el mismo SSID y la configuración de conectividad y seguridad casi idéntica.

En un perfil, puede establecer la autenticación inalámbrica WPA2\-Enterprise con AES y del otro perfil puede especificar WPA\-Enterprise con TKIP.

Normalmente, esto se conoce como mixto\-implementación en modo y permite que los equipos de diferentes tipos y capacidades inalámbricas para compartir la misma red inalámbrica.

### <a name="network-policy-server-nps"></a>Servidor de directivas de red \(NPS\)
NPS permite crear y aplicar directivas de acceso de red para la autenticación de solicitudes de conexión y autorización.

Cuando se usa NPS como servidor RADIUS, configure los servidores de acceso de red, como puntos de acceso inalámbricos como clientes RADIUS en NPS. También configura las directivas de red que usa NPS para los clientes de acceso de autenticar y autorizar las solicitudes de conexión.  

### <a name="wireless-client-computers"></a>Equipos cliente inalámbricos
Con el propósito de esta guía, los equipos cliente inalámbricos son equipos y otros dispositivos que están equipados con adaptadores de red inalámbrica IEEE 802.11 y que ejecutan el cliente de Windows o sistemas operativos Windows Server.

#### <a name="server-computers-as-wireless-clients"></a>Equipos de servidor como los clientes inalámbricos

De forma predeterminada, la funcionalidad de 802.11 inalámbrica está deshabilitada en los equipos que ejecutan Windows Server.

Para habilitar la conectividad inalámbrica en equipos que ejecutan sistemas operativos de servidor, debe instalar y habilitar las redes LAN inalámbricas \(WLAN\) característica del servicio mediante el uso de Windows PowerShell o el agregar Roles y características de asistente en el servidor director.

Al instalar el **servicio WLAN** de características, el nuevo servicio **configuración automática de WLAN** está instalado en **servicios**. Cuando se complete la instalación, debe reiniciar el servidor.

Después de reiniciar el servidor, puede tener acceso a configuración automática de WLAN al hacer clic en **iniciar**, **herramientas administrativas de Windows**, y **servicios**.

Después de instalar y servidor reiniciar, el servicio está en estado detenido con un tipo de inicio de configuración automática de WLAN **automática**. Para iniciar el servicio, haga doble clic en **configuración automática de WLAN**. En el **General** , haga clic **iniciar**y, a continuación, haga clic en **Aceptar**.

El servicio de configuración automática de WLAN enumera los adaptadores inalámbricos y administra las conexiones inalámbricas y los perfiles inalámbricos que contienen valores que son necesarios para configurar el servidor para conectarse a una red inalámbrica.

Para obtener información general de implementación de acceso inalámbrico, consulte [proceso de implementación de acceso inalámbrico](c-wireless-access-deploy-process.md).
