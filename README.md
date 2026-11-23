# Práctica en Odoo

**Nombre:Abad Matute** 
**Número de cuenta:0801200314916** 

## Respuestas

### Paso 1. Catálogo de equipos

**1. ¿Qué pasa con un equipo cuando se desmarca el campo Activo? ¿Por qué es mejor que borrarlo?**

Cuando desmarco Activo el equipo queda archivado: ya no sale en la lista ni en las búsquedas, y tampoco aparece para elegirlo en otros formularios, pero sigue guardado en la base de datos. Lo puedo ver otra vez con el filtro "Archivados" y volver a activarlo. Es mejor que borrarlo porque no se pierde la información, por ejemplo los préstamos viejos que tenían ese equipo siguen mostrando cuál fue. Si lo borro esa referencia se pierde o Odoo no me deja borrarlo.

**2. ¿Para qué sirve copy=False en el campo codigo?**

Sirve para que cuando se duplica un equipo el código de inventario no se copie al nuevo registro. Como cada equipo tiene su propio código así se evita que queden dos equipos con el mismo código, y como el campo es obligatorio hay que ponerle uno nuevo.

