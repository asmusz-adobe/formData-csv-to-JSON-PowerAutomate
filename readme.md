## Get Adobe Sign Form-Data as JSON 

## in a "utility" Power-Automate flow

One of the issues we currently have is that the [GET /agreements/{{agreement-ID}}/formData](https://secure.echosign.com/public/docs/restapi/v6#!/agreements/getFormData) call returns the formData from Adobe Sign agreements as a CSV formatted "string".  This is a bit of an issue in P-A because it expects all data exchange to happen as easily parseable JSON.  Below have included a flow that you can pass values to in an HTTP call which will go get the data from Adobe Sign, convert to JSON, and then returns it in that wonderful JSON format.

 I borrowed most of the conversion code from an article by Manuel Gomes I found [here](https://manueltgomes.com/microsoft/powerautomate/how-to-parse-csv-file/). He also was kind enough to zip up his Flow and made it available [here](https://manueltgomes.com/wp-content/uploads/2020/08/ParseCSVfromOneDrive_20200803163228.zip). 

I added an HTTP "request-reciever" trigger at the beginning where you pass the Adobe Sign "agreement ID" and the sender's email address as these are needed for the API call to get the form data.  You'll need to set the "shard", and the apiToken (here I recommend [getting an integration key](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html) for your account), but that's about it.  It will handle any "commas" in your field-data and so far it has tested well. 

Once you've set the shard, and apiToken variables in the Flow itself, you should be able to pass it the "senderEmail", and the "agreementID" like this:

```json
{
    "senderEmail": "echosmusz1+na2main@gmail.com",
    "agreementID": "CBJCHBCAABAAb2FK7fxBRpK836BVtOBaC_PUdDOpE4iR"
}
```

It will get the csv form-data, convert it to JSON and return a nice response something like this:

```json
[{
	"completed": "2020-10-19 14:06:52",
	"email": "echosmusz1+postman1@gmail.com",
	"role": "SIGNER",
	"first": "Sam",
	"last": "Signer",
	"title": "COO",
	"company": "",
	"AccountName": "Skipp, jouhm",
	"AccountNumber": "",
	"City": ";'",
	"Description": "pmkpopoupoiu, kjkjhkjh",
	"State": ";",
	"Street": "']][p][p",
	"Zip": "",
	"agreementId": "CBJCHBCAABAAb2FK7fxBRpK836BVtOBaC_PUdDOpE4iR"
}]
```

The zip for my Flow can be downloaded [here](https://github.com/asmusz-adobe/formData-csv-to-JSON-PowerAutomate/raw/main/GetAdobeSignagreementformdataasJSON_20201020010137.zip).

Download it, set the variables for your Adobe Sign account and give it a try!
