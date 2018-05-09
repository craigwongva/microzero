3/10/18:

`microqero` has the important Docker info in its solitary Issue.

`microqoder` has the interesting capture-sha-in-code and write to CloudWatch Logs.

3/24/18:
greendots-golang:

  ./bin/rquery <judy access token>
    This creates Polly mp3 files and stores them in ~environment/greendots-golang/static/img/.

3/25/18:
editor-mysql:
  Be sure to use the same dbpassword in the next two CF commands:

What's `cf.yml`?
```
aws cloudformation create-stack --stack-name editordb --template-body file://cf.yml \
    --region us-west-2 --parameter ParameterKey=dbusername,ParameterValue=editoruser \       
    ParameterKey=dbpassword,ParameterValue=REDACTED
```

```
aws cloudformation create-stack --stack-name editor-EC2 --template-body file://df.yml \
    --region us-west-2 --capabilities CAPABILITY_NAMED_IAM \
    --parameter ParameterKey=IP22,ParameterValue=`curl 169.254.169.254/latest/meta-data/public-ipv4` \      
    ParameterKey=dbendpoint,ParameterValue=`aws cloudformation describe-stacks --stack-name editordb --region us-west-2 | jq '.Stacks[].Outputs[0].OutputValue' | sed s/\"//g` \
    ParameterKey=IP8081,ParameterValue=REDACTED ParameterKey=dbpassword,ParameterValue=REDACTED
```

  Inside mysql's interpreter if you're debugging or about to create a new EC2 stack:
```
use innodb;
drop database editordb;
```

  To run manually:

```
cd ~/editor-mysql/scotSpryMedia
node index.js
```

```
http://yacback.redf4rth.net:8081/inline-editing/serverside.html
http://yacback.redf4rth.net:8081/inline-editing/mytab.html

http://yacback.redf4rth.net:8081/inline-editing/student.html
```

```
# Starting the db takes about six minutes.
aws rds start-db-instance     --db-instance-identifier editordb

aws rds describe-db-instances --db-instance-identifier editordb | \
    jq '.DBInstances[0].DBInstanceStatus' | sed s/\"//g

mysql editordb \
    --host=`aws rds describe-db-instances --db-instance-identifier editordb | \
    jq '.DBInstances[0].Endpoint.Address' | sed -e "s/\"//g"` \
    --user=editoruser --password=REDACTED
```
