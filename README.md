# AWS: Real-Time-CDC-ETL-Pipeline-for-Sales-Data
This project implements a Change Data Capture pipeline on AWS to process and analyze real-time sales data. The pipeline captures changes in a DynamoDB table, streams the data through Kinesis, performs transformations using Lambda, and stores the processed data in S3. The data is cataloged using AWS Glue and made queryable in Athena for analysis.

## Data-Flow Diagram

Below is an architecture diagram of the pipeline, showing the flow from data generation to querying in Athena.

![Data-Flow Diagram](DataArchitecture.png)

## Prerequisites

Before setting up the pipeline, ensure you have the following:

- **AWS Account**: Permissions to access DynamoDB, Kinesis, Lambda, S3, Glue, and Athena services.
- **AWS CLI**: Installed and configured with credentials that have the necessary permissions.
- **Python 3.10/11/12**: For generating mock sales data and managing dependencies.
- **Boto3**: AWS SDK for Python, which allows Python code to interact with AWS services.

  ## Steps Involved

1. **Generate Mock Data**: Begin by creating a Python script to generate and stream mock sales order data into DynamoDB using the `boto3` library.

2. **DynamoDB Setup**: Create a DynamoDB table and enable data streams for CDC capture.

3. **Target Configuration**: Set up a Kinesis Data Stream as the target destination for the data from DynamoDB. Ensure appropriate IAM permissions are attached to read data.

4. **Event Bridge Configuration**: Create an EventBridge rule to flow data from DynamoDB to the Kinesis Data Stream. Configure the source as DynamoDB and the target as the Kinesis Data Stream created earlier.

5. **IAM Permissions**: Attach IAM policies to allow reading and writing data from DynamoDB and the Kinesis Data Stream.

6. **Start EventBridge Pipe**: Begin streaming data from DynamoDB to the Kinesis Data Stream by starting the EventBridge pipe.

7. **Kinesis Firehose Configuration**: Set up Kinesis Firehose to batch the near real-time data streaming from the Kinesis Data Stream. Configure the source as the Kinesis Data Stream and the transformation layer as an AWS Lambda function. The target should be an S3 bucket.

8. **Lambda Transformation**: Attach the Lambda function as the transformation layer to process the data before storing it in S3. This function performs necessary transformations on the data.

9. **Data Storage**: Kinesis Firehose batches the transformed data and stores it in the S3 bucket in Hive partitioning style.

10. **AWS Glue Crawler**: Create an AWS Glue Crawler to read metadata from the S3 bucket and create a Glue catalog.

11. **Glue Catalog Configuration**: Configure the Glue catalog in AWS Athena to query the data stored in the S3 bucket. Perform basic analysis on the processed data.

12. **Visualization**: Utilize Tableau or QuickSight for visualization of the analyzed data to derive meaningful insights and KPI metrics.


  ## Usage

Follow these steps to run the ETL pipeline and verify that each component is working as expected.

- **Run the Data Generator Script**  
   Use the [`data_generator.py`](data_generator.py) script to populate the DynamoDB table with sample sales data. This triggers the pipeline to start processing data:

   ```bash
   python data_generator.py
---

## Project Structure

Below is an overview of the project structure and key files:

```plaintext
Real-Time-CDC-ETL-Pipeline-for-Sales-Data/
│
├── data_generator.py       # Script for generating mock sales data and loading it into DynamoDB
├── lambda_function.py      # Lambda function for transforming Kinesis data before storage in S3
├── README.md               # Project documentation with setup and usage instructions
└── architecture.png        # Architecture diagram depicting the ETL pipeline workflow
```

## Future Improvements
**Real-Time Business Insights with Dashboarding Tools**  
   Integrate a dashboarding tool like AWS QuickSight, Tableau, or Power BI to visualize real-time business insights from the processed data. This would allow stakeholders to access up-to-date metrics and key business insights through interactive dashboards, enabling data-driven decision-making.


