---
title: Administrar certificados para redes definidas por Software
description: Puede utilizar este tema para aprender a administrar certificados para Northbound de controladora de red y comunicación de Southbound al implementar definidas por Software Networking (SDN) en Windows Server 2016 Datacenter.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: 618c2c4da60decc94f84c2a40cd4d2aa80d5f26b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827576"
---
# <a name="manage-certificates-for-software-defined-networking"></a>Administrar certificados para redes definidas por Software

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre cómo administrar certificados para las comunicaciones de Northbound de controladora de red y de Southbound al implementar redes definidas por Software \(SDN\) en Windows Server 2016 Datacenter y está usando System Center Virtual Machine Manager \(SCVMM\) como el cliente de administración de SDN.

>[!NOTE]
>Para obtener información general acerca de la controladora de red, consulte [controladora de red](../technologies/network-controller/Network-Controller.md).

Si no se usa Kerberos para proteger la comunicación de controladora de red, puede usar certificados X.509 para la autenticación, autorización y cifrado.

SDN en Windows Server 2016 Datacenter admite ambos self\-firmado y la entidad de certificación \(CA\)-certificados X.509 autofirmados. Este tema proporciona instrucciones paso a paso para crear estos certificados y aplicarla para proteger los canales de comunicación de Northbound de controladora de red con los clientes de administración y comunicación de Southbound con dispositivos de red, como el Software Equilibrador de carga \(SLB\).
.
Cuando se utiliza el certificado\-según la autenticación, debe inscribir un certificado en los nodos de controladora de red que se usa en las siguientes maneras.

1. Cifrar la comunicación de Northbound con capa de Sockets seguros \(SSL\) entre los nodos de controladora de red y los clientes de administración, como System Center Virtual Machine Manager.
2. Autenticación entre los nodos de controladora de red y Southbound dispositivos y servicios, como hosts de Hyper-V y equilibradores de carga de Software \(SLBs\).

## <a name="creating-and-enrolling-an-x509-certificate"></a>Crear e inscribir un certificado X.509

Puede crear e inscribir un self\-firmado el certificado o un certificado emitido por una entidad de certificación.

>[!NOTE]
>Cuando se usa SCVMM para implementar la controladora de red, debe especificar el certificado X.509 que se usa para cifrar las comunicaciones de Northbound durante la configuración de la plantilla de servicio de controladora de red.

La configuración del certificado debe incluir los siguientes valores.

- El valor de la **RestEndPoint** cuadro de texto debe ser la red controlador de nombre de dominio completo \(FQDN\) o la dirección IP. 
- El **RestEndPoint** valor debe coincidir con el nombre de sujeto \(nombre común, CN\) del certificado X.509.

### <a name="creating-a-self-signed-x509-certificate"></a>Creación de un autoservicio\-certificado X.509 autofirmado

Puede crear un certificado X.509 autofirmado y exportarlo con la clave privada \(protegido con contraseña\) siguiendo estos pasos para que el único\-nodo y varios\-implementaciones de nodos de controladora de red .

Cuando creas self\-firmado de certificados, puede usar las siguientes directrices.

- Puede usar la dirección IP del punto de conexión de REST de controlador de red para el parámetro DnsName - pero no se recomienda porque requiere que los nodos de controladora de red están todas ubicados dentro de una subred de administración único \(, por ejemplo, en un único bastidor\)
- Para varias implementaciones de nodo NC, el nombre DNS que especifique se convertirá en el FQDN del clúster de controlador de red \(se crean automáticamente los registros de Host de DNS.\) 
- Para las implementaciones de controladora de red de nodo único, el nombre DNS puede ser el nombre de host del controlador de red seguido del nombre de dominio completo.

#### <a name="multiple-node"></a>Varios nodos

Puede usar el [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) comando de Windows PowerShell para crear un\-certificado autofirmado.

**Sintaxis**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")

**Ejemplo de uso**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")

#### <a name="single-node"></a>Nodo único

Puede usar el [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) comando de Windows PowerShell para crear un\-certificado autofirmado.

**Sintaxis**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")

**Ejemplo de uso**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")

### <a name="creating-a-ca-signed-x509-certificate"></a>Creación de una entidad de certificación\-certificado X.509 autofirmado

Para crear un certificado mediante el uso de una entidad de certificación, ya debe haber implementado una infraestructura de clave pública \(PKI\) con Active Directory Certificate Services \(AD CS\). 

>[!NOTE]
>Puede usar las entidades de certificación de terceros o las herramientas, como openssl, para crear un certificado para su uso con la controladora de red, sin embargo, las instrucciones de este tema son específicas de AD CS. Para obtener información sobre cómo usar una entidad de certificación de terceros o la herramienta, consulte la documentación para el software que esté utilizando.

Creación de un certificado con una entidad de certificación incluye los siguientes pasos.

1. Su organización dominio o administrador de seguridad configura la plantilla de certificado
2. O su organización controlador Administrador de red o administrador de SCVMM solicita un nuevo certificado desde la CA.

#### <a name="certificate-configuration-requirements"></a>Requisitos de configuración de certificado

Durante la configuración de una plantilla de certificado en el paso siguiente, asegúrese de que la plantilla que configura incluye los siguientes elementos necesarios.

1. El nombre de sujeto del certificado debe ser el FQDN del host de Hyper-V
2. El certificado debe colocarse en el almacén personal del equipo local (mi – cert: \localmachine\my)
3. El certificado debe tener tanto la autenticación de servidor (EKU: 1.3.6.1.5.5.7.3.1) y la autenticación de cliente (EKU: 1.3.6.1.5.5.7.3.2) las directivas de aplicación.

>[!NOTE]
>Si el perfil Personal \(mi – cert: \localmachine\my\) almacén de certificados en el Hyper\-host V tiene más de un certificado X.509 con el nombre de asunto (CN) como el nombre de dominio completo del host \(FQDN\), Asegúrese de que el certificado que se usará en SDN tiene una propiedad de uso mejorado de clave personalizada adicional con el OID 1.3.6.1.4.1.311.95.1.1.1. En caso contrario, la comunicación entre el host y la controladora de red podría no funcionar.

#### <a name="to-configure-the-certificate-template"></a>Para configurar la plantilla de certificado
  
>[!NOTE]
>Antes de realizar este procedimiento, debe revisar los requisitos de certificados y las plantillas de certificado disponibles en la consola de plantillas de certificado. Puede modificar una plantilla existente o crear un duplicado de una plantilla existente y, a continuación, modifique la copia de la plantilla. Se recomienda crear una copia de una plantilla existente.

1. En el servidor donde está instalado AD CS, en el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **entidad de certificación**. La consola de administración de Microsoft de entidad de certificación \(MMC\) se abre. 
2. En MMC, haga doble clic en el nombre de entidad de certificación, haga clic en **plantillas de certificado**y, a continuación, haga clic en **administrar**.
3. Se abre la consola de plantillas de certificado. Todas las plantillas de certificado se muestran en el panel de detalles.
4. En el panel de detalles, haga clic en la plantilla que desea duplicar.
5.  Haga clic en el **acción** menú y, a continuación, haga clic en **Duplicar plantilla**. La plantilla **propiedades** abre el cuadro de diálogo.
6.  En la plantilla **propiedades** cuadro de diálogo el **nombre de sujeto** , haga clic **proporcionado por el solicitante**. \(Esta configuración es necesaria para los certificados SSL del controlador de red.\)
7.  En la plantilla **propiedades** cuadro de diálogo el **tratamiento de la solicitud** , asegúrese de que **permitir que la clave privada se pueda exportar** está seleccionada. Asegúrese también de que el **firma y cifrado** propósito está seleccionado.
8.  En la plantilla **propiedades** cuadro de diálogo el **extensiones** ficha, seleccione **uso clave**y, a continuación, haga clic en **editar**.
9.  En **firma**, asegúrese de que **firma Digital** está seleccionada.
10.  En la plantilla **propiedades** cuadro de diálogo el **extensiones** ficha, seleccione **las directivas de aplicación**y, a continuación, haga clic en **editar**.
11.  En **las directivas de aplicación**, asegúrese de que **autenticación de cliente** y **autenticación de servidor** se muestran.
12.  Guarde la copia de la plantilla de certificado con un nombre único, como **plantilla de controladora de red**.

#### <a name="to-request-a-certificate-from-the-ca"></a>Para solicitar un certificado de la entidad de certificación

Puede usar el complemento certificados para solicitar certificados. Puede solicitar cualquier tipo de certificado que se ha preconfigurado y facilitados por un administrador de la CA que procesa la solicitud de certificado.

**Los usuarios** o local **administradores** es la pertenencia a grupos mínima necesaria para completar este procedimiento.

1. Abra el complemento certificados para un equipo.
2. En el árbol de consola, haga clic en **certificados \(ordenador\)**. Seleccione el **Personal** almacén de certificados.
3. En el **acción** menú, seleccione ** todas las tareas ** y, a continuación, haga clic en **solicitar un nuevo certificado** para iniciar el Asistente de inscripción de certificados. Haz clic en **Siguiente**.
4. Seleccione el **configurado por el administrador** directiva de inscripción de certificados y haga clic en **siguiente**.
5. Seleccione el **directiva de inscripción de Active Directory** \(basado en la plantilla de entidad de certificación que configuró en la sección anterior\).
6. Expanda el **detalles** sección y configurar los elementos siguientes.
    1. Asegúrese de que **uso de la clave** incluye ambos ** firma Digital ** y **cifrado de clave**.
    2. Asegúrese de que **las directivas de aplicación** incluye ambos **autenticación de servidor** \(1.3.6.1.5.5.7.3.1\) y **autenticación de cliente** \(1.3.6.1.5.5.7.3.2\).
7. Haga clic en **Propiedades**.
8. En el **asunto** ficha **nombre de sujeto**, en **tipo**, seleccione **nombre común**. En valor, especifique **punto de conexión de REST de controladora de red**.
9. Haga clic en **Aplicar** y en **Aceptar**.
10. Haz clic en **Inscribir.**

En el MMC certificados, haga clic en el almacén Personal para ver el certificado que se han inscrito desde la CA.

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>Exportar y copiar el certificado en la biblioteca de SCVMM

Después de crear un self\-firmado o CA\-firmó el certificado, debe exportar el certificado con la clave privada \(en formato .pfx\) y sin la clave privada \(en formato .cer de base64\) desde el complemento certificados. 

A continuación, debe copiar los archivos exportados de dos a la **ServerCertificate.cr** y **NCCertificate.cr** las carpetas que especifican en el momento al importar la plantilla de servicio de controladora de red.

1. Abra el complemento de certificados (certlm.msc) y busque el certificado en el almacén de certificados personales del equipo local.
2. Derecha\-haga clic en el certificado, haga clic en **todas las tareas**y, a continuación, haga clic en **exportar**. Se abre el Asistente para exportación de certificados. Haz clic en **Siguiente**.
3. Seleccione **Sí**, la opción de clave privada de exportación, haga clic en **siguiente**.
4. Elija **intercambio de información Personal: PKCS #12 (. PFX)** y acepte el valor predeterminado para **incluir todos los certificados en la ruta de certificación** si es posible.
5. Asignar los grupos de usuarios y una contraseña para el certificado que se va a exportar, haga clic en **siguiente**.
6. En el archivo de exportación de la página, busque la ubicación donde desea colocar el archivo exportado y asígnele un nombre.
7. De forma similar, exporte el certificado en. Formato CER. Nota: Para exportar a. Formato CER, desactive la opción Sí, exportar la opción de clave privada.
8. Copia el. PFX en la carpeta ServerCertificate.cr.
9. Copia el. Archivo CER en la carpeta NCCertificate.cr.

Cuando haya terminado, actualice estas carpetas en la biblioteca de SCVMM y asegúrese de que tiene estos certificados copiados. Continuar con la configuración de plantilla de servicio de controlador de red y la implementación.

## <a name="authenticating-southbound-devices-and-services"></a>Autenticación de servicios y dispositivos Southbound 

Comunicación de controladora de red con los hosts y dispositivos de SLB MUX usa certificados para la autenticación. Comunicación con los hosts es a través del protocolo OVSDB mientras que la comunicación con los dispositivos de SLB MUX es a través del protocolo WCF.

### <a name="hyper-v-host-communication-with-network-controller"></a>Comunicación con el Host de Hyper-V con la controladora de red

Controladora de red para la comunicación con los hosts de Hyper-V a través de OVSDB, debe presentar un certificado en los equipos host. De forma predeterminada, SCVMM recoge el certificado SSL configurado en la controladora de red y lo utiliza para la comunicación de southbound con los hosts.

Este es el motivo por qué el certificado SSL debe tener el EKU de autenticación de cliente configurado. Este certificado se configura en "Servidores" REST recursos \(hosts de Hyper-V se representan en la controladora de red como un recurso de servidor\)y se pueden ver ejecutando el comando de Windows PowerShell  **Get-NetworkControllerServer**.

Este es un ejemplo parcial del servidor de recursos REST.

      "resourceId": "host31.fabrikam.com",
      "properties": {
        "connections": [
          {
            "managementAddresses": [
               "host31.fabrikam.com"
            ],
            "credential": {
              "resourceRef": "/credentials/a738762f-f727-43b5-9c50-cf82a70221fa"
            },
            "credentialType": "X509Certificate"
          }
        ],

Para la autenticación mutua, el host de Hyper-V también debe tener un certificado para comunicarse con controladora de red. 

Puede inscribir el certificado de una entidad de certificación \(CA\). Si no se encuentra un certificado de CA basado en el equipo host, SCVMM crea un certificado autofirmado y lo aprovisiona en el equipo host.

Controladora de red y los certificados de host de Hyper-V deben ser de confianza entre sí. Certificado de raíz del certificado del host de Hyper-V debe estar presente en el almacén de las entidades emisoras de raíz de confianza de controlador de red para el equipo Local y viceversa. 

Cuando se usa self\-certificados, autofirmados SCVMM garantiza que los certificados necesarios están presentes en el almacén entidades emisoras raíz de confianza para el equipo Local. 

Si utiliza certificados de CA en función de los hosts de Hyper-V, deberá asegurarse de que el certificado de CA raíz está presente en el almacén de entidades emisoras raíz de confianza de la controladora de red para el equipo Local.

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>Comunicación de MUX de equilibrador de software carga con controladora de red

El Multiplexor de equilibrador de carga de Software \(MUX\) y controladora de red comunicarse a través del protocolo WCF, el uso de certificados para la autenticación.

De forma predeterminada, SCVMM recoge el certificado SSL configurado en la controladora de red y lo utiliza para la comunicación de southbound con los dispositivos Mux. Este certificado está configurado en el "NetworkControllerLoadBalancerMux" recursos de REST y pueden verse mediante la ejecución del cmdlet de Powershell **Get NetworkControllerLoadBalancerMux**.

Ejemplo de recurso REST MUX \(parcial\):

      "resourceId": "slbmux1.fabrikam.com",
      "properties": {
        "connections": [
          {
            "managementAddresses": [
               "slbmux1.fabrikam.com"
            ],
            "credential": {
              "resourceRef": "/credentials/a738762f-f727-43b5-9c50-cf82a70221fa"
            },
            "credentialType": "X509Certificate"
          }
        ],

Para la autenticación mutua, también debe tener un certificado en los dispositivos de SLB MUX. Este certificado se configura automáticamente mediante SCVMM al implementar el equilibrador de carga de software mediante SCVMM.

>[!IMPORTANT]
>En el host y los nodos SLB, es fundamental que el almacén de certificados entidades emisoras raíz de confianza no incluye ningún certificado donde "Emitido para" no es igual a "Emitido por". Si esto ocurre, se produce un error de comunicación entre la controladora de red y el dispositivo de southbound.

Controladora de red y los certificados de SLB MUX deben ser de confianza entre sí \(certificado de raíz del certificado de SLB MUX debe estar presente en la máquina del controlador de red almacenar entidades de certificación raíz de confianza y viceversa\). Cuando se usa self\-firmado de certificados, SCVMM garantiza que los certificados necesarios están presentes en el de las entidades de certificación raíz de confianza almacenar para el equipo Local.



