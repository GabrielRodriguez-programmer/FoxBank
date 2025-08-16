# 🏦 FoxBank – Proyecto Base de Datos  

FoxBank es un proyecto académico diseñado para **modelar el funcionamiento básico de un sistema bancario** utilizando **MySQL**.  
El sistema permite gestionar **usuarios, cuentas, tarjetas y transacciones** con reglas de negocio implementadas mediante **triggers automáticos**.  

Este proyecto muestra cómo se pueden aplicar conceptos de **bases de datos relacionales** y **automatización de procesos** en un entorno bancario simulado.  

---

## ✨ Características principales  

- 👤 **Gestión de usuarios**: registro con datos personales y credenciales.  
- 💳 **Tarjetas**: generación automática de tarjeta de débito para cada usuario.  
- 💰 **Cuentas bancarias**: apertura de cuenta corriente con saldo inicial predefinido.  
- 🔄 **Transacciones**: registro de movimientos financieros (ej. transferencias).  
- ⚡ **Automatización con triggers**:  
  - Creación automática de cuenta + tarjeta al insertar un usuario.  
  - Actualización de saldos al realizar transferencias.  

---

## 📂 Estructura del proyecto  

El archivo principal es `ProyectoBaseDeDatos.sql`, el cual contiene:  

### 🗄 Tablas  
- **Usuarios** → información básica (nombre, apellido, dirección, teléfono, correo, contraseña).  
- **Cuentas** → saldo, fecha de apertura y tipo de cuenta (corriente).  
- **Tarjetas** → datos de tarjeta asociada al usuario (número, tipo, CVV, fecha de vencimiento, contraseña de 4 dígitos).  
- **Transacciones** → operaciones financieras entre cuentas (monto, tipo, fecha, descripción).  

### ⚙️ Triggers  
- **`NuevoUsuarioTrigger`**  
  - Se ejecuta después de insertar un usuario.  
  - Crea automáticamente:  
    - Una cuenta corriente con **saldo inicial de 1000€**.  
    - Una tarjeta de débito con:  
      - Número de tarjeta generado aleatoriamente (16 dígitos).  
      - CVV aleatorio (3 dígitos).  
      - Contraseña de 4 dígitos aleatoria.  

- **`Transferencia`**  
  - Se ejecuta después de insertar una transacción con tipo **Transferencia**.  
  - Resta el monto a la cuenta de origen.  
  - Suma el monto a la cuenta de destino.  

### 📊 Datos iniciales de ejemplo  
El script inserta dos usuarios para pruebas:  
- **Juan Pérez** (con su cuenta y tarjeta generadas automáticamente).  
- **María Gómez** (con su cuenta y tarjeta generadas automáticamente).  

---

## ▶️ Ejecución  

1. Crear la base de datos ejecutando el script en MySQL:  
   ```sql
   SOURCE ProyectoBaseDeDatos.sql;
   ```  

2. Insertar un nuevo usuario en la tabla `Usuarios`:  
   ```sql
   INSERT INTO Usuarios (nombre, apellido, direccion, telefono, correo_electronico, contrasena)
   VALUES ('Carlos', 'Ruiz', 'Calle C #789', '555123456', 'carlos@example.com', 'clave1234');
   ```  

   🔹 Esto generará automáticamente:  
   - Una cuenta corriente con 1000€.  
   - Una tarjeta de débito con número, CVV y PIN únicos.  

3. Registrar una transferencia entre usuarios:  
   ```sql
   INSERT INTO Transacciones (numero_tarjeta_origen, numero_tarjeta_destino, tipo_transaccion, monto, fecha, descripcion)
   VALUES ('1111222233334444', '5555666677778888', 'Transferencia', 250.00, CURDATE(), 'Pago de servicios');
   ```  

   🔹 El trigger actualizará automáticamente los saldos de ambas cuentas.  

---

## 📖 Ejemplo de consultas  

```sql
-- Ver todos los usuarios con sus cuentas
SELECT u.nombre, u.apellido, c.tipo_cuenta, c.saldo
FROM Usuarios u
JOIN Cuentas c ON u.id_usuario = c.id_usuario;

-- Ver todas las transacciones
SELECT * FROM Transacciones;

-- Consultar tarjetas activas
SELECT nombre, apellido, numero_tarjeta, fecha_vencimiento
FROM Usuarios u
JOIN Tarjetas t ON u.id_usuario = t.id_usuario;
```  

---

## 📝 Notas  

- El sistema está pensado como **proyecto educativo** para practicar SQL.  
- Los números de tarjeta, CVV y PIN se generan **aleatoriamente**.  
- Puede ampliarse para incluir:  
  - Tipos adicionales de cuentas.  
  - Validaciones de saldo insuficiente.  
  - Reportes más avanzados de transacciones.  

---

## 👨‍💻 Autor  

Desarrollado por **Gabriel Rodriguez** ✨  
