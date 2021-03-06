# CreateForecastExportJob<a name="API_CreateForecastExportJob"></a>

Exports a forecast created by the [CreateForecast](API_CreateForecast.md) operation to your Amazon Simple Storage Service \(Amazon S3\) bucket\. The forecast file name will match the following conventions:

<ForecastExportJobName>\_<ExportTimestamp>\_<PageNumber>

where the <ExportTimestamp> component is in Java SimpleDateFormat \(yyyy\-MM\-ddTHH\-mm\-ssZ\)\.

You must specify a [DataDestination](API_DataDestination.md) object that includes an AWS Identity and Access Management \(IAM\) role that Amazon Forecast can assume to access the Amazon S3 bucket\. For more information, see [Set Up Permissions for Amazon Forecast](aws-forecast-iam-roles.md)\.

For more information, see [Forecasts](howitworks-forecast.md)\.

To get a list of all your forecast export jobs, use the [ListForecastExportJobs](API_ListForecastExportJobs.md) operation\.

**Note**  
The `Status` of the forecast export job must be `ACTIVE` before you can access the forecast in your Amazon S3 bucket\. To get the status, use the [DescribeForecastExportJob](API_DescribeForecastExportJob.md) operation\.

## Request Syntax<a name="API_CreateForecastExportJob_RequestSyntax"></a>

```
{
   "[Destination](#forecast-CreateForecastExportJob-request-Destination)": { 
      "[S3Config](API_DataDestination.md#forecast-Type-DataDestination-S3Config)": { 
         "[KMSKeyArn](API_S3Config.md#forecast-Type-S3Config-KMSKeyArn)": "string",
         "[Path](API_S3Config.md#forecast-Type-S3Config-Path)": "string",
         "[RoleArn](API_S3Config.md#forecast-Type-S3Config-RoleArn)": "string"
      }
   },
   "[ForecastArn](#forecast-CreateForecastExportJob-request-ForecastArn)": "string",
   "[ForecastExportJobName](#forecast-CreateForecastExportJob-request-ForecastExportJobName)": "string"
}
```

## Request Parameters<a name="API_CreateForecastExportJob_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [Destination](#API_CreateForecastExportJob_RequestSyntax) **   <a name="forecast-CreateForecastExportJob-request-Destination"></a>
The location where you want to save the forecast and an AWS Identity and Access Management \(IAM\) role that Amazon Forecast can assume to access the location\. The forecast must be exported to an Amazon S3 bucket\.  
If encryption is used, `Destination` must include an AWS Key Management Service \(KMS\) key\. The IAM role must allow Amazon Forecast permission to access the key\.  
Type: [DataDestination](API_DataDestination.md) object  
Required: Yes

 ** [ForecastArn](#API_CreateForecastExportJob_RequestSyntax) **   <a name="forecast-CreateForecastExportJob-request-ForecastArn"></a>
The Amazon Resource Name \(ARN\) of the forecast that you want to export\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `^[a-zA-Z0-9\-\_\.\/\:]+$`   
Required: Yes

 ** [ForecastExportJobName](#API_CreateForecastExportJob_RequestSyntax) **   <a name="forecast-CreateForecastExportJob-request-ForecastExportJobName"></a>
The name for the forecast export job\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z][a-zA-Z0-9_]*`   
Required: Yes

## Response Syntax<a name="API_CreateForecastExportJob_ResponseSyntax"></a>

```
{
   "[ForecastExportJobArn](#forecast-CreateForecastExportJob-response-ForecastExportJobArn)": "string"
}
```

## Response Elements<a name="API_CreateForecastExportJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [ForecastExportJobArn](#API_CreateForecastExportJob_ResponseSyntax) **   <a name="forecast-CreateForecastExportJob-response-ForecastExportJobArn"></a>
The Amazon Resource Name \(ARN\) of the export job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `^[a-zA-Z0-9\-\_\.\/\:]+$` 

## Errors<a name="API_CreateForecastExportJob_Errors"></a>

 **InvalidInputException**   
We can't process the request because it includes an invalid value or a value that exceeds the valid range\.  
HTTP Status Code: 400

 **LimitExceededException**   
The limit on the number of resources per account has been exceeded\.  
HTTP Status Code: 400

 **ResourceAlreadyExistsException**   
There is already a resource with this name\. Try again with a different name\.  
HTTP Status Code: 400

 **ResourceInUseException**   
The specified resource is in use\.  
HTTP Status Code: 400

 **ResourceNotFoundException**   
We can't find a resource with that Amazon Resource Name \(ARN\)\. Check the ARN and try again\.  
HTTP Status Code: 400

## See Also<a name="API_CreateForecastExportJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/forecast-2018-06-26/CreateForecastExportJob) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/forecast-2018-06-26/CreateForecastExportJob) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/forecast-2018-06-26/CreateForecastExportJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/forecast-2018-06-26/CreateForecastExportJob) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/forecast-2018-06-26/CreateForecastExportJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/forecast-2018-06-26/CreateForecastExportJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/forecast-2018-06-26/CreateForecastExportJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/forecast-2018-06-26/CreateForecastExportJob) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/forecast-2018-06-26/CreateForecastExportJob) 