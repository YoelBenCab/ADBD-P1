# Modelo Entidad–Relación de la Farmacia

Este documento describe el modelo E/R diseñado para la gestión de una farmacia, incluyendo entidades, atributos, relaciones, cardinalidades y restricciones semánticas.

---

## Entidades y atributos

### 1. **Farmacia**
Entidad central que gestiona el negocio.
- **Atributos**: No se especifican propios, sirve de nodo organizador en el diagrama.

### 2. **Medicamento**
Representa cada medicamento disponible o vendido por la farmacia.  
- **Atributos**:
  - `cod_med`: Identificador único del medicamento.  
  - `nombre_med`: Nombre del medicamento (ej. Paracetamol).  
  - `tipo_med`: Forma farmacéutica (jarabe, comprimido, pomada…).  
  - `u_stock`: Unidades en stock.  
  - `u_vendidas`: Unidades vendidas.  
  - `precio`: Precio de venta.  
  - `receta`: (sí/no) indica si requiere receta médica.  

### 3. **Laboratorio**
Entidad que fabrica o provee medicamentos.  
- **Atributos**:
  - `cod_lab`: Identificador único del laboratorio.  
  - `nombre`: Nombre del laboratorio.  
  - `teléfono`: Número de contacto.  
  - `dir_postal`: Dirección postal.  
  - `fax`: Número de fax.  
  - `nom_contacto`: Persona de contacto.  

### 4. **Familia**
Agrupa medicamentos por tipo de enfermedad a la que se aplican.  
- **Atributos**:
  - `Enf_trata`: Nombre de la enfermedad o grupo terapéutico (ej. Analgésicos, Antibióticos).

### 5. **Cliente**
Persona que compra medicamentos en la farmacia.  
- **Atributos**:
  - `cod_cliente`: Identificador único del cliente.  
  - `crédito`: (sí/no) indica si dispone de crédito mensual.  

#### Subtipos:
- **Cliente con crédito**: hereda de Cliente y añade:
  - Relación obligatoria con **Datos bancarios**.  
- **Cliente sin crédito**: hereda de Cliente, no tiene datos bancarios.

### 6. **Datos bancarios**
Entidad asociada únicamente a clientes con crédito.  
- Representa la información bancaria necesaria para gestionar el pago mensual.

### 7. **Compra**
Entidad débil/intermedia que registra transacciones.  
- **Atributos**:
  - `u_vendidas`: Cantidad de medicamentos comprados.  
  - `fecha_compra`: Fecha en la que se realiza la compra.  
  - `fecha_pago`: Fecha de pago (relevante solo para clientes con crédito).

---

## Relaciones

1. **Farmacia – Medicamento**  
   - Relación: *Tiene*  
   - Cardinalidad: La farmacia gestiona **(1,N)** medicamentos.  

2. **Medicamento – Laboratorio**  
   - Relación: *Fabricado / Provee*  
   - Cardinalidad:  
     - Un medicamento es fabricado por **exactamente un laboratorio (1,1)**.  
     - Un laboratorio puede fabricar **muchos medicamentos (1,N)**.  

3. **Medicamento – Familia**  
   - Relación: *Pertenece*  
   - Cardinalidad:  
     - Un medicamento pertenece a **una familia (1,1)**.  
     - Una familia agrupa **muchos medicamentos (0,N)**.  

4. **Cliente – Compra – Medicamento**  
   - Relación: *Compra*  
   - Cardinalidad:  
     - Un cliente puede realizar **muchas compras (0,N)**.  
     - Una compra está asociada a **al menos un medicamento (1,N)**.  
     - Un medicamento puede aparecer en **muchas compras (0,N)**.  

5. **Cliente con crédito – Datos bancarios**  
   - Relación: *Tiene*  
   - Cardinalidad: **1:1 obligatoria**.  
   - Cada cliente con crédito debe tener datos bancarios; un registro de datos bancarios pertenece a un único cliente con crédito.  

---

## Restricciones semánticas

- **Restricción 1:** Solo los **clientes con crédito** tienen asociados **datos bancarios**.  
- **Restricción 2:** La **fecha de pago** en una compra solo se registra si el cliente es con crédito.  

---
