---
title: Información general de implementación de acceso inalámbrico
description: Este tema es parte de la Guía de redes de Windows Server 2016 "Implementar basada en contraseña 802.1X X autenticados Wireless Access"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 29ae0f54-f045-465a-a08e-5867979345f2
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 11d87254d8819606a06acedd407bb2e09a45cb60
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment-overview"></a>Información general de implementación de acceso inalámbrico

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

La siguiente ilustración muestra los componentes necesarios para implementar 802.1X autentican acceso inalámbrico con PEAP\-MS\-CHAPv2.  

![Descripción de la infraestructura de implementación de 802.1X](../../../media/8021X-Deploy-Overview/8021X-Deploy-Overview.jpg)

## <a name="wireless-access-deployment-components"></a>Componentes de la implementación de acceso inalámbrico
La infraestructura siguiente es necesaria para esta implementación de acceso inalámbrico:

### <a name="8021x-capable-wireless-access-points"></a>802.1X\-puntos de acceso inalámbrico compatible con
Después de los servicios de infraestructura de red necesarios dar soporte a tu red de área local en su lugar, puedes empezar el proceso de diseño para la ubicación de los puntos de acceso inalámbricos. El proceso de diseño de implementación de punto de acceso inalámbrico conlleva los siguientes pasos:

- Identifica las áreas de la cobertura de los usuarios inalámbricos. Mientras identifica las áreas de la cobertura, asegúrate de identificar si desea proporcionar servicios inalámbricos fuera de la compilación y si es así, determinar específicamente dónde están las áreas externas.

- Determinar cuántos puntos de acceso inalámbricos para implementar para garantizar la cobertura adecuada.

- Determinar dónde colocar los puntos de acceso inalámbricos.

- Seleccionar las frecuencias de canal para puntos de acceso inalámbricos.

### <a name="active-directory-domain-services"></a>Servicios de dominio de Active Directory
Los siguientes elementos de AD DS son necesarios para la implementación de acceso inalámbrico.

#### <a name="users-and-computers"></a>Los usuarios y equipos

Usa los usuarios de Active Directory y equipos en snap\ para crear y administrar cuentas de usuario y para crear un grupo de seguridad inalámbrica que incluya a cada miembro de dominio al que quieres conceder acceso inalámbrico.

#### <a name="wireless-network-ieee-80211-policies"></a>Red inalámbrica \ (IEEE 802.11\) directivas

Puedes usar la red inalámbrica \ (IEEE 802.11\) extensión de directivas de administración de directivas de grupo para configurar las directivas que se aplican a los equipos inalámbricos cuando intentan acceder a la red.

En el Editor de administración de directivas de grupo, cuando right\ clic **red inalámbrica \ (IEEE 802.11\) directivas**, tienes las siguientes dos opciones para el tipo de directiva inalámbrica que crees.

- **Crear una nueva directiva de red inalámbrica para Windows Vista y versiones posteriores**

- **Crear una nueva directiva de Windows XP**

>[!TIP]
>Cuando configures una nueva directiva de red inalámbrica, tienes la opción para cambiar el nombre y la descripción de la directiva. Si cambias el nombre de la directiva, el cambio se reflejará en la **detalles** panel del Editor de administración de directiva de grupo y en la barra de título del cuadro de diálogo de la directiva de red inalámbrica. Independientemente de cómo cambiar el nombre las directivas, la nueva directiva de conexión inalámbrica de XP mostrarán siempre en el Editor de administración del directiva de grupo con la **tipo** mostrar **XP**. Otras directivas se enumeran junto con la **tipo** mostrar **Vista y versiones posteriores**.  

El directivas de red inalámbrica para Windows Vista y versiones posteriores permite configurar, priorizar y administrar varios perfiles inalámbricos. Un perfil inalámbrico es una colección de la conectividad y la configuración de seguridad que se usa para conectarse a una red inalámbrica específica. Cuando se actualiza la directiva de grupo en los equipos cliente inalámbrico, los perfiles que crees en la directiva de red inalámbrica se agregan automáticamente a la configuración en los equipos cliente inalámbrico al que se aplica la directiva de red inalámbrica.

##### <a name="allowing-connections-to-multiple-wireless-networks"></a>Permitir las conexiones a redes inalámbricas varios

Si tienes que los clientes inalámbricos que se mueven en ubicaciones físicas de la organización, como entre una oficina principal y una sucursal, es recomendable equipos para conectarte a más de una red inalámbrica. En esta situación, puede configurar un perfil inalámbrico que contiene la configuración de seguridad y conectividad específica para cada red.

Por ejemplo, supongamos que tu empresa tiene una red inalámbrica en la oficina corporativa principal, con un identificador \(SSID\) WlanCorp.

Tu sucursal también tiene una red inalámbrica a la que desea conectarse también. La sucursal tiene el SSID configurado como WlanBranch.

En este escenario, puedes configurar un perfil de cada red y equipos u otros dispositivos que se usan en ambos en la oficina corporativa y sucursal puede conectar a cualquiera de las redes inalámbricas cuando físicamente estén dentro del alcance de la zona de cobertura de una red.

##### <a name="mixed-mode-wireless-networks"></a>Redes inalámbricas en modo de Mixed\

Como alternativa, supongamos que la red tiene una mezcla de equipos inalámbricos y los dispositivos que admitan los estándares de seguridad diferente. Quizás algunos equipos antiguos tienen adaptadores inalámbricos que solo se pueden usar WPA\ de empresa, mientras más reciente dispositivos pueden utilizar el estándar de WPA2\ Enterprise más sólido.

Puedes crear dos perfiles diferentes que usan el mismo SSID y la configuración de seguridad y conectividad casi idénticas.

En un perfil, puedes establecer la autenticación inalámbrica en la empresa WPA2\ con AES y, en el otro perfil puede especificar WPA\ Enterprise con TKIP.

Esto se conoce como una implementación de modo mixed\ y permite que los equipos de distintos tipos y funciones inalámbricas para compartir la misma red inalámbrica.

### <a name="network-policy-server-nps"></a>El servidor de directivas de red \(NPS\)
NPS te permite crear y aplicar directivas de acceso de red para la autorización y autenticación de solicitud de conexión.

Cuando se usa NPS como un servidor RADIUS, configurar los servidores de acceso de red, como puntos de acceso inalámbrico, como los clientes RADIUS en NPS. También configurar las directivas de red que usa NPS para los clientes de acceso de autenticar y autorizar a sus solicitudes de conexión.  

### <a name="wireless-client-computers"></a>Equipos cliente inalámbrica
Con el fin de esta guía, los equipos cliente inalámbricos son equipos y otros dispositivos que están equipados con adaptadores de red inalámbrica IEEE 802.11 y que estén ejecutando el cliente de Windows o sistemas operativos Windows Server.

#### <a name="server-computers-as-wireless-clients"></a>Equipos de servidor como los clientes inalámbricos

De manera predeterminada, la funcionalidad de 802.11 inalámbrica está deshabilitada en los equipos que ejecutan Windows Server.

Para habilitar la conectividad inalámbrica en equipos que ejecutan sistemas operativos de servidor, debes instalar y habilitar la \(WLAN\) LAN inalámbrica característica servicio mediante el uso de Windows PowerShell o la agregar Roles y características de asistente en el administrador del servidor.

Cuando instalas la **servicio WLAN** de características, el nuevo servicio **configuración automática de WLAN** está instalado en **servicios**. Una vez finalizada la instalación, debes reiniciar el servidor.

Después de reinicia el servidor, puede tener acceso a configuración automática de WLAN al hacer clic en **inicio**, **herramientas administrativas de Windows**, y **servicios**.

Después de instalar y server reiniciar, servicio está en un estado de parada con un tipo de inicio de la configuración automática de WLAN **automática**. Para iniciar el servicio, haga doble clic en **configuración automática de WLAN**. En la **General** , haga clic **inicio**y, a continuación, haz clic en **Aceptar**.

El servicio de configuración automática de WLAN enumera los adaptadores inalámbricos y administra las conexiones inalámbricas y los perfiles inalámbricos que contienen valores que son necesarios para configurar el servidor para conectarse a una red inalámbrica.

Para obtener información general de la implementación de acceso inalámbrico, consulta [proceso de implementación de acceso inalámbrico](c-wireless-access-deploy-process.md).
