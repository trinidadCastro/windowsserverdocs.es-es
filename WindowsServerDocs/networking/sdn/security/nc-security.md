---
title: Seguridad del controlador de red
description: Puedes usar este tema para aprender a configurar la seguridad de toda la comunicación entre el controlador de red y otro software y dispositivos.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ab04d3f528beb037988e6390fe85ad9af219266b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="network-controller-security"></a>Seguridad del controlador de red

Puedes usar este tema para aprender a configurar la seguridad de toda la comunicación entre el controlador de red y otro software y dispositivos. 

>[!NOTE]
>Para obtener información general de controlador de red, consulta [controlador de red](../technologies/network-controller/network-controller.md).

Las rutas de acceso de comunicación que puede proteger incluyen la comunicación Northbound en el plano de administración, comunicación entre el controlador de red máquinas virtuales \(VMs\) en un clúster y la comunicación Southbound en el plano de datos del clúster.

1. **Comunicación northbound**. Controlador de red se comunica en el plano de administración con el software de administración compatibles con SDN\ como System Center Virtual Machine Manager \(SCVMM\) y de Windows PowerShell. Estas herramientas de administración proporcionan la capacidad para definir la directiva de red y crea un estado de objetivo de la red, con respecto al cual se puede comparar la configuración de red real para que aparezca la configuración real en la paridad con el estado del objetivo.

2. **Controlador clúster comunicaciones de red**. Cuando configuras tres o más de máquinas virtuales como nodos del clúster de controlador de red, estos nodos comunican entre sí. Esta comunicación podría estar relacionada con la sincronización y la replicación de datos a través de los nodos o específicos de comunicación entre los servicios de controlador de red.

3.  **Comunicación southbound**. Controlador de red se comunica en el plano de datos con la infraestructura SDN y otros dispositivos como equilibradores de carga de software, puertas de enlace y los equipos host. Puedes usar el controlador de red para configurar y administrar estos dispositivos southbound para que deben mantener el estado de objetivo que hayas configurado para la red.

## <a name="northbound-communication"></a>Comunicación northbound

Admite la autenticación, autorización y cifrado para comunicaciones Northbound de controlador de red. Las secciones siguientes proporcionan información sobre cómo configurar estas opciones de configuración de seguridad.

**Autenticación**

Cuando se configura la autenticación para la comunicación de red controlador Northbound, permite nodos del clúster de controlador de red y los clientes de administración para comprobar la identidad del dispositivo con el que se comunican.

Controlador de red admite los siguientes tres modos de autenticación entre los clientes de administración y nodos de controlador de red.

>[!Note]
>Si vas a implementar el controlador de red con System Center Virtual Machine Manager, solo **Kerberos** se admite el modo.

1. **Kerberos**. Puedes usar la autenticación Kerberos cuando ambas cliente de administración, como el equipo que ejecuta SCVMM y todos los nodos del clúster de controlador de red están unidos a un dominio de Active Directory con cuentas de dominio que se usa para la autenticación.

2. **X509**. X509 es la autenticación basada en certificate\. Puedes usar X509 autenticación cuando los clientes de administración no están unidos a un dominio de Active Directory. Para usar X509, deben inscribirse certificados a todos los nodos del clúster de controlador de red y los clientes de administración y deben confiar en todos los nodos y los clientes de administración de los demás ' certificados.

3. **Ninguno**. Cuando se elige este modo, no existe ninguna autenticación realizada entre los clientes de administración y el controlador de red. Este modo se proporciona solo para fines de prueba y no se recomienda para su uso en un entorno de producción. 

Puedes configurar el modo de autenticación para la comunicación Northbound mediante el comando de Windows PowerShell **instalación NetworkController** con la **ClientAuthentication** parámetro. 

Para obtener más información, consulta los siguientes temas. 

- [Install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)
- [Set-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)

**Autorización**

Cuando se configura la autorización para la comunicación de red controlador Northbound, puedes permitir que los nodos de clúster de controlador de red y los clientes de administración para comprobar que el dispositivo con el que se comunican es de confianza y que tiene permiso para participar en la comunicación.

Cada uno de los modos de autenticación admitidos por el controlador de red, se usan los siguientes métodos de autorización.

1.  **Kerberos**. Cuando usas el método de autenticación de Kerberos, defines los usuarios y equipos que tienen autorización para comunicarse con el controlador de red al crear un grupo de seguridad en Active Directory y después agregar los equipos y los usuarios autorizados al grupo. Puede configurar el controlador de red para usar el grupo de seguridad para la autorización mediante la **ClientSecurityGroup** parámetro de la **instalación NetworkController** comando de Windows PowerShell. Después de instalar el controlador de red, puede cambiar el grupo de seguridad mediante la [conjunto NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller) comando con el parámetro **- ClientSecurityGroup**. Si estás usando SCVMM, debes proporcionar el grupo de seguridad como un parámetro durante la implementación.

2.  **X509**. Cuando estés usando el X509 método de autenticación, el controlador de red solo acepta las solicitudes de los clientes de administración cuyos huellas digitales de certificados se sabe que el controlador de red. Puedes configurar estas huellas digitales usando la **ClientCertificateThumbprint** parámetro de la **instalación NetworkController** comando de Windows PowerShell. Puede agregar otras huellas digitales de cliente en cualquier momento mediante la **conjunto NetworkController** comando.

3.  **Ninguno**. Cuando se elige este modo, no existe sin autorización lleva a cabo para intentos de comunicación entre los clientes de administración y nodos de controlador de red. Este modo se proporciona solo para fines de prueba y no se recomienda para su uso en un entorno de producción. 

Para obtener más información, consulta los siguientes temas. 

- [Install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)
- [Set-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)

**Cifrado**

Comunicación northbound usa \(SSL\) capa de Sockets seguros para crear un canal cifrado entre clientes de administración y nodos de controlador de red. El cifrado SSL para la comunicación Northbound incluye los siguientes requisitos.

- Todos los nodos de controlador de red deben tener un certificado idéntico que incluya los fines de autenticación de servidor y cliente en las extensiones \(EKU\) uso mejorado de claves. 

- El URI usado por los clientes de administración para comunicarse con el controlador de red debe ser el nombre de firmante del certificado. El nombre de firmante del certificado debe contener el nombre de dominio completo (FQDN) o la dirección IP del extremo del resto de controlador de red.

- Si los nodos de controlador de red se encuentran en diferentes subredes, el nombre de asunto de su certificado debe ser el mismo que el valor que usas para la **RestName** parámetro en el **instalación NetworkController** comando de Windows PowerShell. 

- El certificado SSL debe ser de confianza para todos los clientes de administración.

### <a name="ssl-certificate-enrollment-and-configuration"></a>Configuración y la inscripción de certificados SSL

Debe inscribir manualmente el certificado SSL en nodos de controlador de red.

Después de que el certificado se inscribe, puedes configurar el controlador de red para usar el certificado con el **- certificados** parámetro de la **instalación NetworkController** comando de Windows PowerShell. Si ya tiene instalado el controlador de red, puede actualizar la configuración en cualquier momento mediante la **conjunto NetworkController** comando.

>[!NOTE]
>Si estás usando SCVMM, debes agregar el certificado como un recurso de la biblioteca. Para obtener más información, consulta [configurar un controlador de red SDN en la estructura VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

## <a name="network-controller-cluster-communication"></a>Comunicación de clúster de controlador de red

Controlador de red admite la autenticación, autorización y cifrado para la comunicación entre los nodos de controlador de red. La comunicación es sobre [Windows Communication Foundation](https://msdn.microsoft.com/library/ms731082.aspx) \(WCF\) y TCP.

Puedes configurar este modo con el **ClusterAuthentication** parámetro de la **instalación NetworkControllerCluster** comando de Windows PowerShell. 

Para obtener más información, consulta [instalación NetworkControllerCluster](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontrollercluster).

**Autenticación**

Cuando se configura la autenticación para la comunicación de clúster de controlador de red, puedes permitir que nodos del clúster de controlador de red comprobar la identidad de los otros nodos con el que se comunican.

Controlador de red admite los siguientes tres modos de autenticación entre los nodos de controlador de red.

>[!NOTE]
>Si implementas un controlador de red mediante SCVMM, solo **Kerberos** se admite el modo.

1. **Kerberos**. Puedes usar la autenticación Kerberos cuando todos los nodos del clúster de controlador de red están unidos a un dominio de Active Directory con cuentas de dominio que se usa para la autenticación.

2. **X509**. X509 es la autenticación basada en certificate\. Puedes usar X509 autenticación cuando los nodos del clúster de controlador de red no están unidos a un dominio de Active Directory. Para usar X509, deben inscribirse certificados para todos los nodos del clúster de controlador de red, y todos los nodos deben confiar en los certificados. Además, el nombre del sujeto del certificado que se inscribe en cada nodo debe ser el mismo que el nombre DNS del nodo.

3. **Ninguno**. Cuando se elige este modo, no existe ninguna autenticación realizada entre los nodos de controlador de red. Este modo se proporciona solo para fines de prueba y no se recomienda para su uso en un entorno de producción.

**Autorización**

Cuando se configura la autorización para la comunicación de clúster de controlador de red, puedes permitir que nodos del clúster de controlador de red comprobar que los nodos con el que se comunican son de confianza y tenga permiso para participar en la comunicación.

Cada uno de los modos de autenticación admitidos por el controlador de red, se usan los siguientes métodos de autorización.

1. **Kerberos**. Nodos de controlador de red aceptan solicitudes de comunicación únicamente de otras cuentas de máquina de controlador de red. Puedes configurar estas cuentas al implementar el controlador de red mediante el **nombre** parámetro de la [nueva NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) comando de Windows PowerShell.

2. **X509**. Nodos de controlador de red aceptan solicitudes de comunicación únicamente de otras cuentas de máquina de controlador de red. Puedes configurar estas cuentas al implementar el controlador de red mediante el **nombre** parámetro de la [nueva NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) comando de Windows PowerShell.

3. **Ninguno**. Cuando se elige este modo, no existe sin autorización realizada entre los nodos de controlador de red. Este modo se proporciona solo para fines de prueba y no se recomienda para su uso en un entorno de producción.

**Cifrado**

Comunicación entre los nodos de controlador de red está cifrada mediante el cifrado de nivel de transporte de WCF. Este método de cifrado se utiliza cuando los métodos de autenticación y autorización de Kerberos o X509 certificados. Para obtener más información, consulta los siguientes temas.

- [Cómo: proteger un servicio con credenciales de Windows](https://msdn.microsoft.com/library/ms734673.aspx)
- [Cómo: proteger un servicio con certificados X.509](https://msdn.microsoft.com/library/ms788968.aspx).

## <a name="southbound-communication"></a>Comunicación southbound

Controlador de red interactúa con diferentes tipos de dispositivos para la comunicación Southbound. Estas interacciones usan diferentes protocolos. Por este motivo, existen distintos requisitos de autenticación, autorización y cifrado según el tipo de dispositivo y el protocolo usado por el controlador de red para comunicarse con el dispositivo.

La siguiente tabla proporciona información sobre la interacción de controlador de red con diferentes dispositivos southbound.

| Southbound dispositivo o servicio | Protocolo              | Usado la autenticación    |
|---------------------------|-----------------------|------------------------|
| Equilibrado de carga de software    | WCF (MUX), TCP (Host) | Certificados           |
| Firewall de                  | OVSDB                 | Certificados           |
| Puerta de enlace                   | WinRM                 | Kerberos, certificados |
| Redes virtuales        | OVSDB, WCF            | Certificados           |
| Enrutamiento definida por el usuario      | OVSDB                 | Certificados           |

Cada uno de estos protocolos, el mecanismo de comunicación se describe en la siguiente sección.

**Autenticación**

Para la comunicación Southbound, se usan los siguientes protocolos y métodos de autenticación.

1. **WCF, TCP/OVSDB**. Para estos protocolos, la autenticación se realiza mediante X509 certificados. Controlador de red y el sistema del mismo nivel de equilibrio de carga de Software \(SLB\) Multiplexador \ (MUX\) / equipos host presentan sus certificados entre sí para la autenticación mutua. Cada certificado debe ser de confianza para el interlocutor remoto.

    Para la autenticación southbound, puedes usar el mismo certificado SSL que está configurado para cifrar la comunicación con los clientes Northbound. También debes configurar un certificado en los dispositivos de host y SLB MUX. El nombre de firmante del certificado debe ser igual que el nombre del dispositivo.

2. **WinRM**. Para este protocolo, la autenticación se realiza mediante Kerberos \ (para machines\ unido a un dominio) y el uso de certificados \ (para no del dominio machines\ combinadas).

**Autorización**

Para la comunicación Southbound, se usan los siguientes protocolos y métodos de autorización.

1. **WCF/TCP**. Para estos protocolos, autorización se basa en el nombre de firmante de la entidad de sistema del mismo nivel. Controlador de red almacena el nombre DNS de dispositivo del mismo nivel y lo usa para la autorización. Este nombre DNS debe coincidir con el nombre del sujeto del dispositivo en el certificado. De igual modo, el certificado del controlador de red debe coincidir con el nombre DNS de controladores de red almacenado en el dispositivo del mismo nivel.

2. **WinRM**. Si se usa Kerberos, la cuenta del cliente WinRM debe estar presente en un grupo predefinido en Active Directory o en el grupo de administradores locales en el servidor. Si se usan los certificados, el cliente presenta un certificado al servidor que el servidor autoriza con el nombre o emisor de asunto y el servidor usa una cuenta de usuario asignado para realizar la autenticación.

3.  **OVSDB**. No hay ninguna autorización para este protocolo.

**Cifrado**

Para la comunicación Southbound, los siguientes métodos de cifrado se usan para los protocolos.

1. **WCF, TCP/OVSDB**. Para estos protocolos, el cifrado se realiza con el certificado que se inscribe en el cliente o servidor.

2. **WinRM**. WinRM tráfico se cifra de forma predeterminada con el proveedor de soporte técnico de seguridad de Kerberos \(SSP\). Puedes configurar cifrado adicional, en forma de SSL, en el servidor de WinRM.
