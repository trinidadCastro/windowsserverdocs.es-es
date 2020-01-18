---
title: Network Controller Security (Seguridad del controlador de red)
description: Puede usar este tema para aprender a configurar la seguridad de todas las comunicaciones entre la controladora de red y otro software y dispositivos.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: 54a8b9490fdf83d04c6b69fa88f4e8beca4f703a
ms.sourcegitcommit: 51e0b575ef43cd16b2dab2db31c1d416e66eebe8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76259070"
---
# <a name="secure-the-network-controller"></a>Proteger la controladora de red

En este tema, aprenderá a configurar la seguridad de todas las comunicaciones entre la [controladora de red](../technologies/network-controller/network-controller.md) y otro software y dispositivos. 

Las rutas de comunicación que puede proteger incluyen la comunicación de Northbound en el plano de administración, la comunicación de clústeres entre máquinas virtuales de controladora de red \(máquinas virtuales\) en un clúster y la comunicación de southbound en el plano de datos.

1. **Comunicación de Northbound**. La controladora de red se comunica en el plano de administración con el software de administración compatible con SDN\-, como Windows PowerShell y System Center Virtual Machine Manager \(SCVMM\). Estas herramientas de administración proporcionan la capacidad de definir la Directiva de red y de crear un estado de objetivo para la red, en la que puede comparar la configuración real de la red para hacer que la configuración real sea paridad con el estado del objetivo.

2. **Comunicación del clúster de controladora de red**. Cuando se configuran tres o más máquinas virtuales como nodos de clúster de controladora de red, estos nodos se comunican entre sí. Esta comunicación puede estar relacionada con la sincronización y la replicación de datos entre nodos, o la comunicación específica entre los servicios de la controladora de red.

3.  **Comunicación de southbound**. La controladora de red se comunica en el plano de datos con la infraestructura de SDN y otros dispositivos, como equilibradores de carga de software, puertas de enlace y máquinas host. Puede usar la controladora de red para configurar y administrar estos dispositivos southbound de modo que mantengan el estado del objetivo que ha configurado para la red.


## <a name="northbound-communication"></a>Comunicación de Northbound

La controladora de red admite la autenticación, autorización y cifrado para la comunicación de Northbound. En las secciones siguientes se proporciona información sobre cómo configurar estas opciones de seguridad.

### <a name="authentication"></a>Autenticación

Al configurar la autenticación para la comunicación de Northbound de controladora de red, permite que los nodos de clúster de la controladora de red y los clientes de administración comprueben la identidad del dispositivo con el que se están comunicando.

La controladora de red admite los tres modos de autenticación entre los clientes de administración y los nodos de controladora de red.

>[!Note]
>Si va a implementar la controladora de red con System Center Virtual Machine Manager, solo se admite el modo **Kerberos** .

1. **Kerberos**. Use la autenticación Kerberos al unir el cliente de administración de y todos los nodos de clúster de controladora de red a un dominio de Active Directory. El dominio Active Directory debe tener cuentas de dominio usadas para la autenticación.

2. **X509**. Use X509 para la autenticación basada en\-de certificados para los clientes de administración que no estén Unidos a un dominio Active Directory. Debe inscribir los certificados en todos los nodos de clúster de la controladora de red y los clientes de administración. Además, todos los nodos y clientes de administración deben confiar en los certificados de todos los demás.

3. **Ninguno**. No use ninguno para realizar pruebas en un entorno de prueba y, por lo tanto, no se recomienda para su uso en un entorno de producción. Al elegir este modo, no se realiza ninguna autenticación entre los nodos y los clientes de administración.

Puede configurar el modo de autenticación para la comunicación de Northbound mediante el comando de Windows PowerShell **[install-NetworkController](https://docs.microsoft.com/powershell/module/networkcontroller/install-networkcontroller)** con el parámetro _ClientAuthentication_ . 


### <a name="authorization"></a>Autorización

Al configurar la autorización para la comunicación de Northbound de controladora de red, permite a los nodos de clúster de la controladora de red y a los clientes de administración comprobar que el dispositivo con el que se están comunicando es de confianza y tiene permiso para participar en el comunicaci.

Use los siguientes métodos de autorización para cada uno de los modos de autenticación admitidos por la controladora de red.

1.  **Kerberos**. Cuando se usa el método de autenticación Kerberos, se definen los usuarios y equipos autorizados para comunicarse con la controladora de red mediante la creación de un grupo de seguridad en Active Directory y, a continuación, la adición de los usuarios y equipos autorizados al grupo. Puede configurar el controlador de red para usar el grupo de seguridad para la autorización mediante el parámetro _ClientSecurityGroup_ del comando **[install-NetworkController](https://docs.microsoft.com/powershell/module/networkcontroller/install-networkcontroller)** de Windows PowerShell. Después de instalar el controlador de red, puede cambiar el grupo de seguridad mediante el comando **[set-NetworkController](https://docs.microsoft.com/powershell/module/networkcontroller/Set-NetworkController)** con el parámetro _-ClientSecurityGroup_. Si usa SCVMM, debe proporcionar el grupo de seguridad como un parámetro durante la implementación.

2.  **X509**. Cuando se usa el método de autenticación X509, la controladora de red solo acepta solicitudes de clientes de administración cuyas huellas digitales de certificado son conocidas para la controladora de red. Puede configurar estas huellas digitales mediante el parámetro _ClientCertificateThumbprint_ del comando **[install-NetworkController](https://docs.microsoft.com/powershell/module/networkcontroller/install-networkcontroller)** de Windows PowerShell. Puede agregar otras huellas digitales de cliente en cualquier momento mediante el comando **[set-NetworkController](https://docs.microsoft.com/powershell/module/networkcontroller/Set-NetworkController)** .

3.  **Ninguno**. Al elegir este modo, no se realiza ninguna autenticación entre los nodos y los clientes de administración. No use ninguno para realizar pruebas en un entorno de prueba y, por lo tanto, no se recomienda para su uso en un entorno de producción. 


### <a name="encryption"></a>Cifrado

La comunicación de Northbound usa Capa de sockets seguros \(\) SSL para crear un canal cifrado entre los clientes de administración y los nodos de controladora de red. El cifrado SSL para la comunicación de Northbound incluye los siguientes requisitos:

- Todos los nodos de la controladora de red deben tener un certificado idéntico que incluya la autenticación del servidor y la autenticación del cliente en las extensiones de uso mejorado de clave \(EKU\). 

- El URI que usan los clientes de administración para comunicarse con el controlador de red debe ser el nombre de sujeto del certificado. El nombre del firmante del certificado debe contener el nombre de dominio completo (FQDN) o la dirección IP del punto de conexión de REST de la controladora de red.

- Si los nodos de controlador de red se encuentran en subredes diferentes, el nombre de sujeto de sus certificados debe ser el mismo que el valor usado para el parámetro _RestName_ en el comando **install-NetworkController** de Windows PowerShell. 

- Todos los clientes de administración deben confiar en el certificado SSL.


### <a name="ssl-certificate-enrollment-and-configuration"></a>Inscripción y configuración de certificados SSL

Debe inscribir manualmente el certificado SSL en los nodos de la controladora de red.

Una vez inscrito el certificado, puede configurar el controlador de red para usar el certificado con el parámetro **-ServerCertificate** del comando **install-NetworkController** de Windows PowerShell. Si ya ha instalado el controlador de red, puede actualizar la configuración en cualquier momento mediante el comando **set-NetworkController** .

>[!NOTE]
>Si está utilizando SCVMM, debe agregar el certificado como un recurso de biblioteca. Para obtener más información, consulte [configuración de una controladora de red de Sdn en el tejido de VMM](https://docs.microsoft.com/system-center/vmm/sdn-controller).

## <a name="network-controller-cluster-communication"></a>Comunicación del clúster de controladora de red

La controladora de red admite la autenticación, autorización y cifrado para la comunicación entre los nodos de controladora de red. La comunicación se realiza a través de [Windows Communication Foundation](https://docs.microsoft.com/dotnet/framework/wcf/whats-wcf) \(\) WCF y TCP.

Puede configurar este modo con el parámetro **ClusterAuthentication** del comando **install-NetworkControllerCluster** de Windows PowerShell. 

Para obtener más información, consulte [install-NetworkControllerCluster](https://docs.microsoft.com/powershell/module/networkcontroller/install-networkcontrollercluster).

### <a name="authentication"></a>Autenticación

Al configurar la autenticación de la comunicación del clúster de controladora de red, permite que los nodos de clúster de la controladora de red comprueben la identidad de los demás nodos con los que se están comunicando.

La controladora de red admite los siguientes tres modos de autenticación entre los nodos de controladora de red.

>[!NOTE]
>Si implementa la controladora de red mediante SCVMM, solo se admite el modo **Kerberos** .

1. **Kerberos**. Puede usar la autenticación Kerberos cuando todos los nodos de clúster de la controladora de red se unen a un dominio de Active Directory, con las cuentas de dominio que se usan para la autenticación.

2. **X509**. X509 es la autenticación basada en certificados\-. Puede usar la autenticación X509 cuando los nodos del clúster de la controladora de red no están Unidos a un dominio de Active Directory. Para usar X509, debe inscribir certificados en todos los nodos de clúster de la controladora de red y todos los nodos deben confiar en los certificados. Además, el nombre de sujeto del certificado que está inscrito en cada nodo debe ser el mismo que el nombre DNS del nodo.

3. **Ninguno**. Al elegir este modo, no se realiza ninguna autenticación entre los nodos de la controladora de red. Este modo se proporciona solo para fines de prueba y no se recomienda para su uso en un entorno de producción.

### <a name="authorization"></a>Autorización

Al configurar la autorización para la comunicación del clúster de controladora de red, permite que los nodos del clúster de la controladora de red comprueben que los nodos con los que se están comunicando son de confianza y tienen permiso para participar en la comunicación.

Para cada uno de los modos de autenticación admitidos por la controladora de red, se usan los siguientes métodos de autorización.

1. **Kerberos**. Los nodos de controladora de red aceptan solicitudes de comunicación solo de otras cuentas de equipo de la controladora de red. Puede configurar estas cuentas al implementar la controladora de red mediante el parámetro **Name** del comando de Windows PowerShell [New-NetworkControllerNodeObject](https://docs.microsoft.com/powershell/module/networkcontroller/new-networkcontrollernodeobject) .

2. **X509**. Los nodos de controladora de red aceptan solicitudes de comunicación solo de otras cuentas de equipo de la controladora de red. Puede configurar estas cuentas al implementar la controladora de red mediante el parámetro **Name** del comando de Windows PowerShell [New-NetworkControllerNodeObject](https://docs.microsoft.com/powershell/module/networkcontroller/new-networkcontrollernodeobject) .

3. **Ninguno**. Al elegir este modo, no se realiza ninguna autorización entre los nodos de la controladora de red. Este modo se proporciona solo para fines de prueba y no se recomienda para su uso en un entorno de producción.

### <a name="encryption"></a>Cifrado

La comunicación entre los nodos de la controladora de red se cifra mediante el cifrado de nivel de transporte WCF. Esta forma de cifrado se utiliza cuando los métodos de autenticación y autorización son certificados de Kerberos o X509. Para obtener más información, vea los temas siguientes:

- [Cómo proteger un servicio con credenciales de Windows](https://docs.microsoft.com/dotnet/framework/wcf/how-to-secure-a-service-with-windows-credentials)
- [Cómo: proteger un servicio con certificados X. 509](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-secure-a-service-with-an-x-509-certificate).

## <a name="southbound-communication"></a>Comunicación de southbound

La controladora de red interactúa con distintos tipos de dispositivos para la comunicación de southbound. Estas interacciones usan protocolos diferentes. Por este motivo, hay diferentes requisitos para la autenticación, la autorización y el cifrado según el tipo de dispositivo y el protocolo que usa la controladora de red para comunicarse con el dispositivo.

En la tabla siguiente se proporciona información acerca de la interacción de la controladora de red con distintos dispositivos southbound.

| Dispositivo o servicio southbound | Protocolo              | Autenticación usada    |
|---------------------------|-----------------------|------------------------|
| Equilibrador de carga de software    | WCF (MUX), TCP (host) | Certificados           |
| Firewall                  | OVSDB                 | Certificados           |
| Puerta de enlace                   | WinRM                 | Kerberos, certificados |
| Redes virtuales        | OVSDB, WCF            | Certificados           |
| Enrutamiento definido por el usuario      | OVSDB                 | Certificados           |

Para cada uno de estos protocolos, el mecanismo de comunicación se describe en la sección siguiente.

### <a name="authentication"></a>Autenticación

Para la comunicación de southbound, se usan los siguientes protocolos y métodos de autenticación.

1. **WCF/TCP/OVSDB**. Para estos protocolos, la autenticación se realiza mediante certificados X509. Tanto el controlador de red como el equilibrio de carga de software del mismo nivel \(el multiplexor de\) de SLB \(MUX\)los equipos/host presentan sus certificados entre sí para la autenticación mutua. Cada certificado debe ser de confianza para el equipo remoto del mismo nivel.

    Para la autenticación southbound, puede usar el mismo certificado SSL que está configurado para cifrar la comunicación con los clientes de Northbound. También debe configurar un certificado en el MUX de SLB y en los dispositivos de host. El nombre del firmante del certificado debe ser el mismo que el nombre DNS del dispositivo.

2. **WinRM**. Para este protocolo, la autenticación se realiza mediante \(de Kerberos para equipos Unidos a un dominio\) y mediante certificados \(para equipos que no están Unidos a un dominio\).

### <a name="authorization"></a>Autorización

Para la comunicación de southbound, se usan los siguientes protocolos y métodos de autorización.

1. **WCF/TCP**. Para estos protocolos, la autorización se basa en el nombre de sujeto de la entidad del mismo nivel. La controladora de red almacena el nombre DNS del dispositivo del mismo nivel y lo usa para la autorización. Este nombre DNS debe coincidir con el nombre de sujeto del dispositivo en el certificado. Del mismo modo, el certificado de controlador de red debe coincidir con el nombre DNS de la controladora de red almacenado en el dispositivo del mismo nivel.

2. **WinRM**. Si se usa Kerberos, la cuenta de cliente de WinRM debe estar presente en un grupo predefinido en Active Directory o en el grupo de administradores locales en el servidor. Si se usan certificados, el cliente presenta un certificado al servidor que el servidor autoriza mediante el nombre de sujeto/emisor y el servidor usa una cuenta de usuario asignada para realizar la autenticación.

3.  **OVSDB**. No se proporcionó autorización para este protocolo.

### <a name="encryption"></a>Cifrado

Para la comunicación de southbound, se usan los siguientes métodos de cifrado para los protocolos.

1. **WCF/TCP/OVSDB**. Para estos protocolos, el cifrado se realiza mediante el certificado que está inscrito en el cliente o el servidor.

2. **WinRM**. El tráfico WinRM se cifra de forma predeterminada mediante el proveedor de compatibilidad con seguridad de Kerberos \(SSP\). Puede configurar el cifrado adicional, en forma de SSL, en el servidor WinRM.
