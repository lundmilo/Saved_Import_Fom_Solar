# Saved_Import_Fom_Solar
Custom sensors to display how much is saved from using solar

To make this work there are a few prerequisites that has to be met:

  1. Make sure that your solar system is correctly integrates into Home Assistant, and that you have entities showing:

          Produced W
          Imported W
          Exported W

  2. You need to have en entity importing the current electricity price . I am using https://github.com/MTrab/energidataservice/tree/master
  
  3. You need to copy the custom sensor "Saved Import" from the sensor.yaml file into your own sensor.yaml og configuration.yaml. You need to edit the sensor.xxx names to match the names of your own sensors and restart Hoe Assistant


After restart you should be able to find a sensor called sensor.saved_import under the developer tab. This value shows you the current power (W) that is being used from Solar instead of imported. This will always be a positive value.

To convert this value to energy (kWh), you have to create a helper:
        Go to Settings > Devices & Services.
        Select Helpers.
        Select Create Helper
        Select  Integration - Riemann sum integral
          Name you helper
          Under input sensor you have to select the sensor.saved_import
          Under Integration method select "Left"
          Under Metric prefix select "k (kilo)
          Press send

          You will now have a new sensor called sensor.saved_import_2

Next step is to create a ultitity meter:
        Go to Settings > Devices & Services.
        Select Helpers.
        Select Create Helper
        Select Utility meter
          Name your Utility meter
          Under Input sensor you have to select the sensor.saved_import_2
          Under Meter reset cycle you have to select "None"
          Press send

          You will now have a sensor called sensor.saved_import_3

You should now be able to go add the new Utlility meter to your Energy Dashboard
        Go to Settings > Dashboard
        Go to Energy
        Add a new Gas Source or Water Consumption (I used Gas, since I do not use gas in my household)
        Under Gas usage select sensor.saved_import_3
        Select "Use an entity with current price"
        Select the entity providing the current price
        Press save

        You should now be able to see how much anergy is saved in the Energy Dashboard. It might take some time for it to start showing data.
        You will also have en new entity called sensor.saved_import_3_cost

If you want to be able to show the total saved import on another dashboard you have to create a new Utility Meter:

        Go to Settings > Devices & Services.
        Select Helpers.
        Select Create Helper
        Select Utility meter
          Name your Utility meter
          Under Input sensor you have to select the sensor.saved_import_3_cost
          Under Meter reset cycle you have to select "None"
          Press send

          You will now have a sensor called sensor.name_of_the_utility_meter which will show the total amount of saved money

