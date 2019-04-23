---
title: Network Controller Security (Seguridad del controlador de red)
description: Puede utilizar este tema para aprender a configurar la seguridad de toda la comunicación entre la controladora de red y otros dispositivos y software.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: fccd6ee1ba734d72629264bd5b3bb7d93ddcdb70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884766"
---
# <a name="secure-the-network-controller"></a>Proteger la controladora de red

En este tema, obtendrá información sobre cómo configurar la seguridad de todas las comunicaciones entre [controladora de red](../technologies/network-controller/network-controller.md) y otro software y dispositivos. 

Las rutas de comunicación que se pueden proteger la comunicación de Northbound en el plano de administración, la comunicación entre las máquinas virtuales de controladora de red del clúster de incluir \(máquinas virtuales\) en un clúster y la comunicación de Southbound en los datos plano.

1. **La comunicación de northbound**. Controladora de red se comunica en el plano de administración con SDN\-software de administración compatible con, como Windows PowerShell y System Center Virtual Machine Manager \(SCVMM\). Estas herramientas de administración proporcionan la capacidad para definir la directiva de red y para crear un estado de objetivo de la red, en el que puede comparar la configuración de red real para hacer que la configuración real en paridad con el estado de objetivo.

2. **Comunicación de clúster de controlador de red**. Cuando configura tres o más máquinas virtuales como nodos de clúster de la controladora de red, estos nodos se comunican entre sí. Esta comunicación podría estar relacionada con la sincronización y replicación de datos entre los nodos o comunicación específica entre los servicios de la controladora de red.

3.  **La comunicación de southbound**. Controladora de red se comunica en el plano de datos con la infraestructura de SDN y otros dispositivos como equipos host, las puertas de enlace y equilibradores de carga de software. Puede usar controladora de red para configurar y administrar estos dispositivos southbound para que mantengan el estado de objetivo que ha configurado para la red.


## <a name="northbound-communication"></a>Comunicación de northbound

Controladora de red admite la autenticación, autorización y cifrado para la comunicación de Northbound. Las secciones siguientes proporcionan información sobre cómo configurar estas opciones de seguridad.

### <a name="authentication"></a>Autenticación

Al configurar la autenticación para la comunicación de Northbound de controladora de red, permite que los nodos de clúster de controladora de red y los clientes de administración para comprobar la identidad del dispositivo con el que se están comunicando.

Controladora de red es compatible con los siguientes tres modos de autenticación entre los clientes de administración y los nodos de controladora de red.

>[!Note]
>Si va a implementar la controladora de red con System Center Virtual Machine Manager, solo **Kerberos** se admite el modo.

1. **Kerberos**. Usar la autenticación Kerberos al unir el cliente de administración y todos los nodos de clúster de controladora de red a un dominio de Active Directory. El dominio de Active Directory debe tener cuentas de dominio utilizadas para la autenticación.

2. **X509**. Use X509 certificado\-en función de autenticación para los clientes de administración no está unido a un dominio de Active Directory. Debe inscribir certificados para todos los nodos del clúster de controladora de red y los clientes de administración. Además, todos los nodos y los clientes de administración deben confiar en los demás certificados.

3. **Ninguna**. Use None para las pruebas con fines de en un entorno de prueba y, por lo tanto, no se recomienda para su uso en un entorno de producción. Al elegir este modo, no hay ninguna autenticación realizada entre los nodos y los clientes de administración.

Puede configurar el modo de autenticación para la comunicación de Northbound con el comando de Windows PowerShell **[Install NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** con el _ClientAuthentication_ parámetro. 


### <a name="authorization"></a>Autorización

Cuando se configura la autorización para la comunicación de Northbound de controladora de red, permite que los nodos de clúster de controladora de red y los clientes de administración para comprobar que el dispositivo con el que se están comunicando es de confianza y que tiene permiso para participar en el comunicación.

Utilice los siguientes métodos de autorización para cada uno de los modos de autenticación admitidos por la controladora de red.

1.  **Kerberos**. Cuando se usa el método de autenticación Kerberos, define los usuarios y equipos autorizados para comunicarse con controladora de red mediante la creación de un grupo de seguridad en Active Directory y, a continuación, agregar los usuarios autorizados y equipos al grupo. Puede configurar la controladora de red para usar el grupo de seguridad para la autorización mediante el _ClientSecurityGroup_ parámetro de la **[Install NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** Comando de Windows PowerShell. Después de instalar el controlador de red, puede cambiar el grupo de seguridad mediante la **[conjunto NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** comando con el parámetro _- ClientSecurityGroup_ . Si se usa SCVMM, debe proporcionar el grupo de seguridad como un parámetro durante la implementación.

2.  **X509**. Cuando se utiliza el X509 método de autenticación, controladora de red solo acepta solicitudes de clientes de administración cuyas huellas digitales de certificados se sabe que la controladora de red. Puede configurar estas huellas digitales mediante la _ClientCertificateThumbprint_ parámetro de la **[Install NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** comando de Windows PowerShell. Puede agregar otras las huellas digitales del cliente en cualquier momento mediante el **[conjunto NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** comando.

3.  **Ninguna**. Al elegir este modo, no hay ninguna autenticación realizada entre los nodos y los clientes de administración. Use None para las pruebas con fines de en un entorno de prueba y, por lo tanto, no se recomienda para su uso en un entorno de producción. 


### <a name="encryption"></a>Cifrado

La comunicación de northbound usa capa de Sockets seguros \(SSL\) para crear un canal cifrado entre los clientes de administración y los nodos de controladora de red. El cifrado SSL para la comunicación de Northbound incluye los siguientes requisitos:

- Todos los nodos de controladora de red deben tener un certificado idéntico que incluye los fines de autenticación de servidor y cliente de uso mejorado de clave \(EKU\) extensiones. 

- El URI usado por los clientes de administración para comunicarse con controladora de red debe ser el nombre de sujeto del certificado. El nombre de sujeto del certificado debe contener el nombre de dominio completo (FQDN) o la dirección IP del punto de conexión de REST de controlador de red.

- Si los nodos de controladora de red están en subredes diferentes, el nombre de sujeto de sus certificados debe ser el mismo que el valor utilizado para la _RestName_ parámetro en el **Install NetworkController** Windows Comando de PowerShell. 

- Todos los clientes de administración deben confiar en el certificado SSL.


### <a name="ssl-certificate-enrollment-and-configuration"></a>Configuración y la inscripción de certificados SSL

Debe inscribir manualmente el certificado SSL en los nodos de controladora de red.

Después de que el certificado esté inscrito, puede configurar la controladora de red para usar el certificado con la **- ServerCertificate** parámetro de la **Install NetworkController** comando de Windows PowerShell . Si ya ha instalado la controladora de red, puede actualizar la configuración en cualquier momento mediante el **conjunto NetworkController** comando.

>[!NOTE]
>Si se usa SCVMM, debe agregar el certificado como un recurso de biblioteca. Para obtener más información, consulte [configurar un controlador de red SDN en el tejido de VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

## <a name="network-controller-cluster-communication"></a>Comunicación de clúster de controlador de red

Controladora de red admite la autenticación, autorización y cifrado para la comunicación entre nodos de controladora de red. La comunicación es a través de [Windows Communication Foundation](https://msdn.microsoft.com/library/ms731082.aspx) \(WCF\) y TCP.

Puede configurar este modo con el **ClusterAuthentication** parámetro de la **Install NetworkControllerCluster** comando de Windows PowerShell. 

Para obtener más información, consulte [Install NetworkControllerCluster](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontrollercluster).

### <a name="authentication"></a>Autenticación

Al configurar la autenticación para la comunicación de clúster de controlador de red, permite que los nodos de clúster de controladora de red comprobar la identidad de los otros nodos con el que se están comunicando.

Controladora de red es compatible con los siguientes tres modos de autenticación entre los nodos de controladora de red.

>[!NOTE]
>Si implementa la controladora de red mediante SCVMM, sólo **Kerberos** se admite el modo.

1. **Kerberos**. Puede usar la autenticación Kerberos cuando todos los nodos de clúster de controladora de red están unidos a un dominio de Active Directory con cuentas de dominio usadas para la autenticación.

2. **X509**. X509 es certificado\-autenticación basada en. Puede usar X509 autenticación cuando los nodos de clúster de la controladora de red no están unidos a un dominio de Active Directory. Para usar X509, debe inscribir certificados para todos los nodos de clúster de controladora de red y todos los nodos deben confiar en los certificados. Además, el nombre del sujeto del certificado que está inscrito en cada nodo debe ser el mismo que el nombre DNS del nodo.

3. **Ninguna**. Al elegir este modo, no hay ninguna autenticación realizada entre los nodos de controladora de red. Este modo se proporciona sólo para fines de prueba y no se recomienda para su uso en un entorno de producción.

### <a name="authorization"></a>Autorización

Cuando se configura la autorización para la comunicación de clúster de controlador de red, permite que los nodos de clúster de controladora de red comprobar que los nodos con el que se están comunicando son de confianza y tenga permiso para participar en la comunicación.

Para cada uno de los modos de autenticación admitidos por la controladora de red, se usan los siguientes métodos de autorización.

1. **Kerberos**. Nodos de controladora de red aceptan solicitudes de comunicación solo desde otras cuentas de equipo de la controladora de red. Puede configurar estas cuentas al implementar la controladora de red mediante el uso de la **nombre** parámetro de la [New NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) comando de Windows PowerShell.

2. **X509**. Nodos de controladora de red aceptan solicitudes de comunicación solo desde otras cuentas de equipo de la controladora de red. Puede configurar estas cuentas al implementar la controladora de red mediante el uso de la **nombre** parámetro de la [New NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) comando de Windows PowerShell.

3. **Ninguna**. Al elegir este modo, no hay ninguna autorización que se realiza entre los nodos de controladora de red. Este modo se proporciona sólo para fines de prueba y no se recomienda para su uso en un entorno de producción.

### <a name="encryption"></a>Cifrado

Comunicación entre los nodos de controladora de red se cifra mediante cifrado de nivel de transporte de WCF. Este tipo de cifrado se utiliza cuando los métodos de autenticación y autorización son Kerberos o X509 certificados. Para obtener más información, vea los temas siguientes:

- [Cómo: Proteger un servicio con credenciales de Windows](https://msdn.microsoft.com/library/ms734673.aspx)
- [Cómo: Proteger un servicio con certificados X.509](https://msdn.microsoft.com/library/ms788968.aspx).

## <a name="southbound-communication"></a>Southbound Communication

Controladora de red interactúa con diferentes tipos de dispositivos para la comunicación de Southbound. Estas interacciones utilizan distintos protocolos. Por este motivo, hay diferentes requisitos de autenticación, autorización y cifrado según el tipo de dispositivo y el protocolo utilizado por la controladora de red para comunicarse con el dispositivo.

En la tabla siguiente proporciona información sobre la interacción de la controladora de red con diferentes dispositivos southbound.

| Dispositivo o servicio southbound | Protocolo              | Usa la autenticación    |
|---------------------------|-----------------------|------------------------|
| Equilibrador de carga de software    | WCF (MUX), TCP (Host) | Certificados           |
| Firewall                  | OVSDB                 | Certificados           |
| Puerta de enlace                   | WinRM                 | Kerberos, certificados |
| Red virtual        | OVSDB, WCF            | Certificados           |
| Enrutamiento definido por el usuario      | OVSDB                 | Certificados           |

Para cada uno de estos protocolos, se describe el mecanismo de comunicación en la sección siguiente.

### <a name="authentication"></a>Autenticación

Para la comunicación de Southbound, se usan los siguientes protocolos y métodos de autenticación.

1. **WCF/TCP/OVSDB**. Para estos protocolos, la autenticación se realiza mediante el uso de X509 certificados. Controladora de red y el punto de equilibrio de carga de Software \(SLB\) multiplexor \(MUX \) /máquinas host presentan sus certificados entre sí para la autenticación mutua. Cada certificado debe ser de confianza interlocutor remoto.

    Para la autenticación de southbound, puede usar el mismo certificado SSL configurado para cifrar la comunicación con los clientes de Northbound. También debe configurar un certificado en los dispositivos de host y un SLB MUX. El nombre de sujeto del certificado debe ser igual que el nombre DNS del dispositivo.

2. **WinRM**. Para este protocolo, la autenticación se realiza mediante Kerberos \(Unidos a dominios equipos\) y mediante el uso de certificados \(para que no sea de dominio unido máquinas\).

### <a name="authorization"></a>Autorización

Para la comunicación de Southbound, se usan los siguientes protocolos y métodos de autorización.

1. **WCF/TCP**. Para estos protocolos, la autorización se basa en el nombre de sujeto de la entidad del mismo nivel. Controladora de red almacena el nombre DNS del dispositivo del mismo nivel y lo usa para la autorización. Este nombre DNS debe coincidir con el nombre del dispositivo en el certificado del sujeto. Del mismo modo, el certificado de controladora de red debe coincidir con el nombre DNS del controlador de red almacenado en el dispositivo del mismo nivel.

2. **WinRM**. Si se utiliza Kerberos, la cuenta del cliente WinRM debe estar presente en un grupo predefinido en Active Directory o en el grupo de administradores Local en el servidor. Si se usan certificados, el cliente presenta un certificado en el servidor que el servidor se autoriza con el nombre de sujeto/emisor y el servidor utiliza una cuenta de usuario asignada para realizar la autenticación.

3.  **OVSDB**. No hay ninguna autorización proporcionada para este protocolo.

### <a name="encryption"></a>Cifrado

Para la comunicación de Southbound, se usan los siguientes métodos de cifrado de protocolos.

1. **WCF/TCP/OVSDB**. Para estos protocolos, el cifrado se realiza mediante el certificado que está inscrito en el cliente o servidor.

2. **WinRM**. Tráfico de WinRM se cifra de forma predeterminada, utiliza el proveedor de soporte técnico de seguridad de Kerberos \(SSP\). Puede configurar el cifrado adicional, en forma de SSL, en el servidor de WinRM.
