# Creación de Base de Datos y Tablas - Clínica

```sql
-- Crear base de datos
CREATE DATABASE Clinica;

-- Conectarse a la base de datos
\c clinica

CREATE TABLE Pacientes(
    id_pacientes SERIAL PRIMARY KEY,
    Nombre VARCHAR(100) NOT NULL,
    Apellido VARCHAR(100) NOT NULL,
    DNI BIGINT NOT NULL,
    Fecha_Nacimiento DATE NOT NULL,
    Genero VARCHAR(1) NOT NULL,
    Telefono VARCHAR(20) NOT NULL,
    Email VARCHAR(50) NOT NULL,
    Direccion VARCHAR(100) NOT NULL,
    Tipo_Sangre VARCHAR(5) NOT NULL,
    Fecha_Registro TIMESTAMP NOT NULL,
    Estado INT NOT NULL CHECK (Estado IN (0, 1))
);

CREATE TABLE Medicos(
    id_medicos SERIAL PRIMARY KEY,
    Nombre VARCHAR(100) NOT NULL,
    Apellido VARCHAR(100) NOT NULL,
    DNI BIGINT NOT NULL,
    Telefono VARCHAR(20) NOT NULL,
    Email VARCHAR(50) NOT NULL,
    Direccion VARCHAR(100) NOT NULL,
    Fecha_Contratacion TIMESTAMP NOT NULL,
    id_especialidad INT NOT NULL,
    Estado INT NOT NULL CHECK (Estado IN (0, 1))
);

CREATE TABLE Especialidades(
    id_especialidad SERIAL PRIMARY KEY,
    Nombre VARCHAR(100) NOT NULL,
    Descripcion VARCHAR(200) NOT NULL
);

CREATE TABLE Administrativos(
    id_administrativo SERIAL PRIMARY KEY,
    Nombre VARCHAR(100) NOT NULL,
    Apellido VARCHAR(100) NOT NULL,
    DNI BIGINT NOT NULL,
    Telefono VARCHAR(20) NOT NULL,
    Email VARCHAR(50) NOT NULL,
    Cargo VARCHAR(100) NOT NULL,
    Estado INT NOT NULL CHECK (Estado IN (0, 1))
);

CREATE TABLE CitasMedicas(
    id_citas SERIAL PRIMARY KEY,
    id_pacientes INT NOT NULL,
    id_medicos INT NOT NULL,
    id_consultorios INT NOT NULL,
    Fecha_Cita DATE NOT NULL,
    Hora_Cita TIME NOT NULL,
    Motivo VARCHAR(200) NOT NULL,
    Estado INT NOT NULL CHECK (Estado IN (0, 1))
);

CREATE TABLE ReservarCitas(
    id_reserva SERIAL PRIMARY KEY,
    id_pacientes INT NOT NULL,
    id_citas INT NOT NULL,
    Fecha_Reserva TIMESTAMP NOT NULL,
    Estado INT NOT NULL CHECK (Estado IN (0, 1))
);

CREATE TABLE Urgencias(
    id_urgencia SERIAL PRIMARY KEY,
    id_pacientes INT NOT NULL,
    Fecha_Ingreso TIMESTAMP NOT NULL,
    Sintomas VARCHAR(200) NOT NULL,
    id_medicos INT NOT NULL,
    Tratamiento_Inicial VARCHAR(200) NOT NULL,
    Estado INT NOT NULL CHECK (Estado IN (0, 1))
);

CREATE TABLE HistorialClinico(
    id_historial SERIAL PRIMARY KEY,
    id_pacientes INT NOT NULL,
    Fecha_Creacion TIMESTAMP NOT NULL,
    Descripcion VARCHAR(200) NOT NULL,
    Alergias VARCHAR(200) NOT NULL,
    Antecedentes_Medicos VARCHAR(200) NOT NULL,
    Estado INT NOT NULL CHECK (Estado IN (0, 1))
);

CREATE TABLE Diagnosticos(
    id_diagnostico SERIAL PRIMARY KEY,
    id_historial INT NOT NULL,
    id_medicos INT NOT NULL,
    Fecha_Diagnostico TIMESTAMP NOT NULL,
    Descripcion VARCHAR(200) NOT NULL,
    id_pacientes INT NOT NULL,
    Estado INT NOT NULL CHECK (Estado IN (0, 1))
);

CREATE TABLE Tratamientos(
    id_tratamiento SERIAL PRIMARY KEY,
    id_diagnostico INT NOT NULL,
    Descripcion VARCHAR(200) NOT NULL,
    Duracion INT NOT NULL,
    Observaciones VARCHAR(200) NOT NULL,
    Estado INT NOT NULL CHECK (Estado IN (0, 1))
);

CREATE TABLE RecetasMedica(
    id_receta SERIAL PRIMARY KEY,
    id_tratamiento INT NOT NULL,
    Fecha_Emision TIMESTAMP NOT NULL,
    Medicamento VARCHAR(200) NOT NULL,
    Dosis VARCHAR(100) NOT NULL,
    Frecuencia VARCHAR(100) NOT NULL,
    Duracion VARCHAR(100) NOT NULL,
    Estado INT NOT NULL CHECK (Estado IN (0, 1))
);

CREATE TABLE InventarioMedicamentos(
    id_medicamento SERIAL PRIMARY KEY,
    Nombre VARCHAR(100) NOT NULL,
    Descripcion VARCHAR(200) NOT NULL,
    Cantidad INT NOT NULL,
    Fecha_Vencimiento TIMESTAMP NOT NULL,
    Proveedor VARCHAR(100) NOT NULL,
    Estado INT NOT NULL CHECK (Estado IN (0, 1))
);

CREATE TABLE Consultorios(
    id_consultorios SERIAL PRIMARY KEY,
    Nombre VARCHAR(100) NOT NULL,
    Piso VARCHAR(1) NOT NULL,
    Estado INT NOT NULL CHECK (Estado IN (0, 1))
);

CREATE TABLE Salas(
    id_sala SERIAL PRIMARY KEY,
    Tipo VARCHAR(100) NOT NULL,
    Capacidad INT NOT NULL,
    Estado INT NOT NULL CHECK (Estado IN (0, 1))
);

CREATE TABLE Facturacion(
    id_factura SERIAL PRIMARY KEY,
    id_pacientes INT NOT NULL,
    id_citas INT NOT NULL,
    Fecha_Emision TIMESTAMP NOT NULL,
    Monto_Total FLOAT NOT NULL,
    Metodo_Pago VARCHAR(200) NOT NULL,
    Estado INT NOT NULL CHECK (Estado IN (0, 1))
);
```

## Llaves foráneas
```sql
ALTER TABLE Medicos ADD CONSTRAINT fk_Medicos_Especialidad FOREIGN KEY (id_especialidad) REFERENCES Especialidades(id_especialidad) ON UPDATE CASCADE ON DELETE RESTRICT;

ALTER TABLE CitasMedicas ADD CONSTRAINT fk_Citas_Pacientes FOREIGN KEY (id_pacientes) REFERENCES Pacientes(id_pacientes) ON UPDATE CASCADE ON DELETE RESTRICT;

ALTER TABLE CitasMedicas ADD CONSTRAINT fk_Citas_Medicos FOREIGN KEY (id_medicos) REFERENCES Medicos(id_medicos) ON UPDATE CASCADE ON DELETE RESTRICT;

ALTER TABLE CitasMedicas ADD CONSTRAINT fk_Citas_Consultorios FOREIGN KEY (id_consultorios) REFERENCES Consultorios(id_consultorios) ON UPDATE CASCADE ON DELETE RESTRICT;

ALTER TABLE ReservarCitas ADD CONSTRAINT fk_Reserva_Pacientes FOREIGN KEY (id_pacientes) REFERENCES Pacientes(id_pacientes) ON UPDATE CASCADE ON DELETE RESTRICT;

ALTER TABLE ReservarCitas ADD CONSTRAINT fk_Reserva_Citas FOREIGN KEY (id_citas) REFERENCES CitasMedicas(id_citas) ON UPDATE CASCADE ON DELETE RESTRICT;

ALTER TABLE Urgencias ADD CONSTRAINT fk_Urgencias_Pacientes FOREIGN KEY (id_pacientes) REFERENCES Pacientes(id_pacientes) ON UPDATE CASCADE ON DELETE RESTRICT;

ALTER TABLE Urgencias ADD CONSTRAINT fk_Urgencias_Medicos FOREIGN KEY (id_medicos) REFERENCES Medicos(id_medicos) ON UPDATE CASCADE ON DELETE RESTRICT;

ALTER TABLE HistorialClinico ADD CONSTRAINT fk_Historial_Pacientes FOREIGN KEY (id_pacientes) REFERENCES Pacientes(id_pacientes) ON UPDATE CASCADE ON DELETE RESTRICT;

ALTER TABLE Diagnosticos ADD CONSTRAINT fk_Diagnostico_Pacientes FOREIGN KEY (id_pacientes) REFERENCES Pacientes(id_pacientes) ON UPDATE CASCADE ON DELETE RESTRICT;

ALTER TABLE Diagnosticos ADD CONSTRAINT fk_Diagnostico_Medicos FOREIGN KEY (id_medicos) REFERENCES Medicos(id_medicos) ON UPDATE CASCADE ON DELETE RESTRICT;

ALTER TABLE Diagnosticos ADD CONSTRAINT fk_Diagnostico_Historial FOREIGN KEY (id_historial) REFERENCES HistorialClinico(id_historial) ON UPDATE CASCADE ON DELETE RESTRICT;

ALTER TABLE Tratamientos ADD CONSTRAINT fk_Tratamiento_Diagnosticos FOREIGN KEY (id_diagnostico) REFERENCES Diagnosticos(id_diagnostico) ON UPDATE CASCADE ON DELETE RESTRICT;

ALTER TABLE RecetasMedica ADD CONSTRAINT fk_Recetas_Tratamiento FOREIGN KEY (id_tratamiento) REFERENCES Tratamientos(id_tratamiento) ON UPDATE CASCADE ON DELETE RESTRICT;

ALTER TABLE Facturacion ADD CONSTRAINT fk_Facturacion_Pacientes FOREIGN KEY (id_pacientes) REFERENCES Pacientes(id_pacientes) ON UPDATE CASCADE ON DELETE RESTRICT;

ALTER TABLE Facturacion ADD CONSTRAINT fk_Facturacion_Citas FOREIGN KEY (id_citas) REFERENCES CitasMedicas(id_citas) ON UPDATE CASCADE ON DELETE RESTRICT;

```

### Inserciones
```sql
INSERT INTO pacientes (Nombre, Apellido, DNI, Fecha_Nacimiento, Genero, Telefono, Email, Direccion, Tipo_Sangre, Fecha_Registro, Estado) 
VALUES 
('Jose Miguel', 'Vera Garzón', 1098631155, '2007-01-20', 'M', '3145613594', 'josemig@mail.com', 'Cr 2 #70-93', 'B+', '2025-04-25 18:09:26', 1),
('María Juliana', 'Ferro Bonilla', 1224562264, '2008-04-12', 'F', '3156480065', 'mariajul@mail.com', 'Av 1 Cll 70', 'B+', '2025-04-25 18:09:26', 1),
('Yenny Patrica', 'Garzón Solarte', 3630641123, '1981-11-06', 'F', '3168786167', 'yennypat@mail.com', 'Cr 2 #70-93', 'B+', '2025-04-25 18:09:26', 1),
('Gustavo Adolfo', 'Vera', 4167815167, '1994-12-31', 'M', '3124567789', 'gustavover@gmail.com', 'Cll 1 #10-15', 'O-', '2025-04-25 18:09:26', 1),
('Angel David', 'Vera Garzón', 1503215657, '2005-05-18', 'M', '3161678594', 'angeldav@mail.com', 'Cll 5 Av2', 'O+', '2025-04-25 18:09:26', 1);


INSERT INTO Especialidades (Nombre, Descripcion) 
VALUES 
('Cardiología', 'Diagnóstico y tratamiento de enfermedades del corazón y vasos sanguíneos.'),
('Pediatría', 'Atención médica integral a bebés, niños y adolescentes.'),
('Dermatología', 'Estudio y tratamiento de enfermedades de la piel, cabello y uñas.'),
('Neurología', 'Diagnóstico y manejo de trastornos del sistema nervioso.'),
('Ortopedia', 'Prevención y corrección de deformidades o lesiones del sistema músculo-esquelético.');


INSERT INTO Medicos (Nombre, Apellido, DNI, Telefono, Email, Direccion, Fecha_Contratacion, id_especialidad, Estado) 
VALUES 
('Julian Andres', 'Fernando Ortiz', 1487963245, '3326597154', 'julianOrt@mail.com', 'Av 1 #70', '2025-04-25 18:09:26', 1, 1), 
('Andrea Matilde', 'Sanchez', 4155997615, '3154871365', 'andresanc@mail.com', 'Cll 1 Av 2', '2025-04-25 18:09:26', 2, 1), 
('Camilo Andres', 'Escobar Zanto', 1633487961, '3156471264', 'camiloesc@mail.com', 'Av 3 Cr 4-12', '2025-04-25 18:09:26', 3, 1), 
('Luz Diaz', 'Andrade Mejia', 3165457862, '3201177465', 'luzand@mail.com', 'Av 5 Cll 10', '2025-04-25 18:09:26', 4, 1), 
('Ludwig Arnovizcha', 'Perez', 1567894135, '3125468915', 'ludwigper5@mail.com', 'Cr 2 #70-92', '2025-04-25 18:09:26', 5, 1);


INSERT INTO Administrativos (Nombre, Apellido, DNI, Telefono, Email, Cargo, Estado) 
VALUES 
('Laura', 'Martínez Gómez', 1523457961, '3196548716', 'laumarti@mail.com', 'Recepcionista', 1), 
('Carlos', 'Pérez Rivas', 1112486512, '3115498794',  'carlperez@mail.com', 'Asistente Administrativo', 1), 
('Mónica', 'López Herrera', 1687493516, '3231579435', 'monicherra@mail.com', 'Coordinador de Citas', 1), 
('Javier', 'Ramírez Díaz', 36789465, '3168975462', 'javidiaz@mail.com', 'Jefe de Admisiones', 1), 
('Sofía', 'Castro Molina', 13658498, '3315648795', 'castrosofi@mail.com', 'Secretaria General', 1);

INSERT INTO Consultorios (Nombre, Piso, Estado) 
VALUES 
('Consultorio 124', '1', 1), 
('Consultorio 123', '2', 1), 
('Consultorio 122', '3', 1), 
('Consultorio 121', '4', TRUE), 
('Consultorio 120', '5', 1);


INSERT INTO Salas (Tipo, Capacidad, Estado) 
VALUES 
('Sala de Urgencias', 10, 1), 
('Sala de Cirugía', 8, 1), 
('Sala de Recuperación', 12, 1), 
('Sala de Pediatría', 6, 1), 
('Sala de Terapia Intensiva', 5, 1);


INSERT INTO CitasMedicas (id_pacientes, id_medicos, id_consultorios, Fecha_Cita, Hora_Cita, Motivo, Estado) 
VALUES 
(1, 2, 3, '2025-05-01', '10:00', 'Consulta general', 1), 
(2, 3, 1, '2025-05-02', '11:30', 'Dolor de cabeza', 1), 
(3, 1, 2, '2025-05-03', '09:00', 'Chequeo anual', 1), 
(4, 4, 5, '2025-05-04', '14:00', 'Problemas respiratorios', 1), 
(5, 5, 4, '2025-05-05', '16:30', 'Revisión de tratamiento', 1);


INSERT INTO ReservarCitas (id_pacientes, id_citas, Fecha_Reserva, Estado) 
VALUES 
(1, 1, '2025-04-25 08:00', 1), 
(2, 2, '2025-04-26 09:30', 1), 
(3, 3, '2025-04-27 10:00', 1), 
(4, 4, '2025-04-28 11:15', 1), 
(5, 5, '2025-04-29 13:45', 1);


INSERT INTO Urgencias (id_pacientes, Fecha_Ingreso, Sintomas, id_medicos, Tratamiento_Inicial, Estado) 
VALUES 
(1, '2025-04-25 20:00', 'Fiebre alta', 2, 'Administración de antipiréticos', 1), 
(2, '2025-04-26 22:30', 'Accidente de tráfico', 3, 'Evaluación primaria', 1), 
(3, '2025-04-27 01:15', 'Dolor abdominal intenso', 1, 'Análisis de sangre', 1), 
(4, '2025-04-28 03:45', 'Fractura de brazo', 4, 'Inmovilización del miembro', 1), 
(5, '2025-04-29 05:00', 'Dificultad respiratoria', 5, 'Oxigenoterapia', 1);


INSERT INTO HistorialClinico (id_pacientes, Fecha_Creacion, Descripcion, Alergias, Antecedentes_Medicos, Estado) 
VALUES 
(1, '2025-04-25 12:00', 'Historial completo', 'Penicilina', 'Hipertensión', 1), 
(2, '2025-04-26 13:30', 'Seguimiento anual', 'Ninguna', 'Diabetes tipo II', 1), 
(3, '2025-04-27 15:00', 'Revisión médica', 'Polen', 'Asma', 1), 
(4, '2025-04-28 16:45', 'Chequeo general', 'Frutos secos', 'Migraña', 1), 
(5, '2025-04-29 18:20', 'Nuevo paciente', 'Lactosa', 'Cirugía previa de apéndice', 1);


INSERT INTO Diagnosticos (id_historial, id_medicos, Fecha_Diagnostico, Descripcion, id_pacientes, Estado) 
VALUES 
(1, 2, '2025-04-25 14:00', 'Diagnóstico de hipertensión', 1, 1), 
(2, 3, '2025-04-26 15:30', 'Diabetes controlada', 2, 1), 
(3, 1, '2025-04-27 16:00', 'Crisis asmática leve', 3, 1), 
(4, 4, '2025-04-28 17:45', 'Migraña episódica', 4, 1), 
(5, 5, '2025-04-29 19:20', 'Chequeo post-operatorio', 5, 1);


INSERT INTO Tratamientos (id_diagnostico, Descripcion, Duracion, Observaciones, Estado) 
VALUES 
(1, 'Medicamento antihipertensivo', 30, 'Monitoreo semanal', 1), 
(2, 'Plan de alimentación especial', 90, 'Revisión mensual', 1), 
(3, 'Inhaladores de rescate', 60, 'Uso en crisis', 1), 
(4, 'Medicamentos para migraña', 45, 'Registrar episodios', 1), 
(5, 'Fisioterapia post-cirugía', 120, 'Seguimiento bimensual', 1);


INSERT INTO RecetasMedica (id_tratamiento, Fecha_Emision, Medicamento, Dosis, Frecuencia, Duracion, Estado) 
VALUES 
(1, '2025-04-25 15:00', 'Losartán', '50 mg', 'Una vez al día', '30 días', 1), 
(2, '2025-04-26 16:30', 'Metformina', '850 mg', 'Dos veces al día', '90 días', 1), 
(3, '2025-04-27 17:00', 'Salbutamol', '2 inhalaciones', 'Según necesidad', '60 días', 1), 
(4, '2025-04-28 18:45', 'Sumatriptán', '50 mg', 'Cuando haya crisis', '45 días', 1), 
(5, '2025-04-29 20:20', 'Ibuprofeno', '400 mg', 'Cada 8 horas', '7 días', 1);


INSERT INTO InventarioMedicamentos (Nombre, Descripcion, Cantidad, Fecha_Vencimiento, Proveedor, Estado) 
VALUES 
('Losartán', 'Antihipertensivo', 500, '2026-12-31 00:00', 'Farmacia Salud', 1), 
('Metformina', 'Antidiabético oral', 300, '2026-06-30 00:00', 'Distribuidora Farma', 1),
('Salbutamol', 'Broncodilatador', 150, '2025-10-15 00:00', 'Laboratorio Respira', 1),
('Sumatriptán', 'Antimigrañoso', 200, '2025-11-01 00:00', 'Farmacia Migraña', 1),
('Ibuprofeno', 'Anti-inflamatorio', 1000, '2027-03-01 00:00', 'Medifarma', 1);
```

# Indices
```sql
CREATE INDEX idx_pacientes_dni ON Pacientes(DNI);
CREATE INDEX idx_pacientes_nombre_apellido ON Pacientes(Nombre, Apellido);
CREATE INDEX idx_fecha_registro ON Pacientes(Fecha_Registro);

-- Índices para Médicos
CREATE INDEX idx_medicos_dni ON Medicos(DNI);
CREATE INDEX idx_medicos_nombre_apellido ON Medicos(Nombre, Apellido);
CREATE INDEX idx_medicos_especialidad ON Medicos(id_especialidad);

--índice para especialidades
CREATE INDEX idx_medicos_nombre ON Especialidades(Nombre);

--índices para administrativos
CREATE INDEX idx_administrativos_dni ON Administrativos(DNI);
CREATE INDEX idx_administrativos_cargo ON Administrativos(Cargo);

-- Índices para Citas Médicas
CREATE INDEX idx_citasmedicas_paciente ON CitasMedicas(id_pacientes);
CREATE INDEX idx_citasmedicas_medico ON CitasMedicas(id_medicos);
CREATE INDEX idx_citasmedicas_medico_fecha ON CitasMedicas(id_medicos, Fecha_Cita);

-- Índices para ReservarCitas
CREATE INDEX idx_reservarcitas_paciente ON ReservarCitas(id_pacientes);
CREATE INDEX idx_reservarcitas_cita ON ReservarCitas(id_citas);

-- Índices para Urgencias
CREATE INDEX idx_urgencias_paciente ON Urgencias(id_pacientes);
CREATE INDEX idx_urgencias_medico ON Urgencias(id_medicos);

-- Índices para Historial Clínico
CREATE INDEX idx_historialclinico_paciente ON HistorialClinico(id_pacientes);

-- Índices para Diagnósticos
CREATE INDEX idx_diagnosticos_historial ON Diagnosticos(id_historial);
CREATE INDEX idx_diagnosticos_medico ON Diagnosticos(id_medicos);
CREATE INDEX idx_diagnosticos_paciente ON Diagnosticos(id_pacientes);

-- Índices para Tratamientos
CREATE INDEX idx_tratamientos_diagnostico ON Tratamientos(id_diagnostico);

-- Índices para Recetas Médicas
CREATE INDEX idx_recetasmedica_tratamiento ON RecetasMedica(id_tratamiento);

-- Índices para Inventario Medicamentos
CREATE INDEX idx_inventario_medicamento_nombre ON InventarioMedicamentos(Nombre);

-- Índices para Consultorios
CREATE INDEX idx_consultorios_nombre ON Consultorios(Nombre);

-- Índice en la columna 'Tipo' para búsquedas rápidas por tipo de sala
CREATE INDEX idx_salas_tipo ON Salas(Tipo);

-- Índices para Facturación
CREATE INDEX idx_facturacion_paciente ON Facturacion(id_pacientes);
CREATE INDEX idx_facturacion_cita ON Facturacion(id_citas);
```

# VISTAS
```sql

1. Vista de citas del día por médico:
Esta vista te permitirá obtener todas las citas médicas programadas para el día actual, organizadas por médico.

CREATE VIEW vista_citas_dia_por_medico AS
SELECT
    m.Nombre AS Nombre_Medico,
    m.Apellido AS Apellido_Medico,
    c.Fecha_Cita,
    c.Hora_Cita,
    p.Nombre AS Nombre_Paciente,
    p.Apellido AS Apellido_Paciente
FROM
    CitasMedicas c
JOIN
    Medicos m ON c.id_medicos = m.id_medicos
JOIN
    Pacientes p ON c.id_pacientes = p.id_pacientes
WHERE
    c.Fecha_Cita = '2025-05-03';



2. Vista de pacientes con diagnóstico específico:
Esta vista muestra todos los pacientes que tienen un diagnóstico específico. Aquí vamos a suponer que buscas pacientes diagnosticados con una enfermedad o condición específica.

CREATE VIEW vista_pacientes_con_diagnostico AS
SELECT 
    p.Nombre AS Nombre_Paciente, 
    p.Apellido AS Apellido_Paciente, 
    d.Descripcion AS Diagnostico, 
    d.Fecha_Diagnostico
FROM 
    Diagnosticos d
JOIN 
    Pacientes p ON d.id_pacientes = p.id_pacientes
WHERE 
    d.Descripcion LIKE '%Diabetes controlada%'; 



3. Vista del historial de recetas por paciente:
Esta vista proporciona una lista de todas las recetas médicas emitidas a un paciente en particular, mostrando el medicamento recetado, la dosis y la duración.

CREATE VIEW vista_historial_recetas_por_paciente AS
SELECT 
    p.Nombre AS Nombre_Paciente, 
    p.Apellido AS Apellido_Paciente, 
    r.Medicamento, 
    r.Dosis, 
    r.Frecuencia, 
    r.Duracion, 
    r.Fecha_Emision
FROM 
    RecetasMedica r
JOIN 
    Tratamientos t ON r.id_tratamiento = t.id_tratamiento
JOIN 
    Diagnosticos d ON t.id_diagnostico = d.id_diagnostico
JOIN 
    Pacientes p ON d.id_pacientes = p.id_pacientes;

4. Vista de pacientes en urgencias:
Esta vista muestra todos los pacientes que se encuentran actualmente en urgencias, junto con el médico asignado y los síntomas que presentan.

CREATE VIEW vista_pacientes_en_urgencias AS
SELECT 
    p.Nombre AS Nombre_Paciente, 
    p.Apellido AS Apellido_Paciente, 
    u.Sintomas, 
    u.Fecha_Ingreso, 
    m.Nombre AS Nombre_Medico, 
    m.Apellido AS Apellido_Medico
FROM 
    Urgencias u
JOIN 
    Pacientes p ON u.id_pacientes = p.id_pacientes
JOIN 
    Medicos m ON u.id_medicos = m.id_medicos
WHERE 
    u.Estado = TRUE; 

5. Vista de pacientes con citas y facturación pendiente:
Esta vista muestra a los pacientes que tienen citas programadas con facturas que ya han sido pagadas.

CREATE VIEW vista_pacientes_citas_y_facturas_pagadas AS
SELECT 
    p.Nombre AS Nombre_Paciente, 
    p.Apellido AS Apellido_Paciente, 
    c.Fecha_Cita, 
    c.Hora_Cita, 
    f.Monto_Total, 
    f.Fecha_Emision AS Fecha_Factura
FROM 
    CitasMedicas c
JOIN 
    Pacientes p ON c.id_pacientes = p.id_pacientes
JOIN 
    Facturacion f ON c.id_citas = f.id_citas
WHERE 
    f.Estado = TRUE; 

```

# Procedimientos almacenados

## tabla **Medicos**



```sql
CREATE OR REPLACE PROCEDURE crear_medico(
    IN p_nombre VARCHAR,
    IN p_apellido VARCHAR,
    IN p_dni BIGINT,
    IN p_telefono VARCHAR,
    IN p_email VARCHAR,
    IN p_direccion VARCHAR,
    IN p_fecha_contratacion TIMESTAMP,
    IN p_id_especialidad INT
)
LANGUAGE plpgsql
AS $$
BEGIN
    INSERT INTO Medicos(Nombre, Apellido, DNI, Telefono, Email, Direccion, Fecha_Contratacion, id_especialidad, Estado)
    VALUES (p_nombre, p_apellido, p_dni, p_telefono, p_email, p_direccion, p_fecha_contratacion, p_id_especialidad, TRUE);
END;
$$;


CREATE OR REPLACE PROCEDURE actualizar_medico(
    IN p_id INT,
    IN p_nombre VARCHAR,
    IN p_apellido VARCHAR,
    IN p_dni BIGINT,
    IN p_telefono VARCHAR,
    IN p_email VARCHAR,
    IN p_direccion VARCHAR,
    IN p_id_especialidad INT
)
LANGUAGE plpgsql
AS $$
BEGIN
    UPDATE Medicos
    SET Nombre = p_nombre,
        Apellido = p_apellido,
        DNI = p_dni,
        Telefono = p_telefono,
        Email = p_email,
        Direccion = p_direccion,
        id_especialidad = p_id_especialidad
    WHERE id_medicos = p_id;
END;
$$;



CREATE OR REPLACE PROCEDURE eliminar_medico(IN p_id INT)
LANGUAGE plpgsql
AS $$
BEGIN
    UPDATE Medicos SET Estado = FALSE WHERE id_medicos = p_id;
END;
$$;
```
```sql
## tabla **Medicos**

CREATE OR REPLACE PROCEDURE crear_paciente(
    IN p_nombre VARCHAR,
    IN p_apellido VARCHAR,
    IN p_dni BIGINT,
    IN p_fecha_nacimiento DATE,
    IN p_genero VARCHAR,
    IN p_telefono VARCHAR,
    IN p_email VARCHAR,
    IN p_direccion VARCHAR,
    IN p_tipo_sangre VARCHAR
)
LANGUAGE plpgsql
AS $$
BEGIN
    INSERT INTO Pacientes(Nombre, Apellido, DNI, Fecha_Nacimiento, Genero, Telefono, Email, Direccion, Tipo_Sangre, Fecha_Registro, Estado)
    VALUES (p_nombre, p_apellido, p_dni, p_fecha_nacimiento, p_genero, p_telefono, p_email, p_direccion, p_tipo_sangre, NOW(), TRUE);
END;
$$;



CREATE OR REPLACE PROCEDURE actualizar_paciente(
    IN p_id INT,
    IN p_nombre VARCHAR,
    IN p_apellido VARCHAR,
    IN p_dni BIGINT
)
LANGUAGE plpgsql
AS $$
BEGIN
    UPDATE Pacientes
    SET Nombre = p_nombre,
        Apellido = p_apellido,
        DNI = p_dni
    WHERE id_pacientes = p_id;
END;
$$;



CREATE OR REPLACE PROCEDURE eliminar_paciente(IN p_id INT)
LANGUAGE plpgsql
AS $$
BEGIN
    UPDATE Pacientes SET Estado = FALSE WHERE id_pacientes = p_id;
END;
$$;
```
## tabla **CitasMedicas**
```sql
CREATE OR REPLACE PROCEDURE crear_cita_medica(
    IN p_id_pacientes INT,
    IN p_id_medicos INT,
    IN p_id_consultorios INT,
    IN p_fecha DATE,
    IN p_hora TIME,
    IN p_motivo VARCHAR
)
LANGUAGE plpgsql
AS $$
BEGIN
    INSERT INTO CitasMedicas(id_pacientes, id_medicos, id_consultorios, Fecha_Cita, Hora_Cita, Motivo, Estado)
    VALUES (p_id_pacientes, p_id_medicos, p_id_consultorios, p_fecha, p_hora, p_motivo, TRUE);
END;
$$;



CREATE OR REPLACE PROCEDURE actualizar_cita_medica(
    IN p_id_cita INT,
    IN p_fecha DATE,
    IN p_hora TIME,
    IN p_motivo VARCHAR
)
LANGUAGE plpgsql
AS $$
BEGIN
    UPDATE CitasMedicas
    SET Fecha_Cita = p_fecha,
        Hora_Cita = p_hora,
        Motivo = p_motivo
    WHERE id_citas = p_id_cita;
END;
$$;



CREATE OR REPLACE PROCEDURE eliminar_cita_medica(IN p_id_cita INT)
LANGUAGE plpgsql
AS $$
BEGIN
    UPDATE CitasMedicas SET Estado = FALSE WHERE id_citas = p_id_cita;
END;
$$;

```

## tabla **HistorialClinico**
```sql
CREATE OR REPLACE PROCEDURE crear_historial_clinico(
    IN p_id_pacientes INT,
    IN p_descripcion VARCHAR,
    IN p_alergias VARCHAR,
    IN p_antecedentes VARCHAR
)
LANGUAGE plpgsql
AS $$
BEGIN
    INSERT INTO HistorialClinico(id_pacientes, Fecha_Creacion, Descripcion, Alergias, Antecedentes_Medicos, Estado)
    VALUES (p_id_pacientes, NOW(), p_descripcion, p_alergias, p_antecedentes, TRUE);
END;
$$;



CREATE OR REPLACE PROCEDURE actualizar_historial_clinico(
    IN p_id_historial INT,
    IN p_descripcion VARCHAR,
    IN p_alergias VARCHAR,
    IN p_antecedentes VARCHAR
)
LANGUAGE plpgsql
AS $$
BEGIN
    UPDATE HistorialClinico
    SET Descripcion = p_descripcion,
        Alergias = p_alergias,
        Antecedentes_Medicos = p_antecedentes
    WHERE id_historial = p_id_historial;
END;
$$;



CREATE OR REPLACE PROCEDURE eliminar_historial_clinico(IN p_id_historial INT)
LANGUAGE plpgsql
AS $$
BEGIN
    UPDATE HistorialClinico SET Estado = FALSE WHERE id_historial = p_id_historial;
END;
$$;


```
# Triggers
Se crearán triggers para las tablas **Pacientes**, **CitasMedicas** y **Facturación**, en los siguientes eventos:

- **Pacientes**: `INSERT`, `UPDATE`, `DELETE`
- **CitasMedicas**: `INSERT`
- **Facturación**: `INSERT`, `UPDATE`, `DELETE`

---

### Creación de la tabla de auditoría: pacientes

```sql
CREATE TABLE audit_pacientes(
    id_audit_pacientes SERIAL PRIMARY KEY,
    operacion VARCHAR(10),
    id_pacientes INT,
    nombre TEXT,
    apellido TEXT,
    fecha_operacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    usuario TEXT DEFAULT current_user
);


CREATE OR REPLACE FUNCTION fnc_pacientes()
RETURNS TRIGGER AS $$
BEGIN
    IF (TG_OP = 'INSERT') THEN
        INSERT INTO audit_pacientes (operacion, id_pacientes, nombre, apellido)
        VALUES ('INSERT', NEW.id_pacientes, NEW.nombre, NEW.apellido);
    ELSIF (TG_OP = 'UPDATE') THEN
        INSERT INTO audit_pacientes (operacion, id_pacientes, nombre, apellido)
        VALUES ('UPDATE', NEW.id_pacientes, NEW.nombre, NEW.apellido);
    ELSIF (TG_OP = 'DELETE') THEN
        INSERT INTO audit_pacientes (operacion, id_pacientes, nombre, apellido)
        VALUES ('DELETE', OLD.id_pacientes, OLD.nombre, OLD.apellido);
    END IF;
    RETURN NULL;
END;
$$ LANGUAGE plpgsql;



CREATE TRIGGER trg_pacientes
AFTER INSERT OR UPDATE OR DELETE ON Pacientes
FOR EACH ROW
EXECUTE FUNCTION fnc_pacientes();
```

## Tabla de auditoría: Citas Médicas
```sql
CREATE TABLE audit_citas_medicas(
    id SERIAL PRIMARY KEY,
    id_citas INT,
    id_pacientes INT,
    id_medicos INT,
    fecha_cita DATE,
    hora_cita TIME,
    fecha_operacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    usuario TEXT DEFAULT current_user
);



CREATE OR REPLACE FUNCTION fnc_CitasMedicas()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO audit_citas_medicas(id_citas, id_pacientes, id_medicos, fecha_cita, hora_cita)
    VALUES (NEW.id_citas, NEW.id_pacientes, NEW.id_medicos, NEW.fecha_cita, NEW.hora_cita);
    RETURN NULL;
END;
$$ LANGUAGE plpgsql;



CREATE TRIGGER trg_citasMedicas
AFTER INSERT ON CitasMedicas
FOR EACH ROW
EXECUTE FUNCTION fnc_CitasMedicas();

```

## Tabla de auditoría: Facturación
```sql
CREATE TABLE audit_facturacion(
    id SERIAL PRIMARY KEY,
    operacion VARCHAR(10),
    id_factura INT,
    id_citas INT,
    monto_total NUMERIC,
    fecha_emision DATE,
    estado BOOLEAN,
    fecha_operacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    usuario TEXT DEFAULT current_user
);



CREATE OR REPLACE FUNCTION fnc_facturacion()
RETURNS TRIGGER AS $$
BEGIN
    IF (TG_OP = 'INSERT') THEN
        INSERT INTO audit_facturacion (operacion, id_factura, id_citas, monto_total, fecha_emision, estado)
        VALUES ('INSERT', NEW.id_factura, NEW.id_citas, NEW.monto_total, NEW.fecha_emision, NEW.estado);
    ELSIF (TG_OP = 'UPDATE') THEN
        INSERT INTO audit_facturacion (operacion, id_factura, id_citas, monto_total, fecha_emision, estado)
        VALUES ('UPDATE', NEW.id_factura, NEW.id_citas, NEW.monto_total, NEW.fecha_emision, NEW.estado);
    ELSIF (TG_OP = 'DELETE') THEN
        INSERT INTO audit_facturacion (operacion, id_factura, id_citas, monto_total, fecha_emision, estado)
        VALUES ('DELETE', OLD.id_factura, OLD.id_citas, OLD.monto_total, OLD.fecha_emision, OLD.estado);
    END IF;
    RETURN NULL;
END;
$$ LANGUAGE plpgsql;



CREATE TRIGGER trg_facturacion
AFTER INSERT OR UPDATE OR DELETE ON Facturacion
FOR EACH ROW
EXECUTE FUNCTION fnc_facturacion();

```

# Funciones
## Pacientes
```sql
CREATE OR REPLACE FUNCTION obtener_edad(p_id INT)
RETURNS INT AS $$
DECLARE
    edad INT;
BEGIN
    SELECT DATE_PART('year', AGE(Fecha_Nacimiento)) INTO edad
    FROM Pacientes
    WHERE id_pacientes = p_id;
    RETURN edad;
END;
$$ LANGUAGE plpgsql;



CREATE OR REPLACE FUNCTION listar_pacientes_activos()
RETURNS SETOF Pacientes AS $$
BEGIN
    RETURN QUERY
    SELECT * FROM Pacientes WHERE Estado = 1; -- Cambiado de TRUE a 1
END;
$$ LANGUAGE plpgsql;
```
## Medicos
```sql
CREATE OR REPLACE FUNCTION datos_medico(p_id INT)
RETURNS RECORD AS $$
DECLARE
    result RECORD;
BEGIN
    SELECT m.Nombre, m.Apellido, e.Nombre AS Especialidad
    INTO result
    FROM Medicos m
    JOIN Especialidades e ON m.id_especialidad = e.id_especialidad
    WHERE m.id_medicos = p_id;

    RETURN result;
END;
$$ LANGUAGE plpgsql;



CREATE OR REPLACE FUNCTION medicos_por_especialidad(p_id_especialidad INT)
RETURNS SETOF Medicos AS $$
BEGIN
    RETURN QUERY
    SELECT * FROM Medicos
    WHERE id_especialidad = p_id_especialidad;
END;
$$ LANGUAGE plpgsql;
```

## Citas Médicas
```sql
CREATE OR REPLACE FUNCTION citas_activas()
RETURNS SETOF CitasMedicas AS $$
BEGIN
    RETURN QUERY
    SELECT * FROM CitasMedicas
    WHERE Estado = 1; -- Cambiado de TRUE a 1
END;
$$ LANGUAGE plpgsql;



CREATE OR REPLACE FUNCTION total_citas_por_fecha(p_medico_id INT, p_fecha DATE)
RETURNS INT AS $$
DECLARE
    total INT;
BEGIN
    SELECT COUNT(*) INTO total
    FROM CitasMedicas
    WHERE id_medicos = p_medico_id AND Fecha_Cita = p_fecha;
    RETURN total;
END;
$$ LANGUAGE plpgsql;
```

## Historial Clinico
```sql
CREATE OR REPLACE FUNCTION contar_historiales_activos(p_id INT)
RETURNS INT AS $$
DECLARE
    total INT;
BEGIN
    SELECT COUNT(*) INTO total
    FROM HistorialClinico
    WHERE id_pacientes = p_id AND Estado = 1; -- Cambiado de TRUE a 1
    RETURN total;
END;
$$ LANGUAGE plpgsql;



CREATE OR REPLACE FUNCTION ultimo_historial(p_id INT)
RETURNS RECORD AS $$
DECLARE
    result RECORD;
BEGIN
    SELECT id_historial, Fecha_Creacion, Descripcion
    INTO result
    FROM HistorialClinico
    WHERE id_pacientes = p_id
    ORDER BY Fecha_Creacion DESC
    LIMIT 1;

    RETURN result;
END;
$$ LANGUAGE plpgsql;
```

# Consultas
## Pacientes
```sql
-- INNER JOIN: Pacientes con sus citas médicas
SELECT p.Nombre, p.Apellido, c.Fecha_Cita
FROM Pacientes p
INNER JOIN CitasMedicas c ON p.id_pacientes = c.id_pacientes;

-- LEFT JOIN: Pacientes y sus reservas (aunque no tengan)
SELECT p.Nombre, p.Apellido, r.Fecha_Reserva
FROM Pacientes p
LEFT JOIN ReservarCitas r ON p.id_pacientes = r.id_pacientes;
```

## Medicos
```sql
-- INNER JOIN: Médicos con su especialidad
SELECT m.Nombre, m.Apellido, e.Nombre AS Especialidad
FROM Medicos m
INNER JOIN Especialidades e ON m.id_especialidad = e.id_especialidad;

-- LEFT JOIN: Médicos con urgencias atendidas (incluso si no han atendido ninguna)
SELECT m.Nombre, m.Apellido, u.Sintomas
FROM Medicos m
LEFT JOIN Urgencias u ON m.id_medicos = u.id_medicos;
```

## Especialidades
```sql
-- LEFT JOIN: Especialidades con sus médicos
SELECT e.Nombre, m.Nombre AS Nombre_Medico
FROM Especialidades e
LEFT JOIN Medicos m ON e.id_especialidad = m.id_especialidad;

-- RIGHT JOIN: Médicos y especialidades (en caso de especialidades sin médicos)
SELECT m.Nombre, e.Nombre AS Especialidad
FROM Medicos m
RIGHT JOIN Especialidades e ON m.id_especialidad = e.id_especialidad;
```

## Administrativos
```sql
-- LEFT JOIN: Administrativos y facturas gestionadas (simulado con pacientes si no hay tabla directa)
SELECT a.Nombre, a.Apellido, f.id_factura
FROM Administrativos a
LEFT JOIN Facturacion f ON a.id_administrativo = f.id_factura; -- Supuesto si existiera tal relación

-- RIGHT JOIN: Simulado (no hay relación directa, se usará Pacientes como ejemplo)
SELECT p.Nombre, a.Nombre AS Nombre_Admin
FROM Administrativos a
RIGHT JOIN Pacientes p ON a.DNI = p.DNI; -- Solo como ejemplo basado en DNI
```

## Citas Médicas
```sql

-- INNER JOIN: Citas con pacientes y médicos
SELECT c.Fecha_Cita, p.Nombre AS Paciente, m.Nombre AS Medico
FROM CitasMedicas c
INNER JOIN Pacientes p ON c.id_pacientes = p.id_pacientes
INNER JOIN Medicos m ON c.id_medicos = m.id_medicos;

-- LEFT JOIN: Citas y consultorios
SELECT c.Fecha_Cita, co.Nombre AS Consultorio
FROM CitasMedicas c
LEFT JOIN Consultorios co ON c.id_consultorios = co.id_consultorios;

```

## Reservar citas
```sql
-- INNER JOIN: Reservas con pacientes
SELECT r.Fecha_Reserva, p.Nombre
FROM ReservarCitas r
INNER JOIN Pacientes p ON r.id_pacientes = p.id_pacientes;

-- LEFT JOIN: Reservas con citas (incluyendo reservas sin citas registradas)
SELECT r.id_reserva, c.Fecha_Cita
FROM ReservarCitas r
LEFT JOIN CitasMedicas c ON r.id_citas = c.id_citas;
```

## Urgencias
```sql

-- INNER JOIN: Urgencias con pacientes
SELECT u.Fecha_Ingreso, p.Nombre, u.Sintomas
FROM Urgencias u
INNER JOIN Pacientes p ON u.id_pacientes = p.id_pacientes;

-- RIGHT JOIN: Médicos con urgencias (para ver médicos que podrían no tener urgencias)
SELECT m.Nombre, u.Sintomas
FROM Urgencias u
RIGHT JOIN Medicos m ON u.id_medicos = m.id_medicos;
```

## Historial Clinico
```sql
-- INNER JOIN: Historial clínico con pacientes
SELECT h.id_historial, p.Nombre
FROM HistorialClinico h
INNER JOIN Pacientes p ON h.id_pacientes = p.id_pacientes;

-- LEFT JOIN: Historiales con diagnósticos
SELECT h.id_historial, d.Descripcion
FROM HistorialClinico h
LEFT JOIN Diagnosticos d ON h.id_historial = d.id_historial;
```

## Diagnosticos
```sql
-- INNER JOIN: Diagnóstico con paciente y médico
SELECT d.Descripcion, p.Nombre AS Paciente, m.Nombre AS Medico
FROM Diagnosticos d
INNER JOIN Pacientes p ON d.id_pacientes = p.id_pacientes
INNER JOIN Medicos m ON d.id_medicos = m.id_medicos;

-- LEFT JOIN: Diagnósticos con tratamientos
SELECT d.Descripcion, t.Descripcion AS Tratamiento
FROM Diagnosticos d
LEFT JOIN Tratamientos t ON d.id_diagnostico = t.id_diagnostico;
```

## Tratamientos
```sql
-- INNER JOIN: Tratamiento con diagnóstico
SELECT t.Descripcion, d.Descripcion AS Diagnostico
FROM Tratamientos t
INNER JOIN Diagnosticos d ON t.id_diagnostico = d.id_diagnostico;

-- LEFT JOIN: Tratamiento con recetas
SELECT t.Descripcion, r.Medicamento
FROM Tratamientos t
LEFT JOIN RecetasMedica r ON t.id_tratamiento = r.id_tratamiento;

```

## Recetas Médicas
```sql

-- INNER JOIN: Recetas con tratamiento
SELECT r.Medicamento, t.Descripcion
FROM RecetasMedica r
INNER JOIN Tratamientos t ON r.id_tratamiento = t.id_tratamiento;

-- RIGHT JOIN: Tratamientos con recetas
SELECT t.Descripcion, r.Medicamento
FROM RecetasMedica r
RIGHT JOIN Tratamientos t ON r.id_tratamiento = t.id_tratamiento;
```

## Recetas Médicas
```sql
-- INNER JOIN: Recetas con tratamiento
SELECT r.Medicamento, t.Descripcion
FROM RecetasMedica r
INNER JOIN Tratamientos t ON r.id_tratamiento = t.id_tratamiento;

-- RIGHT JOIN: Tratamientos con recetas
SELECT t.Descripcion, r.Medicamento
FROM RecetasMedica r
RIGHT JOIN Tratamientos t ON r.id_tratamiento = t.id_tratamiento;

```

## Inventario Medicamentos
```sql
-- Simulación con RIGHT JOIN y LEFT JOIN usando RecetasMedica por nombre de medicamento
SELECT i.Nombre, r.Medicamento
FROM InventarioMedicamentos i
LEFT JOIN RecetasMedica r ON i.Nombre = r.Medicamento;

SELECT i.Nombre, r.Medicamento
FROM InventarioMedicamentos i
RIGHT JOIN RecetasMedica r ON i.Nombre = r.Medicamento;

```

## Consultorios
```sql
-- INNER JOIN: Consultorios con citas
SELECT co.Nombre, c.Fecha_Cita
FROM Consultorios co
INNER JOIN CitasMedicas c ON co.id_consultorios = c.id_consultorios;

-- RIGHT JOIN: Citas con consultorios
SELECT c.Fecha_Cita, co.Nombre AS Consultorio
FROM CitasMedicas c
RIGHT JOIN Consultorios co ON c.id_consultorios = co.id_consultorios;

```

## Salas
```sql
SELECT s.Tipo, c.Nombre
FROM Salas s
LEFT JOIN Consultorios c ON s.id_sala = c.id_consultorios;

SELECT c.Nombre, s.Tipo
FROM Salas s
RIGHT JOIN Consultorios c ON s.id_sala = c.id_consultorios;
```

## Facturación
```sql
-- INNER JOIN: Factura con paciente
SELECT f.id_factura, p.Nombre
FROM Facturacion f
INNER JOIN Pacientes p ON f.id_pacientes = p.id_pacientes;

-- LEFT JOIN: Facturas con citas
SELECT f.id_factura, c.Fecha_Cita
FROM Facturacion f
LEFT JOIN CitasMedicas c ON f.id_citas = c.id_citas;
```


# Subconsultas


```sql
-- 1.1 MEDICOS -> ESPECIALIDADES (Subconsulta 1)
SELECT Nombre,
  (SELECT Nombre FROM Especialidades e WHERE e.id_especialidad = m.id_especialidad) AS Especialidad
FROM Medicos m;

-- 1.2 MEDICOS -> ESPECIALIDADES (Subconsulta 2)
SELECT *,
  (SELECT Descripcion FROM Especialidades e WHERE e.id_especialidad = m.id_especialidad) AS Detalle_Especialidad
FROM Medicos m;

-- 2.1 CITAS MEDICAS -> PACIENTES (Subconsulta 1)
SELECT Fecha_Cita,
  (SELECT Nombre FROM Pacientes p WHERE p.id_pacientes = c.id_pacientes) AS Nombre_Paciente
FROM CitasMedicas c;

-- 2.2 CITAS MEDICAS -> PACIENTES (Subconsulta 2)
SELECT *,
  (SELECT Telefono FROM Pacientes p WHERE p.id_pacientes = c.id_pacientes) AS Contacto_Paciente
FROM CitasMedicas c;

-- 3.1 CITAS MEDICAS -> MEDICOS (Subconsulta 1)
SELECT Fecha_Cita,
  (SELECT Nombre FROM Medicos m WHERE m.id_medicos = c.id_medicos) AS Nombre_Medico
FROM CitasMedicas c;

-- 3.2 CITAS MEDICAS -> MEDICOS (Subconsulta 2)
SELECT *,
  (SELECT id_especialidad FROM Medicos m WHERE m.id_medicos = c.id_medicos) AS Especialidad_Medico
FROM CitasMedicas c;

-- 4.1 CITAS MEDICAS -> CONSULTORIOS (Subconsulta 1)
SELECT Fecha_Cita,
  (SELECT Nombre FROM Consultorios con WHERE con.id_consultorios = c.id_consultorios) AS Consultorio
FROM CitasMedicas c;

-- 4.2 CITAS MEDICAS -> CONSULTORIOS (Subconsulta 2)
SELECT *,
  (SELECT Ubicacion FROM Consultorios con WHERE con.id_consultorios = c.id_consultorios) AS Direccion_Consultorio
FROM CitasMedicas c;

-- 5. RESERVAR CITAS -> PACIENTES
SELECT Fecha_Reserva,
  (SELECT Nombre FROM Pacientes p WHERE p.id_pacientes = r.id_pacientes) AS Paciente
FROM ReservarCitas r;

-- 6.1 RESERVAR CITAS -> CITAS MEDICAS (Subconsulta 1)
SELECT Fecha_Reserva,
  (SELECT Fecha_Cita FROM CitasMedicas c WHERE c.id_citas = r.id_citas) AS Fecha_Cita
FROM ReservarCitas r;

-- 6.2 RESERVAR CITAS -> CITAS MEDICAS (Subconsulta 2)
SELECT *,
  (SELECT id_consultorios FROM CitasMedicas c WHERE c.id_citas = r.id_citas) AS Consultorio_Asignado
FROM ReservarCitas r;

-- 7.1 URGENCIAS -> PACIENTES (Subconsulta 1)
SELECT Fecha_Ingreso,
  (SELECT Nombre FROM Pacientes p WHERE p.id_pacientes = u.id_pacientes) AS Paciente
FROM Urgencias u;

-- 7.2 URGENCIAS -> PACIENTES (Subconsulta 2)
SELECT *,
  (SELECT Grupo_Sanguineo FROM Pacientes p WHERE p.id_pacientes = u.id_pacientes) AS Tipo_Sangre
FROM Urgencias u;

-- 8.1 URGENCIAS -> MEDICOS (Subconsulta 1)
SELECT Fecha_Ingreso,
  (SELECT Nombre FROM Medicos m WHERE m.id_medicos = u.id_medicos) AS Medico
FROM Urgencias u;

-- 8.2 URGENCIAS -> MEDICOS (Subconsulta 2)
SELECT *,
  (SELECT id_especialidad FROM Medicos m WHERE m.id_medicos = u.id_medicos) AS Especialidad_Medico
FROM Urgencias u;

-- 9.1 HISTORIAL CLINICO -> PACIENTES (Subconsulta 1)
SELECT id_historial,
  (SELECT Nombre FROM Pacientes p WHERE p.id_pacientes = h.id_pacientes) AS Paciente
FROM HistorialClinico h;

-- 9.2 HISTORIAL CLINICO -> PACIENTES (Subconsulta 2)
SELECT *,
  (SELECT Fecha_Nacimiento FROM Pacientes p WHERE p.id_pacientes = h.id_pacientes) AS Fecha_Nacimiento
FROM HistorialClinico h;

-- 10.1 DIAGNOSTICOS -> PACIENTES (Subconsulta 1)
SELECT Descripcion,
  (SELECT Nombre FROM Pacientes p WHERE p.id_pacientes = d.id_pacientes) AS Paciente
FROM Diagnosticos d;

-- 10.2 DIAGNOSTICOS -> PACIENTES (Subconsulta 2)
SELECT *,
  (SELECT Grupo_Sanguineo FROM Pacientes p WHERE p.id_pacientes = d.id_pacientes) AS Tipo_Sangre
FROM Diagnosticos d;

-- 11.1 DIAGNOSTICOS -> MEDICOS (Subconsulta 1)
SELECT Descripcion,
  (SELECT Nombre FROM Medicos m WHERE m.id_medicos = d.id_medicos) AS Medico
FROM Diagnosticos d;

-- 11.2 DIAGNOSTICOS -> MEDICOS (Subconsulta 2)
SELECT *,
  (SELECT id_especialidad FROM Medicos m WHERE m.id_medicos = d.id_medicos) AS Especialidad
FROM Diagnosticos d;

-- 12.1 DIAGNOSTICOS -> HISTORIAL CLINICO (Subconsulta 1)
SELECT Descripcion,
  (SELECT Fecha FROM HistorialClinico h WHERE h.id_historial = d.id_historial) AS Fecha_Historial
FROM Diagnosticos d;

-- 12.2 DIAGNOSTICOS -> HISTORIAL CLINICO (Subconsulta 2)
SELECT *,
  (SELECT id_pacientes FROM HistorialClinico h WHERE h.id_historial = d.id_historial) AS Paciente_Historial
FROM Diagnosticos d;

-- 13.1 TRATAMIENTOS -> DIAGNOSTICOS (Subconsulta 1)
SELECT Nombre_Tratamiento,
  (SELECT Descripcion FROM Diagnosticos d WHERE d.id_diagnostico = t.id_diagnostico) AS Diagnostico
FROM Tratamientos t;

-- 13.2 TRATAMIENTOS -> DIAGNOSTICOS (Subconsulta 2)
SELECT *,
  (SELECT id_pacientes FROM Diagnosticos d WHERE d.id_diagnostico = t.id_diagnostico) AS Paciente
FROM Tratamientos t;

-- 14.1 RECETAS MEDICAS -> TRATAMIENTOS (Subconsulta 1)
SELECT Medicamento,
  (SELECT Nombre_Tratamiento FROM Tratamientos t WHERE t.id_tratamiento = r.id_tratamiento) AS Tratamiento
FROM RecetasMedica r;

-- 14.2 RECETAS MEDICAS -> TRATAMIENTOS (Subconsulta 2)
SELECT *,
  (SELECT id_diagnostico FROM Tratamientos t WHERE t.id_tratamiento = r.id_tratamiento) AS Diagnostico
FROM RecetasMedica r;

-- 15.1 FACTURACION -> PACIENTES (Subconsulta 1)
SELECT Monto,
  (SELECT Nombre FROM Pacientes p WHERE p.id_pacientes = f.id_pacientes) AS Paciente
FROM Facturacion f;

-- 15.2 FACTURACION -> PACIENTES (Subconsulta 2)
SELECT *,
  (SELECT Edad FROM Pacientes p WHERE p.id_pacientes = f.id_pacientes) AS Edad
FROM Facturacion f;

-- 16.1 FACTURACION -> CITAS MEDICAS (Subconsulta 1)
SELECT Monto,
  (SELECT Fecha_Cita FROM CitasMedicas c WHERE c.id_citas = f.id_citas) AS Fecha_Cita
FROM Facturacion f;

-- 16.2 FACTURACION -> CITAS MEDICAS (Subconsulta 2)
SELECT *,
  (SELECT id_medicos FROM CitasMedicas c WHERE c.id_citas = f.id_citas) AS Medico_Facturado
FROM Facturacion f;
```

# Optimización: Uso de CTE
```sql
-- 1. CONSULTA COMPLEJA: DIAGNOSTICOS + PACIENTES + MÉDICOS + HISTORIALCLINICO
WITH PacientesCTE AS (
  SELECT id_pacientes, nombre AS nombre_paciente
  FROM Pacientes
),
MedicosCTE AS (
  SELECT id_medicos, nombre AS nombre_medico
  FROM Medicos
),
HistorialCTE AS (
  SELECT id_historial, Fecha_Creacion AS fecha_historial
  FROM HistorialClinico
)
SELECT d.id_diagnostico, 
       d.descripcion,
       p.nombre_paciente,
       m.nombre_medico,
       h.fecha_historial
FROM Diagnosticos d
JOIN PacientesCTE p ON d.id_pacientes = p.id_pacientes
JOIN MedicosCTE m ON d.id_medicos = m.id_medicos
JOIN HistorialCTE h ON d.id_historial = h.id_historial;

-- 2. OPTIMIZACIÓN CON CTE: FACTURACION + PACIENTES + CITASMEDICAS + MÉDICOS
WITH PacientesCTE AS (
  SELECT id_pacientes, nombre
  FROM Pacientes
),
CitasCTE AS (
  SELECT id_citas, fecha_cita, id_medicos
  FROM CitasMedicas
),
MedicosCTE AS (
  SELECT id_medicos, nombre AS nombre_medico
  FROM Medicos
)
SELECT f.id_factura, 
       f.Monto_Total,
       p.nombre AS nombre_paciente,
       c.fecha_cita,
       m.nombre_medico
FROM Facturacion f
JOIN PacientesCTE p ON f.id_pacientes = p.id_pacientes
JOIN CitasCTE c ON f.id_citas = c.id_citas
JOIN MedicosCTE m ON c.id_medicos = m.id_medicos;
```

# Creacion de usuarios
CREATE USER administrativo WITH PASSWORD 'admin1234';

CREATE USER medico WITH PASSWORD 'medic1234';

CREATE USER paciente WITH PASSWORD 'user1234';

CREATE ROLE rol_admin;

CREATE ROLE rol_medic;

CREATE ROLE rol_user;

GRANT ALL PRIVILEGES ON DATABASE clinica TO rol_admin;

GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO rol_admin;

GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO rol_admin;

GRANT CONNECT ON DATABASE clinica TO rol_medic;

GRANT INSERT, UPDATE ON ALL TABLES IN SCHEMA public TO rol_medic;

GRANT USAGE ON ALL SEQUENCES IN SCHEMA public TO rol_medic;

GRANT CONNECT ON DATABASE clinica TO rol_user;

GRANT SELECT ON ALL TABLES IN SCHEMA public TO rol_user;

GRANT rol_admin TO administrativo;

GRANT rol_medic TO medico;

GRANT rol_user TO paciente;


# Caso de estudio
## CONSULTA SIN CTE
```sql
EXPLAIN ANALYZE
SELECT d .* , p.Nombre, p.Apellido, m.Nombre AS
Medico_Nombre, m.Apellido AS Medico_Apellido
FROM Diagnosticos d
JOIN Pacientes p ON d.id_pacientes =
p.id_pacientes
JOIN Medicos m ON d.id_medicos = m.id_medicos
WHERE d.Fecha_Diagnostico >= NOW() -
INTERVAL '6 months'
ORDER BY d.Fecha_Diagnostico DESC
LIMIT 100;
```

## CONSULTA CON CTE MATERIALIZADO
```sql
EXPLAIN ANALYZE
WITH MATERIALIZED diagnosticos_recientes AS (
SELECT *

FROM Diagnosticos
WHERE Fecha_Diagnostico >= NOW() - INTERVAL '6
months'
)
SELECT dr .* , p.Nombre, p.Apellido, m.Nombre AS
Medico_Nombre, m.Apellido AS Medico_Apellido
FROM diagnosticos_recientes dr
JOIN Pacientes p ON dr.id_pacientes = p.id_pacientes
JOIN Medicos m ON dr.id_medicos = m.id_medicos
ORDER BY dr.Fecha_Diagnostico DESC
LIMIT 100;
```