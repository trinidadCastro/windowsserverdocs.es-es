---
title: Implementación de AD FS entre geográficas de alta disponibilidad en Azure con Azure Traffic Manager | Microsoft Docs
description: En este documento, aprenderá a implementar AD FS en Azure para lograr una alta disponibilidad.
keywords: AD FS con Azure Traffic Manager, ADFS con Azure Traffic Manager, geográfico, varios centros de bits, centros de recursos geográficos, centros de recursos multigeográfico, implementación de AD FS en Azure, implementación de Azure ADFS, Azure ADFS, Azure AD FS, implementación de ADFS, implementación de AD FS, ADFS en Azure implementar ADFS en Azure, implementar AD FS en Azure, ADFS Azure, introducción a AD FS, Azure, AD FS en Azure, IaaS, ADFS, movimiento de ADFS a Azure
services: active-directory
documentationcenter: ''
author: anandyadavmsft
manager: mtillman
editor: ''
ms.assetid: a14bc870-9fad-45ed-acd5-a90ccd432e54
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: anandy;billmath
ms.openlocfilehash: d98eb126513d707bce7abe3e901c8bf584d2319c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868016"
---
# <a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Implementación de AD FS entre geográficas de alta disponibilidad en Azure con Azure Traffic Manager
[AD FS implementación en Azure](how-to-connect-fed-azure-adfs.md) proporciona instrucciones paso a paso sobre cómo implementar una infraestructura de AD FS simple para su organización en Azure. En este artículo se proporcionan los siguientes pasos para crear una implementación entre AD FS en Azure con [azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/). Azure Traffic Manager ayuda a crear una infraestructura de AD FS de alto rendimiento y alta disponibilidad distribuida geográficamente para su organización, ya que hace uso de la gama de métodos de enrutamiento disponibles para adaptarse a las diferentes necesidades de la infraestructura.

Una infraestructura de AD FS multigeográfica de alta disponibilidad permite:

* **Eliminación de un único punto de error:** Con las funcionalidades de conmutación por error de Azure Traffic Manager, puede lograr una infraestructura de AD FS de alta disponibilidad incluso cuando uno de los centros de datos de una parte del mundo deja de funcionar.
* **Rendimiento mejorado:** Puede usar la implementación sugerida de este artículo para proporcionar una infraestructura de AD FS de alto rendimiento que pueda ayudar a los usuarios a autenticarse con mayor rapidez. 

## <a name="design-principles"></a>Principios de diseño
![Diseño general](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

Los principios de diseño básicos serán los mismos que los que se indican en principios de diseño en el artículo AD FS implementación en Azure. El diagrama anterior muestra una extensión simple de la implementación básica en otra región geográfica. A continuación se muestran algunos puntos que se deben tener en cuenta al extender la implementación a una nueva región geográfica.

* **Red virtual:** Debe crear una nueva red virtual en la región geográfica en la que desea implementar la infraestructura de AD FS adicional. En el diagrama anterior verá Geo1 VNET y la red virtual Geo2 como las dos redes virtuales en cada región geográfica.
* **Controladores de dominio y servidores de AD FS en la nueva red virtual geográfica:** Se recomienda implementar controladores de dominio en la nueva región geográfica para que los servidores de AD FS de la nueva región no tengan que ponerse en contacto con un controlador de dominio en otra red lejana para completar una autenticación y, de este modo, mejorar el rendimiento.
* **Cuentas de almacenamiento:** Las cuentas de almacenamiento están asociadas a una región. Puesto que va a implementar máquinas en una nueva región geográfica, tendrá que crear nuevas cuentas de almacenamiento para usarlas en la región.  
* **Grupos de seguridad de red:** Como cuentas de almacenamiento, los grupos de seguridad de red creados en una región no se pueden usar en otra región geográfica. Por lo tanto, tendrá que crear nuevos grupos de seguridad de red similares a los de la primera región geográfica de la subred INT y DMZ en la nueva región geográfica.
* **Etiquetas DNS para direcciones IP públicas:** Azure Traffic Manager puede hacer referencia a los puntos de conexión solo a través de etiquetas DNS. Por lo tanto, es necesario crear etiquetas DNS para las direcciones IP públicas de los equilibradores de carga externos.
* **Traffic Manager de Azure:** Microsoft Azure Traffic Manager le permite controlar la distribución del tráfico de usuario a los puntos de conexión de servicio que se ejecutan en distintos centros de usuarios de todo el mundo. Azure Traffic Manager funciona en el nivel de DNS. Usa las respuestas DNS para dirigir el tráfico del usuario final a los puntos de conexión distribuidos globalmente. Después, los clientes se conectan directamente a esos puntos de conexión. Con diferentes opciones de enrutamiento de rendimiento, ponderadas y prioritarias, puede elegir fácilmente la opción de enrutamiento que mejor se adapte a las necesidades de su organización. 
* **Conectividad de v-net a V-net entre dos regiones:** No es necesario tener conectividad entre las redes virtuales. Puesto que cada red virtual tiene acceso a los controladores de dominio y tiene AD FS y el servidor WAP en sí mismo, pueden funcionar sin conectividad entre las redes virtuales de diferentes regiones. 

## <a name="steps-to-integrate-azure-traffic-manager"></a>Pasos para integrar Azure Traffic Manager
### <a name="deploy-ad-fs-in-the-new-geographical-region"></a>Implementar AD FS en la nueva región geográfica
Siga los pasos y las instrucciones de [AD FS implementación en Azure](how-to-connect-fed-azure-adfs.md) para implementar la misma topología en la nueva región geográfica.

### <a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>Etiquetas DNS para direcciones IP públicas de los equilibradores de carga accesibles desde Internet (públicos)
Como se mencionó anteriormente, Azure Traffic Manager solo puede hacer referencia a las etiquetas DNS como puntos de conexión y, por lo tanto, es importante crear etiquetas DNS para las direcciones IP públicas de los equilibradores de carga externos. En la captura de pantalla siguiente se muestra cómo puede configurar la etiqueta DNS para la dirección IP pública. 

![Etiqueta DNS](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

### <a name="deploying-azure-traffic-manager"></a>Implementación de Traffic Manager de Azure
Siga los pasos que se indican a continuación para crear un perfil de Traffic Manager. Para obtener más información, también puede hacer referencia a [Administración de un perfil de Traffic Manager de Azure](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles).

1. **Crear un perfil de Traffic Manager:** Asigne un nombre único al perfil de Traffic Manager. Este nombre del perfil forma parte del nombre DNS y actúa como prefijo del Traffic Manager etiqueta de nombre de dominio. El nombre y el prefijo se agregan a. trafficmanager.net para crear una etiqueta DNS para el administrador de tráfico. La captura de pantalla siguiente muestra el prefijo de DNS del administrador de tráfico que se establece como mysts y la etiqueta DNS resultante será mysts.trafficmanager.net. 
   
    ![Creación de perfiles de Traffic Manager](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
2. **Método de enrutamiento de tráfico:** Hay tres opciones de enrutamiento disponibles en Traffic Manager:
   
   * Prioridad 
   * Rendimiento
   * Ponderado
     
     El **rendimiento** es la opción recomendada para lograr una infraestructura de AD FS con una gran capacidad de respuesta. Sin embargo, puede elegir cualquier método de enrutamiento que mejor se adapte a sus necesidades de implementación. La funcionalidad de AD FS no se ve afectada por la opción de enrutamiento seleccionada. Consulte [Traffic Manager métodos de enrutamiento del tráfico](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) para obtener más información. En la captura de pantalla de ejemplo anterior, puede ver el método de **rendimiento** seleccionado.
3. **Configurar puntos de conexión:** En la página Traffic Manager, haga clic en puntos de conexión y seleccione Agregar. Se abrirá una página Agregar punto de conexión similar a la siguiente captura de pantalla
   
   ![Configurar los puntos de conexión](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
   
   Para las diferentes entradas, siga estas instrucciones:
   
   **Tipo:** Seleccione el punto de conexión de Azure, ya que vamos a apuntar a una dirección IP pública de Azure.
   
   **Nombre:** Cree un nombre que desee asociar al punto de conexión. Este no es el nombre DNS y no tiene ningún cambio en los registros DNS.
   
   **Tipo de recurso de destino:** Seleccione dirección IP pública como valor de esta propiedad. 
   
   **Recurso de destino:** Esto le ofrecerá una opción para elegir entre las distintas etiquetas DNS que tiene disponibles en su suscripción. Elija la etiqueta DNS correspondiente al punto de conexión que está configurando.
   
   Agregue un punto de conexión para cada región geográfica a la que desee que Azure Traffic Manager Enrute el tráfico.
   Para obtener más información y pasos detallados sobre cómo agregar o configurar puntos de conexión en Traffic Manager, consulte [adición, deshabilitación, habilitación o eliminación de puntos de conexión](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints) .
4. **Configurar sondeo:** En la página Traffic Manager, haga clic en configuración. En la página Configuración, debe cambiar la configuración del monitor para sondear en el puerto HTTP 80 y la ruta de acceso relativa/ADFS/Probe
   
    ![Configurar sondeo](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 
   
   > [!NOTE]
   > **Asegúrese de que el estado de los puntos de conexión es en línea una vez completada la configuración**. Si todos los puntos de conexión están en el estado "degradado", Azure Traffic Manager realizará el mejor intento de enrutar el tráfico suponiendo que el diagnóstico es incorrecto y se puede tener acceso a todos los extremos.
   > 
   > 
5. **Modificación de los registros DNS para Azure Traffic Manager:** El servicio de Federación debe ser un CNAME del nombre DNS de Azure Traffic Manager. Cree un CNAME en los registros DNS públicos para que quien intente ponerse en contacto con el servicio de Federación llegue realmente a Azure Traffic Manager.
   
    Por ejemplo, para que el servicio de Federación se fs.fabidentity.com al Traffic Manager, sería necesario actualizar el registro de recursos DNS para que sea el siguiente:
   
    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

## <a name="test-the-routing-and-ad-fs-sign-in"></a>Prueba del enrutamiento y el inicio de sesión AD FS
### <a name="routing-test"></a>Prueba de enrutamiento
Una prueba muy básica del enrutamiento sería intentar hacer ping en el nombre DNS del servicio de Federación desde un equipo en cada región geográfica. En función del método de enrutamiento elegido, el punto de conexión en el que realmente se hace ping se reflejará en la pantalla de ping. Por ejemplo, si seleccionó el enrutamiento de rendimiento, se alcanzará el punto de conexión más cercano a la región del cliente. A continuación se muestra la instantánea de dos pings de dos equipos cliente de región diferentes, uno en la región EastAsia y otro en el oeste de EE. UU. 

![Prueba de enrutamiento](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

### <a name="ad-fs-sign-in-test"></a>Prueba de inicio de sesión AD FS
La forma más fácil de probar AD FS es mediante la página IdpInitiatedSignon. aspx. Para poder hacerlo, es necesario habilitar el IdpInitiatedSignOn en las propiedades del AD FS de. Siga los pasos que se indican a continuación para comprobar la configuración del AD FS

1. Ejecute el siguiente cmdlet en el servidor de AD FS, mediante PowerShell, para establecerlo en habilitado. 
   Set-AdfsProperties-EnableIdPInitiatedSignonPage $true
2. Desde cualquier acceso a máquina externo<yourfederationservicedns>https:///ADFS/LS/IdpInitiatedSignon.aspx
3. Debería ver la página AD FS como se muestra a continuación:
   
    ![Prueba de ADFS: desafío de autenticación](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)
   
    y, si el inicio de sesión se realiza correctamente, se le proporcionará un mensaje de operación correcta, como se muestra a continuación:
   
    ![Prueba de ADFS: autenticación correcta](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)

## <a name="related-links"></a>Vínculos relacionados
* [Implementación básica de AD FS en Azure](how-to-connect-fed-azure-adfs.md)
* [Microsoft Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/)
* [Métodos de enrutamiento de tráfico de Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods)

## <a name="next-steps"></a>Pasos siguientes
* [Administración de un perfil de Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles)
* [Adición, deshabilitación, habilitación o eliminación de puntos de conexión](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints) 

