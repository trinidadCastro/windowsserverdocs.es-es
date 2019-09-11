---
title: Servicios de federación de Active Directory (AD FS) en Azure | Microsoft Docs
description: En este documento, aprenderá a implementar AD FS en Azure para lograr una alta disponibilidad.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 692a188c-badc-44aa-ba86-71c0e8074510
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/28/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 15ebf21973f78ad705aca77e178005dfa9529cf2
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868219"
---
# <a name="deploying-active-directory-federation-services-in-azure"></a>Implementación de Servicios de federación de Active Directory (AD FS) en Azure
AD FS proporciona capacidades de Federación de identidades simplificada y protegida e inicio de sesión único (SSO) Web. La Federación con Azure AD o O365 permite a los usuarios autenticarse con credenciales locales y acceder a todos los recursos de la nube. Como resultado, es importante tener una infraestructura de AD FS de alta disponibilidad para garantizar el acceso a los recursos locales y en la nube. La implementación de AD FS en Azure puede ayudar a lograr la alta disponibilidad necesaria con el mínimo esfuerzo.
Hay varias ventajas de implementar AD FS en Azure, algunas de ellas se enumeran a continuación:

* **Alta disponibilidad** : con la eficacia de los conjuntos de disponibilidad de Azure, garantiza una infraestructura de alta disponibilidad.
* **Fácil de escalar** : ¿Necesita más rendimiento? Migre fácilmente a máquinas más eficaces con solo unos clics en Azure
* **Redundancia geográfica cruzada** : con la redundancia geográfica de Azure, puede estar seguro de que su infraestructura tiene alta disponibilidad en todo el mundo.
* **Fácil de administrar** : con opciones de administración muy simplificadas en Azure portal, la administración de la infraestructura es muy fácil y sin complicaciones. 

## <a name="design-principles"></a>Principios de diseño
![Diseño de implementación](./media/how-to-connect-fed-azure-adfs/deployment.png)

En el diagrama anterior se muestra la topología básica recomendada para empezar a implementar la infraestructura de AD FS en Azure. A continuación se enumeran los principios de los distintos componentes de la topología:

* **Servidores DC/ADFS**: Si tiene menos de 1.000 usuarios, simplemente puede instalar AD FS rol en los controladores de dominio. Si no desea ningún impacto en el rendimiento de los controladores de dominio o si tiene más de 1.000 usuarios, implemente AD FS en servidores independientes.
* **Servidor WAP** : es necesario implementar servidores proxy de aplicación web para que los usuarios puedan tener acceso a la AD FS cuando no estén también en la red de la empresa.
* **DMZ**: Los servidores proxy de aplicación web se colocarán en la red perimetral y solo se permite el acceso a TCP/443 entre la red perimetral y la subred interna.
* **Equilibradores de carga**: Para garantizar la alta disponibilidad de los servidores de AD FS y proxy de aplicación Web, se recomienda usar un equilibrador de carga interno para los servidores AD FS y Azure Load Balancer para los servidores proxy de aplicación Web.
* **Conjuntos de disponibilidad**: Para proporcionar redundancia a la implementación de AD FS, se recomienda agrupar dos o más máquinas virtuales en un conjunto de disponibilidad para cargas de trabajo similares. Esta configuración garantiza que durante un evento de mantenimiento planeado o no planeado, al menos una máquina virtual estará disponible.
* **Cuentas de almacenamiento**: Se recomienda tener dos cuentas de almacenamiento. Tener una sola cuenta de almacenamiento puede dar lugar a la creación de un único punto de error y puede hacer que la implementación deje de estar disponible en un escenario improbable en el que la cuenta de almacenamiento deje de funcionar. Dos cuentas de almacenamiento le ayudarán a asociar una cuenta de almacenamiento para cada línea de error.
* **Segregación de red**:  Los servidores proxy de aplicación web se deben implementar en una red DMZ independiente. Puede dividir una red virtual en dos subredes y, a continuación, implementar los servidores del proxy de aplicación web en una subred aislada. Puede simplemente configurar la configuración del grupo de seguridad de red para cada subred y permitir solo la comunicación necesaria entre las dos subredes. A continuación se proporcionan más detalles para cada escenario de implementación

## <a name="steps-to-deploy-ad-fs-in-azure"></a>Pasos para implementar AD FS en Azure
En los pasos que se mencionan en esta sección se describe la guía para implementar la siguiente infraestructura de AD FS representada en Azure.

### <a name="1-deploying-the-network"></a>1. Implementación de la red
Tal y como se ha descrito anteriormente, puede crear dos subredes en una sola red virtual, o bien crear dos redes virtuales completamente diferentes (VNet). Este artículo se centrará en la implementación de una única red virtual y la divida en dos subredes. Este es actualmente un enfoque más sencillo, ya que dos redes virtuales independientes necesitarían una puerta de enlace de red virtual a red virtual para las comunicaciones.

**1,1 creación de una red virtual**

![Crear red virtual](./media/how-to-connect-fed-azure-adfs/deploynetwork1.png)

En el Azure Portal, seleccione red virtual y puede implementar la red virtual y una subred inmediatamente con solo un clic. La subred INT también está definida y ya está lista para que se agreguen las máquinas virtuales.
El siguiente paso es agregar otra subred a la red, es decir, la subred DMZ. Para crear la subred DMZ, simplemente

* Seleccionar la red recién creada
* En las propiedades, seleccione subred.
* En el panel subred, haga clic en el botón Agregar
* Proporcione el nombre de la subred y la información del espacio de direcciones para crear la subred.

![Subred](./media/how-to-connect-fed-azure-adfs/deploynetwork2.png)

![Subred DMZ](./media/how-to-connect-fed-azure-adfs/deploynetwork3.png)

**1,2. Creación de grupos de seguridad de red**

Un grupo de seguridad de red (NSG) contiene una lista de reglas de lista de Access Control (ACL) que permiten o deniegan el tráfico de red a las instancias de máquina virtual en un Virtual Network. NSG se pueden asociar a subredes o a instancias de máquina virtual individuales dentro de esa subred. Cuando un NSG está asociado a una subred, las reglas de ACL se aplican a todas las instancias de máquina virtual de esa subred.
Con el fin de esta guía, crearemos dos NSG: uno para una red interna y una DMZ. Se etiquetarán como NSG_INT y NSG_DMZ, respectivamente.

![Crear NSG](./media/how-to-connect-fed-azure-adfs/creatensg1.png)

Una vez creado el NSG, habrá 0 reglas de entrada y de salida de 0. Una vez que los roles de los servidores respectivos están instalados y funcionan, las reglas entrantes y salientes se pueden realizar según el nivel de seguridad deseado.

![Inicializar NSG](./media/how-to-connect-fed-azure-adfs/nsgint1.png)

Una vez creados los NSG, asocie NSG_INT con la subred INT y NSG_DMZ con la subred DMZ. A continuación se muestra una captura de pantalla de ejemplo:

![Configuración de NSG](./media/how-to-connect-fed-azure-adfs/nsgconfigure1.png)

* Haga clic en subredes para abrir el panel de las subredes.
* Seleccione la subred que se va a asociar con el NSG 

Después de la configuración, el panel de las subredes debe tener el siguiente aspecto:

![Subredes después de NSG](./media/how-to-connect-fed-azure-adfs/nsgconfigure2.png)

**1,3. Creación de una conexión a local**

Se necesitará una conexión a local para poder implementar el controlador de dominio (DC) en Azure. Azure ofrece varias opciones de conectividad para conectar su infraestructura local a su infraestructura de Azure.

* De punto a sitio
* Virtual Network sitio a sitio
* ExpressRoute

Se recomienda usar ExpressRoute. ExpressRoute le permite crear conexiones privadas entre los centros de recursos de Azure y la infraestructura local o en un entorno de ubicación compartida. Las conexiones ExpressRoute no pasan por la red pública de Internet. Ofrecen más confiabilidad, velocidades más rápidas, latencias más bajas y mayor seguridad que las típicas conexiones a través de Internet.
Aunque se recomienda usar ExpressRoute, puede elegir cualquier método de conexión más adecuado para su organización. Para obtener más información acerca de ExpressRoute y las diversas opciones de conectividad con ExpressRoute, consulte [información técnica de expressroute](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2. Creación de cuentas de almacenamiento
Con el fin de mantener la alta disponibilidad y evitar la dependencia en una sola cuenta de almacenamiento, puede crear dos cuentas de almacenamiento. Divida las máquinas de cada conjunto de disponibilidad en dos grupos y, a continuación, asigne a cada grupo una cuenta de almacenamiento independiente.

![Creación de cuentas de almacenamiento](./media/how-to-connect-fed-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. Crear conjuntos de disponibilidad
Para cada rol (DC/AD FS y WAP), cree conjuntos de disponibilidad que contendrán dos máquinas como mínimo. Esto le ayudará a lograr una mayor disponibilidad para cada rol. Al crear los conjuntos de disponibilidad, es esencial decidir lo siguiente:

* **Dominios de error**: Las máquinas virtuales del mismo dominio de error comparten la misma fuente de alimentación y el mismo conmutador de red física. Se recomienda un mínimo de 2 dominios de error. El valor predeterminado es 3 y puede dejarlo tal cual para esta implementación.
* **Dominios de actualización**: Las máquinas que pertenecen al mismo dominio de actualización se reinician juntas durante una actualización. Desea tener un mínimo de 2 dominios de actualización. El valor predeterminado es 5 y puede dejarlo tal cual para esta implementación.

![Conjuntos de disponibilidad](./media/how-to-connect-fed-azure-adfs/availabilityset1.png)

Crear los siguientes conjuntos de disponibilidad

| Conjunto de disponibilidad | Rol | Dominios de error | Dominios de actualización |
|:---:|:---:|:---:|:--- |
| contosodcset |DC/ADFS |3 |5 |
| contosowapset |WAP |3 |5 |

### <a name="4-deploy-virtual-machines"></a>4. Implementar máquinas virtuales
El siguiente paso es implementar las máquinas virtuales que hospedarán los distintos roles en la infraestructura. En cada conjunto de disponibilidad se recomienda un mínimo de dos equipos. Cree cuatro máquinas virtuales para la implementación básica.

| Machine | Rol | Subred | Conjunto de disponibilidad | Cuenta de almacenamiento | Dirección IP |
|:---:|:---:|:---:|:---:|:---:|:---:|
| contosodc1 |DC/ADFS |INT |contosodcset |contososac1 |Estático |
| contosodc2 |DC/ADFS |INT |contosodcset |contososac2 |Estático |
| contosowap1 |WAP |DMZ |contosowapset |contososac1 |Estático |
| contosowap2 |WAP |DMZ |contosowapset |contososac2 |Estático |

Como podría haber observado, no se ha especificado ningún NSG. Esto se debe a que Azure le permite usar NSG en el nivel de subred. A continuación, puede controlar el tráfico de red de la máquina mediante el NSG individual asociado a la subred o bien al objeto NIC. Obtenga más información sobre [Qué es un grupo de seguridad de red (NSG)](https://aka.ms/Azure/NSG).
Se recomienda usar una dirección IP estática Si está administrando el DNS. Puede usar Azure DNS y, en su lugar, en los registros DNS de su dominio, consulte los nuevos equipos con sus FQDN de Azure.
El panel de la máquina virtual debe tener el siguiente aspecto una vez completada la implementación:

![Virtual Machines implementado](./media/how-to-connect-fed-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5. Configuración de los servidores de controlador de dominio/AD FS
 Para autenticar cualquier solicitud entrante, AD FS deberá ponerse en contacto con el controlador de dominio. Para ahorrar costos de ida y vuelta desde Azure a un controlador de dominio local para la autenticación, se recomienda implementar una réplica del controlador de dominio en Azure. Para lograr una alta disponibilidad, se recomienda crear un conjunto de disponibilidad de al menos 2 controladores de dominio.

| Controlador de dominio | Rol | Cuenta de almacenamiento |
|:---:|:---:|:---:|
| contosodc1 |Réplica |contososac1 |
| contosodc2 |Réplica |contososac2 |

* Promover los dos servidores como controladores de dominio de réplica con DNS
* Configure los servidores de AD FS mediante la instalación del rol de AD FS mediante el administrador del servidor.

### <a name="6-deploying-internal-load-balancer-ilb"></a>6. Implementación de Load Balancer internos (ILB)
**6,1. Crear ILB**

Para implementar un ILB, seleccione equilibradores de carga en el Azure Portal y haga clic en Agregar (+).

> [!NOTE]
> Si no ve **equilibradores de carga** en el menú, haga clic en **examinar** en la parte inferior izquierda del portal y desplácese hasta que vea **equilibradores de carga**.  A continuación, haga clic en la estrella amarilla para agregarla al menú. Ahora, seleccione el icono nuevo equilibrador de carga para abrir el panel y comenzar la configuración del equilibrador de carga.
> 
> 

![Examinar el equilibrador de carga](./media/how-to-connect-fed-azure-adfs/browseloadbalancer.png)

* **Nombre**: Asigne un nombre adecuado al equilibrador de carga.
* **Esquema**: Puesto que este equilibrador de carga se colocará delante de los servidores AD FS y está pensado solo para conexiones de red internas, seleccione "interno".
* **Virtual Network**: Elija la red virtual en la que va a implementar el AD FS
* **Subred**: Elija aquí la subred interna
* **Asignación de direcciones IP**: Estático

![Equilibrador de carga interno](./media/how-to-connect-fed-azure-adfs/ilbdeployment1.png)

Después de hacer clic en crear y ILB está implementado, debería verlo en la lista de equilibradores de carga:

![Equilibradores de carga después de ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment2.png)

El siguiente paso consiste en configurar el grupo de back-end y el sondeo de back-end.

**6,2. Configuración del grupo de back-end de ILB**

Seleccione el ILB recién creado en el panel equilibradores de carga. Se abrirá el panel de configuración. 

1. Seleccione grupos de back-end en el panel de configuración
2. En el panel agregar grupo de back-end, haga clic en agregar máquina virtual.
3. Se le presentará un panel donde puede elegir el conjunto de disponibilidad.
4. Elección del conjunto de disponibilidad de AD FS

![Configuración del grupo de back-end de ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment3.png)

**6,3. Configuración del sondeo**

En el panel de configuración de ILB, seleccione sondeos de estado.

1. Haga clic en Agregar.
2. Proporcione los detalles del sondeo a. **Nombre**: Nombre del sondeo b. **Protocolo**: HTTP c. **Puerto**: 80 (HTTP) d. **Ruta de acceso**:/ADFS/Probe e. **Intervalo**: 5 (valor predeterminado): este es el intervalo en el que ILB sondeará las máquinas en el grupo de back-end f. **Límite de umbral incorrecto**: 2 (valor predeterminado): es el umbral de errores de sondeo consecutivos después del cual ILB declarará una máquina en el grupo de back-end que no responde y dejará de enviarle tráfico.

![Configuración del sondeo ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment4.png)

Usamos el punto de conexión/ADFS/Probe que se creó explícitamente para las comprobaciones de estado en un entorno de AD FS en el que no se puede realizar una comprobación completa de la ruta de acceso HTTPS.  Esto es mucho mejor que una comprobación de Puerto básico 443, que no refleja con precisión el estado de una implementación de AD FS moderna.  Puede encontrar https://blogs.technet.microsoft.com/applicationproxyblog/2014/10/17/hardware-load-balancer-health-checks-and-web-application-proxy-ad-fs-2012-r2/ más información en.

**6,4. Creación de reglas de equilibrio de carga**

Para equilibrar el tráfico de forma eficaz, el ILB debe configurarse con reglas de equilibrio de carga. Para crear una regla de equilibrio de carga, 

1. Seleccione regla de equilibrio de carga en el panel de configuración de ILB
2. Haga clic en agregar en el panel regla de equilibrio de carga.
3. En el panel Agregar regla de equilibrio de carga a. **Nombre**: Proporcione un nombre para la regla b. **Protocolo**: Seleccione TCP c. **Puerto**: 443 d. **Puerto de back-end**: 443 e. **Grupo de back-end**: Seleccione el grupo que ha creado para el clúster de AD FS anterior f. **Sondeo**: Seleccione el sondeo creado para AD FS servidores anteriores

![Configuración de reglas de equilibrio de ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment5.png)

**6,5. Actualización de DNS con ILB**

Vaya al servidor DNS y cree un CNAME para ILB. El CNAME debe ser para el servicio de Federación con la dirección IP que apunta a la dirección IP del ILB. Por ejemplo, si la dirección DIP de ILB es 10.3.0.8 y el servicio de Federación instalado es fs.contoso.com, cree un CNAME para fs.contoso.com que apunte a 10.3.0.8.
Así se asegurará de que toda la comunicación con respecto a fs.contoso.com termina en el ILB y se enruta correctamente.

### <a name="7-configuring-the-web-application-proxy-server"></a>7. Configuración del servidor proxy de aplicación Web
**7,1. Configuración de los servidores proxy de aplicación web para llegar a AD FS servidores**

Para asegurarse de que los servidores proxy de aplicación Web pueden llegar a los servidores AD FS detrás de ILB, cree un registro en%systemroot%\system32\drivers\etc\hosts para ILB. Tenga en cuenta que el nombre distintivo (DN) debe ser el nombre del servicio de Federación, por ejemplo fs.contoso.com. Y la entrada IP debe ser la dirección IP del ILB (10.3.0.8 como en el ejemplo).

**7,2. Instalación del rol de proxy de aplicación Web**

Después de asegurarse de que los servidores proxy de aplicación Web pueden llegar a los servidores AD FS detrás de ILB, puede instalar los servidores proxy de aplicación Web. No es necesario que los servidores proxy de aplicación Web estén Unidos al dominio. Instale los roles de proxy de aplicación web en los dos servidores proxy de aplicación web seleccionando el rol de acceso remoto. El administrador del servidor le guiará para completar la instalación de WAP.
Para obtener más información sobre cómo implementar WAP, lea [instalar y configurar el servidor proxy de aplicación web](https://technet.microsoft.com/library/dn383662.aspx).

### <a name="8--deploying-the-internet-facing-public-load-balancer"></a>8.  Implementación del Load Balancer accesible desde Internet (público)
**8,1.  Cree Load Balancer accesibles desde Internet (público)**

En el Azure Portal, seleccione equilibradores de carga y, a continuación, haga clic en Agregar. En el panel crear equilibrador de carga, escriba la siguiente información:

1. **Nombre**: Nombre del equilibrador de carga
2. **Esquema**: Público: esta opción indica a Azure que este equilibrador de carga necesitará una dirección pública.
3. **Dirección IP**: Crear una nueva dirección IP (dinámica)

![Equilibrador de carga accesible desde Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment1.png)

Después de la implementación, el equilibrador de carga aparecerá en la lista de equilibradores de carga.

![Lista de equilibradores de carga](./media/how-to-connect-fed-azure-adfs/elbdeployment2.png)

**8,2. Asignación de una etiqueta DNS a la dirección IP pública**

Haga clic en la entrada del equilibrador de carga recién creado en el panel equilibradores de carga para que aparezca el panel de configuración. Siga los pasos siguientes para configurar la etiqueta DNS para la dirección IP pública:

1. Haga clic en la dirección IP pública. Se abrirá el panel de la dirección IP pública y su configuración
2. Haga clic en configuración.
3. Proporcione una etiqueta DNS. Se convertirá en la etiqueta DNS pública a la que puede acceder desde cualquier lugar, por ejemplo contosofs.westus.cloudapp.azure.com. Puede Agregar una entrada en el DNS externo para el servicio de Federación (por ejemplo, fs.contoso.com) que se resuelve en la etiqueta DNS del equilibrador de carga externo (contosofs.westus.cloudapp.azure.com).

![Configuración del equilibrador de carga accesible desde Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment3.png) 

![Configuración del equilibrador de carga accesible desde Internet (DNS)](./media/how-to-connect-fed-azure-adfs/elbdeployment4.png)

**8,3. Configurar el grupo de back-end para Load Balancer accesible desde Internet (público)** 

Siga los mismos pasos que en la creación del equilibrador de carga interno para configurar el grupo de back-end para el Load Balancer accesible desde Internet (público) como conjunto de disponibilidad para los servidores WAP. Por ejemplo, contosowapset.

![Configurar el grupo de back-end de Load Balancer accesible desde Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment5.png)

**8,4. Configurar sondeo**

Siga los mismos pasos que en la configuración del equilibrador de carga interno para configurar el sondeo para el grupo de back-end de servidores WAP.

![Configure el sondeo de Load Balancer orientado a Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment6.png)

**8,5. Crear reglas de equilibrio de carga**

Siga los mismos pasos que en ILB para configurar la regla de equilibrio de carga para TCP 443.

![Configuración de reglas de equilibrio de Load Balancer accesibles desde Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment7.png)

### <a name="9-securing-the-network"></a>9. Protección de la red
**9,1. Protección de la subred interna**

En general, necesita las siguientes reglas para proteger eficazmente la subred interna (en el orden que se indica a continuación).

| Regla | Descripción | Flujo |
|:--- |:--- |:---:|
| AllowHTTPSFromDMZ |Permitir la comunicación HTTPS desde la red perimetral |Entrada |
| DenyInternetOutbound |Sin acceso a Internet |Salida |

![Reglas de acceso INT (entrantes)](./media/how-to-connect-fed-azure-adfs/nsg_int.png)

**9,2. Protección de la subred DMZ**

| Regla | Descripción | Flujo |
|:--- |:--- |:---:|
| AllowHTTPSFromInternet |Permitir HTTPS de Internet a la red perimetral |Entrada |
| DenyInternetOutbound |Cualquier cosa excepto HTTPS a Internet está bloqueada |Salida |

![Reglas de acceso EXT (entrantes)](./media/how-to-connect-fed-azure-adfs/nsg_dmz.png)

> [!NOTE]
> Si se requiere la autenticación de certificados de usuario de cliente (autenticación clientTLS mediante certificados de usuario X509), AD FS requiere que el puerto TCP 49443 esté habilitado para el acceso de entrada.
> 
> 

### <a name="10-test-the-ad-fs-sign-in"></a>10. Prueba del inicio de sesión AD FS
La manera más fácil es probar AD FS es mediante la página IdpInitiatedSignon. aspx. Para poder hacerlo, es necesario habilitar el IdpInitiatedSignOn en las propiedades del AD FS de. Siga los pasos que se indican a continuación para comprobar la configuración del AD FS

1. Ejecute el siguiente cmdlet en el servidor de AD FS, mediante PowerShell, para establecerlo en habilitado.
   Set-AdfsProperties-EnableIdPInitiatedSignonPage $true 
2. Desde cualquier máquina externa, acceda a https\/:/ADFS-Server.contoso.com/ADFS/LS/IdpInitiatedSignon.aspx.  
3. Debería ver la página AD FS como se muestra a continuación:

![Página probar inicio de sesión](./media/how-to-connect-fed-azure-adfs/test1.png)

Si el inicio de sesión se realiza correctamente, se le proporcionará un mensaje de operación correcta, como se muestra a continuación:

![Prueba correcta](./media/how-to-connect-fed-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Plantilla para implementar AD FS en Azure
La plantilla implementa una configuración de 6 máquinas, 2 para los controladores de dominio, AD FS y WAP.

[AD FS en la plantilla de implementación de Azure](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

Puede usar una red virtual existente o crear una nueva red virtual durante la implementación de esta plantilla. Los distintos parámetros disponibles para personalizar la implementación se enumeran a continuación con la descripción del uso del parámetro en el proceso de implementación. 

| Parámetro | Descripción |
|:--- |:--- |
| Location |Región en la que se van a implementar los recursos, por ejemplo, este de EE. UU. |
| StorageAccountType |El tipo de la cuenta de almacenamiento creada |
| VirtualNetworkUsage |Indica si se creará una nueva red virtual o usará una existente. |
| VirtualNetworkName |Nombre del Virtual Network que se va a crear, obligatorio en el uso de la red virtual existente o en el nuevo. |
| VirtualNetworkResourceGroupName |Especifica el nombre del grupo de recursos donde reside la red virtual existente. Cuando se usa una red virtual existente, se convierte en un parámetro obligatorio para que la implementación pueda encontrar el identificador de la red virtual existente. |
| VirtualNetworkAddressRange |El intervalo de direcciones de la red virtual nueva, obligatorio si se crea una nueva red virtual. |
| InternalSubnetName |El nombre de la subred interna, obligatorio en ambas opciones de uso de red virtual (nuevo o existente) |
| InternalSubnetAddressRange |El intervalo de direcciones de la subred interna, que contiene los controladores de dominio y los servidores de ADFS, es obligatorio si se crea una nueva red virtual. |
| DMZSubnetAddressRange |El intervalo de direcciones de la subred DMZ, que contiene los servidores proxy de aplicación de Windows, obligatorio si se crea una nueva red virtual. |
| DMZSubnetName |El nombre de la subred interna, obligatorio en ambas opciones de uso de red virtual (nuevo o existente). |
| ADDC01NICIPAddress |La dirección IP interna del primer controlador de dominio, esta dirección IP se asignará estáticamente al DC y debe ser una dirección IP válida dentro de la subred interna. |
| ADDC02NICIPAddress |La dirección IP interna del segundo controlador de dominio, esta dirección IP se asignará estáticamente al DC y debe ser una dirección IP válida dentro de la subred interna. |
| ADFS01NICIPAddress |La dirección IP interna del primer servidor de ADFS, esta dirección IP se asignará estáticamente al servidor de ADFS y debe ser una dirección IP válida dentro de la subred interna. |
| ADFS02NICIPAddress |La dirección IP interna del segundo servidor ADFS, esta dirección IP se asignará estáticamente al servidor ADFS y debe ser una dirección IP válida dentro de la subred interna. |
| WAP01NICIPAddress |La dirección IP interna del primer servidor WAP, esta dirección IP se asignará estáticamente al servidor WAP y debe ser una dirección IP válida dentro de la subred DMZ. |
| WAP02NICIPAddress |La dirección IP interna del segundo servidor WAP, esta dirección IP se asignará estáticamente al servidor WAP y debe ser una dirección IP válida dentro de la subred DMZ. |
| ADFSLoadBalancerPrivateIPAddress |La dirección IP interna del equilibrador de carga de ADFS, esta dirección IP se asignará estáticamente al equilibrador de carga y debe ser una dirección IP válida dentro de la subred interna. |
| ADDCVMNamePrefix |Prefijo de nombre de máquina virtual para controladores de dominio |
| ADFSVMNamePrefix |Prefijo de nombre de máquina virtual para servidores ADFS |
| WAPVMNamePrefix |Prefijo de nombre de máquina virtual para servidores WAP |
| ADDCVMSize |El tamaño de la máquina virtual de los controladores de dominio |
| ADFSVMSize |El tamaño de la máquina virtual de los servidores ADFS |
| WAPVMSize |El tamaño de la máquina virtual de los servidores WAP |
| AdminUserName |El nombre del administrador local de las máquinas virtuales |
| AdminPassword |La contraseña de la cuenta de administrador local de las máquinas virtuales |

## <a name="additional-resources"></a>Recursos adicionales
* [Conjuntos de disponibilidad](https://aka.ms/Azure/Availability) 
* [Azure Load Balancer](https://aka.ms/Azure/ILB)
* [Load Balancer interno](https://aka.ms/Azure/ILB/Internal)
* [Load Balancer orientado a Internet](https://aka.ms/Azure/ILB/Internet)
* [Cuentas de almacenamiento](https://aka.ms/Azure/Storage)
* [Redes virtuales de Azure](https://aka.ms/Azure/VNet)
* [Vínculos de AD FS y proxy de aplicación Web](https://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Pasos siguientes
* [Integración de las identidades locales con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)
* [Configurar y administrar el AD FS mediante Azure AD Connect](/azure/active-directory/hybrid/how-to-connect-fed-whatis)
* [Implementación de AD FS entre geográficas de alta disponibilidad en Azure con Azure Traffic Manager](active-directory-adfs-in-azure-with-azure-traffic-manager.md)
