# How to include new Data

Things to know when we are using CSV files as data into supabase: 

1. You can only seed the TABLE if it's a new table
2. You can have primary keys as AUTOGENERATE Column. Make sure you are not passing into the CSV file.


| Name      | Class | Dorm | Room | GPA |
| ----------| ------|------|------|-----|
| Sally Whittaker| 2018 | McCarren House | 312 | 3.75 |
| Belinda Jameson 	|2017 | 	Cushing House| 148 | 3.52 |
| Jeff Smith | 2018 | Prescott House | 17-D | 3.20 |
| Sandy Allen | 2019 |	Oliver House |	108 |	3.48 |

The above data could be represented in a CSV-formatted file as follows:

**students.csv**
```csv
Sally Whittaker,2018,McCarren House,312,3.75
Belinda Jameson,2017,Cushing House,148,3.52
Jeff Smith,2018,Prescott House,17-D,3.20
Sandy Allen,2019,Oliver House,108,3.4
```

Here, the fields of data in each row are delimited with a comma and individual rows are separated by a newline.

The SQL schema for this table can look like this: 

```SQL
CREATE TABLE "students"(
    "name" VARCHAR(255) NOT NULL,
    "class" VARCHAR(255) NOT NULL,
    "dorm" VARCHAR(255) NOT NULL,
    "room" INTEGER NOT NULL,
    "gpa" DOUBLE PRECISION NOT NULL
);
```

Please check that we are not defining the primary key, this process will be done after the seed. 

Now we can use the direct connection to import the CSV file. We will use docker to install Postgres locally and have access to the PSQL CLI:

``` bash
# Pull the image
docker pull postgres:alpine

# Create the container
# We are define a POSTGRES_PASSWORD just because is a requirement for docker image but we will not use it
docker run --name demo-postgres -e POSTGRES_PASSWORD=password -d postgres:alpine 

# Copy the file into the container
docker cp students.csv demo-postgres:students.csv

# Access the container
docker exec -it demo-postgres bash
```

Now you can use the direct connection from Supabase:

1. _Go to settings -> Database -> Connection String_

2. _Choose PSQL, copy and paste the command_

Now you will be prompted for the password of the Supabase project.

You can validate that you are using the remote connection, by checking the tables information:

```
# list all the tables
\d

# seed database with CSV File
\copy students from '/students.csv' DELIMITER ',' CSV;
```

Now you can see the data in Supabase. At this moment we do not have any primary key, we can add a new column through the UI or SQL CLI and specify that is autogenerate it. 







