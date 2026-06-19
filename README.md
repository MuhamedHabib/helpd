# HelpDesk — Formation Manager

> A small JavaFX desktop application for managing training records ("formations"), backed by a MySQL database.

![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![JavaFX](https://img.shields.io/badge/JavaFX-1F8AC0?style=for-the-badge&logo=java&logoColor=white)
![FXML](https://img.shields.io/badge/FXML-007396?style=for-the-badge&logo=java&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![JDBC](https://img.shields.io/badge/JDBC-FF6F00?style=for-the-badge&logo=databricks&logoColor=white)

## Overview

HelpDesk is a desktop CRUD application built with **JavaFX** and **FXML**. It presents an admin interface that lists training entries (`formation`) from a MySQL database in a table, and provides a separate form window for adding new entries.

The application connects to a local MySQL database named `helpdesk` and reads from / writes to a single table, `formationhd`, with the columns: `id_formation`, `libelle_formation` (label), `description`, and `date_creation` (creation date).

This is a learning project that demonstrates the JavaFX + FXML + JDBC stack with a classic MVC-style layout (FXML views, controller classes, an entity model, and a database helper).

## Features

- **Admin interface** — a main window (`tableView.fxml`) displaying all training records in a `TableView` with columns for ID, label, description, and creation date.
- **Add form** — a secondary form window (`add.fxml`) for entering a new record (label, description, and a date picker).
- **Insert into database** — new records are persisted to MySQL via a parameterized `PreparedStatement`.
- **Refresh** — reloads the table from the database on demand.
- **Clear form** — resets the input fields in the add form.
- **Buttons present in the UI** — `Print` and the close (`X`) button are wired in the FXML but their handlers are currently empty placeholders.

> Note: the codebase contains a query branch for an `UPDATE` (edit) operation, but it is not yet hooked up to an active edit flow in the UI.

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Language | Java |
| UI toolkit | JavaFX (`Application`, `Scene`, `Stage`) |
| Views | FXML (`tableView.fxml`, `add.fxml`) |
| Data access | JDBC (`java.sql`) |
| Database | MySQL (database `helpdesk`, table `formationhd`) |

## Project Structure

```
helpd/
├── Main.java            # JavaFX entry point — loads tableView.fxml
├── Controller.java      # Empty placeholder controller
├── TableViews.java      # Controller for the main table view (load, refresh, open add window)
├── AddController.java   # Controller for the add form (validate + insert)
├── DbConnect.java       # MySQL JDBC connection helper  (package: helpers)
├── formationhd.java     # Entity / model class           (package: Entity)
├── tableView.fxml       # Main admin interface layout
└── add.fxml             # Add-record form layout
```

Java packages referenced in the sources:
- `sample` — `Main`, `Controller`, `TableViews`, `AddController`
- `helpers` — `DbConnect`
- `Entity` — `formationhd`

> The files are stored flat at the repository root. To compile and run, place them under a source tree matching their `package` declarations (e.g. `src/sample/`, `src/helpers/`, `src/Entity/`) and put the `.fxml` files alongside `Main` in the `sample` package, since they are loaded via `getClass().getResource(...)`.

## Getting Started

### Prerequisites

- **JDK** with **JavaFX** available (e.g. JDK 11+ with the JavaFX SDK, or a JDK bundle that includes JavaFX)
- **MySQL** server running locally
- **MySQL JDBC driver** (Connector/J) on the classpath

### Database setup

Create the `helpdesk` database and the `formationhd` table. The application reads/writes these columns:

```sql
CREATE DATABASE helpdesk;
USE helpdesk;

CREATE TABLE formationhd (
    id_formation       INT AUTO_INCREMENT PRIMARY KEY,
    libelle_formation  VARCHAR(255),
    description        TEXT,
    date_creation      VARCHAR(255)
);
```

### Database connection

Connection settings live in `DbConnect.java`:

| Setting | Default value |
|---------|---------------|
| Host | `127.0.0.1` |
| Port | `3306` |
| Database | `helpdesk` |
| Username | `root` |
| Password | *(empty — local default)* |

Update these to match your local MySQL setup before running.

### Run

1. Arrange the sources into a package-correct source tree (see *Project Structure* note above) and keep the `.fxml` files in the same package as `Main`.
2. Compile with the JavaFX modules and the MySQL Connector/J driver on the classpath / module path.
3. Launch `sample.Main`.

The application opens the admin interface (`tableView.fxml`) and loads the current records from the database.

## Notes

- This is a **student / learning project** that demonstrates the JavaFX + FXML + JDBC pattern. It is intentionally small and focused on the core CRUD flow.
- Some UI actions (`Print`, the close `X` button) and the `UPDATE`/edit path are present in the code but not fully implemented.
- The `Add`/`Clean` button handlers exist in two forms in `AddController` (an empty `@FXML` stub and the active implementation) — the active logic is in the methods that build the query, validate the inputs, and insert the record.
- The MySQL credentials in `DbConnect.java` are local-development defaults; change them for any non-local environment.
- There is no build tool configured (no Maven `pom.xml` or Gradle `build.gradle`); the project is a plain set of Java sources and FXML files.

---
<p align="center">Built by <b>Mohamed Habib Khattat</b> — <a href="https://github.com/MuhamedHabib">GitHub (@MuhamedHabib)</a> · <a href="https://www.linkedin.com/in/mohamed-habib-khattat-2b206a173">LinkedIn</a></p>
