---
title: Administrar certificados para redes definidas por software
description: Puede usar este tema para aprender a administrar certificados de Northbound de controladora de red y comunicaciones de southbound al implementar redes definidas por software (SDN) en Windows Server 2016 Datacenter.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/22/2018
ms.openlocfilehash: 3225b3f5065e49521411b35fa3781338086b4e59
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854358"
---
# <a name="manage-certificates-for-software-defined-networking"></a>Administrar certificados para redes definidas por software

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para aprender a administrar certificados de Northbound de controladora de red y comunicaciones de southbound cuando implemente redes definidas por software \(SDN\) en Windows Server 2016 Datacenter y esté usando System Center Virtual Machine Manager \(SCVMM\) como el cliente de administración de SDN.

>[!NOTE]
>Para obtener información general acerca de la controladora de red, consulte [controladora de red](../technologies/network-controller/Network-Controller.md).

Si no usa Kerberos para proteger la comunicación de la controladora de red, puede usar certificados X. 509 para la autenticación, la autorización y el cifrado.

SDN en Windows Server 2016 Datacenter admite\-certificados de entidad de certificación y firmados por la entidad de certificación \(CA con firma\). En este tema se proporcionan instrucciones paso a paso para crear estos certificados y aplicarlos a los canales de comunicación de la controladora de red Northbound con los clientes de administración y las comunicaciones de southbound con dispositivos de red, como el Load Balancer de software \(SLB\).
.
Cuando use la autenticación basada en\-de certificados, debe inscribir un certificado en los nodos de la controladora de red que se utiliza de las siguientes maneras.

1. Cifrar la comunicación de Northbound con Capa de sockets seguros \(SSL\) entre los nodos de controladora de red y los clientes de administración, como System Center Virtual Machine Manager.
2. La autenticación entre los nodos de controladora de red y los dispositivos y servicios de southbound, como los hosts de Hyper-V y los equilibradores de carga de software \(SLBs\).

## <a name="creating-and-enrolling-an-x509-certificate"></a>Creación e inscripción de un certificado X. 509

Puede crear e inscribir un certificado firmado\-mente o un certificado emitido por una CA.

>[!NOTE]
>Cuando use SCVMM para implementar la controladora de red, debe especificar el certificado X. 509 que se usa para cifrar las comunicaciones de Northbound durante la configuración de la plantilla de servicio de la controladora de red.

La configuración del certificado debe incluir los valores siguientes.

- El valor del cuadro de texto **RestEndPoint** debe ser el nombre de dominio completo del controlador de red \(FQDN\) o la dirección IP. 
- El valor de **RestEndPoint** debe coincidir con el nombre de sujeto \(nombre común,\) CN del certificado X. 509.

### <a name="creating-a-self-signed-x509-certificate"></a>Creación de un certificado X. 509 firmado de\-

Puede crear un certificado X. 509 autofirmado y exportarlo con la clave privada \(protegido con una contraseña\) siguiendo estos pasos para un único nodo de\-y varias implementaciones de nodo de\-de la controladora de red.

Al crear certificados con firma\-, puede usar las siguientes directrices.

- Puede usar la dirección IP del punto de conexión de REST de la controladora de red para el parámetro DnsName, pero esto no es recomendable porque requiere que todos los nodos de la controladora de red se encuentren en una sola subred de administración \(por ejemplo, en un solo bastidor\)
- En el caso de las implementaciones de varios nodos NC, el nombre DNS que especifique se convertirá en el FQDN del clúster de la controladora de red \(host DNS A los registros A se crean automáticamente.\) 
- En el caso de las implementaciones de controlador de red de un solo nodo, el nombre DNS puede ser el nombre de host de la controladora de red seguido del nombre de dominio completo.

#### <a name="multiple-node"></a>Varios nodos

Puede usar el comando [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) de Windows PowerShell para crear un certificado firmado por\-.

**Sintaxis**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")

**Ejemplo de uso**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")

#### <a name="single-node"></a>Nodo único

Puede usar el comando [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) de Windows PowerShell para crear un certificado firmado por\-.

**Sintaxis**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")

**Ejemplo de uso**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")

### <a name="creating-a-ca-signed-x509-certificate"></a>Creación de una CA\-certificado X. 509 firmado

Para crear un certificado mediante una entidad de certificación, debe haber implementado una infraestructura de clave pública \(PKI\) con Active Directory servicios de Certificate Server \(AD CS\). 

>[!NOTE]
>Puede usar herramientas o entidades de certificación de terceros, como OpenSSL, para crear un certificado para su uso con la controladora de red, pero las instrucciones de este tema son específicas de AD CS. Para obtener información sobre cómo usar una entidad de certificación o herramienta de terceros, consulte la documentación del software que está usando.

La creación de un certificado con una CA incluye los pasos siguientes.

1. Usted o el administrador de dominio o de seguridad de su organización configura la plantilla de certificado.
2. Usted o el administrador de la controladora de red de la organización o el administrador de SCVMM solicita un nuevo certificado de la CA.

#### <a name="certificate-configuration-requirements"></a>Requisitos de configuración de certificados

Mientras configura una plantilla de certificado en el paso siguiente, asegúrese de que la plantilla que configure incluye los siguientes elementos necesarios.

1. El nombre del firmante del certificado debe ser el FQDN del host de Hyper-V.
2. El certificado debe colocarse en el almacén personal de la máquina local (My-CERT: \ localmachine\my)
3. El certificado debe tener directivas de aplicación de autenticación de servidor (EKU: 1.3.6.1.5.5.7.3.1) y autenticación de cliente (EKU: 1.3.6.1.5.5.7.3.2).

>[!NOTE]
>Si el almacén de certificados personal \(My-CERT: \ localmachine\my\) en el host de Hyper\-V tiene más de un certificado X. 509 con el nombre de sujeto (CN) como nombre de dominio completo del host \(FQDN\), asegúrese de que el certificado que SDN usará tiene una propiedad de uso de clave mejorada personalizada adicional con el OID 1.3.6.1.4.1.311.95.1.1.1. De lo contrario, es posible que la comunicación entre el controlador de red y el host no funcione.

#### <a name="to-configure-the-certificate-template"></a>Para configurar la plantilla de certificado
  
>[!NOTE]
>Antes de llevar a cabo este procedimiento, debe revisar los requisitos de certificado y las plantillas de certificado disponibles en la consola de plantillas de certificado. Puede modificar una plantilla existente o crear un duplicado de una plantilla existente y, a continuación, modificar la copia de la plantilla. Se recomienda crear una copia de una plantilla existente.

1. En el servidor donde está instalado AD CS, en Administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **entidad de certificación**. Se abre Microsoft Management Console \(MMC de la entidad de certificación\). 
2. En MMC, haga doble clic en el nombre de la entidad de certificación, haga clic con el botón secundario en **plantillas de certificado**y, a continuación, haga clic en **administrar**.
3. Se abre la consola de plantillas de certificado. Se mostrarán todas las plantillas de certificado en el panel de detalles.
4. En el panel de detalles, haga clic en la plantilla que desea duplicar.
5.  Haga clic en el menú **acción** y, a continuación, haga clic en **plantilla duplicada**. Se abrirá el cuadro de diálogo **propiedades** de la plantilla.
6.  En el cuadro de diálogo **propiedades** de la plantilla, en la pestaña **nombre de sujeto** , haga clic en **suministrar en la solicitud**. \(este valor es necesario para los certificados SSL de la controladora de red.\)
7.  En el cuadro de diálogo **propiedades** de la plantilla, en la pestaña **tratamiento** de la solicitud, asegúrese de que la opción permitir que la **clave privada se pueda exportar** esté seleccionada. Asegúrese también de que la firma y el uso de **cifrado** estén seleccionados.
8.  En el cuadro de diálogo **propiedades** de la plantilla, en la pestaña **extensiones** , seleccione **uso de clave**y, a continuación, haga clic en **Editar**.
9.  En **firma**, asegúrese de que está seleccionada la opción **firma digital** .
10.  En el cuadro de diálogo **propiedades** de la plantilla, en la pestaña **extensiones** , seleccione **directivas de aplicación**y, a continuación, haga clic en **Editar**.
11.  En **directivas de aplicación**, asegúrese de que se muestra la autenticación del **cliente** y la **autenticación del servidor** .
12.  Guarde la copia de la plantilla de certificado con un nombre único, como la **plantilla de controladora de red**.

#### <a name="to-request-a-certificate-from-the-ca"></a>Para solicitar un certificado de la entidad de certificación

Puede usar el complemento certificados para solicitar certificados. Puede solicitar cualquier tipo de certificado que haya sido preconfigurado y puesto a disposición de un administrador de la CA que procese la solicitud de certificado.

**Los usuarios** o **administradores** locales son la pertenencia al grupo mínima necesaria para completar este procedimiento.

1. Abra el complemento certificados de un equipo.
2. En el árbol de consola, haga clic en **certificados \(equipo Local\)** . Seleccione el almacén de certificados **personal** .
3. En el menú **acción** , seleccione * * todas las tareas<strong>y, a continuación, haga clic en * * solicitar nuevo certificado</strong> para iniciar el Asistente para inscripción de certificados. Haga clic en **Siguiente**.
4. Seleccione la Directiva de inscripción de certificados de **Administrador configurada** y haga clic en **siguiente**.
5. Seleccione la **Directiva de inscripción de Active Directory** \(basada en la plantilla de CA configurada en la sección anterior\).
6. Expanda la sección **detalles** y configure los siguientes elementos.
   1. Asegúrese de que el uso de la **clave** incluye tanto la <strong>firma digital * * como el cifrado de clave * *</strong>.
   2. Asegúrese de que **las directivas de aplicación** incluyan la **autenticación de servidor** \(1.3.6.1.5.5.7.3.1\) y la autenticación de **cliente** \(1.3.6.1.5.5.7.3.2\).
7. Haga clic en **Propiedades**.
8. En la pestaña **sujeto** , en **nombre de sujeto**, en **tipo**, seleccione **nombre común**. En valor, especifique **extremo de REST de la controladora de red**.
9. Haga clic en **Aplicar** y en **Aceptar**.
10. Haz clic en **Inscribir.**

En el MMC certificados, haga clic en el almacén personal para ver el certificado que ha inscrito en la CA.

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>Exportar y copiar el certificado en la biblioteca de SCVMM

Después de crear un certificado\-firmado o de CA\-firmado, debe exportar el certificado con la clave privada \(en formato. pfx\) y sin la \(de la clave privada en formato 64. cer\) del complemento certificados. 

Después, debe copiar los dos archivos exportados en las carpetas **ServerCertificate.cr** y **NCCertificate.cr** que especificó en el momento en que importó la plantilla de servicio de NC.

1. Abra el complemento certificados (certlm. msc) y busque el certificado en el almacén de certificados personal del equipo local.
2. \-haga clic con el botón derecho en el certificado, seleccione **todas las tareas**y, a continuación, haga clic en **exportar**. Se abre el Asistente para exportación de certificados. Haga clic en **Siguiente**.
3. Seleccione la opción exportar la clave **privada y haga**clic en **siguiente**.
4. Elija **intercambio de información personal: PKCS #12 (. PFX)** y acepte el valor predeterminado para **incluir todos los certificados en la ruta de certificación** si es posible.
5. Asigne los usuarios o grupos y una contraseña para el certificado que va a exportar, haga clic en **siguiente**.
6. En la página archivo que se va a exportar, busque la ubicación en la que desea colocar el archivo exportado y asígnele un nombre.
7. Del mismo modo, exporte el certificado en. Formato CER. Nota: para exportar a. Formato CER, desactive la opción sí, exportar la clave privada.
8. Copie el. PFX en la carpeta ServerCertificate.cr.
9. Copie el. Archivo CER en la carpeta NCCertificate.cr

Cuando haya terminado, actualice estas carpetas en la biblioteca de SCVMM y asegúrese de que se han copiado estos certificados. Continúe con la configuración e implementación de la plantilla de servicio de la controladora de red.

## <a name="authenticating-southbound-devices-and-services"></a>Autenticación de dispositivos y servicios de southbound 

La comunicación de la controladora de red con los hosts y los dispositivos MUX de SLB usa certificados para la autenticación. La comunicación con los hosts se realiza a través del protocolo OVSDB mientras que la comunicación con los dispositivos MUX de SLB se realiza a través del protocolo WCF.

### <a name="hyper-v-host-communication-with-network-controller"></a>Comunicación del host de Hyper-V con controladora de red

Para la comunicación con los hosts de Hyper-V a través de OVSDB, la controladora de red debe presentar un certificado a los equipos host. De forma predeterminada, SCVMM toma el certificado SSL configurado en el controlador de red y lo usa para la comunicación de southbound con los hosts.

Este es el motivo por el que el certificado SSL debe tener configurado el EKU de autenticación del cliente. Este certificado se configura en el recurso de REST "servidores" \(los hosts de Hyper-V se representan en el controlador de red como un recurso de servidor\)y se pueden ver ejecutando el comando de Windows PowerShell **Get-NetworkControllerServer**.

A continuación se encuentra un ejemplo parcial del recurso de REST de servidor.

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

Para la autenticación mutua, el host de Hyper-V también debe tener un certificado para comunicarse con el controlador de red. 

Puede inscribir el certificado de una entidad de certificación \(CA\). Si no se encuentra un certificado basado en una entidad de certificación en el equipo host, SCVMM crea un certificado autofirmado y lo aprovisiona en el equipo host.

El controlador de red y los certificados de host de Hyper-V deben ser de confianza entre sí. El certificado raíz del certificado de host de Hyper-V debe estar presente en el almacén de entidades de certificación raíz de confianza de la controladora de red para el equipo local, y viceversa. 

Cuando se usan certificados Auto\-firmados, SCVMM garantiza que los certificados necesarios están presentes en el almacén de entidades de certificación raíz de confianza para el equipo local. 

Si usa certificados basados en CA para los hosts de Hyper-V, debe asegurarse de que el certificado raíz de CA esté presente en el almacén de entidades de certificación raíz de confianza del equipo local.

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>Comunicación de software Load Balancer MUX con controladora de red

El multiplexor de Load Balancer de software \(MUX\) y la controladora de red se comunican a través del protocolo WCF mediante certificados para la autenticación.

De forma predeterminada, SCVMM toma el certificado SSL configurado en el controlador de red y lo usa para la comunicación de southbound con los dispositivos Mux. Este certificado se configura en el recurso de REST "NetworkControllerLoadBalancerMux" y se puede ver mediante la ejecución del cmdlet de PowerShell **Get-NetworkControllerLoadBalancerMux**.

Ejemplo de recurso de REST de MUX \(\)parcial:

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

Para la autenticación mutua, también debe tener un certificado en los dispositivos MUX de SLB. SCVMM configura automáticamente este certificado cuando se implementa el equilibrador de carga de software mediante SCVMM.

>[!IMPORTANT]
>En los nodos host y SLB, es fundamental que el almacén de certificados de entidades de certificación raíz de confianza no incluya ningún certificado en el que "emitido para" no sea el mismo que el "emitido por". Si esto ocurre, se produce un error en la comunicación entre la controladora de red y el dispositivo southbound.

Los certificados de controladora de red y MUX de SLB deben ser de confianza entre sí \(el certificado raíz del certificado MUX de SLB debe estar presente en el almacén de entidades de certificación raíz de confianza del equipo de la controladora de red y viceversa\). Cuando se usan certificados Auto\-firmados, SCVMM garantiza que los certificados necesarios están presentes en en el almacén de entidades de certificación raíz de confianza para el equipo local.



