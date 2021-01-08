---
title: Información general de implementación de acceso inalámbrico
description: Obtenga información acerca de los componentes de implementación de acceso inalámbrico necesarios para implementar el acceso inalámbrico autenticado mediante 802.1 X con PEAP-MS-CHAP v2.
manager: brianlic
ms.topic: article
ms.assetid: 29ae0f54-f045-465a-a08e-5867979345f2
author: eross-msft
ms.author: lizross
ms.date: 08/07/2020
ms.openlocfilehash: 2eb6bf9b3b51416c5ecc5cc49fb4da41b8fbb52a
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98038415"
---
# <a name="wireless-access-deployment-overview"></a>Información general de implementación de acceso inalámbrico

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En la ilustración siguiente se muestran los componentes necesarios para implementar el acceso inalámbrico autenticado mediante 802.1 X con PEAP \- MS \- CHAP v2.

![Introducción a la infraestructura de implementación de 802.1 x](../../../media/8021X-Deploy-Overview/8021X-Deploy-Overview.jpg)

## <a name="wireless-access-deployment-components"></a>Componentes de implementación de acceso inalámbrico
La siguiente infraestructura es necesaria para esta implementación de acceso inalámbrico:

### <a name="8021x-capable-wireless-access-points"></a>puntos de \- acceso inalámbricos compatibles con 802.1 x
Una vez que los servicios de infraestructura de red necesarios que admiten la red de área local inalámbrica estén en su lugar, puede comenzar el proceso de diseño de la ubicación de los AP inalámbricos. El proceso de diseño de la implementación de AP inalámbricos implica estos pasos:

- Identifique las áreas de cobertura para los usuarios inalámbricos. A la vez que se identifican las áreas de cobertura, asegúrese de identificar si desea proporcionar el servicio inalámbrico fuera del edificio y, si es así, determine en concreto dónde se encuentran esas áreas externas.

- Determine el número de AP inalámbricos que se van a implementar para garantizar una cobertura adecuada.

- Determine dónde colocar AP inalámbricos.

- Seleccione las frecuencias de canal para los AP inalámbricos.

### <a name="active-directory-domain-services"></a>Active Directory Domain Services
Los siguientes elementos de AD DS son necesarios para la implementación de acceso inalámbrico.

#### <a name="users-and-computers"></a>Usuarios y equipos

Use el complemento usuarios y equipos \- de Active Directory para crear y administrar cuentas de usuario, así como para crear un grupo de seguridad inalámbrica que incluya cada miembro del dominio al que desea conceder acceso inalámbrico.

#### <a name="wireless-network-ieee-80211-policies"></a>\(Directivas IEEE 802,11 de red inalámbrica \)

Puede usar la \( \) extensión de directivas IEEE 802,11 de red inalámbrica de administración de directiva de grupo para configurar las directivas que se aplican a los equipos inalámbricos cuando intentan acceder a la red.

En Editor de administración de directivas de grupo, al hacer clic con el botón secundario \- en **\( \) directivas IEEE 802,11 de red inalámbrica**, tiene las dos opciones siguientes para el tipo de directiva inalámbrica que cree.

- **Crear una nueva Directiva de red inalámbrica para Windows Vista y versiones posteriores**

- **Crear una nueva Directiva de Windows XP**

>[!TIP]
>Al configurar una nueva Directiva de red inalámbrica, tiene la opción de cambiar el nombre y la descripción de la Directiva. Si cambia el nombre de la Directiva, el cambio se refleja en el panel de **detalles** de editor de administración de directivas de grupo y en la barra de título del cuadro de diálogo Directiva de red inalámbrica. Independientemente de cómo cambie el nombre de las directivas, la nueva Directiva de la red inalámbrica XP se mostrará siempre en Editor de administración de directivas de grupo con el **tipo** que muestra **XP**. Se muestran otras directivas con el **tipo** que muestra **vista y versiones posteriores**.

La Directiva de red inalámbrica de Windows Vista y versiones posteriores permite configurar, priorizar y administrar varios perfiles inalámbricos. Un perfil inalámbrico es una colección de opciones de conectividad y seguridad que se usan para conectarse a una red inalámbrica específica. Cuando directiva de grupo se actualiza en los equipos cliente inalámbricos, los perfiles creados en la Directiva de red inalámbrica se agregan automáticamente a la configuración de los equipos cliente inalámbricos a los que se aplica la Directiva de red inalámbrica.

##### <a name="allowing-connections-to-multiple-wireless-networks"></a>Permitir conexiones a varias redes inalámbricas

Si tiene clientes inalámbricos que se mueven entre ubicaciones físicas de la organización, por ejemplo, entre una oficina principal y una sucursal, puede que desee que los equipos se conecten a más de una red inalámbrica. En esta situación, puede configurar un perfil inalámbrico que contenga la configuración de seguridad y conectividad específica de cada red.

Por ejemplo, suponga que su empresa tiene una red inalámbrica para la oficina central principal, con un identificador de conjunto de servicios \( SSID \) WlanCorp.

La sucursal también tiene una red inalámbrica a la que también desea conectarse. La sucursal tiene el SSID configurado como WlanBranch.

En este escenario, puede configurar un perfil para cada red, y los equipos u otros dispositivos que se usan tanto en la oficina corporativa como en la sucursal pueden conectarse a cualquiera de las redes inalámbricas cuando están físicamente dentro del alcance del área de cobertura de una red.

##### <a name="mixed-mode-wireless-networks"></a>\-Redes inalámbricas en modo mixto

Como alternativa, supongamos que la red tiene una mezcla de equipos inalámbricos y dispositivos que admiten diferentes normas de seguridad. Quizás algunos equipos más antiguos tienen adaptadores inalámbricos que solo pueden usar WPA \- Enterprise, mientras que los dispositivos más recientes pueden usar el estándar WPA2 Enterprise más seguro \- .

Puede crear dos perfiles distintos que usen el mismo SSID y una configuración de seguridad y conectividad prácticamente idéntica.

En un perfil, puede establecer la autenticación inalámbrica en WPA2 \- Enterprise con AES y, en el otro perfil, puede especificar WPA \- Enterprise con TKIP.

Esto se conoce normalmente como una \- implementación en modo mixto y permite que los equipos de diferentes tipos y capacidades inalámbricas compartan la misma red inalámbrica.

### <a name="network-policy-server-nps"></a>NPS del servidor de directivas de redes \(\)
NPS le permite crear y aplicar directivas de acceso a la red para la autenticación y autorización de solicitudes de conexión.

Cuando se usa NPS como un servidor RADIUS, se configuran los servidores de acceso a la red, como los puntos de acceso inalámbrico, como clientes RADIUS en NPS. También se configuran las directivas de red que usa NPS para autenticar a los clientes de acceso y autorizar las solicitudes de conexión.

### <a name="wireless-client-computers"></a>Equipos cliente inalámbricos
En esta guía, los equipos cliente inalámbricos son equipos y otros dispositivos equipados con adaptadores de red inalámbrica IEEE 802,11 y que ejecutan sistemas operativos cliente de Windows o Windows Server.

#### <a name="server-computers-as-wireless-clients"></a>Equipos servidor como clientes inalámbricos

De forma predeterminada, la funcionalidad de la red inalámbrica 802,11 está deshabilitada en los equipos que ejecutan Windows Server.

Para habilitar la conectividad inalámbrica en equipos que ejecutan sistemas operativos de servidor, debe instalar y habilitar la característica de servicio WLAN de LAN inalámbrica mediante \( \) Windows PowerShell o el Asistente para agregar roles y características en Administrador del servidor.

Al instalar la característica de **servicio de LAN inalámbrica** , la **configuración automática** del nuevo servicio WLAN se instala en los **servicios**. Una vez completada la instalación, debe reiniciar el servidor.

Una vez reiniciado el servidor, puede tener acceso a la configuración automática de WLAN al hacer clic en **Inicio**, **herramientas administrativas de Windows** y **servicios**.

Después de instalar y reiniciar el servidor, el servicio de configuración automática de WLAN se encuentra en estado detenido con un tipo de inicio **automático**. Para iniciar el servicio, haga doble clic en **configuración automática de WLAN**. En la pestaña **General** , haga clic en **Inicio** y, a continuación, haga clic en **Aceptar**.

El servicio de configuración automática de WLAN enumera los adaptadores inalámbricos y administra las conexiones inalámbricas y los perfiles inalámbricos que contienen la configuración necesaria para configurar el servidor para conectarse a una red inalámbrica.

Para obtener información general sobre la implementación de acceso inalámbrico, consulte [proceso de implementación del acceso inalámbrico](c-wireless-access-deploy-process.md).
