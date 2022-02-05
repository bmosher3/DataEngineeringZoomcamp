Homework Week 1
1. Google Cloud SDK 370.0.0

2. 
(base) ben@pop-os:~/ben/git/data-engineering-zoomcamp/week_1_basics_n_setup/1_terraform_gcp/terraform$ terraform plan
var.project
  Your GDP Project ID

  Enter a value: centered-router-339503      

google_bigquery_dataset.dataset: Refreshing state... [id=projects/centered-router-339503/datasets/trips_data_all]
google_storage_bucket.data-lake-bucket: Refreshing state... [id=dtc_data_lake_centered-router-339503]

Note: Objects have changed outside of Terraform

Terraform detected the following changes made outside of Terraform since the last "terraform apply":

  # google_bigquery_dataset.dataset has changed
  ~ resource "google_bigquery_dataset" "dataset" {
        id                              = "projects/centered-router-339503/datasets/trips_data_all"
      + labels                          = {}
        # (10 unchanged attributes hidden)

        # (4 unchanged blocks hidden)
    }

  # google_storage_bucket.data-lake-bucket has changed
  ~ resource "google_storage_bucket" "data-lake-bucket" {
        id                          = "dtc_data_lake_centered-router-339503"
      + labels                      = {}
        name                        = "dtc_data_lake_centered-router-339503"
        # (9 unchanged attributes hidden)


        # (2 unchanged blocks hidden)
    }


Unless you have made equivalent changes to your configuration, or ignored the relevant attributes using ignore_changes, the following plan may include actions to undo or respond to these
changes.

───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

No changes. Your infrastructure matches the configuration.

Your configuration already matches the changes detected above. If you'd like to update the Terraform state to match, create and apply a refresh-only plan:
  terraform apply -refresh-only
(base) ben@pop-os:~/ben/git/data-engineering-zoomcamp/week_1_basics_n_setup/1_terraform_gcp/terraform$ terraform apply
var.project
  Your GDP Project ID

  Enter a value: centered-router-339503

google_bigquery_dataset.dataset: Refreshing state... [id=projects/centered-router-339503/datasets/trips_data_all]
google_storage_bucket.data-lake-bucket: Refreshing state... [id=dtc_data_lake_centered-router-339503]

Note: Objects have changed outside of Terraform

Terraform detected the following changes made outside of Terraform since the last "terraform apply":

  # google_bigquery_dataset.dataset has changed
  ~ resource "google_bigquery_dataset" "dataset" {
        id                              = "projects/centered-router-339503/datasets/trips_data_all"
      + labels                          = {}
        # (10 unchanged attributes hidden)

        # (4 unchanged blocks hidden)
    }

  # google_storage_bucket.data-lake-bucket has changed
  ~ resource "google_storage_bucket" "data-lake-bucket" {
        id                          = "dtc_data_lake_centered-router-339503"
      + labels                      = {}
        name                        = "dtc_data_lake_centered-router-339503"
        # (9 unchanged attributes hidden)


        # (2 unchanged blocks hidden)
    }


Unless you have made equivalent changes to your configuration, or ignored the relevant attributes using ignore_changes, the following plan may include actions to undo or respond to these
changes.

───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

No changes. Your infrastructure matches the configuration.

Your configuration already matches the changes detected above. If you'd like to update the Terraform state to match, create and apply a refresh-only plan:
  terraform apply -refresh-only

Apply complete! Resources: 0 added, 0 changed, 0 destroyed.

3. SELECT count(*) from yellow_taxi_trips WHERE tpep_pickup_datetime::date = '2021-01-15'
    
    53024

4. SELECT tpep_pickup_datetime::date, MAX(tip_amount) AS max_tip
   FROM yellow_taxi_trips
   GROUP BY tpep_pickup_datetime::date
   ORDER BY max_tip desc
    
    2021-01-20

5. SELECT doz."Zone", count(*) AS ct
   FROM yellow_taxi_trips trips
   LEFT JOIN yellow_taxi_zones puz ON puz."LocationID" = trips."PULocationID"
   LEFT JOIN yellow_taxi_zones doz ON doz."LocationID" = trips."DOLocationID"
   WHERE puz."Zone" = 'Central Park'
   GROUP BY doz."Zone"
   ORDER BY ct desc;
    
    Upper East Side North

6. SELECT CONCAT(puz."Zone",'  / ' , doz."Zone"), avg(trips.total_amount) AS avg_trip, count(*) 
   FROM yellow_taxi_trips trips
   LEFT JOIN yellow_taxi_zones puz ON puz."LocationID" = trips."PULocationID"
   LEFT JOIN yellow_taxi_zones doz ON doz."LocationID" = trips."DOLocationID"
   GROUP BY puz."Zone", doz."Zone"
   ORDER BY avg_trip desc;
    
    Alphabet City  / Unknown