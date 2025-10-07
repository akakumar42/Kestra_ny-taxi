# Kestra_ny-taxi
Using kestra to automate ETL pipeline on NY-taxi database
Collecting workspace information```md
# Kestra NY-Taxi ETL Pipeline

This project uses [Kestra](https://kestra.io/) to automate an ETL pipeline for the NYC Taxi dataset, leveraging PostgreSQL for data storage and orchestration. The pipeline supports both Yellow and Green taxi data for the years 2019 and 2020.

---

## Project Structure

```
kestra_project/
│
├── docker-compose.yml                # Main Docker Compose for Kestra + Postgres
├── README.md                         # Project documentation
└── local_database/
    ├── docker-compose.yml            # Local Postgres + pgAdmin Compose file
    └── flow_code_postgres.yaml       # Kestra flow definition for ETL
```

---

## Components

### 1. Kestra Flow ([local_database/flow_code_postgres.yaml](local_database/flow_code_postgres.yaml))

- **Inputs:** Taxi type (yellow/green), year (2019/2020), month (01-12)
- **Extraction:** Downloads the appropriate CSV file from the NYC TLC GitHub releases.
- **Staging:** Loads data into a staging table in PostgreSQL.
- **Transformation:** Adds unique row IDs and filenames.
- **Merge:** Inserts new records into the main table, avoiding duplicates.
- **Cleanup:** Optionally purges temporary files after execution.

### 2. PostgreSQL & pgAdmin ([local_database/docker-compose.yml](local_database/docker-compose.yml))

- **Postgres:** Stores the taxi trip data.
- **pgAdmin:** Web UI for managing and inspecting the database.

### 3. Kestra Orchestration ([docker-compose.yml](docker-compose.yml))

- **Kestra:** Orchestrates the ETL workflow.
- **Postgres:** Used by Kestra for metadata and queue management.

---

## Getting Started

### Prerequisites

- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

### 1. Start Local Database (Postgres + pgAdmin)

```sh
cd local_database
docker-compose up -d
```

- Postgres: `localhost:5432` (user: `kestra`, password: `k3str4`, db: `postgres-zoomcamp`)
- pgAdmin: [http://localhost:5050](http://localhost:5050) (user: `admin@admin.com`, password: `admin`)

### 2. Start Kestra Orchestration

```sh
cd ..
docker-compose up -d
```

- Kestra UI: [http://localhost:8080](http://localhost:8080)

### 3. Configure and Run the ETL Flow

1. Open Kestra UI.
2. Import the flow from [`local_database/flow_code_postgres.yaml`](local_database/flow_code_postgres.yaml).
3. Run the flow, selecting taxi type, year, and month as needed.

---

## Database Details

- **Yellow Taxi Table:** `public.yellow_tripdata`
- **Green Taxi Table:** `public.green_tripdata`
- **Staging Tables:** Used for temporary data loading before merging.

---

## Credentials

- **Postgres User:** `kestra`
- **Postgres Password:** `k3str4`
- **Database:** `postgres-zoomcamp` (local), `kestra` (for Kestra metadata)

---

## Useful Links

- [Kestra Documentation](https://kestra.io/docs/)
- [NYC TLC Data](https://github.com/DataTalksClub/nyc-tlc-data)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)

---

## Notes

- The ETL flow is parameterized for easy selection of taxi type, year, and month.
- Data is deduplicated using a unique row ID.
- Temporary files are purged after each run for storage efficiency.

---

## License

MIT License
```
