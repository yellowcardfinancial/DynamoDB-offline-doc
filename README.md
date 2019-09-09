## Run Dynamodb Offline
**documentation to run dynamodb offline** 
this is a documentation on how to run dynamodb offline

**quick overview** DynamoDB is a fully-managed NoSQL database service designed to deliver fast and predictable performance.

## Getting started

**setup serverless if not already installed** 

    npm install -g serverless

**setup serverless offline**

    npm i  serverless-offline

**configure your aws if not already configured** 
doing this you will have to download the AWS CLI for [windows](https://docs.aws.amazon.com/cli/latest/userguide/install-windows.html), [macOS](https://docs.aws.amazon.com/cli/latest/userguide/install-macos.html), or [Linux](https://docs.aws.amazon.com/cli/latest/userguide/install-linux.html)

    aws configure

**install serverless-dynamodb-local**

       npm i serverless-dynamodb-local 


**install dynamodb offline**
this installs dynamodb in your project folder
 

       sls dynamodb install

 


**start dynamodb** 

    sls dynamodb start

 **dynamodb start options**
 you can add this as you start your dynamodb instance
|*options*| *description*  | *Command*  | 
|--|--|--|
|--port  | used to set port where dynamodb will run default is 8000  | `sls dynamodb start --port 3000` 
|--seed  | this determines which data to onload  | `sls dynamodb start --seed`
|--migrate | creates dynamodb offline tables from serverless configs   | `sls dynamodb start --seed`

**update serverless.yml file**

    plugins:
     -  serverless-dynamodb-local
     -  serverless-offline


**seeding with dynamodb**
this is used to onload data in dynamodb .this is found in `project-folder/resourses/seeds`. here's a case scenario i *make changes to my dynamodb seed for instance users-seed.json. to update dynamodb running instance with this data i run this command*. **P.S**  considering you have all your `serverless.yml` settings done 

    sls dynamodb seed

**view table data in dynamodb** 
open up `http://localhost:{your running port here}/shell` 
and execute the below script. Please change the table name as per your requirement.

```
var dynamodb = new AWS.DynamoDB({
region: 'us-east-1',
endpoint: "http://localhost:8000"
});
var tableName = "TESTTABLE";

var params = {
TableName: tableName,
Select: "ALL_ATTRIBUTES"
};


function doScan(response) {
if (response.error) ppJson(response.error); // an error occurred
else {
    ppJson(response.data); // successful response

    // More data.  Keep calling scan.
    if ('LastEvaluatedKey' in response.data) {
        response.request.params.ExclusiveStartKey = response.data.LastEvaluatedKey;
        dynamodb.scan(response.request.params)
            .on('complete', doScan)
            .send();
    }
}
}
console.log("Starting a Scan of the table");
dynamodb.scan(params)
.on('complete', doScan)
.send();
```
**uninstall dynamodb**
this is useful in debugging when dynamodb fails

    sls dynamodb remove

## BUGS 😭😭😭
still having bugs? no issue go to 
[we hate bugs](https://github.com/yellowcardfinancial/we-hate-bugs) locate your bugs in the list with solutions 
if bug is `404` in list then ask anyone for help









