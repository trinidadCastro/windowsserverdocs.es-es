---
title: Servicios de federación de Active Directory en Azure | Microsoft Docs
description: En este documento obtendrá información sobre cómo implementar AD FS en Azure para alta disponibilidad.
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
ms.openlocfilehash: f075f91e97f806555507bfc0e0c5f3d1589a71e6
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469650"
---
# <a name="deploying-active-directory-federation-services-in-azure"></a>Implementación de servicios de federación de Active Directory en Azure
AD FS proporciona capacidades de inicio de sesión único (SSO) Web y de federación de identidades simplificada y protegida. Federación con Azure AD u Office 365 permite a los usuarios autenticarse mediante las credenciales de forma local y tener acceso a todos los recursos de nube. Como resultado, es importante tener una infraestructura de AD FS de alta disponibilidad para garantizar el acceso a recursos locales y en la nube. Implementación de AD FS en Azure puede ayudar a lograr la alta disponibilidad necesaria con el mínimo esfuerzo.
Hay varias ventajas de la implementación de AD FS en Azure, algunas de ellas se enumeran a continuación:

* **Alta disponibilidad** -con la potencia de conjuntos de disponibilidad de Azure, se asegura una infraestructura de alta disponibilidad.
* **¿Fácil de escalar** – necesita más rendimiento? Migre fácilmente a máquinas más eficaces mediante unos pocos clics en Azure
* **Redundancia geográfica cruzada** : con Azure la redundancia geográfica puede estar seguro de que su infraestructura tiene alta disponibilidad en todo el mundo
* **Fácil de administrar** : con opciones de administración muy simplificadas en Azure portal, administrar su infraestructura es muy fácil y sin complicaciones 

## <a name="design-principles"></a>Principios de diseño
![Diseño de implementación](./media/how-to-connect-fed-azure-adfs/deployment.png)

El diagrama anterior muestra la topología básica recomendada para empezar a implementar la infraestructura de AD FS en Azure. Los principios de los distintos componentes de la topología se enumeran a continuación:

* **Controlador de dominio o servidores de AD FS**: Si tiene menos de 1000 usuarios, simplemente puede instalar el rol de AD FS en los controladores de dominio. Si no desea que afecte al rendimiento en los controladores de dominio o si tiene más de 1000 usuarios, a continuación, implementar AD FS en servidores independientes.
* **Servidor WAP** : es necesario implementar servidores Proxy de aplicación Web, por lo que los usuarios puedan acceder a AD FS cuando no están en la red de empresa también.
* **DMZ**: Los servidores Proxy de aplicación Web se colocarán en la red Perimetral y se permite el acceso de solo TCP/443 entre la red Perimetral y la subred interna.
* **Los equilibradores de carga**: Para garantizar una alta disponibilidad de servidores de AD FS y Proxy de aplicación Web, se recomienda usar un equilibrador de carga interno para servidores de AD FS y Azure Load Balancer para los servidores Proxy de aplicación Web.
* **Conjuntos de disponibilidad**: Para proporcionar redundancia a la implementación de AD FS, se recomienda agrupar dos o más máquinas virtuales en un conjunto de disponibilidad para cargas de trabajo similares. Esta configuración garantiza que durante un evento de mantenimiento planeado o no planeado, al menos una máquina virtual estará disponible
* **Las cuentas de almacenamiento**: Se recomienda tener dos cuentas de almacenamiento. Tener una cuenta de almacenamiento única puede llevar a la creación de un único punto de error y puede provocar la implementación esté disponible en un escenario poco probable que la cuenta de almacenamiento deja de funcionar. Dos cuentas de almacenamiento que le ayudarán a asociar una cuenta de almacenamiento para cada línea del error.
* **Segregación de la red**:  Servidores Proxy de aplicación Web deben implementarse en una red Perimetral independiente. Puede dividir una red virtual en dos subredes y, a continuación, implementar los servidores Proxy de aplicación Web en una subred aislada. Puede configurar la configuración del grupo de seguridad de red para cada subred y permitir solo la comunicación necesaria entre las dos subredes. Se proporcionan más detalles por el siguiente escenario de implementación

## <a name="steps-to-deploy-ad-fs-in-azure"></a>Pasos para implementar AD FS en Azure
Los pasos mencionados en esta sección describen las directrices para implementar la siguiente infraestructura de AD FS en Azure.

### <a name="1-deploying-the-network"></a>1. Implementación de la red
Como se describió anteriormente, puede ser cree dos subredes en una única red virtual o bien crear dos redes virtuales completamente diferentes (VNet). En este artículo se centrará en la implementación de una única red virtual y dividirla en dos subredes. Actualmente, esto es un enfoque más sencillo como dos redes virtuales independientes requerirían una red virtual a puerta de enlace de red virtual para las comunicaciones.

**1.1 Creación de red virtual**

![Crear red virtual](./media/how-to-connect-fed-azure-adfs/deploynetwork1.png)

En el portal de Azure, seleccionar red virtual y se pueden implementar la red virtual y una subred inmediatamente con un solo clic. La subred INT también se define y está ahora lista para las máquinas virtuales que se va a agregar.
El siguiente paso es agregar otra subred a la red, es decir, la subred de la red Perimetral. Para crear la subred Perimetral, simplemente

* Seleccione la red recién creada.
* En las propiedades, seleccione la subred
* En la subred de panel, haga clic en el botón Agregar
* Proporcione la información de espacio de nombre y la dirección de subred para crear la subred

![Subred](./media/how-to-connect-fed-azure-adfs/deploynetwork2.png)

![Subred DMZ](./media/how-to-connect-fed-azure-adfs/deploynetwork3.png)

**1.2. Creación de grupos de seguridad de red**

Un grupo de seguridad de red (NSG) contiene una lista de reglas de lista de Control de acceso (ACL) que permiten o deniegan el tráfico de red a las instancias de máquina virtual en una red Virtual. Los NSG se pueden asociar con subredes o las instancias de máquina virtual individuales dentro de esa subred. Cuando un NSG está asociado con una subred, las reglas de ACL se aplican a todas las instancias de máquina virtual en esa subred.
Para esta guía, crearemos dos NSG: uno para una red interna y una red Perimetral. Denominará NSG_INT y NSG_DMZ respectivamente.

![Create NSG](./media/how-to-connect-fed-azure-adfs/creatensg1.png)

Después de crea el NSG, habrá 0 reglas de salida de entrada y de 0. Una vez que los roles de los servidores respectivos están instalados y funcionales, se pueden realizar las reglas entrantes y salientes según el nivel de seguridad deseado.

![Inicialización de GIT](./media/how-to-connect-fed-azure-adfs/nsgint1.png)

Después de crea el NSG, asocie NSG_INT a la subred INT y NSG_DMZ a la subred DMZ. Una captura de pantalla de ejemplo se indica a continuación:

![Configurar NSG](./media/how-to-connect-fed-azure-adfs/nsgconfigure1.png)

* Haga clic en subredes para abrir el panel subredes
* Seleccione la subred para asociar el NSG 

Después de la configuración, el panel subredes debe ser similar al siguiente:

![Subredes después GSN](./media/how-to-connect-fed-azure-adfs/nsgconfigure2.png)

**1.3. Crear la conexión a local**

Se necesita una conexión en el entorno local con el fin de implementar el controlador de dominio (DC) en azure. Azure ofrece varias opciones de conectividad para conectar su infraestructura local a la infraestructura de Azure.

* Punto a sitio
* Red virtual de sitio a sitio
* ExpressRoute

Se recomienda usar ExpressRoute. ExpressRoute le permite crear conexiones privadas entre centros de datos y la infraestructura local o en un entorno de coubicación. Las conexiones ExpressRoute no pasan a través de Internet pública. Ofrecen más confiabilidad, velocidades más rápidas, latencias más bajas y mayor seguridad que las conexiones habituales a través de Internet.
Aunque se recomienda usar ExpressRoute, puede elegir cualquier método de conexión más adecuado para su organización. Para obtener más información acerca de ExpressRoute y las diversas opciones de conectividad con ExpressRoute, lea [información técnica de ExpressRoute](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2. Crear cuentas de almacenamiento
Con el fin de mantener la alta disponibilidad y evitar la dependencia de una única cuenta de almacenamiento, puede crear dos cuentas de almacenamiento. Dividir las máquinas en cada conjunto de disponibilidad en dos grupos y, a continuación, asigne una cuenta de almacenamiento independiente a cada grupo.

![Crear cuentas de almacenamiento](./media/how-to-connect-fed-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. Crear conjuntos de disponibilidad
Para cada rol (DC/AD FS y WAP), crear conjuntos de disponibilidad que va a contener 2 máquinas como mínimo. Esto le ayudará a lograr una mayor disponibilidad para cada rol. Al crear la disponibilidad de conjuntos, es esencial para decidir cuál es el siguiente:

* **Dominios de error**: Máquinas virtuales en el mismo dominio de error comparten la misma fuente de alimentación y el conmutador de red físico. Se recomienda un mínimo de 2 dominios de error. El valor predeterminado es 3 y puede dejarlo así para esta implementación.
* **Dominios de actualización**: Las máquinas que pertenezcan al mismo dominio de actualización se reinician juntas durante una actualización. Desea tener como mínimo 2 dominios de actualización. El valor predeterminado es 5 y puede dejarlo así para esta implementación.

![Conjuntos de disponibilidad](./media/how-to-connect-fed-azure-adfs/availabilityset1.png)

Crear los siguientes conjuntos de disponibilidad

| Conjunto de disponibilidad | Rol | Dominios de error | Dominios de actualización |
|:---:|:---:|:---:|:--- |
| contosodcset |DC/ADFS |3 |5 |
| contosowapset |WAP |3 |5 |

### <a name="4-deploy-virtual-machines"></a>4. Implementar máquinas virtuales
El siguiente paso es implementar las máquinas virtuales que hospedarán los distintos roles en su infraestructura. Se recomienda un mínimo de dos máquinas en cada conjunto de disponibilidad. Cree cuatro máquinas virtuales para la implementación básica.

| Machine | Rol | Subred | Conjunto de disponibilidad | Cuenta de almacenamiento | Dirección IP |
|:---:|:---:|:---:|:---:|:---:|:---:|
| contosodc1 |DC/ADFS |INT |contosodcset |contososac1 |Estático |
| contosodc2 |DC/ADFS |INT |contosodcset |contososac2 |Estático |
| contosowap1 |WAP |DMZ |contosowapset |contososac1 |Estático |
| contosowap2 |WAP |DMZ |contosowapset |contososac2 |Estático |

Como habrá observado, se ha especificado ningún NSG. Esto es porque azure le permite usar NSG en el nivel de subred. A continuación, puede controlar el tráfico de red de máquina utilizando el NSG individual asociado en la subred, o bien el objeto NIC. Obtenga más información sobre [¿qué es un grupo de seguridad de red (NSG)](https://aka.ms/Azure/NSG).
Se recomienda la dirección IP estática si está administrando el DNS. Puede utilizar DNS de Azure y en su lugar en los registros DNS para el dominio, consulte las nuevas máquinas por su FQDN de Azure.
El panel de la máquina virtual debe ser similar al siguiente una vez completada la implementación:

![Máquinas virtuales implementadas](./media/how-to-connect-fed-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5. Configurar el controlador de dominio o servidores de AD FS
 Para autenticar cualquier solicitud entrante, AD FS deberá ponerse en contacto con el controlador de dominio. Para evitar el costoso recorrido desde Azure a controlador de dominio local para la autenticación, se recomienda implementar una réplica del controlador de dominio en Azure. Para lograr una alta disponibilidad, se recomienda crear un conjunto de disponibilidad de al menos 2 controladores de dominio.

| Controlador de dominio | Rol | Cuenta de almacenamiento |
|:---:|:---:|:---:|
| contosodc1 |Réplica |contososac1 |
| contosodc2 |Réplica |contososac2 |

* Promueva a los dos servidores como controladores de dominio de réplica con DNS
* Configurar los servidores de AD FS mediante la instalación del rol de AD FS mediante el administrador del servidor.

### <a name="6-deploying-internal-load-balancer-ilb"></a>6. Implementación de equilibrador de carga interno (ILB)
**6.1. Creación del ILB**

Para implementar un ILB, seleccione equilibradores de carga en Azure portal y haga clic en Agregar (+).

> [!NOTE]
> Si no ve **equilibradores de carga** en el menú, haga clic en **examinar** en la parte inferior izquierda del portal y desplácese hasta que vea **equilibradores de carga**.  A continuación, haga clic en la estrella amarilla para agregarlo al menú. Ahora seleccione el nuevo icono del equilibrador de carga para abrir el panel para empezar la configuración del equilibrador de carga.
> 
> 

![Examinar el equilibrador de carga](./media/how-to-connect-fed-azure-adfs/browseloadbalancer.png)

* **Nombre**: Asigne un nombre adecuado al equilibrador de carga
* **Esquema**: Dado que este equilibrador de carga se colocará delante de los servidores de AD FS y está destinada a solo las conexiones de red interna, seleccione "Interno"
* **Red virtual**: Elija la red virtual donde va a implementar AD FS
* **Subnet**: Elija aquí la subred interna
* **Asignación de direcciones IP**: Estático

![Equilibrador de carga interno](./media/how-to-connect-fed-azure-adfs/ilbdeployment1.png)

Tras hacer clic en crear y el ILB se implementa, debería ver en la lista de equilibradores de carga:

![Los equilibradores de carga después del ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment2.png)

Siguiente paso es configurar el grupo de back-end y el sondeo de back-end.

**6.2. Configurar el grupo de back-end ILB**

Seleccione el ILB recién creado en el panel equilibradores de carga. Se abrirá el panel de configuración. 

1. Seleccione los grupos de back-end en el panel de configuración
2. En el panel grupo de servidores back-end para agregar, haga clic en Agregar máquina virtual
3. Aparecerá un panel donde puede elegir el conjunto de disponibilidad
4. Elija el conjunto de disponibilidad de AD FS

![Configurar el grupo de back-end ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment3.png)

**6.3. Configuración del sondeo**

En el panel de configuración de ILB, seleccione los sondeos de estado.

1. Haga clic en Agregar
2. Proporcione detalles de sondeo de una. **Nombre**: Nombre del sondeo b. **Protocolo**: C HTTP. **Puerto**: 80 (HTTP) d. **Ruta de acceso**: / adfs/probe e. **Intervalo**: 5 (valor predeterminado): este es el intervalo en el que el ILB comprobará las máquinas en el grupo de back-end f. **Límite de umbral incorrecto**: 2 (valor predeterminado): este es el umbral de errores de sondeo consecutivos después del cual el ILB declara que una máquina en el grupo de back-end no responde y dejará de enviarle tráfico.

![Configuración de sondeo ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment4.png)

Estamos usando el punto de conexión/ADFS/Probe creado explícitamente para las comprobaciones de mantenimiento en un entorno de AD FS donde no se puede producir una comprobación de ruta de acceso completa de HTTPS.  Esto es mucho mejor que una comprobación básica de puerto 443, que no reflejan con precisión el estado de una implementación de AD FS modernas.  Encontrará más información sobre esto en https://blogs.technet.microsoft.com/applicationproxyblog/2014/10/17/hardware-load-balancer-health-checks-and-web-application-proxy-ad-fs-2012-r2/.

**6.4. Creación de reglas de equilibrio de carga**

Con el fin de equilibrar el tráfico de forma eficaz, el ILB debe configurarse con reglas de equilibrio de carga. Con el fin de crear una regla de equilibrio de carga 

1. Seleccionar regla desde el panel de configuración del ILB de equilibrio de carga
2. Haga clic en Agregar en el panel reglas de equilibrio de carga
3. En el panel reglas de equilibrio de carga de agregar una. **Nombre**: Proporcione un nombre para la regla b. **Protocolo**: Seleccione TCP c. **Puerto**: 443 d. **Puerto back-end**: 443 e. **Grupo back-end**: Seleccione el grupo que creó para que el clúster de AD FS f anterior. **Sondeo**: Seleccione el sondeo que creó anteriormente para servidores de AD FS

![Configurar reglas de equilibrio de carga](./media/how-to-connect-fed-azure-adfs/ilbdeployment5.png)

**6.5. Actualización de DNS con el ILB**

Vaya a su servidor DNS y cree un registro CNAME para el ILB. Debe ser el CNAME para el servicio de federación con la dirección IP que apunta a la dirección IP del ILB. Por ejemplo, si la dirección DIP de ILB es 10.3.0.8 y el servicio de federación instalado es fs.contoso.com, a continuación, cree un registro CNAME para fs.contoso.com apuntando a 10.3.0.8.
Así se asegurará de que todas las comunicaciones relativas a fs.contoso.com terminen en el ILB y se enruten correctamente.

### <a name="7-configuring-the-web-application-proxy-server"></a>7. Configuración del servidor Proxy de aplicación Web
**7.1. Configuración de los servidores Proxy de aplicación Web para acceder a servidores de AD FS**

Con el fin de garantizar que los servidores Proxy de aplicación Web puedan llegar a los servidores de AD FS detrás del ILB, cree un registro en %systemroot%\system32\drivers\etc\hosts para el ILB. Tenga en cuenta que el nombre distintivo (DN) debe ser el nombre del servicio de federación, por ejemplo, fs.contoso.com. Y la entrada IP debe ser la dirección de IP del ILB (10.3.0.8 como en el ejemplo).

**7.2. Instalar el rol de Proxy de aplicación Web**

Después de asegurarse de que los servidores Proxy de aplicación Web son capaces de alcanzar los servidores de AD FS detrás del ILB, a continuación puede instalar a los servidores Proxy de aplicación Web. Servidores Proxy de aplicación Web no necesitan estar unidos al dominio. Instalar los roles de Proxy de aplicación Web en los dos servidores Proxy de aplicación Web, seleccione el rol de acceso remoto. El administrador del servidor le guiará para completar la instalación de WAP.
Para obtener más información sobre cómo implementar WAP, lea [instalar y configurar el servidor de Proxy de aplicación Web](https://technet.microsoft.com/library/dn383662.aspx).

### <a name="8--deploying-the-internet-facing-public-load-balancer"></a>8.  Implementar el equilibrador de carga (público) de Internet
**8.1.  Crear el equilibrador de carga (público) de Internet.**

En el portal de Azure, seleccione equilibradores de carga y, a continuación, haga clic en Agregar. En el panel de equilibrador de carga de crear, escriba la siguiente información

1. **Nombre**: Nombre del equilibrador de carga
2. **Esquema**: Público: esta opción indica a Azure que este equilibrador de carga necesitará una dirección pública.
3. **Dirección IP**: Crear una nueva dirección IP (dinámica)

![Equilibrador de carga accesible desde Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment1.png)

Después de la implementación, el equilibrador de carga aparecerán en la lista de equilibradores de carga.

![Lista de equilibradores de carga](./media/how-to-connect-fed-azure-adfs/elbdeployment2.png)

**8.2. Asignar una etiqueta DNS a la dirección IP pública**

Haga clic en la entrada del equilibrador de carga recién creado en el panel equilibradores de carga para que aparezca el panel de configuración. Siga estos pasos para configurar la etiqueta DNS para la dirección IP pública:

1. Haga clic en la dirección IP pública. Esto abrirá el panel de la dirección IP pública y su configuración
2. Haga clic en configuración
3. Proporcione una etiqueta DNS. Esto se convertirá en la etiqueta DNS pública que puede tener acceso desde cualquier lugar, por ejemplo, contosofs.westus.cloudapp.azure.com. Puede agregar una entrada en el DNS externo para el servicio de federación (por ejemplo, fs.contoso.com) que se resuelve en la etiqueta DNS del equilibrador de carga externo (contosofs.westus.cloudapp.azure.com).

![Configurar el equilibrador de carga accesible desde internet](./media/how-to-connect-fed-azure-adfs/elbdeployment3.png) 

![Configurar el equilibrador de carga (DNS) de internet.](./media/how-to-connect-fed-azure-adfs/elbdeployment4.png)

**8.3. Configurar el grupo back-end de equilibrador de carga accesible desde Internet (público)** 

Siga los mismos pasos que al crear el equilibrador de carga interno para configurar el grupo de back-end de equilibrador de carga de accesible desde Internet (público) como el conjunto de disponibilidad para los servidores WAP. Por ejemplo, contosowapset.

![Configurar el grupo de back-end del equilibrador de carga accesible desde Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment5.png)

**8.4. Configuración de sondeo**

Siga los mismos pasos que al configurar el equilibrador de carga interno para configurar el sondeo para el grupo de back-end de los servidores WAP.

![Configuración de sondeo del equilibrador de carga accesible desde Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment6.png)

**8.5. Creación de reglas de equilibrio de carga**

Siga los mismos pasos que en el ILB para configurar el equilibrio de carga regla para TCP 443.

![Configurar reglas de equilibrio del equilibrador de carga accesible desde Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment7.png)

### <a name="9-securing-the-network"></a>9. Protección de la red
**9.1. Protección de la subred interna**

En general, necesita las siguientes reglas para proteger eficazmente la subred interna (en el orden que figura a continuación)

| Regla | Descripción | Flujo |
|:--- |:--- |:---:|
| AllowHTTPSFromDMZ |Permitir la comunicación HTTPS de la red Perimetral |Entrante |
| DenyInternetOutbound |Sin acceso a internet |Saliente |

![Reglas de acceso INT (entrantes)](./media/how-to-connect-fed-azure-adfs/nsg_int.png)

**9.2. Protección de la subred DMZ**

| Regla | Descripción | Flujo |
|:--- |:--- |:---:|
| AllowHTTPSFromInternet |Permite HTTPS de internet a la red Perimetral |Entrante |
| DenyInternetOutbound |Se bloquea nada, excepto HTTPS a internet |Saliente |

![Reglas de acceso EXT (entrantes)](./media/how-to-connect-fed-azure-adfs/nsg_dmz.png)

> [!NOTE]
> Si el usuario del cliente un certificado de autenticación (autenticación de clientTLS mediante X509 certificados de usuario) se requiere, a continuación, AD FS requiere TCP habilitarse el puerto 49443 para el acceso de entrada.
> 
> 

### <a name="10-test-the-ad-fs-sign-in"></a>10. Probar el inicio de sesión de AD FS
La forma más sencilla es probar AD FS es mediante el uso de la página IdpInitiatedSignon.aspx. Para poder hacer, que es necesario para habilitar IdpInitiatedSignOn en las propiedades de AD FS. Siga los pasos siguientes para comprobar la configuración de AD FS

1. Ejecute el siguiente cmdlet en el servidor de AD FS, con PowerShell, habilítela.
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true 
2. Desde cualquier equipo externo, obtener acceso a https:\//adfs-server.contoso.com/adfs/ls/IdpInitiatedSignon.aspx.  
3. Debería ver la página de AD FS como aparece a continuación:

![Página de inicio de sesión de prueba](./media/how-to-connect-fed-azure-adfs/test1.png)

En el inicio de sesión correcto, le proporcionará un mensaje de confirmación tal como se muestra a continuación:

![Prueba correcta](./media/how-to-connect-fed-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Plantilla para la implementación de AD FS en Azure
La plantilla implementa una instalación de 6 equipos, 2 para los controladores de dominio, AD FS y WAP.

[AD FS en la plantilla de implementación de Azure](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

Puede usar una red virtual existente o crear una nueva red virtual durante la implementación de esta plantilla. Los distintos parámetros disponibles para personalizar la implementación se muestran a continuación con la descripción del uso del parámetro en el proceso de implementación. 

| Parámetro | Descripción |
|:--- |:--- |
| Location |La región para implementar los recursos en, por ejemplo, este de EE. |
| StorageAccountType |El tipo de la cuenta de almacenamiento creada |
| VirtualNetworkUsage |Indica si una nueva red virtual se creará o use uno existente |
| VirtualNetworkName |El nombre de la red Virtual para crear, obligatorio en el uso de red virtual nueva o existente |
| VirtualNetworkResourceGroupName |Especifica el nombre del grupo de recursos donde reside la red virtual existente. Cuando se usa una red virtual existente, esto se convierte en un parámetro obligatorio para que la implementación pueda encontrar el identificador de la red virtual existente |
| VirtualNetworkAddressRange |El intervalo de direcciones de la nueva red virtual, obligatorio si se crea una nueva red virtual |
| InternalSubnetName |El nombre de la subred interna, obligatorio en ambas opciones de uso de la red virtual (nuevas o existentes) |
| InternalSubnetAddressRange |El intervalo de direcciones de la subred interna, que contiene los servidores controladores de dominio y ADFS, obligatorios si se crea una nueva red virtual. |
| DMZSubnetAddressRange |El intervalo de direcciones de la subred dmz, que contiene los servidores proxy de aplicación de Windows, obligatorios si se crea una nueva red virtual. |
| DMZSubnetName |El nombre de la subred interna, obligatorio en ambas opciones de uso de la red virtual (nuevas o existentes). |
| ADDC01NICIPAddress |La dirección IP interna del primer controlador de dominio, esta dirección IP se asignarán estáticamente al controlador de dominio y debe ser una dirección ip válida dentro de la subred interna |
| ADDC02NICIPAddress |La dirección IP interna del segundo controlador de dominio, esta dirección IP se asignarán estáticamente al controlador de dominio y debe ser una dirección ip válida dentro de la subred interna |
| ADFS01NICIPAddress |La dirección IP interna del primer servidor de ADFS, esta dirección IP se asignarán estáticamente al servidor de ADFS y debe ser una dirección ip válida dentro de la subred interna |
| ADFS02NICIPAddress |La dirección IP interna del segundo servidor ADFS, esta dirección IP se asignarán estáticamente al servidor de ADFS y debe ser una dirección ip válida dentro de la subred interna |
| WAP01NICIPAddress |La dirección IP interna del primer servidor WAP, esta dirección IP se asignarán estáticamente al servidor WAP y debe ser una dirección ip válida dentro de la subred DMZ |
| WAP02NICIPAddress |La dirección IP interna del segundo servidor WAP, esta dirección IP se asignarán estáticamente al servidor WAP y debe ser una dirección ip válida dentro de la subred DMZ |
| ADFSLoadBalancerPrivateIPAddress |La dirección IP interna del equilibrador de carga ADFS, esta dirección IP se asignarán estáticamente al equilibrador de carga y debe ser una dirección ip válida dentro de la subred interna |
| ADDCVMNamePrefix |Prefijo de nombre de máquina virtual para los controladores de dominio |
| ADFSVMNamePrefix |Prefijo de nombre de la máquina virtual de servidores de AD FS |
| WAPVMNamePrefix |Prefijo de nombre de máquina virtual para los servidores WAP |
| ADDCVMSize |El tamaño de máquina virtual de los controladores de dominio |
| ADFSVMSize |El tamaño de máquina virtual de los servidores ADFS |
| WAPVMSize |El tamaño de máquina virtual de los servidores WAP |
| AdminUserName |El nombre del administrador local de las máquinas virtuales |
| AdminPassword |La contraseña de la cuenta de administrador local de las máquinas virtuales |

## <a name="additional-resources"></a>Recursos adicionales
* [Conjuntos de disponibilidad](https://aka.ms/Azure/Availability) 
* [Equilibrador de carga de Azure](https://aka.ms/Azure/ILB)
* [Equilibrador de carga interno](https://aka.ms/Azure/ILB/Internal)
* [Equilibrador de carga accesible desde Internet](https://aka.ms/Azure/ILB/Internet)
* [Cuentas de almacenamiento](https://aka.ms/Azure/Storage)
* [Redes virtuales de Azure](https://aka.ms/Azure/VNet)
* [AD FS y vínculos de Proxy de aplicación Web](https://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Pasos siguientes
* [Integración de las identidades locales con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)
* [Configuración y administración de AD FS con Azure AD Connect](/azure/active-directory/hybrid/how-to-connect-fed-whatis)
* [Implementación de alta disponibilidad entre regiones geográficas de AD FS en Azure con Azure Traffic Manager](active-directory-adfs-in-azure-with-azure-traffic-manager.md)
