---
title: Guía de planificación de máquinas virtuales blindadas y tejido protegido para inquilinos
ms.prod: windows-server
ms.topic: article
ms.assetid: 392af37f-a02d-4d40-a25d-384211cbbfdd
manager: dongill
author: nirb-ms
ms.author: nirb
ms.technology: security-guarded-fabric
ms.openlocfilehash: 5d50721ddf51bc956d8afd256ff047c94c5d5a5e
ms.sourcegitcommit: acfdb7b2ad283d74f526972b47c371de903d2a3d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/05/2020
ms.locfileid: "87769453"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-tenants"></a>Guía de planificación de máquinas virtuales blindadas y tejido protegido para inquilinos

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Este tema se centra en los propietarios de VM que desean proteger sus máquinas virtuales (VM) para fines de cumplimiento y seguridad. Independientemente de si las máquinas virtuales se ejecutan en un tejido protegido de un proveedor de hospedaje o en un tejido protegido privado, los propietarios de máquinas virtuales deben controlar el nivel de seguridad de sus máquinas virtuales blindadas, lo que incluye mantener la capacidad de descifrarlas si es necesario.

Hay tres áreas que se deben tener en cuenta al usar máquinas virtuales blindadas:

- El nivel de seguridad de las máquinas virtuales
- Claves criptográficas utilizadas para protegerlas
- Datos de blindaje: información confidencial que se usa para crear máquinas virtuales blindadas

## <a name="security-level-for-the-vms"></a>Nivel de seguridad de las máquinas virtuales

Al implementar máquinas virtuales blindadas, se debe seleccionar uno de los dos niveles de seguridad:

- Blindada
- Cifrado admitido

Tanto las máquinas virtuales blindadas como las que admiten cifrado tienen un TPM virtual conectado y las que ejecutan Windows están protegidas por BitLocker. La principal diferencia es que las máquinas virtuales blindadas bloquean el acceso por parte de los administradores del tejido mientras que las máquinas virtuales compatibles con el cifrado permiten a los administradores del tejido el mismo nivel de acceso que tenían para una máquina virtual normal. Para más información sobre estas diferencias, consulte [información general sobre el tejido protegido y las máquinas virtuales blindadas](guarded-fabric-and-shielded-vms.md).

Elija **máquinas virtuales blindadas** si desea proteger la máquina virtual de un tejido comprometido (incluidos los administradores en peligro). Deben usarse en entornos en los que los administradores del tejido y el propio tejido no son de confianza. Elija **cifrado máquinas virtuales compatibles** si desea cumplir una barra de cumplimiento que pueda requerir cifrado en reposo y cifrado de la máquina virtual en la conexión (por ejemplo, durante la migración en vivo).

Las máquinas virtuales compatibles con el cifrado son ideales en entornos donde los administradores del tejido son de plena confianza, pero el cifrado sigue siendo un requisito.

Puede ejecutar una mezcla de máquinas virtuales normales, máquinas virtuales blindadas y máquinas virtuales compatibles con el cifrado en un tejido protegido e incluso en el mismo host de Hyper-V.

Si una máquina virtual está blindada o se admite el cifrado, se determinan mediante los datos de blindaje seleccionados al crear la máquina virtual. Los propietarios de máquinas virtuales configuran el nivel de seguridad al crear los datos de blindaje (consulte la sección [datos de blindaje](#shielding-data) ).
Tenga en cuenta que, una vez realizada esta opción, no se puede cambiar mientras la máquina virtual permanezca en el tejido de virtualización.

## <a name="cryptographic-keys-used-for-shielded-vms"></a>Claves criptográficas que se usan para las máquinas virtuales blindadas

Las máquinas virtuales blindadas están protegidas contra vectores de ataque de tejido de virtualización que usan discos cifrados y otros elementos cifrados que solo puede descifrar:

- Una clave de propietario: se trata de una clave criptográfica mantenida por el propietario de la máquina virtual que se usa normalmente para la recuperación del último recurso o la solución de problemas. Los propietarios de máquinas virtuales son responsables del mantenimiento de las claves de propietario en una ubicación segura.
- Una o más protecciones (claves de protección de host): cada guardián representa un tejido de virtualización en el que un propietario autoriza la ejecución de máquinas virtuales blindadas. Las empresas suelen tener un tejido de virtualización principal y de recuperación ante desastres (DR) y, por lo general, autorizar a sus máquinas virtuales blindadas a ejecutarse en ambos. En algunos casos, el tejido secundario (DR) podría estar hospedado por un proveedor de nube pública. Las claves privadas de cualquier tejido protegido solo se mantienen en el tejido de virtualización, mientras que sus claves públicas se pueden descargar y se encuentran dentro de su guardián.

**¿Cómo crear una clave de propietario?** Una clave de propietario se representa mediante dos certificados. Un certificado para el cifrado y un certificado para firmar. Puede crear estos dos certificados con su propia infraestructura de PKI u obtener certificados SSL de una entidad de certificación (CA) pública. Con fines de prueba, también puede crear un certificado autofirmado en cualquier equipo que empiece con Windows 10 o Windows Server 2016.

**¿Cuántas claves de propietario debe tener?** Puede usar una sola clave de propietario o varias claves de propietario. Los procedimientos recomendados recomiendan una única clave de propietario para un grupo de máquinas virtuales que comparten la misma seguridad, confianza o nivel de riesgo, y para el control administrativo. Puede compartir una única clave de propietario para todas las máquinas virtuales blindadas Unidas a un dominio y custodiar la clave propietaria que administrarán los administradores de dominio.

**¿Puedo usar mis propias claves para el guardián del host?** Sí, puede "traiga su propia clave" al proveedor de hospedaje y use esa clave para las máquinas virtuales blindadas. Esto le permite usar sus claves específicas (en lugar de usar la clave de proveedor de hospedaje) y se puede usar cuando tenga una seguridad o normativa específica que deba respetar. Para fines de higiene clave, las claves de protección de host deben ser diferentes de la clave de propietario.

## <a name="shielding-data"></a>Datos de blindaje

Los datos de blindaje contienen los secretos necesarios para implementar máquinas virtuales blindadas o compatibles con el cifrado. También se usa al convertir máquinas virtuales normales en máquinas virtuales blindadas.

Los datos de blindaje se crean mediante el Asistente para archivos de datos de blindaje y se almacenan en archivos PDK que los propietarios de máquinas virtuales cargan en el tejido protegido.

Las máquinas virtuales blindadas ayudan a protegerse frente a ataques de un tejido de virtualización en peligro, por lo que necesitamos un mecanismo seguro para pasar datos de inicialización confidenciales, como la contraseña del administrador, las credenciales de unión al dominio o los certificados RDP, sin revelarlos al tejido de virtualización en sí o a sus administradores. Además, los datos de blindaje contienen lo siguiente:

1. Nivel de seguridad: protegido o compatible con cifrado
2. Propietario y lista de protecciones de host de confianza en las que se puede ejecutar la máquina virtual
3. Datos de inicialización de máquina virtual (unattend.xml, certificado RDP)
4. Lista de discos de plantilla firmada de confianza para crear la máquina virtual en el entorno de virtualización

Al crear una máquina virtual blindada o compatible con el cifrado o al convertir una máquina virtual existente, se le pedirá que seleccione los datos de blindaje en lugar de que se le solicite la información confidencial.

**¿Cuántos archivos de datos de blindaje necesito?** Se puede usar un único archivo de datos de blindaje para crear cada máquina virtual blindada. Sin embargo, si una máquina virtual blindada determinada requiere que cualquiera de los cuatro elementos sea diferente, es necesario un archivo de datos de blindaje adicional. Por ejemplo, podría tener un archivo de datos de blindaje para el Departamento de ti y un archivo de datos de blindaje diferente para el Departamento de recursos humanos porque la contraseña de administrador inicial y los certificados RDP diferían.

Aunque es posible usar archivos de datos de blindaje independientes para cada máquina virtual blindada, no es necesariamente la opción óptima y debe realizarse por las razones adecuadas. Por ejemplo, si todas las máquinas virtuales blindadas necesitan tener una contraseña de administrador diferente, considere la posibilidad de usar en su lugar un servicio de administración de contraseñas o una herramienta como [la solución de contraseña de administrador local de Microsoft (laps)](https://www.microsoft.com/download/details.aspx?id=46899).

## <a name="creating-a-shielded-vm-on-a-virtualization-fabric"></a>Creación de una máquina virtual blindada en un tejido de virtualización

Hay varias opciones para crear una máquina virtual blindada en un tejido de virtualización (lo siguiente es pertinente para las máquinas virtuales blindadas y compatibles con el cifrado):

1. Creación de una máquina virtual blindada en el entorno y su carga en el tejido de virtualización
2. Creación de una nueva máquina virtual blindada a partir de una plantilla firmada en el tejido de virtualización
3. Blindar una máquina virtual existente (la máquina virtual existente debe ser de generación 2 y debe ejecutar Windows Server 2012 o posterior)

La creación de nuevas máquinas virtuales a partir de una plantilla es una práctica normal. Sin embargo, puesto que el disco de plantilla que se usa para crear una nueva máquina virtual blindada reside en el tejido de virtualización, son necesarias medidas adicionales para asegurarse de que no se han manipulado por un administrador de tejido malintencionado o por malware que se ejecuta en el tejido. Este problema se resuelve mediante el uso de discos de plantilla firmados: los discos de plantilla firmados y sus firmas de disco se crean mediante administradores de confianza o el propietario de la máquina virtual. Cuando se crea una máquina virtual blindada, la firma del disco de plantilla se compara con las firmas contenidas en el archivo de datos de blindaje especificado. Si alguna de las firmas del archivo de datos de blindaje coincide con la firma del disco de plantilla, el proceso de implementación continúa. Si no se encuentra ninguna coincidencia, se anula el proceso de implementación, lo que garantiza que los secretos de la máquina virtual no se verán comprometidos debido a un disco de plantilla que no es de confianza.

Al usar discos de plantilla firmados para crear máquinas virtuales blindadas, hay dos opciones disponibles:

1. Use un disco de plantilla firmado existente proporcionado por el proveedor de virtualización. En este caso, el proveedor de virtualización mantiene los discos de plantilla firmados.
2. Cargue un disco de plantilla firmado en el tejido de virtualización. El propietario de la máquina virtual es responsable de mantener los discos de plantilla firmados.


