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
