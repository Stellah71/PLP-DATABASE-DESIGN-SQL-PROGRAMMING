# PLP-DATABASE-DESIGN-SQL-PROGRAMMING

Use Case: Clinic Booking System
The system will manage patients, doctors, appointments, and treatments.

CREATE TABLE Doctors (
    doctor_id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    specialization VARCHAR(100),
    email VARCHAR(100) UNIQUE NOT NULL,
    phone_number VARCHAR(15) UNIQUE
);

-- Table: Patients
CREATE TABLE Patients (
    patient_id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    date_of_birth DATE NOT NULL,
    gender ENUM('Male', 'Female', 'Other'),
    email VARCHAR(100) UNIQUE,
    phone_number VARCHAR(15) UNIQUE
);

-- Table: Appointments
CREATE TABLE Appointments (
    appointment_id INT AUTO_INCREMENT PRIMARY KEY,
    doctor_id INT NOT NULL,
    patient_id INT NOT NULL,
    appointment_date DATETIME NOT NULL,
    reason TEXT,
    status ENUM('Scheduled', 'Completed', 'Cancelled') DEFAULT 'Scheduled',
    FOREIGN KEY (doctor_id) REFERENCES Doctors(doctor_id),
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id)
);

-- Table: Treatments
CREATE TABLE Treatments (
    treatment_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE,
    description TEXT,
    cost DECIMAL(10, 2) NOT NULL
);

-- Junction Table: Appointment_Treatment
CREATE TABLE Appointment_Treatment (
    appointment_id INT,
    treatment_id INT,
    PRIMARY KEY (appointment_id, treatment_id),
    FOREIGN KEY (appointment_id) REFERENCES Appointments(appointment_id) ON DELETE CASCADE,
    FOREIGN KEY (treatment_id) REFERENCES Treatments(treatment_id) ON DELETE CASCADE
);

-- Insert sample Doctors
INSERT INTO Doctors (first_name, last_name, specialization, email, phone_number) VALUES
('John', 'Doe', 'Cardiologist', 'johndoe@example.com', '0712345678'),
('Jane', 'Smith', 'Pediatrician', 'janesmith@example.com', '0723456789');

-- Insert sample Patients
INSERT INTO Patients (first_name, last_name, date_of_birth, gender, email, phone_number) VALUES
('Alice', 'Wycliff', '1990-05-15', 'Female', 'alice@example.com', '0734567890'),
('Bob', 'Njenga', '1985-08-22', 'Male', 'bob@example.com', '0745678901');

-- Insert sample Treatments
INSERT INTO Treatments (name, description, cost) VALUES
('ECG', 'Electrocardiogram Test', 1500.00),
('Vaccination', 'Routine immunization', 500.00),
('Consultation', 'Doctor consultation', 1000.00);

-- Insert sample Appointments
INSERT INTO Appointments (doctor_id, patient_id, appointment_date, reason, status) VALUES
(1, 1, '2025-05-10 09:30:00', 'Chest pain', 'Scheduled'),
(2, 2, '2025-05-11 11:00:00', 'Child vaccination', 'Scheduled');

-- Insert into Appointment_Treatment
INSERT INTO Appointment_Treatment (appointment_id, treatment_id) VALUES
(1, 1),  -- ECG for appointment 1
(1, 3),  -- Consultation for appointment 1
(2, 2);  -- Vaccination for appointment 2
