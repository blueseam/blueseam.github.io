---
layout: post
title:  "AWS Lambda Example Source"
categories: [ Jekyll, tutorial ]
image: assets/images/3.jpg
---

## S3 Query

```
import json
import boto3
import logging

logger = logging.getLogger()
logger.setLevel(logging.INFO)

def lambda_handler(event, context):
    imsi = ""

    s3 = boto3.client('s3')

    getYW = "202126"
    getLocType = "sido"
    getFilename = "part-00000-0c157631-538b-470a-b7b9-04b662d9fb18-c000.snappy.parquet"
    getFileType = "Parquet"
    
    iloccd = ""
    isexcd = ""
    iagecd = ""

    try:
        iloccd = event["queryStringParameters"]["loccd"]
        logger.info("Location Code: %s", iloccd)
    except KeyError:
        logger.info("Location Code 입력값이 없습니다.")

    try:
        isexcd = event["queryStringParameters"]["sexcd"]
        logger.info("Sec Code: %s", isexcd)
    except KeyError:
        logger.info("Sex Code 입력값이 없습니다.")

    try:
        iagecd = event["queryStringParameters"]["agecd"]
        logger.info("Age Code: %s", iagecd)
    except KeyError:
        logger.info("Age Code입력값이 없습니다.")


    # if event["queryStringParameters"]["yw"] == None:
    #     getYW = event["queryStringParameters"]["yw"]

    query = " "
    query = query + " SELECT * "
    query = query + " FROM s3object s "
    query = query + " WHERE 1=1 "
    
    if iloccd != "":
        query = query + " and home_loc_cd='" + iloccd + "' "

    if isexcd != "":
        query = query + " and sex_cd='" + isexcd + "' "

    if iagecd != "":
        query = query + " and age_cd='" + iagecd + "' "
    
    logger.info(">>> Query : " + query)

    resp = s3.select_object_content(
        Bucket='skapi-dev-kepler',
        #Key='sty_ptrn_w_idx/',
        # Key='sty_ptrn_w_idx/yw=202126/loc_type=sido/part-00000-0c157631-538b-470a-b7b9-04b662d9fb18-c000.snappy.parquet',
        Key='sty_ptrn_w_idx/yw=' + getYW + '/loc_type=' + getLocType + '/'+ getFilename,
        ExpressionType='SQL',
        # Expression="SELECT * FROM s3object s LIMIT 10",
        Expression=query,
        # Expression="SELECT home_loc_nm FROM s3object s WHERE s.age_cd='10' and s.home_loc_cd='11'",
        InputSerialization = {getFileType: {}},
        #InputSerialization = {'CSV': {"FileHeaderInfo": "Use"}, 'CompressionType': 'NONE'},
        # OutputSerialization = {'JSON': {'RecordDelimiter': ','}}
        OutputSerialization = {'CSV': {}}
    )
    
    # dataResults = []
    strResult = ""

    for event in resp['Payload']:
        if 'Records' in event:
            records = event['Records']['Payload'].decode('utf-8')
            #json_acceptable_string = records.replace("\"", "\'")
            #json_rec = json.loads(records + "\n")
            # dataResults.append(records)
            strResult = strResult + records
            
            print(records)
        elif 'Stats' in event:
            statsDetails = event['Stats']['Details']
            print("Stats details bytesScanned: ")
            print(statsDetails['BytesScanned'])
            print("Stats details bytesProcessed: ")
            print(statsDetails['BytesProcessed'])
            print("Stats details bytesReturned: ")
            print(statsDetails['BytesReturned'])

    # records = json.dumps(records)
    # records = records.replace("\\n{",",{")
    # records = json.loads(records)
    # dataResults = records
    # logger.info(">>>>>>>>>>>>>>" + dataResults)

    #str1 = ''.join(records)
    #records.replace("\"", "\'")
    
    # str1 = ''.join(records)
    # ijson = str1.replace("\n{",",{")
    #s1 = json.loads(ijson)
    #print(ijson['home_loc_nm'])
    
    

    # imsi['body'] = json.loads(records)
    # imsi['body'] = strResult
    # imsi['body'] = 'Welcome to World'
    #imsi.append(ijson)
    # logger.info(json.dumps(imsi))
    
    
    return strResult
```

## Athena Query

```
import time
import boto3
import re
import logging
import json

logger = logging.getLogger()
logger.setLevel(logging.INFO)


def lambda_handler(event, context):
    imsi = {
     	"statusCode": 200,
		"statusDescription": "200 OK",
		"headers": {
			"Content-Type": "text/json; charset=utf-8"
	    }
    }
    client = boto3.client('athena')
    s3 = boto3.client('s3')

    iyw = ""
    iloctype = ""
    iloccd = ""
    isexcd = ""
    iagecd = ""
    
    DATABASE = "dev_kepler"
    TABLE = "sty_ptrn_w_idx"
    S3_OUTPUT='s3://skapi-dev-query-results/sido/'
    
    GROUP_COLUMNS = "home_loc_cd, home_loc_nm"
    
    RESULT_COLUMNS = "home_loc_cd, home_loc_nm"
    if isexcd == "":
        RESULT_COLUMNS = RESULT_COLUMNS + ", sex_cd"
        GROUP_COLUMNS = GROUP_COLUMNS + ", sex_cd"
    if iagecd == "":
        RESULT_COLUMNS = RESULT_COLUMNS + ", age_cd"
        GROUP_COLUMNS = GROUP_COLUMNS + ", age_cd"
    RESULT_COLUMNS = RESULT_COLUMNS + \
    ", avg(h0d0h0_dur_r) as h0d0h0, avg(h0d0h1_dur_r) as h0d0h1, avg(h0d1h0_dur_r) as h0d1h0, avg(h0d1h1_dur_r) as h0d1h1" + \
    ", avg(h1d0h0_dur_r) as h1d0h0, avg(h1d0h1_dur_r) as h1d0h1, avg(h1d1h0_dur_r) as h1d1h0, avg(h1d1h1_dur_r) as h1d1h1"
    
    try:
        iyw = event["yw"]
        logger.info("YearWeekly: %s", iyw)
    except KeyError:
        logger.info("Year Weekly 입력값이 없습니다.")
        iyw = "202126"

    try:
        iloctype = event["loctype"]
        logger.info("Location Type: %s", iloctype)
    except KeyError:
        logger.info("Location Type 입력값이 없습니다.")
        iloctype = "sido"

    try:
        iloccd = event["loccd"]
        logger.info("Location Code: %s", iloccd)
    except KeyError:
        logger.info("Location Code 입력값이 없습니다.")

    try:
        isexcd = event["sexcd"]
        logger.info("Sec Code: %s", isexcd)
    except KeyError:
        logger.info("Sex Code 입력값이 없습니다.")

    try:
        iagecd = event["agecd"]
        logger.info("Age Code: %s", iagecd)
    except KeyError:
        logger.info("Age Code입력값이 없습니다.")

    query = " SELECT %s" % (RESULT_COLUMNS)
    query = query + " FROM %s.%s " % (DATABASE, TABLE)
    query = query + " WHERE yw = '" + iyw + "' and loc_type = '" + iloctype + "' "
    
    if iloccd != "":
        query = query + " and home_loc_cd='" + iloccd + "' "

    if isexcd != "":
        query = query + " and sex_cd='" + isexcd + "' "

    if iagecd != "":
        query = query + " and age_cd='" + iagecd + "' "
    
    query = query + " GROUP BY %s;" % (GROUP_COLUMNS)
    logger.info(">>> Query : " + query)

    # Execution
    response = client.start_query_execution(
        QueryString=query,
        QueryExecutionContext={
            'Database': DATABASE
        },
        ResultConfiguration={
            'OutputLocation': S3_OUTPUT,
        }
    )
	
    query_execution_id = response['QueryExecutionId']
    logger.info("QueryExecutionID: " + query_execution_id)

    max_execution = 30
    query_execution_status = 'RUNNING'
    for i in range (1, 1 + max_execution):
        max_execution = max_execution - 1
        query_status = client.get_query_execution(QueryExecutionId=query_execution_id)
        query_execution_status = query_status['QueryExecution']['Status']['State']
        logger.info("STATUS:" + query_execution_status)
        
        if query_execution_status == 'SUCCEEDED':
            logger.info("STATUS: SUCCEEDED at %i", i)
            
            s3_path = query_status['QueryExecution']['ResultConfiguration']['OutputLocation']
            filename = re.findall('.*\/(.*)', s3_path)[0]
            logger.info("athena_to_s3 response : %s", filename)
            result = client.get_query_results(QueryExecutionId = query_execution_id)
            break
    else:
        client.stop_query_execution(QueryExecutionId=query_execution_id)
        raise Exception('TIME OVER')

    dataResults = []
    column_names = None

    for row in result['ResultSet']['Rows']:
        column_values = [col.get('VarCharValue', None) for col in row['Data']]
        if not column_names:
            column_names = column_values
        else:
            addResult = dict(zip(column_names, column_values))
            dataResults.append(addResult)

    response = result['ResultSet']['Rows']
    
    imsi['body'] = json.dumps(dataResults, ensure_ascii=False)
    return imsi
```

## Athena + S3 Query

```
import time
import boto3
import re
import logging
import json

logger = logging.getLogger()
logger.setLevel(logging.INFO)

# query = 'SELECT * FROM dev_kepler.sido'
# DATABASE = 'dev_kepler'
#output='s3://skapi-dev-query-results/sido/'

def lambda_handler(event, context):
    imsi = {
     	"statusCode": 200,
		"statusDescription": "200 OK",
		"headers": {
			"Content-Type": "text/json; charset=utf-8"
	    }
    }
    client = boto3.client('athena')
    s3 = boto3.client('s3')

    iyw = "202126"
    iloctype = "sido"

    iloccd = ""
    isexcd = ""
    iagecd = ""

    try:
        iyw = event["queryStringParameters"]["yw"]
        logger.info("YearWeekly: %s", iyw)
    except KeyError:
        logger.info("Year Weekly 입력값이 없습니다.")

    try:
        iloctype = event["queryStringParameters"]["loctype"]
        logger.info("Location Type: %s", iloctype)
    except KeyError:
        logger.info("Location Type 입력값이 없습니다.")

    try:
        iloccd = event["queryStringParameters"]["loccd"]
        logger.info("Location Code: %s", iloccd)
    except KeyError:
        logger.info("Location Code 입력값이 없습니다.")

    try:
        isexcd = event["queryStringParameters"]["sexcd"]
        logger.info("Sec Code: %s", isexcd)
    except KeyError:
        logger.info("Sex Code 입력값이 없습니다.")

    try:
        iagecd = event["queryStringParameters"]["agecd"]
        logger.info("Age Code: %s", iagecd)
    except KeyError:
        logger.info("Age Code입력값이 없습니다.")


    # query = "SELECT * FROM dev_kepler.sido limit 1"
    # query = "SELECT * FROM dev_kepler.sty_ptrn_w_idx limit 1"
    # query = " SELECT a.sex_cd, b.wkt "
    # query = " SELECT a.sex_cd "
    # query = query + " FROM dev_kepler.sty_ptrn_w_idx as a, dev_kepler.sido as b "
    # query = query + " WHERE a.yw = '202126' and a.loc_type='sido' and a.age_cd='10' and a.home_loc_cd='11' and a.home_loc_cd=b.area_cd "
    # query = query + " limit 1 "
    
    query = " SELECT * "
    query = query + " FROM dev_kepler.sty_ptrn_w_idx as a "
    query = query + " WHERE a.yw = '" + iyw + "' and a.loc_type = '" + iloctype + "' "
    
    if iloccd != "":
        query = query + " and home_loc_cd='" + iloccd + "' "

    if isexcd != "":
        query = query + " and sex_cd='" + isexcd + "' "

    if iagecd != "":
        query = query + " and age_cd='" + iagecd + "' "
    
    logger.info(">>> Query : " + query)

    DATABASE = 'dev_kepler'
    output='s3://skapi-dev-query-results/sido/'

    # Execution
    response = client.start_query_execution(
        QueryString=query,
        QueryExecutionContext={
            'Database': DATABASE
        },
        ResultConfiguration={
            'OutputLocation': output,
        }
    )
	
    query_execution_id = response['QueryExecutionId']
    #time.sleep(30)

    max_execution = 30
    query_execution_status = 'RUNNING'
    while max_execution > 0 and query_execution_status in ['RUNNING', 'QUEUED']:
        max_execution = max_execution - 1
        query_status = client.get_query_execution(QueryExecutionId=query_execution_id)
        query_execution_status = query_status['QueryExecution']['Status']['State']
        logger.info("STATUS:" + query_execution_status)
        
        if query_execution_status == 'SUCCEEDED':
            logger.info("STATUS: %s", max_execution)
            
            s3_path = query_status['QueryExecution']['ResultConfiguration']['OutputLocation']
            filename = re.findall('.*\/(.*)', s3_path)[0]
            logger.info("athena_to_s3 response : %s", filename)
            result = client.get_query_results(QueryExecutionId = query_execution_id)

        time.sleep(1)


    #result = client.get_query_results(QueryExecutionId = query_execution_id)
    # dataResults = []
    # dataResults = {}
    # column_names = None

    # for row in result['ResultSet']['Rows']:
    #     column_values = [col.get('VarCharValue', None) for col in row['Data']]
    #     if not column_names:
    #         column_names = column_values
    #     else:
    #         addResult = dict(zip(column_names, column_values))

    #         dataResults.append(addResult)


    # response = result['ResultSet']['Rows']
    # response = result['ResultSet']['Rows']['Data'][1]


    resp = s3.select_object_content(
        Bucket='skapi-dev-query-results',
        Key='sido/' + filename ,
        ExpressionType='SQL',
        Expression="SELECT * FROM s3object s",
        InputSerialization = {'CSV': {"FileHeaderInfo": "Use"}, 'CompressionType': 'NONE'},
        # OutputSerialization = {'JSON': {'RecordDelimiter': ','}}
        OutputSerialization = {'CSV': {}}
    )
    
    strResult = ""

    for event in resp['Payload']:
        if 'Records' in event:
            records = event['Records']['Payload'].decode('utf-8')
            # iresults = records.replace("\\n{",",{")
            strResult = strResult + records
            print(records)
        elif 'Stats' in event:
            statsDetails = event['Stats']['Details']
            print("Stats details bytesScanned: ")
            print(statsDetails['BytesScanned'])
            print("Stats details bytesProcessed: ")
            print(statsDetails['BytesProcessed'])
            print("Stats details bytesReturned: ")
            print(statsDetails['BytesReturned'])

    # idata = []
    # idata['data'] = dataResults

    imsi['body'] = strResult
    # imsi['body'] = dataResults[0]
    # imsi['body'] = dataResults[0]['wkt']
    # imsi['body'] = response
    # imsi['body'] = "Welcom to Hell"
    return imsi

```

