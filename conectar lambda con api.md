# lambda_function.py
def handler(event, context):
    name = event.get("queryStringParameters", {}).get("name", "Mundo")
    
    return {
        "statusCode": 200,
        "body": f"Hola {name} desde Lambda y API!"
    }


# Crear la funcion lambda
aws lambda create-function --function-name HolaLambda --runtime python3.11 --role arn:aws:iam::000000000000:role/lambda-role --handler lambda_function.handler --zip-file fileb://function.zip

# Crear un nuevo recurso en la API 
aws apigateway create-resource --rest-api-id kybdi24iv2 --parent-id 6tb4cstaet --path-part hola-lambda

# Agregar Metodo GET 
aws apigateway put-method --rest-api-id kybdi24iv2 --resource-id 3b5gkw8ji6 --http-method GET --authorization-type NONE

# Integracion con Lambda 
aws apigateway put-integration --rest-api-id kybdi24iv2 --resource-id 3b5gkw8ji6 --http-method GET --type AWS_PROXY --integration-http-method POST --uri arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-1:000000000000:function:HolaLambda/invocations

# Dar permisos a Lambda 
aws lambda add-permission --function-name HolaLambda --statement-id apigateway-invoke --action lambda:InvokeFunction --principal apigateway.amazonaws.com --source-arn arn:aws:execute-api:us-east-1:000000000000:kybdi24iv2/*/GET/hola-lambda

# Desplegar la Api 
aws apigateway create-deployment --rest-api-id kybdi24iv2 --stage-name dev

# Probar la Api con el recuerso /hola-lambda
curl "http://localhost:4566/restapis/kybdi24iv2/dev/_user_request_/hola-lambda?name=Maria"
