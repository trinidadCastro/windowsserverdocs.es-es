---
title: Guía de planeación de los proveedores de hospedaje VM blindada y tejido protegido
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 392af37f-a02d-4d40-a25d-384211cbbfdd
manager: dongill
author: nirb-ms
ms.technology: security-guarded-fabric
ms.openlocfilehash: 0a43cedf8ce0138e89624ff3df5bc7088f583252
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875306"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-tenants"></a>Máquina virtual blindada Guía de planificación de los inquilinos y tejido protegido

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

En este tema se centra en los propietarios de máquina virtual que le gustaría proteger sus máquinas virtuales (VM) con fines de seguridad y cumplimiento. Independientemente de si las máquinas virtuales se ejecutan en el tejido protegido de un proveedor de hospedaje o de un tejido protegido privado, los propietarios de la máquina virtual necesitan controlar el nivel de seguridad de sus máquinas virtuales blindadas, que incluye el mantenimiento de la capacidad de descifrar los archivos si es necesario.

Hay tres áreas a tener en cuenta al usar máquinas virtuales blindadas:

- El nivel de seguridad para las máquinas virtuales
- Las claves criptográficas utilizadas para protegerlos
- Los datos de blindaje: información confidencial que se usa para crear máquinas virtuales blindadas 

## <a name="security-level-for-the-vms"></a>Nivel de seguridad de las máquinas virtuales

Al implementar las máquinas virtuales blindadas, debe seleccionarse uno de dos niveles de seguridad:

- Blindada 
- Cifrado admitido

Ambos blindadas y admiten el cifrado de las máquinas virtuales tienen un TPM virtual asociado a ellos y aquellos que la ejecución de Windows están protegidos por BitLocker. La principal diferencia es que blindadas máquinas virtuales de bloquear el acceso por los administradores del tejido, mientras que admiten el cifrado de las máquinas virtuales permiten a los administradores de tejido del mismo nivel de acceso que tendría a una máquina virtual normal. Para obtener más detalles sobre estas diferencias, vea [protegidos fabric y blindadas información general de las máquinas virtuales](guarded-fabric-and-shielded-vms.md). 

Elija **las máquinas virtuales blindadas** si desea para proteger la máquina virtual de un tejido comprometido (incluidos los administradores en peligro). Se debe usar en entornos donde los administradores de fabric y el tejido de sí mismo no son de confianza. Elija **máquinas virtuales de cifrado compatibles con** si desea para cumplir con una barra de compatibilidad que puede requerir cifrado en reposo y el cifrado de la máquina virtual en la conexión (por ejemplo, durante la migración en vivo).

Máquinas virtuales compatibles con el cifrado son ideales en entornos donde los administradores del tejido son de plena confianza, pero cifrado sigue siendo un requisito.

Puede ejecutar una combinación de las máquinas virtuales normales, las máquinas virtuales blindadas y máquinas virtuales compatibles con el cifrado en un tejido protegido e incluso en el mismo host de Hyper-V. 

Si una máquina virtual está blindada o admite el cifrado viene determinada por los datos de blindaje que se selecciona al crear la máquina virtual. Los propietarios de la máquina virtual configuración el nivel de seguridad al crear los datos de blindaje (consulte la [los datos de blindaje](#shielding-data) sección).
Tenga en cuenta que una vez que se ha realizado esta opción, no se puede cambiar mientras la máquina virtual permanece en el tejido de virtualización.

## <a name="cryptographic-keys-used-for-shielded-vms"></a>Claves criptográficas utilizadas para las máquinas virtuales blindadas

Las máquinas virtuales blindadas están protegidas contra virtualización fabric vectores de ataque con discos cifrados y varios otros elementos cifrados que solo se pueden descifrar:

- Una clave de propietario: se trata de una clave criptográfica mantenida por el propietario de máquina virtual que se utiliza normalmente para la última recuperación o solución de problemas. Los propietarios de la máquina virtual son responsables de mantener las claves de propietario en una ubicación segura.
- Uno o más tutores (claves de guardián de Host): cada protección representa a un tejido de virtualización en el que un propietario autoriza para ejecutar máquinas virtuales blindadas. Las empresas a menudo tienen un elemento principal y un tejido de virtualización de la recuperación ante desastres y normalmente harían autorizar a sus máquinas virtuales blindadas para ejecutarse en ambos. En algunos casos, el tejido (DR) secundaria puede ser hospedado por un proveedor de nube pública. Las claves privadas para cualquier tejido protegido se mantienen solo en el tejido de virtualización, mientras que sus claves públicas se pueden descargar y están dentro de su protección. 

**¿Cómo se puede crear una clave de propietario?** Una clave de propietario se representa mediante dos certificados. Un certificado para el cifrado y un certificado para firmar. Puede crear estos dos certificados con su propia infraestructura PKI u obtener certificados SSL de una entidad de certificación pública (CA). Para fines de prueba, también puede crear un certificado autofirmado en cualquier equipo a partir de Windows 10 o Windows Server 2016.

**¿Cuántas claves propietario debe tener?** Puede usar una clave de propietario único o varias claves de propietario. Se recomienda una clave única de propietario para un grupo de máquinas virtuales que comparten la misma seguridad, nivel de confianza o riesgo y para el control administrativo. Puede compartir una clave única de propietario para todas las VM blindadas unido al dominio y asignarles una custodia esa clave del propietario que administrará los administradores de dominio.

**¿Puedo usar mis propias claves para la protección de Host?** Sí, puede "Bring Your Own" clave para el hospedaje proveedor y use esa clave para las máquinas virtuales blindadas. Esto le permite usar sus claves específicas (en comparación con la clave de proveedor de hospedaje) y se puede usar cuando haya seguridad específico o normativa que deba cumplir. Con fines de higiene de clave, las claves de guardián de Host deben ser diferentes de la clave del propietario.

## <a name="shielding-data"></a>Los datos de blindaje

Los datos de blindaje contienen los secretos necesarios para implementar máquinas virtuales blindadas o admite el cifrado. También se usa al convertir las máquinas virtuales normales a las máquinas virtuales blindadas.

Los datos de blindaje se crean mediante el Asistente de archivo de datos de blindaje y se almacenan en archivos PDK que los propietarios de la máquina virtual se cargue en el tejido protegido.

Las máquinas virtuales blindadas ayudar a proteger contra ataques procedentes de un tejido de virtualización en peligro, por lo que necesitamos un mecanismo seguro para transferir datos de inicialización confidenciales, como contraseña del administrador, credenciales de unión al dominio o los certificados RDP, sin revelar a el mismo tejido de virtualización o a sus administradores. Además, los datos de blindaje contiene lo siguiente:

1. Nivel: blindado o admiten el cifrado de seguridad
2. Propietario y la lista de protecciones de Host de confianza donde se puede ejecutar la máquina virtual
3. Datos de inicialización de la máquina virtual (unattend.xml, certificado RDP)
4. Lista de los discos de plantilla firmados de confianza para crear la máquina virtual en el entorno de virtualización 

Al crear una máquina virtual blindada o admite el cifrado o convertir una máquina virtual existente, se le pedirá que seleccione los datos de blindaje en lugar de que se le pida la información confidencial.

**¿Cuántos archivos de datos de blindaje es necesario?** Un único archivo de datos de blindaje puede usarse para crear todas las máquinas virtuales blindadas. Si, sin embargo, una máquina virtual blindada determinada requiere que cualquiera de los cuatro elementos diferentes, un archivo de datos de blindaje adicional es necesario. Por ejemplo, podría tener un archivo de datos de blindaje para el departamento de TI y un archivo de datos de blindaje diferente para el departamento de recursos humanos porque su contraseña de administrador inicial y los certificados RDP son distintas.

Aunque es posible usar archivos de datos de blindaje independientes para cada máquina virtual blindada, no es necesariamente la mejor opción y debe realizarse por los motivos apropiados. Por ejemplo, considere si todas las máquinas virtuales blindadas debe tener una contraseña de administrador diferente, en lugar de utilizar la administración de contraseñas de servicio o herramienta, como [Local administrador contraseña solución (LAPS) de Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=46899).

## <a name="creating-a-shielded-vm-on-a-virtualization-fabric"></a>Creación de una máquina virtual blindada en un tejido de virtualización

Hay varias opciones para crear una máquina virtual blindada en un tejido de virtualización (el siguiente es relevante para las máquinas virtuales blindadas y admiten el cifrado):

1. Crear una máquina virtual blindada en su entorno y cargarlo en el tejido de virtualización
2. Crear una nueva máquina virtual blindada desde una plantilla con signo en el tejido de virtualización
3. Escudo de una máquina virtual existente (la máquina virtual existente debe ser de generación 2 y debe ejecutar Windows Server 2012 o posterior)

Creación de nuevas máquinas virtuales desde una plantilla es una práctica habitual. Sin embargo, puesto que el disco de plantilla que se usa para crear nueva máquina virtual blindada reside en el tejido de virtualización, son necesarias para asegurarse de que no se ha manipulado por un administrador de tejido malintencionado o malware que se ejecutan en el tejido de medidas adicionales. Este problema se resuelve utilizando los discos de plantilla firmada, los discos de plantilla firmada y sus firmas de disco se crean los administradores de confianza o el propietario de la máquina virtual. Cuando se crea una máquina virtual blindada, la firma del disco de plantilla se compara con las firmas contenidas en el archivo de datos de blindaje especificado. Si cualquiera de las firmas del archivo de datos de blindaje coincide con la firma del disco de plantilla, el proceso de implementación continúa. Si puede se encuentra ninguna coincidencia, se anula el proceso de implementación, lo que garantiza que los secretos de máquina virtual no correrá peligro debido a un disco de plantilla no es de confianza.

Al usar discos de plantilla firmada para crear máquinas virtuales blindadas, existen dos opciones:

1. Usar un disco de plantilla firmada existente proporcionada por el proveedor de virtualización. En este caso, el proveedor de virtualización mantiene discos de plantilla firmada.
2. Cargar un disco de plantilla firmada en el tejido de virtualización. El propietario de la máquina virtual es responsable de mantener los discos de plantilla firmada. 


