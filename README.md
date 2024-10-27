# palo-ngfw-azure-arm-template
An Azure ARM template to deploy Cloud NGFWs by Palo Alto Networks resource.

Blog Post: https://www.jcdoes.com/index.php/programming/cloud/palo-alto-ngfw-in-azure


The script is formatted to use Panorama, but look below on some other setting settings you can use. There isn't a reference for all the options you can apply to the Palo resource. A lot of things I had to discover via having to have Azure create the script and seeing the values.

### Using

Simply navigate to 'Deploy from a custom template' > 'Build your own template in the editor' and copy/paste either of the JSON templates.

### Circular dependency solved!

I found that if you wanted to update a subnet resource while referencing a existing value, it would throw the "circular dependency error". In order to lookup an existing value, you have to use a Powershell script. Look in the panorama-template.json file in the "Microsoft.Resources/deploymentScripts" resource. This will run the PS commands and populate a variable you can reference in another resource.

### Other settings

Here are some 'hidden' values you can use for the Palo resource.

**Ingress Rule:**

    "frontEndSettings": [
        {
        "name": "inbound-443",
        "protocol": "TCP",
        "frontendConfiguration": {
        "port": "443",
        "address": {
            "resourceId": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('public_ip_address_name'))]"
        }
        },
        "backendConfiguration": {
            "port": "443",
            "address": {
                "address": "10.0.0.5"
                }
            }
        }
    ]

