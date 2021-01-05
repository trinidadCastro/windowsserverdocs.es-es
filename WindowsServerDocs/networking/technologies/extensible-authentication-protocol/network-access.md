---
title: Protocolo de autenticación extensible (EAP) para el acceso a la red
description: En este tema se presenta información acerca de la configuración predeterminada del Protocolo de autenticación extensible (EAP) que se puede usar para configurar equipos basados en Windows.
author: khdownie
ms.author: v-kedow
ms.topic: conceptual
ms.date: 12/28/2020
ms.openlocfilehash: a1db4a4fc78496068e3f0525de0f72d9dd569d06
ms.sourcegitcommit: e2dadc9b0c227a489a945bbc531aca5e101f18cd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804364"
---
# <a name="extensible-authentication-protocol-eap-for-network-access"></a>Protocolo de autenticación extensible (EAP) para el acceso a la red

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1

El protocolo de autenticación extensible (EAP) es un marco de arquitectura que proporciona extensibilidad para los métodos de autenticación para las tecnologías de acceso a redes protegidas que se usan habitualmente, como el acceso inalámbrico basado en IEEE 802.1 X, el acceso cableado basado en IEEE 802.1 X y las conexiones Protocolo punto a punto (PPP), como las redes privadas virtuales (VPN). EAP no es un método de autenticación como MS-CHAP v2, sino un marco en el cliente de acceso y en el servidor de autenticación que permite a los proveedores de servicios de red desarrollar e instalar fácilmente nuevos métodos de autenticación conocidos como métodos EAP.

## <a name="authentication-methods"></a>Métodos de autenticación

Este tema contiene información de configuración específica de los siguientes métodos de autenticación de EAP: Tenga en cuenta que los métodos de autenticación EAP que se usan dentro de métodos EAP de túnel se conocen comúnmente como **métodos internos** o **tipos de EAP**.

- **EAP protegido (PEAP)**

  Esta sección contiene información de configuración de los dos métodos EAP internos predeterminados que se proporcionan con PEAP.

  - EAP-Seguridad de la capa de transporte (TLS)

     Que aparecen como **propiedades de tarjeta inteligente u otro certificado** en el sistema operativo, EAP-TLS se puede implementar como un método interno para PEAP o como un método EAP independiente. Cuando se configura como un método de autenticación interno, la configuración de seguridad para EAP-TLS es idéntica a la configuración que se usa para implementar EAP-TLS como un método externo, excepto que se configura para que funcione dentro de PEAP. Para obtener detalles de configuración, consulte [elementos de configuración de las propiedades de tarjeta inteligente u otro certificado](#smart-card-or-other-certificate-properties-configuration-items).

  - EAP-Protocolo de autenticación por desafío mutuo de Microsoft versión 2 (MS-CHAP v2)

     Contraseña segura: EAP-MS-CHAP V2 es un tipo de EAP que se puede usar con PEAP para la autenticación de red basada en contraseña. EAP-MsCHAP V2 también puede usarse como un método independiente para VPN, pero solo como un método interno PEAP para redes inalámbricas.

- **EAP-Seguridad de la capa de transporte en túnel (TTLS)**

- **EAP-Módulo de identidad del suscriptor (SIM), EAP-Contrato de autenticación y claves (AKA) y EAP-AKA prima (AKA')**

  Habilita la autenticación mediante tarjetas SIM y se implementa cuando un cliente adquiere un plan de servicio de banda ancha inalámbrica de un operador de red móvil. Como parte del plan, el cliente normalmente recibe un perfil para red inalámbrica preconfigurado para la autenticación de SIM.

  En este tema se proporciona información acerca de lo siguiente:

  - [Opciones de configuración de EAP-SIM](#eap-sim-configuration-settings)
  - [Opciones de configuración de EAP-AKA y EAP-AKA](#eap-aka-configuration-settings)

## <a name="eap-tls-peap-and-eap-ttls"></a>EAP-TLS, PEAP y EAP-TTLS

Puede obtener acceso a las propiedades de EAP para acceso inalámbrico y cableado autenticado 802.1X de las siguientes formas:

- Mediante la configuración de las extensiones de directivas de redes cableadas (IEEE 802.3) y directivas de redes inalámbricas (IEEE 802.11) en directiva de grupo.

- Mediante la configuración manual de conexiones inalámbricas o cableadas en equipos cliente.

Puede obtener acceso a las propiedades de EAP para conexiones de red privada virtual (VPN) de las siguientes formas:

- Mediante el Kit de administración de Connection Manager (CMAK) para configurar conexiones VPN.

- Mediante la configuración manual de conexiones VPN en equipos cliente.

De manera predeterminada, puede configurar las opciones de EAP de los siguientes métodos de autenticación para acceso cableado autenticado 802.1X, acceso inalámbrico autenticado 802.1X y VPN:

- Microsoft: tarjeta inteligente u otro certificado (EAP-TLS)
- Microsoft: EAP protegido (PEAP)
- Microsoft: EAP-TTLS

Además, el método de autenticación de red MS-CHAP-V2 está disponible para VPN de manera predeterminada.

## <a name="protected-eap-properties-configuration-settings"></a>Opciones de configuración de las propiedades de EAP protegido

En esta sección se enumeran los valores que se pueden configurar para EAP protegido.

   > [!IMPORTANT]
   > Si se implementa el mismo tipo de autenticación para PEAP y EAP, se crea una vulnerabilidad de seguridad. Al implementar PEAP y EAP (sin proteger), no use el mismo tipo de autenticación. Por ejemplo, si implementa PEAP-TLS, no implemente también EAP-TLS.

### <a name="verify-the-servers-identity-by-validating-the-certificate"></a>Comprobar la identidad del servidor validando el certificado

Este elemento especifica que el cliente comprueba que los certificados de servidor presentados al equipo cliente tengan las firmas correctas, no hayan expirado y hayan sido emitidos por una entidad de certificación (CA) raíz de confianza. El valor predeterminado es "habilitado". **Si deshabilita esta casilla, los equipos cliente no podrán comprobar la identidad de los servidores durante el proceso de autenticación. Si no se produce la autenticación del servidor, los usuarios están expuestos a riesgos graves de seguridad, incluida la posibilidad de que los usuarios se conecten sin saberlo a una red no autorizada.**

### <a name="connect-to-these-servers"></a>Conectar a estos servidores

Este elemento le permite especificar el nombre de los servidores Servicio de autenticación remota telefónica de usuario (RADIUS) que proporcionan autorización y autenticación de red. Tenga en cuenta que debe escribir el nombre **exactamente** como aparece en el campo **asunto** de cada certificado de servidor RADIUS o usar expresiones regulares para especificar el nombre del servidor. Se puede usar la sintaxis completa de la expresión regular para especificar el nombre del servidor, pero para diferenciar una expresión regular con la cadena literal, debe usar al menos un "" en la cadena especificada. Por ejemplo, puede especificar nps.example.com para especificar el servidor RADIUS nps1.example.com o nps2.example.com.

Incluso si no se especifica ningún servidor RADIUS, el cliente comprueba que el emisor del certificado de servidor RADIUS sea una entidad de certificación raíz de confianza.

Valores predeterminados:

- Cableado e inalámbrico = no habilitado
- VPN = habilitado

### <a name="trusted-root-certification-authorities"></a>Entidades de certificación raíz de confianza

Este elemento enumera las entidades de certificación raíz de confianza. La lista se crea a partir de las CA raíz de confianza que están instaladas en el equipo y en los almacenes de certificados de usuario. Puede especificar los certificados de CA raíz de confianza que usan los suplicantes para determinar si confían en los servidores como, por ejemplo, el servidor que ejecuta el Servidor de directivas de redes (NPS) o el servidor de aprovisionamiento. Si no se selecciona ninguna CA raíz de confianza, el cliente 802.1X comprueba que el certificado de equipo del servidor RADIUS haya sido emitido por una CA raíz de confianza instalada. Si se seleccionan una o varias CA raíz de confianza, el cliente 802.1X comprueba que el certificado de equipo del servidor RADIUS haya sido emitido por una CA raíz de confianza seleccionada. Incluso si no se selecciona ninguna entidad de certificación raíz de confianza, el cliente comprueba que el emisor del certificado de servidor RADIUS sea una CA raíz de confianza.

Si tiene una infraestructura de clave pública (PKI) en la red y usa la CA para emitir certificados para los servidores RADIUS, se agregará automáticamente el certificado de CA a la lista de entidades de certificación raíz de confianza.

También puede adquirir un certificado de CA de un proveedor distinto de Microsoft. Algunas CA raíz de confianza que no son de Microsoft proporcionan software al adquirir el certificado, el cual instala automáticamente el certificado adquirido en el almacén de certificados de **entidades de certificación raíz de confianza**. En este caso, la CA raíz de confianza aparece automáticamente en la lista de CA raíz de confianza.

   > [!NOTE]
   > No especifiques un certificado de CA raíz de confianza que no aparezca en la lista de los almacenes de certificados de **entidades de certificación raíz de confianza** del equipo cliente para el **Usuario actual** y el **Equipo local**. Si designa un certificado que no esté instalado en los equipos cliente, se producirá un error de autenticación.

Valor predeterminado = no habilitado, no hay ninguna CA raíz de confianza seleccionada

### <a name="notifications-before-connecting"></a>Notificaciones antes de conectar

Este elemento especifica si se notifica al usuario si no se especifica el nombre del servidor o el certificado raíz, o si no se puede comprobar la identidad del servidor.

De forma predeterminada, se proporcionan las siguientes opciones:

- Caso 1: **No pedir al usuario que autorice nuevos servidores o entidades de certificación raíz de confianza** especifica que si:

  - El nombre de servidor no aparece en la lista **Conectarse a estos servidores**
  - o el certificado raíz se encuentra, pero no está seleccionado en la lista de **entidades de certificación raíz** de confianza en **Propiedades PEAP**
  - o el certificado raíz no se encuentra en el equipo

  no se notifica al usuario y se produce un error en el intento de conexión.

- Caso 2: **Informar al usuario si no se especificó el nombre del servidor o el certificado raíz** especifica que si:

  - El nombre de servidor no aparece en la lista **Conectarse a estos servidores**
  - o el certificado raíz se encuentra, pero no está seleccionado en la lista de **entidades de certificación raíz** de confianza en **Propiedades PEAP**

  a continuación, se solicita al usuario que acepte el certificado raíz. Si el usuario acepta el certificado, la autenticación continúa. Si el usuario rechaza el certificado, se produce un error en el intento de conexión. En esta opción, si el certificado raíz no está presente en el equipo, no se notifica al usuario y se produce un error en los intentos de conexión.

- Caso 3: **indicar al usuario si no se puede comprobar la identidad del servidor** especifica que si:

  - El nombre de servidor no aparece en la lista **Conectarse a estos servidores**
  - o el certificado raíz se encuentra, pero no está seleccionado en la lista de **entidades de certificación raíz** de confianza en **Propiedades PEAP**
  - o el certificado raíz no se encuentra en el equipo

  a continuación, se solicita al usuario que acepte el certificado raíz. Si el usuario acepta el certificado, la autenticación continúa. Si el usuario rechaza el certificado, se produce un error en el intento de conexión.

### <a name="select-authentication-method"></a>Seleccionar método de autenticación

Este elemento permite seleccionar el tipo de EAP que se va a usar con PEAP para la autenticación de red. De forma predeterminada, hay dos tipos de EAP disponibles: **contraseña segura (EAP-MSCHAP V2)** y **tarjeta inteligente u otro certificado (EAP-TLS)**. No obstante, EAP es un protocolo flexible que permite la inclusión de métodos EAP adicionales y no está restringido a estos dos tipos.

Para más información, consulte:

- [Elementos de configuración de propiedades de contraseña segura (EAP-MSCHAP V2)](#secure-password-properties-configuration-items)

- [Elementos de configuración de las propiedades de tarjeta inteligente u otro certificado](#smart-card-or-other-certificate-properties-configuration-items)

Valor predeterminado = contraseña segura (EAP-MSCHAP V2)

### <a name="configure"></a>Configurar

Este elemento proporciona acceso a la configuración de propiedades para el tipo de EAP especificado. 

### <a name="enable-fast-reconnect"></a>Habilitar reconexión rápida

Permite crear una asociación de seguridad nueva o actualizada más eficazmente o en un número menor de recorridos de ida y vuelta, en el caso de que se haya establecido previamente una asociación de seguridad.

Para las conexiones VPN, la reconexión rápida usa la tecnología IKEv2 para ofrecer una conectividad perfecta y coherente de la VPN cuando los usuarios pierden temporalmente sus conexiones a Internet. Los usuarios que se conecten mediante banda ancha móvil inalámbrica serán los que más se beneficien de esta funcionalidad.

Un ejemplo de esta ventaja es un escenario común en el que un usuario viaja en tren, usa una tarjeta de banda ancha móvil inalámbrica para conectarse a Internet y, a continuación, establece una conexión VPN con la red corporativa.

Cuando el tren pasa por un túnel, se pierde la conexión a Internet. Una vez que el tren está fuera del túnel, la tarjeta de banda ancha móvil inalámbrica vuelve a conectarse automáticamente a Internet.

La reconexión rápida vuelve a establecer automáticamente las conexiones VPN activas cuando se restablece la conectividad a Internet. Aunque la reconexión pueda tardar algunos segundos, se realiza de manera transparente para los usuarios.

Valor predeterminado = habilitado

### <a name="enforce-network-access-protection"></a>Aplicar protección de acceso a redes

Este elemento especifica que antes de que se permitan las conexiones a una red, se realizan comprobaciones de mantenimiento del sistema en los suplicantes EAP para determinar si cumplen los requisitos de mantenimiento del sistema.

Valor predeterminado = no habilitado

### <a name="disconnect-if-server-does-not-present-cryptobinding-tlv"></a>Desconectar si el servidor no presenta TLV de cryptobinding

Este elemento especifica que los clientes que se conectan deben finalizar el proceso de autenticación de red si el servidor RADIUS no presenta tipo-longitud-valor (TLV) de cryptobinding. TLV de cryptobinding aumenta la seguridad del túnel TSL en PEAP mediante la combinación de las autenticaciones de método interno y externo, de forma que los atacantes no puedan realizar ataques de tipo "Man in the middle" al redirigir una autenticación MS-CHAP v2 con el canal PEAP.

Valor predeterminado = no habilitado

### <a name="enable-identity-privacy-windows-8-only"></a>Habilitar la privacidad de identidad (solo Windows 8)

Especifica que se configuren los clientes para que no puedan enviar su identidad antes de que el cliente haya autenticado el servidor RADIUS y, de manera opcional, proporciona un espacio para escribir un valor de identidad anónima. Por ejemplo, si selecciona **Habilitar privacidad de identidad** y escribe "invitado" como valor de identidad anónima, la respuesta de identidad para un usuario con identidad alice@example es guest@example . Si selecciona **Habilitar privacidad de identidad** pero no proporciona un valor de identidad anónima, la respuesta de identidad para el usuario alice@example es @example .

Esta configuración solo se aplica a equipos que ejecutan Windows 8 y versiones anteriores.

Valor predeterminado = no habilitado

## <a name="secure-password-properties-configuration-items"></a>Elementos de configuración de propiedades de contraseña segura

La comprobación de **usar automáticamente mi nombre de inicio de sesión y contraseña de Windows (y dominio si existe)** especifica que el nombre de inicio de sesión y la contraseña de Windows del usuario actual se usan como credenciales de autenticación de red.

Valores predeterminados:

- Cableado e inalámbrico = habilitado
- VPN = no habilitado

## <a name="smart-card-or-other-certificate-properties-configuration-items"></a>Elementos de configuración de las propiedades de tarjeta inteligente u otro certificado

En esta sección se enumeran los elementos que se pueden configurar para EAP-TLS.

### <a name="use-my-smart-card"></a>Usar mi tarjeta inteligente

Este elemento especifica que los clientes que realizan solicitudes de autenticación deben presentar un certificado de tarjeta inteligente para la autenticación de red.

Valores predeterminados:

- Cableado e inalámbrico = no habilitado
- VPN = habilitado

### <a name="use-a-certificate-on-this-computer"></a>Usar un certificado en este equipo

Este elemento especifica que los clientes que se autentican deben usar un certificado ubicado en los almacenes de certificados del **usuario actual** o del **equipo local** .

Valores predeterminados:

- Cableado e inalámbrico = habilitado
- VPN = no habilitado

### <a name="use-simple-certificate-selection-recommended"></a>Usar selección de certificado simple (recomendado)

Este elemento especifica si Windows filtra los certificados que es improbable que cumplan los requisitos de autenticación. Esto sirve para limitar la lista de certificados disponibles al pedirle al usuario que se seleccione un certificado.

Valores predeterminados:

- Cableado e inalámbrico = habilitado
- VPN = no habilitado

### <a name="advanced"></a>Avanzado

Este elemento abre el cuadro de diálogo **Configurar selección de certificado** . Para obtener más información sobre la configuración de la **selección de certificados**, vea [configurar los nuevos elementos de configuración](#configure-new-certificate-selection-configuration-items)de la selección de certificados.

### <a name="verify-the-servers-identity-by-validating-the-certificate"></a>Verificar la identidad del servidor validando el certificado

Este elemento especifica que el cliente comprueba que los certificados de servidor presentados al equipo cliente tengan las firmas correctas, no hayan expirado y hayan sido emitidos por una entidad de certificación (CA) raíz de confianza. **No deshabilite esta casilla o los equipos cliente no podrán comprobar la identidad de los servidores durante el proceso de autenticación. Si no se produce la autenticación del servidor, los usuarios están expuestos a riesgos graves de seguridad, incluida la posibilidad de que los usuarios se conecten sin saberlo a una red no autorizada.**

Valor predeterminado = habilitado

### <a name="connect-to-these-servers"></a>Conectar a estos servidores

Este elemento le permite especificar el nombre de los servidores RADIUS que proporcionan autorización y autenticación de red. Tenga en cuenta que debe escribir el nombre **exactamente** como aparece en el campo asunto de cada certificado de servidor RADIUS o usar expresiones regulares para especificar el nombre del servidor. Se puede usar la sintaxis completa de la expresión regular para especificar el nombre del servidor, pero para diferenciar una expresión regular con la cadena literal, debe usar al menos un "" en la cadena especificada. Por ejemplo, puede especificar nps.example.com para especificar el servidor RADIUS nps1.example.com o nps2.example.com.

Incluso si no se especifica ningún servidor RADIUS, el cliente comprueba que el emisor del certificado de servidor RADIUS sea una entidad de certificación raíz de confianza.

Valores predeterminados:

- Cableado e inalámbrico = no habilitado
- VPN = habilitado

### <a name="trusted-root-certification-authorities"></a>Entidades de certificación raíz de confianza

Este elemento enumera las **entidades de certificación raíz de confianza**. La lista se crea a partir de las CA raíz de confianza que están instaladas en los almacenes de certificados de equipo y usuario. Puede especificar los certificados de entidades de certificación raíz de confianza que usan los suplicantes para determinar si confían en los servidores como, por ejemplo, el servidor que ejecuta NPS o el servidor de aprovisionamiento. Si no se selecciona ninguna CA raíz de confianza, el cliente 802.1X comprueba que el certificado de equipo del servidor RADIUS haya sido emitido por una CA raíz de confianza instalada. Si se seleccionan una o varias CA raíz de confianza, el cliente 802.1X comprueba que el certificado de equipo del servidor RADIUS haya sido emitido por una CA raíz de confianza seleccionada.

Si tiene una infraestructura de clave pública (PKI) en la red y usa la CA para emitir certificados para los servidores RADIUS, se agregará automáticamente el certificado de CA a la lista de entidades de certificación raíz de confianza.

También puede adquirir un certificado de CA de un proveedor distinto de Microsoft. Algunas CA raíz de confianza que no son de Microsoft proporcionan software al adquirir el certificado, el cual instala automáticamente el certificado adquirido en el almacén de certificados de **entidades de certificación raíz de confianza**. En este caso, la CA raíz de confianza aparece automáticamente en la lista de CA raíz de confianza. Incluso si no se selecciona ninguna entidad de certificación raíz de confianza, el cliente comprueba que el emisor del certificado de servidor RADIUS sea una CA raíz de confianza.

No especifiques un certificado de CA raíz de confianza que no aparezca en la lista de los almacenes de certificados de **entidades de certificación raíz de confianza** del equipo cliente para el **Usuario actual** y el **Equipo local**.

   > [!NOTE]
   > Si designa un certificado que no esté instalado en los equipos cliente, se producirá un error de autenticación.

Valor predeterminado = no habilitado, ninguna entidad de certificación raíz de confianza seleccionada

### <a name="view-certificate"></a>Ver certificado

Este elemento le permite ver las propiedades del certificado seleccionado.

### <a name="do-not-prompt-user-to-authorize-new-servers-or-trusted-certification-authorities"></a>No pedir la intervención del usuario para autorizar nuevos servidores o entidades de certificación de confianza

Este elemento impide que se solicite al usuario que confíe en un certificado de servidor si el certificado está configurado incorrectamente, no es de confianza o ambos (si están habilitados). Se recomienda activar esta casilla para simplificar la experiencia del usuario y para impedir que los usuarios seleccionen por error que confían en un servidor implementado por un atacante.

Valor predeterminado = no habilitado

### <a name="use-a-different-user-name-for-the-connection"></a>Usar un nombre de usuario distinto para la conexión

Este elemento especifica si se debe usar un nombre de usuario para la autenticación distinto del nombre de usuario en el certificado.

Valor predeterminado = no habilitado

## <a name="configure-new-certificate-selection-configuration-items"></a>Elementos de configuración para configurar Selección de certificado nuevo

Usa **Selección de certificado nuevo** para configurar los criterios que usan los equipos cliente para seleccionar automáticamente el certificado correcto en el equipo cliente para fines de autenticación. Al proporcionar la configuración a los equipos cliente de la red a través de las directivas de redes cableadas (IEEE 802.3), las directivas de redes inalámbricas (IEEE 802.11) o el Kit de administración de Connection Manager (CMAK) para VPN, se aprovisiona automáticamente a los clientes con los criterios de autenticación especificados.

En esta sección se enumeran los elementos de configuración para la **nueva selección de certificados**, junto con una descripción de cada uno.

### <a name="certificate-issuer"></a>Emisor de certificados

Este elemento especifica si está habilitado el filtrado de emisores de certificados.

Valor predeterminado = no seleccionado

### <a name="certificate-issuer-list"></a>Lista Emisor de certificado

Se usa para especificar uno o varios emisores de certificado para los certificados.

Enumera los nombres de todos los emisores para los cuales están presentes los certificados de entidad de certificación (CA) correspondientes en el almacén de certificados de **entidades de certificación raíz de confianza** o **entidades de certificación intermedias** de la cuenta del equipo local.

- Incluye todas las entidades de certificación raíz y entidades de certificación intermedias.

- Contiene solo los emisores para los cuales existen certificados válidos correspondientes en el equipo (por ejemplo, los certificados que no expiraron ni fueron revocados).

La lista final de certificados habilitados para la autenticación contiene solo aquellos certificados emitidos por alguno de los emisores seleccionados en esta lista.

Valor predeterminado = ninguno seleccionado

### <a name="extended-key-usage-eku"></a>Uso mejorado de clave (EKU)

Puede seleccionar **todos los propósitos**, la **autenticación del cliente**, **cualquier propósito** o cualquier combinación de estos. Especifica que cuando se selecciona una combinación, todos los certificados que cumplen con al menos una de las tres condiciones se consideran válidos para el fin de autenticar el cliente en el servidor. Si se habilita el filtrado de EKU, se debe seleccionar una de estas opciones; de lo contrario, el control de comando **Aceptar** estará deshabilitado.

Valor predeterminado = no habilitado

### <a name="all-purpose"></a>Todos los propósitos

Cuando se selecciona, este elemento especifica que los certificados que tienen el EKU de **todos los propósitos** se consideran certificados válidos con el fin de autenticar el cliente en el servidor.

Predeterminado = seleccionado cuando se selecciona el **uso mejorado de clave (EKU)**

### <a name="client-authentication"></a>Autenticación de clientes

Cuando se selecciona, este elemento especifica que los certificados que tienen el EKU de **autenticación de cliente** y la lista de EKU especificados se consideran certificados válidos con el fin de autenticar el cliente en el servidor.

Predeterminado = seleccionado cuando se selecciona el **uso mejorado de clave (EKU)**

### <a name="any-purpose"></a>Cualquier propósito

Cuando se selecciona, este elemento especifica que todos los certificados que tienen un EKU de **propósito** y la lista de EKU especificados se consideran certificados válidos con el fin de autenticar el cliente en el servidor.

Predeterminado = seleccionado cuando se selecciona el **uso mejorado de clave (EKU)**

### <a name="add"></a>Sumar

Este elemento abre el cuadro de diálogo **seleccionar EKU** , que permite agregar EKU estándar, personalizados o específicos del proveedor a la lista de **autenticación del cliente** o a **cualquier propósito** .

Valor predeterminado = no aparecen EKU

### <a name="remove"></a>Remove

Este elemento quita el EKU seleccionado de la lista de **autenticación del cliente** o **cualquier propósito** .

Valor predeterminado = N/D

   > [!NOTE]
   > Si **Emisor de certificado** y **Uso mejorado de clave (EKU)** están habilitados, solo aquellos certificados que cumplen con las dos condiciones se consideran válidos para el fin de autenticar el cliente con el servidor.

## <a name="select-ekus"></a>Seleccionar EKU

Puede seleccionar un EKU de la lista proporcionada o agregar un nuevo EKU.

| Elemento          | Detalles |
| :------------ | :------ |
| **Add (Agregar)**       | Abre el cuadro de diálogo **Agregar o editar EKU** , que permite definir y agregar EKU personalizados. En **Seleccione los EKU de la lista siguiente**, selecciona un EKU de la lista y, a continuación, haz clic en **Aceptar** para agregar EKU a la lista **Autenticación del cliente** o **Cualquier propósito**. |
| **Editar**      | Abre el cuadro de diálogo **Agregar o editar EKU** y permite editar los EKU personalizados que ha agregado. No se pueden editar los EKU predeterminados predefinidos. |
| **Remove**    | Quita el EKU personalizado seleccionado de la lista de EKU en el cuadro de diálogo **seleccionar EKU** . No se pueden quitar los EKU predeterminados predefinidos. |

## <a name="add-or-edit-eku"></a>Agregar o editar EKU

| Elemento          | Detalles |
| :------------ | :------ |
| **Escriba el nombre del EKU** | Proporciona un lugar para escribir el nombre del EKU personalizado. |
| **Escriba el OID del EKU** | Proporciona un lugar para escribir el OID del EKU. Solo se permiten dígitos numéricos, separadores y “.”. Se permiten los caracteres comodín, en cuyo caso se permiten también todos los OID de los elementos secundarios de la jerarquía. Por ejemplo, si se escribe 1.3.6.1.4.1.311.*, se permiten 1.3.6.1.4.1.311.42 y 1.3.6.1.4.1.311.42.2.1 |

## <a name="ttls-configuration-items"></a>Elementos de configuración de TTLS

EAP-TTLS es un método de túnel EAP basado en estándares que admite la autenticación mutua y proporciona un túnel seguro para la autenticación de la inclusión de clientes mediante métodos EAP y otros protocolos heredados. La adición de EAP-TTLS en Windows Server 2012 solo proporciona compatibilidad del lado cliente, con el fin de admitir la interoperación con los servidores RADIUS que se implementan con más frecuencia y que admiten EAP-TTLS.

En esta sección se enumeran los elementos que se pueden configurar para EAP-TTLS.

### <a name="enable-identity-privacy-windows-8-only"></a>Habilitar la privacidad de identidad (solo Windows 8)

Este elemento especifica que los clientes están configurados de forma que no puedan enviar su identidad antes de que el cliente haya autenticado el servidor RADIUS y, opcionalmente, proporciona un lugar para escribir un valor de identidad anónima. Por ejemplo, si selecciona **Habilitar privacidad de identidad** y escribe "invitado" como valor de identidad anónima, la respuesta de identidad para un usuario con identidad alice@example es guest@example . Si selecciona **Habilitar privacidad de identidad** pero no proporciona un valor de identidad anónima, la respuesta de identidad para el usuario alice@example es @example .

Esta configuración solo se aplica a equipos que ejecutan Windows 8.

Valor predeterminado = no habilitado

### <a name="connect-to-these-servers"></a>Conectar a estos servidores

Este elemento le permite especificar el nombre de los servidores RADIUS que proporcionan autorización y autenticación de red. Tenga en cuenta que debe escribir el nombre **exactamente** como aparece en el campo asunto de cada certificado de servidor RADIUS o usar expresiones regulares para especificar el nombre del servidor. La sintaxis completa de la expresión regular puede usarse para especificar el nombre de servidor. Pero para diferenciar una expresión regular con la cadena literal, debe usar al menos un * en la cadena especificada. Por ejemplo, puedes especificar nps*.example.com para especificar el servidor RADIUS nps1.example.com o nps2.example.com. Incluso si no se especifica ningún servidor RADIUS, el cliente comprueba que el emisor del certificado de servidor RADIUS sea una entidad de certificación raíz de confianza.

Valor predeterminado = ninguno

### <a name="trusted-root-certification-authorities"></a>Entidades de certificación raíz de confianza

Este elemento enumera las **entidades de certificación raíz de confianza**. La lista se crea a partir de las CA raíz de confianza que están instaladas en los almacenes de certificados de equipo y usuario. Puede especificar los certificados de entidades de certificación raíz de confianza que usan los suplicantes para determinar si confían en los servidores como, por ejemplo, el servidor que ejecuta NPS o el servidor de aprovisionamiento. Si no se selecciona ninguna CA raíz de confianza, el cliente 802.1X comprueba que el certificado de equipo del servidor RADIUS haya sido emitido por una CA raíz de confianza instalada. Si se seleccionan una o varias CA raíz de confianza, el cliente 802.1X comprueba que el certificado de equipo del servidor RADIUS haya sido emitido por una CA raíz de confianza seleccionada. Incluso si no se selecciona ninguna entidad de certificación raíz de confianza, el cliente comprueba que el emisor del certificado de servidor RADIUS sea una CA raíz de confianza.

Si tiene una infraestructura de clave pública (PKI) en la red y usa la CA para emitir certificados para los servidores RADIUS, se agregará automáticamente el certificado de CA a la lista de entidades de certificación raíz de confianza. Si se selecciona, el certificado de CA raíz se instala en el equipo cliente cuando los equipos se unen al dominio.

También puede adquirir un certificado de CA de un proveedor distinto de Microsoft. Algunas CA raíz de confianza que no son de Microsoft proporcionan software al adquirir el certificado, el cual instala automáticamente el certificado adquirido en el almacén de certificados de **entidades de certificación raíz de confianza**. En este caso, la CA raíz de confianza aparece automáticamente en la lista de CA raíz de confianza.

No especifiques un certificado de CA raíz de confianza que no aparezca en la lista de los almacenes de certificados de **entidades de certificación raíz de confianza** del equipo cliente para el **Usuario actual** y el **Equipo local**.

   > [!NOTE]
   > Si designa un certificado que no esté instalado en los equipos cliente, se producirá un error de autenticación.

Valor predeterminado = no habilitado, no hay ninguna CA raíz de confianza seleccionada

### <a name="do-not-prompt-user-if-unable-to-authorize-server"></a>No avisar al usuario si no se puede autorizar el servidor

Este elemento especifica (si no se selecciona) que si se produce un error en la validación del certificado de servidor debido a alguna de las siguientes razones, se solicita al usuario que acepte o rechace el servidor:

- Un certificado raíz para el certificado de servidor no se encuentra o no se selecciona en la lista de **entidades de certificación raíz de confianza**.
- No se encuentra uno o varios de los certificados raíz intermedios de la cadena de certificados.
- El nombre de sujeto del certificado de servidor no coincide con ninguno de los servidores que se especifican en la lista **Conectarse a estos servidores**.

Valor predeterminado = no seleccionado

### <a name="select-a-non-eap-method-for-authentication"></a>Seleccione un método que no sea EAP para la autenticación

Especifica si un método que no es EAP o un tipo de EAP se usa para la autenticación. Si está seleccionada **la opción seleccionar un método no EAP para la autenticación** , **Seleccione un método EAP para la autenticación** está deshabilitado. Si se selecciona **Seleccione un método que no sea EAP para la autenticación**, se proporcionan los siguientes tipos de autenticación que no sean EAP en la lista desplegable:

- PAP
- CHAP
- MS-CHAP
- MS-CHAP v2

Valores predeterminados:

- Seleccione un método que no sea EAP para la autenticación = habilitado
- Tipo que no sea de EAP = PAP

### <a name="automatically-use-my-windows-logon-name-and-password"></a>Usar automáticamente mi nombre de inicio y contraseña de Windows

Cuando está habilitada, este elemento usa las credenciales de inicio de sesión de Windows. Esta casilla solo se activa si se selecciona **MS-CHAP v2** en la lista desplegable **Seleccione un método que no sea EAP para la autenticación**. **Usar automáticamente mi nombre de inicio de sesión y contraseña de Windows** está deshabilitado para los tipos de autenticación **PAP**, **CHAP** y **MS-CHAP** .

### <a name="select-an-eap-method-for-authentication"></a>Seleccione un método EAP para la autenticación

Este elemento especifica si se utiliza un tipo de EAP o un tipo no EAP para la autenticación. Si está seleccionada **la opción seleccionar un método EAP para la autenticación** , **Seleccione un método que no sea EAP para la autenticación** está deshabilitado. Si se selecciona **Seleccione un método que no sea EAP para la autenticación**, se proporcionan los siguientes tipos de autenticación que no sean EAP en la lista desplegable de manera predeterminada:

- Microsoft: tarjeta inteligente u otro certificado
- Microsoft: MS-CHAP v2
- MS-CHAP
- MS-CHAP v2

   > [!NOTE]
   > La lista desplegable **Seleccione un método EAP para la autenticación** enumerará todos los métodos EAP que están instalados en el servidor, excepto los métodos de túnel **rápido** y **PEAP** . Los tipos de EAP se enumeran en el orden en que el equipo los detecta.

### <a name="configure"></a>Configurar

Abre el cuadro de diálogo de propiedades del tipo de EAP especificado. Para obtener más información acerca de los tipos de EAP predeterminados, vea elementos de configuración de [propiedades de tarjeta inteligente u otros certificados](#smart-card-or-other-certificate-properties-configuration-items) o de [contraseña segura (EAP-MSCHAP V2)](#secure-password-properties-configuration-items).

## <a name="eap-sim-configuration-settings"></a>Opciones de configuración de EAP-SIM

EAP Módulo de identidad del suscriptor (SIM) se usa para la autenticación y distribución de claves de sesión del Sistema global para comunicaciones móviles (GSM). EAP-SIM se define en RFC 4186.

En la tabla siguiente se enumeran los valores de configuración para EAP-SIM.

| Elemento                                                                           | Descripción |
| :----------------------------------------------------------------------------- | :---------- |
| **Usar claves de cifrado seguras**                                                     | Especifica que si se selecciona, el perfil usa un cifrado de alta seguridad. |
| **No revelar la identidad real al servidor cuando hay disponible una de seudónimo** | Si se habilita, fuerza al cliente para que genere un error en la autenticación si las solicitudes del servidor de una identidad permanente a través del cliente tienen una identidad de seudónimo con ellas. Las identidades de seudónimo se usan para dar privacidad a la identidad, de modo que la identidad real o permanente de un usuario no se revele durante la autenticación. |
| **Habilitar el uso de dominios kerberos**                                                     | Proporciona un lugar para escribir el nombre de dominio kerberos. Si este campo se deja en blanco con la opción **Habilitar el uso de dominios Kerberos** seleccionada, el dominio Kerberos se deriva de la identidad del suscriptor móvil internacional (IMSI) mediante el 3GPP.org de dominio Kerberos, tal como se describe en el estándar 23,003 v 6.8.0 del del proyecto de Asociación de tercera generación (3GPP). |
| **Especificar un dominio kerberos**                                                            | Proporciona un lugar para escribir el nombre de dominio Kerberos. |

## <a name="eap-aka-configuration-settings"></a>Opciones de configuración de EAP-AKA

EAP Contrato de autenticación y claves (AKA) del Sistema universal de telecomunicaciones móviles (UMTS) se usa para la autenticación y la distribución de claves de sesión mediante el Módulo de identidad de suscriptor universal (USIM) de UMTS. EAP AKA se define en RFC 4187.

En la tabla siguiente se enumeran los valores de configuración para EAP-AKA.

| Elemento                                                                           | Descripción |
| :----------------------------------------------------------------------------- | :---------- |
| **No revelar la identidad real al servidor cuando hay disponible una de seudónimo** | Si se habilita, fuerza al cliente para que genere un error en la autenticación si las solicitudes del servidor de una identidad permanente a través del cliente tienen una identidad de seudónimo con ellas. Las identidades de seudónimo se usan para dar privacidad a la identidad, de modo que la identidad real o permanente de un usuario no se revele durante la autenticación. |
| **Habilitar el uso de dominios kerberos**                                                     | Proporciona un lugar para escribir el nombre de dominio kerberos. Si este campo se deja en blanco con la opción **Habilitar el uso de dominios Kerberos** seleccionada, el dominio Kerberos se deriva de la identidad del suscriptor móvil internacional (IMSI) mediante el 3GPP.org de dominio Kerberos, tal como se describe en el estándar 23,003 v 6.8.0 del del proyecto de Asociación de tercera generación (3GPP). |
| **Especificar un dominio kerberos**                                                            | Proporciona un lugar para escribir el nombre de dominio kerberos. |

## <a name="eap-aka-configuration-settings"></a>Opciones de configuración de EAP-AKA

EAP-AKA prima (AKA') es una versión modificada de EAP-AKA que se usa para habilitar el acceso a las redes basadas en el Proyecto de asociación de tercera generación (3GPP) mediante estándares que no sean de 3GPP, como:

- WiFi (también conocido como wireless fidelity)

- datos de evolución optimizados (EVDO)

- Interoperabilidad mundial para acceso por microondas (WiMax)

EAP-AKA' se define en RFC 5448.

En la tabla siguiente se enumeran los valores de configuración para EAP-AKA '.

| Elemento                                                                           | Descripción |
| :----------------------------------------------------------------------------- | :---------- |
| **No revelar la identidad real al servidor cuando hay disponible una de seudónimo** | Si se habilita, fuerza al cliente para que genere un error en la autenticación si las solicitudes del servidor de una identidad permanente a través del cliente tienen una identidad de seudónimo con ellas. Las identidades de seudónimo se usan para dar privacidad a la identidad, de modo que la identidad real o permanente de un usuario no se revele durante la autenticación. |
| **Habilitar el uso de dominios kerberos**                                                     | Proporciona un lugar para escribir el nombre de dominio kerberos. Si este campo se deja en blanco con la opción **Habilitar el uso de dominios Kerberos** seleccionada, el dominio Kerberos se deriva de la identidad del suscriptor móvil internacional (IMSI) mediante el 3GPP.org de dominio Kerberos, tal como se describe en el estándar 23,003 v 6.8.0 del del proyecto de Asociación de tercera generación (3GPP). |
| **Especificar un dominio kerberos**                                                            | Proporciona un lugar para escribir el nombre de dominio kerberos. |
| **Pasar por alto error de coincidencia de nombres de red**                                               | El cliente compara el nombre de red que conoce, con el nombre que envió el servidor RADIUS durante la autenticación. Si no coincide, se omite si se selecciona esta opción. Si no se selecciona, se produce un error en la autenticación. |
| **Habilitar la reautenticación rápida**                                               | Especifica que la reautenticación rápida está habilitada. La reautenticación rápida es útil cuando la autenticación de SIM se lleva a cabo con frecuencia. Las claves de cifrado que derivan de la autenticación completa se reutilizan. Como resultado, no se requiere que el algoritmo de SIM se ejecute en cada uno de los intentos de autenticación y el número de operaciones de red que derivan de los intentos de autenticación frecuentes se reduce. |

## <a name="additional-resources"></a>Recursos adicionales

Para obtener información adicional sobre la configuración inalámbrica autenticada en directiva de grupo, consulte [Administración de las opciones de configuración de directivas de red inalámbrica (IEEE 802,11)](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh994701(v=ws.11)) .

Para obtener más información acerca de la configuración de redes cableadas en directiva de grupo, consulte [Administración de la nueva configuración de directivas de red cableada (IEEE 802,3)](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831813(v=ws.11)) .

Para obtener información sobre la configuración avanzada del acceso cableado autenticado y el acceso inalámbrico autenticado, consulte [configuración de seguridad avanzada para directivas de red cableada e inalámbrica](/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh994696(v=ws.11)).
