# GFMS - Garbage Fleet Management System 🚛🗑️

This project was developed as part of my Final Year Project when I was still fairly new to programming. The system is designed to manage and monitor garbage truck fleets efficiently, integrating real-time tracking and maintenance management.

## 💻 Built With

- **PHP** (Core backend)
- **HTML, CSS, JS** (Frontend)
- **MySQL** (Database)
- **Google Maps API** (Real-time truck location monitoring)
- **ESP32 v2 + GY-NEO6MV2 GPS Sensor** (Hardware-based tracking)

## ⚙️ Features

- Truck check-in and check-out system
- Damage reporting and tracking
- Maintenance logging
- Real-time GPS tracking of trucks via Google Maps
- Route and driver management
- Admin login and user roles

## 🧰 Tools Used (Hardware)

- **ESP32 v2**
- **GPS Sensor GY-NEO6MV2**
- **LED** *(optional)*
- **Breadboard** *(optional if not soldered)*
- **External power supply** *(for ESP32 module)*
- **LCD Panel (Optional)*

---

## 📊 Database Schema (MySQL)

All tables use **InnoDB** engine and `utf8mb4` charset. Foreign key constraints are enforced as defined below. All primary keys are `AUTO_INCREMENT`.

### `checkin`

| Column         | Type        | Notes                                  |
|----------------|-------------|----------------------------------------|
| id             | int         | Primary Key                            |
| name           | varchar     |                                        |
| plate_number   | varchar     | FK → routes.no_plate                   |
| status         | varchar     |                                        |
| arrival_time   | datetime    |                                        |
| checkout_time  | datetime    |                                        |
| driver_id      | int         |                                        |
| route_id       | int         | FK → routes.id                         |
| remarks        | text        |                                        |
| image          | varchar     | (image path or URL)                    |

**Indexes**: `PRIMARY`, `plate_number`, `driver_id`, `route_id`

---

### `damagefrm`

| Column        | Type       | Notes                           |
|---------------|------------|---------------------------------|
| id            | int        | Primary Key                     |
| description   | varchar    |                                 |
| date          | date       |                                 |
| location      | varchar    |                                 |
| image         | varchar    |                                 |
| truckid       | int        | FK → trucks.truckid             |
| severity      | varchar    |                                 |
| repair_status | varchar    |                                 |
| repair_date   | date       |                                 |
| cost          | decimal    |                                 |

**Indexes**: `PRIMARY`, `truckid`

---

### `garbagetruckmaintenance`

| Column           | Type     | Notes                           |
|------------------|----------|---------------------------------|
| maintenance_id   | int      | Primary Key                     |
| truckid          | int      | FK → trucks.truckid             |
| routeid          | int      | FK → routes.id                  |
| maintenance_date | date     |                                 |
| maintenance_type | varchar  |                                 |
| description      | text     |                                 |
| cost             | decimal  |                                 |

**Indexes**: `PRIMARY`, `truckid`, `routeid`

---

### `locations`

| Column    | Type     | Notes         |
|-----------|----------|---------------|
| id        | int      | Primary Key   |
| latitude  | double   |               |
| longitude | double   |               |
| timestamp | timestamp|               |

---

### `managers`

| Column     | Type      | Notes         |
|------------|-----------|---------------|
| id         | int       | Primary Key   |
| name       | varchar   |               |
| email      | varchar   |               |
| password   | varchar   |               |
| phone      | varchar   |               |
| address    | varchar   |               |
| created_at | datetime  |               |

---

### `routes`

| Column     | Type     | Notes                     |
|------------|----------|---------------------------|
| id         | int      | Primary Key               |
| name       | varchar  |                           |
| no_plate   | varchar  | Unique                    |
| model      | varchar  |                           |
| location   | varchar  |                           |
| status     | varchar  |                           |
| start_time | time     |                           |
| end_time   | time     |                           |
| chosen     | tinyint  | Boolean (0 or 1)          |

**Indexes**: `PRIMARY`, `UNIQUE(no_plate)`

---

### `trucks`

| Column       | Type     | Notes                     |
|--------------|----------|---------------------------|
| truckid      | int      | Primary Key               |
| routeid      | int      | FK → routes.id            |
| model        | varchar  |                           |
| licensePlate | varchar  |                           |
| driver_name  | varchar  |                           |
| capacity     | int      |                           |

**Indexes**: `PRIMARY`, `routeid`

---

### `users`

| Column     | Type      | Notes             |
|------------|-----------|-------------------|
| id         | int       | Primary Key       |
| name       | varchar   |                   |
| email      | varchar   | Unique            |
| password   | varchar   |                   |
| phone      | varchar   |                   |
| address    | varchar   |                   |
| created_at | datetime  |                   |

**Indexes**: `PRIMARY`, `UNIQUE(email)`

## 📦 Installation & Setup

> ⚙️ **Preparation**
- Download the project ZIP file and extract it.
- Inside the extracted folder, you'll find:
  - PHP files for the system backend.
  - Embedded JavaScript, CSS, and HTML files for frontend interaction.
  - Arduino code for hardware integration (ESP32 and GPS).

---

> 🛠 **Hardware Requirements**
- **Microcontroller:** ESP32 (v2)
- **GPS Sensor:** GY-NEO6MV2
- **Optional:** LED, LCD Panel, Breadboard (only if not soldered)
- **Power Supply:** Use only compatible voltage for ESP32 to avoid hardware damage.

---

> 🧪 **Arduino Setup**
1. Open the Arduino IDE.
2. Load the Arduino code from the included folder or write your own for extended features (e.g., LED/LCD usage).
3. Install required libraries for ESP32 support and GPS functionality.
4. Connect the ESP32 and GY-NEO6MV2 properly.
5. Ensure GPS sensor is placed in an open area for optimal satellite signal.

---

> 💾 **Database Setup**
1. Use the provided database SQL file to set up the structure.
2. If preferred, you can use **Firebase** to collect and store GPS data in real time.
3. Later, sync Firebase data to your **XAMPP** database (XAMPP is included for ease of installation).

---

> 🧬 **Running the Project**
1. Start Apache and MySQL in XAMPP.
2. Place the project files in the `htdocs` directory.
3. Access the system via `http://localhost/<your_project_folder>`.
4. Make sure ESP32 sends data correctly to the database (either via direct HTTP or Firebase route).
5. Run the system in a browser to view the real-time location tracking via Google Maps API.

---

> 🧩 **Additional Notes**
- If using a different microcontroller, ensure it's compatible with the **GY-NEO6MV2 GPS sensor**.
- Real-time location tracking is embedded using **Google Maps API**.

---

> ✅ **Project Summary**
This project is a simple IoT-based Garbage Fleet Management System created using:
- PHP backend
- Embedded JavaScript, CSS, and HTML frontend
- Google Maps API for real-time fleet tracking
- Arduino-compatible ESP32 microcontroller and GPS module



## 📍 Real-Time Location Feature

The system supports real-time GPS tracking using ESP32 and GY-NEO6MV2 GPS module. Location data is sent to the backend via HTTP/MQTT and shown on the dashboard using the **Google Maps API**.

---

## 🙋‍♂️ Questions or Support

If you have any questions, suggestions, or need support regarding this project, feel free to reach out or open an issue.

📬 **Preferred Contact Methods**  
I'm not very active on other platforms, so please use one of the following:

- 📧 Email: [atifafifi16@gmail.com](mailto:atifafifi16@gmail.com)  
- 📷 Instagram: [___a_t_i_f__](https://www.instagram.com/___a_t_i_f__?igsh=bXJ5cTNqZWV3dng3)

I'll do my best to get back to you as soon as possible!



---

