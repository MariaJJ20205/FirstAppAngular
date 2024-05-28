Código 
El código incluye dos partes principales: una función para cargar facturas desde una API y otra para suscribirse a esa función y manejar los datos recibidos.
a. Definición de la función `cargarFacturas`
Esta función utiliza el servicio HTTP para realizar una solicitud GET a la API y devolver un observable de un array de facturas (`factura[]`).
```
cargarFacturas(): Observable<factura[]> {
    return this.http.get<factura[]>(this.baseUrl);
}
```
b. Uso de la función `cargarFacturas`
Esta segunda función llama a `cargarFacturas`, se suscribe al observable resultante, y maneja los datos recibidos asignándolos a una fuente de datos (`dataSource`) y registrando la salida en la consola.
```
cargarFacturas(): void {
    this._facturaService.cargarFacturas().subscribe(
      data => {
        this.dataSource = data;
        console.log("Facturas de la base de datos", data)
      }
    );
}
```

Creación de la base de datos en SQL Server
La sección siguiente del código es un script SQL que crea una base de datos y varias tablas. Vamos a desglosar cada paso:


a. Crear la base de datos
```
CREATE DATABASE pazSalvoApi;
USE pazSalvoApi;
```
Estas líneas crean una base de datos llamada `pazSalvoApi` y la seleccionan para su uso.

b. Crear las tablas 
```
CREATE TABLE Clientes (
    Id INT IDENTITY(1,1) PRIMARY KEY,
    PersonaId INT,
    RolId INT,
    FechaDeCreacion DATETIME
);
CREATE TABLE Estados (
    Id INT IDENTITY(1,1) PRIMARY KEY,
    Nombre VARCHAR(100),
    Descripcion VARCHAR(255)
);
CREATE TABLE MediosDePago (
    Id INT IDENTITY(1,1) PRIMARY KEY,
    Nombre VARCHAR(100),
    Descripcion VARCHAR(255)
);
CREATE TABLE Facturas (
    Id INT IDENTITY(1,1) PRIMARY KEY,
    Saldo DECIMAL(18, 2),
    ClienteId INT,
    ServicioAdquiridoId INT,
    MedioDePagoId INT,
    Valor DECIMAL(18, 2),
    EstadoId INT,
    FechaDeCreacion DATETIME,
    FOREIGN KEY (ClienteId) REFERENCES Clientes(Id),
    FOREIGN KEY (EstadoId) REFERENCES Estados(Id),
    FOREIGN KEY (MedioDePagoId) REFERENCES MediosDePago(Id)
```
Inserción de registros en las tablas
a. Insertar un registro en las tablas
```
1.	INSERT INTO Clientes (PersonaId, RolId, FechaDeCreacion)
VALUES (1, 1, GETDATE());

2.	INSERT INTO Estados (Nombre, Descripcion)
VALUES ('Pagado', 'La factura ha sido pagada exitosamente.');

3.	INSERT INTO MediosDePago (Nombre, Descripcion)
VALUES ('Tarjeta de crédito', 'Pago realizado con tarjeta de crédito.');

4.	INSERT INTO Facturas (Saldo, ClienteId, ServicioAdquiridoId, MedioDePagoId, Valor, EstadoId, FechaDeCreacion)
```


