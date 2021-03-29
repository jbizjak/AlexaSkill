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
This gets the entry from the database with the most recent time, and the devicestate represents the request from the user, if it is 0 the most recent request was to turn off the light, if it is 1 the most recent request was to turn it on.

With this setup this skill can be used to turn one light on or off with Alexa.
