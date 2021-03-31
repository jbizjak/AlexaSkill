# Device Setup
To set up a device compatible with this skill, a rasberry pi or other internet connected gadget needs to run, continuously reading from an RDS database using an API request like this (Node.js):
```
   let sqlParams = {
        secretArn: 'arn:aws:secretsmanager:us-east-1:613705143570:secret:baseSecret-5EYo7Q',
        resourceArn: 'arn:aws:rds:us-east-1:613705143570:cluster:database-1',
        sql: "SELECT * FROM LightTable WHERE(CurrentTime = (SELECT Max(CurrentTime) FROM LightTable  ) )"
        database: 'testbase',
        includeResultMetadata: true
     }
   rdsData.executeStatement(sqlParams, function (err, data) {
        if (err) {
          // error
          console.log(err)

       } else {
         //turn on/off light on device

      }
      })
```

The sql query will read the most recent entry in the RDS database. The devicestate field of this entry will represent the most recent request sent from Alexa to the skill. A 1 will indicate the most recent request was to turn the light on, the devicestate being 0 will indicate that the most recent request was to turn the device off. After reading from the database and finding the devicestate, the raspberry pi should handle the actual turning on or off of the light. This is dependent on the raspberry pi and the light connected to it. One example tutorial for setting up a raspberry pi to control a light can be found here: https://www.instructables.com/Raspberry-Pi-Home-Automation-Control-lights-comput/ .

With this setup this skill can be used to turn one light on or off with Alexa and a raspberry pi.
