---
title: Implementación de alta disponibilidad entre regiones geográficas de AD FS en Azure con Azure Traffic Manager | Microsoft Docs
description: En este documento obtendrá información sobre cómo implementar AD FS en Azure para alta disponibilidad.
keywords: AD fs con el Administrador de tráfico de Azure, adfs con Azure Traffic Manager, geográfico, centro de datos de múltiples, centros de datos geográficos, varios centros de datos geográficos, implementación AD FS en azure, implementación azure adfs, adfs azure, azure ad fs, implementación adfs, implementación ad fs, adfs en azure, implementar AD FS en azure, implementar AD FS en azure, adfs azure, introducción a AD FS, Azure, AD FS en Azure, iaas, ADFS, mover adfs a azure
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
ms.openlocfilehash: 4af0386ef97694baa9ba7f1e5e7163554d79734a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874126"
---
# <a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Implementación de alta disponibilidad entre regiones geográficas de AD FS en Azure con Azure Traffic Manager
[Implementación de AD FS en Azure](how-to-connect-fed-azure-adfs.md) proporciona instrucciones paso a paso sobre cómo puede implementar una infraestructura de AD FS sencilla para su organización en Azure. Este artículo proporcionan los pasos siguientes para crear una implementación entre regiones geográficas de AD FS en Azure mediante [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/). Azure Traffic Manager ayuda a crear una propagación geográficamente alta disponibilidad y la infraestructura de AD FS de alto rendimiento para su organización mediante la realización de la serie de métodos de enrutamiento satisfacer diferentes necesidades de la infraestructura.

Permite una infraestructura de AD FS alta disponibilidad entre regiones geográficas:

* **Eliminación de puntos únicos de error:** Con capacidades de conmutación por error de Azure Traffic Manager, puede lograr una infraestructura de AD FS de alta disponibilidad incluso cuando uno de los centros de datos en una parte de todo el mundo deja de funcionar
* **Mejorar el rendimiento:** Puede usar la implementación recomendada en este artículo para proporcionar una infraestructura de AD FS de alto rendimiento que puede ayudar a los usuarios a autenticarse con mayor rapidez. 

## <a name="design-principles"></a>Principios de diseño
![Diseño general](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

Los principios de diseño básicos serán los mismos, como se muestra en los principios de diseño en el artículo de la implementación de AD FS en Azure. El diagrama anterior muestra una extensión simple de la implementación básica en otra región geográfica. A continuación se muestran algunos puntos a tener en cuenta al ampliar la implementación a la nueva región geográfica

* **Red virtual:** Debe crear una nueva red virtual en la región geográfica que desea implementar la infraestructura de AD FS adicional. En el diagrama anterior puede ver la red virtual Geo1 y la red virtual Geo2 como las dos redes virtuales en cada región geográfica.
* **Controladores de dominio y servidores de AD FS en la nueva red virtual geográfica:** Se recomienda implementar controladores en la nueva región geográfica para que los servidores de AD FS en la nueva región no es necesario ponerse en contacto con un controlador de dominio en otro lejos de red para realizar una autenticación y lo que mejora el rendimiento de dominio.
* **Cuentas de almacenamiento:** Las cuentas de almacenamiento asociadas a una región. Puesto que va a implementar las máquinas en una nueva región geográfica, tendrá que crear nuevas cuentas de almacenamiento que se usará en la región.  
* **Grupos de seguridad de red:** Como las cuentas de almacenamiento, grupos de seguridad de red creados en una región no se puede usar en otra región geográfica. Por lo tanto, deberá crear nueva red de grupos de seguridad similares a los de la primera región geográfica para subredes INT y DMZ en la nueva región geográfica.
* **Etiquetas DNS para direcciones IP públicas:** Azure Traffic Manager puede hacer referencia a los puntos de conexión solo a través de etiquetas DNS. Por lo tanto, son necesarios para crear etiquetas DNS para las direcciones IP públicas de los equilibradores de carga externo.
* **Traffic Manager de Azure:** Microsoft Azure Traffic Manager permite controlar la distribución del tráfico de usuarios a los puntos de conexión de servicio que se ejecutan en distintos centros de datos en todo el mundo. Azure Traffic Manager funciona en el nivel de DNS. Usa las respuestas DNS para dirigir el tráfico de usuarios finales a puntos de conexión distribuidos globalmente. Los clientes, a continuación, conectarse a esos puntos de conexión directamente. Con diferentes opciones de enrutamientos de rendimiento, ponderado y prioridad, puede elegir fácilmente la opción de enrutamiento que mejor se adapte a las necesidades de su organización. 
* **Conectividad de red virtual a red virtual entre dos regiones:** No es necesario disponer de conectividad entre las redes virtuales propio. Ya que cada red virtual tiene acceso a los controladores de dominio y servidor de AD FS y WAP en sí mismo, puede funcionar sin ninguna conectividad entre las redes virtuales en diferentes regiones. 

## <a name="steps-to-integrate-azure-traffic-manager"></a>Pasos para integrar Azure Traffic Manager
### <a name="deploy-ad-fs-in-the-new-geographical-region"></a>Implementar AD FS en la nueva región geográfica
Siga los pasos y las directrices descritas en [implementación de AD FS en Azure](how-to-connect-fed-azure-adfs.md) para implementar la misma topología en la nueva región geográfica.

### <a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>Etiquetas DNS para direcciones IP públicas de los equilibradores de carga accesible desde Internet (público)
Como se mencionó anteriormente, Azure Traffic Manager solo puede hacer referencia a etiquetas DNS como puntos de conexión y, por tanto, es importante crear etiquetas DNS para las direcciones IP públicas de los equilibradores de carga externo. Captura de pantalla siguiente muestra cómo puede configurar la etiqueta DNS para la dirección IP pública. 

![Etiqueta de DNS](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

### <a name="deploying-azure-traffic-manager"></a>Implementación de Azure Traffic Manager
Siga estos pasos para crear un perfil de traffic manager. Para obtener más información, puede también hacer referencia a [administrar un perfil de Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles).

1. **Crear un perfil de Traffic Manager:** Asigne un nombre único a su perfil de traffic manager. Este nombre del perfil forma parte del nombre DNS y actúa como un prefijo para la etiqueta de nombre de dominio de Traffic Manager. El nombre / prefijo se agrega a. trafficmanager.net para crear una etiqueta DNS para el tráfico de su administrador. La captura de pantalla siguiente muestra al administrador de tráfico de prefijo de DNS que se establece como mysts y la etiqueta DNS resultante será mysts.trafficmanager.net. 
   
    ![Creación de perfiles de Traffic Manager](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
2. **Método de enrutamiento de tráfico:** Hay tres opciones de enrutamiento disponibles en el Administrador de tráfico:
   
   * Prioridad 
   * Rendimiento
   * Ponderado
     
     **Rendimiento** es la opción recomendada para lograr la infraestructura de AD FS de alta capacidad de respuesta. Sin embargo, puede elegir el método de enrutamiento que mejor se adapte a sus necesidades de implementación. La funcionalidad de AD FS no se ve afectada por la opción de enrutamiento seleccionada. Consulte [métodos de enrutamiento de tráfico de Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) para obtener más información. En el ejemplo de captura de pantalla anterior, puede ver el **rendimiento** método seleccionado.
3. **Configurar los puntos de conexión:** En la página Administrador de tráfico, haga clic en los puntos de conexión y seleccione Agregar. Se abrirá una página de punto de conexión de agregar similar a la siguiente captura de pantalla
   
   ![Configurar los puntos de conexión](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
   
   Para las distintas entradas, siga este criterio:
   
   **Tipo:** Seleccione el punto de conexión de Azure como que se va a apuntar a una dirección IP pública de Azure.
   
   **Nombre:** Cree un nombre que desea asociar con el punto de conexión. Esto no es el nombre DNS y no influye en los registros DNS.
   
   **Tipo de recurso de destino:** Seleccione la dirección IP pública como el valor para esta propiedad. 
   
   **Recurso de destino:** Esto le dará una opción para elegir entre las diferentes etiquetas DNS que tiene disponibles con su suscripción. Elija la etiqueta DNS correspondiente al punto de conexión que está configurando.
   
   Agregar punto de conexión para cada región geográfica que desea que el Administrador de tráfico de Azure para enrutar el tráfico.
   Para obtener más información e instrucciones detalladas sobre cómo agregar y configurar los puntos de conexión en el Administrador de tráfico, consulte [agregar, deshabilitar, habilitar o eliminar puntos de conexión](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints)
4. **Configurar sondeo:** En la página Administrador de tráfico, haga clic en configuración. En la página de configuración, deberá cambiar la configuración del monitor de sondeo en el puerto HTTP 80 y la ruta de acceso relativa/ADFS/Probe
   
    ![Configuración de sondeo](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 
   
   > [!NOTE]
   > **Asegúrese de que el estado de los puntos de conexión está en línea una vez completada la configuración**. Si todos los extremos están en estado 'Degradado"', Azure Traffic Manager hará un mejor intento para enrutar el tráfico que suponiendo que el diagnóstico sea correcto y que todos los puntos de conexión estén accesibles.
   > 
   > 
5. **Modificar registros DNS de Azure Traffic Manager:** El servicio de federación debe ser un registro CNAME para el nombre DNS de Azure Traffic Manager. Crear un CNAME en los registros DNS públicos para que quien está intentando tener acceso el servicio de federación llegue realmente a Azure Traffic Manager.
   
    Por ejemplo, para que apunte el servicio de federación fs.fabidentity.com a Traffic Manager, deberá actualizar el registro de recursos DNS para que sea el siguiente:
   
    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

## <a name="test-the-routing-and-ad-fs-sign-in"></a>Probar el enrutamiento y de inicio de sesión de AD FS
### <a name="routing-test"></a>Prueba de enrutamiento
Una prueba muy básica para el enrutamiento sería intentar hacer ping en el nombre DNS del servicio de federación de una máquina de cada región geográfica. Según el método de enrutamiento seleccionado, el punto de conexión realmente hace ping se reflejará en la pantalla de ping. Por ejemplo, si ha seleccionado el enrutamiento de rendimiento, se alcanza el punto de conexión más cercano a la región del cliente. A continuación es la instantánea de dos pings desde dos máquinas de cliente de una región diferente, uno en la región de Asia oriental y otro en el oeste de Estados Unidos. 

![Prueba de enrutamiento](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

### <a name="ad-fs-sign-in-test"></a>Inicio de sesión de prueba de AD FS
La manera más fácil de probar AD FS es mediante el uso de la página IdpInitiatedSignon.aspx. Para poder hacer, que es necesario para habilitar IdpInitiatedSignOn en las propiedades de AD FS. Siga los pasos siguientes para comprobar la configuración de AD FS

1. Ejecute el siguiente cmdlet en el servidor de AD FS, con PowerShell, habilítela. 
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true
2. Desde cualquier equipo externo acceso https://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. Debería ver la página de AD FS como aparece a continuación:
   
    ![Prueba de ADFS - desafío de autenticación](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)
   
    y en el inicio de sesión correcto, le proporcionará un mensaje de confirmación tal como se muestra a continuación:
   
    ![Prueba de ADFS - autenticación correcta](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)

## <a name="related-links"></a>Vínculos relacionados
* [Implementación básica de AD FS en Azure](how-to-connect-fed-azure-adfs.md)
* [Microsoft Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/)
* [Métodos de enrutamiento de tráfico de Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods)

## <a name="next-steps"></a>Pasos siguientes
* [Administrar un perfil de Traffic Manager de Azure](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles)
* [Agregar, deshabilitar, habilitar o eliminar puntos de conexión](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints) 

