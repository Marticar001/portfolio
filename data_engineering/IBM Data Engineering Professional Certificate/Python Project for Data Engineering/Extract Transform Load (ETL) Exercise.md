0. Import the required modules and functions
    ```Python
    import glob                         # this module helps in selecting files
    import pandas as pd                 # this module helps in processing CSV files
    import xml.etree.ElementTree as ET  # this module helps in processing XML files
    from datetime import datetime
    ```


1. Download Files
    ```Python
    !wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0221EN-SkillsNetwork/labs/module%206/Lab%20-%20Extract%20Transform%20Load/data/source.zip
    ```
    
2. Unzip Files
    ```Python
    !unzip datasource.zip -d dealership_data
    ```
    
### About the Data
The file `dealership_data` contains CSV, JSON, and XML files for used car data which contain features named `car_model`, `year_of_manufacture`, `price`, and `fuel`.

3. Set Paths
    ```Python
    tmpfile    = "dealership_temp.tmp"             # file used to store all extracted data
    logfile    = "dealership_logfile.txt"          # all event logs will be stored in this file
    targetfile = "dealership_transformed_data.csv" # file where transformed data is stored
    ```

## Extract

4. Question 1: CSV Extract Function
    ```Python
    # Add the CSV extract function below
    def extract_from_csv(file_to_process):
    dataframe = pd.read_csv(file_to_process)
    return dataframe
    ```
    
5. Question 2: JSON Extract Function
    ```Python
    # Add the JSON extract function below
    def extract_from_json(file_to_process):
    dataframe = pd.read_json(file_to_process,lines=True)
    return dataframe
    ```
 
 6. Question 3: XML Extract Function
    ```Python
    # Add the XML extract function below
    def extract_from_xml(file_to_process):
        dataframe = pd.DataFrame(columns=['car_model', 'year_of_manufacture', 'price', 'fuel'])
        tree = ET.parse(file_to_process)
        root = tree.getroot()
        for person in root:
            car_model = person.find("car_model").text
            year_of_manufacture = int(person.find("year_of_manufacture").text)
            price = float(person.find("price").text)
            fuel = person.find("fuel").text
            dataframe = dataframe.append({"car_model":car_model, "year_of_manufacture":year_of_manufacture, "price":price, "fuel":fuel}, ignore_index=True)
        return dataframe
    ```
    
 7. Question 4: Extract Function
    ```Python
    def extract():
        extracted_data = pd.DataFrame(columns=['car_model','year_of_manufacture','price', 'fuel']) # create an empty data frame to hold extracted data

        #process all csv files
        for csvfile in glob.glob("dealership_data/*.csv"):
            extracted_data = extracted_data.append(extract_from_csv(csvfile), ignore_index=True)

        #process all json files
        for jsonfile in glob.glob("dealership_data/*.json"):
            extracted_data = extracted_data.append(extract_from_json(jsonfile), ignore_index=True)            

        #process all xml files
        for xmlfile in glob.glob("dealership_data/*.xml"):
            extracted_data = extracted_data.append(extract_from_xml(xmlfile), ignore_index=True)

        return extracted_data
    ```
    
## Transform

 8. Question 5: Transform
    ```Python
    # Add the transform function below
    def transform(data):
    data['price'] = round(data.price.2)
    return data
    ```
    
## Loading

9. Question 6: Load
  ```Python
  def load(targetfile,data_to_load):
    data_to_load.to_csv(targetfile)
  ```
  
## Logging

10. Question 7: Log
  ```Python
  def log(message):
      timestamp_format = '%Y-%h-%d-%H:%M:%S' # Year-Monthname-Day-Hour-Minute-Second
      now = datetime.now() # get current timestamp
      timestamp = now.strftime(timestamp_format)
      with open("dealership_logfile.txt","a") as f:
          f.write(timestamp + ',' + message + '\n')
  ```
  
## Running ETL Process

11. Question 8: ETL Process
    ```Python
    # Log that you have started the ETL Process
    log("ETL Job Started")
    
    # Log that you have started the Extract step
    log("Extract phase Started")

    # Call the Extract function
    extracted_data = extract()
 
    # Log that you have completed the Extract step
    log("Extract phase Ended")
    
    # Log that you have started the Transform step
    log("Transform phase Started")
 
    # Call the Transform function
    transformed_data = transform(extracted_data)
  
    # Log that you have completed the Transform step
    log("Transform phase Ended")     
    
    # Log that you have started the Load step
    log("Load phase Started")

    # Call the Load Function
    load(targetfile,transformed_data)
      
    # Log that you have completed the Load step
    log("Load phase Ended")    
    
    # Log that you have completed the ETL process
    log("ETL Job Ended")    
    ```     
