# New Assignment - Airflow Connection & DAGs Running

## Pendahuluan
Tujuan dari tugas ini adalah untuk menjalankan Apache Airflow menggunakan Docker, menghubungkan database MySQL dan PostgreSQL, membuat koneksi di Apache Airflow, memodifikasi dan menjalankan DAG, serta menganalisis dan menyimpulkan hasil yang diperoleh. Berikut adalah langkah-langkah yang harus diikuti beserta bukti yang diperlukan.

## Langkah-langkah

### 1. Jalankan Docker Apache Airflow
- Jalankan Apache Airflow menggunakan Docker.
- Lampirkan bukti berupa:
  - Screenshot terminal yang menunjukkan Apache Airflow berjalan.
  - Screenshot localhost:8080 yang menunjukkan antarmuka Airflow.

### 2. Jalankan Docker Database MySQL dan PostgreSQL
- Jalankan database MySQL dan PostgreSQL menggunakan Docker.
- Lampirkan bukti berupa:
  - Screenshot terminal yang menunjukkan kedua database berjalan.
  - Screenshot DBeaver atau ekstensi koneksi MySQL yang terhubung dengan database PostgreSQL dan MySQL.

### 3. Buat Koneksi MySQL dan PostgreSQL di Apache Airflow
- Buat koneksi ke database MySQL dan PostgreSQL di Apache Airflow.
- Lampirkan bukti berupa screenshot UI hasil testing koneksi yang menunjukkan status "successful".

### 4. Modifikasi DAGs
- Modifikasi DAG `op_mysql` dan `op_postgresql` yang sudah ada dengan menambahkan `EmptyOperator`.
- Struktur DAG yang diharapkan:
  - **op_mysql**:
    ```python
    from datetime import datetime
    from airflow import DAG
    from airflow.providers.mysql.operators.mysql import MySqlOperator
    from airflow.operators.empty import EmptyOperator

    with DAG(
        dag_id='op_mysql_example', 
        start_date=datetime(2024, 5, 1),
        catchup=False,
        schedule_interval="@daily"
    ) as dag:
        start = EmptyOperator(task_id='start')
        create_new_table = MySqlOperator(
            task_id='create_new_table',
            mysql_conn_id='mysql_conn',
            sql="CREATE TABLE IF NOT EXISTS students (student_id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(256));",
        )
        insert_data = MySqlOperator(
            task_id='insert_data',
            mysql_conn_id='mysql_conn',
            sql="INSERT INTO students (name) VALUES ('Alex Jones');",
        )
        first_task = EmptyOperator(task_id='start')
        last_task = EmptyOperator(task_id='end')

    first_task >> create_new_table >> insert_data >> last_task
    ```

  - **op_postgresql**:
    ```python
    import datetime
    from airflow import DAG
    from airflow.providers.postgres.operators.postgres import PostgresOperator
    from airflow.operators.empty import EmptyOperator

    with DAG(
        dag_id='op_postgres_example',
        start_date=datetime.datetime(2024, 5, 1),
        catchup=False,
        schedule_interval="@weekly"
    ) as dag:
        start = EmptyOperator(task_id='start')
        create_pet_table = PostgresOperator(
            task_id='create_pet_table',
            sql="""
                CREATE TABLE IF NOT EXISTS pet (
                    pet_id SERIAL PRIMARY KEY,
                    name VARCHAR NOT NULL,
                    pet_type VARCHAR NOT NULL,
                    birth_date DATE NOT NULL,
                    owner VARCHAR NOT NULL
                );
            """,
            postgres_conn_id='postgres_default',
        )
        populate_pet_table = PostgresOperator(
            task_id='populate_pet_table',
            sql="sql/insert_pet.sql",
            postgres_conn_id='postgres_default',
        )
        end = EmptyOperator(task_id='end')

        start >> create_pet_table >> populate_pet_table >> end
    ```

### 5. Jalankan DAGs
- Jalankan DAG `op_mysql` dan `op_postgresql` hingga sukses.
- Lampirkan bukti berupa screenshot yang menunjukkan pipeline berjalan sukses semua.

### 6. Tampilkan Hasil Tabel
- Tampilkan hasil tabel yang dibuat oleh `op_mysql` dan `op_postgresql`.
- Lampirkan screenshot hasil tabel dari kedua DAG tersebut.

### 7. Analisis
- Jelaskan apa yang dikerjakan oleh `op_mysql` dan `op_postgresql`.
- Jelaskan perbedaan sebelum dan sesudah dijalankannya DAG.
- Berikan saran perbaikan dari kedua DAG tersebut.

### 8. Kesimpulan
- Buat kesimpulan mengenai Apache Airflow berdasarkan pengalaman dan hasil yang diperoleh dari tugas ini.
