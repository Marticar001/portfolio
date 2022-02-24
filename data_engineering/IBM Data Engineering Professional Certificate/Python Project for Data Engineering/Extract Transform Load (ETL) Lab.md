[Source](https://labs.cognitiveclass.ai/tools/jupyterlab/lab/tree/labs/PY0221EN/ExtractTransformLoad_V2.ipynb?lti=true)

1. Import the required modules and functions
    ```Python
    import glob                         # this module helps in selecting files
    import pandas as pd                 # this module helps in processing CSV files
    import xml.etree.ElementTree as ET  # this module helps in processing XML files
    from datetime import datetime
    ```
    
2. Download Files
    ```Python
    !wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0221EN-SkillsNetwork/labs/module%206/Lab%20-%20Extract%20Transform%20Load/data/source.zip
    ```
    
3. Unzip Files
    ```Python
    !unzip source.zip
    ```
    
4. Set Paths
    ```Python
    tmpfile     = "temp.tmp"                # file used to store all extracted data
    logfile     = "logfile.txt"             # all event logs will be stored in this file
    targetfile  = "transformed_data.csv"    # file where transformed data is stored
    ```
    
5. CSV Extract Function
    ```Python
    def extract_from_csv(file_to_process):
        dataframe = pd.read_csv(file_to_process)
        return dataframe
    ```
    
6. JSON Extract Function
    ```Python
    def extract_from_json(file_to_process):
        dataframe = pd.read_json(file_to_process,lines=True)
        return dataframe
    ```    

7. XML Extract Function
    ```Python
    def extract_from_xml(file_to_process):
        dataframe = pd.DataFrame(columns=["name", "height", "weight"])
        tree = ET.parse(file_to_process)
        root = tree.getroot()
        for person in root:
            name = person.find("name").text
            height = float(person.find("height").text)
            weight = float(person.find("weight").text)
            dataframe = dataframe.append({"name":name, "height":height, "weight":weight}, ignore_index=True)
        return dataframe
    ``` 
    
8. Extract Function
    ```Python
    def extract():
        extracted_data = pd.DataFrame(columns=['name','height','weight']) # create an empty data frame to hold extracted data
        
        #process all csv files
        for csvfile in glob.glob("*.csv"):
            extracted_data = extracted_data.append(extract_from_csv(csvfile), ignore_index=True)
            
        #process all json files
        for jsonfile in glob.glob("*.json"):
            extracted_data = extracted_data.append(extract_from_json(jsonfile), ignore_index=True)            

        #process all xml files
        for xmlfile in glob.glob("*.xml"):
            extracted_data = extracted_data.append(extract_from_xml(xmlfile), ignore_index=True)
            
        return extracted_data
    ```

9. Transform
    ```Python
    def transform(data):
            #Convert height which is in inches to millimeter
            #Convert the datatype of the column into float
            #data.height = data.height.astype(float)
            #Convert inches to meters and round off to two decimals(one inch is 0.0254 meters)
            data['height'] = round(data.height * 0.0254,2)
            
            #Convert weight which is in pounds to kilograms
            #Convert the datatype of the column into float
            #data.weight = data.weight.astype(float)
            #Convert pounds to kilograms and round off to two decimals(one pound is 0.45359237 kilograms)
            data['weight'] = round(data.weight * 0.45359237,2)
            return data
    ```
    
10. Loading
    ```Python
    def load(targetfile,data_to_load):
        data_to_load.to_csv(targetfile)
    ```
    
11. Logging
    ```Python
    def log(message):
        timestamp_format = '%Y-%h-%d-%H:%M:%S' # Year-Monthname-Day-Hour-Minute-Second
        now = datetime.now() # get current timestamp
        timestamp = now.strftime(timestamp_format)
        with open("logfile.txt","a") as f:
            f.write(timestamp + ',' + message + '\n')
    ```    
