## Modelos de Implementación de Nube

### Nube Privada

Toda la infraestructura lo gestiona una única organización. Puede estar alojada en las instalaciones del cliente (on-premises) o ser proporcionada por un proveedor en sus propios centros de datos. Para poder acceder a la nube se requiere de poder el equipo de TI debe configurar un acceso especial a la red.

* Tiene el control total de la infraestructura y seguridad.
* Sus recursos son dedicados exclusivamente a una organización.


### Nube Pública

Los recursos de la infraestructura son propiedad del proveedor de servicios de la nube (AWS, Azure, GCP, etc.), y es compartida por varios usuarios (empresas o personas). El acceso se realiza a través de internet.

* Los recursos son compartidos por múltiples clientes.
* Alta escalabilidad y recursos de bajo costo debido a la economía de escala.

### Nube Híbrida

Combina tanto la nube privada y la pública, permitiendo que los datos y aplicaciones se muevan entre ellos. Esta combinación permite flexibilidad y opciones de optimización. Un ejemplo de esta opción es solo el uso de la nube pública cuando se espera un gran aumento en la carga de trabajo en cierta fecha (black friday).

* Permite mover cargas de trabajos entre ellos
* Las organizaciones pueden aprovechar la seguridad y el control de la nube privada junto con la escalabilidad de la nube pública.

Eso se puede realizar mediante:

* Azure Arc: Permite gestionar recursos híbridos.
* AWS Outposts: Permite extender los servicios de AWS a centro de datos locales.

### Nube Comunitaria

Los recursos son compartidos por un grupo específico de organizaciones que tienen un interés o problemática en común, como investigaciones de seguridad pública, temas gubernamentales, investigaciones médicas, etc.

* Puede ser gestionada por una de las organizaciones participantes o por un proveedor externo.

### Multicloud

Es la combinación de múltiples proveedores de nube, esto es útil a la hora de optimizar los gastos de la implementación de una aplicación o servicio, podría elegir en guardar las imagines en AWS, mientras que las bases relacionales estarán en Azure.

---

## Regulaciones en Cloud

Las regulaciones son una parte importante a la hora de querer tener varios centro de datos en distintos lugar del mundo. El no cumplir con ellas podría traer como consecuenta una alta multa. Por ejemplo, en los países que pertenecen a la Unión Europea, sería el 20 millones de euros o $4\%$ de la facturación anual de toda la empresa (de acuerdo con el Reglamento General de Protección de Datos - RGPD).

Este reglamento es la base de muchos, por lo tanto, es útil saber sobre la protección de los datos personales que nos indica el **RGPD**, según esto, los datos personales son:

Los datos personales es cualquier información sobre una persona física identificada o identificable. Ejemplo de esto es: Nombre, País, Tipo de Sangre, Salario, Dirección IP, entre otros.
