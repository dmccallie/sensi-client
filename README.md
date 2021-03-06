# sensi-client
sensi-client is an unofficial Node.js module for interacting with the Sensi thermostat API.
Note: sensi-client is still under active development and should be considered alpha-quality.

# Requirements
`request` for Sensi API connectivity

`tough-cookie` for http cookie handling

# Example
    var SensiClient = require("./sensi-client.js");
    
    var options = {
        username: process.env.SENSI_USERNAME,
        password: process.env.SENSI_PASSWORD,
        pollingRetryCount: 5
    };
    

    client = new SensiClient(options);

    client.on("online", function(data) {
        console.log("Thermostat is online");
        console.dir(data);
    });
    
    client.on("update", function(data) {
        console.log("Thermostat update received");
        console.dir(data);
    });
    
    client.on("systemModeChanged", function(modeChange) {
        console.log("System mode has changed from " +
            modeChange.oldMode +
            " to " +
            modeChange.newMode);
    });
    
    client.on("coolSetpointChanged", function(setpointChange) {
        console.log("Cool setpoint has changed from " +
            setpointChange.oldSetpoint +
            " to " +
            setpointChange.newSetpoint);
    });
    
    client.connect(function(err) {
        if (err) {
            console.error(err);
            process.exit(1);
        }
      
        if (client._thermostats) {
            client.subscribe(client._thermostats[0].ICD, function(err) {
                if (err) {
                    console.error(err);  
                    process.exit(2);
                }
            });
        }
    });
    
# Todo
- Add support for changing temperature, schedule, etc.
