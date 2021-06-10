Calculate how much money is worth in a different country. Uses data from World
Bank.

Note: Works on python3.4 and above.

### Installation Instructions
1. Clone the source code

        git clone https://github.com/dransome/pppconverter.git

2. Create a virtual environments and install the dependencies

        virtualenv -p python3 env
        source env/bin/activate
        pip install -r requirements.txt
        
**OR**

If deploying on Jelastic, just edit the server's `wsgi.conf` to point `WSGIDaemonProcess` `home="/var/www/webroot/ROOT"` and Jelastic will take care of the rest.
        

3. Create the sqlite database by running the website.py file.

        ./manage.py db_init

4. Import the CSV into the sqlite database.

        ./manage.py importcsv -f data.csv

5. Import countries CSV into the sqlite database

        ./manage.py importcountries -f countries.csv
        
6. Define Open Exchange Rates API key in `instance/local.py`

        OPEN_EXCHANGE = 'write the APP ID here'
        
7. Update FX rates       

        ./manage.py update_conversion_rate


### Updating the data
1. Download the CSV data from the [world bank portal][wb] and unzip the file.

2. Review the CSV file to check which year has data you need for the countries you're interested in

3. Edit manage.py `list_of_years` value to the year you want to extract

4. Delete the preamble metadata rows from the top of the CSV (data source, last updated date) to leave the country name and years headers as the top row of the CSV

5. Run the parsecsv.py script to create a file called parsed\_data.csv.

        ./manage.py parsecsv -f /path/to/file

6. Replace data.csv file with the newly created parsed\_data.csv file.

7. Import the new CSV into the sqlite database.

        ./manage.py importcsv -f data.csv

**If this step fails due to UNIQUE constraint failed**

You may need to rename the sqlite db file and repeat the setup steps using your new data file.


[wb]: http://data.worldbank.org/indicator/PA.NUS.PPP
